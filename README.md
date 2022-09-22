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


