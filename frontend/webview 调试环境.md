## webview 调试环境


###方式一：真机 + Chrome inspect 

Chrome inspect ：Chrome 版本必须高于 32，其次你的测试机 Android 系统高于 4.4

1.  先用数据线将 Android 测试机连接到电脑上。需要打开测试机上面“开发者选项”中的 “USB 调试”功能。 
2. 在PC的Chrome上打开`Chrome://inspect`即可找到你的设备
3. 手机进入一个webview页面，即可在Chrome上看到调试台了


###方式二：真机 + weinre

在你本地创建一个监听服务器，并提供一个JS脚本，需要在需要测试的页面中加载这段 JS，就可以被 Weinre 监听到，在 Inspect 面板中调试你这个页面。

1. 安装 weinre，`npm install -g weinre`，缺少node环境，需要先安装node。
2. 获取本机IP地址（ipconfig或者ifconfig），假设获取到的IP地址为 `4.4.4.4`。
3. 开启 weinre，`weinre --boundHost 4.4.4.4`，其中IP地址为上一步所获取的地址。
4. 在PC或MAC上用浏览器打开 http://4.4.4.4:8080/client/#anonymous ，其中IP地址为第三步所获取的地址。
5. 在需要调试的页面code 文件底部加入 `<script src="http://4.4.4.4:8080/target/target-script-min.js#anonymous"></script>`，上线之前要记得移除这个js的引用。
6. 将移动设备连接到与PC或MAC同一局域网，打开移动设备上的需要调试的浏览器，打开想要调试的页面开始调试。

以上两种方式是常见两种调试方式，基本可覆盖大部分的浏览器，无论是微信中的浏览器还是手机上的UC，都可方便调试。除此之外还有多种调试方式，比如：调试Android上的UC、调试iOS上的Safari等等。


参考资料：[https://github.com/jieyou/remote_inspect_web_on_real_device](https://github.com/jieyou/remote_inspect_web_on_real_device)