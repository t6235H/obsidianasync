#c语言 #基础 #cmd 


```
stdlib.h
system("pause");   调用系统暂停命令  只能在WINDOWS使用


//getchar();       也可以  获取单字节
int c =0;
while ((c = getchar()) != '\n' && c != EOF);//清空缓冲区 
//EOF是-1 只要不是回车或文件结尾，就消耗；读取并丢弃输入缓冲区中剩余的所有字符，直到遇到换行符 `\n` 或文件结束符 EOF 为止

printf("按回车键退出...");
getchar();


要两次ENTER,第一次刷新缓冲区
```