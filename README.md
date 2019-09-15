![readme](https://user-images.githubusercontent.com/37614260/43947531-922a0712-9cb2-11e8-8f8d-4823a21308d3.png)

[![Build Status](https://travis-ci.org/changsanjiang/SJVideoPlayer.svg?branch=master)](https://travis-ci.org/changsanjiang/SJVideoPlayer)
[![Version](https://img.shields.io/cocoapods/v/SJVideoPlayer.svg?style=flat)](https://cocoapods.org/pods/SJVideoPlayer)
[![Platform](https://img.shields.io/badge/platform-iOS-blue.svg)](https://github.com/changsanjiang)
[![License](https://img.shields.io/github/license/changsanjiang/SJVideoPlayer.svg)](https://github.com/changsanjiang/SJVideoPlayer/blob/master/LICENSE.md)

### Installation
```ruby
# Player with default control layer.
pod 'SJVideoPlayer'

# The base player, without the control layer, can be used if you need a custom control layer.
pod 'SJBaseVideoPlayer'
```

### 天朝
```ruby
# 如果网络不行安装不了, 可改成以下方式进行安装
pod 'SJBaseVideoPlayer', :git => 'https://gitee.com/changsanjiang/SJBaseVideoPlayer.git'
pod 'SJVideoPlayer', :git => 'https://gitee.com/changsanjiang/SJVideoPlayer.git'
pod 'SJUIKit/AttributesFactory', :git => 'https://gitee.com/changsanjiang/SJUIKit.git'
pod 'SJUIKit/ObserverHelper', :git => 'https://gitee.com/changsanjiang/SJUIKit.git'
pod 'SJUIKit/Queues', :git => 'https://gitee.com/changsanjiang/SJUIKit.git'
$ pod update --no-repo-update   (不要用 pod install 了, 用这个命令安装)
```

## Example

```Objective-C
_player = [SJVideoPlayer player];
_player.view.frame = CGRectMake(0, 0, 200, 200);
[self.view addSubview:_player.view];

// 设置资源进行播放
_player.URLAsset = [[SJVideoPlayerURLAsset alloc] initWithURL:URL];

... 等等, 更多设置, 请查看头文件. 相应功能均为懒加载, 用到时才会创建. 
```

## Documents

#### [1. 视图层次结构](#1)
* [1.1 UIView](#1.1)
* [1.2 UITableView 中的层次结构](#1.2)
    * [1.2.1 UITableViewCell](#1.2.1)
    * [1.2.2 UITableView.tableHeaderView](#1.2.2)
    * [1.2.3 UITableView.tableFooterView](#1.2.3)
    * [1.2.4 UITableViewHeaderFooterView](#1.2.4)
* [1.3 UICollectionView 中的层次结构](#1.3)
    * [1.3.1 UICollectionViewCell](#1.3.1)
* [1.4  嵌套时的层次结构](#1.4)
    * [1.4.1 UICollectionView 嵌套在 UITableViewCell 中](#1.4.1)
    * [1.4.2 UICollectionView 嵌套在 UITableViewHeaderView 中](#1.4.2)
    * [1.4.3 UICollectionView 嵌套在 UICollectionViewCell 中](#1.4.3)

<p>

由于 UITableView 及 UICollectionView 的复用机制, 会导致播放器视图显示在错误的位置上, 为防止出现此种情况, 在创建资源时指定视图层次结构, 使得播放器能够定位具体的父视图, 依此来控制隐藏与显示. 

</p>

___

#### [2. URLAsset](#2)
* [2.1 播放 URL(本地文件或远程资源)](#2.1)
* [2.2 播放 AVAsset 或其子类](#2.2)
* [2.3 从指定的位置开始播放](#2.3)
* [2.4 续播(进入下个页面时, 继续播放)](#2.4)
* [2.5 销毁时的回调. 可在此做一些记录工作, 如播放记录](#2.5)

<p>

```Objective-C
SJVideoPlayerURLAsset *asset = [[SJVideoPlayerURLAsset alloc] initWithURL:URL];
_player.URLAsset = asset;
```

播放资源是通过 SJVideoPlayerURLAsset 进行创建的. 默认情况下, 创建了 SJVideoPlayerURLAsset , 赋值给播放器后即可播放.  

</p>

#### [3. 播放控制](#3)
* [3.1 当前时间和时长](#3.1)
* [3.2 时间改变时的回调](#3.2)
* [3.3 播放结束后的回调](#3.3)
* [3.4 资源准备状态](#3.4)
* [3.5 播放控制状态](#3.5)
* [3.6 播放等待的原因](#3.6)
* [3.7 播放状态改变的回调](#3.7)
* [3.8 是否自动播放 - 当资源初始化完成后](#3.8)
* [3.9 刷新 ](#3.9)
* [3.10 播放器的声音设置 & 静音](#3.10)
* [3.11 播放](#3.11)
* [3.12 暂停](#3.12)
* [3.13 是否暂停 - 当App进入后台后](#3.13)
* [3.14 停止](#3.14)
* [3.15 重播](#3.15)
* [3.16 跳转到指定的时间播放](#3.16)
* [3.17 调速 & 速率改变时的回调](#3.17)
* [3.18 接入别的视频 SDK, 自己动手撸一个 SJMediaPlaybackController, 替换作者原始实现](#3.18)

#### [4. 控制层的显示和隐藏](#4)
* [4.1 让控制层显示](#4.1)
* [4.2 让控制层隐藏](#4.2)
* [4.3 控制层是否显示中](#4.3)
* [4.4 是否在暂停时保持控制层显示](#4.4)
* [4.5 是否自动显示控制层 - 资源初始化完成后](#4.5)
* [4.6 控制层显示状态改变的回调](#4.6)
* [4.7 禁止管理控制层的显示和隐藏](#4.7)
* [4.8 自己动手撸一个 SJControlLayerAppearManager, 替换作者原始实现](#4.8)

#### [5. 设备亮度和音量](#5)
* [5.1 调整设备亮度](#5.1)
* [5.2 调整设备声音](#5.2)
* [5.3 亮度 & 声音改变后的回调](#5.3)
* [5.4 禁止播放器设置](#5.4)
* [5.5 自己动手撸一个 SJDeviceVolumeAndBrightnessManager, 替换作者原始实现](#5.5)

#### [6. 旋转](#6)
* [6.1 自动旋转](#6.1)
* [6.2 设置自动旋转支持的方向](#6.2)
* [6.3 禁止自动旋转](#6.3)
* [6.4 主动调用旋转](#6.4)
* [6.5 是否全屏](#6.5)
* [6.6 是否正在旋转](#6.6)
* [6.7 当前旋转的方向 ](#6.7)
* [6.8 旋转开始和结束的回调](#6.8) 
* [6.9 自己动手撸一个 SJRotationManager, 替换作者原始实现](#6.9)

#### [7. 直接全屏而不旋转](#7)
* [7.1 全屏和恢复](#7.1)
* [7.2 开始和结束的回调](#7.2)
* [7.3 是否是全屏](#7.3)
* [7.4 自己动手撸一个 SJFitOnScreenManager, 替换作者原始实现](#7.4)

#### [8. 镜像翻转](#8)
* [8.1 翻转和恢复](#8.1)
* [8.2 开始和结束的回调](#8.2)
* [8.3  自己动手撸一个 SJFlipTransitionManager, 替换作者原始实现](#8.3)

#### [9. 网络状态](#9)
* [9.1 当前的网络状态](#9.1)
* [9.2 网络状态改变的回调](#9.2)
* [9.3 自己动手撸一个 SJReachability, 替换作者原始实现](#9.3)

#### [10. 手势](#10)
* [10.1 单击手势](#10.1)
* [10.2 双击手势](#10.2)
* [10.3 移动手势](#10.3)
* [10.4 捏合手势](#10.4)
* [10.5 禁止某些手势](#10.5)
* [10.6 自定义某个手势的处理](#10.6)
* [10.7 自己动手撸一个 SJPlayerGestureControl, 替换作者原始实现](#10.7)

#### [11. 占位图](#11)
* [11.1 设置本地占位图](#11.1)
* [11.2 设置网络占位图](#11.2)
* [11.3 是否隐藏占位图 - 播放器准备好显示时](#11.3)

#### [12. 显示提示文本](#12)
* [12.1 显示文本及持续时间 - (NSString or NSAttributedString)](#12.1)
* [12.2 配置提示文本](#12.2)

#### [13. 一些固定代码](#13)
* [13.1 - (void)vc_viewDidAppear; ](#13.1)
* [13.2 - (void)vc_viewWillDisappear;](#13.2)
* [13.3 - (void)vc_viewDidDisappear;](#13.3)
* [13.4 - (BOOL)vc_prefersStatusBarHidden;](#13.4)
* [13.5 - (UIStatusBarStyle)vc_preferredStatusBarStyle;](#13.5)
* [13.6 - 临时显示状态栏](#13.6)
* [13.7 - 临时隐藏状态栏](#13.7)

#### [14. 截屏](#14)
* [14.1 当前时间截图](#14.1)
* [14.2 指定时间截图](#14.2)
* [14.3 生成预览视图, 大约20张](#14.3)

#### [15. 导出视频或GIF](#15)
* [15.1 导出视频](#15.1)
* [15.2 导出GIF](#15.2)
* [15.3 取消操作](#15.3)

#### [16. 滚动相关](#16)
* [16.1 是否在 UICollectionView 或者 UITableView 中播放](#16.1)
* [16.2 是否滚动显示](#16.2)
* [16.3 播放器视图将要滚动显示和消失的回调](#16.3)

#### [17. 自动播放 - 在 UICollectionView 或者 UITableView 中](#17)
* [17.1 开启](#17.1)
* [17.2 配置](#17.2)
* [17.3 关闭](#17.3)
* [17.4 主动调用播放下一个资源](#17.4)


___


## 以下为详细介绍: 


<h3 id="1.1">1.1 UIView</h3>  

```Objective-C
_player = [SJVideoPlayer player];
_player.view.frame = ...;
[self.view addSubview:_player.view];

// 设置资源进行播放
SJVideoPlayerURLAsset *asset = [[SJVideoPlayerURLAsset alloc] initWithURL:URL];
_player.URLAsset = asset;
```

<p>

在普通视图中播放时, 不需要指定视图层次, 直接创建资源进行播放即可.  

</p>

___

<h3 id="1.2">1.2 UITableView 中的层次结构</h3>

<p>

在 UITableView 中播放时, 需指定视图层次, 使得播放器能够定位具体的父视图, 依此来控制隐藏与显示.

</p>

___


<h3 id="1.2.1">1.2.1 UITableViewCell</h3>

```Objective-C
--  UITableView
    --  UITableViewCell
        --  Player.superview
            --  Player.view


_player = [SJVideoPlayer player];

UIView *playerSuperview = cell.coverImageView;
SJPlayModel *playModel = [SJPlayModel UITableViewCellPlayModelWithPlayerSuperviewTag:playerSuperview.tag atIndexPath:indexPath tableView:self.tableView];

_player.URLAsset = [[SJVideoPlayerURLAsset alloc] initWithURL:URL playModel:playModel];
```

<p>

在 UITableViewCell 中播放时, 需指定 Cell 所处的 indexPath 以及播放器父视图的 tag. 

在滑动时, 管理类将会通过这两个参数控制播放器父视图的显示与隐藏.

</p>

___


<h3 id="1.2.2">1.2.2 UITableView.tableHeaderView</h3>

```Objective-C
--  UITableView
    --  UITableView.tableHeaderView 或者 UITableView.tableFooterView  
        --  Player.superview
            --  Player.view

UIView *playerSuperview = self.tableView.tableHeaderView;
// 也可以设置子视图
// playerSuperview = self.tableView.tableHeaderView.coverImageView;
SJPlayModel *playModel = [SJPlayModel UITableViewHeaderViewPlayModelWithPlayerSuperview:playerSuperview tableView:self.tableView];
```

___


<h3 id="1.2.3">1.2.3 UITableView.tableFooterView</h3>

```Objective-C
--  UITableView
    --  UITableView.tableHeaderView 或者 UITableView.tableFooterView  
        --  Player.superview
            --  Player.view

UIView *playerSuperview = self.tableView.tableFooterView;
// 也可以设置子视图
// playerSuperview = self.tableView.tableFooterView.coverImageView;
SJPlayModel *playModel = [SJPlayModel UITableViewHeaderViewPlayModelWithPlayerSuperview:playerSuperview tableView:self.tableView];
```

___


<h3 id="1.2.4">1.2.4 UITableViewHeaderFooterView</h3>

```Objective-C
--  UITableView
    --  UITableViewHeaderFooterView 
        --  Player.superview
            --  Player.view            

/// isHeader: 当在header中播放时, 传YES, 在footer时, 传NO.
SJPlayModel *playModel = [SJPlayModel UITableViewHeaderFooterViewPlayModelWithPlayerSuperviewTag:sectionHeaderView.coverImageView.tag inSection:section isHeader:YES tableView:self.tableView];
```

___


<h3 id="1.3">1.3 UICollectionView 中的层次结构</h3>

<p>

在 UICollectionView 中播放时, 同 [UITableView](#1.2) 中一样, 需指定视图层次, 使得播放器能够定位具体的父视图, 依此来控制隐藏与显示.

</p>

___


<h3 id="1.3.1">1.3.1 UICollectionViewCell</h3>

```Objective-C
--  UICollectionView
    --  UICollectionViewCell
        --  Player.superview
            --  Player.view

SJPlayModel *playModel = [SJPlayModel UICollectionViewCellPlayModelWithPlayerSuperviewTag:cell.coverImageView.tag atIndexPath:indexPath collectionView:self.collectionView];
```

___


<h3 id="1.4">1.4 嵌套时的视图层次</h3>

<p>

嵌套的情况下, 传递的参数比较多, 不过熟悉了前面的套路, 下面的这些也不成问题.  (会被复用的视图, 传 tag. 如果不会被复用, 则直接传视图)

</p>

___


<h3 id="1.4.1">1.4.1 UICollectionView 嵌套在 UITableViewCell 中</h3>

```Objective-C
--  UITableView
    --  UITableViewCell
        --  UITableViewCell.UICollectionView
            --  UICollectionViewCell
                --  Player.superview
                    --  Player.view

SJPlayModel *playModel = [SJPlayModel UICollectionViewNestedInUITableViewCellPlayModelWithPlayerSuperviewTag:collectionViewCell.coverImageView.tag atIndexPath:collectionViewCellAtIndexPath collectionViewTag:tableViewCell.collectionView.tag collectionViewAtIndexPath:tableViewCellAtIndexPath tableView:self.tableView];
```

___


<h3 id="1.4.2">1.4.2 UICollectionView 嵌套在 UITableViewHeaderView 中</h3>

```Objective-C
--  UITableView
    --  UITableView.tableHeaderView 或者 UITableView.tableFooterView  
        --  tableHeaderView.UICollectionView
            --  UICollectionViewCell
                --  Player.superview
                    --  Player.view

SJPlayModel *playModel = [SJPlayModel UICollectionViewNestedInUITableViewHeaderViewPlayModelWithPlayerSuperviewTag:cell.coverImageView.tag atIndexPath:indexPath collectionView:tableHeaderView.collectionView tableView:self.tableView];
```

___


<h3 id="1.4.3">1.4.3 UICollectionView 嵌套在 UICollectionViewCell 中</h3>

```Objective-C
--  UICollectionView
    --  UICollectionViewCell
        --  UICollectionViewCell.UICollectionView
            --  UICollectionViewCell
                --  Player.superview
                    --  Player.view

SJPlayModel *playModel = [SJPlayModel UICollectionViewNestedInUICollectionViewCellPlayModelWithPlayerSuperviewTag:collectionViewCell.coverImageView.tag atIndexPath:collectionViewCellAtIndexPath collectionViewTag:rootCollectionViewCell.collectionView.tag collectionViewAtIndexPath:collectionViewAtIndexPath rootCollectionView:self.collectionView];
```

___


<h3 id="2">2. URLAsset</h3>

<p>

播放器 播放的资源是通过 SJVideoPlayerURLAsset 创建的. SJVideoPlayerURLAsset 由两部分组成:

视图层次 (第一部分中的SJPlayModel)
资源地址 (可以是本地资源/URL/AVAsset)

默认情况下, 创建了 SJVideoPlayerURLAsset , 赋值给播放器后即可播放.

</p>

___

<h3 id="2.1">2.1 播放 URL(本地文件或远程资源)</h3>

```Objective-C
NSURL *URL = [NSURL URLWithString:@"https://...example.mp4"];
_player.URLAsset = [[SJVideoPlayerURLAsset alloc] initWithURL:URL];
```

<h3 id="2.2">2.2 播放 AVAsset 或其子类</h3>

```Objective-C
_player.URLAsset = [[SJVideoPlayerURLAsset alloc] initWithAVAsset:avAsset];
```

<h3 id="2.3">2.3 从指定的位置开始播放</h3>

```Objective-C
NSTimeInterval secs = 20.0;
_player.URLAsset = [[SJVideoPlayerURLAsset alloc] initWithURL:URL specifyStartTime:secs]; // 直接从20秒处开始播放
```

<h3 id="2.4">2.4 续播(进入下个页面时, 继续播放)</h3>

<p>

我们可能需要切换界面时, 希望视频能够在下一个界面无缝的进行播放. 使用如下方法, 传入正在播放的资源, 将新的资源赋值给播放器播放即可. 

</p>

```Objective-C
// otherAsset 即为上一个页面播放的Asset
// 除了需要一个otherAsset, 其他方面同以上的示例一模一样
_player.URLAsset = [SJVideoPlayerURLAsset.alloc initWithOtherAsset:otherAsset]; 
```

<h3 id="2.5">2.5 销毁时的回调. 可在此做一些记录工作, 如播放记录</h3>

```Objective-C
// 每个资源dealloc时的回调
_player.assetDeallocExeBlock = ^(__kindof SJBaseVideoPlayer * _Nonnull videoPlayer) {
    // .....
};
```

当资源销毁时, 播放器将会回调该 block. 

___


<h2 id="3">3. 播放控制</h2>

<p>
播放控制: 对播放进行的操作. 此部分的内容由 "id &lt;SJMediaPlaybackController&gt; playbackController" 提供支持.

大多数对播放进行的操作, 均在协议 SJMediaPlaybackController 进行了声明. 

正常来说实现了此协议的任何对象, 均可赋值给 player.playbackController 来替换原始实现.
</p>

<h3 id="3.1">3.1 当前时间和时长</h3>

```Objective-C
/// 当前时间
_player.currentTime

/// 时长
_player.duration

/// 字符串化, 
/// - 格式为 00:00(小于 1 小时) 或者 00:00:00 (大于 1 小时)
_player.currentTimeStr
_player.durationStr
```

<h3 id="3.2">3.2 时间改变时的回调</h3>

```Objective-C
_player.playbackObserver.currentTimeDidChangeExeBlock = ^(__kindof SJBaseVideoPlayer * _Nonnull player) {
    /// ...
};
```

<h3 id="3.3">3.3 播放结束后的回调</h3>

```Objective-C
_player.playbackObserver.didPlayToEndTimeExeBlock = ^(__kindof SJBaseVideoPlayer * _Nonnull player) {
    /// ...
};
```

<h3 id="3.4">3.4 资源准备状态</h3>

<p>

资源准备(或初始化)的状态

         当未设置资源时, 此时 player.assetStatus = .unknown
         当设置新资源时, 此时 player.assetStatus = .preparing
         当准备好播放时, 此时 player.assetStatus = .readyToPlay
         当初始化失败时, 此时 player.assetStatus = .failed
         
</p>

```Objective-C
typedef NS_ENUM(NSInteger, SJAssetStatus) {
    ///
    /// 未知状态
    ///
    SJAssetStatusUnknown,
    
    ///
    /// 准备中
    ///
    SJAssetStatusPreparing,
    
    ///
    /// 当前资源可随时进行播放(播放控制请查看`timeControlStatus`)
    ///
    SJAssetStatusReadyToPlay,
    
    ///
    /// 发生错误
    ///
    SJAssetStatusFailed
};
```

<h3 id="3.5">3.5 播放控制状态</h3>

<p>

 暂停或播放的控制状态

         当调用了暂停时, 此时 player.timeControlStatus = .paused
         
         当调用了播放时, 此时 将可能处于以下两种状态中的任意一个:
                         - player.timeControlStatus = .playing
                             正在播放中.

                         - player.timeControlStatus = .waitingToPlay
                             等待播放, 等待的原因请查看 player.reasonForWaitingToPlay

</p>

```Objective-C
typedef NS_ENUM(NSInteger, SJPlaybackTimeControlStatus) {
    ///
    /// 暂停状态(已调用暂停或未执行任何操作的状态)
    ///
    SJPlaybackTimeControlStatusPaused,
    
    ///
    /// 播放状态(已调用播放), 当前正在缓冲或正在评估能否播放. 可以通过`reasonForWaitingToPlay`来获取原因, UI层可以根据原因来控制loading视图的状态.
    ///
    SJPlaybackTimeControlStatusWaitingToPlay,
    
    ///
    /// 播放状态(已调用播放), 当前播放器正在播放
    ///
    SJPlaybackTimeControlStatusPlaying
};
```

<h3 id="3.6">3.6 播放等待的原因</h3>

<p>

 当调用了播放, 播放器未能播放处于等待状态时的原因

         等待原因有以下3种状态:
             1.未设置资源, 此时设置资源后, 当`player.assetStatus = .readyToPlay`, 播放器将自动进行播放.
             2.可能是由于缓冲不足, 播放器在等待缓存足够时自动恢复播放, 此时可以显示loading视图.
             3.可能是正在评估缓冲中, 这个过程会进行的很快, 不需要显示loading视图.

</p>

```Objective-C
///
/// 缓冲中, UI层建议显示loading视图 
///
extern SJWaitingReason const SJWaitingToMinimizeStallsReason;

///
/// 正在评估能否播放, 处于此状态时, 不建议UI层显示loading视图
///
extern SJWaitingReason const SJWaitingWhileEvaluatingBufferingRateReason;

///
/// 未设置资源
///
extern SJWaitingReason const SJWaitingWithNoAssetToPlayReason;
```

<h3 id="3.7">3.7 播放状态改变的回调</h3>

<p>
对播放状态的判断我添加了一个便利的分类 `SJBaseVideoPlayer (PlayStatus)`

如需判断状态, 可导入头文件 `#import "SJBaseVideoPlayer+PlayStatus.h"` 使用. 
</p>

```Objective-C
/// 播放状态改变的回调
_player.playStatusDidChangeExeBlock = ^(__kindof SJBaseVideoPlayer * _Nonnull videoPlayer) {

};

/// 对播放状态的判断我添加了一个便利的分类
@interface SJBaseVideoPlayer (PlayStatus)

- (NSString *)getPlayStatusStr:(SJVideoPlayerPlayStatus)status;

- (BOOL)playStatus_isUnknown;

- (BOOL)playStatus_isPrepare;

- (BOOL)playStatus_isReadyToPlay;

- (BOOL)playStatus_isPlaying;

- (BOOL)playStatus_isPaused;

- (BOOL)playStatus_isPaused_ReasonBuffering;

- (BOOL)playStatus_isPaused_ReasonPause;

- (BOOL)playStatus_isPaused_ReasonSeeking;

- (BOOL)playStatus_isInactivity;

- (BOOL)playStatus_isInactivity_ReasonPlayEnd;

- (BOOL)playStatus_isInactivity_ReasonPlayFailed;

@end
```

<h3 id="3.8">3.8 是否自动播放 - 当资源初始化完成后</h3>

```Objective-C
_player.autoPlayWhenPlayStatusIsReadyToPlay = YES;
```

<h3 id="3.9">3.9 刷新</h3>

<p>
在播放一个资源时, 可能有一些意外情况导致播放失败(如网络环境差). 

此时当用户点击刷新按钮, 我们需要对当前的资源(Asset)进行刷新. 

SJBaseVideoPlayer提供了直接的方法去刷新, 不需要开发者再重复的去创建新的Asset.
</p>

```Objective-C
[_player refresh];
```

<h3 id="3.10">3.10 播放器的声音设置 & 静音</h3>

```Objective-C
/// 默认值为 1.0, 最小为 0.0
_player.playerVolume = 1.0;

/// 设置静音
_player.mute = YES;
```

<h3 id="3.11">3.11 播放</h3>

```Objective-C
[_player play];
```

<h3 id="3.12">3.12 暂停</h3>

```Objective-C
[_player pause];
```

<h3 id="3.13">3.13 是否暂停 - 当App进入后台后</h3>

<p>
关于后台播放视频, 引用自: https://juejin.im/post/5a38e1a0f265da4327185a26

当您想在后台播放视频时:

1. 需要设置 videoPlayer.pauseWhenAppDidEnterBackground = NO; (该值默认为YES, 即App进入后台默认暂停).

2. 前往 `TARGETS` -> `Capability` -> enable `Background Modes` -> select this mode `Audio, AirPlay, and Picture in Picture`
</p>

```Objective-C
_player.pauseWhenAppDidEnterBackground = NO; // 默认值为 YES, 即进入后台后 暂停.
```

<h3 id="3.14">3.14 停止</h3>

<p>
注意, 调用此方法后, 当前的 asset 将会被清空. 也就是说, 调用 play等播放操作将会无效.
</p>

```Objective-C
[_player stop];
```

<h3 id="3.15">3.15 重播</h3>

<p>
从头开始重新播放
</p>

```Objective-C
[_player replay];
```

<h3 id="3.16">3.16 跳转到指定的时间播放</h3>

```Objective-C
NSTimeInterval secs = 20.0;
[_player seekToTime:secs completionHandler:^(BOOL finished) {
    // ....
}];
```

<h3 id="3.17">3.17 调速 & 速率改变时的回调</h3>

```Objective-C

/// 默认值为 1.0
_player.rate = 1.0;


_player.rateDidChangeExeBlock = ^(__kindof SJBaseVideoPlayer * _Nonnull player) {
    /// .. 
}
```

<h3 id="3.18">3.18 接入别的视频 SDK, 自己动手撸一个 SJMediaPlaybackController, 替换作者原始实现</h3>

<p>
某些时候, 我们需要接入第三方的视频SDK, 但是又想使用 SJBaseVideoPlayer 封装的其他的功能. 

这个时候, 我们可以自己动手, 将第三方的SDK封装一下, 实现 SJMediaPlaybackController 协议, 管理 SJBaseVideoPlayer 中的播放操作.

示例:

- 可以参考 SJAVMediaPlaybackController 中的实现.
- 封装 ijkplayer 的示例:  https://gitee.com/changsanjiang/SJIJKMediaPlaybackController
</p>

```Objective-C
_player.playbackController = Your PlaybackController.
```

___

<h2 id="4">4. 控制层的显示和隐藏</h4>

<p>
控制层的显示和隐藏, 此部分的内容由 "id &lt;SJControlLayerAppearManager&gt; controlLayerAppearManager" 提供支持.

controlLayerAppearManager 内部存在一个定时器, 当控制层显示时, 会开启此定时器. 一定间隔后,  会尝试隐藏控制层.

其他相关操作, 请见以下内容. 
</p>

<h3 id="4.1">4.1 让控制层显示</h3>

<p>
当控制层需要显示时, 可以调用下面方法. 

此方法将会回调控制层的代理方法:

 "- (void)controlLayerNeedAppear:(__kindof SJBaseVideoPlayer *)videoPlayer;"
 
 代理将会对当前的控制层进行显示处理.
</p>

```Objective-C
[_player controlLayerNeedAppear];
```

<h3 id="4.2">4.2 让控制层隐藏</h3>

<p>
当控制层需要隐藏时, 可以调用下面方法. 

此方法将会回调控制层的代理方法:

"- (void)controlLayerNeedDisappear:(__kindof SJBaseVideoPlayer *)videoPlayer;"

代理将会对当前的控制层进行隐藏处理.
</p>

```Objective-C
[_player controlLayerNeedDisappear];
```

<h3 id="4.3">4.3 控制层是否显示中</h3>

```Objective-C
/// 是否显示, YES为显示, NO为隐藏
_player.controlLayerIsAppeared
```

<h3 id="4.4">4.4 是否在暂停时保持控制层显示</h3>

```Objective-C
/// 默认为 NO, 即不保持显示
_player.pausedToKeepAppearState = YES;
```

<h3 id="4.5">4.5 是否自动显示控制层 - 资源初始化完成后</h3>

```Objective-C
/// 默认为 NO, 即不显示
_player.controlLayerAutoAppearWhenAssetInitialized = YES;
```

<h3 id="4.6">4.6 控制层显示状态改变的回调</h3>

```Objective-C
@property (nonatomic, copy, nullable) void(^controlLayerAppearStateDidChangeExeBlock)(__kindof SJBaseVideoPlayer *player, BOOL state);
```

<h3 id="4.7">4.7 禁止管理控制层的显示和隐藏</h3>

<p>
有时候, 我们可能不需要对控制层的显示和隐藏进行管理.  这个时候可以设置如下属性, 来禁止管理类的操作.
</p>

```Objective-C
@property (nonatomic) BOOL disabledControlLayerAppearManager; // default value is NO.
```

<h3 id="4.8">4.8 自己动手撸一个 SJControlLayerAppearManager, 替换作者原始实现</h3>

<p>
同样的, 协议 "SJControlLayerAppearManager" 定义了一系列的操作, 只要实现了这些协议方法的对象, 就可以管理控制层的显示和隐藏.
</p>

```Objective-C
_player.controlLayerAppearManager = Your controlLayerAppearManager; 
```

___

<h2 id="5">5. 设备亮度和音量</h2>

<p>
设备亮度和音量的调整, 此部分的内容由 "id &lt;SJDeviceVolumeAndBrightnessManager&gt; deviceVolumeAndBrightnessManager" 提供支持.
</p>

<h3 id="5.1">5.1 调整设备亮度</h2>

```Objective-C
_player.deviceBrightness = 1.0;
```

<h3 id="5.2">5.2 调整设备声音</h2>

```Objective-C
_player.deviceVolume = 1.0;
```

<h3 id="5.3">5.3 亮度 & 声音改变后的回调</h2>

```Objective-C
_observer = [_player.deviceVolumeAndBrightnessManager getObserver];

observer.volumeDidChangeExeBlock = ...;
observer.brightnessDidChangeExeBlock = ...;
```

<h3 id="5.4">5.4 禁止播放器设置</h2>

```Objective-C
_player.disableBrightnessSetting = YES;
_player.disableVolumeSetting = YES;
```

<h3 id="5.5">5.5 自己动手撸一个 SJDeviceVolumeAndBrightnessManager, 替换作者原始实现</h2>

<p>
当需要对设备音量视图进行自定义时, 可以自己动手撸一个 SJDeviceVolumeAndBrightnessManager. 
</p>

```Objective-C
_player.deviceVolumeAndBrightnessManager = Your deviceVolumeAndBrightnessManager;
```

___

<h2 id="6">6. 旋转</h2>

<p>
此部分的内容由 "id &lt;SJRotationManagerProtocol&gt; rotationManager" 提供支持.

对于旋转, 我们开发者肯定需要绝对的控制, 例如: 设置自动旋转所支持方向. 能够主动+自动旋转, 而且还需要能在适当的时候禁止自动旋转. 旋转前后的回调等等... 放心这些功能都有, 我挨个给大家介绍.

另外旋转有两种方式:

- 仅旋转播放器视图 (默认情况下)
- 使 ViewController 也一起旋转

具体请看下面介绍.
</p>

<h3 id ="6.1">6.1 自动旋转</h3>

<p>
先说说何为自动旋转. 其实就是当设备方向变更时, 播放器根据设备方向进行自动旋转.
</p>

<h3 id ="6.2">6.2 设置自动旋转支持的方向</h3>

```Objective-C
/// 设置自动旋转支持的方向
_player.supportedOrientation = SJAutoRotateSupportedOrientation_LandscapeLeft | SJAutoRotateSupportedOrientation_LandscapeRight;


/**
 自动旋转支持的方向
 
 - SJAutoRotateSupportedOrientation_Portrait:       竖屏
 - SJAutoRotateSupportedOrientation_LandscapeLeft:  支持全屏, Home键在右侧
 - SJAutoRotateSupportedOrientation_LandscapeRight: 支持全屏, Home键在左侧
 - SJAutoRotateSupportedOrientation_All:            全部方向
 */
typedef NS_ENUM(NSUInteger, SJAutoRotateSupportedOrientation) {
    SJAutoRotateSupportedOrientation_Portrait = 1 << 0,
    SJAutoRotateSupportedOrientation_LandscapeLeft = 1 << 1,  
    SJAutoRotateSupportedOrientation_LandscapeRight = 1 << 2, 
    SJAutoRotateSupportedOrientation_All = SJAutoRotateSupportedOrientation_Portrait | SJAutoRotateSupportedOrientation_LandscapeLeft | SJAutoRotateSupportedOrientation_LandscapeRight,
};
```

<h3 id ="6.3">6.3 禁止自动旋转</h3>

<p>
这里有两点需要注意: 

- 合适的时候要记得恢复自动旋转. 
- 禁止自动旋转后, 主动调用旋转, 还是可以旋转的.
</p>

```Objective-C
_player.disableAutoRotation = YES;
```

<h3 id ="6.4">6.4 主动调用旋转</h3>

<p>
主动旋转. 当我们想主动旋转时, 大概分为以下三点:

-   播放器旋转到用户当前的设备方向或恢复小屏.
-   主动旋转到指定方向.
-   主动旋转完成后的回调.

请看以下方法, 分别对应以上三点:
</p>

```Objective-C
- (void)rotate;
- (void)rotate:(SJOrientation)orientation animated:(BOOL)animated;
- (void)rotate:(SJOrientation)orientation animated:(BOOL)animated completion:(void (^ _Nullable)(__kindof SJBaseVideoPlayer *player))block;
```

<h3 id ="6.5">6.5 是否全屏</h3>

```Objective-C
/// 如果为YES, 表示全屏
_player.isFullScreen
```

<h3 id ="6.6">6.6 是否正在旋转</h3>

```Objective-C
/// 如果为YES, 表示正在旋转中
_player.isTransitioning
```

<h3 id ="6.7">6.7 当前旋转的方向</h3>

```Objective-C
_player.orientation
```

<h3 id ="6.8">6.8 旋转开始和结束的回调</h3>

```Objective-C
_observer = [self.rotationManager getObserver];
_observer.rotationDidStartExeBlock = ^(id<SJRotationManagerProtocol>  _Nonnull mgr) {
    /// ...
};
    
_observer.rotationDidEndExeBlock = ^(id<SJRotationManagerProtocol>  _Nonnull mgr) {
    /// ...
};
```

<h3 id ="6.9">6.9 自己动手撸一个 SJRotationManager, 替换作者原始实现</h3>

当你想替换原始实现时, 可以实现 SJRotationManagerProtocol 中定义的方法.

___

<h2 id="7">7. 直接全屏而不旋转</h2>

<p>
直接全屏, 或者说充满屏幕, 但不旋转.
</p>

<h3 id="7.1">7.1 全屏和恢复</h3>

```Objective-C
_player.fitOnScreen = YES;

[_player setFitOnScreen:NO animated:NO];

[_player setFitOnScreen:YES animated:YES completionHandler:^(__kindof SJBaseVideoPlayer * _Nonnull player) {
    /// ...
}];
```

<h3 id="7.2">7.2 开始和结束的回调</h3>

```Objective-C
@property (nonatomic, copy, nullable) void(^fitOnScreenWillBeginExeBlock)(__kindof SJBaseVideoPlayer *player);
@property (nonatomic, copy, nullable) void(^fitOnScreenDidEndExeBlock)(__kindof SJBaseVideoPlayer *player);;
```

<h3 id="7.3">7.3 是否是全屏</h3>

```Objective-C
/// YES 为充满屏幕 
_player.isFitOnScreen
```

<h3 id="7.4">7.4 自己动手撸一个 SJFitOnScreenManager, 替换作者原始实现</h3>

该部分管理类的协议定义在 SJFitOnScreenManagerProtocol 中, 实现该协议的任何对象, 均可赋值给播放器, 替换原始实现.

___

<h2 id="8">8. 镜像翻转</h2>

<p>
此部分内容由 id&lt;SJFlipTransitionManager&gt; flipTransitionManager 提供支持

目前镜像翻转只写了 水平翻转, 未来可能会加入更多的翻转类型.
</p>

```Objective-C
typedef enum : NSUInteger {
    SJViewFlipTransition_Identity,
    SJViewFlipTransition_Horizontally, // 水平翻转
} SJViewFlipTransition;
```

<h3 id="8.1">8.1 翻转和恢复</h3>

```Objective-C
/// 当前的翻转类型
_player.flipTransition

/// 翻转相关方法
[_player setFlipTransition:SJViewFlipTransition_Horizontally];
[_player setFlipTransition:SJViewFlipTransition_Horizontally animated:YES];
[_player setFlipTransition:SJViewFlipTransition_Identity animated:YES completionHandler:^(__kindof SJBaseVideoPlayer * _Nonnull player) {
    /// ...
}];
```

<h3 id="8.2">8.2 开始和结束的回调</h3>

```Objective-C
@property (nonatomic, copy, nullable) void(^flipTransitionDidStartExeBlock)(__kindof SJBaseVideoPlayer *player);
@property (nonatomic, copy, nullable) void(^flipTransitionDidStopExeBlock)(__kindof SJBaseVideoPlayer *player);
```

<h3 id="8.3">8.3  自己动手撸一个 SJFlipTransitionManager, 替换作者原始实现</h3>

<p>
该部分管理类的协议定义在 SJFlipTransitionManagerProtocol 中, 实现该协议的任何对象, 均可赋值给播放器, 替换原始实现.
</p>

___

<h2 id="9">9. 网络状态</h2>

<p>
此部分内容由 id&lt;SJReachability&gt; reachability 提供支持

默认的 reachability 是个单例, 在App生命周期中, 仅创建一次. 因此每个播放器对象持有的 reachability 都是相同的. 
</p>

<h3 id="9.1">9.1 当前的网络状态</h3>

```Objective-C
@property (nonatomic, readonly) SJNetworkStatus networkStatus;
```

<h3 id="9.1">9.2 网络状态改变的回调</h3>

```Objective-C
@property (nonatomic, copy, nullable) void(^networkStatusDidChangeExeBlock)(__kindof SJBaseVideoPlayer *player);
```

<h3 id="9.1">9.3 自己动手撸一个 SJReachability, 替换作者原始实现</h3>

<p>
该部分管理类的协议定义在 SJNetworkStatus 中, 实现该协议的任何对象, 均可赋值给播放器, 替换原始实现.
</p>

___

<h2 id="10">10. 手势</h2>
<p>
此部分内容由 id&lt;SJPlayerGestureControl&gt; gestureControl 提供支持

播放器默认存在四种手势, 每个手势触发的回调均定义在 SJPlayerGestureControl 中, 当想改变某个手势的处理时, 可以直接修改对应手势触发的 block 即可.

具体请看以下部分.
</p>

<h3 id="10.1">10.1 单击手势</h3>

当用户单击播放器时, 播放器会调用 [显示或隐藏控制层的操作](#4)

以下为默认实现: 

```Objective-C
__weak typeof(self) _self = self;
_gestureControl.singleTapHandler = ^(id<SJPlayerGestureControl>  _Nonnull control, CGPoint location) {
    __strong typeof(_self) self = _self;
    if ( !self ) return ;
    /// 让控制层显示或隐藏
    [self.controlLayerAppearManager switchAppearState];
};
```

<h3 id="10.2">10.2 双击手势</h3>

<p>
双击会触发暂停或播放的操作
</p>

```Objective-C
__weak typeof(self) _self = self;
_gestureControl.doubleTapHandler = ^(id<SJPlayerGestureControl>  _Nonnull control, CGPoint location) {
    __strong typeof(_self) self = _self;
    if ( !self ) return ;
    if ( [self playStatus_isPlaying] )
        [self pause];
    else
        [self play];
};
```

<h3 id="10.3">10.3 移动手势</h3>

- 垂直滑动时, 默认情况下如果在屏幕左边, 则会触发调整亮度的操作, 并显示亮度提示视图. 如果在屏幕右边, 则会触发调整声音的操作, 并显示系统音量提示视图
- 水平滑动时, 会触发控制层相应的代理方法

```Objective-C
__weak typeof(self) _self = self;
_gestureControl.panHandler = ^(id<SJPlayerGestureControl>  _Nonnull control, SJPanGestureTriggeredPosition position, SJPanGestureMovingDirection direction, SJPanGestureRecognizerState state, CGPoint translate) {
    __strong typeof(_self) self = _self;
    if ( !self ) return ;
    /// ....
};
```

<h3 id="10.4">10.4 捏合手势</h3>

<p>
当用户做放大或收缩触发该手势时, 会设置播放器显示模式`Aspect`或`AspectFill`.
</p>

```Objective-C
__weak typeof(self) _self = self;
_gestureControl.pinchHandler = ^(id<SJPlayerGestureControl>  _Nonnull control, CGFloat scale) {
    __strong typeof(_self) self = _self;
    if ( !self ) return ;
    self.playbackController.videoGravity = scale > 1 ?AVLayerVideoGravityResizeAspectFill:AVLayerVideoGravityResizeAspect;
};
```

<h3 id="10.5">10.5 禁止某些手势</h3>

<p>
当需要禁止某个手势时, 可以像如下设置:
</p>

```Objective-C
/// 此处为禁止单击和双击手势
_player.disabledGestures = SJPlayerGestureType_SingleTap | SJPlayerGestureType_DoubleTap;  

typedef enum : NSUInteger {
    SJPlayerGestureType_SingleTap,
    SJPlayerGestureType_DoubleTap,
    SJPlayerGestureType_Pan,
    SJPlayerGestureType_Pinch,
} SJPlayerGestureType;
```

<h3 id="10.6">10.6 自定义某个手势的处理</h3>

```Objective-C
/// 例如 替换单击手势的处理
__weak typeof(self) _self = self;
_player.gestureControl.singleTapHandler = ^(id<SJPlayerGestureControl>  _Nonnull control, CGPoint location) {
    __strong typeof(_self) self = _self;
    if ( !self ) return ;
    /// .....你的处理
};
```

<h3 id="10.7">10.7 自己动手撸一个 SJPlayerGestureControl, 替换作者原始实现</h3>

<p>
该部分管理类的协议定义在 SJPlayerGestureControlProtocol 中, 实现该协议的任何对象, 均可赋值给播放器, 替换原始实现.
</p>

___

<h2 id="11">11. 占位图</h2>

<p>
资源在初始化时, 由于暂时没有画面可以呈现, 会出现短暂的黑屏. 在此期间, 建议大家设置一下占位图.
</p>

<h3 id="11.1">11.1 设置本地占位图</h3>

```Objective-C
_player.placeholderImageView.image = [UIImage imageNamed:@"..."];
```

<h3 id="11.2">11.2 设置网络占位图</h3>

```Objective-C
[_player.placeholderImageView sd_setImageWithURL:URL placeholderImage:img];
```

<h3 id="11.3">11.3 是否隐藏占位图 - 播放器准备好显示时</h3>

```Objective-C
/// 播放器准备好显示时, 是否隐藏占位图
/// - 默认为YES
@property (nonatomic) BOOL hiddenPlaceholderImageViewWhenPlayerIsReadyForDisplay;
```

___

<h2 id="12">12. 显示提示文本</h2>

<p>
目前提示文本支持 NSString 以及 NSAttributedString. 
</p>

<h3 id="12.1">12.1 显示文本及持续时间 - (NSString or NSAttributedString)</h3>

```Objective-C
/// duration 如果为 -1, 则会一直显示 
- (void)showTitle:(NSString *)title duration:(NSTimeInterval)duration;

- (void)showTitle:(NSString *)title duration:(NSTimeInterval)duration hiddenExeBlock:(void(^__nullable)(__kindof SJBaseVideoPlayer *player))hiddenExeBlock;

- (void)showAttributedString:(NSAttributedString *)attributedString duration:(NSTimeInterval)duration;

- (void)showAttributedString:(NSAttributedString *)attributedString duration:(NSTimeInterval)duration hiddenExeBlock:(void(^__nullable)(__kindof SJBaseVideoPlayer *player))hiddenExeBlock;

/// 隐藏
- (void)hiddenTitle;
```

<h3 id="12.1">12.2 配置提示文本</h3>

```Objective-C

/// Update
_player.prompt.update(^(SJPromptConfig * _Nonnull config) {
    config.font = [UIFont systemFontOfSize:12];
});

/// 所有属性如下: 
@interface SJPromptConfig : NSObject

/// default is UIEdgeInsetsMake( 8, 8, 8, 8 ).
@property (nonatomic, assign) UIEdgeInsets insets;

/// default is 8.
@property (nonatomic, assign) CGFloat cornerRadius;

/// default is black.
@property (nonatomic, strong) UIColor *backgroundColor;

/// default is systemFont( 14 ).
@property (nonatomic, assign) UIFont *font;

/// default is white.
@property (nonatomic, strong) UIColor *fontColor;

/// default is ( superview.width * 0.6 ).
@property (nonatomic, assign) CGFloat maxWidth;

- (void)reset;

@end
```

___

<h2 id="13">13. 一些固定代码</h2>

<p>
接入播放器的 ViewController 中, 会写一些固定的代码, 我将这些固定代码(例如 进入下个页面时, 需要当前页面的播放器暂停), 都封装在了以下方法中. 

```Objective-C
- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];
    [_player vc_viewDidAppear];
}
```

在适当的时候直接调用即可, 以下为内部实现:
</p>

<h3 id="13.1">13.1 - (void)vc_viewDidAppear; </h3>

<p>
当 ViewController 的 viewDidAppear 调用时, 恢复播放

实现如下:
</p>

```Objective-C
- (void)vc_viewDidAppear {
    if ( !self.isPlayOnScrollView || (self.isPlayOnScrollView && self.isScrollAppeared) ) {
    /// 恢复播放
        [self play];
    }
    
    /// 标识vc已显示 
    /// vc_isDisappeared 是自动旋转触发的条件之一, 如果控制器 disappear 了, 就不会触发旋转 
    self.vc_isDisappeared = NO;
}
```

<h3 id="13.2">13.2 - (void)vc_viewWillDisappear;</h3>

<p>
当 ViewController 的 viewWillDisappear 调用时, 设置标识为YES

实现如下:
</p>

```Objective-C
- (void)vc_viewWillDisappear {
    /// 标识vc已显示 
    /// vc_isDisappeared 是自动旋转触发的条件之一, 如果控制器 disappear 了, 就不会触发旋转 
    self.vc_isDisappeared = YES;
}
```

<h3 id="13.3">13.3 - (void)vc_viewDidDisappear;</h3>

<p>
当 ViewController 的 viewDidDisappear 调用时, 暂停播放

实现如下:
</p>

```Objective-C
- (void)vc_viewDidDisappear {
    [self pause];
}
```

<h3 id="13.4">13.4 - (BOOL)vc_prefersStatusBarHidden;</h3>

<p>
状态栏是否可以隐藏

实现如下: 
</p>

```Objective-C
- (BOOL)vc_prefersStatusBarHidden {
    if ( _tmpShowStatusBar ) return NO;         // 临时显示
    if ( _tmpHiddenStatusBar ) return YES;      // 临时隐藏
    if ( self.lockedScreen ) return YES;        // 锁屏时, 不显示
    if ( self.rotationManager.isTransitioning ) { // 旋转时, 不显示
        if ( !self.disabledControlLayerAppearManager && self.controlLayerIsAppeared ) return NO;
        return YES;
    }
    // 全屏播放时, 使状态栏根据控制层显示或隐藏
    if ( self.isFullScreen ) return !self.controlLayerIsAppeared;
    return NO;
}
```

<h3 id="13.5">13.5 - (UIStatusBarStyle)vc_preferredStatusBarStyle;</h3>

<p>
状态栏显示白色还是黑色

实现如下:
</p>

```Objective-C
- (UIStatusBarStyle)vc_preferredStatusBarStyle {
    // 全屏播放时, 使状态栏变成白色
    if ( self.isFullScreen || self.fitOnScreen ) return UIStatusBarStyleLightContent;
    return UIStatusBarStyleDefault;
}
```

<h3 id="13.6">13.6 - 临时显示状态栏</h3>

<p>
有时候, 可能会希望临时显示状态栏, 例如全屏转回小屏时, 旋转之前, 需要将状态栏显示.
</p>

```Objective-C
[_player needShowStatusBar]; 
```

<h3 id="13.7">13.7 - 临时隐藏状态栏</h3>

<p>
有时候, 可能会希望临时隐藏状态栏, 例如某个播放器控制层不需要显示状态栏.
</p>

```Objective-C
[_player needHiddenStatusBar]; 
```

___

<h2 id="14">14. 截屏</h2>

<h3 id="14.1">14.1 当前时间截图</h3>

```Objective-C
UIImage *img = [_player screenshot];
```

<h3 id="14.1">14.2 指定时间截图</h3>

```Objective-C
- (void)screenshotWithTime:(NSTimeInterval)secs
                completion:(void(^)(__kindof SJBaseVideoPlayer *videoPlayer, UIImage * __nullable image, NSError *__nullable error))block;

/// 可以通过 _player.playbackController.presentationSize 来获取当前视频宽高
- (void)screenshotWithTime:(NSTimeInterval)secs
                      size:(CGSize)size
                completion:(void(^)(__kindof SJBaseVideoPlayer *videoPlayer, UIImage * __nullable image, NSError *__nullable error))block;
```

<h3 id="14.1">14.3 生成预览视图, 大约20张</h3>

```Objective-C
/// 可以通过 _player.playbackController.presentationSize 来获取当前视频宽高
/// itemSize 应该尽可能的小一点, 这样处理的效率会更快
- (void)generatedPreviewImagesWithMaxItemSize:(CGSize)itemSize
                                   completion:(void(^)(__kindof SJBaseVideoPlayer *player, NSArray<id<SJVideoPlayerPreviewInfo>> *__nullable images, NSError *__nullable error))block;
```

<h2 id="15">15. 导出视频或GIF</h2>

<h3 id="15.1">15.1 导出视频</h3>

```Objective-C
/**
 export session.
 
 @param beginTime           开始的位置, 单位是秒
 @param endTime             结束的位置, 单位是秒
 @param presetName            default is `AVAssetExportPresetMediumQuality`.
 @param progressBlock       progressBlock
 @param completion            completion
 @param failure               failure
 */
- (void)exportWithBeginTime:(NSTimeInterval)beginTime
                    endTime:(NSTimeInterval)endTime
                 presetName:(nullable NSString *)presetName
                   progress:(void(^)(__kindof SJBaseVideoPlayer *videoPlayer, float progress))progressBlock
                 completion:(void(^)(__kindof SJBaseVideoPlayer *videoPlayer, NSURL *fileURL, UIImage *thumbnailImage))completion
                    failure:(void(^)(__kindof SJBaseVideoPlayer *videoPlayer, NSError *error))failure;
```

<h3 id="15.2">15.2 导出GIF</h3>

```Objective-C
/**
生成GIF

@param beginTime 开始的位置, 单位是秒
@param duration  时长
@param progressBlock 进度回调
@param completion 完成的回调
@param failure 失败的回调
*/
- (void)generateGIFWithBeginTime:(NSTimeInterval)beginTime
                        duration:(NSTimeInterval)duration
                        progress:(void(^)(__kindof SJBaseVideoPlayer *videoPlayer, float progress))progressBlock
                      completion:(void(^)(__kindof SJBaseVideoPlayer *videoPlayer, UIImage *imageGIF, UIImage *thumbnailImage, NSURL *filePath))completion
                         failure:(void(^)(__kindof SJBaseVideoPlayer *videoPlayer, NSError *error))failure;
```

<h3 id="15.3">15.3 取消操作</h3>

```Objective-C
/// 取消导出操作
/// 播放器 dealloc 时, 会调用一次 
- (void)cancelExportOperation;

/// 取消GIF操作
/// 播放器 dealloc 时, 会调用一次 
- (void)cancelGenerateGIFOperation;
```

<h2 id="16">16. 滚动相关</h2>

<p>
此部分的内容由 SJPlayModelPropertiesObserver 提供支持.
</p>


<h3 id="16.1">16.1 是否在 UICollectionView 或者 UITableView 中播放</h3>

```Objective-C
/// 是否是在 UICollectionView 或者 UITableView 中播放
_player.isPlayOnScrollView
```

<h3 id="16.2">16.2 是否滚动显示</h3>

```Objective-C
/// 是否滚动显示
_player.isScrollAppeared
```

<h3 id="16.3">16.3 播放器视图将要滚动显示和消失的回调</h3>

```Objective-C
@property (nonatomic, copy, nullable) void(^playerViewWillAppearExeBlock)(__kindof SJBaseVideoPlayer *videoPlayer);
@property (nonatomic, copy, nullable) void(^playerViewWillDisappearExeBlock)(__kindof SJBaseVideoPlayer *videoPlayer);
```

<h2 id="17">17. 自动播放 - 在 UICollectionView 或者 UITableView 中</h2>

<p>
目前支持在 UICollectionViewCell 和 UITableViewCell 中自动播放.

使用之前, 请导入头文件 `#import "UIScrollView+ListViewAutoplaySJAdd.h"`
</p>

<h3 id="17.1">17.1 开启</h3>

```Objective-C
/// 配置列表自动播放
[_tableView sj_enableAutoplayWithConfig:[SJPlayerAutoplayConfig configWithPlayerSuperviewTag:101 autoplayDelegate:self]];


/// Delegate method
- (void)sj_playerNeedPlayNewAssetAtIndexPath:(NSIndexPath *)indexPath {

}
```

<h3 id="17.2">17.2 配置</h3>

```Objective-C
typedef NS_ENUM(NSUInteger, SJAutoplayScrollAnimationType) {
    SJAutoplayScrollAnimationTypeNone,
    SJAutoplayScrollAnimationTypeTop,
    SJAutoplayScrollAnimationTypeMiddle,
};

@interface SJPlayerAutoplayConfig : NSObject
+ (instancetype)configWithPlayerSuperviewTag:(NSInteger)playerSuperviewTag
                            autoplayDelegate:(id<SJPlayerAutoplayDelegate>)autoplayDelegate;

/// 滚动的动画类型
/// default is .Middle;
@property (nonatomic) SJAutoplayScrollAnimationType animationType;

@property (nonatomic, readonly) NSInteger playerSuperviewTag;
@property (nonatomic, weak, nullable, readonly) id<SJPlayerAutoplayDelegate> autoplayDelegate;
@end

@protocol SJPlayerAutoplayDelegate <NSObject>
- (void)sj_playerNeedPlayNewAssetAtIndexPath:(NSIndexPath *)indexPath;
@end
```

<h3 id="17.3">17.3 关闭</h3>

```Objective-C
[_tableView sj_disenableAutoplay];
```

<h3 id="17.4">17.4 主动调用播放下一个资源</h3>

```Objective-C
[_tableView sj_needPlayNextAsset];
```

___
