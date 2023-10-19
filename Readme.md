# Google form + Line notify
## _ตั้งเวลาแจ้งเตือนผ่าน Linenotify_

## สร้างฟอร์ม
![google form]()
## Add Script
```sh
function myFunction() {
  var token = 'xxx'
  var ss = SpreadsheetApp.openById('xxx')
  var sh = ss.getSheetByName('xxx')
  var row = sh.getLastRow();

  var today = Utilities.formatDate(new Date(), "Asia/Bangkok", "dd/MM/yyyy")
  var time = Utilities.formatDate(new Date(), "Asia/Bangkok", "HH:mm")

  for (i = 2; i <= row; i++) {
    var date = Utilities.formatDate(sh.getRange(i, 2).getValue(), "Asia/Bangkok", "dd/MM/yyyy")
    Logger.log("date : "+ date)
    var timer = Utilities.formatDate(sh.getRange(i, 3).getValue(), "Asia/Bangkok", "HH:mm")
    Logger.log("time : "+ timer)
    if (today == date && time == timer) {
      var msg = sh.getRange(i, 4).getValue() + '\n'
      var message = '\n แจ้งเตือน : ' + msg
      sendLineNotify(message, token)
    }
  }
}

function sendLineNotify(message, token) {
  var options = {
    "method": "post",
    "payload": {
      "message": message
    },
    "headers": { "Authorization": "Bearer " + token }
  };
  UrlFetchApp.fetch("https://notify-api.line.me/api/notify", options);
}
```

#### เปลี่ยนค่าตัวแปร xxx ทั้งหมด 3 จุด ดังนี้

```sh
1. line token
var token = 'xxx'
```

```sh
2. Sheet id
var ss = SpreadsheetApp.openById('xxx')
```

```sh
3. Sheet name
var sh = ss.getSheetByName('xxx')
```
## Trigger

## ตัวอย่าง
