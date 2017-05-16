---
layout : post
title: App中如何实现快速实现打电话，发短信功能
date: 2014-10-12 16:33:51
tags: Blog
comments: true
---

### 前言
在开发App中，经常会使用到点击拨打用户电话功能，短信功能可能会比较少见，其实在iOS中提供了接口，让我们调用。下面就让我们快速的了解这两个功能。

####  1.打电话功能


- 第一种是用UIWebView加载电话，这种是合法的，可以上App Store的。在实际开发中，本人一直使用此方法。 代码如下：

```
UIWebView*callWebview =[[UIWebView alloc] init];  
	NSURL *telURL =[NSURL URLWithString:@"tel:10010"];  
	[callWebview loadRequest:[NSURLRequest requestWithURL:telURL]];  
	//记得添加到view上  
	[self.view addSubview:callWebview];  
```

- 第二种是私有方法，网上查资料说是不能上App Store的（自己没试过）。

```
	[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"telprompt://10010"]];  
```

####  2.短信功能

- 第一种最简单是使用openURL：

```
	[[UIApplication sharedApplication]openURL:[NSURL URLWithString:@"sms://10010"]];//发短信
```
此方法虽然方便，但是上面无法指定短信内容，iOS4.0新加入了MFMessageComposeViewController和MFMessageComposeViewControllerDelegate，提供了发送短信的接口,可以像发送邮件那样不用跳出程序来发送短信.


- 第二种MFMessageComposeViewController：
MFMessageComposeViewController提供了操作界面使用前必须检查canSendText方法,若返回NO则不应将这个controller展现出来,而应该提示用户不支持发送短信功能. messageComposeDelegate ：代理，处理发送结果 recipients ：收信人<列表，支持群发> body ：短信内容
Frameworks中要引入MessageUI.framework

```
#import <MessageUI/MessageUI.h>
	添加协议：<MFMessageComposeViewControllerDelegate>
	
	#import <MessageUI/MessageUI.h>   
	@interface DemoViewController : UIViewController <MFMessageComposeViewControllerDelegate>  
	  
	@end 
```
同时实现协议MFMessageComposeViewControllerDelegate

```
- (void)showMessageView  
	{  
      
    if( [MFMessageComposeViewController canSendText] ){  
          
        MFMessageComposeViewController * controller = [[MFMessageComposeViewController alloc]init]; //autorelease];  
          
        controller.recipients = [NSArray arrayWithObject:@"10010"];       
        controller.body = @"测试发短信";          
        controller.messageComposeDelegate = self;  
  
        [self presentModalViewController:controller animated:YES];  
          
        [[[[controller viewControllers] lastObject] navigationItem] setTitle:@"测试短信"];//修改短信界面标题  
    }else{  
          
        [self alertWithTitle:@"提示信息" msg:@"设备没有短信功能"];          
	 }      
	}
	
	
	//MFMessageComposeViewControllerDelegate  
- (void)messageComposeViewController:(MFMessageComposeViewController *)controller didFinishWithResult:(MessageComposeResult)result{  
	    [controller dismissModalViewControllerAnimated:NO];//关键的一句   不能为YES  
      
	    switch ( result ) {  
              
        case MessageComposeResultCancelled:  
  
            [self alertWithTitle:@"提示信息" msg:@"发送取消"];   
            break;  
        case MessageComposeResultFailed:// send failed  
            [self alertWithTitle:@"提示信息" msg:@"发送成功"];   
            break;  
        case MessageComposeResultSent:  
            [self alertWithTitle:@"提示信息" msg:@"发送失败"];   
            break;  
        default:  
            break;   
	    }  
	  }  
        
- (void) alertWithTitle:(NSString *)title msg:(NSString *)msg {  
	    UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title  
	           message:msg  
	           delegate:self  
	           cancelButtonTitle:nil  
	           otherButtonTitles:@"确定", nil];  
			[alert show];  
		}  
    }
```


