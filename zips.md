# 压缩与解压

## 压缩

```js

//压缩文件路径(必须是完整路径)
var filePath = "/sdcard/脚本.7z";
//目录路径(必须是完整路径)
var dirPath = "/sdcard/脚本";
//压缩类型
//支持的压缩类型包括:
//  zip 7z bz2 bzip2 tbz2 tbz gz gzip tgz tar wim swm xz txz。
var type = "7z";
//压缩密码
var password = "password"

//7z加密压缩(若文件已存在则跳过)，dirPath是指要压缩的文件夹
//zips.A(type, filePath, dirPath, password)

//压缩
switch (zips.A(type, filePath, dirPath)) {
    case 0:
        toastLog("压缩成功！文件已保存为： " + filePath)
        break;
    case 1:
        toastLog("压缩结束，存在非致命错误（例如某些文件正在被使用，没有被压缩）")
        break;
    case 2:
        toastLog("致命错误")
        break;
    case 7:
        toastLog("命令行错误")
        break;
    case 8:
        toastLog("没有足够内存")
        break;
    case 255:
        toastLog("用户中止操作")
        break;
    default: toastLog("未知错误")
}

```


## 解压

```js
//压缩文件路径(必须是完整路径)
var filePath = files.path("./bonus.rar");
//目录路径(必须是完整路径)
var dirPath = "/sdcard/脚本";
//压缩密码
var password = "password"

//支持的解压缩类型包括：
// zip、7z、bz2、bzip2、tbz2、tbz、gz、gzip、tgz、tar、
// wim、swm、xz、txz以及rar、chm、iso、msi等众多格式。

//解压无加密的压缩包(若文件已存在则跳过)
//zips.X(filePath, dirPath)

//解压加密的压缩包(若文件已存在则跳过)
switch (zips.X(filePath, dirPath, password)) {
    case 0:
        toastLog("解压缩成功！请到 " + dirPath + " 目录下查看。")
        break;
    case 1:
        toastLog("压缩结束，存在非致命错误（例如某些文件正在被使用，没有被压缩）")
        break;
    case 2:
        toastLog("致命错误")
        break;
    case 7:
        toastLog("命令行错误")
        break;
    case 8:
        toastLog("没有足够内存")
        break;
    case 255:
        toastLog("用户中止操作")
        break;
    default: toastLog("未知错误")
}

```

## 7zip_commands
```js
//压缩文件路径(必须是完整路径)
var filePath = "/sdcard/脚本.7z";
//目录路径(必须是完整路径)
var dirPath = "/sdcard/脚本";
//自定义命令:7z加密压缩
var password = "password"
switch (zips.cmdExec("7z a -y -ms -t7z -p" + password + " " + filePath + " -r " + dirPath)) {
    case 0: toastLog("压缩成功！文件已保存为： " + filePath)
    case 1: toastLog("压缩结束，存在非致命错误（例如某些文件正在被使用，没有被压缩）")
    case 2: toastLog("致命错误")
    case 7: toastLog("命令行错误")
    case 8: toastLog("没有足够内存")
    case 255: toastLog("用户中止操作")
    default: toastLog("未知错误")
}

/*
7zip命令行使用参考

7z|7za 子命令 [选项] 压缩包 [文件]
子命令
a    添加
d    删除
e    解压
x    带路径解压
l    列表查看
t    测试
u    更新
选项
 -m 压缩方式
-m0=压缩算法    默认使用 lzma
-mx=数字    1~9 压缩级别
-mfb=64    number of fast bytes for LZMA = 64
-md=字典大小    设置字典大小，例如 -md=32m
-ms=on|off    是否固实压缩
-o输出目录    设置输出目录
-p密码    使用密码
-r数字    递归，使用数字定义递归子目录的深度
-sfx[模块名称]    使用自解压模块
-si    从标准输入设备读入数据
-so    将数据写入标准输出设备
-y    所有询问均回答 Yes
-w路径    设置工作目录

用法
7za <命令> [<参数开关>...] <档案名称> [<文件名称>|<@列表文件...>]
在方括号内的表达式(“[” 和 “]”之间的字符)是可选的。在书名号内的表达式(“<” 和 “>”之间的字符)是必须替换的表达式(而且要去掉括号)。
7-Zip 支持和 Windows 相类似的通配符：
“*”可以使用星号代替零个或多个字符。
“?”可以用问号代替名称中的单个字符。
如果只用*，7-Zip 会将其视为任何扩展名的全部文件。
命令
a: 添加到压缩文件
b: 基准测试
d: 从档案中删除文件
e: 解压档案文件 (无目录名称)
l: 列出压缩文件内容
t: 测试压缩文件的完整性
u: 更新文件到压缩文件中
x: 完整路径下解压文件
参数开关
-ai[r[-|0]]{@列表文件|!通配符}: 包括 压缩文件
-ax[r[-|0]]{@列表文件|!通配符}: 排除 压缩文件
-bd: 禁用百分比显示功能
-i[r[-|0]]{@列表文件|!通配符}: 包括 文件名称
-m{参数}: 设置压缩算法
-o{目录}: 设置输出目录
-p{密码}: 设置密码
-r[数字]: 递归子目录，使用数字定义递归子目录的深度
-scs{UTF-8 | WIN | DOS}: 设置列表文件的编码格式
-sfx[{名称}]: 创建 SFX 自解压文件
-si[{名称}]: 从stdin(标准输入设备)读取数据
-slt: 为 l (列表) 命令显示技术信息
-so: 将数据写入stdout(标准输出设备)
-ssc[-]: 设置大小写区分模式
-ssw: 压缩共享文件
-t{类型}: 设置压缩文件格式，(7z, zip, gzip, bzip2 or tar. 7z是默认的格式)
-v{大小}[b|k|m|g]: 分卷压缩
-u[-][p#][q#][r#][x#][y#][z#][!新建档案_名称]: 更新选项
-w[{路径}]: 指定工作目录，路径为空时代表临时文件夹目录
-x[r[-|0]]]{@列表文件|!通配符}: 排除 文件名称
-y: 所有询问选是
使用'-m'参数，即设置压缩算法时，有许多选项可以进行设置。
-m<压缩方式>，关于它有许多参数及指令，这里仅介绍简单且常用的用法。
-m0=<压缩算法> 默认使用 lzma
-md=<字典大小> 设置字典大小,例如 -md=32m
-mfb=64 number of fast bytes for LZMA = 64
-ms=<on|off> 是否固实压缩
-mx=<1~9> 压缩级别

退出代码
0 ：正常，没有错误
1 ：警告，没有致命的错误，例如某些文件正在被使用，没有被压缩
2 ：致命错误
7 ：命令行错误
8 ：没有足够的内存
255 ：用户中止了操作
*/
```

```js
// 准备工作，创建文件夹与文件，以便后续用于压缩
// 创建两个文件夹与三个文件
$files.create("/sdcard/脚本/zip_test/");
$files.create("/sdcard/脚本/zip_out/");
$files.write("/sdcard/脚本/zip_test/1.txt", "Hello, World");
$files.write("/sdcard/脚本/zip_test/2.txt", "GoodBye, World");
$files.write("/sdcard/脚本/zip_test/3.txt", "Auto.js Pro");

// 1. 压缩文件夹
// 要压缩的文件夹路径
let dir = '/sdcard/脚本/zip_test/';
// 压缩后的文件路径
let zipFile = '/sdcard/脚本/zip_out/未加密.zip';
$files.remove(zipFile);
$zip.zipDir(dir, zipFile);

// 2.加密压缩文件夹
let encryptedZipFile = '/sdcard/脚本/zip_out/加密.zip';
$files.remove(encryptedZipFile);
$zip.zipDir(dir, encryptedZipFile, {
    password: 'Auto.js Pro'
});

// 3. 压缩单个文件
let zipSingleFie = '/sdcard/脚本/zip_out/单文件.zip'
$files.remove(zipSingleFie);
$zip.zipFile('/sdcard/脚本/zip_test/1.txt', zipSingleFie);

// 4. 压缩多个文件
let zipMultiFile = '/sdcard/脚本/zip_out/多文件.zip';
$files.remove(zipMultiFile);
let fileList = ['/sdcard/脚本/zip_test/1.txt', '/sdcard/脚本/zip_test/2.txt']
$zip.zipFiles(fileList, zipMultiFile);

// 5. 解压文件
$zip.unzip('/sdcard/脚本/zip_out/未加密.zip', '/sdcard/脚本/zip_out/未加密/');

// 6. 解压加密的zip
$zip.unzip('/sdcard/脚本/zip_out/加密.zip', '/sdcard/脚本/zip_out/加密/', {
    password: 'Auto.js Pro'
});

// 7. 从压缩包删除文件
let z = $zip.open('/sdcard/脚本/zip_out/多文件.zip');
z.removeFile('1.txt');

// 8. 为压缩包增加文件
z.addFile('/sdcard/脚本/zip_test/3.txt');
```