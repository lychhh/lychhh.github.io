---
layout:  post
title: webservice接口开发流程
category: blog
description: 学习使用
---



一些理解  
配置拦截器用来拦截相应的接口请求  
配置的拦截路径，我们生成相应的url进行访问  


web.xml  
配置拦截器  
axis:
```
	<servlet>
		<servlet-name>axis</servlet-name>
		<servlet-class>org.apache.axis.transport.http.AxisServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>axis</servlet-name>
		<url-pattern>/services/*</url-pattern>
	</servlet-mapping>
```

axis2:
```
	<servlet>
        <servlet-name>AxisServlet</servlet-name>
        <servlet-class>org.apache.axis2.transport.http.AxisServlet</servlet-class>
        <load-on-startup>2</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>AxisServlet</servlet-name>
        <url-pattern>/services/*</url-pattern>
    </servlet-mapping>
```

server-config.wsdd  
定义接口  
	接口名称  
	```
	<service name="webserviceForTest" provider="java:RPC">
	```  
		允许的方法  
		```
        <parameter name="allowedMethods" value="*"/>
		```  
		类路径  
		```
        <parameter name="className"  value="com.zres.product.trs.webservice.WebserviceForTest"/>
		```  
		命名空间  
		```
        <namespace>http://www.zznode.com/axis/webserviceForTest</namespace>
    </service>
		```

接口访问  
http://localhost:8081/trsweb/services/webserviceForResources?wsdl


使用soapUi测试接口功能  

xml入参解析
```
		Map<String,Object> map =new HashMap<String, Object>();
        Document docRequest = DocumentHelper.parseText(responseInfo);
        Element headRequest = (Element) docRequest.selectSingleNode("//PACKET/HEAD");
        map.put("FLOW_ID",headRequest.elementTextTrim("FLOW_ID"));//服务流水
        map.put("RESPONSE_CODE",headRequest.elementTextTrim("RESPONSE_CODE"));//响应编码,000000代表成功
        map.put("RESPONSE_MSG",headRequest.elementTextTrim("RESPONSE_MSG"));//响应信息
        map.put("SERVICE_CODE",headRequest.elementTextTrim("SERVICE_CODE"));//服务编码
        map.put("RESPONSE_TIME",headRequest.elementTextTrim("RESPONSE_TIME"));//响应时间
        map.put("FTP_USER",headRequest.elementTextTrim("FTP_USER"));//FTP账号
        map.put("FTP_PWD",headRequest.elementTextTrim("FTP_PWD"));//FTP密码
        map.put("FILE_NAME",headRequest.elementTextTrim("FILE_NAME"));//FTP文件名称，多个则英文逗号
        map.put("FILE_PATH",headRequest.elementTextTrim("FILE_PATH"));//文件所在路径
```


xml出参拼接
```
		Document requestDoc = DocumentHelper.createDocument();
        Element requestPacket = requestDoc.addElement("PACKET");
        Element requestHead=requestPacket.addElement("HEAD");
        requestHead.addElement("SYS_COMPANY").setText(sysCompany);
        requestHead.addElement("SERVICE_CODE").setText(serviceCode);
        requestHead.addElement("FILE_TYPE").setText(fileType);
        requestHead.addElement("REQUEST_TIME").setText(requestTime);
        requestHead.addElement("ACCESS_TOKEN").setText(accessToken);
        requestHead.addElement("CUST_COMPANY").setText(custCompany);
        requestHead.addElement("HANDLE_TYPE").setText(handleType);
        requestHead.addElement("ACCOUNT_PERIOD").setText(accountPeriod);
        requestHead.addElement("PROVINCE_ID").setText(provinceId);
        requestHead.addElement("CITY_ID").setText(cityId);
        requestHead.addElement("FLOW_ID").setText(flowId);
        requestHead.addElement("STATUS").setText(status);
		element.addCDATA(JsonParser.convertObjectToJson(map));
		element.selectSingleNode(nodeName) != null
        return requestDoc.asXML();
```


xml中封装list字符串  
```
//		list使用json出参拼接
		JSONArray array = JSONArray.fromObject(list);
		String result = array.toString();
		
		
//		list使用json入参获取
		JSONArray resultArray = JSONArray.fromObject(result);
		for (int i = 0; i < resultArray.size(); i++) {
			Map<String,String> tempMap = (Map) resultArray.get(i);
			System.out.println(tempMap);
			for (String key : tempMap.keySet()) {
				System.out.println(key + ":" + map.get(key));
			}
		}
```

直接获取userName节点，不在乎其他头节点  
document.selectSingleNode("//userName")


