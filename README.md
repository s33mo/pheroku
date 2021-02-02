> Reminder: Abuse may lead to account being BAN! ! !


  [Original script](https://github.com/mixool/kuhero), on the basis of the original author, the website has added a jump function, which can modify the jump page code to achieve the purpose of jumping to any website.
  
  Currently trojan uses ws, I use v2rayNG to test, because v2rayNG uses SNI, try to modify the program file-to be resolved
  
* Use v2ray+caddy to simultaneously deploy protocols such as vmess vless trojan shadowsocks socks transmitted via ws
* Support tor network, and you can start v2ray and caddy through a custom network configuration file to configure various functions on demand
* Supports storage of custom files, directories and account passwords are all AUUID, the client must use TLS to connect
  
[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/shyperwang/proxy_heroku)
  
### Server
Click on the purple `Deploy to Heroku` above, it will jump to the heroku app creation page, fill in the name of the app, select the node, modify some parameters and AUUID as needed, and click deploy below to create the app to start deployment
If an error occurs, you can try a few more times. After the deployment is complete, the bottom of the page will display Your app was successfully deployed
  * Click Manage App to view and reset the parameters in the Config Vars item under Settings**
  * Click Open app to jump to the [Welcome Page] (/etc/CADDYIndexPage.md) domain name to assign the domain name to heroku, the format is `appname.herokuapp.com`, which is used on the client side
  * The default protocol password is $UUID, and the WS path is in the format of $UUID-[vmess|vless|trojan|ss|socks]
  
### Client
* **Be sure to replace all the project domain names assigned to heroku by appname.herokuapp.com**
* **Be sure to replace all 8f91b6a0-e8ee-11ea-adc1-0242ac120002 with the AUUID set during deployment**
  
<details>
<summary>v2ray</summary>

```bash
* Client download: https://github.com/v2fly/v2ray-core/releases
* Agency agreement: vless or vmess
* Address: appname.herokuapp.com
* Port: 443
* Default UUID: 8f91b6a0-e8ee-11ea-adc1-0242ac120002
* Encryption: none
* Transmission protocol: ws
* Camouflage type: none
* Path:/8f91b6a0-e8ee-11ea-adc1-0242ac120002-vless // default vless use/$uuid-vless, vmess use/$uuid-vmess
* Underlying transmission security: tls
```
</details>
  
<details>
<summary>trojan-go</summary>

```bash
* Client download: https://github.com/p4gefau1t/trojan-go/releases
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "appname.herokuapp.com",
    "remote_port": 443,
    "password": [
        "8f91b6a0-e8ee-11ea-adc1-0242ac120002"
    ],
    "websocket": {
        "enabled": true,
        "path": "/8f91b6a0-e8ee-11ea-adc1-0242ac120002-trojan",
        "host": "appname.herokuapp.com"
    }
}
```
</details>
  
<details>
<summary>shadowsocks</summary>

```bash
* Client download: https://github.com/shadowsocks/shadowsocks-windows/releases/
* Server address: appname.herokuapp.com
* Port: 443
* Password: password
* Encryption: chacha20-ietf-poly1305
* Plug-in program: v2ray-plugin_windows_amd64.exe //You need to download and unzip the plugin https://github.com/shadowsocks/v2ray-plugin/releases and place it in the same directory as shadowsocks
* Plug-in options: tls;host=appname.herokuapp.com;path=/8f91b6a0-e8ee-11ea-adc1-0242ac120002-ss
```
</details>
  
<details>
<summary>cloudflare workers example</summary>

```js
const SingleDay ='appname.herokuapp.com'
const DoubleDay ='appname.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>cloudflare workers example2</summary>
  
  ```js
  addEventListener(
"fetch",event => {
let url=new URL(event.request.url);
url.hostname="xx.xxxx.xx";//Your heroku domain name
let request=new Request(url,event.request);
event. respondWith(
fetch(request)
)
}
)
  ```
</details>
> [More tutorials from enthusiastic netizens PR](/tutorial)
