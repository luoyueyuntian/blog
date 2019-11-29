Buffer 类是用于在 TCP 流、文件系统操作、以及其他上下文中与八位字节流进行交互。js现在已经增加了`TypedArray`，Buffer 类以更优化和更适合 Node.js 的方式实现了 Uint8Array API。

Buffer 类在全局作用域中，因此无需使用 require('buffer').Buffer。

### 特点

+ Uint8Array的实例
+ 大小是固定
+ 堆外分配物理内存

### buffer创建方式

+ 构造函数 `new Buffer` ——不推荐，高版本已废弃
+ `Buffer.from(array)` 返回一个新的 Buffer，其中包含提供的八位字节数组的副本
+ `Buffer.from(arrayBuffer[, byteOffset [, length]])` 返回一个新的 Buffer，它与给定的 ArrayBuffer 共享相同的已分配内存。
+ `Buffer.from(buffer)` 返回一个新的 Buffer，其中包含给定 Buffer 的内容的副本。
+ `Buffer.from(string[, encoding])` 返回一个新的 Buffer，其中包含提供的字符串的副本。
+ `Buffer.alloc(size[, fill[, encoding]])` 返回一个指定大小的新建的的已初始化的 Buffer。
+ `Buffer.allocUnsafe(size)` 返回一个指定大小的新建的未初始化的 Buffer，Buffer 实例可能是从共享的内部内存池中分配。
+ `Buffer.allocUnsafeSlow(size)` 返回一个指定大小的新建的未初始化的 Buffer，Buffer 实例则从不使用共享的内部内存池。

### Buffer.allocUnsafe() 和 Buffer.allocUnsafeSlow() 不安全

当调用 Buffer.allocUnsafe() 和 Buffer.allocUnsafeSlow() 时，分配的内存片段是未初始化的（没有被清零，可能包含敏感的旧数据），可能使旧数据泄露。

### --zero-fill-buffers 命令行选项

+ 强制新分配的 Buffer 实例在创建时自动用 0 填充
+ 会改变方法默认行为
+ 对性能有明显的影响

### 字符编码

Node.js 当前支持的字符编码有：

+ `ascii`: 仅适用于 7 位 ASCII 数据。此编码速度很快，如果设置则会剥离高位。
+ `utf8`: 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8。
+ `utf16le`: 2 或 4 个字节，小端序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。
+ `ucs2`: `utf16le` 的别名。
+ `base64`: Base64 编码。当从字符串创建 Buffer 时，此编码也会正确地接受 RFC 4648 第 5 节中指定的 “URL 和文件名安全字母”。
+ `latin1`: 一种将 Buffer 编码成单字节编码字符串的方法（由 RFC 1345 中的 IANA 定义，第 63 页，作为 Latin-1 的补充块和 C0/C1 控制码）。
+ `binary`: `latin1` 的别名。
+ `hex`: 将每个字节编码成两个十六进制的字符。

### Buffer 与 TypedArray

Buffer 实例也是 Uint8Array 实例，但是与 TypedArray 有微小的不同。

例如，ArrayBuffer#slice() 会创建切片的拷贝，而 Buffer#slice() 是在现有的 Buffer 上创建而不拷贝，这使得 Buffer#slice() 效率更高。

可以从一个 Buffer 创建新的 TypedArray 实例，但需要注意以下事项：

1. Buffer 对象的内存是被拷贝到 TypedArray，而不是共享。
2. Buffer 对象的内存是被解释为不同元素的数组，而不是目标类型的字节数组。 也就是说， new Uint32Array(Buffer.from([1, 2, 3, 4])) 会创建一个带有 4 个元素 [1, 2, 3, 4] 的 Uint32Array，而不是带有单个元素 [0x1020304] 或 [0x4030201] 的 Uint32Array。

通过使用 TypedArray 对象的 .buffer 属性，可以创建一个与 TypedArray 实例共享相同内存的新 Buffer。当使用 TypedArray 的 .buffer 创建 Buffer 时，也可以通过传入 byteOffset 和 length 参数只使用 TypedArray 的一部分。

Buffer.from() 与 TypedArray.from() 有着不同的实现。 具体来说，TypedArray 可以接受第二个参数作为映射函数，在类型数组的每个元素上调用：`TypedArray.from(source[, mapFn[, thisArg]])`。而`Buffer.from()` 则不支持映射函数的使用：

### Buffer 与迭代器

Buffer 实例可以使用 for..of 语法进行迭代

### Buffer 类

Buffer 类是一个全局变量，用于直接处理二进制数据。 它可以使用多种方式构建。

#### 属性

##### Buffer.poolSize

用于缓冲池的预分配的内部 Buffer 实例的大小（以字节为单位）。 该值可以修改。

#### 方法

+ Buffer.alloc(size[, fill[, encoding]])
+ Buffer.allocUnsafe(size)
+ Buffer.allocUnsafeSlow(size)
+ Buffer.from(array)
+ Buffer.from(arrayBuffer[, byteOffset[, length]])
+ Buffer.from(buffer)
+ Buffer.from(string[, encoding])
+ Buffer.from(object[, offsetOrEncoding[, length]])
+ Buffer.isBuffer(obj)
+ Buffer.isEncoding(encoding)
+ Buffer.byteLength(string[, encoding])
+ Buffer.compare(buf1, buf2)
+ Buffer.concat(list[, totalLength])

### Buffer 实例

#### 实例属性

+ buf[index]
+ buf.buffer
+ buf.length

#### 实例方法

+ buf.values()
+ buf.toString([encoding[, start[, end]]])
+ buf.toJSON()
+ buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])
+ buf.slice([start[, end]])
+ buf.keys()
+ buf.entries()
+ buf.equals(otherBuffer)
+ buf.compare(target[, targetStart[, targetEnd[, sourceStart[, sourceEnd]]]])
+ buf.fill(value[, offset[, end]][, encoding])
+ buf.includes(value[, byteOffset][, encoding])
+ buf.indexOf(value[, byteOffset][, encoding])
+ buf.lastIndexOf(value[, byteOffset][, encoding])
+ buf.swap16()
+ buf.swap32()
+ buf.swap64()

##### 从 `buffer` 中读数据
> BE 结尾的方法返回大端序，LE 结尾的方法返回小端序

+ `buf.readInt8(offset[, noAssert])`从 buf 中指定的 offset 读取一个有符号的 8 位整数值。从 Buffer 中读取的整数值会被解析为二进制补码值。
+ `buf.readInt16BE(offset[, noAssert])`从 buf 中指定的 offset 读取一个有符号的 16 位整数值。
+ `buf.readInt16LE(offset[, noAssert])`从 buf 中指定的 offset 读取一个有符号的 16 位整数值。
+ `buf.readFloatBE(offset[, noAssert])`从 buf 中指定的 offset 读取一个 32 位浮点值。
+ `buf.readFloatLE(offset[, noAssert])`从 buf 中指定的 offset 读取一个 32 位浮点值。
+ `buf.readInt32BE(offset[, noAssert])`从 buf 中指定的 offset 读取一个有符号的 32 位整数值。
+ `buf.readInt32LE(offset[, noAssert])`从 buf 中指定的 offset 读取一个有符号的 32 位整数值。
+ `buf.readIntBE(offset, byteLength[, noAssert])`从 buf 中指定的 offset 读取 byteLength 个字节，并将读取的值解析为二进制补码值。 最高支持 48 位精度。
+ `buf.readIntLE(offset, byteLength[, noAssert])`从 buf 中指定的 offset 读取 byteLength 个字节，并将读取的值解析为二进制补码值。 最高支持 48 位精度。
+ `buf.readDoubleBE(offset[, noAssert])`从 buf 中指定的 offset 读取一个 64 位双精度值。
+ `buf.readDoubleLE(offset[, noAssert])`从 buf 中指定的 offset 读取一个 64 位双精度值。
+ `buf.readBigInt64BE([offset])`从 buf 中指定的 offset 读取一个有符号的 64 位整数值。
+ `buf.readBigInt64LE([offset])`从 buf 中指定的 offset 读取一个有符号的 64 位整数值。

+ `buf.readUInt8(offset[, noAssert])`从 buf 中指定的 offset 读取一个无符号的 8 位整数值。
+ `buf.readUInt16BE(offset[, noAssert])`从 buf 中指定的 offset 读取一个无符号的 16 位整数值。
+ `buf.readUInt16LE(offset[, noAssert])`从 buf 中指定的 offset 读取一个无符号的 16 位整数值。
+ `buf.readUInt32BE(offset[, noAssert])`从 buf 中指定的 offset 读取一个无符号的 32 位整数值。
+ `buf.readUInt32LE(offset[, noAssert])`从 buf 中指定的 offset 读取一个无符号的 32 位整数值。
+ `buf.readUIntBE(offset, byteLength[, noAssert])`从 buf 中指定的 offset 读取 byteLength 个字节，并将读取的值解析为无符号的整数。 最高支持 48 位精度。
+ `buf.readUIntLE(offset, byteLength[, noAssert])`从 buf 中指定的 offset 读取 byteLength 个字节，并将读取的值解析为无符号的整数。 最高支持 48 位精度。
+ `buf.readBigUInt64BE([offset])`从 buf 中指定的 offset 读取一个无符号的 64 位整数值。
+ `buf.readBigUInt64LE([offset])`从 buf 中指定的 offset 读取一个无符号的 64 位整数值。

+ `buf.write(string[, offset[, length]][, encoding])`
+ `buf.writeDoubleBE(value, offset[, noAssert])`
+ `buf.writeDoubleLE(value, offset[, noAssert])`
+ `buf.writeFloatBE(value, offset[, noAssert])`
+ `buf.writeFloatLE(value, offset[, noAssert])`
+ `buf.writeInt8(value, offset[, noAssert])`
+ `buf.writeInt16BE(value, offset[, noAssert])`
+ `buf.writeInt16LE(value, offset[, noAssert])`
+ `buf.writeInt32BE(value, offset[, noAssert])`
+ `buf.writeInt32LE(value, offset[, noAssert])`
+ `buf.writeIntBE(value, offset, byteLength[, noAssert])`
+ `buf.writeIntLE(value, offset, byteLength[, noAssert])`
+ `buf.writeUInt8(value, offset[, noAssert])`
+ `buf.writeUInt16BE(value, offset[, noAssert])`
+ `buf.writeUInt16LE(value, offset[, noAssert])`
+ `buf.writeUInt32BE(value, offset[, noAssert])`
+ `buf.writeUInt32LE(value, offset[, noAssert])`
+ `buf.writeUIntBE(value, offset, byteLength[, noAssert])`
+ `buf.writeUIntLE(value, offset, byteLength[, noAssert])`
