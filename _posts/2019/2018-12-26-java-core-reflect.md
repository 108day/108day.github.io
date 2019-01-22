---
layout: post
title: JAVA基础-反射的应用
keywords: Map对象通过反射转为POJO
category: java
tags: [java] 
---

## 问题（Question）
在使用springboot时，浏览器过来数据在后台接收时有几种注解配置的方式 

- `@PathVariable("currPage")`在url路径中，也就是问号前面 例如：www.thevil.com/admin/list/page/1/20?,其中 1,20作为分页参数，就是PathVariable参数
- `@RequestBody` 要求请求Content-type 必须是json 
- `@RequestParam（"name")`  这个就比较简单，直接就是对一个String,Long 等基本类型的参数
- `@RequestParam `  没有括号一般都是用Map接收 

当你需要写一个接口时，但是又不想使用@equestBody 建立一个POJO 时 ，一般都会选择@RequestParam 用Map接收参数。但是有时候又会有把Map转为POJO的需要，而且每个字段的值都不可以为空时，也就是对 对象的每个值都做非空判断 ，这时你有两个选择：
- JO 的每个属性都单独拿出来进行 if else 判断
- 一个通用的方法判断

这里我就是写了一个Map转换工具类，利用反射将Map对象的值设置到 POJO对象的每个对应的属性，同时判断这个属性值是否为空，如果为空就抛出异常提示，提示这个字段为空，如果格式不正确就提示格式不对。这样就一段代码把数据格式，非空判断都做完了，这样用起来就很方便了，还可以设置一个参数 ：isAllowNull 是否允许为空，当不需要进行非空判断的时候用。是不是很方便！

说了这么多，直接上代码

```java

import java.lang.reflect.Field;
import java.math.BigDecimal;
import java.sql.Timestamp;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

/**
 *  Map工具类
 * @author Kevin zhaosheji
 * @date 2019年1月22日
 */
public class MapUtils extends HashMap<String, Object> {
    
    public static<T> T map2Entity(Map<String,Object> map,T t) throws Exception {
    	if(map!=null) {
    		Class<?> clazz = t.getClass();
        	for (Entry<String,Object> entry : map.entrySet()) {
    			String mapName = entry.getKey();
    			Object value = entry.getValue();
    			Field declaredField = null;
    			if(value!=null){
    				try {
    					declaredField = clazz.getDeclaredField(mapName);
    					if(declaredField!=null){
    						declaredField.setAccessible(true);
    						if(declaredField.getType() == String.class){
    							declaredField.set(t,value.toString());
    						}else if(declaredField.getType() == Date.class){
    							declaredField.set(t,DateUtils.parse(value.toString()));
    						}else if(declaredField.getType() == Timestamp.class){
    							declaredField.set(t,DateUtils.parse(value.toString()));
    						}else if(declaredField.getType() == Double.class){
    							declaredField.set(t,new Double(value.toString()));
    						}else if(declaredField.getType() == Float.class){
    							declaredField.set(t,new Float(value.toString()));
    						}else if(declaredField.getType() == Integer.class){
    							declaredField.set(t,new Integer(value.toString()));
    						}else if(declaredField.getType() == BigDecimal.class){
    							declaredField.set(t,new BigDecimal(value.toString()));
    						}
    					}
    				} catch (Exception e) {
    					throw new Exception("The "+value+ " of "  +mapName+" data format is "+" error.");
    				}
    			}
        	}
    	}
    	return t;
    }
   
    /**
     * 
     * @param t
     * @return
     */
    public static<T> Map<String,Object> entity2Map(T t) {
    	Map<String,Object> map = new HashMap<String,Object>();
    	try {
			if(t!=null) {
				Class<?> clazz = t.getClass();
				Field [] field=clazz.getDeclaredFields();
				for (int i = 0 ; i< field.length; i++) {
					Field declaredField = field[i];
					declaredField.setAccessible( true );
					Object obj = declaredField.get(t);
					if(obj!=null){
					 if(obj instanceof String){
		                	map.put(declaredField.getName(), obj+"");
						}else if(obj instanceof Boolean){
							map.put(declaredField.getName(), Boolean.valueOf(obj.toString()));
						}else if(obj instanceof Integer){
							map.put(declaredField.getName(), Integer.valueOf(obj.toString()));
						}else if(obj instanceof Double){
							map.put(declaredField.getName(), Double.valueOf(obj.toString()));
						}else if(obj instanceof Float){
							map.put(declaredField.getName(), Float.valueOf(obj.toString()));
						}else if(obj instanceof Date){
							map.put(declaredField.getName(), DateUtils.parse(obj.toString()));
						}else if(obj instanceof Timestamp){
							map.put(declaredField.getName(),DateUtils.parse(obj.toString()));
						}else if(obj instanceof BigDecimal){
							map.put(declaredField.getName(), new BigDecimal(Long.valueOf(obj.toString())));
						}
					}
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		} 
    	return map;
    }
}
```
代码中使用的DateUtils工具类在[上一篇文章](https://108day.github.io/java/2018/12/26/java-core.html),也可以在[源码](https://github.com/108day/java-common-utils)中找到 
使用反射虽然开始时会损失性能，但是如果这段代码经常被调用时就会被JVM自动生成热点代码，性能和其他代码没有什么区别。

反射是框架的核心工具，掌握反射的API很有必要！

更多的一些工具请看[源码](https://github.com/108day/java-common-utils)
