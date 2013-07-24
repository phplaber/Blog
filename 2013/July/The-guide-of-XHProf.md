### XHProf入门 ###

「XHProf」是Facebook开发的一款PHP性能分析开源软件，提供阻塞时间，CPU时间和内存使用等指标。相比于普遍使用的Xdebug，XHProf具有轻量级的最大优势，在生存环境下也能使用。同时，配合Graphviz能生成清晰直观的性能图像，为性能优化提供科学指导。

下面介绍在Windows下安装配置XHProf，以及通过一个简单的例子，简单介绍下XHProf的使用。

#### 一，XHProf在Windows下的安装和配置 ####

XHProf是作为PHP的扩展工作的，因此首先需要下载扩展文件「php_xhprof.dll」，将其放在PHP扩展目录下，接着，打开PHP配置文件php.ini，加入如下配置：

    extension=php_xhprof.dll

    [xhprof]
    ; Config the dir to store the profiling data
    xhprof_output.dir="D:\xampp\www\xhprof\data"

保存文件后，重启Web服务器。查看phpinfo页面，如果出现「XHProf」节，如图所示，则表明XHProf安装成功。

![phpinfo中XHProf节](https://github.com/phplaber/phplaber.github.com/blob/master/images/graph_00.png "phpinfo中XHProf节")

#### 二，「hello, world」程序 ####

考虑这样一种情况：使用for循环实现「1+2+3+...+100」的和。通过在代码中加入XHProf，考察其性能。代码大致为：

    <?php

    // start profiling
    xhprof_enable(XHPROF_FLAGS_CPU + XHPROF_FLAGS_MEMORY);

    // run program
    $sum = 0;
    for ($i=1; $i<101; $i++) {
		$sum += $i;
    }
	echo '<h3>The result is: '.$sum.'</h3>';

    // stop profiler
    $xhprof_data = xhprof_disable();

    // display raw xhprof data for the profiler run
    //var_dump($xhprof_data);

    $XHPROF_ROOT = realpath(dirname(__FILE__));
    include_once $XHPROF_ROOT . "/xhprof_lib/utils/xhprof_lib.php";
    include_once $XHPROF_ROOT . "/xhprof_lib/utils/xhprof_runs.php";

    // save raw data for this profiler run using default
    // implementation of iXHProfRuns.
    $xhprof_runs = new XHProfRuns_Default();

    // save the run under a namespace "xhprof_foo"
    $run_id = $xhprof_runs->save_run($xhprof_data, "xhprof_foo");

    echo '<a href="http://lab.com/xhprof_html/index.php?run='.$run_id.'&source=xhprof_foo">View Data</a>';

注意正确部署xhprof_lib目录和测试文件的位置。执行该脚本文件，会返回如图结果：

![执行脚本页面](https://github.com/phplaber/phplaber.github.com/blob/master/images/graph_01.png "执行脚本页面")

同时，查看D:\xampp\www\xhprof\data目录会发现，目录下多了一个「51effc17207ce.xhprof_foo」文件。其中「51effc17207ce」为进行XHProf性能分析文件标识ID，「xhprof_foo」为命名空间。为方便查看相关数据，XHProf提供了一个通过Web访问的UI，即：xhprof_html。点击「View Data」链接（注意链接URL形式），就可以查看相关性能数据。如下图所示：

![性能查看之HTML页面](https://github.com/phplaber/phplaber.github.com/blob/master/images/graph_02.png "性能查看之HTML页面")

至此，XHProf的使用流程就算结束了。但是，展示性能数据的HTML页面实在有些不太友好，要是能够图形化就最好了。你可能注意到，HTML页面上有一个[View Full Callgraph]的链接，但是目前是不可用的，因为还缺少图形可视化的软件，接下来安装它。

#### 三，安装Graphviz ####

Graphviz是一款图形可视化软件，可以从[官网](http://www.graphviz.org/ "Graphviz官方网站")下载Windows版本。接着，一路next，直到安装完毕。安装完成后，配置文件让Graphviz对XHProf可用。打开config.php文件，配置「ERROR_FILE」，「TMP_DIRECTORY」和「DOT_BINARY」三个常量，分别表示：错误日志文件，临时目录和dot.exe路径（Graphviz安装目录下）。完成后，点击[View Full Callgraph]链接，就能看到可视化的性能信息，如图：

![性能查看之图形可视化](https://github.com/phplaber/phplaber.github.com/blob/master/images/graph_03.png "性能查看之图形可视化")

以上，作为XHProf的简单入门应该足够了。想获取更多关于XHProf的资料，请自行上网查找。

（完）
