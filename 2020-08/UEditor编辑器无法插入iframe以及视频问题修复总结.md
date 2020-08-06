UEditor 是百度出品，最近一次更新发布时间在2016-05-18，距今已经有4年多的历史，总结下目前遇到的问题和解决方案



本次修改涉及相关文件：
```
ueditor/ueditor.all.js
ueditor/ueditor.config.js
ueditor/dialogs/video/video.css
ueditor/dialogs/video/video.js
ueditor/themes/default/css/ueditor.css
```

1. 无法插入iframe。白名单配置问题
2. 上传视频预览提示 **Cross-origin plugin content from must have a visible size larger than 400 x 300 pixels** 。视频可视区域过小，需要调整容器大小，CSS和JS都需要调整
3. 视频预览窗口显示 **输入的视频地址有误，请检查后再试！** ，使用了老旧的flash播放器，需要修改源码，详情见参考链接

参考链接：
1. https://www.cnblogs.com/beyonds/p/8988207.html
2. https://blog.csdn.net/qq_33769914/article/details/96473884
