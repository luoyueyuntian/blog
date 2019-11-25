文件是字节序列。每个I/O设备，包括磁盘、键盘、显示器，甚至网络，都可以看成文件。系统中的所有输入输出都是通过使用一小组称为Unix I/O的系统调用读写文件来实现的。

文件为应用程序提供了一个统一的视图，来看待系统中可能存在的所有格式的I/O设备。

#### 文件类型
+ 普通文件包含任意数据。应用程序常常要区分文本文件和二进制文件，文本文件是只包含ASCII或Unicode字符的普通文件；二进制文件是所有其它的文件。对于内核而言，文本文件和二进制文件没有区别。
+ 目录是包含一组链接的文件，其中每个链接都将一个文件名映射到一个文件，这个文件可能是另一个目录。每个目录至少含有两个条目：“.”是到该目录自身的链接，以及".."是到该目录层次结构中的父目录的链接。你可以用mkdir命令创建一个目录，用ls查看其内容，用rmdir删除该目录。
+ 套接字是用来与另一个进程进行跨网络通信的文件
+ 命名管道
+ 符号链接
+ 字符
+ 块设备

#### 文件路径
大多数 fs 操作接受的文件路径可以指定为字符串、Buffer、或使用 file: 协议的 URL 对象。

字符串形式的路径被解析为标识绝对或相对文件名的 UTF-8 字符序列。

+ 相对路径以文件名开始，表示从当前工作目录（process.cwd() 指定的当前工作目录）开始的路径。
+ 绝对路径以一个斜杠开始，表示同根目录开始的路径

使用 Buffer 指定的路径主要用于将文件路径视为不透明字节序列的某些 POSIX 操作系统。 在这样的系统上，单个文件路径可以包含使用多种字符编码的子序列。 与字符串路径一样， Buffer 路径可以是相对路径或绝对路径

对于大多数 fs 模块的函数， path 或 filename 参数可以传入 WHATWG URL 对象。 仅支持使用 file: 协议的 URL 对象。file: URL 始终是绝对路径。

+ 在 Windows 上，带有主机名的 file: URL 转换为 UNC 路径，而带有驱动器号的 file: URL 转换为本地绝对路径。 没有主机名和驱动器号的 file: URL 将导致抛出错误，带有驱动器号的 file: URL 必须使用 : 作为驱动器号后面的分隔符。 使用其他分隔符将导致抛出错误。包含编码后的反斜杆字符（%5C）的 file: URL 将导致抛出错误。包含编码后的斜杆字符（%2F）的 file: URL 都将导致抛出错误
+ 在所有其他平台上，不支持带有主机名的 file: URL，使用时将导致抛出错误。包含编码后的斜杆字符（%2F）的 file: URL 在所有平台上都将导致抛出错误

#### 文件描述符
在 POSIX 系统上，对于每个进程，内核都维护着一张当前打开着的文件和资源的表格。 每个打开的文件都分配了一个称为文件描述符的简单的数字标识符。 在系统层，所有文件系统操作都使用这些文件描述符来标识和跟踪每个特定的文件。 Windows 系统使用了一个虽然不同但概念上类似的机制来跟踪资源。 为了简化用户的工作，Node.js 抽象出操作系统之间的特定差异，并为所有打开的文件分配一个数字型的文件描述符。

fs.open() 方法用于分配新的文件描述符。 一旦被分配，则文件描述符可用于从文件读取数据、向文件写入数据、或请求关于文件的信息。

大多数操作系统限制在任何给定时间内可能打开的文件描述符的数量，因此当操作完成时关闭描述符至关重要。 如果不这样做将导致内存泄漏，最终导致应用程序崩溃。

注：当使用fd指定操作文件时，操作完成后文件不会自动关闭，需要手动的调用fs.close(fd)关闭文件。使用其他方式指定操作文件时，操作完成后文件会自动关闭。

## fs.Stats 类
fs.Stats 对象提供了关于文件的信息。

从 fs.stat()、fs.lstat() 和 fs.fstat() 及其同步的方法返回的对象都属于此类型。 如果传给这些方法的 options 中的 bigint 为 true，则数值将会为 bigint 型而不是 number 型，并且该对象将会包含额外的以 Ns 为后缀的纳秒精度的属性。

#### 属性
+ dev 文件所在的设备的标识
+ ino 文件的索引节点
+ mode 文件类型与模式的位域
+ nlink 文件的硬链接数
+ uid 文件所有者的用户标识
+ gid 文件所在组的群组标识
+ rdev 特殊文件的设备标识
+ size 文件的字节大小
+ blksize 用于 I/O 操作的内存块的大小
+ blocks 分配给文件的内存块的数量
+ atimeMs 最后一次访问文件的时间，单位是毫秒
+ mtimeMs 最后一次修改文件的时间，单位是毫秒
+ ctimeMs 最后一次改变文件状态的时间，单位是毫秒
+ birthtimeMs 创建文件的时间，单位是毫秒
+ atime 最后一次访问文件的时间
+ mtime 最后一次修改文件的时间
+ ctime 最后一次改变文件状态的时间
+ birthtime 创建文件的时间

#### 方法
+ stats.isFile()    判断是否是常规文件
+ stats.isDirectory()    判断是否是文件系统目录
+ stats.isBlockDevice()    判断是否是块设备
+ stats.isCharacterDevice()    判断是否是字符设备
+ stats.isSymbolicLink() (仅对 fs.lstat() 有效)    判断是否是符号链接
+ stats.isFIFO()    判断是否是先进先出（FIFO）管道
+ stats.isSocket()    判断是否是套接字


#### 文件属性的时间值
atimeMs、 mtimeMs、 ctimeMs 和 birthtimeMs 属性是保存相应时间（以毫秒为单位）的数值。 它们的精度取决于平台。 当将 bigint: true 传给生成该对象的方法时，属性将会是 bigint 型，否则它们将会是数字型。

atimeNs、 mtimeNs、 ctimeNs 和 birthtimeNs 属性是保存相应时间（以纳秒为单位）的 bigint。 仅当将 bigint: true 传给生成该对象的方法时，它们才会出现。 它们的精度取决于平台。

atime、 mtime、 ctime 和 birthtime 是对应时间的 Date 对象。 Date 值和数值没有关联性。 赋值新的数值、或者改变 Date 的值，都将不会影响到对应的属性。

stat 对象中的时间具有以下语义：
+ atime "访问时间" - 上次访问文件数据的时间。由 mknod(2)、 utimes(2) 和 read(2) 系统调用更改。
+ mtime "修改时间" - 上次修改文件数据的时间。由 mknod(2)、 utimes(2) 和 write(2) 系统调用更改。
+ ctime "更改时间" - 上次更改文件状态（修改索引节点数据）的时间。由 chmod(2)、 chown(2)、 link(2)、 mknod(2)、 rename(2)、 unlink(2)、 utimes(2)、 read(2) 和 write(2) 系统调用更改。
+ birthtime "创建时间" - 创建文件的时间。当创建文件时设置一次。 在不支持创建时间的文件系统上，该字段可能改为保存 ctime 或 1970-01-01T00:00Z（即 Unix 纪元时间戳 0）。 在这种情况下，该值可能大于 atime 或 mtime。 在 Darwin 和其他的 FreeBSD 衍生系统上，也可能使用 utimes(2) 系统调用将 atime 显式地设置为比 birthtime 更早的值。

## fs.Dirent 类
目录项的表示形式，通过从 fs.Dir 中读取而返回。

当使用 withFileTypes 选项设置为 true 调用 fs.readdir() 或 fs.readdirSync() 时，生成的数组将会填充 fs.Dirent 对象，而不是字符串或 Buffer。

#### 属性
+ dirent.name：fs.Dirent 对象指向的文件名。 此值的类型取决于传递给 fs.readdir() 或 fs.readdirSync() 的 options.encoding。

#### 方法
+ dirent.isBlockDevice：判断是否是块设备
+ dirent.isCharacterDevice：判断是否是字符设备
+ dirent.isDirectory：判断是否是目录
+ dirent.isFIFO：判断是否是先进先出（FIFO）管道
+ dirent.isFile：判断是否是普通文件
+ dirent.isSocket：判断是否是socket
+ dirent.isSymbolicLink：判断是否是符号链接


## fs.Dir 类
代表目录流的类。

由 fs.opendir()、fs.opendirSync() 或 fsPromises.opendir() 创建。

#### 属性
+ dir.path : 此目录的只读路径，与提供给 fs.opendir()、fs.opendirSync() 或 fsPromises.opendir() 的一样。

#### 方法
+ dir.close() : 异步地关闭目录的底层资源句柄。 随后的读取将会导致错误。返回一个 Promise，将会在关闭资源之后被解决。
+ dir.close(callback) : 异步地关闭目录的底层资源句柄。 随后的读取将会导致错误。关闭资源句柄之后将会调用 callback。
+ dir.closeSync() : 同步地关闭目录的底层资源句柄。 随后的读取将会导致错误。
+ dir.read() : 通过 readdir(3) 异步地读取下一个目录项作为 fs.Dirent。读取完成之后，将会返回一个 Promise，它被解决时将会返回 fs.Dirent 或 null（如果没有更多的目录项要读取）。此函数返回的目录项不遵循操作系统的底层目录机制所提供的特定顺序。 遍历目录时添加或删除的目录项可能会或可能不会包含在遍历的结果中。
+ dir.read(callback) : 通过 readdir(3) 异步地读取下一个目录项作为 fs.Dirent。读取完成之后，将会调用 callback 并传入 fs.Dirent 或 null（如果没有更多的目录项要读取）。
+ dir.readSync() : 此函数返回的目录项不遵循操作系统的底层目录机制所提供的特定顺序。 遍历目录时添加或删除的目录项可能会或可能不会包含在遍历的结果中。
+ dir[Symbol.asyncIterator] () : 通过 readdir(3) 同步地读取下一个目录项作为 fs.Dirent。如果没有更多的目录项要读取，则将会返回 null。此函数返回的目录项不遵循操作系统的底层目录机制所提供的特定顺序。 遍历目录时添加或删除的目录项可能会或可能不会包含在遍历的结果中。

## fs.FSWatcher 类

#### 事件
+ 'change' 当被监视的目录或文件有变化时触发
+ 'close' 停止监视文件变化时触发
+ 'error' 监视文件发生错误时触发

#### 方法
+ watcher.close() 停止监视文件的变化。一旦停止，fs.FSWatcher 对象将不再可用。


## fs.ReadStream 类

#### 事件
+ 'close'  fs.ReadStream 的文件描述符关闭时触发
+ 'open'  fs.ReadStream' 的文件描述符打开时触发
+ 'ready'  fs.ReadStream 已准备好可以使用时触发

#### 属性
+ readStream.bytesRead 已读取的字节数
+ readStream.path 流正在读取的文件的路径

## fs.WriteStream 类

#### 事件
+ 'close'  WriteStream 的文件描述符关闭时触发
+ 'open'  WriteStream 的文件打开时触发
+ 'ready'  fs.WriteStream 已准备好可以使用时触发。'open' 事件之后立即触发

#### 属性
+ writeStream.bytesWritten 已写入的字节数。 不包括正在队列中等待写入的数据。
+ writeStream.path 流正在写入的文件的路径

## fs模块提供的api

#### 判断是否可以访问文件
+ fs.access(path[, mode], callback)
+ fs.accessSync(path[, mode])

#### 判断文件是否存在
+ fs.existsSync(path)

#### 打开关闭文件
+ fs.open(path, flags[, mode], callback)
+ fs.openSync(path, flags[, mode])
+ fs.close(fd, callback)
+ fs.closeSync(fd)

#### 获取文件属性
+ fs.stat(path[, options], callback)
+ fs.statSync(path[, options])
+ fs.fstat(fd[, options], callback)
+ fs.fstatSync(fd[, options])
+ fs.lstat(path[, options], callback)
+ fs.lstatSync(path[, options])

#### 修改文件属性
+ fs.futimes(fd, atime, mtime, callback)
+ fs.futimesSync(fd, atime, mtime)
+ fs.utimes(path, atime, mtime, callback)
+ fs.utimesSync(path, atime, mtime)

#### 修改文件权限
+ fs.chmod(path, mode, callback)
+ fs.chmodSync(path, mode)
+ fs.fchmod(fd, mode, callback)
+ fs.fchmodSync(fd, mode)
+ fs.lchmod(path, mode, callback)
+ fs.lchmodSync(path, mode)

#### 更改文件的所有者和群组
+ fs.chown(path, uid, gid, callback)
+ fs.chownSync(path, uid, gid)
+ fs.fchown(fd, uid, gid, callback)
+ fs.fchownSync(fd, uid, gid)
+ fs.lchown(path, uid, gid, callback)
+ fs.lchownSync(path, uid, gid)

#### 创建读写流
+ fs.createReadStream(path[, options])
+ fs.createWriteStream(path[, options])

#### 读写文件
+ fs.read(fd, buffer, offset, length, position, callback)
+ fs.readSync(fd, buffer, offset, length, position)
+ fs.readFile(path[, options], callback)
+ fs.readFileSync(path[, options])
+ fs.write(fd, buffer[, offset[, length[, position]]], callback)
+ fs.write(fd, string[, position[, encoding]], callback)
+ fs.writeSync(fd, buffer[, offset[, length[, position]]])
+ fs.writeSync(fd, string[, position[, encoding]])
+ fs.writeFile(file, data[, options], callback)
+ fs.writeFileSync(file, data[, options])

#### 同步磁盘缓存
+ fs.fsync(fd, callback)
+ fs.fsyncSync(fd)
+ fs.fdatasync(fd, callback)
+ fs.fdatasyncSync(fd)

#### 截取文件内容
+ fs.ftruncate(fd[, len], callback)
+ fs.ftruncateSync(fd[, len])

#### 追加内容
+ fs.appendFile(path, data[, options], callback)
+ fs.appendFileSync(path, data[, options])

#### 拷贝文件
+ fs.copyFile(src, dest[, flags], callback)
+ fs.copyFileSync(src, dest[, flags])

#### 重命名
+ fs.rename(oldPath, newPath, callback)
+ fs.renameSync(oldPath, newPath)

#### 读取创建删除目录
+ fs.mkdir(path[, options], callback)
+ fs.mkdirSync(path[, options])
+ fs.mkdtemp(prefix[, options], callback)
+ fs.mkdtempSync(prefix[, options])
+ fs.readdir(path[, options], callback)
+ fs.readdirSync(path[, options])
+ fs.rmdir(path, callback)
+ fs.rmdirSync(path)

#### 创建删除读取链接
+ fs.link(existingPath, newPath, callback)
+ fs.linkSync(existingPath, newPath)
+ fs.readlink(path[, options], callback)
+ fs.readlinkSync(path[, options])
+ fs.symlink(target, path[, type], callback)
+ fs.symlinkSync(target, path[, type])
+ fs.unlink(path, callback)
+ fs.unlinkSync(path)

#### 观察文件
+ fs.watch(filename[, options][, listener])
+ fs.watchFile(filename[, options], listener)
+ fs.unwatchFile(filename[, listener])

#### 真实路径
+ fs.realpath(path[, options], callback)
+ fs.realpath.native(path[, options], callback)
+ fs.realpathSync(path[, options])
+ fs.realpathSync.native(path[, options])

### constants
+ 文件可访问性的常量
		F_OK 、 R_OK 、 W_OK 、 X_OK
+ 文件拷贝的常量
		COPYFILE_EXCL 、 COPYFILE_FICLONE 、 COPYFILE_FICLONE_FORCE
+ 文件打开的常量
		O_RDONLY 、 O_WRONLY 、 O_RDWR 、 O_CREAT 、 O_EXCL 、 O_NOCTTY 、 O_TRUNC 、 O_APPEND 、 O_DIRECTORY 、 O_NOATIME 、 O_NOFOLLOW 、 O_SYNC 、 O_DSYNC 、 O_SYMLINK 、 O_DIRECT 、 O_NONBLOCK
+ 文件类型的常量
		S_IFMT 、 S_IFREG 、 S_IFDIR 、 S_IFCHR 、 S_IFBLK 、 S_IFIFO 、 S_IFLNK 、 S_IFSOCK
+ 文件模式的常量
		S_IRWXU 、 S_IRUSR 、 S_IWUSR 、 S_IXUSR 、 S_IRWXG 、 S_IRGRP 、 S_IWGRP 、 S_IXGRP 、 S_IRWXO 、 S_IROTH 、 S_IWOTH 、 S_IXOTH

### 文件系统标志
+ 'a' - 打开文件用于追加。如果文件不存在，则创建该文件。
+ 'ax' - 与 'a' 相似，但如果路径存在则失败。
+ 'a+' - 打开文件用于读取和追加。如果文件不存在，则创建该文件。
+ 'ax+' - 与 'a+' 相似，但如果路径存在则失败。
+ 'as' - 以同步模式打开文件用于追加。如果文件不存在，则创建该文件。
+ 'as+' - 以同步模式打开文件用于读取和追加。如果文件不存在，则创建该文件。
+ 'r' - 打开文件用于读取。如果文件不存在，则会发生异常。
+ 'r+' - 打开文件用于读取和写入。如果文件不存在，则会发生异常。
+ 'rs+' - 以同步模式打开文件用于读取和写入。指示操作系统绕开本地文件系统缓存。
+ 'w' - 打开文件用于写入。创建文件（如果它不存在）或截断文件（如果存在）。
+ 'wx' - 与 'w' 相似，但如果路径存在则失败。
+ 'w+' - 打开文件用于读取和写入。创建文件（如果它不存在）或截断文件（如果存在）。
+ 'wx+' - 与 'w+' 相似，但如果路径存在则失败。
