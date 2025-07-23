#基础 #c语言 


const int p，            p不能修改。 
const int * p，       * p不能修改，p可修改。
const int ** p，    ** p不能修改，* p可修改，p可修改， 
const int *** p，*** p不能修改，** p能修改，* p可修改，p可修改， 
由上可见 const往右指向的最终地址不能修改，const int *** p的const往右最终指向*** p，所以*** p不能修改。 


int *const ** p，***           p可修改，** p不能修改，* p可修改，p可修改，这里的const指向** p，所以** p不能修改。
int *const *const * p，*** p可修改，** p不能修改，* p不能修改，p可修改，这里的第一个const指向** p，第二个const指向* p，所以** p和* p不能修改。 
int *const ** const p，***  p可修改，** p不能修改，* p可修改，p不能修改，这里的第一个const指向** p，第二个const指向p，所以** p和p不能修改。 
const int ** const * p，*** p不能修改，** p可修改，* p不能修改，p可修改，这里的第一个const指向*** p，第二个const指向* p，所以*** p和* p不能修改。