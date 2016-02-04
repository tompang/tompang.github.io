---
layout: post
title:  "WKWebView not load file"
date:   2016-02-04 14:50:42 +0800
categories: WKWebView update
---
---

They finally solved the bug! Now we can use `loadFileURL:allowingReadAccessToURL:`. Apparently the fix was worth some seconds in [WWDC video 504 Introducing Safari View Controller](https://developer.apple.com/videos/wwdc/2015/?id=504)

Swift:

{% highlight swift  %}
func fileURLForBuggyWKWebView8(fileURL: NSURL) throws -> NSURL {
    // Some safety checks
    var error:NSError? = nil;
    if (!fileURL.fileURL || !fileURL.checkResourceIsReachableAndReturnError(&error)) {
        throw error ?? NSError(
            domain: "BuggyWKWebViewDomain",
            code: 1001,
            userInfo: [NSLocalizedDescriptionKey: NSLocalizedString("URL must be a file URL.", comment:"")])
    }

    // Create "/temp/www" directory
    let fm = NSFileManager.defaultManager()
    let tmpDirURL = NSURL.fileURLWithPath(NSTemporaryDirectory()).URLByAppendingPathComponent("www")
    try! fm.createDirectoryAtURL(tmpDirURL, withIntermediateDirectories: true, attributes: nil)

    // Now copy given file to the temp directory
    let dstURL = tmpDirURL.URLByAppendingPathComponent(fileURL.lastPathComponent!)
    let _ = try? fileMgr.removeItemAtURL(dstURL)
    try! fm.copyItemAtURL(fileURL, toURL: dstURL)

    // Files in "/temp/www" load flawlesly :)
    return dstURL
}

override func viewDidLoad() {
    super.viewDidLoad()
    var filePath = NSBundle.mainBundle().pathForResource("file", ofType: "pdf")

    if #available(iOS 9.0, *) {
        // iOS9. One year later things are OK.
        webView.loadFileURL(fileURL, allowingReadAccessToURL: fileURL)
    } else {
        // iOS8. Things can be workaround-ed
        //   Brave people can do just this
        //   fileURL = try! pathForBuggyWKWebView8(fileURL)
        //   webView.loadRequest(NSURLRequest(URL: fileURL))
        do {
            fileURL = try fileURLForBuggyWKWebView8(fileURL)
            webView.loadRequest(NSURLRequest(URL: fileURL))
        } catch let error as NSError {
            print("Error: " + error.debugDescription)
        }
    }
}

{% endhighlight %}

Objective-C:

{% highlight objc  %}
- (NSURL *)fileURLForBuggyWKWebView8:(NSURL *)fileURL error:(NSError **)error{
    // Some safety checks
    NSError *newError = nil;
    if (!fileURL.fileURL || ![fileURL checkResourceIsReachableAndReturnError:&newError]) {
        NSError *buggyError = [NSError errorWithDomain:@"BuggyWKWebViewDomain" code:1001 userInfo:@{NSLocalizedDescriptionKey: @"URL must be a file URL."}];
        if (error != NULL) {
            *error = newError != nil ? newError : buggyError;
        }
        return nil;
    }
    // Create "/temp/www" directory
    NSFileManager *manager = [NSFileManager defaultManager];
    NSURL *tmpDirURL = [[NSURL fileURLWithPath:NSTemporaryDirectory()] URLByAppendingPathComponent:@"www"];

    if(![manager createDirectoryAtURL:tmpDirURL withIntermediateDirectories:YES attributes:nil error:&newError]) {
        if (error != NULL) {
            *error = newError;
        }
        return nil;
    }
    // Now copy given file to the temp directory
    NSURL *dstURL = [tmpDirURL URLByAppendingPathComponent:fileURL.lastPathComponent];
    if (![manager copyItemAtURL:fileURL toURL:dstURL error:&newError]) {
        if (error != NULL) {
            *error = newError;
        }
        return nil;
    }
    // Files in "/temp/www" load flawlesly :)
    return dstURL;
}

- (void)viewDidLoad{
    [super viewDidLoad];
    if ([self.webView respondsToSelector:@selector(loadFileURL:allowingReadAccessToURL:)]) {
        // iOS9. One year later things are OK.
        [webView loadFileURL:URL allowingReadAccessToURL:URL];
    }else{
        // iOS8. Things can be workaround-ed
        //   Brave people can do just this
        NSError *error = nil;
        NSURL *fileURL = [self fileURLForBuggyWKWebView8:URL error:&error];
        if (error != nil) {
            NSLog(@"%@",error.localizedDescription);
        }else{
            NSURLRequest *request = [[NSURLRequest alloc] initWithURL:fileURL];
            [webView loadRequest: request];
        }
    }
}
{% endhighlight %}


**See also:**  
StackOverflow: [WKWebView not loading local files under iOS 8][1]

[1]:  http://stackoverflow.com/questions/24882834/wkwebview-not-loading-local-files-under-ios-8
