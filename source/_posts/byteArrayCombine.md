---
title: 字节数组合并
date: 2019-04-20 14:59:02
tags:
    - java
---
最近碰到有同事处理字符数组合并时写法很麻烦, 所以帮着改了下,比之前的写法稍微简单一些.
<!-- more -->

### 原始写法  
```java
 public static byte[] bytesCombine(byte[] a, byte[] b){
    int newLength = a.length+b.length;
    byte[] result = new byte[newLength];
    for (int i = 0; i < a.length; i++) {
        result[i] = a[i];
    }
    for (int i = 0; i < b.length; i++) {
        result[a.length + i] = b[i];
    }
    return result;
}
```  

### 改进写法  
for循环来复制数组的话, 写法略微有些累赘,所以稍微改进一下:
```java
public static byte[] bytesCombine(byte[] a, byte[] b){
    int newLength = a.length+b.length;
    byte[] result = new byte[newLength];
    System.arraycopy(a, 0, result, 0, a.length);
    System.arraycopy(b, 0, result, a.length , b.length);
    return result;
}
```  

### 拓展写法
- 虽然这个方法现在的确能处理两个字节数组合并, 但是若是遇到多个字节数组合并时还得多次调用,考虑改成可变长度参数形式  
```java
public static byte[] bytesCombine(byte[]... bytes) {
        if (bytes.length == 1) {
            return bytes[0];
        }
        int newLength = 0;
        for (byte[] array : bytes) {
            newLength += array.length;
        }
        byte[] result = new byte[newLength];
        int currentLength = 0;
        for (byte[] array : bytes) {
            System.arraycopy(array, 0, result, currentLength, array.length);
            currentLength += array.length;
        }
        return result;
}
```  
- 循环了两次字符数组, 可以改用字节流来进行处理  
```java
public static byte[] bytesCombine(byte[]... bytes) {
        if (bytes.length == 1) {
            return bytes[0];
        }
        try(ByteArrayOutputStream outputStream = new ByteArrayOutputStream()){
            for (byte[] array : bytes) {
                outputStream.write(array, 0, array.length);
            }
            return outputStream.toByteArray();
        }catch (IOException e){
        //       do something
        }
        return null;
}
```  

### 总结
虽然只是简单的字节数组合并, 可以看到还是有很多不同的写法, 对应的效率也不同(以后可能会做一个性能测试). 最后拓展的写法中, 使用字节流处理的代码结构更加清晰, 但是需要对异常进行处理, 而不使用字节流的话会多一次循环过程, 所以大家在使用的时候可以根据个人喜好、具体场景等自由选择.  
补充一个性能测试图： 可以看到使用System.arraycopy比使用流效率更好, 在测试结果中， 使用流进行字节数组合并的效率甚至不及普通的for循环处理。
![benchmark](./benchmark.png)