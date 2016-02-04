---
layout: post
title:  "iOSCompileOrRunTimeVersionCheck"
date:   2016-01-22 00:00:42 +0800
categories: iOS Code
---

用于iOS运行时判断系统版本

**Code snippets:**

---
Objectiv-C
{% highlight objc %}

//macros
#define isAtLeast_iOS6 isAtLeast_iOS(6)
#define isAtLeast_iOS7 isAtLeast_iOS(7)
#define isAtLeast_iOS8 isAtLeast_iOS(8)
#define isAtLeast_iOS9 isAtLeast_iOS(9)

#pragma mark - Runtime Version Check Helper

BOOL isAtLeast_iOS(NSInteger majorVersion) {
    return isOperatingSystemAtLeastVersion(majorVersion, 0, 0);
}

/// Check runtime iOS version, [Semantic Versioning](http://semver.org)
BOOL isOperatingSystemAtLeastVersion(NSInteger majorVersion, NSInteger minorVersion, NSInteger patchVersion){
    BOOL isAtLeast = NO;
    if ([[NSProcessInfo processInfo] respondsToSelector:NSSelectorFromString(@"isOperatingSystemAtLeastVersion:")]) {
#if __IPHONE_OS_VERSION_MAX_ALLOWED >= __IPHONE_8_0
        NSOperatingSystemVersion minimumSystemVersion = {majorVersion, minorVersion, patchVersion};
        isAtLeast = [[NSProcessInfo processInfo] isOperatingSystemAtLeastVersion:minimumSystemVersion];
#endif
    }else{
        NSString *minimumVersionString = [NSString stringWithFormat:@"%d.%d.%d",(int)majorVersion, (int)minorVersion, (int)patchVersion];
        isAtLeast = [[UIDevice currentDevice].systemVersion compare:minimumVersionString options:NSNumericSearch] != NSOrderedAscending;
    }
    return isAtLeast;
}
{% endhighlight %}

**See also:**  
[语义化版本](http://semver.org/ "Semantic Versioning")
