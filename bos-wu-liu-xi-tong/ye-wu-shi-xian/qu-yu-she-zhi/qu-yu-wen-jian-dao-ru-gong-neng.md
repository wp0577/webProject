# 区域文件导入功能

## jquery OCUpload一键上传插件使用

```text
OCUpload（One Click Upload）
第一步：将js文件引入页面
<script type="text/javascript" src="${pageContext.request.contextPath }/js/jquery-1.8.3.js"></script>
<script type="text/javascript" src="${pageContext.request.contextPath }/js/jquery.ocupload-1.1.2.js"></script>
第二步：在页面中提供任意一个元素
 
第三步：调用插件提供的upload方法，动态修改HTML页面元素
<script type="text/javascript">
	$(function(){
		//页面加载完成后，调用插件的upload方法，动态修改了HTML页面元素
		$("#myButton").upload({
			action:'xxx.action',
			name:'myFile'
		});
	});
</script>
```

![](../../../.gitbook/assets/image%20%2887%29.png)

## 在服务端接收上传的文件

```text
在Action中提供一个File类型的属性，名称和上传的文件输入框名称一致regionFile
@Controller
@Scope("prototype")
public class RegionAction extends BaseAction<Region>{
	//属性驱动，接收上传的文件
	private File regionFile;

	public void setRegionFile(File regionFile) {
		this.regionFile = regionFile;
	}
	
	/**
	 * 区域导入
	 */
	public String importXls(){
		System.out.println(regionFile);
		return NONE;
	}
}

```

### 区域上传实例

```text
public String upload() throws Exception {
        HSSFWorkbook hssfWorkbook = new HSSFWorkbook(new FileInputStream(myFile));
        HSSFSheet sheet1 = hssfWorkbook.getSheet("Sheet1");
        ArrayList<Region> regionArray = new ArrayList();
        for(Row row : sheet1){
            if(row.getRowNum() == 0) continue;
            Region region = new Region();
            region.setId(row.getCell(0).getStringCellValue());
            region.setProvince(row.getCell(1).getStringCellValue());
            region.setCity(row.getCell(2).getStringCellValue());
            region.setDistrict(row.getCell(3).getStringCellValue());
            region.setPostcode(row.getCell(4).getStringCellValue());
            //通过pingyin4j创建shrotcode和citycode
            String province = region.getProvince().substring(0, region.getProvince().length()-1);
            String city = region.getCity().substring(0, region.getCity().length()-1);
            String district = region.getDistrict().substring(0, region.getDistrict().length()-1);
            String[] shortcodeTemp = PinYin4jUtils.getHeadByString(province+city+district);
            String shortcode = StringUtils.join(shortcodeTemp);
            String citycode = PinYin4jUtils.hanziToPinyin(city,"");
            region.setShortcode(shortcode);
            region.setCitycode(citycode);
            regionArray.add(region);
        }
        iRegionService.uploadFile(regionArray);
        return NONE;
    }
```

