<center>
	<img src="https://s2.ax1x.com/2019/11/11/MMGWyn.png" width="100" />
</center>
 
<center>
	<font size=6>Siphan</font>
</center>

# 入门 

> 当前最新版本`V0.0.3` 

## 何为 Siphan

`Siphan`是一个轻量的JS前端字符串数据加密库

对于一些不适于直接公开的数据，`Siphan`可以将字符串数据，转为一个加密字符串和一个密钥，做到一分为二，从而可以分开保管。

轻量|支持同/异步|格式接口|表格化方案
:--:|:--:|:--:|:--:
全部引入只有6KB|直接调用函数进行操作|支持所有类型字符加密解密|采用83个字符随机排序
导入很轻松|轻便无压力|解码无意外|安全有保障

## 安装Siphan

```bash
npm install siphan@latest --save
```

也可以直接在浏览器内引入

```HTML
<script type="text/javascript" src="//unpkg.com/siphan@0.0.3/dist/bundle.js"></script>
```

## 快速开始

安装`Siphan`之后，就可以立即使用功能。

```javascript
import siphan from 'siphan'
const text = "这是测试的字符串";
const encryptedText = siphan.encryptUTF8(text);
console.log(encryptedText);
```

# 教程

## 全局变量

### name

`name`是`Siphan`的类名，以用于`Siphan`被其他库引用，其默认是`siphan`。

### version

`version`是`Siphan`的版本号。

## 全局方法

### new

`new`返回一个新的`Siphan`类，一般不需要使用，因为`Siphan`并不需要一个副本类。

```javascript
import siphan from 'siphan'
const newSiphan = siphan.new();
/* newSiphan具有和siphan相同的功能 */
```

但是请求的副本类不在有`new`方法

```javascript
const newSiphan = siphan.new();
/* newSiphan没有绑定new方法 */
console.log(newSiphan.new); // undefined
```

## 加密方法

### newKey

`newKey`返回一个随机的合法密钥。

```javascript
import siphan from 'siphan';

const key = siphan.newKey();
console.log(key);
/*
*	key = ^qxE%chPku;L#nzQbt1wj,RC!MH89lvU.O=op-&IF@+ 3YKWDmg52|VJasBS:7fTAriGNd60y>?$<X/*4Z_e
*/
```

### encrypt

`encrypt(String)`返回一个新的加密后的对象，储存了加密结果`cipher`、加密密钥`solution`和判断密钥是否随机的布尔值`random`。

!> `encrypt`必须接受一个ASCII字符，不能接受中文或其他字符，否则需要使用`encryptUTF8`方法。

```javascript
import siphan from 'siphan';

const text = "test_string";
const encryptedText = siphan.encrypt、(text);
console.log(encryptedText);
/*
*	encryptedText是一个[object object]对象
*	{
*		cipher: 加密结果字符串,
*		solution: 加密密钥,
*		random: true
*	}
*/
```

### encryptUTF8

`encryptUTF8(String)`返回的结果和`encrypt`相同，不过`encryptUTF8`可以接收所有UNICODE字符，并进行URL转码。

```javascript
import siphan from 'siphan';

const text = "这是测试的字符串";
const encryptedText = siphan.encryptUTF8(text);
console.log(encryptedText);
/*
*	encryptedText是一个[object object]对象
*	{
*		cipher: //加密结果字符串,
*		solution: //加密密钥,
*		random: true
*	}
*/
```

### encryptAs

`encryptAs(String, solution)`返回一个新的加密后的对象，不过这个对象解密密钥就是`solution`。

!> `encryptAs`必须接受一个ASCII字符，不能接受中文或其他字符，否则需要使用`encryptUTF8As`方法。

```javascript
import siphan from 'siphan';

const text = "test_string";
const solution = "^qxE%chPku;L#nzQbt1wj,RC!MH89lvU.O=op-&IF@+ 3YKWDmg52|VJasBS:7fTAriGNd60y>?$<X/*4Z_e";
const encryptedText = siphan.encryptAs(text, solution);
console.log(encryptedText);
/*
*	encryptedText是一个[object object]对象
*	{
*		cipher: //加密结果字符串,
*		solution: solution //加密密钥,
*		random: false
*	}
*/
```

### encryptUTF8As

`encryptAs(String, solution)`返回一个新的加密后的对象，不过这个对象解密密钥就是`solution`。

!> `encryptUTF8As`可以接收所有UNICODE字符，并进行URL转码。

```javascript
import siphan from 'siphan';

const text = "这是测试的字符串";
const solution = "^qxE%chPku;L#nzQbt1wj,RC!MH89lvU.O=op-&IF@+ 3YKWDmg52|VJasBS:7fTAriGNd60y>?$<X/*4Z_e";
const encryptedText = siphan.encryptAs(text, solution);
console.log(encryptedText);
/*
*	encryptedText是一个[object object]对象
*	{
*		cipher: //加密结果字符串,
*		solution: solution //加密密钥,
*		random: false
*	}
*/
```

## 解密方法

### decrypt

`decrypt(String, solution)`返回一个新的加密后的对象，返回它的明文。

!> `decrypt`只能解密`encrypt`加密的字符串，并没有解码。

```javascript
import siphan from 'siphan';

const encryptedText = "wcjRqCM!bRb";
const solution = "^qxE%chPku;L#nzQbt1wj,RC!MH89lvU.O=op-&IF@+ 3YKWDmg52|VJasBS:7fTAriGNd60y>?$<X/*4Z_e";
const text = siphan.decrypt(encryptedText, solution);
console.log(text); // 'test_string'
```

由此我们可以得出，由`encrypt`加密的字符串得到的cipher和solution经过`decrypt`方法即可得出原文。

### decryptUTF8

`decryptUTF8(String, solution)`返回一个新的加密后的对象，返回它的明文。

!> `decryptUTF8`能解密`encryptUTF8`加密的字符串，并进行解码。

```javascript
import siphan from 'siphan';

const encryptedText = "?U?XUp44ZeFZxxxc+DkmhLmkzn|tBtjsTCCjHGMllG.U9o<.&$*@@F3q W^EgED|;ma;aSn|";
const solution = "^qxE%chPku;L#nzQbt1wj,RC!MH89lvU.O=op-&IF@+ 3YKWDmg52|VJasBS:7fTAriGNd60y>?$<X/*4Z_e";
const text = siphan.decrypt(encryptedText, solution);
console.log(text); // '这是测试的字符串'
```

### catchError

`decryptUTF8`方法进行解码的时候，可能会发生无法解码的错误，默认的情况下会中断JS，并抛出错误显示在控制台。

这时我们可以编写`catchError`函数，让其进行错误处理。

```javascript
import siphan from 'siphan';
/* catchError的回调参数是( {cipher, solution}, { type, error} ) */
siphan.catchError = (info, e) => {
	alert(e.type);
}
//Siphan在解码错误时会弹出 'decrypt_decode_url_error'
```
