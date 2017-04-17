# wechat-app-LeftSlide
微信小程序仿微信， QQ 向左滑动删除操作。


## 废话不多说， 先看效果， 能满足您的需求， 您在接着往下看。

![](./images/gif.gif)


    本人比较懒， 删除动作有点太生硬， 实在看不惯的同学可以加个动画上去。
   
### OK 下面来看代码

#### 首先是 wxml 里面绑定事件 
    
 ``` html
   <view class="item-wrapper">
    <view class="item-list" wx:for="{{itemData}}" wx:for-item="item" wx:for-index="index" wx:key="that">
        <view class="item-info" data-index="{{index}}" bindtouchstart="touchS" bindtouchmove="touchM" bindtouchend="touchE" style="left:{{item.left + 'rpx'}}">
            <image class="info-img" src="{{item.img}}"></image>
            <view class="info-wrapper">
                <view class="info-desc">
                    <view class="name">{{item.name}}</view>
                    <view class="time">{{item.time}}</view>
                </view>
                <view class="info-content">{{item.info}}</view>
            </view>
        </view>
        <view class="item-oper">
            <view class="oper-delete" bindtap="itemDelete" data-index="{{index}}">删除</view>
        </view>
    </view>
</view>
```

    主要就是用的小程序的 touch 来进行处理

    样式就不说了， 可以自己写， 不想写的同学也可以拿来直接用。
    
#### 然后就是事件的处理函数了

``` javascript
touchS: function (e) {  // touchstart
    let startX = App.Touches.getClientX(e)
    startX && this.setData({ startX })
  },
  touchM: function (e) {  // touchmove
    let itemData = App.Touches.touchM(e, this.data.itemData, this.data.startX)
    itemData && this.setData({ itemData })

  },
  touchE: function (e) {  // touchend
    const width = 150  // 定义操作列表宽度
    let itemData = App.Touches.touchE(e, this.data.itemData, this.data.startX, width)
    itemData && this.setData({ itemData })
  },
  itemDelete: function(e){  // itemDelete
    let itemData = App.Touches.deleteItem(e, this.data.itemData)
    itemData && this.setData({ itemData })
  },
  ```
  
    事件函数主演也是 touch 的三个事件和删除事件， 每个事件的核心是更新数据， 也就是 setData()
