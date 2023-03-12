# DSA
DSA（Digital Signature Algorithm）是Schnorr和ElGamal签名算法的变种，被美国NIST作为DSS(DigitalSignature Standard)。 
DSA加密算法主要依赖于整数有限域离散对数难题，素数P必须足够大，且p-1至少包含一个大素数因子以抵抗Pohlig &Hellman算法的攻击。M一般都应采用信息的HASH值。DSA加密算法的安全性主要依赖于p和g，若选取不当则签名容易伪造，应保证g对于p-1的大素数因子不可约。其安全性与RSA相比差不多。 

DSA 一般用于数字签名和认证。在DSA数字签名和认证中，发送者使用自己的私钥对文件或消息进行签名，接受者收到消息后使用发送者的公钥来验证签名的真实性。DSA只是一种算法，和RSA不同之处在于它不能用作加密和解密，也不能进行密钥交换，只用于签名,它比RSA要快很多. 

DSA算法中应用了下述参数： 
    p：L bits长的素数。L是64的倍数，范围是512到1024； 
    q：p – 1的160bits的素因子； 
    g：g = h^((p-1)/q) mod p，h满足h < p – 1, h^((p-1)/q) mod p > 1； 
    x：x < q，x为私钥 ； 
    y：y = g^x mod p ，( p, q, g, y )为公钥； 
    H( x )：One-Way Hash函数。DSS中选用SHA( Secure Hash Algorithm )。 
    p, q, g可由一组用户共享，但在实际应用中，使用公共模数可能会带来一定的威胁。 

签名及验证协议： 
1. P产生随机数k，k < q； 
2. P计算 r = ( g^k mod p ) mod q 
    s = ( k^(-1) (H(m) xr)) mod q 
    签名结果是( m, r, s )。 
3. 验证时计算 w = s^(-1)mod q 
    u1 = ( H( m ) w ) mod q 
    u2 = ( r w ) mod q 
    v = (( g^u1 * y^u2 ) mod p ) mod q 
    若v = r，则认为签名有效。 

## golang实现DSA签名及验证 
```go
package main 
 
import ( 
    "crypto/dsa" 
    "crypto/rand" 
    "fmt" 
 
) 
//作用1 确保传递数据的完整性 2 确保数据的来源 
func main()  { 
    //DSA专业做签名和验签 
    var param dsa.Parameters//结构体里有三个很大很大的数bigInt 
    //结构体实例化 
    dsa.GenerateParameters(&param,rand.Reader,dsa.L1024N160)//L是1024，N是160，这里的L是私钥，N是公钥初始参数 
    //通过上边参数生成param结构体，里面有三个很大很大的数 
 
    //生成私钥 
    var priv dsa.PrivateKey//privatekey是个结构体，里面有publickey结构体，该结构体里有Parameters字段 
    priv.Parameters=param 
    //通过随机读数与param一些关系生成私钥 
    dsa.GenerateKey(&priv,rand.Reader) 
 
    //通过私钥生成公钥 
    pub:=priv.PublicKey 
    message:=[]byte("hello world") 
    //r,s是两个整数,通过私钥给message签名,得到两个随机整数r，s 
    r,s,_:=dsa.Sign(rand.Reader,&priv,message) 
 
    //利用公钥验签，验证r，s 
    b:= dsa.Verify(&pub,message,r,s) 
    if b==true{ 
        fmt.Println("验签成功") 
    }else { 
        fmt.Println("验证失败") 
    } 
} 
 
```
