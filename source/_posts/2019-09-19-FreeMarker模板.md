---
 layout: post
 title:  "FreeMarker模板" 
 date:  2019-09-22 00:20:04 +0800
--- 
FreeMarker的使用
Apache FreeMarker是一个模板引擎，通过他我们可以根据模板生成html、java代码等文件，这在文件结构类似的的情况下可以避免我们很多重复操作。

配置模板路径：不要重新建立配置对象，很费资源。
Configuration conf = new Configuration(Configuration.VERSION_2_3_27);
conf.setDirecotoryForTemplateLoading(new File(“D:/confs/”));
conf.setDefaultEncoding(“utf-8”);

数据模型：
FreeMarker里面的数据模型，可以是JavaBean，也可以是Map对象。同时这两个数据结构里面都支持内嵌对象。

加载模板：
Template template = conf.getTemplate(“test.ftlh”);
// 会从配置的目录里面去读取模板数据

数据和模板结合输出：
Writer out = new OutputSteamWriter(System.out); // 也可以输出到文件
template.process(root, out); // root 是数据模型对象。
// 注意时刻关闭out对象

模板文件的书写：
FTL（FreeMarker Template Language）:
（1）	注释： <#-- something to say -->
（2）	插入符\${data}只能用在显示的数据的地方，不能用在判断地方<#if \${big}> </#if> 错误的用法
  预定义指令：
（1）	列表： <#list animals as animal> ​\${animal.name}</#list>
（2）	变量：${name} , 对象属性：${production.name}
（3）	条件：<#if something></#if>
（4）	从hash中获取数据：​\${book.name} 或者 ​\${book[“name”]}
（5）	赋值：<#assign s = “ hello freemarker”>
（6）	获取字符串字符： ${name[0]} , name 首字母
（7）	字符串截取：${name[start..end]}
（8）	逻辑判断符号： > gt; >= gte; < lt ; <= lte; //freemarker会把< 当成标签所有要用转义符号
（9）	字符串处理：\${name?upper_case}, \${name?size}，\${name?cap_first}
（10）	默认值：${name!”PeterYe”} // 指定变量的默认值，从!标记