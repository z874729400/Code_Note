# Zend案件总结01_数据库查询和处理

## 1.用于统计jp的个数和en的个数

```PHP
    $sql2 = "SELECT

​        SUM(CASE WHEN lang_div = '1' THEN 1 ELSE 0 END) AS jp,

​        SUM(CASE WHEN lang_div = '2' THEN 1 ELSE 0 END) AS en

​        FROM eveautonomy_contact

​        where reg_date = to_char(to_date('$today', 'YYYYMMDD') - 1, 'yyyy/mm/dd');";‘
```



## 2.数据的查询和处理

```PHP
 $sql1 = sprintf(

​          "SELECT A.user_key,A.contact_type,A.contact_content,A.lang_div,A.dl_content,A.company,A.department,A.name,A.email,A.tel,A.country,A.reg_date,A.reg_time,B.dl_content as Bdl_content,B.reg_date as Breg_date,B.reg_time as Breg_time

​          FROM eveautonomy_contact as A LEFT JOIN eveautonomy_contact_dl as B   

​          ON

​          A.user_key = B.user_key

​          WHERE

​          A.reg_date = to_char(to_date('$today', 'YYYYMMDD') - 1, 'yyyy/mm/dd')

​          ORDER BY A.lang_div, A.reg_date, A.reg_time, Breg_date;");

  $result1 = pg_query($sql1);

  //クエリを実行する

  $resurtdata = pg_fetch_all($result1);

  $len = count($resurtdata);

  for ($j = 0; $j < $len-1; $j++) {

​    if ($resurtdata[$j]['user_key'] == $resurtdata[$j + 1]['user_key']&&$resurtdata[$j]['bdl_content'] == $resurtdata[$j + 1]['bdl_content']) {

​      array_splice($resurtdata,$j+1,1); //截掉自己，如果使用unset，键不会重置，用array_splice则可以重置

​      $len = $len-1;

​    }

​    if($j+1>=$len){

​      break;

​    }

​    if($resurtdata[$j]['user_key'] == $resurtdata[$j + 1]['user_key']&&$resurtdata[$j]['bdl_content'] != $resurtdata[$j + 1]['bdl_content']){

​      $resurtdata[$j+1]['contact_type']='';

​      $resurtdata[$j+1]['contact_content']='';

​      $resurtdata[$j+1]['dl_content']='';

​      $resurtdata[$j+1]['company']='';

​      $resurtdata[$j+1]['department']='';

​      $resurtdata[$j+1]['name']='';

​      $resurtdata[$j+1]['email']='';

​      $resurtdata[$j+1]['tel']='';

​      $resurtdata[$j+1]['country']='';

​      $resurtdata[$j+1]['reg_date']='';

​      $resurtdata[$j+1]['reg_time']='';

​    }  
  }
```

