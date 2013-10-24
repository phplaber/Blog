### 「Conclusion」系列：第一篇，好的编程实践 ###

好的编程实践有哪些？这个问题的答案，如果罗列出来，恐怕一张A4纸是不够的。今天，我不想干这个事，事实上也没有能力干这个事，因为项目经验还是太少了。写这篇文章的动机来之[stackoverflow](http://stackoverflow.com/)上的一个回答，这个回答被赞了7次（截至目前），确实是个有价值的答案。

原帖在这里：[mysqli - fetch_Array error call to a member function fetch_array() on a non-object mysqli](http://stackoverflow.com/questions/14639965/mysqli-fetch-array-error-call-to-a-member-function-fetch-array-on-a-non-obje)

******

**Your Common Sense** 的答案我觉得有三点非常可取的地方：

#### 一，构造数组的方式 ####

**好的实践** （作者的做法）：

	$arrChartData = array();
	...
	while($row = $res->fetch_assoc()) {
	    $arrChartData[] = $row;
	}

**坏的实践** ：

	$arrChartData = array();
	$i = 0;
	...
	while($row = $res->fetch_assoc()) {
	    $arrChartData[$i]['date'] = $row['date'];
	    ...
	    $i++;
	}

这两种构造数组方式，孰优孰劣，非常清楚。

#### 二，便利的Debug机制 ####

作者在回答中提到，在执行query时，都应该使用函数trigger_error，方便开发调试。这样在执行SQL出错时，能快速定位到对应SQL。

	$res = $mysqli->query($sql) or trigger_error($mysqli->error."[$sql]");

#### 三，善用helper function ####

对于一些逻辑相同，只是数据不同的实现，完全可以借助helper function将其封装。这样不但能使代码更加简洁，同时又提高代码的可读性。例如：

	$arrChartData[] = array();
	$sql = "SELECT date, ratepersqft, location FROM ratepersqft WHERE project_id = 1";
	$res = $mysqli->query($sql) or trigger_error($mysqli->error."[$sql]");
	while($row = $res->fetch_assoc()) {
	    $arrChartData[] = $row;
	}

这里构造数组的实现就可以通过helper function进行封装，因为逻辑实现都一样，只是SQL不一样。

编写dbGetAll函数，用来实现构造数组的逻辑：

	function dbGetAll($sql) {
		global $mysqli;
		$arrChartData[] = array();
		$res = $mysqli->query($sql) or trigger_error($mysqli->error."[$sql]");
		while($row = $res->fetch_assoc()) {
		    $arrChartData[] = $row;
		}
		return $arrChartData;
	}

这样，原代码就可以重构为：

	$sql = "SELECT date, ratepersqft, location FROM ratepersqft WHERE project_id = 1";
	$arrChartData = dbGetAll($sql);

这个dbGetAll函数就得到了重用。

（完）
