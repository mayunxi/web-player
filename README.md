### 网页播放器总结
1. srs的flv不能播放，但 illuspas/node-media-server可以播放，同一台机器上1080p 3秒延时
2. [在线srs-player可以播放flv](http://ossrs.net/players/srs_player.html?app=live&stream=livestream.flv&server=r.ossrs.net&port=8080&autostart=true&vhost=r.ossrs.net&schema=http)
3. 问题

浏览器报错：
```
TransmuxingController] > DemuxException: type = CodecUnsupported, info = Flv: Unsupported audio codec idx: 7
```
可用的解决办法(用ffmpeg推流):
1. 在推流时加上 -an 参数,关掉音频流.
1. 在推流时加上 -acodec aac参数,用aac对音频流进行编码
### nginx配置
/etc/nginx/conf.d/80.conf
```
server{
    listen  80;    # 指定端口
    #server_name  localhost 192.168.200.175;   #指定域名
    server_name mayunxi.com;
    location / {
        root /home/mayunxi/myworkspace/github/web-player;  # 指定静态网站跟目录
        index http-flv-player.html;  # 指定默认访问文件
    }
}

```

modifyflv.js修改历史
1. 增加sei中提取私有信息功能
if (unitType == 6)
2. 解决不能播放问题，将return this._audioInitialMetadataDispatched && this._videoInitialMetadataDispatched;改为return this._audioInitialMetadataDispatched || this._videoInitialMetadataDispatched;
音视频只要有一个获取到信息就发送显示