# eso_source
亦搜源
### esoTools函数
esoTools函数:
esoTools.encode(type, body) //编码数据 type可以是'base64' 'gbk' 'utf8' 'md5'

esoTools.decode(type, body) //解码数据 type可以是'base64' 'gbk' 'utf8'

esoTools.AES_Encode (string, inkey, opt) //返回字典{'base16':x,'base64':x,'bytes':x}

esoTools.AES_Decode (string, inkey, opt) //string需提供base64格式数据 返回解密文本数据

esoTools.AES_EncodeCBC (string, inkey, iniv) // 返回值同AES_Encode,默认为PKCS7模式

esoTools.AES_DecodeCBC (string, inkey, iniv) // 同AES_Decode,默认为PKCS7模式

esoTools.AES_EncodeECB (string, inkey) //ECB模式加密返回值同AES_Encode

esoTools.AES_DecodeECB (string, inkey) //ECB模式解密

esoTools.RSA_encrypt (string, key) //RSA公钥加密

esoTools.RSA_decrypt (string, key) //RSA私钥解密

esoTools.RSA_encryptWithPrivate (string, key) //RSA私钥加密

esoTools.RSA_decryptWithPublic (string, key) //RSA公钥解密

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




