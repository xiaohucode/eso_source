# 亦搜IOS版

安装包获取:https://github.com/xiaohucode/eso_source/releases

规则导入链接:https://raw.githubusercontent.com/xiaohucode/eso_source/main/%E6%95%B4%E5%90%88%E8%A7%84%E5%88%99/EsoSource.es

# eso_source
app作者开源地址:https://github.com/mabDc/eso

### app规则内置CryptoJS

### esoTools函数

- 下列是异步函数,需要使用 await 或 then 获取返回值

    - esoTools.encode(type, body) //编码数据 type可以是'base64' 'gbk' 'utf8' 'md5'

    - esoTools.decode(type, body) //解码数据 type可以是'base64' 'gbk' 'utf8'

    - esoTools.AES_Encode (string, inkey, opt) //返回字典{'base16':x,'base64':x,'bytes':x}

    - esoTools.AES_Decode (string, inkey, opt) //string需提供base64格式数据 返回解密文本数据

    - esoTools.AES_EncodeCBC (string, inkey, iniv) // 返回值同AES_Encode,默认为PKCS7模式

    - esoTools.AES_DecodeCBC (string, inkey, iniv) // 同AES_Decode,默认为PKCS7模式

    - esoTools.AES_EncodeECB (string, inkey) //ECB模式加密返回值同AES_Encode

    - esoTools.AES_DecodeECB (string, inkey) //ECB模式解密

- 下列函数不必使用await 或 then

    - esoTools.RSA_encrypt (string, key) //RSA公钥加密

    - esoTools.RSA_decrypt (string, key) //RSA私钥解密

    - esoTools.RSA_encryptWithPrivate (string, key) //RSA私钥加密

    - esoTools.RSA_decryptWithPublic (string, key) //RSA公钥解密

    - esoTools.md5Encode (str)

    - esoTools.base64Encode (str)

    - esoTools.base64Decode (str)

    - esoTools.sha1Encode (str)

    - esoTools.sha224Encode (str)

    - esoTools.sha256Encode (str)

    - esoTools.sha348Encode (str)

    - esoTools.sha512Encode (str)

    - esoTools.ripemd160Encode (str)


esoTools.AES例:
```
// enum AESMode {
//     cbc,
//     cfb64,
//     ctr,
//     ecb,
//     ofb64Gctr,
//     ofb64,
//     sic,
// };
let encodeStr = esoTools.AES_Encode("你好", "x;j/6olSp})&{ZJD", {
    mode: AESMode.cbc,
    padding: 'PKCS7',
    iv: "znbV%$JN5olCpt<c"
});
print(encodeStr);
print(encodeStr.base64);
print(encodeStr.base16);
print(encodeStr.bytes);
let res = esoTools.AES_Decode(encodeStr.base64, "x;j/6olSp})&{ZJD", {
    mode: AESMode.cbc,
    padding: 'PKCS7',
    iv: "znbV%$JN5olCpt<c"
});
print(res);
```
### params规则
在设置moreKeys(更多键)后有效

params.pageIndex    等价于page 当前页码

params.tabIndex     分组索引

params.filters      筛选器



### webview参数
```
/*
新的webview参数:
webview仅支持GET,POST请求,不支持PUT其他请求

webview：非空表示开启webview请求完整网页
sourceRegex：匹配正则表达式,嗅探资源url
webviewJs：webview完成后执行的js,结果就是响应数据
webviewJsDelay：webview完成后延时执行js,默认1秒

*/
let resp = httpByte({url: "https://www.baidu.com/", webview: true,sourceRegex:'(?:\.m3u8|\.mp4|api\/source)'});
print(resp.body);
/* 
## webview目前问题：
- Android平台：
    - 请求响应后没有添加响应头
- ios\macos平台:
    - 无
- Windows平台
    - 暂时不支持,等我修改webview插件
*/
```


### moreKeys(更多键)
用于一些筛选配置,必须提供JSON数据 格式为:object{isWrap<bool>,list[object{title,requestFilters[object{key, item[object{title,value}]}]}]}
```
object{
    isWrap,                         // bool型 筛选器是否为瀑布流布局,默认为true,false为横向滑动布局
    list[                           // list数组 多类型分组数组
        object{                     // 分组{n}
            title,                  // 分组标题名
            requestFilters[         // 筛选器数组,
                object{             // 筛选器{n}
                    key,            // key标识 用于在js里读取value  如此处key为'rank' js获取应为params.filters.rank
                    items[          // 项目数组 
                        object{     // 项目{n}
                            title,  // 筛选标签名字
                            value   // 数值
                        }
                    ] 
                }
            ]
        }
    ]
}
```
```
把这个JSON数据复制到JSON查看器里观察
{"list":[{"title":"电视剧","requestFilters":[{"items":[{"title":"全部","value":"/tv/index.html"},{"title":"国产剧","value":"/guochan/index.html"},{"title":"港台剧","value":"/tangtai/index.html"},{"title":"欧美剧","value":"/oumei/index.html"},{"title":"日韩剧","value":"/rihan/index.html"}],"key":"type","value":"/tv/index.html"}]},{"title":"电影","requestFilters":[{"items":[{"title":"全部","value":"/dy/index.html"},{"title":"动作片","value":"/dongzuo/index.html"},{"title":"爱情片","value":"/aiqing/index.html"},{"title":"科幻片","value":"/kehuan/index.html"},{"title":"恐怖片","value":"/kongbu/index.html"},{"title":"喜剧片","value":"/xiju/index.html"},{"title":"剧情片","value":"/juqing/index.html"},{"title":"在线直播","value":"/zaixianzhibo/index.html"}],"key":"type","value":"/dy/index.html"}]},{"title":"综艺","requestFilters":[{"items":[{"title":"全部","value":"/zy/index.html"}],"key":"type","value":"/zy/index.html"}]},{"title":"动漫","requestFilters":[{"items":[{"title":"全部","value":"/dm/index.html"}],"key":"type","value":"/dm/index.html"}]}]}
```

