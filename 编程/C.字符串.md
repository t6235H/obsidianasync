#基础 #c语言 


`char x = 'a';`
`char array[4] = "abc";`
`char arr1[]   = "abc";`
`char arr2[]   = {'a','b','c','\0'};
`int  y        = strlen(array);     => true: y == 3;   --string.h`

转义字符：
	\n 换行
	\0 字符串终止
	\r 回车
	\a 蜂鸣
	\b 退格
	\f 进纸
	\t 水平制表 tab键
	\v 垂直制表
	\ddd ddd表示1-3个八进制数字，如\130 -> ascii: X
	\xdd dd表示2个八进制数字，如\x30 -> ascii: 0
	\? 在连续书写多个问号时使用，防止它们被解析成三字母词

三字母词：在早期编译器，??) 是 ]  ??( 是 [

占位符：
	%d 打印整数
	%c  打印字符
	%s 打印字符串
	%f 打印float
	%lf 打印double
	%zu 打印sizeof
	%