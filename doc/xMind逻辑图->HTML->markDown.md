# xMind逻辑图->HTML->markDown

## 将xMind文件导出的HTML中解析的JSON数据转成MarkDown文件的js代码如下：
<pre><code>var treeData = [ /*这个数组放从HMTL里面解析出的数据*/ ];

function generateTreeHtmlStr(data) {
	var treeHtmlStr = '';
	var i, ii = data.length;
	treeHtmlStr += '&lt;ul style="margin-left:8px"&gt;';
	for (i = 0; i &lt; ii; i++) {
		treeHtmlStr += '&lt;li&gt;';
		treeHtmlStr += data[i].topic;
		if (data[i].iTopic !== undefined) {
			treeHtmlStr += generateTreeHtmlStr(data[i].iTopic);
		}
		treeHtmlStr += '&lt;/li&gt;'
	}
	treeHtmlStr += '&lt;/ul&gt;';
	return treeHtmlStr;
}
console.log(generateTreeHtmlStr(treeData));</code></pre>

## nodejs Buffer类方法分来总接
### 从HTML解析出的JSON数据格式如下：
<pre><code>[{
	"topic": "Buffer",
	"iTopic": [{
		"topic": "类方法",
		"iTopic": [{
			"topic": "创建实例",
			"iTopic": [{
				"topic": "Buffer.from()",
				"iTopic": [{
					"topic": "Buffer.from(array)"
				},
				{
					"topic": "Buffer.from(arrayBuffer[, byteOffset [, length]])"
				},
				{
					"topic": "Buffer.from(buffer)"
				},
				{
					"topic": "Buffer.from(string[, encoding])"
				}]
			},
			{
				"topic": "Buffer.alloc(size[, fill[, encoding]])"
			},
			{
				"topic": "Buffer.allocUnsafe(size)"
			},
			{
				"topic": "Buffer.allocUnsafeSlow(size)"
			}]
		},
		{
			"topic": "Buffer.byteLength(string[, encoding])"
		},
		{
			"topic": "Buffer.compare(buf1, buf2)"
		},
		{
			"topic": "Buffer.concat(list[, totalLength])"
		},
		{
			"topic": "Buffer.isBuffer(obj)"
		},
		{
			"topic": "Buffer.isEncoding(encoding)"
		}]
	},
	{
		"topic": "类属性",
		"iTopic": [{
			"topic": "Buffer.poolSize"
		}]
	},
	{
		"topic": "Buffer与TypedArray的区别",
		"iTopic": [{
			"topic": "对象的内存是否共享"
		}]
	},
	{
		"topic": "实例属性",
		"iTopic": [{
			"topic": "buf[index]"
		},
		{
			"topic": "buf.length"
		},
		{
			"topic": "buffer.INSPECT_MAX_BYTES"
		},
		{
			"topic": "buffer.kMaxLength"
		}]
	},
	{
		"topic": "实例方法",
		"iTopic": [{
			"topic": "buf.compare(target[, targetStart[, targetEnd[, sourceStart[, sourceEnd]]]])"
		},
		{
			"topic": "buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])"
		},
		{
			"topic": "buf.entries()"
		},
		{
			"topic": "buf.equals(otherBuffer)"
		},
		{
			"topic": "buf.fill(value[, offset[, end]][, encoding])"
		},
		{
			"topic": "buf.indexOf(value[, byteOffset][, encoding])"
		},
		{
			"topic": "buf.lastIndexOf(value[, byteOffset][, encoding])"
		},
		{
			"topic": "buf.includes(value[, byteOffset][, encoding])"
		},
		{
			"topic": "buf.keys()"
		},
		{
			"topic": "buf.readDoubleBE(offset[, noAssert])"
		},
		{
			"topic": "buf.readDoubleLE(offset[, noAssert])"
		},
		{
			"topic": "buf.readFloatBE(offset[, noAssert])"
		},
		{
			"topic": "buf.readFloatLE(offset[, noAssert])"
		},
		{
			"topic": "buf.readInt8(offset[, noAssert])"
		},
		{
			"topic": "buf.readInt16BE(offset[, noAssert])"
		},
		{
			"topic": "buf.readInt16LE(offset[, noAssert])"
		},
		{
			"topic": "buf.readInt32BE(offset[, noAssert])"
		},
		{
			"topic": "buf.readInt32LE(offset[, noAssert])"
		},
		{
			"topic": "buf.readIntBE(offset, byteLength[, noAssert])"
		},
		{
			"topic": "buf.readIntLE(offset, byteLength[, noAssert])"
		},
		{
			"topic": "buf.readUInt8(offset[, noAssert])"
		},
		{
			"topic": "buf.readUInt16BE(offset[, noAssert])"
		},
		{
			"topic": "buf.readUInt16LE(offset[, noAssert])"
		},
		{
			"topic": "buf.readUInt32BE(offset[, noAssert])"
		},
		{
			"topic": "buf.readUInt32LE(offset[, noAssert])"
		},
		{
			"topic": "buf.readUIntBE(offset, byteLength[, noAssert])"
		},
		{
			"topic": "buf.readUIntLE(offset, byteLength[, noAssert])"
		},
		{
			"topic": "buf.slice([start[, end]])"
		},
		{
			"topic": "将 buf 解析为数值数组",
			"iTopic": [{
				"topic": "buf.swap16()"
			},
			{
				"topic": "buf.swap32()"
			},
			{
				"topic": "buf.swap64()"
			}]
		},
		{
			"topic": "buf.toString([encoding[, start[, end]]])"
		},
		{
			"topic": "buf.toJSON()"
		},
		{
			"topic": "buf.values()"
		},
		{
			"topic": "buf.write(string[, offset[, length]][, encoding])"
		},
		{
			"topic": "buf.writeDoubleBE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeDoubleLE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeFloatBE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeFloatLE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeInt8(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeInt16BE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeInt16LE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeInt32BE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeInt32LE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeIntBE(value, offset, byteLength[, noAssert])"
		},
		{
			"topic": "buf.writeIntLE(value, offset, byteLength[, noAssert])"
		},
		{
			"topic": "buf.writeUInt8(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeUInt16BE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeUInt16LE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeUInt32BE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeUInt32LE(value, offset[, noAssert])"
		},
		{
			"topic": "buf.writeUIntBE(value, offset, byteLength[, noAssert])"
		},
		{
			"topic": "buf.writeUIntLE(value, offset, byteLength[, noAssert])"
		}]
	},
	{
		"topic": "Buffer 与字符编码",
		"iTopic": [{
			"topic": "支持的字符编码",
			"iTopic": [{
				"topic": "'ASCII'",
				"iTopic": [{
					"topic": "仅支持 7 位 ASCII 数据。如果设置去掉高位的话，这种编码是非常快的"
				}]
			},
			{
				"topic": "'utf8'",
				"iTopic": [{
					"topic": "多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8 "
				}]
			},
			{
				"topic": "'utf16le'",
				"iTopic": [{
					"topic": "2 或 4 个字节，小字节序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）"
				}]
			},
			{
				"topic": "'ucs2'",
				"iTopic": [{
					"topic": "'utf16le' 的别名"
				}]
			},
			{
				"topic": "'base64'",
				"iTopic": [{
					"topic": "Base64 编码。当从字符串创建 Buffer 时，按照 RFC4648 第 5 章的规定，这种编码也将正确地接受“URL 与文件名安全字母表”"
				}]
			},
			{
				"topic": "'latin1'",
				"iTopic": [{
					"topic": "一种把 Buffer 编码成一字节编码的字符串的方式（由 IANA 定义在 RFC1345 第 63 页，用作 Latin-1 补充块与 C0/C1 控制码）"
				}]
			},
			{
				"topic": "'binary'",
				"iTopic": [{
					"topic": "'latin1' 的别名"
				}]
			},
			{
				"topic": "'hex'",
				"iTopic": [{
					"topic": "将每个字节编码为两个十六进制字符"
				}]
			}]
		}]
	}]
}]
</code></pre>

### 转换成MarkDown文件如下：
<ul style="margin-left:8px"><li>Buffer<ul style="margin-left:8px"><li>类方法<ul style="margin-left:8px"><li>创建实例<ul style="margin-left:8px"><li>Buffer.from()<ul style="margin-left:8px"><li>Buffer.from(array)</li><li>Buffer.from(arrayBuffer[, byteOffset [, length]])</li><li>Buffer.from(buffer)</li><li>Buffer.from(string[, encoding])</li></ul></li><li>Buffer.alloc(size[, fill[, encoding]])</li><li>Buffer.allocUnsafe(size)</li><li>Buffer.allocUnsafeSlow(size)</li></ul></li><li>Buffer.byteLength(string[, encoding])</li><li>Buffer.compare(buf1, buf2)</li><li>Buffer.concat(list[, totalLength])</li><li>Buffer.isBuffer(obj)</li><li>Buffer.isEncoding(encoding)</li></ul></li><li>类属性<ul style="margin-left:8px"><li>Buffer.poolSize</li></ul></li><li>Buffer与TypedArray的区别<ul style="margin-left:8px"><li>对象的内存是否共享</li></ul></li><li>实例属性<ul style="margin-left:8px"><li>buf[index]</li><li>buf.length</li><li>buffer.INSPECT_MAX_BYTES</li><li>buffer.kMaxLength</li></ul></li><li>实例方法<ul style="margin-left:8px"><li>buf.compare(target[, targetStart[, targetEnd[, sourceStart[, sourceEnd]]]])</li><li>buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])</li><li>buf.entries()</li><li>buf.equals(otherBuffer)</li><li>buf.fill(value[, offset[, end]][, encoding])</li><li>buf.indexOf(value[, byteOffset][, encoding])</li><li>buf.lastIndexOf(value[, byteOffset][, encoding])</li><li>buf.includes(value[, byteOffset][, encoding])</li><li>buf.keys()</li><li>buf.readDoubleBE(offset[, noAssert])</li><li>buf.readDoubleLE(offset[, noAssert])</li><li>buf.readFloatBE(offset[, noAssert])</li><li>buf.readFloatLE(offset[, noAssert])</li><li>buf.readInt8(offset[, noAssert])</li><li>buf.readInt16BE(offset[, noAssert])</li><li>buf.readInt16LE(offset[, noAssert])</li><li>buf.readInt32BE(offset[, noAssert])</li><li>buf.readInt32LE(offset[, noAssert])</li><li>buf.readIntBE(offset, byteLength[, noAssert])</li><li>buf.readIntLE(offset, byteLength[, noAssert])</li><li>buf.readUInt8(offset[, noAssert])</li><li>buf.readUInt16BE(offset[, noAssert])</li><li>buf.readUInt16LE(offset[, noAssert])</li><li>buf.readUInt32BE(offset[, noAssert])</li><li>buf.readUInt32LE(offset[, noAssert])</li><li>buf.readUIntBE(offset, byteLength[, noAssert])</li><li>buf.readUIntLE(offset, byteLength[, noAssert])</li><li>buf.slice([start[, end]])</li><li>将 buf 解析为数值数组<ul style="margin-left:8px"><li>buf.swap16()</li><li>buf.swap32()</li><li>buf.swap64()</li></ul></li><li>buf.toString([encoding[, start[, end]]])</li><li>buf.toJSON()</li><li>buf.values()</li><li>buf.write(string[, offset[, length]][, encoding])</li><li>buf.writeDoubleBE(value, offset[, noAssert])</li><li>buf.writeDoubleLE(value, offset[, noAssert])</li><li>buf.writeFloatBE(value, offset[, noAssert])</li><li>buf.writeFloatLE(value, offset[, noAssert])</li><li>buf.writeInt8(value, offset[, noAssert])</li><li>buf.writeInt16BE(value, offset[, noAssert])</li><li>buf.writeInt16LE(value, offset[, noAssert])</li><li>buf.writeInt32BE(value, offset[, noAssert])</li><li>buf.writeInt32LE(value, offset[, noAssert])</li><li>buf.writeIntBE(value, offset, byteLength[, noAssert])</li><li>buf.writeIntLE(value, offset, byteLength[, noAssert])</li><li>buf.writeUInt8(value, offset[, noAssert])</li><li>buf.writeUInt16BE(value, offset[, noAssert])</li><li>buf.writeUInt16LE(value, offset[, noAssert])</li><li>buf.writeUInt32BE(value, offset[, noAssert])</li><li>buf.writeUInt32LE(value, offset[, noAssert])</li><li>buf.writeUIntBE(value, offset, byteLength[, noAssert])</li><li>buf.writeUIntLE(value, offset, byteLength[, noAssert])</li></ul></li><li>Buffer 与字符编码<ul style="margin-left:8px"><li>支持的字符编码<ul style="margin-left:8px"><li>'ASCII'<ul style="margin-left:8px"><li>仅支持 7 位 ASCII 数据。如果设置去掉高位的话，这种编码是非常快的</li></ul></li><li>'utf8'<ul style="margin-left:8px"><li>多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8 </li></ul></li><li>'utf16le'<ul style="margin-left:8px"><li>2 或 4 个字节，小字节序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）</li></ul></li><li>'ucs2'<ul style="margin-left:8px"><li>'utf16le' 的别名</li></ul></li><li>'base64'<ul style="margin-left:8px"><li>Base64 编码。当从字符串创建 Buffer 时，按照 RFC4648 第 5 章的规定，这种编码也将正确地接受“URL 与文件名安全字母表”</li></ul></li><li>'latin1'<ul style="margin-left:8px"><li>一种把 Buffer 编码成一字节编码的字符串的方式（由 IANA 定义在 RFC1345 第 63 页，用作 Latin-1 补充块与 C0/C1 控制码）</li></ul></li><li>'binary'<ul style="margin-left:8px"><li>'latin1' 的别名</li></ul></li><li>'hex'<ul style="margin-left:8px"><li>将每个字节编码为两个十六进制字符</li></ul></li></ul></li></ul></li></ul></li></ul> 

### 从xMind导出的HTML中解析出fs模块的JSON输入如下：
<pre><code>[{
	"topic": "fs模块",
	"iTopic": [{
		"topic": "读写文件",
		"iTopic": [{
			"topic": "直接读写文件",
			"iTopic": [{
				"topic": "读取文件内容",
				"iTopic": [{
					"topic": "fs.readFile(filename[, options], callback)"
				},
				{
					"topic": "fs.readFileSync(filename[, options])"
				}]
			},
			{
				"topic": "写入文件，文件已存在的情况下，原内容将被替换",
				"iTopic": [{
					"topic": "fs.writeFile(filename, data[, options], callback)"
				},
				{
					"topic": "fs.writeFileSync(filename, data[, options])"
				}]
			},
			{
				"topic": "将 data 插入到文件里，如果文件不存在会自动创建。data可以是任意字符串或者缓存",
				"iTopic": [{
					"topic": "fs.appendFile(filename, data[, options], callback)"
				},
				{
					"topic": "fs.appendFileSync(filename, data[, options])"
				}]
			}]
		},
		{
			"topic": "读写函数",
			"iTopic": [{
				"topic": "打开文件",
				"iTopic": [{
					"topic": "fs.open(path, flags[, mode], callback)"
				},
				{
					"topic": "fs.openSync(path, flags[, mode])"
				}]
			},
			{
				"topic": "根据指定的文件描述符fd来读取文件数据并写入buffer指向的缓冲区对象",
				"iTopic": [{
					"topic": "fs.read(fd, buffer, offset, length, position, callback)"
				},
				{
					"topic": "fs.readSync(fd, buffer, offset, length, position)"
				}]
			},
			{
				"topic": "写入文件（根据文件描述符）",
				"iTopic": [{
					"topic": "fs.write(fd, buffer, offset, length[, position], [callback(err, bytesWritten, buffer)]) "
				},
				{
					"topic": "fs.writeSync(fd, buffer, offset, length[, position])"
				}]
			},
			{
				"topic": "逐行读取",
				"iTopic": [{
					"topic": "fs.readlink(path[, options], callback)"
				},
				{
					"topic": "fs.readlinkSync(path[, options])"
				}]
			},
			{
				"topic": "将 buffer 写到 fd指定的文件里",
				"iTopic": [{
					"topic": "fs.write(fd, data[, position[, encoding]], callback)"
				},
				{
					"topic": "fs.writeSync(fd, data[, position[, encoding]])"
				}]
			},
			{
				"topic": "关闭文件",
				"iTopic": [{
					"topic": "fs.close(fd, callback)"
				},
				{
					"topic": "fs.closeSync(fd)"
				}]
			},
			{
				"topic": "文件内容截取操作",
				"iTopic": [{
					"topic": "fs.ftruncate(fd, len, callback) "
				},
				{
					"topic": "fs.ftruncateSync(fd, len)"
				},
				{
					"topic": "fs.truncate(path, len, callback)"
				},
				{
					"topic": "fs.truncateSync(path, len)"
				}]
			},
			{
				"topic": "同步磁盘缓存",
				"iTopic": [{
					"topic": "fs.fsync(fd, callback)"
				},
				{
					"topic": "fs.fsyncSync(fd)"
				}]
			}]
		}]
	},
	{
		"topic": "文件目录",
		"iTopic": [{
			"topic": "创建文件目录。如果目录已存在，将抛出异常。",
			"iTopic": [{
				"topic": "fs.mkdir(path[, mode], callback)"
			},
			{
				"topic": "fs.mkdirSync(path[, mode])"
			}]
		},
		{
			"topic": "删除文件目录",
			"iTopic": [{
				"topic": "fs.rmdir(path, callback)"
			},
			{
				"topic": "fs.rmdirSync(path)"
			}]
		},
		{
			"topic": "读取文件目录",
			"iTopic": [{
				"topic": "fs.readdir(path, callback)"
			},
			{
				"topic": " fs.readdirSync(path)"
			}]
		}]
	},
	{
		"topic": "链接",
		"iTopic": [{
			"topic": "创建一个链接 ",
			"iTopic": [{
				"topic": "创建硬链接",
				"iTopic": [{
					"topic": "fs.link(srcpath, dstpath, callback) "
				},
				{
					"topic": "fs.linkSync(srcpath, dstpath)"
				}]
			},
			{
				"topic": "创建符号链接",
				"iTopic": [{
					"topic": "fs.symlink(srcpath, dstpath[, type], callback) "
				},
				{
					"topic": "fs.symlinkSync(srcpath, dstpath[, type])"
				}]
			}]
		},
		{
			"topic": "读取链接",
			"iTopic": [{
				"topic": "fs.readlink(path, [callback(err,linkstr)])  "
			},
			{
				"topic": "fs.readlinkSync(path)"
			}]
		},
		{
			"topic": "删除链接",
			"iTopic": [{
				"topic": "fs.unlink(path,[callback])"
			},
			{
				"topic": "fs.unlinkSync(path)"
			}]
		}]
	},
	{
		"topic": "fs.ReadStream 类",
		"iTopic": [{
			"topic": "事件",
			"iTopic": [{
				"topic": "'open' 事件"
			},
			{
				"topic": "'close' 事件"
			}]
		},
		{
			"topic": "属性",
			"iTopic": [{
				"topic": "readStream.bytesRead"
			},
			{
				"topic": "readStream.path"
			}]
		}]
	},
	{
		"topic": "fs.WriteStream 类",
		"iTopic": [{
			"topic": "事件",
			"iTopic": [{
				"topic": "'open' 事件"
			},
			{
				"topic": "'close' 事件"
			}]
		},
		{
			"topic": "属性",
			"iTopic": [{
				"topic": "writeStream.bytesWritten"
			},
			{
				"topic": "writeStream.path"
			}]
		}]
	},
	{
		"topic": "fs.Stats 类",
		"iTopic": [{
			"topic": "方法",
			"iTopic": [{
				"topic": "stats.isFile()"
			},
			{
				"topic": "stats.isDirectory()"
			},
			{
				"topic": "stats.isBlockDevice()"
			},
			{
				"topic": "stats.isCharacterDevice()"
			},
			{
				"topic": "stats.isSymbolicLink()",
				"iTopic": [{
					"topic": " (仅对 fs.lstat() 有效)"
				}]
			},
			{
				"topic": "stats.isFIFO()"
			},
			{
				"topic": "stats.isSocket()"
			}]
		},
		{
			"topic": "属性",
			"iTopic": [{
				"topic": "dev"
			},
			{
				"topic": "ino"
			},
			{
				"topic": "mode"
			},
			{
				"topic": "nlink"
			},
			{
				"topic": "uid"
			},
			{
				"topic": "gid"
			},
			{
				"topic": "rdev"
			},
			{
				"topic": "size"
			},
			{
				"topic": "blksize"
			},
			{
				"topic": "blocks"
			},
			{
				"topic": "atime",
				"iTopic": [{
					"topic": "\"访问时间\" - 文件数据最近被访问的时间。 会被 mknod(2)、 utimes(2) 和 read(2) 系统调用改变"
				}]
			},
			{
				"topic": "mtime",
				"iTopic": [{
					"topic": "\"修改时间\" - 文件数据最近被修改的时间。 会被 mknod(2)、 utimes(2) 和 write(2) 系统调用改变"
				}]
			},
			{
				"topic": "ctime",
				"iTopic": [{
					"topic": "\"变化时间\" - 文件状态最近更改的时间（修改索引节点数据） 会被 chmod(2)、 chown(2)、 link(2)、 mknod(2)、 rename(2)、 unlink(2)、 utimes(2)、 read(2) 和 write(2) 系统调用改变"
				}]
			},
			{
				"topic": "birthtime",
				"iTopic": [{
					"topic": "\"创建时间\" - 文件创建的时间。 当文件被创建时设定一次。 在创建时间不可用的文件系统中，该字段可能被替代为 ctime 或 1970-01-01T00:00Z（如 Unix 的纪元时间戳 0）。 注意，该值在此情况下可能会大于 atime 或 mtime。 在 Darwin 和其它的 FreeBSD 衍生系统中，如果 atime 被使用 utimes(2) 系统调用显式地设置为一个比当前 birthtime 更早的值，也会有这种情况"
				}]
			}]
		}]
	},
	{
		"topic": "fs.FSWatcher 类",
		"iTopic": [{
			"topic": "事件",
			"iTopic": [{
				"topic": "'change' 事件"
			},
			{
				"topic": "'error' 事件"
			}]
		},
		{
			"topic": "方法",
			"iTopic": [{
				"topic": "watcher.close()"
			}]
		}]
	},
	{
		"topic": "文件信息",
		"iTopic": [{
			"topic": "获取文件信息",
			"iTopic": [{
				"topic": "fs.stat(path, callback)"
			},
			{
				"topic": "fs.statSync(path)"
			},
			{
				"topic": "fs.fstat(fd, callback)"
			},
			{
				"topic": "fs.fstatSync(fd)"
			}]
		},
		{
			"topic": "获取文件信息（不解析符号链接）",
			"iTopic": [{
				"topic": "fs.lstat(path, [callback(err, stats)])"
			},
			{
				"topic": "fs.lstatSync(path)"
			}]
		},
		{
			"topic": "改变指定路径文件的时间戳",
			"iTopic": [{
				"topic": "fs.utimes(path, atime, mtime, callback)"
			},
			{
				"topic": "fs.utimesSync(path, atime, mtime)"
			}]
		},
		{
			"topic": "改变传入的文件描述符指向文件的时间戳",
			"iTopic": [{
				"topic": "fs.futimes(fd, atime, mtime, callback)"
			},
			{
				"topic": "fs.futimesSync(fd, atime, mtime)"
			}]
		},
		{
			"topic": "测试某个路径下的文件是否存在",
			"iTopic": [{
				"topic": "fs.exists(path, callback) "
			},
			{
				"topic": "fs.existsSync(path)"
			}]
		},
		{
			"topic": "获取路径",
			"iTopic": [{
				"topic": "真实路径",
				"iTopic": [{
					"topic": "fs.realpath(path[, cache], callback)"
				},
				{
					"topic": "fs.realpathSync(path[, cache])"
				}]
			},
			{
				"topic": "相对路径",
				"iTopic": [{
					"topic": "process.cwd"
				}]
			}]
		}]
	},
	{
		"topic": "权限",
		"iTopic": [{
			"topic": "更改文件所有权",
			"iTopic": [{
				"topic": "fs.chown(path, uid, gid, callback)"
			},
			{
				"topic": "fs.chownSync(path, uid, gid)"
			}]
		},
		{
			"topic": "更改文件所有权(文件描述符)",
			"iTopic": [{
				"topic": "fs.fchown(fd, uid, gid, callback)"
			},
			{
				"topic": "fs.fchownSync(fd, uid, gid)"
			}]
		},
		{
			"topic": "更改文件所有权（不解析符号链接）",
			"iTopic": [{
				"topic": "fs.lchown(path, uid, gid, callback)"
			},
			{
				"topic": "fs.lchownSync(path, uid, gid)"
			}]
		},
		{
			"topic": "改写文件的读写权限",
			"iTopic": [{
				"topic": "fs.chmod(path, mode, callback)"
			},
			{
				"topic": "fs.chmodSync(path, mode)"
			}]
		},
		{
			"topic": "更改文件权限（文件描述符）",
			"iTopic": [{
				"topic": "fs.fchmod(fd, mode, callback)"
			},
			{
				"topic": "fs.fchmodSync(fd, mode)"
			}]
		},
		{
			"topic": "检查到指定path路径的目录或文件的访问权限",
			"iTopic": [{
				"topic": "fs.access(path[, mode], callback)"
			},
			{
				"topic": "fs.accessSync(path[, mode])"
			}]
		}]
	},
	{
		"topic": "操作文件",
		"iTopic": [{
			"topic": "监视 filename 文件的变化",
			"iTopic": [{
				"topic": "fs.watchFile(filename[, options], listener)"
			},
			{
				"topic": "fs.unwatchFile(filename[, listener])"
			}]
		},
		{
			"topic": "观察 filename 指定的文件或文件夹的改变",
			"iTopic": [{
				"topic": "fs.watch(filename[, options][, listener])   "
			}]
		},
		{
			"topic": "文件或文件夹重命名",
			"iTopic": [{
				"topic": "fs.rename(oldPath, newPath, callback)"
			},
			{
				"topic": "fs.renameSync(oldPath, newPath)"
			}]
		}]
	},
	{
		"topic": "文件读写流",
		"iTopic": [{
			"topic": "创建readStream（文件读取流，输入流）对象",
			"iTopic": [{
				"topic": "fs.createReadStream(path[, options])"
			}]
		},
		{
			"topic": "创建WriteStream（输出流）对象（可写流）",
			"iTopic": [{
				"topic": "fs.createWriteStream(path[, options])"
			}]
		}]
	},
	{
		"topic": "fs 常量",
		"iTopic": [{
			"topic": "文件访问常量",
			"iTopic": [{
				"topic": "F_OK",
				"iTopic": [{
					"topic": "该标志表明文件对于调用进程是可见的"
				}]
			},
			{
				"topic": "R_OK",
				"iTopic": [{
					"topic": "标志表明文件可被调用进程读取"
				}]
			},
			{
				"topic": "W_OK",
				"iTopic": [{
					"topic": "该标志表明文件可被调用进程写入"
				}]
			},
			{
				"topic": "X_OK",
				"iTopic": [{
					"topic": "该标志表明文件可被调用进程执行"
				}]
			}]
		},
		{
			"topic": "文件打开常量",
			"iTopic": [{
				"topic": "O_RDONLY",
				"iTopic": [{
					"topic": "该标志表明打开一个文件用于只读访问"
				}]
			},
			{
				"topic": "O_WRONLY",
				"iTopic": [{
					"topic": "该标志表明打开一个文件用于只写访问"
				}]
			},
			{
				"topic": "O_RDWR",
				"iTopic": [{
					"topic": "该标志表明打开一个文件用于读写访问"
				}]
			},
			{
				"topic": "O_CREAT",
				"iTopic": [{
					"topic": "该标志表明如果文件不存在则创建一个文件"
				}]
			},
			{
				"topic": "O_EXCL",
				"iTopic": [{
					"topic": "该标志表明如果设置了 O_CREAT 标志且文件已经存在，则打开一个文件应该失败"
				}]
			},
			{
				"topic": "O_NOCTTY",
				"iTopic": [{
					"topic": "该标志表明如果路径是一个终端设备，则打开该路径不应该造成该终端变成进程的控制终端（如果进程还没有终端）"
				}]
			},
			{
				"topic": "O_TRUNC",
				"iTopic": [{
					"topic": "该标志表明如果文件存在且为一个常规文件、且文件被成功打开为写入访问，则它的长度应该被截断至零"
				}]
			},
			{
				"topic": "O_APPEND",
				"iTopic": [{
					"topic": "该标志表明数据会被追加到文件的末尾"
				}]
			},
			{
				"topic": "O_DIRECTORY",
				"iTopic": [{
					"topic": "该标志表明如果路径不是一个目录，则打开应该失败"
				}]
			},
			{
				"topic": "O_NOATIME",
				"iTopic": [{
					"topic": "该标志表明文件系统的读取访问权不再引起相关文件 atime 信息的更新。该标志只在 Linux 操作系统有效"
				}]
			},
			{
				"topic": "O_NOFOLLOW",
				"iTopic": [{
					"topic": "该标志表明如果路径是一个符号链接，则打开应该失败"
				}]
			},
			{
				"topic": "O_SYNC",
				"iTopic": [{
					"topic": "该标志表明文件打开用于同步 I/O"
				}]
			},
			{
				"topic": "O_SYMLINK",
				"iTopic": [{
					"topic": "该标志表明打开符号链接自身，而不是它指向的资源"
				}]
			},
			{
				"topic": "O_DIRECT",
				"iTopic": [{
					"topic": "\t当设置它时，会尝试最小化文件 I/O 的缓存效果"
				}]
			},
			{
				"topic": "O_NONBLOCK",
				"iTopic": [{
					"topic": "该标志表明当可能时以非阻塞模式打开文件"
				}]
			}]
		},
		{
			"topic": "文件类型常量",
			"iTopic": [{
				"topic": "S_IFMT",
				"iTopic": [{
					"topic": "用于提取文件类型码的位掩码"
				}]
			},
			{
				"topic": "S_IFREG",
				"iTopic": [{
					"topic": "表示一个常规文件的文件类型常量"
				}]
			},
			{
				"topic": "S_IFDIR",
				"iTopic": [{
					"topic": "\t表示一个目录的文件类型常量"
				}]
			},
			{
				"topic": "S_IFCHR",
				"iTopic": [{
					"topic": "表示一个面向字符的设备文件的文件类型常量"
				}]
			},
			{
				"topic": "S_IFBLK",
				"iTopic": [{
					"topic": "表示一个面向块的设备文件的文件类型常量"
				}]
			},
			{
				"topic": "S_IFIFO",
				"iTopic": [{
					"topic": "表示一个 FIFO/pipe 的文件类型常量"
				}]
			},
			{
				"topic": "S_IFLNK",
				"iTopic": [{
					"topic": "表示一个符号链接的文件类型常量"
				}]
			},
			{
				"topic": "S_IFSOCK",
				"iTopic": [{
					"topic": "表示一个 socket 的文件类型常量"
				}]
			}]
		},
		{
			"topic": "文件模式常量",
			"iTopic": [{
				"topic": "S_IRWXU",
				"iTopic": [{
					"topic": "该文件模式表明可被所有者读取、写入、执行"
				}]
			},
			{
				"topic": "S_IRUSR",
				"iTopic": [{
					"topic": "该文件模式表明可被所有者读取"
				}]
			},
			{
				"topic": "S_IWUSR",
				"iTopic": [{
					"topic": "该文件模式表明可被所有者写入"
				}]
			},
			{
				"topic": "S_IXUSR",
				"iTopic": [{
					"topic": "该文件模式表明可被所有者执行"
				}]
			},
			{
				"topic": "S_IRWXG",
				"iTopic": [{
					"topic": "\t该文件模式表明可被群组读取、写入、执行"
				}]
			},
			{
				"topic": "S_IRGRP",
				"iTopic": [{
					"topic": "该文件模式表明可被群组读取"
				}]
			},
			{
				"topic": "S_IWGRP",
				"iTopic": [{
					"topic": "该文件模式表明可被群组写入"
				}]
			},
			{
				"topic": "S_IXGRP",
				"iTopic": [{
					"topic": "该文件模式表明可被群组执行"
				}]
			},
			{
				"topic": "S_IRWXO",
				"iTopic": [{
					"topic": "该文件模式表明可被其他人读取、写入、执行"
				}]
			}]
		}]
	}]
}]</code></pre>

### 将JSON格式数据转换成MarkDown文件如下：
<pre><code><ul style="margin-left:8px"><li>fs模块<ul style="margin-left:8px"><li>读写文件<ul style="margin-left:8px"><li>直接读写文件<ul style="margin-left:8px"><li>读取文件内容<ul style="margin-left:8px"><li>fs.readFile(filename[, options], callback)</li><li>fs.readFileSync(filename[, options])</li></ul></li><li>写入文件，文件已存在的情况下，原内容将被替换<ul style="margin-left:8px"><li>fs.writeFile(filename, data[, options], callback)</li><li>fs.writeFileSync(filename, data[, options])</li></ul></li><li>将 data 插入到文件里，如果文件不存在会自动创建。data可以是任意字符串或者缓存<ul style="margin-left:8px"><li>fs.appendFile(filename, data[, options], callback)</li><li>fs.appendFileSync(filename, data[, options])</li></ul></li></ul></li><li>读写函数<ul style="margin-left:8px"><li>打开文件<ul style="margin-left:8px"><li>fs.open(path, flags[, mode], callback)</li><li>fs.openSync(path, flags[, mode])</li></ul></li><li>根据指定的文件描述符fd来读取文件数据并写入buffer指向的缓冲区对象<ul style="margin-left:8px"><li>fs.read(fd, buffer, offset, length, position, callback)</li><li>fs.readSync(fd, buffer, offset, length, position)</li></ul></li><li>写入文件（根据文件描述符）<ul style="margin-left:8px"><li>fs.write(fd, buffer, offset, length[, position], [callback(err, bytesWritten, buffer)]) </li><li>fs.writeSync(fd, buffer, offset, length[, position])</li></ul></li><li>逐行读取<ul style="margin-left:8px"><li>fs.readlink(path[, options], callback)</li><li>fs.readlinkSync(path[, options])</li></ul></li><li>将 buffer 写到 fd指定的文件里<ul style="margin-left:8px"><li>fs.write(fd, data[, position[, encoding]], callback)</li><li>fs.writeSync(fd, data[, position[, encoding]])</li></ul></li><li>关闭文件<ul style="margin-left:8px"><li>fs.close(fd, callback)</li><li>fs.closeSync(fd)</li></ul></li><li>文件内容截取操作<ul style="margin-left:8px"><li>fs.ftruncate(fd, len, callback) </li><li>fs.ftruncateSync(fd, len)</li><li>fs.truncate(path, len, callback)</li><li>fs.truncateSync(path, len)</li></ul></li><li>同步磁盘缓存<ul style="margin-left:8px"><li>fs.fsync(fd, callback)</li><li>fs.fsyncSync(fd)</li></ul></li></ul></li></ul></li><li>文件目录<ul style="margin-left:8px"><li>创建文件目录。如果目录已存在，将抛出异常。<ul style="margin-left:8px"><li>fs.mkdir(path[, mode], callback)</li><li>fs.mkdirSync(path[, mode])</li></ul></li><li>删除文件目录<ul style="margin-left:8px"><li>fs.rmdir(path, callback)</li><li>fs.rmdirSync(path)</li></ul></li><li>读取文件目录<ul style="margin-left:8px"><li>fs.readdir(path, callback)</li><li> fs.readdirSync(path)</li></ul></li></ul></li><li>链接<ul style="margin-left:8px"><li>创建一个链接 <ul style="margin-left:8px"><li>创建硬链接<ul style="margin-left:8px"><li>fs.link(srcpath, dstpath, callback) </li><li>fs.linkSync(srcpath, dstpath)</li></ul></li><li>创建符号链接<ul style="margin-left:8px"><li>fs.symlink(srcpath, dstpath[, type], callback) </li><li>fs.symlinkSync(srcpath, dstpath[, type])</li></ul></li></ul></li><li>读取链接<ul style="margin-left:8px"><li>fs.readlink(path, [callback(err,linkstr)])  </li><li>fs.readlinkSync(path)</li></ul></li><li>删除链接<ul style="margin-left:8px"><li>fs.unlink(path,[callback])</li><li>fs.unlinkSync(path)</li></ul></li></ul></li><li>fs.ReadStream 类<ul style="margin-left:8px"><li>事件<ul style="margin-left:8px"><li>'open' 事件</li><li>'close' 事件</li></ul></li><li>属性<ul style="margin-left:8px"><li>readStream.bytesRead</li><li>readStream.path</li></ul></li></ul></li><li>fs.WriteStream 类<ul style="margin-left:8px"><li>事件<ul style="margin-left:8px"><li>'open' 事件</li><li>'close' 事件</li></ul></li><li>属性<ul style="margin-left:8px"><li>writeStream.bytesWritten</li><li>writeStream.path</li></ul></li></ul></li><li>fs.Stats 类<ul style="margin-left:8px"><li>方法<ul style="margin-left:8px"><li>stats.isFile()</li><li>stats.isDirectory()</li><li>stats.isBlockDevice()</li><li>stats.isCharacterDevice()</li><li>stats.isSymbolicLink()<ul style="margin-left:8px"><li> (仅对 fs.lstat() 有效)</li></ul></li><li>stats.isFIFO()</li><li>stats.isSocket()</li></ul></li><li>属性<ul style="margin-left:8px"><li>dev</li><li>ino</li><li>mode</li><li>nlink</li><li>uid</li><li>gid</li><li>rdev</li><li>size</li><li>blksize</li><li>blocks</li><li>atime<ul style="margin-left:8px"><li>"访问时间" - 文件数据最近被访问的时间。 会被 mknod(2)、 utimes(2) 和 read(2) 系统调用改变</li></ul></li><li>mtime<ul style="margin-left:8px"><li>"修改时间" - 文件数据最近被修改的时间。 会被 mknod(2)、 utimes(2) 和 write(2) 系统调用改变</li></ul></li><li>ctime<ul style="margin-left:8px"><li>"变化时间" - 文件状态最近更改的时间（修改索引节点数据） 会被 chmod(2)、 chown(2)、 link(2)、 mknod(2)、 rename(2)、 unlink(2)、 utimes(2)、 read(2) 和 write(2) 系统调用改变</li></ul></li><li>birthtime<ul style="margin-left:8px"><li>"创建时间" - 文件创建的时间。 当文件被创建时设定一次。 在创建时间不可用的文件系统中，该字段可能被替代为 ctime 或 1970-01-01T00:00Z（如 Unix 的纪元时间戳 0）。 注意，该值在此情况下可能会大于 atime 或 mtime。 在 Darwin 和其它的 FreeBSD 衍生系统中，如果 atime 被使用 utimes(2) 系统调用显式地设置为一个比当前 birthtime 更早的值，也会有这种情况</li></ul></li></ul></li></ul></li><li>fs.FSWatcher 类<ul style="margin-left:8px"><li>事件<ul style="margin-left:8px"><li>'change' 事件</li><li>'error' 事件</li></ul></li><li>方法<ul style="margin-left:8px"><li>watcher.close()</li></ul></li></ul></li><li>文件信息<ul style="margin-left:8px"><li>获取文件信息<ul style="margin-left:8px"><li>fs.stat(path, callback)</li><li>fs.statSync(path)</li><li>fs.fstat(fd, callback)</li><li>fs.fstatSync(fd)</li></ul></li><li>获取文件信息（不解析符号链接）<ul style="margin-left:8px"><li>fs.lstat(path, [callback(err, stats)])</li><li>fs.lstatSync(path)</li></ul></li><li>改变指定路径文件的时间戳<ul style="margin-left:8px"><li>fs.utimes(path, atime, mtime, callback)</li><li>fs.utimesSync(path, atime, mtime)</li></ul></li><li>改变传入的文件描述符指向文件的时间戳<ul style="margin-left:8px"><li>fs.futimes(fd, atime, mtime, callback)</li><li>fs.futimesSync(fd, atime, mtime)</li></ul></li><li>测试某个路径下的文件是否存在<ul style="margin-left:8px"><li>fs.exists(path, callback) </li><li>fs.existsSync(path)</li></ul></li><li>获取路径<ul style="margin-left:8px"><li>真实路径<ul style="margin-left:8px"><li>fs.realpath(path[, cache], callback)</li><li>fs.realpathSync(path[, cache])</li></ul></li><li>相对路径<ul style="margin-left:8px"><li>process.cwd</li></ul></li></ul></li></ul></li><li>权限<ul style="margin-left:8px"><li>更改文件所有权<ul style="margin-left:8px"><li>fs.chown(path, uid, gid, callback)</li><li>fs.chownSync(path, uid, gid)</li></ul></li><li>更改文件所有权(文件描述符)<ul style="margin-left:8px"><li>fs.fchown(fd, uid, gid, callback)</li><li>fs.fchownSync(fd, uid, gid)</li></ul></li><li>更改文件所有权（不解析符号链接）<ul style="margin-left:8px"><li>fs.lchown(path, uid, gid, callback)</li><li>fs.lchownSync(path, uid, gid)</li></ul></li><li>改写文件的读写权限<ul style="margin-left:8px"><li>fs.chmod(path, mode, callback)</li><li>fs.chmodSync(path, mode)</li></ul></li><li>更改文件权限（文件描述符）<ul style="margin-left:8px"><li>fs.fchmod(fd, mode, callback)</li><li>fs.fchmodSync(fd, mode)</li></ul></li><li>检查到指定path路径的目录或文件的访问权限<ul style="margin-left:8px"><li>fs.access(path[, mode], callback)</li><li>fs.accessSync(path[, mode])</li></ul></li></ul></li><li>操作文件<ul style="margin-left:8px"><li>监视 filename 文件的变化<ul style="margin-left:8px"><li>fs.watchFile(filename[, options], listener)</li><li>fs.unwatchFile(filename[, listener])</li></ul></li><li>观察 filename 指定的文件或文件夹的改变<ul style="margin-left:8px"><li>fs.watch(filename[, options][, listener])   </li></ul></li><li>文件或文件夹重命名<ul style="margin-left:8px"><li>fs.rename(oldPath, newPath, callback)</li><li>fs.renameSync(oldPath, newPath)</li></ul></li></ul></li><li>文件读写流<ul style="margin-left:8px"><li>创建readStream（文件读取流，输入流）对象<ul style="margin-left:8px"><li>fs.createReadStream(path[, options])</li></ul></li><li>创建WriteStream（输出流）对象（可写流）<ul style="margin-left:8px"><li>fs.createWriteStream(path[, options])</li></ul></li></ul></li><li>fs 常量<ul style="margin-left:8px"><li>文件访问常量<ul style="margin-left:8px"><li>F_OK<ul style="margin-left:8px"><li>该标志表明文件对于调用进程是可见的</li></ul></li><li>R_OK<ul style="margin-left:8px"><li>标志表明文件可被调用进程读取</li></ul></li><li>W_OK<ul style="margin-left:8px"><li>该标志表明文件可被调用进程写入</li></ul></li><li>X_OK<ul style="margin-left:8px"><li>该标志表明文件可被调用进程执行</li></ul></li></ul></li><li>文件打开常量<ul style="margin-left:8px"><li>O_RDONLY<ul style="margin-left:8px"><li>该标志表明打开一个文件用于只读访问</li></ul></li><li>O_WRONLY<ul style="margin-left:8px"><li>该标志表明打开一个文件用于只写访问</li></ul></li><li>O_RDWR<ul style="margin-left:8px"><li>该标志表明打开一个文件用于读写访问</li></ul></li><li>O_CREAT<ul style="margin-left:8px"><li>该标志表明如果文件不存在则创建一个文件</li></ul></li><li>O_EXCL<ul style="margin-left:8px"><li>该标志表明如果设置了 O_CREAT 标志且文件已经存在，则打开一个文件应该失败</li></ul></li><li>O_NOCTTY<ul style="margin-left:8px"><li>该标志表明如果路径是一个终端设备，则打开该路径不应该造成该终端变成进程的控制终端（如果进程还没有终端）</li></ul></li><li>O_TRUNC<ul style="margin-left:8px"><li>该标志表明如果文件存在且为一个常规文件、且文件被成功打开为写入访问，则它的长度应该被截断至零</li></ul></li><li>O_APPEND<ul style="margin-left:8px"><li>该标志表明数据会被追加到文件的末尾</li></ul></li><li>O_DIRECTORY<ul style="margin-left:8px"><li>该标志表明如果路径不是一个目录，则打开应该失败</li></ul></li><li>O_NOATIME<ul style="margin-left:8px"><li>该标志表明文件系统的读取访问权不再引起相关文件 atime 信息的更新。该标志只在 Linux 操作系统有效</li></ul></li><li>O_NOFOLLOW<ul style="margin-left:8px"><li>该标志表明如果路径是一个符号链接，则打开应该失败</li></ul></li><li>O_SYNC<ul style="margin-left:8px"><li>该标志表明文件打开用于同步 I/O</li></ul></li><li>O_SYMLINK<ul style="margin-left:8px"><li>该标志表明打开符号链接自身，而不是它指向的资源</li></ul></li><li>O_DIRECT<ul style="margin-left:8px"><li>	当设置它时，会尝试最小化文件 I/O 的缓存效果</li></ul></li><li>O_NONBLOCK<ul style="margin-left:8px"><li>该标志表明当可能时以非阻塞模式打开文件</li></ul></li></ul></li><li>文件类型常量<ul style="margin-left:8px"><li>S_IFMT<ul style="margin-left:8px"><li>用于提取文件类型码的位掩码</li></ul></li><li>S_IFREG<ul style="margin-left:8px"><li>表示一个常规文件的文件类型常量</li></ul></li><li>S_IFDIR<ul style="margin-left:8px"><li>	表示一个目录的文件类型常量</li></ul></li><li>S_IFCHR<ul style="margin-left:8px"><li>表示一个面向字符的设备文件的文件类型常量</li></ul></li><li>S_IFBLK<ul style="margin-left:8px"><li>表示一个面向块的设备文件的文件类型常量</li></ul></li><li>S_IFIFO<ul style="margin-left:8px"><li>表示一个 FIFO/pipe 的文件类型常量</li></ul></li><li>S_IFLNK<ul style="margin-left:8px"><li>表示一个符号链接的文件类型常量</li></ul></li><li>S_IFSOCK<ul style="margin-left:8px"><li>表示一个 socket 的文件类型常量</li></ul></li></ul></li><li>文件模式常量<ul style="margin-left:8px"><li>S_IRWXU<ul style="margin-left:8px"><li>该文件模式表明可被所有者读取、写入、执行</li></ul></li><li>S_IRUSR<ul style="margin-left:8px"><li>该文件模式表明可被所有者读取</li></ul></li><li>S_IWUSR<ul style="margin-left:8px"><li>该文件模式表明可被所有者写入</li></ul></li><li>S_IXUSR<ul style="margin-left:8px"><li>该文件模式表明可被所有者执行</li></ul></li><li>S_IRWXG<ul style="margin-left:8px"><li>	该文件模式表明可被群组读取、写入、执行</li></ul></li><li>S_IRGRP<ul style="margin-left:8px"><li>该文件模式表明可被群组读取</li></ul></li><li>S_IWGRP<ul style="margin-left:8px"><li>该文件模式表明可被群组写入</li></ul></li><li>S_IXGRP<ul style="margin-left:8px"><li>该文件模式表明可被群组执行</li></ul></li><li>S_IRWXO<ul style="margin-left:8px"><li>该文件模式表明可被其他人读取、写入、执行</li></ul></li></ul></li></ul></li></ul></li></ul></code></pre>
