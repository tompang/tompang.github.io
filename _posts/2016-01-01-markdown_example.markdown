---
layout: post
title:  "Markdown练习"
date:   2016-01-01 00:00:42 +0800
categories: jekyll update
---
---

To add new posts, simply add a file in the `行内代码` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

>Jekyll also offers powerful support for code snippets:  
>d引用
>>引用

##块级代码
**Swift Code Example:**

{% highlight swift %}
class func attributedStringWithDoctorLevelString(levelString: String?, fontSize: CGFloat) -> NSAttributedString?{
       guard let string = levelString else {
           return nil
       }

       let mAttrStr = NSMutableAttributedString(string: string)
       let pattern = ZTIcons.icons().doctorLevelRegexString
       let regex = try! NSRegularExpression(pattern: pattern, options: NSRegularExpressionOptions())

       let imageBundleURL = NSBundle.mainBundle().URLForResource("Emoticons", withExtension: "bundle")!
       let imageBundle = NSBundle(URL: imageBundleURL)!

       repeat{
           let aMatches = regex.firstMatchInString(mAttrStr.string, options: NSMatchingOptions(), range: NSMakeRange(0, mAttrStr.length))

           guard let matches = aMatches else {
               break
           }
           if(matches.range.location != NSNotFound){

               let range = matches.range
               var subString = (mAttrStr.string as NSString).substringWithRange(range)
               subString = subString.stringByTrimmingCharactersInSet(NSCharacterSet(charactersInString: "[]"))

               let attachment = NSTextAttachment()
               //                attachment.image = UIImage(named: subString as String)
               attachment.image = UIImage(contentsOfFile: imageBundle.pathForResource(subString, ofType: "png") ?? "")
               attachment.bounds = CGRectMake(0, 0, fontSize, fontSize)

               let imageAttrString = NSAttributedString(attachment: attachment)

               mAttrStr.replaceCharactersInRange(range, withAttributedString: imageAttrString)
           }

       } while (true)
       return mAttrStr
   }
 {% endhighlight %}

<http://www.wikipedia.org>

[(链接到百度)](http://www.baidu.com/ "Baidu")  
![(图片)](https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/bd_logo1_31bdc765.png/ "Baidu")  
[好123][1]  

[Baidu]Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[Baidu]: http://www.baidu.com
[1]: https://www.hao123.com
[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
