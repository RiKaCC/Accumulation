## 握手过程

- Server收到CHLO
```
2019/07/01 21:26:02 server Serving new connection: 0x88c1677a196380a8, version gQUIC 44 from 127.0.0.1:33356
2019/07/01 21:26:02 server Creating new session. Destination Connection ID: (empty), Source Connection ID: 0x88c1677a196380a8
2019/07/01 21:26:02 server <- Reading packet 0x1 (1072 bytes) for connection 0x88c1677a196380a8, unencrypted
2019/07/01 21:26:02 server 	Long Header{Type: Initial, DestConnectionID: 0x88c1677a196380a8, SrcConnectionID: (empty), PacketNumber: 0x1, PacketNumberLen: 4, Version: gQUIC 44}
2019/07/01 21:26:02 server 	Queueing ACK because the first packet should be acknowledged.
2019/07/01 21:26:02 server 	<- &wire.StreamFrame{StreamID: 1, FinBit: false, Offset: 0x0, Data length: 0x410, Offset + Data length: 0x410}
2019/07/01 21:26:02 server Got CHLO:
	SNI : "localhost"
	VER : "\x00\x00\x00\x00"
	CCS : "\x01\xe8\x81`\x92\x92\x1a\xe8~퀆\xa2\x15\x82\x91"
	PDMD: "X509"
	ICSL: "\x1e\x00\x00\x00"
	MIDS: "d\x00\x00\x00"
	CFCW: "\x00\xc0\x00\x00"
	SFCW: "\x00\x80\x00\x00"
	PAD : (911 bytes)
2019/07/01 21:26:02 server Sending REJ :
	STK : "HMʵ\xdc\xc7\x18:fk.\xc4\xcf )l\x82\xef\x9a[g\xbb\xb2V\\\xaa\t\xe9\xc1\xb7\xb6}5\x8a\xc6=\xf1\xbd\xe3\xdeC\x06}V\b\xba{$\xa4\xe6\xe0ð\xa38?\xbb1pty\x9b{"
	SVID: "quic-go"
	SCFG: "SCFG\x06\x00\x00\x00AEAD\x04\x00\x00\x00SCID\x14\x00\x00\x00PUBS7\x00\x00\x00KEXS;\x00\x00\x00OBITC\x00\x00\x00EXPYK\x00\x00\x00AESG7\xbf\x87ҕ\xe6d\x91֡\x84H\xb3Cc\xba \x00\x00\xdex!w\xc4,m\xa2z\xb9X\b\xae\x16GP\xaf\xb3\xea\xce\xe2tۚu\xe9}\x91EF^.C255\xb1j\x83\x95i\xdc~\xcd\xff\xff\xff\xff\xff\xff\xff\xff"
```

- Server第二次收到CHLO
```
2019/07/01 21:26:02 server Got CHLO:
	SNI : "localhost"
	STK : "HMʵ\xdc\xc7\x18:fk.\xc4\xcf )l\x82\xef\x9a[g\xbb\xb2V\\\xaa\t\xe9\xc1\xb7\xb6}5\x8a\xc6=\xf1\xbd\xe3\xdeC\x06}V\b\xba{$\xa4\xe6\xe0ð\xa38?\xbb1pty\x9b{"
	VER : "\x00\x00\x00\x00"
	CCS : "\x01\xe8\x81`\x92\x92\x1a\xe8~퀆\xa2\x15\x82\x91"
	SCID: "7\xbf\x87ҕ\xe6d\x91֡\x84H\xb3Cc\xba"
	PDMD: "X509"
	ICSL: "\x1e\x00\x00\x00"
	MIDS: "d\x00\x00\x00"
	CFCW: "\x00\xc0\x00\x00"
	SFCW: "\x00\x80\x00\x00"
	PAD : (816 bytes)
```

- Server Send REJ
```
2019/07/01 21:26:02 server Sending REJ :
	STK : "\xad\x93\xde*\xb2\x90z\xab\xb5!\xe4xG\x1c\x8b\xef\xce\xd8\x038\xbf\xb0\x03|꒛#\x9f\xfdL\xd7&\x9eIY\xf4K\xe4\u05f8\x14\xbc\xc9\x175G\xff\xd2\xf2s\xbdm\r\x96\xa1\xb4\xf0\x85\x1du \xfc"
	SVID: "quic-go"
	PROF: "41\xf9̶\x1c\xb1\xb8\x8a}\x8b\xd6\xc5\xdb\xf0\xa3D\xd7\xd298\x05\x90ؙ\xce\xc15\x02\xa3\x17T]\xabN\x15c\xe7\xe6A\x97B1\xa8\x97dd\x17a\x8d(`\xb3ע,\x1aUC\x9d\xcc\xe1V\x82\x18\xe6\xf4\xf2\xe7wS\xdcF\x99\x86\x1dm\xa9*d\xae\xf6)\xfc$n\xc3\x00\xaf\x14\xa7\x8c\f\xb3\xba\xb4j^\xcev\x9d\xb9Z\"2\x97%j%zr\xc9YN/g\xe7\xd1\xcc\xf5)\xd9\xefN\x1d\xff\xb50y\xd8\r\xf5\x01h\xad\xbae\xd8\x1e͝\xde\xf1\x12\x14\xff\x9e\x1d\xc8\xfa\xb57+m,\x8ae\x93\xa9V\xc2\xe0oF\xecv\x02廕\xbf[\x9c\xf8ʗ\x9a\xdaR%ce\x18\xc6\xde\xf4\xf5\xc9\x12?\xeeũ\xa7z\x01*\x89д\x86ԁ\xcc}\xd5S2S\x83x\xb3\v\xdf`4ʔ\xfa\\\x97n\xef\xca¢(\xb6\xb9\x9aP\xb9\x14\xb7\xe5Tpq\x96X_\xbcc`[ T\x86\x84\x0e$\x96>l`~"
	SCFG: "SCFG\x06\x00\x00\x00AEAD\x04\x00\x00\x00SCID\x14\x00\x00\x00PUBS7\x00\x00\x00KEXS;\x00\x00\x00OBITC\x00\x00\x00EXPYK\x00\x00\x00AESG7\xbf\x87ҕ\xe6d\x91֡\x84H\xb3Cc\xba \x00\x00\xdex!w\xc4,m\xa2z\xb9X\b\xae\x16GP\xaf\xb3\xea\xce\xe2tۚu\xe9}\x91EF^.C255\xb1j\x83\x95i\xdc~\xcd\xff\xff\xff\xff\xff\xff\xff\xff"
	CRTÿ: "\x01\x00\x95\x02\x00\x00x\xf9\"Q~\xb1\x9a\xc8\xc4\xc0`\xd0\xc4\xd4k\xd0\xc4X\xba\x80\x99\x89\x91\x89\x89\x11\xa5\x1c\xe2fe0`\x00g^K\x03s\x03CCc#SS\x93(8\xd7\x04\xcc5` X(\x9e\xdc\xce\xde\x1b\xde\xffo\xa5\xe9Js\xe17\xeb$\xbb\xae\x9f_]]\xed\xf7\xfe\xfb?\xafI\x9c;\x03N\xfc\xffT\x11\xc8s}\x8e^\u007f\xcf\x1f\xfd\xf7\xedۢ\xf4.\x1f\xf2\n\xae\x9f\x9dݳ\xff\xcbm\r\xfdiy\\Y\u007f\f\xff1\xed\xbcf\xb6Q\xcdv\xf3\x1b\xdfo\xbf4\x9e<z\x1f]\xb3\xfa\x89\xc57\rװ\x8b\x0f\xea\xf3\x92\xa7,\x0f\xbd\xbdx\xf5\xadMw\x9f\xce\fhK\xe1\u007f\xa5:\xd7\xe6\xa6\xfd\x1f\xe1\xdc\xc3\xc5l\a\x8b\xd4}[\xd5\xec_mu\x9eښ\xa6\xf1f\xc9\xed\xf2E\xdb\x0fMz\xb6H\xaelM\x13K\xf83\xcfW\xed\x9f\xfb6\xadQ+o\r?\xe0\xfb\xd0\xc4\xf5\x8b\xfc\xed\xd91\xaf\xef\xdc\xf9\xb7\xcc\xed\xd8\xf2\xf3\xbb\xd6\xf7\x14\xf0\xf8f\xac\xf9\xb0\xaa\xe5ӫ\xc4T\x9eW_vUMx\xf3.\xed\xd0E\xe6\xce#/\xb9^~x\x91\xd2\x18\xf7dֳm3_3F-\xf3\xf4\x9a\xf4\xdboB\x02\xeb\xfb\xce7\xbb\xf5D\u007f\xed\xdb\xf2ݼky䝍Ľ\f\x8c\x8b\x85\f\x04P\x8b,\xa6\x16\xb4\xc0\x06W5y\xf3>\xa5e\x1e\xb2\xb2\x9c\x98\x94\xe4[ڷog\xea\uf737\xb3\xf6\xcd\u007f\x16\xb3\x99E\xe2\xf8\xca99\x8a\x8b\xf2\xfdV\xbf*\xd0?\xd3rfAT\xf8\xf9\xe0#&\x87o\\\xbe\xc3\x17\xd2{l\x9as\x9e\x86\xa2\xbbZ\xf5\xb9#k?\xe5Ɇ\xe5\xbeH\xd1ۮ\xbdԵ\xeeŪ\x8fm缫U\x92En\xa9\xad\xf0\xc98Z\xfe\xf3\x94\x95\xe9\xff\x0foy\xe7D\xbd\\/\xb3\xd7p\xf1qu\xc5m\xbdK\fWp\xddخ\xbb٣y\xa5\xf6K\x8f\x82\x98\x9c\x1d\x1ca1g\xbf\x9a]{\xe2\xc98\x8b'\xbf~[\x9a\xfd۾\x1f\x8f\xd8\"5\xae\xec\xf9\xddQWj{\xfdY\xbeƳ\xdcȦ9\xdaS\x996\xcb\x1f\x91~S\xff1w\x9f־\x94f\x1e\xd3\x1f\x92\x02\x8fD\xd6q\x85\xfd\xfc\xc6v\xf9\x8e\xa7ߎ,e\x1d+\xbb\x19\xf6\x0fb\x02\xfb\xa6Oo]\xd2%8\xc3R\xf6\xc1\xbf˾i\xf9\xb3\xaevU\xb1l\x97\xec\xb7\xd9ݸ\xa3$8\xf9\xd9\x06~@\x00\x00\x00\xff\xff`\x1f+3"
```

- Server再次收到CHLO
```
2019/07/01 21:26:02 server Got CHLO:
	SNI : "localhost"
	STK : "\xad\x93\xde*\xb2\x90z\xab\xb5!\xe4xG\x1c\x8b\xef\xce\xd8\x038\xbf\xb0\x03|꒛#\x9f\xfdL\xd7&\x9eIY\xf4K\xe4\u05f8\x14\xbc\xc9\x175G\xff\xd2\xf2s\xbdm\r\x96\xa1\xb4\xf0\x85\x1du \xfc"
	VER : "\x00\x00\x00\x00"
	CCS : "\x01\xe8\x81`\x92\x92\x1a\xe8~퀆\xa2\x15\x82\x91"
	NONC: "]\x1a\t\xea\xb1j\x83\x95i\xdc~\xcdT\xf4>C\xbc\x99\xa4\xbfZ\"\x89\x110\xea\x96:\x15\x8d\xf7\f"
	AEAD: "AESG"
	SCID: "7\xbf\x87ҕ\xe6d\x91֡\x84H\xb3Cc\xba"
	PDMD: "X509"
	ICSL: "\x1e\x00\x00\x00"
	PUBS: "@\xcdn2\xac5\xfd\xfdk\xb7\xa7]b\x9b\xbe\xbf\xf0_\xf1C$G\xdfmh[\xd1\xddO2&&"
	MIDS: "d\x00\x00\x00"
	KEXS: "C255"
	XLCT: "g|\x9a\u05f6\xd9ќ"
	CFCW: "\x00\xc0\x00\x00"
	SFCW: "\x00\x80\x00\x00"
```

- Server Create AEAD
```
2019/07/01 21:26:02 server Creating AEAD for secure encryption.
2019/07/01 21:26:02 server Creating AEAD for forward-secure encryption.
```

- Server Send SHLO
```
2019/07/01 21:26:02 server Sending SHLO:
	SNO : "\xe9H\xd5C\xaa\xb5E\x05~\xfeԪ\xfe<f=\x12\x14\x93h\xd0B&g\xbe)E\xc1\xa3\x84\x02\xcb"
	VER : "Q044Q043Q039"
	ICSL: "\x1e\x00\x00\x00"
	PUBS: "\x12\x9b-\x8e;v\x16\x15$8\xae8\xb1\xec\xca \xc6\xc8\x16R4-\x8f\xc7m\xf0\xc2z\x1f\x97\x1a\x12"
	MIDS: "d\x00\x00\x00"
	CFCW: "\x00\xc0\x00\x00"
	SFCW: "\x00\x80\x00\x00"
```

### 问题
```
Q：Server为什么收到了三次HELO？
A：一开始 client 不知道server端的信息， 所以在握手开始之前， client 端将会发送一个 “不完全的” client Hello
消息去获得server端的配置以及server端的认证信息，这个过程可能持续好几个RTT 的 “不完全的” client hello 请求，
因为在这个过程中server 也不愿意发送大量的认证信息给未验证的IP地址
```

消息说明
```
Client hello 消息含有 CHLO tag 以及以下的各种 tag／value 数值

SNI Server Name Indication (optional) RFC 5890 , 不能是 IP 地址
STK Source-address token (optional) server 给的token
PDMD Proof demand: 现在只有 x509
CCS Common certificate sets (optional): 客户端证书hash
CCRT Cached certificates (optional): 客户端缓存的server证书hash‘
VER 客户端quic 版本
XLCT 客户端期望server使用的叶结点证书的hash
不同版本的协议有不同的tag， 比如最大流的数量等
作为 CHLO 的回应， server 将会发送 SHLO 或者 rejection。 SHLO 意味着握手已经成功， rejection 包含了 client 端去构造一个完整的握手请求所需要的信息
rejection 有 REJ 标签， 同时还有以下的tag／value 数据
SCFG server config (optional):
STK Source-address token (optional)
SNO Server nonce (optional)
STTL server config 的有效期
ff545243 Certificate chain (optional)
PROF Proof of authenticity (optional)
SCFG 中包含了加密算法等其他信息
一旦client 端收到了server config， 并且已经被验证了之后，client 端可以构造一个不会失败的 CHLO 请求， 同时这个完整的请求比之前的 CHLO 多了一些 SCFG 中的 TAG, 比如加密所用的公钥等
一旦 client 端发送了完整的 CHLO 请求之后，client 接受请求和发送信息都要使用加密。
这时候client 可以自由的给server 发送数据而不用等待回复
握手成功之后， server 将会返回一个 SHLO 信息， 而且是加密过的
```
