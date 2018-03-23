# Cocos Creator 的一些使用技巧

---

1.开启抗锯齿，关闭抗锯齿传入 false 即可
```$xslt
cc.view.enableAntiAlias(true);
```

2.限定游戏每秒帧数，cocos 默认是 60 帧，现在设置为 24 帧
```$xslt
cc.game.setFrameRate(24);
```

3.隐藏 cocos 调试模式下左下角的 FPS 性能显示面板
```$xslt
cc.director.setDisplayStats(false);
```

4.将数据存在 LocalStorage，保存一些游戏记录
```$xslt
// 存数据
saveData: function (key, val) {
            if ((typeof val) !== "string") {
                val = JSON.stringify(val);
            }
            try {
                cc.sys.localStorage.setItem(key, val);
            } catch (e) {
                cc.error("set local storage item error, key: " + key + ", val: " + val + ", e: " + e);
            }
        },

// 获取数据
getData: function (key) {
            try {
                return cc.sys.localStorage.getItem(key);
            } catch (e) {
                cc.error("get local storage item error, key: " + key + ", e: " + e);
                return undefined;
            }
        },
```

5.获取一个节点的方法有两种，一种是通过父节点去找对应的子节点，另一种是通过该节点在节点树的路径去找到这个节点
```$xslt
 // 得到一个节点
this.playerNode = this.node.getChildByName('player');
this.playerNode = cc.find('Canvas/player')
```

6.获取当前系统时间
```$xslt
getNowFormatDate: function () {
                let date = new Date();
                let year = date.getFullYear();
                let month = date.getMonth() + 1;
                let day = date.getDate();
                let hour = date.getHours();
                let minute = date.getMinutes();
                let second = date.getSeconds();
                let currentdate = (year + '年' + month + '月' + day + '日 ' + hour + ':' + minute + ':' + second);
                return currentdate;
            }
```

7.时间戳格式化，时间戳的定义是从格林威治时间1970年01月01日00时00分00秒起至现在的总秒数
```$xslt
// 获取当前时间戳
let timestamp = Math.round(new Date().getTime()/1000);

// 将时间戳转化为时间字符串
let timestampToTime = function (timestamp) {
            let date;

            //时间戳为10位需*1000，时间戳为13位的话不需乘1000
            if (10 === (timestamp + "").length) {
                date = new Date(timestamp * 1000);
            } else if (13 === (timestamp + "").length) {
                date = new Date(timestamp);
            } else {
                console.log("error timestamp length", timestamp);
                return;
            }

            let Y = date.getFullYear() + '-';
            let M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
            let D = date.getDate() + ' ';
            let h = date.getHours() + ':';
            let m = date.getMinutes() + ':';
            let s = date.getSeconds();
            return Y + M + D + h + m + s;
        }
```
