## FileApi

file 文件通过监听 change 事件进行处理文件

1. name 文件的名字
2. size 文件的大小
3. type 字符串类型 文件的 MIME 类型
4. lastModifiedDate 字符串类型 文件上一次修改的时间

## FileReader 是一种异步读取文件的形式 可以将 fileReader 想象成 xhr

提供的方法

1. readAsText(file,encoding) 以纯文本的形式读取文件，将读取的文件保存到 result 中。第二个参数用于指定编码的类型
2. readAsDataURL(file) 读取文件将文件以数据 url 的形式保存与 result 中 大多数用 base64
3. readAsBinaryString(file) 读取文件并将一个字符串保存与 result 中，字符串中的每个字符代表一个字节
4. readAsArrayBuffer(file) 读取文件将一个包含文件内容的 ArrayBuffer 保存在 result 中

提供的事件

1. progress 每过 50ms 就会触发一次 progress 事件 参数为 lengthComputable，loaded，total 注意可以通过 filereader 中的 result 读取到每次读取的数据
2. error 表示发生的错误的事件，code 错误码有 1 未找到文件 2 表示安全性的问题 3 读取中断 4 文件不可读
3. load 文件加载成功之后会触发 load 事件 4.想要中断读取的过程 使用 abort 方法 会触发 abort 事件
4. loadend 事件 只要是读取文件结束就会触发的事件，不管 error 事件的触发还是 load 事件的触发

## 读取部分内容

1. 首先 file 是继承 blob 类型的对象
2. slice() 方法对文件进行切割 3.应用的场景：files[0].slice(0,32) 以上的例子展示了只读取 32b 的文件，然后接下来可以用 readAsText()进行处理，比如获取文件的头部等等

## 对象 URL

1. 对象 url 被称为 blob 对象，指的是引用保存 file 或者 blob 中的数据的 URL，这样的好处是并不需要将文件读取到 js 中进而使用文件。
2. createObjectURl()
3. 函数返回的是一个字符串，指向的是一块内存地址，因为 URL 是字符串所以 dom 中也可以使用的
4. 如果不需要的时候最好释放他所占用的内存，只要有代码引用对象 URL，内存就不会进行释放，所以最好手动释放内存。
5. revokeObjectURl()
6. 页面卸载的时候会自动释放对象 url，但是为了尽可能的减少内存最好手动释放内存空间

## web Worker

1. new Worker(文件)
2. postMessage 来进行同时两个不同作用域的代码空间进行通信，所以可以理解为执行的方法
3. onmessage 接收消息
4. onerror 失败的回调
5. 在全局作用域中调用 terminate()方法停止线程，在 worker 中调用 close()进行停止线程
6. 在 worker 作用域中调用 importScripts() 来进行动态的加载其他的脚本。

## arrayBuffer 数组理解

ArrayBuffer 是一(大)块内存，但不能直接访问 ArrayBuffer 里面的字节。要访问 ArrayBuffer，需要用到 Typed Array。其实 ArrayBuffer 跟 Typed Array 是一个东西，前者是一(大)块内存，后者用来访问这块内存。
网上文章建议使用 blob 进行传输图片文件，这样会比 file 更快，建议进行分片分段压缩上传这样效率更高