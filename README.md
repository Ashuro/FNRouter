# FNRouter

这是一个简单高效的路由插件

备注：本插件遵循面向协议编程思想，增强各模块之间的独立性，高效解耦合。推荐以基础数据类型传递参数，但从实际出发具体业务中不使用Model将有可能降低开发效率和项目稳定性。所以建议将工程中所有的Model归类，统一做全局引用。传递参数时可传递模型数据

注意：在业务逻辑中必须重写相关的初始化方法。具体参照代码标注。


## 解决什么问题


1.Native模块相互调用

2.URL调用Native模块


## 适用场景


1.滚动展示图，一些调用Web一些调用内部模块

2.动态的菜单列表

3.Hybrid应用，H5调用Native模块

![](./img/taobao.png)


## 文件介绍


```
FNRouter
	|-FNRouter.h
	|-NSObject+FNRouterVCObject.h/m
	|-UINavigationController+FNRouterPush.h/m
	|-UIViewController+FNRouterInit.h/m
	|-FNURLRouter.h/m
	|-FNRModuleList.plist

```
```
##FNRouter.h
第一部分是相关引用。若工程中有很多跨模块引用的模型，可以在这里添加引用声明
第二部分是控制器别名声明。主要是防止调用期间写错，所以在这里用变量给控制器起别名。建议加前缀和原控制器名区别开。

例：
///模块1
static NSString * FNR_FNNewsDetailViewController = @"FNNewsDetailViewController";


##NSObject+FNRouterVCObject.h/m
通过名称获取控制器实例相关方法


##UINavigationController+FNRouterPush.h/m
push操作相关方法

##UIViewController+FNRouterInit.h/m
UIViewController相关初始化方法，有些方法需要重写，具体看文件注释

##FNURLRouter.h/m
URL调用Native相关逻辑都在这里



##FNRModuleList.plist
这个很重要，这里是别名和真是VC类型的对应表。通过URL调起Native模块必须在这里边做声明。URL中的模块名也是这里声明的别名

```


## 使用方法


1.页面跳转

```

-(void)pushVC:(NSString *)name animation:(BOOL)animation;

-(void)pushVC:(NSString *)name ;

-(void)pushVC:(NSString *)name Param:(id)param animation:(BOOL)animation;

-(void)pushVC:(NSString *)name Param:(id)param;

-(void)pushVC:(NSString *)name Para1:(id)param1 Param:(id)param2 animation:(BOOL)animation;

-(void)pushVC:(NSString *)name Para1:(id)param1 Param:(id)param2;

-(void)pushVC:(NSString *)name url:(NSString*)url animation:(BOOL)animation;

-(void)pushVC:(NSString *)name url:(NSString*)url;

e.p. 
[self.navigationController pushVC:FNR_FNNewsDetailViewController ParamDictionary:@{@"contentId":@10001} animation:YES];

```

2.URL解析跳转

注意：

<1>协议名。先要在FNURLRouter.h中约定好链接的协议名 如：fnr

<2>链接格式。对于调用内部的链接有严格的格式要求 

![](./img/url.jpg)


```
-(void)openUrl:(NSString *)url withNavigationController:(UINavigationController *)navController animation:(BOOL)animation;

-(void)openUrl:(NSString *)url withNavigationController:(UINavigationController *)navController;

-(void)openUrl:(NSString *)url paramDictionary:(NSDictionary *)param withNavigationController:(UINavigationController *)navController animation:(BOOL)animation;

-(void)openUrl:(NSString *)url paramDictionary:(NSDictionary *)param withNavigationController:(UINavigationController *)navController;




e.p.
跳转Native模块
 [[FNURLRouter shared] openUrl:@"fnr://NewsDetail?pno=0&psize=1" withNavigationController:self.navigationController];
 
跳转WebController
<1>.先要注册Web控制器
-(void)registerWebViewController:(NSString*)clsName;
    
<2>.[[FNURLRouter shared] openUrl:@"http://m.baidu.com" withNavigationController:self.navigationController];

```




