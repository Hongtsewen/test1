# About_zscmt2.php

*在新太系統的所有程式，如果沒有在xdxd_menu做登記的話，執行時就會碰到說 **您無權使用本功能** 的回答！   
我試著在lkhy/module/下放入昀達的編程器程式zscmt2.php果然如此！因此，我用以下方法，在xdxd_menu上登錄了資料。而且執行此程式時，還會產稱一支叫htw_try.php程式，因此最後，我也幫這程式都登記進去了。過程如下:*

---

***我想先看看，xdxd_menu是怎樣登記某支程式的!***

2020/09/24我到harbor這樣查了一下xdxd_menu: `select * from xdxd_menu where target_ZH_TW='zscmtest1.php' ;`  
可以看到這支程式登記內容如下：  
```
id = 9011
func = 1  #動作#(1:執行;2:列印;3:拋出;4:管理)
group = 70 群組#
parent = 1473 #父層級#
order = 95 #順序#
name_ZH_TW = 程式產生器(2022)  #程式名#
target_ZH_TW = zscmtest1.php #檔名或路徑#
name_ZH_CN = 程式产生器(2022) #程式名#簡體中文
target_ZH_CN = zscmtest1.php #檔名或路徑#
enable = 1  #使用否＃
hotbar = 0  #抬頭列#
hotbar_order = 0 #抬頭列序#
click = 0   #敲按#
```

我執行了`http://127.0.0.1/lkhy/xdxd_menu_manager.php` [請按](http://127.0.0.1/lkhy/xdxd_menu_manager.php) 然後選擇在-程式設計師區當前流水號[ 9012 ]-按[新增程設區] 填入資料後，它執行了以下指令:  
資料已新增！參考SQL語法如下：   

```
xdxd_menu:
INSERT INTO `xdxd_menu` (`id`, `func`, `group`, `parent`, `order`, `name_ZH_TW`, `target_ZH_TW`, `name_ZH_CN`, `target_ZH_CN`, `enable`, `hotbar`, `hotbar_order`, `click`) VALUES (9013, 1, 1000, 1473, 99, '編程器t2', 'zscmt2.php', '編程器t2', 'zscmt2.php', 1, 0, 0, 0);

xdxd_group:
REPLACE INTO `xdxd_group` VALUES ('10','9013');
```

***這樣做完，果然就可以執行 了!***

只是因SYSTBR裡面 都沒有打勾，我試著隨便勾了一些，按執行後，他回答說： 
```
請檢查config.inc.php的引用路徑是否正確! systbr_head_update:REPLACE INTO SYSTBR(`FTYPE`,`FLEN`,`FNAME`,`CTOPIC`,`PADD`,`PUPD`,`ORD`,`RCN`,`TBNAME`) values('date','0','DATE','','on','on','100','36','xsalary')
ERROR IN UPDATE REPLACE INTO SYSTBR(`FTYPE`,`FLEN`,`FNAME`,`CTOPIC`,`PADD`,`PUPD`,`ORD`,`RCN`,`TBNAME`) values('date','0','DATE','','on','on','100','36','xsalary')  

Field 'NOTE' doesn't have a default value
```
***這是因為我用mariaDB限制較嚴的關係，因此我去SYSTBR的結構： 把NOTE的[無]改為 null***  
命令句如下: `ALTER TABLE `SYSTBR` CHANGE `NOTE` `NOTE` TEXT CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL;`

***解決後，又發現這個問題！***
```
Unknown column 'PUPD_IDX' in 'field list'

請檢查config.inc.php的引用路徑是否正確! systbr_head_update:REPLACE INTO SYSTBR(`FTYPE`,`FLEN`,`FNAME`,`CTOPIC`,`PADD`,`PSER`,`PLST`,`PUPD`,`PUPD_IDX`,`ORD`,`RCN`,`TBNAME`) values('date','0','DATE','','on','on','on','on','on','100','36','xsalary')
ERROR IN UPDATE REPLACE INTO SYSTBR(`FTYPE`,`FLEN`,`FNAME`,`CTOPIC`,`PADD`,`PSER`,`PLST`,`PUPD`,`PUPD_IDX`,`ORD`,`RCN`,`TBNAME`) values('date','0','DATE','','on','on','on','on','on','100','36','xsalary')
Unknown column 'PUPD_IDX' in 'field list'
```

這下子發現有些地方用的SYSTBR是舊版，新版的 蕭又誠有加欄位還有中文註解：  
>  
>akira查🤣LKHY.SYSTBR確實是沒有 PUPD_IDX 這一欄(有的是只有PUPD) 我來看看🤣akira.HARBOR2.SYSTBR 的也沒有  
>select * from SYSTBR再查localhost.HARBOR.SYSTBR是有的, 但是🤣LKHY.SYSTBR 也是沒有PUPD_IDX 這一欄的, 再查LFH.SYSTBR也是有的,  
>我把loclhost上的HARBOR.SYSTBR昨成 SYSTBR.sql檔，準備去換掉那些舊版的。先換localhost的LKHY的。  
>  

我還需要把htw_try.php這程式 寫入xdxd_menu中 (用 `http://127.0.0.1/lkhy/xdxd_menu_manager.php` ) [請按](http://127.0.0.1/lkhy/xdxd_menu_manager.php)
資料已新增！參考SQL語法如下：  

```
xdxd_menu:
INSERT INTO `xdxd_menu` (`id`, `func`, `group`, `parent`, `order`, `name_ZH_TW`, `target_ZH_TW`, `name_ZH_CN`, `target_ZH_CN`, `enable`, `hotbar`, `hotbar_order`, `click`) VALUES (9014, 1, 1000, 1473, 95, '測試用', 'htw_try.php', '測試用', 'htw_try.php', 1, 0, 0, 0);

xdxd_group:
REPLACE INTO `xdxd_group` VALUES ('10','9014');
```

***❤️ `http://127.0.0.1/lkhy/zscmt2.php` 生成的htw_try.php沒有在module/下，還要去mv一下。***

***❤️ 這樣可以關閉視窗 `<input type="button" value="關閉" onclick="window.close()" />`***

