---
layout: post
title:  "NSURLSession DownloadFile Demo"
date:   2016-02-03 21:00:42 +0800
categories: jekyll update
---
---

A download example `NSURLSession` with JSON HTTPBody

Example with `NSURLSessionDataTask`:
{% highlight objc %}
NSDictionary *params ＝ <#request params#>;
NSURL *url = <#request url#>;
NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];

[request setHTTPBody:[NSJSONSerialization dataWithJSONObject:params options:NSJSONWritingPrettyPrinted error:nil]];

// optional for demo only
[request setHTTPMethod:@"POST"];
[request setValue:@"v2.0.0" forHTTPHeaderField:@"version"];
[request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

NSURLSession *session = [NSURLSession sharedSession];// singleton

NSURLSessionDataTask *task = [session dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
  NSString *cache = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];
  NSString *dstPath = [cache stringByAppendingPathComponent:@"myFile.pdf"]; //filename.extension
  NSFileManager *manager = [NSFileManager defaultManager];
  if (![manager createFileAtPath:dstPath contents:data attributes:nil]) {
    NSLog(@"can't create file at path %@",path);
  }
}];

[task resume]; // NSURLSessionTask objects are always created in a suspended state and must be sent the -resume message before they will execute.
{% endhighlight %}

Example with `NSURLSessionDownloadTask`:
{% highlight objc %}
NSDictionary *params ＝ <#request params#>;
NSURL *url = <#request url#>;
NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];

[request setHTTPBody:[NSJSONSerialization dataWithJSONObject:params options:NSJSONWritingPrettyPrinted error:nil]];

// optional for demo only
[request setHTTPMethod:@"POST"];
[request setValue:@"v2.0.0" forHTTPHeaderField:@"version"];
[request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

NSURLSession *session = [NSURLSession sharedSession];// singleton

NSURLSessionDownloadTask *task = [session downloadTaskWithRequest:request completionHandler:^(NSURL * _Nullable location, NSURLResponse * _Nullable response, NSError * _Nullable error) {
  //not on main thread
  NSString *doc = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject];
  NSString *dstPath = [doc stringByAppendingPathComponent:@"myFile.pdf"];
  NSURL *dstURL = [NSURL fileURLWithPath:dstPath isDirectory:NO];

  NSFileManager *manager = [NSFileManager defaultManager];
  NSError *newError = nil;

  if ([manager fileExistsAtPath:dstPath]) {
    if (![manager removeItemAtPath:dstPath error:&newError]) {
      NSLog(@"%@",newError.userInfo);
    }
  }
  if (![manager copyItemAtURL:location toURL:dstURL error:&newError]) {
    NSLog(@"%@",newError.userInfo);
  }
}];
[task resume];
{% endhighlight %}

#See also:

Reference: [NSURLSession]  
Guide: [URL Session Programming Guide]  

[URL Session Programming Guide]:  https://developer.apple.com/library/etc/redirect/xcode/ios/1151/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html
[NSURLSession]: https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html
