# Ask

我看了[PHP Manual MySQL 函数 取得结果中指定字段的字段名](https://www.xuchao.org/docs/php/function.mysql-field-name.html)

***說 mysql_field_name 這是(PHP 4, PHP 5) 的語法，***  

```
$str    ="select * from EMD ";        //取這個檔案  
$result =mysql_query($str) or die("errow: $str \n");  
$fields = mysql_num_fields($result);  //計算欄位數目  
$row    = mysql_fetch_field($result); //取欄位資訊  
// 列出各個欄位資料  
$xno=0;  
for ($i=0; $i<$fields; $i++) {  
    $xno++;  
    $row=mysql_fetch_field($result);  
    $fname[$i] = mysql_field_name ($result,$i); //欄名  
    $ftype[$i] = mysql_field_type ($result,$i); //型態  
    $flen[$i]  = mysql_field_len  ($result,$i); //長度  
    $fflags[$i]= mysql_field_flags($result,$i); //  
    echo "\$xno: ".$xno." \$fname[".$i."]  is ".$fname[$i].";  ".$ftype[$i].";  ".$flen[$i].";  ".$fflags[$i]."; <br";  
}  
exit();  
```

***到(PHP 5, PHP 7, PHP 8) 版本換了不同寫法***  

```
mysqli_fetch_field_direct() [name] or [orgname]  
PDOStatement::getColumnMeta() [name]  
```
***我想寫一段，除了取得某table的欄名、型態、長度...  
想要取得 「中文備註說明」 那部份，你們知道怎麼寫嗎?***

## 蕭又誠答:
```
select COLUMN_NAME,COLUMN_COMMENT from INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'DS' AND TABLE_SCHEMA='LFH' 
```
洪哲文備註:  
在**information_schema**有個table叫 **COLUMNS**，其中的
**COLUMN_NAME** 專門紀錄所有欄位名稱 ， **COLUMN_COMMENT** 記欄位備註 ，TABLE_NAME 是資料表名稱 ，TABLE_SCHEMA 則是資料庫名稱(如果已連到該資料庫，這段可省略)，所以

```
select * from INFORMATION_SCHEMA.COLUMNS; #可以看到所有登錄的TABLE_CATALOG	TABLE_SCHEMA	TABLE_NAME	COLUMN_NAME ....等等

select * from INFORMATION_SCHEMA.COLUMNS where TABLE_NAME = 'EMD' AND TABLE_SCHEMA='LFH' ; 
 #紀錄了欄位的各種屬性，其中有一欄叫 COLUMN_COMMENT 就是在紀錄這欄的備註的。而
 
select COLUMN_NAME,COLUMN_TYPE,COLUMN_COMMENT from INFORMATION_SCHEMA.COLUMNS where TABLE_NAME = 'EMD' AND TABLE_SCHEMA='LFH' ;  
#這樣就可以掌握各欄的型態與備註說明
```




