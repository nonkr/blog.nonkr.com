---
title: sqlite tutorial - json
date: 2017-05-14 12:58:31
categories: sqlite
tags: sqlite
---

SQLite从3.9.0(2015-10-14)版本开始支持JSON格式（[http://sqlite.org/releaselog/3_9_0.html](http://sqlite.org/releaselog/3_9_0.html)），本篇记录一下学习过程。
[SQLite - JSON官方文档](http://sqlite.org/json1.html)

## 编译安装
这里使用当前最新的3.18.0版本
```bash
yum install readline-devel -y
wget -c "http://sqlite.org/2017/sqlite-autoconf-3180000.tar.gz
tar zxvf sqlite-autoconf-3180000.tar.gz
cd sqlite-autoconf-3180000/
env CFLAGS="-Os" ./configure --enable-json1
make
make install
```

## 函数列表
```text
json(json)
json_array(value1,value2,...)
json_array_length(json)
json_array_length(json,path)
json_extract(json,path,...)
json_insert(json,path,value,...)
json_object(label1,value1,...)
json_patch(json1,json2)
json_remove(json,path,...)
json_replace(json,path,value,...)
json_set(json,path,value,...)
json_type(json)
json_type(json,path)
json_valid(json)
json_quote(value)
json_group_array(value)
json_group_object(name,value)
json_each(json)
json_each(json,path)
json_tree(json)
json_tree(json,path)
```

<!-- more -->

## 使用示例
```sql
select json_object('ex',json('[52,3.14159]'));
-- {"ex":[52,3.14159]}
select json_extract('{"a":2,"c":[4,5,{"f":7}]}', '$');
-- '{"a":2,"c":[4,5,{"f":7}]}'
select json_extract('{"a":2,"c":[4,5,{"f":7}]}', '$.c');
-- '[4,5,{"f":7}]'
select json_extract('{"a":2,"c":[4,5,{"f":7}]}', '$.c[2]');
-- '{"f":7}'
select json_extract('{"a":2,"c":[4,5,{"f":7}]}', '$.c[2].f');
-- 7
select json_extract('{"a":2,"c":[4,5],"f":7}','$.c','$.a');
-- '[[4,5],2]'
select json_extract('{"a":2,"c":[4,5,{"f":7}]}', '$.x');
-- NULL
select json_extract('{"a":2,"c":[4,5,{"f":7}]}', '$.x', '$.a');
-- '[null,2]'
select json_remove('[0,1,2,3,4]','$[1]','$[1]');
-- [0,3,4]
```
