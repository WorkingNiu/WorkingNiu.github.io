# Oracle去除某一列值重复的数据


业务中去除一些无法避免的脏数据

<!--more-->

## 1 问题

在项目中有一张设备检测信息表DEVICE_INFO_TBL, 每个设备每天都会产生一条检测信息，现在需要从该表中检索出每个设备的最新检测信息。也就是device_id字段不能重复，消除device_id字段重复的记录，而且device_id对应的检测信息test_result是最新的。

## 2 解决思路

用Oracle的row_number() over函数来解决该问题。

## 3 解决过程

### 查看表中的重复记录
```sql
select
    t.id,
    t.device_id,
    t.update_dtm,
    t.test_result
from DEVICE_INFO_TBL t
```

查询结果

|id     |device_id       |update_dtm      |test_result       |
|---------|-----------|---------|-----------| 
| 1     | 21     | 2019-4-3     | 正常
| 2    | 21       | 2019-4-4     | 异常   
| 3     | 22 | 2019-4-3     | 警告
| 4     | 22 | 2019-4-4     | 正常
| 5    | 23 | 2019-4-4     | 异常

### 标记重复的记录
```sql
select
    t.id,
    t.device_id,
    t.update_dtm,
    t.test_result,
    row_number() OVER(PARTITION BY device_id ORDER BY t.update_dtm desc) as row_flg   
from DEVICE_INFO_TBL t
```
查询结果

|id     |device_id       |update_dtm      |test_result   |row_flg       |
|---------|-----------|---------|-----------| -----------| 
| 1     | 21     | 2019-4-3     | 正常| 1
| 2    | 21       | 2019-4-4     | 异常| 2   
| 3     | 22 | 2019-4-3     | 警告| 1
| 4     | 22 | 2019-4-4     | 正常| 2
| 5    | 23 | 2019-4-4     | 异常| 1

### 过滤重复数据

```sql
select
    temp.id,
    temp.device_id,
    temp.update_dtm,
    temp.test_result
from (
         select
             t.id,
             t.device_id,
             t.update_dtm,
             t.test_result,
             row_number() OVER(PARTITION BY device_id ORDER BY t.update_dtm desc) as row_flg   
          from DEVICE_INFO_TBL t ) temp
where temp.row_flg  = '1'
```

查询结果

|id     |device_id       |update_dtm      |test_result       |
|---------|-----------|---------|-----------| 
| 2    | 21       | 2019-4-4     | 异常   
| 3     | 22 | 2019-4-3     | 警告
| 5    | 23 | 2019-4-4     | 异常

row_number() OVER (PARTITION BY COL1 ORDER BY COL2) 表示根据COL1分组，在分组内部根据 COL2排序，而此函数计算的值就表示每组内部排序后的顺序编号（组内连续的唯一的).

　　与rownum的区别在于：使用rownum进行排序的时候是先对结果集加入伪列rownum然后再进行排序，而此函数在包含排序从句后是先排序再计算行号码．

　　row_number()和rownum差不多，功能更强一点（可以在各个分组内从1开时排序）．

　　rank()是跳跃排序，有两个第二名时接下来就是第四名（同样是在各个分组内）．

　　dense_rank()l是连续排序，有两个第二名时仍然跟着第三名。相比之下row_number是没有重复值的 ．

　　lag（arg1,arg2,arg3):
arg1是从其他行返回的表达式
arg2是希望检索的当前行分区的偏移量。是一个正的偏移量，时一个往回检索以前的行的数目。
arg3是在arg2表示的数目超出了分组的范围时返回的值.