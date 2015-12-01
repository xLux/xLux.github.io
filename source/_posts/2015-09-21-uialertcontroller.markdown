---
layout: post
title: "iOS8新控件UIAlertController"
date: 2015-09-21 22:04:56 +0800
comments: true
categories: ['iOS开发']
---
UIAlertController是iOS8中新出现的控件，之前项目中一直使用的是UIAlertView，但是iOS9之后，UIAlertView彻底废除了，总结一下两个控件的用法。
<!--more-->
##UIAlertView
UIAlertView对话框是iOS5中出现的控件，不仅可以实现提示用户的作用，还可以添加UITextField输入控件，最多可以添加两个。UIAlertView的使用非常简单，最简单的用法如下：
```
UIAlertView *alertview = [[UIAlertView alloc] initWithTitle:@"标题" message:@"默认样式" delegate:self cancelButtonTitle:@"取消" otherButtonTitles:@"好的", nil];
[alertview show];
```
在UIAlertViewDelegate中监听哪个按钮被按下。
通过设置alertViewStyle属性来实现输入文字、密码，也可以实现作为登陆框的效果。
##UIAlertController
UIAlertController的使用也比较简单，按钮的动作回调方法不再使用delegate，而是用block，代码结构看起来更加清晰。UIAlertController可以添加多个UItextField来作为输入框，使用更加灵活。使用方法如下：
```
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"请输入抢单积分" message:@"积分不能小于5分" preferredStyle:UIAlertControllerStyleAlert];
                [alertController addTextFieldWithConfigurationHandler:^(UITextField * _Nonnull textField) {
                    textField.keyboardType = UIKeyboardTypeNumberPad;
                    textField.placeholder = @"所投积分";
                }];
                UIAlertAction *canselAction = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:nil];
                kWeakSelf(self);
                UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
                    UITextField *textField = [[alertController textFields] firstObject];
                    if (textField.text.length == 0) {
                        ALTip *tip = [[ALTip alloc] init];
                        tip.tip = @"请输入积分";
                        tip.parent = weakSelf;
                        [tip show];
                    }else if([textField.text integerValue] <= 5){
                        ALTip *tip = [[ALTip alloc] init];
                        tip.tip = @"输入的积分不符合规则";
                        tip.parent = weakSelf;
                        [tip show];
                    }else{
                        [weakSelf getTheGrab:textField.text];
                    }
                }];
                [alertController addAction:canselAction];
                [alertController addAction:okAction];
                [self presentViewController:alertController animated:YES completion:nil];
```