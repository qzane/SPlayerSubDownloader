# SPlayerSubDownloader
Download subtitles using SPlayer API

# Usage
``` sh
python SPlayerSub.py movie.mkv
```

# Reference
https://docs.google.com/document/d/1ufdzy6jbornkXxsD-OGl3kgWa4P9WO5NZb6_QYZiGI0/preview
https://docs.google.com/document/d/1w5MCBO61rKQ6hI5m9laJLWse__yTYdRugpVyz4RzrmM/preview

射手影音智能字幕查询API（JSON）</br>
功能: 根据客户端提交的信息，返回匹配字幕信息和字幕数据包</br>
通讯协议: HTTP POST</br>
接口地址: https://www.shooter.cn/api/subapi.php</br>
上行参数:</br>

|参数名称|类型|含义|备注|示例|
|--------|----|----|----|----|
|filehash|String|视频文件的hash码|必填|参考“文件Hash算法协议”|
|pathinfo|String|视频文件的所在路径名和文件名|必填|D:/Matrix.720p.HDTV.X264-DIMENSION/md.mkv|
|format|String|结果输出格式，值必须为 json|必填|json|
|lang|String|字幕语言|可选|eng或者chn|

注：参数均以标准的RFC 3986协议编码传送。即以文字的UTF8值进行URLEncode编码传送。例如“中国”，将以“%E4%B8%AD%E5%9B%BD”传送。

下行数据:</br>
尚不能提供字幕时将返回一个字节，值为0xff(-1)。</br>
可提供字幕时会返回json格式的Subinfo结构的Array数据，结构如下：</br>

``` c
type Fileinfo struct {
  Ext    string // 文件扩展名
  Link   string // 文件下载链接
}
type Subinfo struct {
  Desc   string // 备注信息
  Delay  int32  // 字幕相对于视频的延迟时间，单位是毫秒
  Files  []Fileinfo  // 包含文件信息的Array。 注：一个字幕可能会包含多个字幕文件，例如：idx+sub格式
}
```

SVPlayer视频文件hash算法 （草）</br>
Version. 0.1    Date: 2008-11-04</br>
方案</br>
取文件第4k位置，再根据floor( 文件总长度/3 )计算，取中间2处，再取文件结尾倒数第8k的位置， 4个位置各取4k区块做md5。共得到4个md5值，均设为索引。可以进行智能匹配。 （可以应用于不完全下载的p2p文件）
