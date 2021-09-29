## ABAP 多表查询

![img](C:\Users\yesen\AppData\Local\Temp\企业微信截图_16326433337117.png)

一： 数据来源

lips

项目： POSNR ：POSNR_VL

物料：MATNR：MATNR

单位：VRKME： VRKME

描述：ARLKTX：ARKTX

lipsd

交货数量：G_LFIMG ： LFIMG



likp

凭证日期： BLDAT ：BLDAT

出库交货： :     vbeln_vl

//查询订单 vl03n;



二，多表查询两种方法            

inner join(等值连接) 只返回两个表中联结字段相等的行
left join(左联接) 返回包括左表中的所有记录和右表中联结字段相等的记录
right join(右联接) 返回包括右表中的所有记录和左表中联结字段相等的记录

INNER JOIN 语法：

INNER JOIN 连接两个数据表的用法：
SELECT * FROM 表1 INNER JOIN 表2 ON 表1~字段号=表2~字段号

INNER JOIN 连接三个数据表的用法：
SELECT * FROM (表1 INNER JOIN 表2 ON 表1~字段号=表2~字段号) INNER JOIN 表3 ON 表1~字段号=表3~字段号

INNER JOIN 连接四个数据表的用法：
SELECT * FROM ((表1 INNER JOIN 表2 ON 表1~字段号=表2~字段号) INNER JOIN 表3 ON 表1~字段号=表3~字段号) INNER JOIN 表4 as Member ON Member~字段号=表3~字段号

INNER JOIN 连接五个数据表的用法：
SELECT * FROM (((表1 INNER JOIN 表2 ON 表1.字段号=表2.字段号) INNER JOIN 表3 ON 表1.字段号=表3.字段号) INNER JOIN 表4 ON Member~字段号=表4~字段号) INNER JOIN 表5 ON Member~字段号=表5~字段号

inner join 内部每行具有相同字段的不同表的数据放在同一行

