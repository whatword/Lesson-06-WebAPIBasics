# WebAPI Basics

and something useful

# Who am I

worked in SkyWatch (video streaming, IoT)

working in Mozilla's performance team

open source contributor (FFmpeg, Tornado, HIME ... etc.)

enjoying making tools to solve my own problems

[more ...](https://about.me/legnaleurc)

# HTML

HyperText Markup Language

描述網頁的結構

```html
<div id="example-id">
  <p class="example-class">
    <button>Text</button>
  </p>
</div>
```

# CSS

Cascading Style Sheet

描述網頁的外觀

```css
#example-id .example-class > button {
  font-size: 16px;
  height: 300px;
  color: red;
}
```

# DOM

Document Object Model

連接 JavaScript 與頁面內容的橋樑

用 JavaScript 操作網頁就靠它了

# Global Object

特殊的全域物件

在瀏覽器裡叫 window

所有其他的全域變數都會自動成為 window 的屬性

# 常用物件

```javascript
// 瀏覽器相關資訊
navigator;
// URL 相關
location;
// 歷史記錄
history;
// 存取網頁內容
document;
```

# 選取元素

CSS selector

```javascript
var btn = document.querySelector('#example-id .example-class > button');
```

# 事件

發生這件事情的時候做那個動作

```javascript
btn.addEventListener('click', function (event) {
  console.info('hello!');
});
```

# 除錯

* Web Console
* debugger statement
* console API

# Web Console

* Inspector: 觀察並修改元素
* Console: 詳細訊息，動態執行程式
* Debugger: 除錯工具

# debugger statement

寫死在 code 裡

沒開 Console 就不會停

```javascript
var i = 0;
debugger; // 會停在這裡
i = i + 1;
```

# console API

* 印除錯訊息
* 計時

沒看 Console 就看不到

# WebAPP 結構

manifest.webapp

```javascript
{
  "name": "hello_world",
  "description": "A Hello World app",
  "launch_path": "/index.html",
  "icons": {
    "128": "/icons/icon128x128.png"
  },
  "developer": {
    "name": "Your name",
    "url": "http://example.com"
  },
  "type": "privileged",
  "permissions": {
    "systemXHR": {
      "description": "Required to load remote content"
    }
  }
}
```

# WebAPI 權限

* non-privileged
  * Hosted or Packaged
* privileged
  * Packaged **and** Signed (i.e. from marketplace)
* certified required
  * (官方特權)

# WebAPI 功能

* Communication
* Hardware Access
* Data Management
* Other

See Also: [MDN](https://developer.mozilla.org/en-US/docs/WebAPI)

# Communication API

TCPSocket

```javascript
"permissions" : {
  "tcp-socket" : {
    "description" : "Create TCP sockets and communicate over them."
  }
}
```
```javascript
// 連到 localhost:80
var socket = navigator.mozTCPSocket.open('localhost', 80);
// 監聽 localhost:8080 (一定要大於 1024)
var socket = navigator.mozTCPSocket.listen(8080);
```

# TCPSocket

```javascript
// receive
socket.ondata = function onDataReceived (event) {
  if (typeof event.data === 'string') {
    console.info('Get a string: ' + event.data);
  } else {
    console.info('Get a Uint8Array');
  }
};
// send
function pushData() {
  // Do stuff that retrieves data
  var data = getData();
  // Fill the buffer as much as you can
  while(data != null && socket.send(data));
}
// Resume pushing data when the buffer is flushed
socket.ondrain = pushData;
// Start pushing data
pushData();
```

# Hardware Access API

* Ambient Light Sensor API
* Battery Status API
* Geolocation API
* Pointer Lock API
* Proximity API
* Device Orientation API
* Screen Orientation API
* Vibration API
* WebFM API
* Camera API

# Example: Device Orientation API

```javascript
window.addEventListener("deviceorientation", function (event) {
  var absolute = event.absolute;
  var yaw      = event.alpha;
  var roll     = event.beta;
  var pitch    = event.gamma;
});
```

# Data Management API

* FileHandle API
* IndexedDB API
* Contacts API
* Device Storage API

# IndexedDB API

* IndexedDB databases store key-value pairs
* IndexedDB is built on a transactional database model
* The IndexedDB API is mostly asynchronous

# IndexedDB API

```javascript
// the number is your schema version, so you can do migration if needed
var dbRequest = indexedDB.open('your_database_name', 1);

dbRequest.onupgradeneeded = function (event) {
  // first time or version upgraded
};

dbRequest.onsuccess = function (event) {
  // do actions after success
};

dbRequest.onerror = function (event) {
  // handle error
};
```

# IndexedDB API

```javascript
dbRequest.onupgradeneeded = function (event) {
  var theDB = event.target.result;

  // Object Stores is like "tables" in RDB
  if (theDB.objectStoreNames.contains('my_table')) {
    theDB.createObjectStore('my_table', {
      keyPath: 'name',
    });
  }
};
```

# IndexedDB API

```javascript
var transaction = db.transaction(['my_table'], 'readwrite');
var table = transaction.objectStore('my_table');

var request = table.add({
  name: 'John',
});

request.onerror = function (event) {
  console.warn(event.target.error.name);
}

request.onsuccess = function (event) {
  // ok
}
```

# IndexedDB API

```javascript
var request = table.get('John');

request.onerror = function (event) {
  console.warn(event.target.error.name);
}

request.onsuccess = function (event) {
  var result = event.target.result;
  console.info(result);
}
```

# Other API

* Alarm API
* Simple Push API
* Web Notifications
* Apps API
* Web Activities
* WebPayment API
* Browser API

# Web Notifications

```javascript
Notification.requestPermission(function (permission) {
  // check permission
});

var n = new Notification('Your Title', {
  body: 'Message Body',
  icon: 'img/your_image_path.png',
});
setTimeout(function () {
  n.close();
}, 5000);
```

# Q&A

# Assignment

寫一個時鐘 app:

1. 顯示現在時間(年月日時分秒) (30%)
  1. 星期 (+5%)
  2. 時區 (+5%)
2. 碼表計時(啟動, 暫停, 歸零) (30%)
3. 設定鬧鈴, 可在指定時間響鈴 (40%)

# Turn In

在 Github 開 pull request

標題請包含學號
