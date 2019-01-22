---
layout: post
title: JAVA基础-日期格式转换
keywords: 解决JAVA时间格式2018-08-15T16:00:00.000Z转换报错的问题
category: java
tags: [java] 
---

## 描述（Description）
最近在用[Element的UI框架](http://element-cn.eleme.io/)，需要使用日期控件DateTimePicker  

``` javascript
<el-date-picker
      v-model="value1"
      type="datetime"
      placeholder="选择日期时间">
</el-date-picker>

```
使用的是VUE绑定的数据,发现控件显示的和传递到后台的值格式不一致

## 问题现象（Error）
绑定的参数传递到后台之后报错了，看报错信息才知道这个控件返回的时间格式和显示的不是一样的 ，显示是 yyyy-MM-dd'T'HH:mm:ss ，但是后台传递的是 yyyy-MM-dd'T'HH:mm:ss.SSSZ ，所以使用 new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss").parse(date) 格式化时就报错了。

## 源代码（Code）
因为不知道这个UI怎么设置传递参数和界面显示一致，所以只能java写了个日期转换的工具，然后问题就解决了！

工具类如下：

``` java
public static Date parse(String date) {
    	if(date!=null) {
		try {
		  if(date.contains("-")) {
			if(date.contains(":") && date.lastIndexOf("Z")>0) {
				date = date.replace("Z", " UTC");
					return new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS Z").parse(date);
			}else if(date.contains(":")){
					return new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss").parse(date);
			}else {
					return new SimpleDateFormat("yyyy-MM-dd").parse(date);
			}
		}
		if(date.contains("/")) {
			if(date.contains(":") && date.lastIndexOf("Z")>0) {
					date = date.replace("Z", " UTC");
					return new SimpleDateFormat("yyyy/MM/dd'T'HH:mm:ss.SSS Z").parse(date);
			}else  if(date.contains(":")) {
					return new SimpleDateFormat("yyyy/MM/dd'T'HH:mm:ss").parse(date);
			}else {
					return new SimpleDateFormat("yyyy/MM/dd").parse(date);
			}
		}
		} catch (ParseException e) {
			e.printStackTrace();
		}
    	}
        return null;
    }
```

这个UI和平时用的 不一样，所以特别记录下使用的过程，以免后续遗忘！

更多的一些工具请看[开源代码](https://github.com/108day/java-common-utils)
