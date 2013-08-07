### 重构数组 ###

#### Source code ####

	<?php
	/**
	 * 需求描述：
	 * 将数组以「name」键进行重组，即：将「name」键值相同的元素合并，「country」键值以","连接。
	 * 注意「id」键值是无序的，和「name」键值一一对应。
	 *
	 **/
	$mouse = array(
				array('id'=>1, 'name'=>'Bruce', 'country'=>'America'),
				array('id'=>2, 'name'=>'Jack', 'country'=>'England'),
				array('id'=>1, 'name'=>'Bruce', 'country'=>'England'),
				array('id'=>3, 'name'=>'Jay', 'country'=>'China'),
				array('id'=>2, 'name'=>'Jack', 'country'=>'China'),
				array('id'=>1, 'name'=>'Bruce', 'country'=>'China'));

	// Step 1: 生成id为元素的数组
	$idArr = array();
	for ($i=0, $j=count($mouse); $i<$j; $i++) {
		$idArr[] = $mouse[$i]['id'];
	}

	// Step 2: 获取重复的id
	$idArrUnique = array_unique($idArr);
	$idArrDouble = array_diff_assoc($idArr, $idArrUnique);

	// Step 3: 将重复id对应元素的「country」键值以","连接，并且，删除重复元素
	$result = $mouse;
	$idArrDoubleKeys = array_keys($idArrDouble);
	for ($i=0, $j=count($idArrDoubleKeys); $i<$j; $i++) {
		for ($m=0, $n=count($result); $m<$n; $m++) {
			if ($result[$m]['id'] == $idArrDouble[$idArrDoubleKeys[$i]]) {
				$result[$m]['country'] .= ','.$result[$idArrDoubleKeys[$i]]['country'];
				// 删除元素后，跳出当前循环
				unset($result[$idArrDoubleKeys[$i]]);
				break;
			}
		}
	}
	$result = array_values($result);

	var_dump($result);

#### Return ####

	array
	  0 => 
	    array
	      'id' => int 1
	      'name' => string 'Bruce' (length=5)
	      'country' => string 'America,England,China' (length=21)
	  1 => 
	    array
	      'id' => int 2
	      'name' => string 'Jack' (length=4)
	      'country' => string 'England,China' (length=13)
	  2 => 
	    array
	      'id' => int 3
	      'name' => string 'Jay' (length=3)
	      'country' => string 'China' (length=5)

（完）