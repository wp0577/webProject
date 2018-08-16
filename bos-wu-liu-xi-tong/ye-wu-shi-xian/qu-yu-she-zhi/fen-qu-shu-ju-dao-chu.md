# 分区数据导出

## 为页面中导出按钮绑定事件

![](../../../.gitbook/assets/image%20%2836%29.png)

## 服务端实现

第一步：查询所有的分区数据 

第二步：使用POI将数据写到Excel文件中 

第三步：使用输出流进行文件下载

```text
/**
	 * 分区数据导出功能
	 * @throws IOException 
	 */
	public String exportXls() throws IOException{
		//第一步：查询所有的分区数据
		List<Subarea> list = subareaService.findAll();
		
		//第二步：使用POI将数据写到Excel文件中
		//在内存中创建一个Excel文件
		HSSFWorkbook workbook = new HSSFWorkbook();
		//创建一个标签页
		HSSFSheet sheet = workbook.createSheet("分区数据");
		//创建标题行
		HSSFRow headRow = sheet.createRow(0);
		headRow.createCell(0).setCellValue("分区编号");
		headRow.createCell(1).setCellValue("开始编号");
		headRow.createCell(2).setCellValue("结束编号");
		headRow.createCell(3).setCellValue("位置信息");
		headRow.createCell(4).setCellValue("省市区");
		
		for (Subarea subarea : list) {
			HSSFRow dataRow = sheet.createRow(sheet.getLastRowNum() + 1);
			dataRow.createCell(0).setCellValue(subarea.getId());
			dataRow.createCell(1).setCellValue(subarea.getStartnum());
			dataRow.createCell(2).setCellValue(subarea.getEndnum());
			dataRow.createCell(3).setCellValue(subarea.getPosition());
			dataRow.createCell(4).setCellValue(subarea.getRegion().getName());
		}
		
		//第三步：使用输出流进行文件下载（一个流、两个头）
		
		String filename = "分区数据.xls";
		String contentType = ServletActionContext.getServletContext().getMimeType(filename);
		ServletOutputStream out = ServletActionContext.getResponse().getOutputStream();
		ServletActionContext.getResponse().setContentType(contentType);
		
		//获取客户端浏览器类型
		String agent = ServletActionContext.getRequest().getHeader("User-Agent");
		filename = FileUtils.encodeDownloadFilename(filename, agent);
		ServletActionContext.getResponse().setHeader("content-disposition", "attachment;filename="+filename);
		workbook.write(out);
		return NONE;
	}

```

