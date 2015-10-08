---
layout: post
title: Mybatis3 判断字符串
categories:
- J2EE笔记
---

在使用Mybatis3过程中发现一个奇怪的问题，判断字符串必须要用指定的格式

mapper内如下：

```xml
<choose>
	<when test="regOrSign != null and regOrSign == 'R' ">
    	ORDER BY a.registrationDate DESC
    </when>
    <otherwise>
        ORDER BY a.signDate DESC
    </otherwise>
</choose>
    
```

报错：
> \### Error querying database.  Cause: java.lang.NumberFormatException: For input string: "R"
\### Cause: java.lang.NumberFormatException: For input string: "R"] with root cause
java.lang.NumberFormatException: For input string: "R"

</br>

`test="regOrSign != null and regOrSign == 'R' "` 

-> `test='regOrSign != null and regOrSign == "R" '`

改成这样就可以了，这个问题同样适用if标签