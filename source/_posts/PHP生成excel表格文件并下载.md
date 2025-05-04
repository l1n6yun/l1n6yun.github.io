title: PHP生成excel表格文件并下载
tags: []
date: 2017-10-26 11:46:34
categories:
---
利用php导出excel我们大多会直接生成.xls文件，这种方便快捷。

```php
function createtable($list,$filename){
    header("Content-type:application/vnd.ms-excel");
    header("Content-Disposition:filename=".$filename.".xls");

    $strexport="编号\t姓名\t性别\t年龄\r";
    foreach ($list as $row){
        $strexport.=$row['id']."\t";
        $strexport.=$row['username']."\t";
        $strexport.=$row['sex']."\t";
        $strexport.=$row['age']."\r";
    }
	
    $strexport=iconv('UTF-8',"GB2312//IGNORE",$strexport);
    exit($strexport);
}
```

基于这个我们可以将方法封装一下:

```php
/**
 * 创建(导出)Excel数据表格
 * @param  array   $list 要导出的数组格式的数据
 * @param  string  $filename 导出的Excel表格数据表的文件名
 * @param  array   $header Excel表格的表头
 * @param  array   $index $list数组中与Excel表格表头$header中每个项目对应的字段的名字(key值)
 * 比如: $header = array('编号','姓名','性别','年龄');
 *       $index = array('id','username','sex','age');
 *       $list = array(array('id'=>1,'username'=>'YQJ','sex'=>'男','age'=>24));
 * @return [array] [数组]
*/
protected function createtable($list,$filename,$header=array(),$index = array()){
	header("Content-type:application/vnd.ms-excel");
	header("Content-Disposition:filename=".$filename.".xls");
	$teble_header = implode("\t",$header);
	$strexport = $teble_header."\r";
	foreach ($list as $row){
		foreach($index as $val){
			$strexport.=$row[$val]."\t";
		}
		$strexport.="\r";
	}
	$strexport=iconv('UTF-8',"GB2312//IGNORE",$strexport);
	exit($strexport);
}
```

方法调用:

```php
$filename = '提现记录'.date('YmdHis');  
$header = array('会员','编号','联系电话','开户名','开户行','申请金额','手续费','实际金额','申请时间');  
$index = array('username','vipnum','mobile','checkname','bank','money','handling_charge','real_money','applytime');  
$this->createtable($cash,$filename,$header,$index);  
```

这种方式生成Excel文件,生成速度很快,但是有缺点是:

1. 单纯的生成Excel文件,生成的文件没有样式,单元格属性(填充色,宽度,高度,边框颜色...)不能自定义。
2. 生成的文件虽然可以打开,但是兼容性很差,每次打开,都会报一个警告。
	
	
	
	