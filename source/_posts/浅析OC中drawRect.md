---
layout : post
title: 浅析OC中drawRect
date: 2014-08-21 16:33:51
tags: Blog
comments: true
---

#### 废话不多说，直接上代码
- 在View中重写- （void）drawRect：（CGRect）rect这个方法

```
@interface DrawView(){
	    UIImage *image1;
	    UIImage* image2;
	}
	@end
－ (instancetype)initWithFrame:(CGRect)frame
	{
	    self = [super initWithFrame:frame];
	    if (self) {
	         image1 = [UIImage imageNamed:@"image.png"];
	         image2 = [UIImage imageNamed:@"BGG.png"];
	    }
	    return self;
	}
－ (void)drawRect:(CGRect)rect
	{
	//    [self drawText];
	//    [self drawLine];
	//    [self draeRectangle:CGRectMake(10, 100, 50, 50)];
	//    [self drawImage];
	//    [self animationImage];
	}
```

####  1.先从简单的画线开始

```
－(void)drawLine
	{
	CGContextRef context    =UIGraphicsGetCurrentContext();//获取画布
	CGContextSetStrokeColorWithColor(context, [UIColor redColor].CGColor);//线条颜色
	CGContextSetShouldAntialias(context,NO);//设置线条平滑，不需要两边像素宽
	CGContextSetLineWidth(context,1.0f);//设置线条宽度
	CGContextMoveToPoint(context,153,10); //线条起始点
	CGContextAddLineToPoint(context,253,10);//线条结束点
	CGContextStrokePath(context);//结束，也就是开始画
	}
	
```

####  2.绘制文本

```
－(void)drawText
	{
	    UIColor *color =[UIColor colorWithRed:0.5f
	                                           green:0.0f
	                                            blue:0.5f
	                                           alpha:1.0f];
	    [color set];
	    UIFont *withFont:helvetica = [UIFont fontWithName:@"HelveticaNeue-Bold"size:30.0f];
	    NSString *string =@"李先森";
	    [string drawAtPoint:CGPointMake(25,190)withFont:helvetica];
	}
```

#### 3.绘制矩形：这里有分为两种风格{1.无框矩形2.有框矩形}

```
－ (void)draeRectangle:(CGRect)rect{
    //首先，获取上下文
    CGContextRef context = UIGraphicsGetCurrentContext();
    /*
     //画无框矩形
    //设置矩形填充颜色：红色
    CGContextSetRGBFillColor(context, 1.0, 0.0, 0.0, 1.0);
    //填充矩形
    CGContextFillRect(context, rect);
    //执行绘画
    CGContextStrokePath(context);
    */
    
    //画有框矩形
    //设置矩形填充颜色：红色
    CGContextSetRGBFillColor(context, 1.0, 0.0, 0.0, 1.0);
    //填充矩形
    CGContextFillRect(context, rect);
    //设置画笔颜色：黑色
    CGContextSetStrokeColorWithColor(context, BLUECOLOR.CGColor);
    //设置画笔线条粗细
    CGContextSetLineWidth(context, 3.0);
    //画矩形边框
    CGContextAddRect(context,rect);
    //执行绘画  
    CGContextStrokePath(context);
	}
```

#### 4.绘制图片

```
－(void)drawImage{
	    CGContextRef context    =UIGraphicsGetCurrentContext();//获取画布
	    
	//    CGContextDrawImage(context,CGRectMake(160,0,160, 150), [image1 CGImage]);
	//上面这种方式，绘制出来的图片是翻转的，开始不知道。因为测试的图片都比较对称。后发发现是上下颠倒了
	
	    //下面才是正确的方法。
	    UIGraphicsPushContext( context );
	//    [image1 drawInRect:CGRectMake(160, 0, 160, 150)];
	    [image1 drawInRect:CGRectMake(160, 0, 160, 150) blendMode:kCGBlendModeColor alpha:1];
	
	    UIGraphicsPopContext();
	}
```
#### 5.给图片添加动画

```
－ (void)animationImage{
	    UIImageView *imageView = [[UIImageView alloc]initWithImage:image2];
	    imageView.frame =CGRectMake(100,100,100,100);
	    imageView.layer.cornerRadius = 50;
	    imageView.layer.masksToBounds = YES;
	    [self addSubview:imageView];
	    imageView.userInteractionEnabled =YES;
	    UITapGestureRecognizer* singleTap =
	    [[UITapGestureRecognizer  alloc]initWithTarget:self action:@selector(onImageClick)];
	    [imageView addGestureRecognizer:singleTap];
	    
	    //animation
	    CABasicAnimation *animation = [CABasicAnimation animationWithKeyPath:@"transform"];
	    animation.delegate =self;
	    animation.toValue = [NSValue valueWithCATransform3D:CATransform3DMakeRotation(M_PI ,0, 0,1.0)];
	    animation.duration =1;
	    animation.cumulative =YES;
	    animation.repeatCount =INT_MAX;
	    
	    [imageView.layer addAnimation:animation forKey:@"animation"];
	}
```

```
	- (void)onImageClick
	{
	// someing code   
	}
```


