---
layout: post
title:  "Markdown Demo"
date:   2016-02-03 21:00:42 +0800
categories: markdown update
---
---

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

>Jekyll also offers powerful support for code snippets:  


**Objectiv-C Code Example:**


``` objc
#import "AppDelegate.h"

#import "ViewController.h"

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
  self.window.backgroundColor = [UIColor whiteColor];
  self.window.rootViewController = [[ViewController alloc] init];
  [self.window makeKeyAndVisible];
  return YES;
}

@end
```

[(链接到百度)](http://www.baidu.com/ "Baidu")  
![(图片)](https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/bd_logo1_31bdc765.png/ "Baidu")  
[好123][1]  
[Baidu]

[Baidu]: http://www.baidu.com
[1]: https://www.hao123.com
