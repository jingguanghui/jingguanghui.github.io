---
title: Java中对File的操作
date: 2018-04-20 10:09:20
tags:
  - Java
  - File
  - Java I/O系统
categories: Java
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/File.jpeg" alt="File" style="width:100%" />
<!-- more -->
一直以来对File类中的API不太了解，今天借着对《Java编程思想（第4版）》中第18.1章File类的学习，做一个相对深入的研究。
## 创建
### 创建目录
```java
  /**
	 * 创建目录
	 * @param as_Path 路径
	 * @return 创建后的文件对象 
	 * @throws Exception 方法异常
	 * 
	 */
	public static File uf_CreateDirectory(String as_Path) throws Exception{
		File file=new File(as_Path);
		if(file.exists()){
			if(!file.isDirectory()){
			 throw new Exception("路径存在但不是目录！");	
			}
		}
		//如果文件夹不存在，则会创建，并返回true;如果存在，则不会创建，并返回false
		file.mkdirs();
		return file;
	}
```
在这里普及一下mkdirs()和mkdir()的区别：

mkdirs()可以建立多级文件夹， mkdir()只会建立一级的文件夹， 如下：
```java
new File("/tmp/one/two/three").mkdirs();
```
执行后， 会建立tmp/one/two/three四级目录
```java
new File("/tmp/one/two/three").mkdir();
```
则不会建立任何目录， 因为找不到/tmp/one/two目录， 结果返回false
### 创建文件
```java
/**
 * 创建文件
 * @param as_Path 路径
 * @param isDelete 如果文件已存在，是否删除
 * @return 创建后的文件对象
 * @throws Exception 方法异常
 */
public static File uf_CreateFile(String as_Path,boolean isDelete) throws Exception{
	File file=new File(as_Path);
	if(file.exists()){
		if(isDelete){
			//删除文件
			file.delete();
			file.createNewFile();
		}
	}else{
		if(as_Path.endsWith(File.separator)){
			throw new Exception("创建单个文件" +as_Path + "失败，目标文件不能为目录！");		
		}
		//判断目标文件所在的目录是否存在 
		if(file.getParentFile()!=null){
		if(!file.getParentFile().exists()){
			if(!file.getParentFile().mkdirs()) {  
				throw new Exception("创建目标文件所在目录失败！");      
            }  
		}
		}
		file.createNewFile();
	}
	return file;
}
```
<div class="note danger"><p>在创建文件时，必须保证目标文件不存在，而且父目录存在，否则会创建失败 </p></div>
## 删除
### 删除目录
```java
/**
 * 删除目录（包括子目录） 
 * @param as_Path 要删除的目录路径
 * @param isIncludeRoot 是否包含路径本身
 * @throws Exception 方法异常
 */
public static void uf_DelTree(String as_Path,boolean isIncludeRoot) throws Exception{
	File file=new File(as_Path);
	if(!file.exists()){
		throw new Exception("要删除的路径不存在！");
	}
	if(!file.isDirectory()){
		throw new Exception("路径存在，但不是目录！");
	}
	File[] files=file.listFiles();
	String fileName;
	for(File file1:files){
		fileName=file1.getName();
		if(file1.isDirectory()){
			FileHelper.uf_DelTree(as_Path+File.separator+fileName, true);
		}else{
			file1.delete();
		}
	}
	if(isIncludeRoot){
		file.delete();
	}
}
```
在这里普及一下File类中的list()和listFiles()方法的区别：

* list()方法是返回某个目录下的所有文件和目录的文件名，返回的是String数组

* listFiles()方法是返回某个目录下所有文件和目录的绝对路径，返回的是File数组

```java list()
public static void main(String[] args) throws Exception {
	    File file=new File("E:/thinkingInJava/Think in Java 4 code/object");
	    String[] fileNames=file.list();
	    for(String fileName:fileNames){
	    	System.out.println(fileName);
	    }
	}
```
结果如下：
```
build.xml
Documentation1.java
Documentation2.java
Documentation3.java
HelloDate.java
HelloDate1.java
ShowProperties.java
```
```java listFiles()
public static void main(String[] args) throws Exception {
	    File file=new File("E:/thinkingInJava/Think in Java 4 code/object");
	    File[] fileNames=file.listFiles();
	    for(File fileName:fileNames){
	    	System.out.println(fileName);
	    }
	}
```
结果如下：
```
E:\thinkingInJava\Think in Java 4 code\object\build.xml
E:\thinkingInJava\Think in Java 4 code\object\Documentation1.java
E:\thinkingInJava\Think in Java 4 code\object\Documentation2.java
E:\thinkingInJava\Think in Java 4 code\object\Documentation3.java
E:\thinkingInJava\Think in Java 4 code\object\HelloDate.java
E:\thinkingInJava\Think in Java 4 code\object\HelloDate1.java
E:\thinkingInJava\Think in Java 4 code\object\ShowProperties.java
```
### 删除文件
```java
/**
 * 删除文件 
 * @param as_Path 要删除的文件路径
 * @throws Exception 方法异常
 */
public static void uf_DelFile(String as_Path) throws Exception{
	File file=new File(as_Path);
	if(!file.exists()){
		throw new Exception("要删除的文件不存在！");
	}
	if(as_Path.endsWith(File.separator)){
		throw new Exception("删除单个文件" +as_Path + "失败，目标文件不能为目录！");
	}
	if(!file.isFile()){
		throw new Exception("删除单个文件" +as_Path + "失败，目标文件不是文件！");
	}
	file.delete();
}
```
## 修改
### 复制文件
```java
/**
 * 复制文件
 * @param as_SourFile 源文件路径
 * @param as_DestFile 目标文件路径
 * @param isOvercast 如果目标文件存在是否覆盖
 * @throws Exception 方法异常
 */
public static File uf_CopyFile(String as_SourFile,String as_DestFile,boolean isOvercast) throws Exception{
	File sourFile=new File(as_SourFile);
	if(!sourFile.exists()){
		throw new Exception("复制文件" +as_SourFile + "失败，源文件不存在！");
	}
	if(!sourFile.isFile()){
		throw new Exception("复制文件" +as_SourFile + "失败，源文件不是文件！");
	}
	File destFile=new File(as_DestFile);
	if(destFile.exists()){
		if(!destFile.isFile()){
			throw new Exception("复制文件" +as_SourFile + "失败，目标路径"+as_DestFile+"存在,但不是文件！");
		}
		if(!isOvercast){
			return destFile;
		}
	}
	FileInputStream fileInputStream=null;
	FileOutputStream fileOutputStream=null;
	try {
		fileInputStream=new FileInputStream(sourFile);
		fileOutputStream=new FileOutputStream(destFile);
		byte[] buf=new byte[1024];
		int len=0;
		while((len=fileInputStream.read(buf))!=-1){
			fileOutputStream.write(buf, 0, len);
		}
	} catch (Exception e) {
		e.printStackTrace();
		throw new Exception(e);
	}finally{
		if(fileInputStream!=null){
			fileInputStream.close();
		}
		if(fileOutputStream!=null){
			fileOutputStream.close();
		}
	}
	
	return destFile;
}
```
### 复制目录
```java
/**
 * 目录复制(含子目录)
 * @param as_sPath 源路径       
 * @param as_dPath 目标路径
 * @param ab_IsIncludeRoot 是否包含源路径本身
 * @throws Exception 方法异常
 */
public static void uf_CopyTree(String as_sPath, String as_dPath, boolean ab_IsIncludeRoot) throws Exception
{
    File lo_sFile = new File(as_sPath);
    if (!lo_sFile.exists())
        throw new Exception("源路径并不存在。");
    if (!lo_sFile.isDirectory())
        throw new Exception("源路径并非目录。");
    File lo_dFile = new File(as_dPath);
    if (!lo_dFile.exists()){
    	lo_dFile.mkdirs();
    }
    if (!lo_dFile.isDirectory())
        throw new Exception("目标路径并非目录。");
    File lo_NewFile, lo_File;
    String ls_Name;
    if (ab_IsIncludeRoot)
    {
        ls_Name = lo_sFile.getName();
        lo_NewFile = new File(as_dPath +File.separator + ls_Name);
        if (!lo_NewFile.exists())
        {
            lo_NewFile.mkdir();
        }
        else
        {
            if (!lo_NewFile.isDirectory())
                throw new Exception("目标路径存在但并非目录。");
        }
        FileHelper.uf_CopyTree(as_sPath, as_dPath + File.separator  + ls_Name, false);
    }
    else
    {
        File[] lo_Files = lo_sFile.listFiles();
        for (int ii = 0; ii < lo_Files.length; ii++)
        {
            lo_File = lo_Files[ii];
            ls_Name = lo_File.getName();
            if (lo_File.isDirectory())
            {
                lo_NewFile = new File(as_dPath + File.separator  + ls_Name);
                if (!lo_NewFile.exists())
                {
                    lo_NewFile.mkdir();
                }
                else
                {
                    if (!lo_NewFile.isDirectory())
                        throw new Exception("目标路径存在但并非目录。");
                }
                FileHelper.uf_CopyTree(as_sPath + File.separator  + ls_Name, as_dPath + File.separator  + ls_Name, false);
            }
            else
            {
                FileHelper.uf_CopyFile(as_sPath + File.separator  + ls_Name, as_dPath + File.separator  + ls_Name, true);
            }
        }
    }
}
```
### 移动文件
```java
/**
 * 移动文件
 * @param as_sFile 源文件路径
 * @param as_dFile 目标文件路径
 * @param ab_IsOvercast 目标文件存在是否覆盖
 * @return 移动后的File对象
 * @throws Exception 方法异常
 */
public static File uf_MoveFile(String as_sFile, String as_dFile, boolean ab_IsOvercast) throws Exception
{
    File lo_sFile = new File(as_sFile);
    File lo_dFile = FileHelper.uf_CopyFile(as_sFile, as_dFile, ab_IsOvercast);
    lo_sFile.delete();
    return lo_dFile;
}
```
## 查看
首先，再普及一下File类中的list()和listFiles()方法的区别：

* list()方法是返回某个目录下的所有文件和目录的文件名，返回的是String数组

* listFiles()方法是返回某个目录下所有文件和目录的绝对路径，返回的是File数组

```java list()
public static void main(String[] args) throws Exception {
	    File file=new File("E:/thinkingInJava/Think in Java 4 code/object");
	    String[] fileNames=file.list();
	    for(String fileName:fileNames){
	    	System.out.println(fileName);
	    }
	}
```
结果如下：
```
build.xml
Documentation1.java
Documentation2.java
Documentation3.java
HelloDate.java
HelloDate1.java
ShowProperties.java
```
```java listFiles()
public static void main(String[] args) throws Exception {
	    File file=new File("E:/thinkingInJava/Think in Java 4 code/object");
	    File[] fileNames=file.listFiles();
	    for(File fileName:fileNames){
	    	System.out.println(fileName);
	    }
	}
```
结果如下：
```
E:\thinkingInJava\Think in Java 4 code\object\build.xml
E:\thinkingInJava\Think in Java 4 code\object\Documentation1.java
E:\thinkingInJava\Think in Java 4 code\object\Documentation2.java
E:\thinkingInJava\Think in Java 4 code\object\Documentation3.java
E:\thinkingInJava\Think in Java 4 code\object\HelloDate.java
E:\thinkingInJava\Think in Java 4 code\object\HelloDate1.java
E:\thinkingInJava\Think in Java 4 code\object\ShowProperties.java
```
### 目录列表器
对符合条件的文件进行筛选
```java
/**
 * 筛选符合条件的文件
 * @param as_sFile 目录文件
 * @param reg 要筛选的文件正则表达式
 * @return 符合条件的文件集合
 * @throws Exception
 */
public static String[] uf_ListFile(String as_sFile,final String reg)throws Exception{
	    File path = new File(as_sFile);
	    if (!path.exists())
            throw new Exception("源路径并不存在。");
        if (!path.isDirectory())
            throw new Exception("源路径并非目录。");
	    String[] list;
	    if(reg==null||"".equals(reg))
	      list = path.list();
	    else
	    	//匿名内部类
	      list = path.list(new FilenameFilter() {
	        private Pattern pattern = Pattern.compile(reg);
	        public boolean accept(File dir, String name) {
	          return pattern.matcher(name).matches();
	        }
	      });
	    //按字母顺序排序
	    Arrays.sort(list, String.CASE_INSENSITIVE_ORDER);
	  return list;
}
```

- <i class="fa fa-book fa-lg"></i>[查看源码](https://github.com/jingguanghui/bolgSource/blob/master/File/FileHelper.java)