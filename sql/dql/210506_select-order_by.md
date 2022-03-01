# `select`:`order by` 子句

关系数据库设计理论认为：  
**如果不明确规定排序顺序，则不应该假定检索出的数据的顺序有任何意义**

> 如果不进行排序，数据**可能**会以它在表中出现的先后顺序显示

`order by`子句可以对查询结果按照指定的列进行**字母排序**  
在使用`order by`子句时，**应确保它是select的最后一条子句**

下面的例子按照cust_id列的字母顺序对查询结果进行排序： 
```sql
select cust_id from Customers order by cust_id;
```
```
-- 查询结果
Fun4All                                           
Fun4All                                           
Kids Place                                        
The Toy Store                                     
Village Toys                                      
```

下面的例子查询多个列，并按`cust_id`列进行排序：
```sql
select cust_id, cust_address from Customers order by cust_id
```
```
-- 查询结果
1000000001  200 Maple Lane                                    
1000000002  333 South Lake Drive                              
1000000003  1 Sunny Place                                     
1000000004  829 Riverside Drive                               
1000000005  4545 53rd Street                                  
```

`order by`也可以指定多个列进行排序：
```sql
select prod_id, prod_name, prod_price from Products order by prod_id, prod_name;
```
```
-- 查询结果
BNBG01  Fish bean bag toy   3.49
BNBG02  Bird bean bag toy   3.49
BNBG03  Rabbit bean bag     3.49
BR01    8 inch teddy bear   5.99
BR02    12 inch teddy bear  8.99
BR03    18 inch teddy bear  11.99
RGAN01  Raggedy Ann         4.99
RYL01   King doll           9.49
RYL02   Queen doll          9.49
```

