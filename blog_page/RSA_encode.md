##使用hutool工具进行RSA加密

```java
package com.feparks.temp;

import cn.hutool.core.codec.Base64;
import cn.hutool.crypto.asymmetric.AsymmetricCrypto;
import cn.hutool.crypto.asymmetric.KeyType;
import sun.misc.BASE64Decoder;

import java.security.KeyFactory;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;

public class Temp {
// 主类
    public Temp() throws Exception{
        AsymmetricCrypto asymmetricCrypto = null;
        asymmetricCrypto = new AsymmetricCrypto("RSA", getRSAPrivateKeyBybase64("私钥解密"),
                getRSAPublicKeyBybase64("公钥加密"));

        String s1 = Base64.encode(asymmetricCrypto.encrypt("{\"code\":\"FE-SP_0001\",\"expire\":\"20221231\",\"name\":\"广东珠海南方软件园\",\"device\":[{\"value\":\"\",\"key\":\"mac\"}]}", KeyType.PrivateKey));
        System.out.println(s1);
        String temp =asymmetricCrypto.getPublicKeyBase64();//用String显示公钥

        String s = new String(asymmetricCrypto.decrypt(Base64.decode(s1), KeyType.PublicKey));
        System.out.println(s);
    }

// 私钥
    public static RSAPrivateKey getRSAPrivateKeyBybase64(String base64s) throws Exception {
        PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec((new BASE64Decoder()).decodeBuffer(base64s));
        RSAPrivateKey Key = null;
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        try {
            Key = (RSAPrivateKey) keyFactory.generatePrivate(keySpec);
        } catch (InvalidKeySpecException var4) {
        }
        return Key;
    }
// 公钥
    public static RSAPublicKey getRSAPublicKeyBybase64(String base64s) throws Exception {
        X509EncodedKeySpec keySpec = new X509EncodedKeySpec((new BASE64Decoder()).decodeBuffer(base64s));
        RSAPublicKey Key = null;
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        try {
            Key = (RSAPublicKey) keyFactory.generatePublic(keySpec);
        } catch (InvalidKeySpecException var4) {
        }
        return Key;
    }
}

```

