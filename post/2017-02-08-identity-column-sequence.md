+++
thumbnail = ""
tags = ["oracle"]
categories = ["DevOps"]
date = "2017-02-08T10:53:03+08:00"
description = "查看 identity column 关联的 sequence name"
title = "在 Oracle 12c 中，查看 identity column 关联的 sequence name"
+++

在 Oracle 12c 中，新增加了 identity column 的功能。创建表的时候可以指定一个 column 为 identity。Oracle 会自动创建相关联的 sequence。

在一些特殊情形下，我们需要知道这个 sequence 的名字，比如要知道当前 sequence 的值。这时我们可以通过如下 SQL 来找到这个 sequence 的名字。


```
select table_name,data_default 
  from all_tab_columns 
  where identity_column = 'YES' and table_name='TEST';
```

> 避免在程序中直接使用这个 sequence 的名字。
