# Zend案件总结03 

## 1.文件上传功能

```js
    <script src="/fcharter/shared/js/jquery-2.2.0.min.js"></script>
    <script src="/fcharter/shared/js/jquery.easing.1.3.js"></script>
    <script src="/fcharter/shared/js/ajaxupload.js"></script>
    <script src="/fcharter/shared/js/up_image.js"></script>
```

至少需要加载这四个js依赖，才可以使用文件上传功能

```PHP
// ライブラリをロードする
require_once SYSTEM_DIR . '/lib/validate_file.php';    //ファイルエラーチェック
    
protected $_errfile;      // ファイルチェック関数のインスタンス
// 在init中
$this->_errfile = new ValidateFile();

    /**
     * uploadアクション
     * ：アップロードファイル(顔写真画像)を処理する
     */
    public function uploadAction()
    {

        if ( isset($_SERVER['HTTP_REFERER']) === false ){
            $this->_redirect('/Fcharter/Planmainte/list/');
        }

        // アップロードファイルの入力チェックを行う
        $result = $this->checkUpFile('updFile');

        // エラーの場合はメッセージを返す
        if (count($result['errMessage']) > 0) {
            return $this->getResponse()->setBody(rawurlencode('error:' . Zend_Json::encode($result['errMessage'])));
        }

        // 一時ファイルのファイル名を生成する
        $tmpFile = md5(microtime(true));
        // アップロードされたファイルを移動する

        $pathname = '/var/www/html/seastyle_marina/httpdocs/fcharter/tmp/';

        if (!is_dir($pathname)) {
            mkdir($pathname, 0777);
            chmod($pathname, 0777);
        }

        if (!move_uploaded_file($result['fileInfo']['tmp_name'],  $pathname . $tmpFile)) {
            return $this->getResponse()->setBody(rawurlencode('failure'));
        }

        // 一時保存ファイル名を返す
        return $this->getResponse()->setBody(rawurlencode('success:' . $tmpFile));
    }

    /**
     * アップロードファイルをチェックする
     *
     * @param  string $element アップロードフォームの名前
     * @return array ファイル情報
     */
    private function checkUpFile($element)
    {
        // エラーメッセージを設定する
        $message = array(
            'extension'   => '「.jpg」「.JPG」「.png」「.PNG」の拡張子のみ有効です。',
            'fileSize'    => 'ファイルサイズは3MB以内にしてください。',
            'failure'     => 'アップロードに失敗しました。',
            'invalidFile' => '正しいファイルをアップロードしてください。'
        );

        $fileSize = 3145728;      // サイズ上限

        $allowExtArray = array('jpg','png'); // 許可する拡張子

        $allowExt = array_map(function($value)
        {
            return strtolower($value);
        }, $allowExtArray);

        $notEmpty = false;         // 必須フラグ

        $ret = $this->_errfile->check($message, $element, $fileSize, $allowExt, $notEmpty);

        return array(
            'errMessage' => (count($ret['errMessage']) > 0) ? $ret['errMessage'] : array(),
            'fileInfo'   => $ret['fileInfo']
        );
    }
```

## 2.字符串截取和数组函数

```php
substr($plan_no,2);//cp001,可以截取cp，只留001
strstr($boat_name,'|')//返回|第一次出现，之后的字符串
strstr($boat_name,'|',true)//返回|第一次出现，之前的字符串

max($arr)//返回数组中最大的数字，前提必须是int类型，所以需要类型转换（int）
```

## 3.数据库更新操作

```php
    /**
     * update mst_plan_charter 
     *
     * @param   $plan_no　データクエリ
     * 
     * @param   $post [array]　処理の値
     *
     * @return  $res [boolean] 結果
     */

    public function upd_mst_plan_charter($plan_no, $post)
    {
        $data = [
            'plan_name' => $post['plan_name'],
            'plan_sub_name' => $post['plan_sub_name'],
            'plan_description' => $post['plan_description'],
            'operation_fl' => $post['operation_fl'],
            'plan_capacity' => $post['plan_capacity'],
            'target' => $post['target'],
            'schedule' => $post['schedule'],
            'clothes' => $post['clothes'],
            'wind_speed' => $post['wind_speed'],
            'high_waves' => $post['high_waves'],
            'visibility' => $post['visibility'],
            'cancel_other' => $post['cancel_other'],
            'open_status' => $post['open_status'],
            'upd_date' => Date("Y/m/d"),
            'upd_time' => Date("h:i:s"),
            'upd_user' => $post['upd_user']
        ];

        if(!empty($post['banner_photo'])){
            $data['banner_photo'] = $post['banner_photo'];
        }
        if(!empty($post['body_photo1'])){
            $data['body_photo1'] = $post['body_photo1'];
        }
        if(!empty($post['body_photo2'])){
            $data['body_photo2'] = $post['body_photo2'];
        }
        if(!empty($post['body_photo3'])){
            $data['body_photo3'] = $post['body_photo3'];
        }
        if(!empty($post['ss_plan_fee_weekdays'])){
            $data['ss_plan_fee_weekdays'] = $post['ss_plan_fee_weekdays'];
        }
        if(!empty($post['ss_plan_fee_holiday'])){
            $data['ss_plan_fee_holiday'] = $post['ss_plan_fee_holiday'];
        }
        if(!empty($post['plan_fee_weekdays'])){
            $data['plan_fee_weekdays'] = $post['plan_fee_weekdays'];
        }
        if(!empty($post['plan_fee_holiday'])){
            $data['plan_fee_holiday'] = $post['plan_fee_holiday'];
        }

        try{
            $this->_db->beginTransaction();
            // update処理
            $res = $this->_db->update('mst_plan_charter', $data, "plan_no = '{$plan_no}'");

            $this->_db->commit();

        }catch (Zend_Exception $e) {
            $this->_db->rollback();
            throw new Zend_Exception(__FILE__ . '(' . __LINE__ . '): ' . $e->getMessage());
        }
        return $res;
    }
```

## 4.JQuery发送ajax请求

```js
       var data = $('#search_form').serialize(); //可以获取整张表单的数据，再用ajax传出去
       $.ajax({
            url: '/Fcharter/Planmainte/get-fee/',
            data: {"val":val},
            type: 'POST',
            dataType: 'JSON',
            success: function(data){
                $("#half_fee_wd").html('平日料金：￥'+ data['half_fee_wd']);
                $("#full_fee_wd").html('平日料金：￥'+ data['full_fee_wd']);
                $("#half_fee_hd").html('土日祝料金:￥'+ data['half_fee_hd']);
                $("#full_fee_hd").html('土日祝料金:￥'+ data['full_fee_hd']);
                $("#half_fee_wd_hidden").val(data['half_fee_wd']);
                $("#full_fee_wd_hidden").val(data['full_fee_wd']);
                $("#half_fee_hd_hidden").val(data['half_fee_hd']);
                $("#full_fee_hd_hidden").val(data['full_fee_hd']);
                $("#fee_boat_weekdays").val(data['half_fee_wd']);
                $("#fee_boat_holiday").val(data['half_fee_hd']);
            },

            error: function(jqXHR,textStatus){
                console.log(jqXHR,textStatus);
            }
        })
```

## 5.Jquery常用的一些语法、节点操作

```js
removeClass()
addClass()
$('#' + error_id).css({ "display": "block", "color": "red" })
$("#"+id+"_error").parent().parent().addClass("tr_error");

if($("#img01").next("br").length == 0){
	 $("#img01").after("<br>");					
}

 //获取第2个li节点   
 var $li_2=$("ul li:eq(1)");

 //1.创建节点
  //创建2个li节点,要求这2个li节点包含文本包含属性title

  var $dm1=$("<li title='西瓜44'>西瓜</li>");

  var $dm2=$("<li title='香蕉55'>香蕉</li>");

  //将新创建的第1个li节点追加到ul节点下的最后一个子节点

  $("ul").append($dm1);

  $("ul").append($dm1);
 
  //将新创建的第2个li节点追加到ul节点下的第一个子节点

  $("ul").prepend($dm2);

//2.插入节点
//用2种方式来将创造的第1个节点添加到ul下的最后一个子节点

  //$ulDm.append($dm1);

  //$dm1.appendTo($ulDm);

  

  //用2种方式来讲创造的第2个节点添加到ul下的第一个子节点

  //$ulDm.prepend($dm2);

  //$dm2.prependTo($ulDm);

  

  //用2种方式将创造的li节点追加到第2个已经存在的li节点之后

  //$li_2.after($dm3);

  //$dm3.insertAfter($li_2);

  

  //用2种方式将创造的li节点追加到第2个已经存在的li节点之前

  //$li_2.before($dm3);

  //$dm3.insertBefore($li_2);

//3.自杀:直接找到我们要操作的节点删除即可.

 $("ul li:eq(1)").remove();

 jquery种删除掉的节点还可以继续获得

var $dm=$("ul li:eq(1)").remove();

$dm.appendTo($("ul"));

JQuery中删除节点可以加过滤条件

$("ul li").remove("li[title!=菠萝]"); 

获取第2个li节点后,将其元素中的内容清空

$("ul li:eq(1)").empty();

//4.遍历节点操作
利用JQuery的chldren方法来获取页面上所有的body子元素

children()方法只考虑子元素而不考虑任何后代元素。。

alert($("body").children().length);

var dms=$("body").children();

for(var i=0;i<dms.length;i++){

alert(dms[i].innerHTML);

 

next（）方法获取匹配元素后面紧临的同辈元素

prev（）方法获取匹配元素前面紧临的同辈元素。

siblings（）匹配元素前后所有的同辈元素。

页面加载完毕之后,获取p节点前的同辈元素,输出其文本内容

页面加载完毕之后,获取p节点所有的同辈元素

window.onload=function(){

      //获取p节点之后所有的同辈元素

      var dms=$("p").nextAll();

      //for(var i=0;i<dms.length;i++){

        // alert(dms[i].innerHTML);

      //}

      

      //获取节点P之前所有的兄弟节点

      //dms=$("p").prevAll();

      //for(var i=0;i<dms.length;i++){

        // alert(dms[i].innerHTML);

      //}

      

      //获取p节点所有的兄弟节点

      dms=$("p").siblings();

      for(var i=0;i<dms.length;i++){

         alert(dms[i].innerHTML);

      }
//5.其他
    
 //* toggle模拟了鼠标的连续点击事件

 $("h5.head").toggle(function(){

 alert("1111");

 },function(){

  alert("22222");

},function(){

  alert("3333");

 });

 

 $("h5.head").toggle(function(){

    $(this).next().show();

 },function(){

    $(this).next().hide();

 });
    
 //mouseover、mouserout
 //show()/hide()
    
 //each（）
    $("button").click(function(){
       $("li").each(function(){
       alert($(this).text())；
       });
    });
```

