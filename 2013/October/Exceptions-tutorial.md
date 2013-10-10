### 异常指南（PHP版）

#### 简介

异常是把代码中的错误或异常事件传递给调用方代码的一种特殊手段。如果在一个子程序中遇到了预料之外的情况，但不知道该如何处理的话，它就可以抛出一个异常。对出错的前因后果不甚了解的代码，可以把对控制权转交给系统中其它能更好地解释错误并采取措施的部分。

还可以用异常来清理一段代码中存在的杂乱逻辑。异常的基本结构是：子程序使用throw抛出一个异常对象，再被调用链上层其他子程序的try-catch语句捕获。

维基百科：[更多]（http://en.wikipedia.org/wiki/Exception_handling）

#### Exception类

```bash
Exception {
	/* Properties */
	protected string $message ;	// The exception message
	protected int $code ;		// The exception code
	protected string $file ;	// The filename where the exception was created
	protected int $line ;		// The line where the exception was created

	/* Methods */
	public __construct ([ string $message = "" [, int $code = 0 [, Exception $previous = NULL ]]] )	// Construct the exception
	final public string getMessage ( void )		// Gets the Exception message
	final public Exception getPrevious ( void )	// Returns previous Exception
	final public mixed getCode ( void )		// Gets the Exception code
	final public string getFile ( void )		// Gets the file in which the exception occurred
	final public int getLine ( void )		// Gets the line in which the exception occurred
	final public array getTrace ( void )		// Gets the stack trace
	final public string getTraceAsString ( void )	// Gets the stack trace as a string
	public string __toString ( void )		// String representation of the exception
	final private void __clone ( void )		// Clone the exception
}
```

#### 使用

##### Source code

```bash
<?php
	class Test
	{
		public function __construct()
		{
			throw new Exception('oops!');
		}
	}

	try {
		$obj = new Test();
	} catch(Exception $e) {
		var_dump($e->getMessage();
		echo "\n";
		var_dump($e->getCode());
		echo "\n";
		var_dump($e->getFile());
		echo "\n";
		var_dump($e->getLine());
		echo "\n";
		var_dump($e->getTrace());
		echo "\n";
		var_dump($e->getTraceAsString());
		echo "\n";
		var_dump($e->__toString());
	}
```

##### Return data

```bash
string 'oops!' (length=5)

int 23

string 'D:\xampp\www\test\index.php' (length=27)

int 6

array
  0 => 
    array
      'file' => string 'D:\xampp\www\test\index.php' (length=27)
      'line' => int 11
      'function' => string '__construct' (length=11)
      'class' => string 'Test' (length=4)
      'type' => string '->' (length=2)
      'args' => 
        array
          empty

string '#0 D:\xampp\www\test\index.php(11): Test->__construct()
#1 {main}' (length=65)
string 'exception 'Exception' with message 'oops!' in D:\xampp\www\test\index.php:6
Stack trace:
#0 D:\xampp\www\test\index.php(11): Test->__construct()
#1 {main}' (length=154)
```

（完）