uni.getLocation用于获取当前用户的地理位置、速度。当用户离开小程序后，此接口无法调用。开启高精度定位，接口耗时会增加，可指定 highAccuracyExpireTime 作为超时时间。地图相关使用的坐标格式应为 gcj02。下面简单介绍怎样在uni-app项目中使用uni.getLocation;
# 配置manifest.json
  因为getLocation的使用有点特殊，必须要在调用前需要用户授权（scope.userLocation），因此我们在manifest.json里面找到"mp-weixin"且在"mp-weixin":{}进行配置，配置完成后我们就可以愉快的在真机上使用uni.getLocation了；
```
  "permission": {
			"scope.userLocation": {
				"desc": "你的位置信息将用于小程序位置接口的效果展示" 
			}
		},
```
**desc：用于描述获取你的地理位置的原由**
![An image](./uni_map_example1.jpeg)


# 举个页面中使用的🌰：
```
uni.getLocation({
   type: 'gcj02',  // 默认值为wgs84；可选值（ 1.wgs84 返回 gps 坐标，2.gcj02 返回可用于 wx.openLocation 的坐标）
    wgs84: false, // 传入 true 会返回高度信息，由于获取高度需要较高精确度，会减慢接口返回速度
    sHighAccuracy: false, // 开启高精度定位
    highAccuracyExpireTime: 3000, //高精度定位超时时间(ms)，指定时间内返回最高精度，该值3000ms以上高精度定位才有效果
    success: function (res) {
      console.log('成功获取位置信息',res)
    },
    fail: function (error) {
        console.log('获取当前位置失败',error)
    },
    complete: function(com){
      console.log('接口调用结束的回调函数（调用成功、失败都会执行）',com)     
   }
 });
```
个人建议把获取当前定位的API使用new Promise封装起来使用，避免代码多次重复，(或者各位大神有更好的方法，麻烦评论一下，给大家参考一下，让我们在敲码的路上潇潇洒洒👀)例如：
```
function getLocationFun() {
  return new Promise((resolve, reject) => {
    uni.getLocation({
      type: 'gcj02',
      success: function (res) {
        resolve(res)
      },
      fail: function (error) {
        console.log('执行',error)
        reject(error)
      }
    });
  })

}
```
# 附上获取当前位置成功后返回的参数
![An image](./uni_map_example2.png)

以免因小弟浅薄的理解误导大家，最后附上uni-app关于获取当前位置的文档，具体的大家也可以去官网文档了解：https://uniapp.dcloud.io/api/location/location?id=getlocation