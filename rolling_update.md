# 問題
當使用Rolling Update時，預期更新期間不影響使用者體驗。
<br />
但實際有機率出現 "Server is busy" 錯誤。

# 原因
```js
if (options.serverReady === false) 
  next(new Error('Server is busy'));
```
當新程式啟動時，未等待serverReady，即被加入Load Balancer處理新進入的request。

# 解決方法
尋找方法，使新程式啟動時可以等待 serverReady 後，才加入Load Balancer處理。

# Elastic Beanstalk Deployment
[Deployment Documentation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rolling-version-deploy.html)
> If a health check URL is configured, the load balancer doesn't route traffic to the updated instances until they return a 200 OK status code in response to an HTTP GET request to the health check URL.

# 程式修改
```js
const { Timer } = require('nodejs-timer');

const readyCheckTimer = new Timer(() => {
  if (!options.serverReady) {
    readyCheckTimer.start(1000);
    return;
  }
  server.listen(process.env.PORT);
});

readyCheckTimer.start(1000);
```
每秒巡一次是否 serverReady，直到serverReady才listen。