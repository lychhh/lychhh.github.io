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

cxf:
```
	<servlet>
		<servlet-name>CXFService</servlet-name>
		<servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
		<load-on-startup>4</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>CXFService</servlet-name>
		<url-pattern>/service/*</url-pattern>
	</servlet-mapping>
```


发布接口  
axis：  
server-config.wsdd（位于WEB-INF文件夹下）  

```
<service name="webserviceForTest" provider="java:RPC">
	<parameter name="allowedMethods" value="*"/>
	<parameter name="className"  value="com.zres.product.trs.webservice.WebserviceForTest"/>
	<namespace>http://www.zznode.com/axis/webserviceForTest</namespace>
</service>
```

#### allowedMethods——允许的方法 , className——类路径 , namespace——命名空间

		
cxf：  
通过spring直接引入  
```
<bean id="ILaunchProjectTransAssets" class="com.ztesoft.resmaster.module.smrh.interfacemanage.bizwebservice.webservice.LaunchProjectTransAssets"></bean> 
    <jaxws:endpoint id="ILaunchProjectTransAssetsWSDL" implementor="#ILaunchProjectTransAssets"
    	address="/ILaunchProjectTransAssets" />
```

#### bean id接口 , class 实现类 , endpoint id 自定义 , implementor 实现类 , address 自定义

axis2：  
services.xml（位于WEB-INF下services下工程名下META-INF里）  
```
<?xml version="1.0" encoding="UTF-8"?>
<serviceGroup>
	<service name="TieTaRoutingService" >
		<description>TieTaRoutingService</description>
		<messageReceivers>
			<messageReceiver class="org.apache.axis2.rpc.receivers.RPCInOnlyMessageReceiver" mep="http://www.w3.org/2004/08/wsdl/in-only" />
			<messageReceiver class="org.apache.axis2.rpc.receivers.RPCMessageReceiver" mep="http://www.w3.org/2004/08/wsdl/in-out" />
		</messageReceivers>
		<parameter name="ServiceObjectSupplier">
			org.apache.axis2.extensions.spring.receivers.SpringServletContextObjectSupplier
		</parameter>
		<parameter name="SpringBeanName">tieTaRouting</parameter>
	</service>
</serviceGroup>
```

接口访问  
axis：  
http://localhost:8081/trsweb/services/webserviceForResources?wsdl

services——拦截器配置 , webserviceForResources——服务名 


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


### webservice接口调用
```
Service service = new Service();
Call call = (Call) service.createCall();
call.setTargetEndpointAddress(new java.net.URL(url));
call.setOperationName(new QName(null, methodName));
call.addParameter("XML", XMLType.XSD_STRING, ParameterMode.IN);
call.setEncodingStyle("UTF-8"); 
call.setReturnType(XMLType.XSD_STRING);
ret = (String)call.invoke(new Object[] {XML});
```


### wsdl文件链接转为java代码

axis-1_4  
1、下载相应包并解压  
2、写个bat文件  
bat文件如下：（去掉后面备注）  
set Axis_Lib=C:\axislib\axislib\lib    （这是axis的lib包）
set Java_Cmd=java -Djava.ext.dirs=%Axis_Lib%
set Output_Path=C:\soap_src\src （这是生成的java代码输出的位置）
set Package=com.huawei.mdn.wsi.engine.client
%Java_Cmd% org.apache.axis.wsdl.WSDL2Java -o%Output_Path% -p%Package% --server-side TvodMergeStatus.wsdl （源wsdl文件）
3、运行就可以得到相应的文件


axis2  
1、下载解压包  
2、进入bin目录执行命令  
wsdl2java.bat -uri http://10.6.135.5/iAppServices/iApp/ThirdDataServies.asmx?wsdl -o X:\axis -p com.bd.zd  
-uri : wsdl文件的位置，注意检查文件路径之间不要有空格哦~有空格就需要把这段路径加“”（引号）  
-o:文件的输出位置。默认情况两个文件（ java文件及build.xml）都在axis2-1.5\bin目录下  
-p:生成的java文件的包名  
注意生成的java包目录会默认加上一层src（如axis2-1.5.5\bin\src\com\zhejiangcourt\ssoverifyweb）  

