# S-AES
这是一个S-AES算法实现
## **开发说明**
根据"信息安全导论"课程第8-9次课讲述的AES算法：

* 提供GUI解密支持用户交互
* 对相同的密文进行交叉测试
* 把ASII编码字符串作为加密算法的数据进行测试
* 将S-AES算法通过双重加密进行扩展
* 使用相同密钥的明、密文对，使用中间相遇攻击的方法找到正确的密钥Key
* 将S-AES算法通过三重加密进行扩展
* 基于S-AES算法，使用密码分组链(CBC)模式对较长的明文消息进行加密

## **开发团队**
* 开发者队名：吴彦组
* 开发者姓名：郑铭杰、余宏城
* 联系信息：1072200141@qq.com

## **开发环境**
* 操作系统：Windows11
* 版本控制：Github
* 语言&框架：HTML5，CSS3，JavaScript
* Web浏览器：Microsoft Edge

## **运行程序**
```bash
s_aes.html
```

## **功能测试与运行界面**

* **基本界面**

![基本界面](https://github.com/TeFur0/S-AES/blob/main/png/基本界面.png?raw=true)

* **基础加密**

根据S-AES算法编写和调试程序，提供GUI解密支持用户交互。输入可以是16bit的数据和16bit的密钥，输出是16bit的密文。

![基础加密](https://github.com/TeFur0/S-AES/blob/main/png/基础加密.png?raw=true)

* **交叉测试**

考虑到是"算法标准"，所有人在编写程序的时候需要使用相同算法流程和转换单元(替换盒、列混淆矩阵等)，以保证算法和程序在异构的系统或平台上都可以正常运行。
设有A和B两组位同学(选择相同的密钥K)；则A、B组同学编写的程序对明文P进行加密得到相同的密文C；或者B组同学接收到A组程序加密的密文C，使用B组程序进行解密可得到与A相同的P。

我组：

![我组](https://github.com/TeFur0/S-AES/blob/main/png/我组.png?raw=true)


他组：

![他组](https://github.com/TeFur0/S-AES/blob/main/png/他组.png?raw=true)

* **ASCII码加密**

考虑到向实用性扩展，加密算法的数据输入可以是ASII编码字符串(分组为2 Bytes)，对应地输出也可以是ACII字符串(很可能是乱码)。

![ASCII加密](https://github.com/TeFur0/S-AES/blob/main/png/ASCII加密.png?raw=true)

* **双重加密**

将S-AES算法通过双重加密进行扩展，分组长度仍然是16 bits，但密钥长度为32 bits。

![双重加密](https://github.com/TeFur0/S-AES/blob/main/png/双重加密.png?raw=true)

* **中间相遇攻击**

假设你找到了使用相同密钥的明、密文对(一个或多个)，请尝试使用中间相遇攻击的方法找到正确的密钥Key(K1+K2)。

![中间相遇攻击](https://github.com/TeFur0/S-AES/blob/main/png/中间相遇攻击.png?raw=true)

* **三重加密**

将S-AES算法通过三重加密进行扩展：

![三重加密](https://github.com/TeFur0/S-AES/blob/main/png/三重加密.png?raw=true)

* **工作模式**

基于S-AES算法，使用密码分组链(CBC)模式对较长的明文消息进行加密。注意初始向量(16 bits) 的生成，并需要加解密双方共享。
在CBC模式下进行加密，并尝试对密文分组进行替换或修改，然后进行解密，请对比篡改密文前后的解密结果。
![CBC-1](https://github.com/TeFur0/S-AES/blob/main/png/CBC-1.png?raw=true)


![CBC-2.1](https://github.com/TeFur0/S-AES/blob/main/png/CBC-1.png?raw=true)


对密文进行更改替换
![CBC-2.2](https://github.com/TeFur0/S-AES/blob/main/png/CBC-1.png?raw=true)


