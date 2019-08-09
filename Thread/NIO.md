# NIO

## 概述

使用IO流与存放在磁盘上的文件能够很方便的进行交互，但是它也存在一定的问题，问题的根源在于速度，由于磁盘IO速度远远小于CPU处理速度，这导致程序在进行IO操作的时候，不得不进行阻塞线程，等待IO操作完成，才能继续向下执行。

若某线程除了进行IO操作之外，还有其他重要的事情需要处理（给用户响应，并且不是IO的内容），但是现在遇到了IO，就必须停下来等IO，完了之后才进行下一步的操作，这有时候会使得用户觉得这个程序的处理速度太慢了，不能接受。

如上所述，Java IO可以说一种阻塞IO，这个时候就需要设计一种非阻塞IO，于是提出了Java NIO，可以称之为新IO，也可以称之为非阻塞IO。

Java NIO在进行read和write操作的时候，并不阻塞，会立即返回。



Java BIO，其全称是java blocking IO，相对的Java NIO 全称为java non-blocking IO。顾名思义，java nio 是一种非阻塞IO



Java NIO中有三个非常重要的对象：Buffer、Channel和Selector



高并发场景，并且请求处理的数据量偏小

Netty

## Channel

channel是一个通道，它和Java IO中的流对象，用于连接文件实体。Java通过NIO来从文件中读写数据的时候，必须建立一个通道，通过这个通道将数据写入到文件中，或者从文件中读取数据。 

Channel不能够直接被程序操作，channel中要么和文件打交道（网络上的资源也看做文件），要么和缓冲区打交道，除此之外，其他对象不能喝Channel交互。

通道可以是单向或者双向的。一个 channel 类可能实现定义read( )方法的 ReadableByteChannel 接口,而另一个 channel 类也许实现 WritableByteChannel 接口以提供 write( )方法。实现这两种接口其中之一的类都是单向的,只能在一个方向上传输数据。如果一个类同时实现这两个接口,那么它是双向的,可以双向传输数据。 

```
FileChannel          // 可以从文件中读写数据
DatagramChannel      //可以从UDP连接中读写数据
SocketChannel        //可以从TCP连接中读写数据
ServerSocketChannel  //用于监听TCP连接，一旦有连接来了之后，接受连接，并创建一个SocketChannel 
```

## Buffer

缓冲区是程序和通道之间的桥梁，程序有数据要写入文件的时候，会首先将数据写入到缓冲区中，之后在由缓冲区与Channel交互，将这些数据写入到文件中，反过来就是将Channel从文件中读取的数据先放到缓冲区中，之后程序从缓冲区中拿数据。

Buffer是一个对象，它用来存放即将发送的数据和即将到来的数据。Buffer是NIO核心思想，它与普通流IO的区别是，普通流IO直接把数据写入或读取到Stream对象中，而NIO是先把读写数据交给Buffer，后在用流处理的。Buffer实际上就是一个数组，通常是字节数组，这个数组提供了访问数据的读写等操作属性，如位置，容量，上限等概念。

缓冲区类型,基本上就是按照Java类型来分的

```
ByteBuffer
MappedByteBuffer
CharBuffer
DoubleBuffer
FloatBuffer
IntBuffer
LongBuffer
ShortBuffer
```

获取缓冲区对象的方法

```
//获取以字节缓冲区对象
ByteBuffer byteBuffer = ByteBuffer.allocate(48);
```

capacity 
capacity表示的是一块缓冲区的总容量，它一旦被设定好之后就不在变化。

position 
表示当前指针位置，缓冲区目前所处的状态不同，它所表示的含义也不同，当在往缓冲区中写入数据的时候，position只想最后写入的位置；当从缓冲区中读取数据的时候，position表示目前已经读到哪个位置了。

limit 
冲区目前所处的状态不同，它所表示的含义也不同，当在往缓冲区中写入数据的时候，limit表示缓冲区还有多少未用空间；当从缓冲区中读取数据的时候，limit表示的是在读数据之前，最后写入数据的位置。

mark()/reset() 

mark在对缓冲区进行操作过程中，对缓冲区打个标记点，在继续对缓冲区操作一会，调用reset()方法，就会使得position指向到刚刚打标记点的那个位置。



缓冲区有两种状态，写状态和读状态，当之前一直在往缓冲区中写入数据，此时想从缓冲区中读取数据，此时不能够直接调用get()方法，必须首先调用flip（）进行缓冲区翻转，之后才能够读取。

```
//首先翻转
buffer.flip();
//读取数据
buffer.get()
```

原因：从缓冲区中读取数据是根据position来读取的。当一直写数据的时候，position会不断变化，一直指向当前正在操作的位置，此时想从缓冲区中读取数据的话，不翻转直接读，那么读取position的下一位，要么是脏数据要么就没有数据，为了避免这种情况，NIO设计师们设计不翻转直接读的时候会报错。 

调用flip()之后,limit的值将会置为position，position将会置为0。然后在将缓冲区清空或者清空已经读取的数据位置，为下一次往缓冲区中写入数据腾出位置。

因此操作缓冲区的步骤： 
1.往缓冲区中写数据 
2.buffer.flip() 
3.从缓冲区中读取数据 
2.清空缓冲区（clear()/compact()）

## Selector

Selector会不断轮询注册在Selector上的通道(Channel)，如果这个通道发生了读写操作，这个通道就会处于就绪状态，会被Selector察觉到，然后通过SelectionKey可以取出就绪的Channel集合，从而进行IO操作。

Selector可以用于管理Channel，此时Channel需要向Selector进行注册，只有注册之后，Selector才会真正的去管理Channel。一个Selector可以管理多个通道，Selector通过select()方法不断的轮休这些Channel，当所有的Channel上都没有IO操作的时候，select()会阻塞，当注册在Selector上的Channel至少有一个需要进行IO操作的时候，正在执行select()方法的线程将会被唤醒，并且返回一个整数，代表有几个Channel需要进行IO操作。

每当有用户连接时，就会创建一个SocketChannel，并将其注册到Selector上。服务器只需要开启一个线程就可以处理多个用户连接：这条线程执行Selector，如果没有连接，阻塞，有连接的话，建立通道并注册到Selector上，交由Selector管理，Selector不管轮询注册在其上的Channel，有IO操作的话，就进行处理，当数据没有完全到来的时候也可以处理，此时read操作非阻塞不需要等待，立即返回，这样就可以使得一条线程处理多个用户，大大提高了线程的数据处理能力，也减轻了服务器端的压力。（netty框架据说可以一条线程处理成千上万个用户连接）

```
SelectionKey.OP_CONNECT    //关注连接
SelectionKey.OP_ACCEPT     
SelectionKey.OP_READ       //关注读IO
SelectionKey.OP_WRITE      //关注写IO
```



