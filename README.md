# LXRunTimeAll
runtime各类问题的探究，都在这！！！

-------------------------
简书地址：http://www.jianshu.com/p/bebd89bd7ca8

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

demo_002


demo_003
目的：给系统NSURL这个类的URLWithString 方法添加一个功能，创建URL又能判断是否为空
解决方案：
```
//交换我们的URLWithString和LX_URLWithString方法
//第一步：拿到这两个方法
//class_getClassMethod     获取类方法
//class_getInstanceMethod  获取对象方法
Method URLWithStr = class_getClassMethod(self, @selector(URLWithString:));
Method LXURLWithStr = class_getClassMethod(self, @selector(LX_URLWithString:));
//第二步：交换方法
method_exchangeImplementations(URLWithStr, LXURLWithStr);
```

demo_4
归档解档
```
unsigned int count = 0;
Ivar * ivars = class_copyIvarList([Person class], &count);

//for 搞定
for (int i = 0; i < count; i++) {
//拿个每一个ivar
Ivar ivar = ivars[i];
//ivar对应的名称
const char * name = ivar_getName(ivar);
//转成 OC
NSString * key = [NSString stringWithUTF8String:name];
//解档
id value = [coder decodeObjectForKey:key];

//设置到属性 -- KVC
[self setValue:value forKey:key];

NSLog(@"BBBBB == %@",key);
}

free(ivars);
```
