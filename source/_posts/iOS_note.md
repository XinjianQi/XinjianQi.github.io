---
title: iOS note
date: 2017-07-29 10:36:24
tags:
---

### UITabBarItem

> 在使用UITabBar时，默认的UITabBarItem显示title和icon的距离有些大，title会太靠近屏幕底部边缘；我们可以通过修改UITabBarItem的属性，来调整title和icon的间距，当然也可以改变title和icon的上下位置。 

```
for (UITabBarItem *item in vc.tabBar.items) {
  item.titlePositionAdjustment = UIOffsetMake(0, -2);
  item.imageInsets = UIEdgeInsetsMake(-2, 0, 2, 0);
}
```

### UINavigationBar

> 系统的UINavigationBar底部会有一条横线，颜色较深，很多时候与我们的设计不搭，我们可以通过几种方法来解决。

* 设置图片

> 一种是让美工给 @2x 和 @3x 的图片，具体效果让设计出；
一种是通过纯色图片；
但前提是必须设置 UINvigationBar 的 backgroundImage 后，setShadowImage才会起作用。
代码如下：

```
[[UINavigationBar appearance] setBackgroundImage:[UIImage imageWithColor:[UIColor whiteColor]] forBarMetrics:UIBarMetricsDefault];
```

```
// 设计图
[[UINavigationBar appearance] setShadowImage:[UIImage imageNamed:@"line_img"]]; 
```
or
```
// 纯色
[[UINavigationBar appearance] setShadowImage:[UIImage imageWithColor:[UIColor redColor]]]; 
```

> ps: 上面提到了 imageWithColor 方法，可以通过 UIImage 的 Category 方法添加，代码如下：

```
/** 根据颜色生成纯色图片 **/
+ (UIImage *)imageWithColor:(UIColor *)color
{
    CGRect rect = CGRectMake(0.0f, 0.0f, 1.0f, 1.0f);
    UIGraphicsBeginImageContext(rect.size);
    CGContextRef context = UIGraphicsGetCurrentContext();
    
    CGContextSetFillColorWithColor(context, [color CGColor]);
    CGContextFillRect(context, rect);
    
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    
    return image;
}
```

* 修改透明度

> 首先要获取到 UINavigationBar 的底部横线；

```
#pragma mark - 返回 UINavigationBar 下方的黑色横线
- (UIImageView *)findBottomlineImageView:(UIView *)view
{
    if ([view isKindOfClass:UIImageView.class] && view.bounds.size.height <= 1.0) {
        return (UIImageView *)view;
    }
    for (UIView *subview in view.subviews) {
        UIImageView *imageView = [self findHairlineImageViewUnder:subview];
        if (imageView) {
            return imageView;
        }
    }
    return nil;
}
```

> 然后设置 alpha 值。

```
UIImageView *lineView = [self findBottomlineImageView:self.navigationBar];
lineView.alpha = 0.3;
```

> 可以发现，通过以上几种方式都可以隐藏 UINavigationBar 下方的横线。

```
[[UINavigationBar appearance] setShadowImage:[UIImage imageWithColor:[UIColor clearColor]]];
```
```
[[UINavigationBar appearance] setShadowImage:[UIImage new]];
```
```
UIImageView *lineView = [self findBottomlineImageView:self.navigationBar];
lineView.alpha = 0;
// lineView.hidden = YES;
```

### UITabBar 
> 更改 UITabBar 上方的横线可以采用与 UINavigationBar 同样的方式解决；
如果采用 setShadowImage 方法，必须先 setBackgroundImage，二者缺一不可。

```
[self.tabBar setShadowImage:img];
[self.tabBar setBackgroundImage:[UIImage new]];
```



