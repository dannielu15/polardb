# ST\_attrIntFilter {#reference_bqs_b2q_3gb .reference}

指定轨迹属性字段，根据固定值，过滤出符合条件的轨迹点，返回过滤后的属性字段值（数组）。

## 语法 {#section_og1_4hn_qfb .section}

```
int8[] ST_attrIntFilter(trajectory traj, cstring attr_field_name,cstring operator, int8 value);
int8[] ST_attrIntFilter(trajectory traj, cstring attr_field_name,cstring operator, int8 value1, int8 value2);
```

## 参数 {#section_cxv_qhn_qfb .section}

|参数名称|描述|
|----|--|
|traj|轨迹对象attr。|
|attr\_field\_name|指定的属性名称。|
|operator|过滤符'=','!=','\>', '<', '\>=', '<=', '\[\]', '\(\]', '\[\)', '\(\)'.|
|value、value1|属性固定值、下限。|
|value2|属性固定值-上限|

## 描述 {#section_qmn_klq_3gb .section}

只支持类型为integer的属性字段。

## 示例 {#section_lmw_qhn_qfb .section}

```
create table traj(id integer, traj trajectory);
insert into traj values(ST_makeTrajectory('STPOINT'::leaftype, 'LINESTRING(-179.48077 51.72814,-179.46731 51.74634,-179.46502 51.74934,-179.46183 51.75378,-179.45943 51.75736,-179.45560 51.76273,-179.44845 51.77186,-179.43419 51.78977,-179.41259 51.81643,-179.41001 51.81941,-179.40751 51.82223,-179.40497 51.82505,-179.40242 51.82796,-179.39981 51.83095,-179.39734 51.83398,-179.39499 51.83709)'::geometry, ARRAY['2017-01-15 09:06:39'::timestamp,'2017-01-15 09:14:48','2017-01-15 09:13:39','2017-01-15 09:16:28','2017-01-15 09:19:48','2017-01-15 09:17:48','2017-01-15 09:23:19','2017-01-15 09:34:40','2017-01-15 09:30:28','2017-01-15 09:36:59','2017-01-15 09:38:09','2017-01-15 09:39:18','2017-01-15 09:40:40','2017-01-15 09:47:38','2017-01-15 21:18:30','2017-01-15 09:48:49'], '{"leafcount": 16, "attributes" : {"heading" : {"type": "integer", "length": 4, "nullable" : false,"value":[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]}}}'));


select st_attrIntFilter(traj, 'heading', '>', 5) from traj where id = 1;
      st_attrintfilter       
-----------------------------
 {6,7,8,9,10,11,12,13,14,15}
(1 row)

select st_attrIntFilter(traj, 'heading', '>=', 5) from traj where id = 1;
       st_attrintfilter        
-------------------------------
 {5,6,7,8,9,10,11,12,13,14,15}
(1 row)

select st_attrIntFilter(traj, 'heading', '<', 5) from traj where id = 1;
 st_attrintfilter 
------------------
 {0,1,2,3,4}
(1 row)

select st_attrIntFilter(traj, 'heading', '<=', 5) from traj where id = 1;
 st_attrintfilter 
------------------
 {0,1,2,3,4,5}
(1 row)

select st_attrIntFilter(traj, 'heading', '=', 5) from traj where id = 1;
 st_attrintfilter 
------------------
 {5}
(1 row)

select st_attrIntFilter(traj, 'heading', '!=', 5) from traj where id = 1;
           st_attrintfilter            
---------------------------------------
 {0,1,2,3,4,6,7,8,9,10,11,12,13,14,15}
(1 row)

select st_attrIntFilter(traj, 'heading', '()', 5,8) from traj where id = 1;
 st_attrintfilter 
------------------
 {6,7}
(1 row)

select st_attrIntFilter(traj, 'heading', '(]', 5,8) from traj where id = 1;
 st_attrintfilter 
------------------
 {6,7,8}
(1 row)

select st_attrIntFilter(traj, 'heading', '[)', 5,8) from traj where id = 1;
 st_attrintfilter 
------------------
 {5,6,7}
(1 row)

select st_attrIntFilter(traj, 'heading', '[]', 5,8) from traj where id = 1;
 st_attrintfilter 
------------------
 {5,6,7,8}
(1 row)
```

