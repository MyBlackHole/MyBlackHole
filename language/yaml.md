# yaml

## YAML语法介绍 

  

### YAML语法介绍 

  

-   YAML是一个类似于XML、JSON的标记性语言。它强调的是以“数据”为中心，并不是以标记语言为重点。因而YAML本身的定义比较简单，号称是“一种人性化的数据格式语言”。 
    
-   YAML的语法比较简单，主要有下面的几个： 
    
    -   大小写敏感。 
        
    -   使用缩进表示层级关系。 
        
    -   缩进不允许使用tab，只允许空格（低版本限制）。 
        
    -   缩进的空格数不重要，只要相同层级的元素左对齐即可。 
        
    -   ‘#’表示注释。 
        
-   YAML支持以下几种数据类型： 
    
    -   常量：单个的、不能再分的值。 
        
    -   对象：键值对的集合，又称为映射/哈希/字典。 
        
    -   数组：一组按次序排列的值，又称为序列/列表。 
        

  

### YAML语法示例 

  

#### YAML常量 
常量，就是指的是一个简单的值，字符串、布尔值、整数、浮点数、NUll、时间、日期 # 布尔类型 c1: true # 整型 c2: 123456 # 浮点类型 c3: 3.14 # null类型 c4: ~ # 使用~表示null # 日期类型 c5: 2019-11-11 # 日期类型必须使用ISO 8601格式，即yyyy-MM-dd # 时间类型 c6: 2019-11-11T15:02:31+08.00 # 时间类型使用ISO 8601格式，时间和日期之间使用T连接，最后使用+代表时区 # 字符串类型 c7: haha # 简单写法，直接写值，如果字符串中间有特殊符号，必须使用双引号或单引号包裹 c8: line1     line2 # 字符串过多的情况可以折成多行，每一行都会转换成一个空格 
 