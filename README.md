# RZColorful

[![CI Status](https://img.shields.io/travis/rztime/RZColorful.svg?style=flat)](https://travis-ci.org/rztime/RZColorful)
[![Version](https://img.shields.io/cocoapods/v/RZColorful.svg?style=flat)](https://cocoapods.org/pods/RZColorful)
[![License](https://img.shields.io/cocoapods/l/RZColorful.svg?style=flat)](https://cocoapods.org/pods/RZColorful)
[![Platform](https://img.shields.io/cocoapods/p/RZColorful.svg?style=flat)](https://cocoapods.org/pods/RZColorful)

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Requirements

## Installation

RZColorful is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'RZColorful'
```

## Author

rztime, rztime@vip.qq.com QQ群：580839749

## License

RZColorful is available under the MIT license. See the LICENSE file for more info.


# RZColorful
NSAttributedString 富文本方法 (图文混排、多样式文本)


## 更新日志
[更新日志](https://github.com/rztime/RZColorful/blob/master/UpdateLog.md)

swift版本[RZColorfulSwift](https://github.com/rztime/RZColorfulSwift)

UITextView实现的富文本编辑器[RZRichTextView](https://github.com/rztime/RZRichTextView)

* NSAttributedString 的多样化设置(文字字体、颜色、阴影、段落样式、url、下划线，以及图文混排等等)
* 添加UITextField、UITextView、UILabel的attributedText的富文本设置。
* 扩展：添加一个刷新界面时保持文本框焦点的方法 [demo查看](https://github.com/rztime/ContinueFirsterResponder)
* 富文本方法内容可单独抽出来,在下边这个文件夹中
```
#import "NSAttributedString+RZColorful.h"
```

## 关于RZColorful
* 支持UILabel、UITextView、UITextField的attributedText的设置。
* 支持获取NSAttributedString中的图片
* 支持 HTML 与 NSAttributedString互换（支持图片）
* 包含的属性快捷设置：
    * 段落样式
    * 阴影
    * 文本字体、颜色
    * 文本所在区域对应的背景颜色
    * 连体字
    * 字间距
    * 删除线、下划线，及其线条颜色
    * 描边，及其颜色
    * 斜体字
    * 拉伸
    * 上下标
    * 书写方向（从左到右，从右到左）
    * 通过html源码加载富文本
    * 通过url添加图片到富文本

## How to use
* 请在需要使用的地方加上

```objc
#import "RZColorful.h"
```

* 主要的功能：
    * RZColorfulConferrer 
        * text                              -- 添加文本
        * htmlText                          -- 添加html源码
        * image                             -- 添加图片 
        * imageByUrl                        -- 添加图片（通过图片的URL添加）
        * paragraphStyle                    -- 全局的段落样式
        * shadow                            -- 全局的阴影样式
        
    * RZColorfulAttribute           -- 设置文本的所有的属性
    * RZImageAttachment         -- 设置图片的所有的属性

### 图示说明

<p align="center" >
    <img src="line.png" title="支持的功能结构">
</p>

### 讲解

*  `RZColorfulConferrer` 中 `paragraphStyle` 和 `shadow` 属于全局的设置，在block中任何位置都可设置
* 文字和图片都可设置`paragraphStyle` 和 `shadow`，对齐方式是`paragraphStyle` 的`alignment`，设置后，当前行文字图片的全局样式将被此覆盖，即全局样式无效，不影响其他行。

    
### 基本的简单使用方法
```objc
    [cell.textLabel rz_colorfulConfer:^(RZColorfulConferrer * _Nonnull confer) {
        confer.paragraphStyle.paragraphSpacing(15); // 设置段落之间的行距

        confer.text(@"日常用处:(图片+标题+描述)\n").font(rzFont(17)).textColor([UIColor redColor]);

        confer.appendImage([UIImage imageNamed:@"test.jpg"]).bounds(CGRectMake(0, -2, 15, 15)); // 图片
        confer.text(@" 姓        名: ").font(rzFont(15)).textColor(RGBA(151, 151, 151, 11));
        confer.text(@"rztime").font(rzFont(15)).textColor(RGBA(51, 51, 51, 1));

        confer.text(@"\n");
        confer.appendImage([UIImage imageNamed:@"test.jpg"]).bounds(CGRectMake(0, -2, 15, 15));
        confer.text(@" 时        间: ").font(rzFont(15)).textColor(RGBA(151, 151, 151, 11));
        confer.text([NSString stringWithFormat:@"%@", [NSDate new]]).font(rzFont(15)).textColor(RGBA(51, 51, 51, 1));

        confer.text(@"\n");
        confer.appendImage([UIImage imageNamed:@"test.jpg"]).bounds(CGRectMake(0, -2, 15, 15));
        confer.text(@" 当次消费: ").font(rzFont(15)).textColor(RGBA(151, 151, 151, 11));
        confer.text(@"￥").font(rzFont(15)).textColor(RGBA(51, 51, 51, 1));
        confer.text(@"100").font(rzFont(15)).textColor(RGBA(251, 51, 51, 1));
        confer.text(@"元").font(rzFont(15)).textColor(RGBA(51, 51, 51, 1));
    }];
```
效果如下
<p align="center" >
    <img src="image_1.jpg" title="日常列表中常用展示方法">
</p>

### 段落样式、阴影（局部与全局统一的区别）
如果设置有局部样式，则全局样式无效
```objc
    [cell.textLabel rz_colorfulConfer:^(RZColorfulConferrer * _Nonnull confer) {
        confer.paragraphStyle.lineSpacing(5).paragraphSpacingBefore(5).alignment(NSTextAlignmentCenter); // 段落全局样式
        confer.shadow.color(RGBA(255, 0, 0, 0.3)).offset(CGSizeMake(1, 1));   // 阴影全局
        
        // 此部分显示全局样式的风格 （红色阴影，居中对齐，段落行距等）
        confer.appendImage([UIImage imageNamed:@"test.jpg"]).bounds(CGRectMake(0, -2, 15, 15));
        confer.text(@" 姓名: ").font(rzFont(15)).textColor(RGBA(151, 151, 151, 11));
        confer.text(@"rztime").font(rzFont(15)).textColor(RGBA(51, 51, 51, 1));
        
        confer.text(@"\n");
        confer.appendImage([UIImage imageNamed:@"test.jpg"]).bounds(CGRectMake(0, -2, 15, 15));
        confer.text(@" 时间: ").font(rzFont(15)).textColor(RGBA(151, 151, 151, 11));
        confer.text([NSString stringWithFormat:@"%@", [NSDate new]]).font(rzFont(15)).textColor(RGBA(51, 51, 51, 1));
        
        // 此部分显示全局样式的风格 （居中对齐，段落行距等）  阴影将被局部覆盖（灰色）
        confer.text(@"\n地址: ").font(rzFont(15)).textColor(RGBA(151, 151, 151, 11)).paragraphStyle.paragraphSpacingBefore(20).and.shadow.color(GRAY(151)).offset(CGSizeMake(3, 3));;
        confer.text(@"成都-软件园").font(rzFont(15)).textColor(RGBA(51, 51, 51, 1)).shadow.color(GRAY(151)).offset(CGSizeMake(3, 3));
        
        // 此部分段落样式被局部覆盖  阴影显示全局的
        confer.text(@"\n爱好: ").font(rzFont(15)).textColor(RGBA(151, 151, 151, 11)).paragraphStyle.paragraphSpacingBefore(20);
        confer.text(@"游山、").font(rzFont(15)).textColor(RGBA(151, 51, 51, 1));
        confer.text(@"玩水、").font(rzFont(10)).textColor(RGBA(51, 151, 51, 1));
        confer.text(@"听歌、").font(rzFont(18)).textColor(RGBA(51, 51, 151, 1));
        confer.text(@"美食、").font(rzFont(17)).textColor(RGBA(51, 151, 51, 1));
        confer.text(@"看电影、").font(rzFont(16)).textColor(RGBA(151, 51, 51, 1));
        confer.text(@"撸代码、").font(rzFont(15)).textColor(RGBA(51, 151, 51, 1));
        confer.text(@"等等\n\n").font(rzFont(15)).textColor(RGBA(251, 51, 51, 1));
    }];
```
效果如下
<p align="center" >
    <img src="image_2.png" title="日常列表中常用展示方法">
</p>

通过url加载图片
```objc
    [cell.textLabel rz_colorfulConfer:^(RZColorfulConferrer * _Nonnull confer) {
        confer.appendImageByUrl(@"http://pic28.photophoto.cn/20130830/0005018667531249_b.jpg").bounds(CGRectMake(0, 0, 200, 0)).paragraphStyle.alignment(NSTextAlignmentLeft); // 宽或高为0时，即自动宽/高按照图片比例来
    }];
```
通过html源码加载文本
```objc
[cell.textLabel rz_colorfulConfer:^(RZColorfulConferrer * _Nonnull confer) {
    NSString *resourcePath = [[NSBundle mainBundle] resourcePath];
    NSString *filePath =[resourcePath stringByAppendingPathComponent:@"test.html"];
    NSString *htmlstring=[[NSString alloc] initWithContentsOfFile:filePath encoding:NSUTF8StringEncoding error:nil];
    confer.htmlText(htmlstring);
}];
```

给UILabel添加富文本可点击功能  给文本添加tapActionByLable属性
```objc 
[_label rz_colorfulConfer:^(RZColorfulConferrer * _Nonnull confer) {
    confer.text(@"可点击的：").font([UIFont systemFontOfSize:17]).textColor(UIColor.blackColor);
    confer.text(@"文本").font([UIFont systemFontOfSize:17]).textColor(UIColor.redColor).tapActionByLable(@"1");
}];
[_label rz_tapAction:^(UILabel * _Nonnull label, NSString * _Nonnull tapActionId, NSRange range) {
    NSLog(@"%@", tapActionId); // print: 1
}];
```

给UILabel添加超行之后的折叠、展开功能
```objc
UILabel

/// 设置富文本（超过行数后，自动追加“展开” “收起”）
/// @param attr 原文
/// @param line 最大显示行数
/// @param width 最大显示宽度，这个宽度用于计算文本行
/// @param fold 当前是否折叠
/// @param allText 超过了行数之后，折叠状态显示的文本 如”展开“  需要给文本设置NSTapActionByLabel属性  (tapActionByLable)
/// @param foldText 超过行数之后，全部展开状态显示的文本  如”收起“  需要给文本设置NSTapActionByLabel属性 (tapActionByLable)
- (void)rz_setAttributedString:(NSAttributedString * _Nullable)attr maxLine:(NSInteger)line maxWidth:(CGFloat)width isFold:(BOOL)fold showAllText:(NSAttributedString *_Nullable)allText showFoldText:(NSAttributedString *_Nullable)foldText;
```

# 备注：
* 多种属性使用名请参考对应的文件。
* UILabel、UITextFile是同样的使用方法。
* 在UILabel、UITextFiled上url点击方法无效。
* 在UITextView中若要设置文本和图片的点击事件（即对文本和图片设置附带URL的属性），请先设置其editable = NO, 并实现代理。
    * 设置了URL属性的文本，内部自带蓝色和下划线，若要去掉，可设置 `textView.linkTextAttributes = @{};`

```objc
// 实现下列方法 （二选一、或都可以实现，当都返回YES时，可能会打开safari浏览器）
textView.rzDidTapTextView = ^BOOL(id  _Nullable tapObj) {
    NSString *url = tapObj;
    if ([tapObj isKindOfClass:[NSURL class]]) {
        url = [(NSURL *)tapObj absoluteString];
    }
    url = url.rz_decodedString;
            
    NSLog(@"rzDidTapTextView：tapObj:%@  \n如果url中包含了有中文,URL将会进行编码，所以请rz_decodedString解码之后查看:%@", tapObj, url);
    return NO;
};

- (BOOL)textView:(UITextView *)textView shouldInteractWithURL:(NSURL *)URL inRange:(NSRange)characterRange interaction:(UITextItemInteraction)interaction {
    NSLog(@"URL:%@", URL);
    // return NO;则不跳转，这里可以做一些基本判断在执行是否跳转浏览器打开url
    return YES; 
}
```

## 注意

* 尽管我已经在代码中已经处理过（弱）引用问题，但是在实际运用写入text时，还是请尽量检查避免循环引用


## 最后
* 在使用过程中，如果您发现有什么问题，欢迎向我反馈，谢谢
