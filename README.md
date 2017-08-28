# LXRunTimeAll
runtime各类问题的探究，都在这！！！

-------------------------

demo_001
```
//普通写法
Person * p = [[Person alloc]init];

//runtime
NSObject * p = objc_msgSend(objc_getRequiredClass("Person"), @selector(alloc));
objc_msgSend(p, @selector(init));
```
```
//普通写法
[p eat];
//runtime
objc_msgSend(p, @selector(eat));
```




