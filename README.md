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

# 字幕网站推荐
[转自http://tieba.baidu.com/p/3542963142](http://tieba.baidu.com/p/3542963142)</br>
射手倒了，但射手的字幕数据没有消失，相反感谢射手站长无私举动，分享了射手建站15年来积累的字幕数据，总共75G，将近20万份字幕文件。</br>
随后，有几家字幕网站纷纷宣布，成功导入射手网字幕数据。所以，字幕网站百家齐放、百家争鸣的时代来临。仅介绍几家名气较大的。</br>

1. SubHD：http://subhd.com/ ，继承射手网数后，目前做的最好的一家。界面简洁、功能齐全，还支持上传功能。目前各大字幕小组也都入住，还有许多热心网友，每天有许多新鲜字幕上传，从而新翻译电影、美剧等字幕也可以搜到。</br>
2. Subom：http://www.subom.net/ ，这家以前就做字幕服务，下载时提供跳转到射手。现在也导入射手数据，但以前抓取射手数据时不完整，导致部分字幕无法下载。并且，不提供上传功能，无法及时更新字幕。</br>
3. 伪射手：http://sub.makedie.me/ ，该站从界面到底部链接，完全仿照原射手。但中文搜索是软肋，如何通过中文搜索，索引到原射手字幕数据，可能还没有完全解决。英文搜索完美。支持字幕上传功能。</br>
4. 字幕库，http://www.zimuku.net/ ，据说全部字幕均由站长一人上传，所以字幕质量有保障。</br>
5. OpenSubtitles：http://www.opensubtitles.org/zh ，国际知名字幕网站，提供各国语言的字幕下载。射手关站后，OpenSubtitles站长表示，会导入射手字幕数据。但中文搜索功能、界面的水土不服，还是让人难以寄予厚望。</br>
6. A7美剧字幕站：http://www.addic7ed.com/ ，人人影视关站页面推荐过的站，各国字幕都有。无中文界面。</br>
7. Subscene，http://subscene.com/ ，国外字幕站，国语字幕大部是港台影迷上传，所以繁体字幕为主，可以转换为简体字幕后使用。具体使用教程可以参照“Subscene使用方法”一文。</br>
8. 还有国内几个名气较大的美剧字幕组论坛：</br>
破烂熊字幕组（国内老牌美剧字幕组之一）:： http://www.ragbear.com/</br>
伊甸园字幕组（国内老牌美剧字幕组之一）：http://bbs.sfile2012.com/index.php</br>
风软字幕组（国内老牌美剧字幕组之一）：http://www.1000fr.net/</br>

总结：以SubHD为主，尤其是最新电影、美剧的字幕；Subom、伪射手等作为备用。需要国外字幕，如英、法等语种字幕，可以上OpenSubtitles、Subscene。美剧迷还可以破烂熊字幕组、伊甸园字幕组、风软字幕组等论坛。</br>
