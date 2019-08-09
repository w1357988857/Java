## Map

HashMap

key - value

key -》无序不重复	Set集合

value - 》 可以重复 Collection

map.entrySet();

Entry<K，V> getKey  getValue

TreeMap implements SortedMap extends Map

TreeMap<Student>

implements Comparable<Student> CompareTo

定制排序	TreeMap<Student> (new Compartor<Student>(){

​	//Override 	compareTo方法

});



## IO

字节流：InputStram（文件 -》内存）	OutputStream（内存 -》文件）

​		网络中，发送请求到服务器获取数据，服务器返回的数据，一般要通过（输入流）来获取

​	文件输入流（FileInputStream） 

​			int read() 一次读取一个字节

​			int read(byte[] b) 一次读取 b 个字节

​			int read(byte[] b,offset,length) 一次读取 b 个字节，从offset开始，读取长度 length

​	文件输出流（FileOutputStream）

​			write() 其余和read()类似







字节流：