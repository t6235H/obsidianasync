#基础 #c语言 


scanf("%d %d", &a, &b);

- 使用scanf函数时，可能会出现下面的提示：
	
	'scanf': This function or variable may be unsafe. Consider using scanf_s 
	instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help 
	for details.
	
- 要在第一行添加：`#define _CRT_SECURE_NO_WARNINGS`  这样可以取消提示
- 最好不要用scanf_s函数：因为这个函数VS编译器自己提供的，只能在VS上用。由于不是标准C提供的函数，所以在其它编译器上会出现错误

