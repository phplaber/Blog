### 使用MySQLi（MySQL Improved） ###

#### 简介 ####

> MySQLi扩展是在PHP使用的一个关系型数据库驱动类，它提供了操作MySQL的接口。MySQLi是老版本MySQL驱动类的改良版，提供了多方面的便利。当MySQL服务器的版本为4.1.3或更新（使用新功能）时，建议PHP开发者使用MySQLi。

> 以上关于MySQLi介绍，来自[wikipedia::MySQLi](http://en.wikipedia.org/wiki/MySQLi "MySQLi")

附：MySQLi提供的几个典型便利：

* 提供面向对象的接口
* 支持预处理语句
* 支持多语句
* 支持事务
* 加强调试支持
* 支持嵌入式服务器

#### 实例 ####

##### 需求描述 #####

有一条旅游线路，有线路编号（id），线路名称（route_name），价格（route_price）三个属性。在线路上会存在多个旅游景点，有所属线路编号（route_id），景点编号（spot_id），景点名称（spot_name）三个属性。在大数据的前提下，为了优化取产品的性能，需要将线路相关数据进行整合。

##### 数据库设计 #####

创建数据库routes，包含三张数据表：一张线路基础数据表（routes_basic），一张景点数据表（routes_spot），一张整合表（routes_all）。基础数据表和景点数据表通过线路编号（route_id）进行关联。创建三张表的SQL如下：

1, 表routes_basic

    CREATE TABLE `routes_basic` (
     `id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '线路编号',
     `route_name` varchar(255) NOT NULL COMMENT '线路名称',
     `route_price` float NOT NULL COMMENT '线路价格',
     PRIMARY KEY (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='线路基础数据表'

2, 表routes_spot

    CREATE TABLE `routes_spot` (
     `id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '景点ID',
     `route_id` int(11) NOT NULL COMMENT '线路编号',
     `spot_name` varchar(255) NOT NULL,
     PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8 COMMENT='景点数据表'

3, 表routes_all

    CREATE TABLE `routes_all` (
     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
     `route_id` int(10) unsigned NOT NULL,
     `route_name` varchar(255) NOT NULL,
     `route_price` float NOT NULL,
     `spot_id` int(10) unsigned NOT NULL,
     `spot_name` varchar(255) NOT NULL,
     `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
     PRIMARY KEY (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='线路整合表'


##### 代码实现 #####

1, 数据库配置文件（db.conf.php）

	<?php
	// 数据库配置文件

	define('DB_HOST',    '127.0.0.1');
	define('DB_PORT',    '3306');
	define('DB_USER',    'root');
	define('DB_PASS',    '19880210');
	define('DB_NAME',    'routes');

2, 执行脚本文件（merge_routes.php）

	<?php

	// 整合产品基础数据和景点数据脚本

	require './db.conf.php';

	$mysqli = @new mysqli(DB_HOST, DB_USER, DB_PASS, DB_NAME, DB_PORT);

	if ($mysqli->connect_errno) {
		die('Connect error: ' . $mysqli->connect_error);
	}

	// create a prepared statement
	$sql_insert = 'INSERT INTO routes_all(route_id, route_name, route_price, spot_id, spot_name) VALUES(?, ?, ?, ?, ?)';
	if ($stmt = $mysqli->prepare($sql_insert)) {
		$sql_select = 'SELECT rb.id AS route_id, 
							  rb.route_name, 
							  rb.route_price, 
							  rs.id AS spot_id, 
							  rs.spot_name 
					   FROM routes_basic rb 
					   LEFT JOIN routes_spot rs 
					   ON rb.id = rs.route_id 
					   ORDER BY rb.id';
		$mysqli->query("set names 'gb2312'");
		$result = $mysqli->query($sql_select);
		while ($row = $result->fetch_array(MYSQLI_ASSOC)) {
			// bind parameters for markers
			$stmt->bind_param('isdis', $row['route_id'], $row['route_name'], $row['route_price'], $row['spot_id'], $row['spot_name']);
			// execute query
			$stmt->execute();
			echo 'inserted route id: ' . $row['route_id'] . "\n";
		}
		$stmt->close();
		unset($result);
	} else {
		trigger_error($mysqli->error . "[$sql_insert]");
		exit;
	}
	$mysqli->close();

p.s. 代码在这里：[mysqli_demo](https://github.com/phplaber/mysqli_demo)

3, 运行脚本

在CLI下运行脚本，在脚本路径下执行：``php merge_routes.php`` 命令，如图：

![alt run script](https://raw.github.com/phplaber/phplaber.github.com/master/images/graph_04.png 'run script')

4, 查看结果

``mysql> select * from routes_basic;``

	+----+------------------------------+-------------+
	| id | route_name                   | route_price |
	+----+------------------------------+-------------+
	|  1 | 苏州-周庄2日游               |         397 |
	|  2 | 西塘2日游                    |         306 |
	|  3 | 千岛湖中心湖区-森林氧吧2日游 |         367 |
	+----+------------------------------+-------------+

``mysql> select * from routes_spot;``

	+----+----------+------------+
	| id | route_id | spot_name  |
	+----+----------+------------+
	|  1 |        1 | 狮子林     |
	|  2 |        1 | 寒山寺     |
	|  3 |        1 | 周庄       |
	|  4 |        2 | 送子来凤桥 |
	|  5 |        2 | 烟雨长廊   |
	|  6 |        2 | 石皮弄     |
	|  7 |        2 | 永宁桥     |
	|  8 |        3 | 森林氧吧   |
	|  9 |        3 | 千岛湖     |
	+----+----------+------------+

``mysql> select * from routes_all;``

	+----+----------+------------------------------+-------------+---------+------------+---------------------+
	| id | route_id | route_name                   | route_price | spot_id | spot_name  | update_time         |
	+----+----------+------------------------------+-------------+---------+------------+---------------------+
	|  1 |        1 | 苏州-周庄2日游               |         397 |       1 | 狮子林     | 2013-09-11 13:23:53 |
	|  2 |        1 | 苏州-周庄2日游               |         397 |       2 | 寒山寺     | 2013-09-11 13:23:53 |
	|  3 |        1 | 苏州-周庄2日游               |         397 |       3 | 周庄       | 2013-09-11 13:23:54 |
	|  4 |        2 | 西塘2日游                    |         306 |       4 | 送子来凤桥 | 2013-09-11 13:23:54 |
	|  5 |        2 | 西塘2日游                    |         306 |       5 | 烟雨长廊   | 2013-09-11 13:23:54 |
	|  6 |        2 | 西塘2日游                    |         306 |       6 | 石皮弄     | 2013-09-11 13:23:54 |
	|  7 |        2 | 西塘2日游                    |         306 |       7 | 永宁桥     | 2013-09-11 13:23:54 |
	|  8 |        3 | 千岛湖中心湖区-森林氧吧2日游 |         367 |       8 | 森林氧吧   | 2013-09-11 13:23:54 |
	|  9 |        3 | 千岛湖中心湖区-森林氧吧2日游 |         367 |       9 | 千岛湖     | 2013-09-11 13:23:54 |
	+----+----------+------------------------------+-------------+---------+------------+---------------------+	

#### 更多 ####

[MySQLi documentation on PHP.net](http://www.php.net/manual/en/book.mysqli.php)