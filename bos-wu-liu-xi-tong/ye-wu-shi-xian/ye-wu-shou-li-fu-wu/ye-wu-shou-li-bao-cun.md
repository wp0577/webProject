# 业务受理保存

## 第一步：为手机号输入框绑定离焦事件

```markup
<td>来电号码:</td>
<td><input type="text" class="easyui-validatebox" name="telephone" required="true" />
	<script type="text/javascript">
		$(function(){
			//页面加载完成后，为手机号输入框绑定离焦事件
			$("input[name=telephone]").blur(function(){
				//获取页面输入的手机号
				var telephone = this.value;
				//发送ajax请求，请求Action，在Action中远程掉调用crm服务，获取客户信息，用于页面回显
				$.post('noticebillAction_findCustomerByTelephone.action',{"telephone":telephone},function(data){
					if(data != null){
						//查询到了客户信息，可以进行页面回显
						var customerId = data.id;
						var customerName = data.name;
						var address = data.address;
						$("input[name=customerId]").val(customerId);
						$("input[name=customerName]").val(customerName);
						$("input[name=delegater]").val(customerName);
						$("input[name=pickaddress]").val(address);
					}else{
						//没有查询到客户信息，不能进行页面回显
						$("input[name=customerId]").val("");
						$("input[name=customerName]").val("");
						$("input[name=delegater]").val("");
						$("input[name=pickaddress]").val("");
					}
				});
			});
		});
	</script>	
</td>

```

## 第二步：创建NoticebillAction，注入crm代理对象，提供方法根据手机号查询客户信息，返回json

![](../../../.gitbook/assets/image%20%28129%29.png)

注意：配置struts.xml 

## 第三步：为页面中“新单”按钮绑定事件

![](../../../.gitbook/assets/image%20%2837%29.png)

## 在NoticebillAction中提供方法实现业务受理自动分单

![](../../../.gitbook/assets/image%20%28165%29.png)

```text
@Service
@Transactional
public class NoticebillServiceImpl implements INoticebillService {
	@Autowired
	private INoticebillDao noticebillDao;
	@Autowired
	private IDecidedzoneDao decidedzoneDao; 
	@Autowired
	private IWorkbillDao workbillDao;
	@Autowired
	private ICustomerService customerService;
	/**
	 * 保存业务通知单，还有尝试自动分单
	 */
	public void save(Noticebill model) {
		User user = BOSUtils.getLoginUser();
		model.setUser(user);//设置当前登录用户
		noticebillDao.save(model);
		//获取客户的取件地址
		String pickaddress = model.getPickaddress();
		//远程调用crm服务，根据取件地址查询定区id
		String decidedzoneId = customerService.findDecidedzoneIdByAddress(pickaddress);
		if(decidedzoneId != null){
			//查询到了定区id，可以完成自动分单
			Decidedzone decidedzone = decidedzoneDao.findById(decidedzoneId);
			Staff staff = decidedzone.getStaff();
			model.setStaff(staff);//业务通知单关联取派员对象
			//设置分单类型为：自动分单
			model.setOrdertype(Noticebill.ORDERTYPE_AUTO);
			//为取派员产生一个工单
			Workbill workbill = new Workbill();
			workbill.setAttachbilltimes(0);//追单次数
			workbill.setBuildtime(new Timestamp(System.currentTimeMillis()));//创建时间，当前系统时间
			workbill.setNoticebill(model);//工单关联页面通知单
			workbill.setPickstate(Workbill.PICKSTATE_NO);//取件状态
			workbill.setRemark(model.getRemark());//备注信息
			workbill.setStaff(staff);//工单关联取派员
			workbill.setType(Workbill.TYPE_1);//工单类型
			workbillDao.save(workbill);
			//调用短信平台，发送短信
		}else{
			//没有查询到定区id，不能完成自动分单
			model.setOrdertype(Noticebill.ORDERTYPE_MAN);
		}
	}
}

```

