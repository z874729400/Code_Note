# Zend案件总结02_PHPExcel

## 1.js中的Ajax上传文件：//异步上传

```PHP
new AjaxUpload("updFile",{

​    action:"XXXX",

​    name:"updFile",

​    //发送前处理

​    onSubmit: function(file,ext){}

​    //完了时的处理

​    onComplete: function(file,data){

​           var response = decodeURIComponent(data);//似乎是对data进行解码

​           if(response.match(/^success:/)){}//成功时处理

​           else if (response.match(/^error:/)){}//失败时处理、输出失败信息data[0]

​           else{}//其他情况

​     }

})
```

## **2.同步跳转到控制器，button失效的情况**

```html
解：

<button class="btnForward" style="width: 235px;" onClick="javascript:location.href='/Amazon/Direct/outflex'; return false;">FLEX用データ出力button>

onClick = “javascript:location.href='Amazon/Direct/outflex';return false;”

加上标红的就可以解决
```

## **3.session的使用**

```php
在init里面初始化、session似乎还跟命名空间有关系

$session = new Zend_Session_Namespace('ysgearkanri');

​    **if (empty($session->isec_userid)) {**

​      **return $this->_redirect('login');**

​    **}
```

**//用来判断是否登录**

## **4.获取数据库配置错误**

```php
$this->_ysgear = new DirectModel($this->_config->ysgeardatasource->database->toArray());
```

## **5.配置Model**

```PHP
require_once 'Zend/Db.php';

class DirectModel

{

  private $_db;// データベースアダプタのハンドル

  /**

   \* コンストラクタ

   *

   */

  public function __construct($ysgear)

  {

​    global $_db;

​    // 接続情報を取得する

​    if (!isset($ysgear) || count($ysgear) < 1) {

​      throw new Zend_Exception(__FILE__ . '(' . __LINE__ . '): ' . 'データベース接続情報が取得できませんでした。');

​    }

​    // データベースに接続する

​    if (is_null($_db)) {

​      // データベースの接続パラメータを定義する

​      $params = array(  'host'   => $ysgear['host'],

​                'username' => $ysgear['user'],

​                'port'   => $ysgear['port'],

​                'charset' => 'UTF8',

​                'password' => $ysgear['password'],

​                'dbname'  => $ysgear['dbname']

​      );

​      try {

​        // データベースアダプタを作成する

​        $_db = Zend_Db::factory($ysgear['type'], $params);

​        // データ取得形式を設定する

​        $_db->setFetchMode(Zend_Db::FETCH_ASSOC);

​      } catch (Zend_Exception $e) {

​        throw new Zend_Exception(__FILE__ . '(' . __LINE__ . '): ' . $e->getMessage());

​      }

​    }

​    $this->_db = $_db;

 }
```

## **6.数据库查询**

```php
    $select = $this->_db->select()->from('azn_trn_buyhistory_direct','MAX(reg_date)');

​   $ret = $select->query()->fetch();
```

​        

## **7.删除整张表的数据**

```php
$ret = $this->_db->delete('azn_trn_buyhistory_direct');
```

## **8.给表的主键id重新设为1**

```php
      $seqclear_wk ="select setval ('azn_trn_buyhistory_direct_renno_seq', 1, false)";

​      $seqclear = $this->_db->query($seqclear_wk);
```

## **9.插入数据**

```php
$this->_db->insert('azn_trn_buyhistory_direct',$data)
```

## **10.字符串替换 str_replace的使用**

```php
str_replace('，','',$data['azn_price]);

str_replace(array("\r\n","\r","\n"),"",$data)//去掉$data字符串中的换行符
```

## **11.联表查询**

```php
  $select = $this->_db->select()->from('azn_mst_product',array('product_no','shipping_product_name'))->join('azn_trn_buyhistory_direct','azn_trn_buyhistory_direct.asin_no=azn_mst_product.asin_no');

   $res1 = $select->query()->fetchAll();
```

## **12.给数字加逗号** 

```php
   number_format($price);
```

## **13.PHPExcel的使用**

### **1.读取xlsx和xls的excel文件**

```PHP
use PhpOffice\PhpSpreadsheet\Reader\Xlsx as XlsxReader;

​      if ($last=='.xlsx'||$last=='.xls') {

​       $reader = new XlsxReader();

​       $spreadsheet = $reader->load($file);

 

​       $sheet = $spreadsheet->getSheet(0);

​       $highestRow = $sheet->getHighestRow();

​       $highestColumn = $sheet->getHighestColumn();

​       $rowData = [];

​       for($row=2;$row<=$highestRow;$row++){

​        array_push($rowData, $sheet->rangeToArray('A' . $row . ':' . $highestColumn . $row, NULL, TRUE, FALSE));

​       }

​       $this->_ysgear->beginTranction();

​       try{

​         if (!empty($rowData)) {

​          $this->_ysgear->deleteDirect();

​         }

​         foreach ($rowData as $value){

​          foreach ($value as $val){

​            $this->_ysgear->insertRowData($val);

​          }

​         }

​        }catch (Zend_Exception $e) {

​          throw new Zend_Exception(__FILE__ . '(' . __LINE__ . '): ' . $e->getMessage());

​          $this->_ysgear->rollback();

​        }

​       $this->_ysgear->commit();

​       return $this->getResponse()->setBody(rawurlencode('success:' . $rowData));

​      }
```

### **2.读取csv后缀的excel文件**

```PHP
use PhpOffice\PhpSpreadsheet\IOFactory;

​      if ($last=='.csv'){

​       $reader = \PhpOffice\PhpSpreadsheet\IOFactory::createReader("Csv"); // 指定为xlsx格式

​       $spreadsheet = $reader->load($file);

​       $sheet = $spreadsheet->getSheet(0);

​       $highestRow = $sheet->getHighestRow();

​       $highestColumn = $sheet->getHighestColumn();

​       $rowData = [];

​       for($row=2;$row<=$highestRow;$row++){

​        array_push($rowData, $sheet->rangeToArray('A' . $row . ':' . $highestColumn . $row, NULL, TRUE, FALSE));

​       }

​       if (!empty($rowData)){

​         $this->_ysgear->deleteDirect();

​       }

​       foreach($rowData as $value){

​         foreach($value as $val){

​           $this->_ysgear->insertRowData($val);

​         }

​       }

​       return $this->getResponse()->setBody(rawurlencode('success:' . $rowData));

​      }
```

### **3.以一个EXCEL文件的格式为模板，输出**

```PHP
use PhpOffice\PhpSpreadsheet\Writer\Xlsx as XlsxWriter;

use PhpOffice\PhpSpreadsheet\Writer\CSV as CSVWriter;

use PhpOffice\PhpSpreadsheet\Style\Alignment;

use PhpOffice\PhpSpreadsheet\Style\Border;

use PhpOffice\PhpSpreadsheet\Cell\DataType;

​     $source = PUBLIC_DIR . "/Amazon/out_template/flex_direct_template.xlsx";

​     $reader = new XlsxReader();

 

​     // シートを指定してインスタンス作成

​     // flex_template.xlsx内の"1．通常受注"のシートをメモリ上に取ってくるイメージ

​     $reader->setLoadSheetsOnly( array("1．通常受注") );

​     $spreadsheet = $reader->load($source);

  

​     // ロードしたシートの中から"1．通常受注"シートを$sheetとする

​     $sheet = $spreadsheet->getSheetByName("1．通常受注");

 

​     $itemno = 1;

​     foreach ($getflexList as $eachRow) {

​       $itemno = $itemno + 1;

​       // A1に"データ"というテキストを入れる

​       $sheet->setCellValue("A".$itemno, "00080");

​       $sheet->setCellValue("B".$itemno, "16237");

​       $sheet->setCellValueExplicit("C".$itemno,$eachRow['product_no'],\PhpOffice\PhpSpreadsheet\Cell\DataType::TYPE_STRING);

​       $sheet->setCellValue("D".$itemno, $eachRow['amount']);

​       $sheet->setCellValue('E'.$itemno, $eachRow['azn_price']);

​       $sheet->setCellValue("P".$itemno, $eachRow['po_no']);

​       $sheet->setCellValue("Q".$itemno, $eachRow['name']);

​       $sheet->getStyle('E'.$itemno)->getAlignment()->setHorizontal(\PhpOffice\PhpSpreadsheet\Style\Alignment::HORIZONTAL_RIGHT);

​    }

​    $highestRow = count($getflexList) + 1;

​    $sheet->getStyle('A2:'.'U'.$highestRow)->getBorders()->getAllBorders()->setBorderStyle(\PhpOffice\PhpSpreadsheet\Style\Border::BORDER_THIN);

​     //ダウンロード用

​     //MIMEタイプ：https://technet.microsoft.com/ja-jp/ee309278.aspx

​     header("Content-Description: File Transfer");

​     header('Content-Disposition:attachment;filename="flex_direct_out.xlsx"');

​     header('Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');

​     header('Content-Transfer-Encoding: binary');

​     header('Cache-Control: must-revalidate, post-check=0, pre-check=0');

​     header('Expires: 0');

​     ob_end_clean(); //バッファ消去

 

​     // Excel(.xlsx)として開く

​     $writer = new XlsxWriter($spreadsheet);

​     $writer->save('php://output');
```

 

​     