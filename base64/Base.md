# Base64

## Base64是做什么的？

##### 	1、Base64是一个编码格式，他不算一个加密算法

##### 	2、加密和编码的区别：

​		加密：把明文变为一种不可以破解的密文，提高识别难度

​		编码：换一种体现形式，以便于传输，提高可读性

##### 	 3、什么时候使用：

​			URL特殊字符，转码或者转义的时候使用

​			嵌入图片，src=“base64编码后的内容”

​			语言文字，底层都是用二进制来进行传输的

## Base转换的例子

##### 1、首先看看base64的最常用索引表

![base64索引表](image\base64索引表.png)

​			为什么只有64个，因为编码的时候，使用6位进行编码的，所以只有 2^6个

##### 2、看看Man是如何使用base64转换称为TWFu的

​	1）首先：M对应的ASCII码是77，二进制形式为01001101

​					 a的ASCII码是97，二进制表现形式为01100001

​					 n的ASCII码是110，二进制表现形式为01101110

​	 2）变化规则就是将他们抽取变为6个一组   即：010011 010110 000101 101110

​	 3）这样最终就生成的对应的Base64的编码格式

**需要注意的是：Base64编码24位【即四个字节】为一组，如果位数不够，就会补零。比如：当前只有01两位，就会加上四个零在进行计算。但是如果没有构成一组，就会补上“=”符号，用来填充字节**

## Base64算法的原理

​		即将3个8位字节（3✖8=24）转换称为4个6位字节（4✖6=24），不足六位，就进行后位补零，不能形成一组（24位为一组）就用=好代替字节

## 如何使用Base64传输图片

​	1、使用Spring或者Java提供的原生API将图片转换称为字符串

​	2、然后在传输的时候，在该字符串的头部加上 data:image/jpg;base64,

​	3、前面用于表示图片的格式，后面的base用于表示使用base64编码

```java
void contextLoads() throws IOException {
    BASE64Encoder base64Encoder = new BASE64Encoder();
    //将字节数组进行base64编码，并转换称为字符串
    String img = base64Encoder.encode(readFileToBytes("image.jpg"));
    //加上，用于表示使用了base编码
    System.out.println("data:image/jpg;base64,"+img);
}

//获取文件的字节数组
public static byte[] readFileToBytes(String fileName) throws IOException {
    BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream(fileName));
    byte[] bytes = new byte[4*1024*1024];
    bufferedInputStream.read(bytes);
    return bytes;
}
```

