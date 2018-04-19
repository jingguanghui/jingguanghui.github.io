---
title: Java中MD5加密和JavaScript中MD5加密
date: 2018-04-19 12:10:20
tags:
  - 前端
  - JavaScript
  - js
  - Java
categories: Java
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/md5.jpg" alt="JavaScript" style="width:100%" />
<!-- more -->
## Java中MD5加密
废话不多说，直接上代码
```java
import java.security.MessageDigest;
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.File;

/**
 * <p>
 * Title: Md5Helper类
 * </p>
 * <p>
 * Description: 获取一个文件或字节数组的MD5信息摘要
 */
public final class Md5Helper
{
	/**
	 * 类禁止外部实例化
	 */
	private Md5Helper()
	{
		//donone
	}
	
	/**
	 * 获取一个字节数组的MD5信息摘要
	 * 
	 * @param as_Info
	 *            原始信息
	 * @return MD5信息摘要
	 * @throws java.lang.IllegalArgumentException
	 *             如果参数无效
	 * @throws java.lang.Exception
	 *             包含其它任何异常
	 */
	public static byte[] uf_Md5(byte[] as_Info)
			throws IllegalArgumentException, Exception
	{
		if ((as_Info == null) || (as_Info.length == 0))
		{
			throw new IllegalArgumentException("无效参数");
		}
        MessageDigest alga = java.security.MessageDigest.getInstance("MD5");
		alga.update(as_Info);
		return alga.digest();
	}

	/**
	 * 加密
	 * 字节数组转换为16进制显示
	 * @param info 字节数组
	 * @return 16进制显示字符串
	 */
	public static String uf_byte2hex(byte[] info)
	{
		String hs = "";
		String stmp = "";
		
		for (int n = 0; n < info.length; n++)
		{
			stmp = (java.lang.Integer.toHexString(info[n] & 0XFF));
			if (stmp.length() == 1)
			{
				hs = hs + "0" + stmp;
			}
			else
			{
				hs = hs + stmp;
			}
		}	
		return hs;
	}

	/**
	*主方法测试
	*/
	public static void main(String[] args) throws IllegalArgumentException, Exception {
		System.out.println(uf_byte2hex(uf_Md5("value".getBytes())));//2063c1608d6e0baf80249c42e2be5804
		System.out.println(uf_byte2hex(uf_Md5("111111".getBytes())));//96e79218965eb72c92a549dd5a330112
	}
}
```
## JavaScript中MD5加密
废话不多说，直接上代码
```javascript
<script src="yourUrl/md5.min.js"></script>  
或者：  
<script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>  
<script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.min.js"></script>  
```
示例：
```javascript
var hash = md5("value");  // 2063c1608d6e0baf80249c42e2be5804  
var hash = md5("111111");  // 96e79218965eb72c92a549dd5a330112  
```
参考：
1. [http://www.bootcdn.cn/blueimp-md5/](http://www.bootcdn.cn/blueimp-md5/)
2. [https://github.com/blueimp/JavaScript-MD5](https://github.com/blueimp/JavaScript-MD5)