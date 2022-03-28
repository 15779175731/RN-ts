# 图灵盾web

为您的web 移动应用提供了web sdk , 开始使用有以下步骤

## 1.服务流程图

![avatar](https://tianyu-app-sdk-1258773362.cos.ap-beijing.myqcloud.com/image/web/flowchart.png)

### 1.1 术语解释

- **deviceToken**

      运⽤AI技术⽣成的设备openid

- 设备**OpenId**票据（**88**个字符）
    
    调⽤前端SDK接⼝获取的设备OpenId的票据，短期有效，不 能⻓期缓存使⽤，不能作为设备唯⼀标识。仅⽀持后台对接换取，不返回到前端。
    

## 2.集成SDK

### 2.1 嵌入SDK

点击以下链接下载文件解压并导入您的项目中

[https://tianyu-app-sdk-1258773362.cos.ap-beijing.myqcloud.com/TDID-web.zip](https://tianyu-app-sdk-1258773362.cos.ap-beijing.myqcloud.com/TDID-web.zip)

<aside>
💡 在⻚⾯中嵌⼊TDID的JS SDK（建议CDN⽅式引⼊）

</aside>

```java
  <script type="text/javascript" src="./libs/TDID-1.0.14-WCM6E4A10JBRIDXB_build_all_20220225162528.js"></script>
```

### 2.2 初始化sdk

<aside>
💡 先通过JS SDK init接⼝，进⾏SDK初始化。初始化前需获取到 channel,若需要私有化版本，获取 hostUrl，获取标识或服务器部署名都需要联系SDK提供人员

</aside>

```java
_TDID.init({
      channel: 100001,//接入产品标识，找SDK提供人员获取，定制版本不需要传入
      apiUrl: "https://tdid.m",//仅当私有化版本需要传入服务器部署域名
    }, null);
```

### 2.3 获取设备结果

<aside>
💡 通过getRiskDetect接⼝，检测当前设备浏览器环境⻛险，通过callback回调⻛险及错误信息。

</aside>

```java
_TDID.getDeviceToken(
      function (res) {
        if (res.ret == 0) {
         console.log(res.deviceToken);
         console.log("deviceToken 获取成功");
        } else {
          console.log("deviceToken 获取失败");
        }
      }
);
```

### 2.4 常见错误提示

<aside>
💡 因localhost没有配置跨域,若获取设备结果localhost本地地址出现跨域，需在浏览器将localhost改成 127.0.0.1

</aside>

![avatar](https://tianyu-app-sdk-1258773362.cos.ap-beijing.myqcloud.com/image/web/1.png)

## 3.调用接口

<aside>
💡 调用接口需要使用腾讯云api 3.0,有相关疑问可点击此链接查看[https://cloud.tencent.com/document/product/1278/46713](https://cloud.tencent.com/document/product/1278/46714)，使用前请您需要获取到您腾讯云账号的 APP_ID， SECRET_ID，SECRET_KEY

</aside>

- 首先，点击以下链接下载并解压文件，将文件中的两个js文件导入到您的项目内

[https://tianyu-app-sdk-1258773362.cos.ap-beijing.myqcloud.com/tencent-web.zip](https://tianyu-app-sdk-1258773362.cos.ap-beijing.myqcloud.com/tencent-web.zip)

- 使用 npm、包管理器和依赖项管理系统将以下 JavaScript 包集成到您的项目中

```jsx
npm install axios base-64 buffer crypto-js 
```

### 3.1 调用参数

- 公共参数

| 参数名称 | 说明 | 类型 | 参数值 |
| --- | --- | --- | --- |
| Action | 方法名 | String | DescribeRiskAssessment   |
| SecretId | 密钥 ID | String | AKIDz8krbsJ5**********mLPx3EXAMPL |
| Timestamp | 当前时间戳 | String | 1465185768 |
| Nonce | 随机正整数 | String | 11886 |
| Region | 实例所在区域 | String | ap-singapore |
| Version | 接口版本号 | String | 2020-11-03 |
| Signature | 请求签名,用来验证此次请求的合法性， | String | 需要用户根据实际的输入参数计算得出 |
- 业务参数: BizCryptoData

| 参数名 | 类型 | 参数值 |
| --- | --- | --- |
| BizCryptoData.IsAuthorized | String | 1 |
| BizCryptoData.CryptoType | String | 1 |
| BizCryptoData.CryptoContent | String | BizData  |

BizData 

| 参数名 | 类型 | 参数值 | 必须 | 说明 |
| --- | --- | --- | --- | --- |
| SceneCode | String  | gb_shield_01 | 是 |            / |
| PostTime | Int | 1465185768 | 是 | 当前时间戳 |
| AccountType | String  | 2 | 是 | 账户类型 |
| UserIp | String  | 10.XXX.XXX.25 | 否 | 用户IP |
| UserPhone | String  | 157XXXX5731 | 否 | 用户电话号码 |
| Mode | String  | 2 | 是 | 图灵盾集成模式 |
| PlatformName | String  | web | 是 | 平台名称(app/web) |
| OSName | String  | H5 | 是 | 系统类型(Android/iOS/H5) |
| DeviceToken | String  | ⻛险deviceToken | 否 | ⻛险deviceToken |

### 3.2 api 3.0签名示例

<aside>
💡 请注意以下需要导入AesEncryption，getSign内部方法

</aside>

```jsx
async function getData(deviceToken) {
    const Appid = "130****050";
    const SECRET_ID = "AKIDz8krbsJ5**********mLPx3EXAMPL";
    const SECRET_KEY = "Gu5t9xGAR***********EXAMPLE";
    const baseURL = "https://rce.tencentcloudapi.com";
    const BizData  = {
      "SceneCode": "gb_shield_01",
      "PostTime": Math.round(new Date().getTime() / 1000),
      "AccountType": "2",
      "UserIp": "",
      "UserPhone": "",
      "Mode": "2",
      "PlatformName": "web",
      "OSName": "H5",
      "DeviceToken": deviceToken,
    }
    let encryptionData = AesEncryption(Appid, JSON.stringify(BizData));
    let data = getSign(encryptionData,SECRET_ID,SECRET_KEY);

    await axios.post(baseURL, data).then((res) => {
      console.log(res.data.Response);
    }).catch((error) => {
      console.log(error);
    });
  }
```

调用成功示例

```jsx
{
    "Data": {
        "UUid": "08c1f381-b0d5-42ba-9ad0-42a744f7e5b6",
        "Code": 0,
        "Message": "OK",
        "Value": {
            "RiskLevel": "pass",
            "RiskType": null,
            "DeviceId": "C30FB440E085260448C07474",
            "ExtraInfo": [
                {
                    "Key": "displayHeight",
                    "Value": "900"
                },
                {
                    "Key": "screenWidth",
                    "Value": "1440"
                },
                {
                    "Key": "cookieDisabled",
                    "Value": "false"
                },
                {
                    "Key": "model",
                    "Value": ""
                },
                {
                    "Key": "packageName",
                    "Value": ""
                },
                {
                    "Key": "platform",
                    "Value": "4"
                },
                {
                    "Key": "sdkBuildno",
                    "Value": "172"
                },
                {
                    "Key": "brand",
                    "Value": ""
                },
                {
                    "Key": "clientIP",
                    "Value": "43.132.98.39"
                },
                {
                    "Key": "historyLength",
                    "Value": "2"
                },
                {
                    "Key": "system",
                    "Value": "MacIntel"
                },
                {
                    "Key": "appVersion",
                    "Value": ""
                },
                {
                    "Key": "displayWidth",
                    "Value": "1440"
                },
                {
                    "Key": "networkType",
                    "Value": "4g"
                },
                {
                    "Key": "screenHeight",
                    "Value": "900"
                }
            ]
        }
    },
    "RequestId": "bed13938-dbf7-4908-983c-c3b56f57b6b6"
}
```

<aside>
💡 如果返回结果中存在 Error 字段，则表示调用 API 接口失败,如有错误码，请店家查看以下链接:[https://cloud.tencent.com/document/product/1278/55263](https://cloud.tencent.com/document/product/1278/55263)     API调用失败例如：

</aside>

```javascript
{
    "Code": "AuthFailure.SignatureFailure",
    "Message": "The provided credentials could not be validated. Please check your signature is correct."
}
```
