---
title: JavaScript中取得链接参数某参数的值
date: 2018-04-19 09:38:20
tags:
  - 前端
  - JavaScript
  - js
categories: JavaScript
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/javascript.jpg" alt="JavaScript" style="width:100%" />
<!-- more -->
不多说，直接上代码
```javascript
function getLinkValue(paraName)
	{
		var strHref = window.document.location.href;
		var intPos = strHref.indexOf("?");
		var strRight = strHref.substr(intPos + 1);
		var arrTmp = strRight.split("&");
		for ( var i = 0; i < arrTmp.length; i=i+1)
		{
			var arrTemp = arrTmp[i].split("=");
			if (arrTemp[0].toUpperCase() == paraName.toUpperCase()) return arrTemp[1];
		}
		return "";
	}
```
用法如下：
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
 <HEAD>
  <TITLE> New Document </TITLE>
  <META NAME="Generator" CONTENT="EditPlus">
  <META NAME="Author" CONTENT="">
  <META NAME="Keywords" CONTENT="">
  <META NAME="Description" CONTENT="">
  <script type="text/javascript">

	window.onload=function(){
	alert(getLinkValue('jhg'));
	}

	function getLinkValue(paraName)
		{
			var strHref = window.document.location.href;
			var intPos = strHref.indexOf("?");
			var strRight = strHref.substr(intPos + 1);
			var arrTmp = strRight.split("&");
			for ( var i = 0; i < arrTmp.length; i=i+1)
			{
				var arrTemp = arrTmp[i].split("=");
				if (arrTemp[0].toUpperCase() == paraName.toUpperCase()) return arrTemp[1];
			}
			return "";
		}
  </script>
 </HEAD>

 <BODY>
  
 </BODY>
</HTML>
```