# Google form + Line notify
## _ตั้งเวลาแจ้งเตือนผ่าน Linenotify_

## 1.สร้างฟอร์ม
สร้าง google form ตามตัวอย่างด้านล่าง
![1](/media/1-form.png)
## 2.Setting Response
เลือกแทบ การตอบกลับ  >> ดูในชีต
![2](/media/2-response.png)
เลือกเมนู ส่วนขยาย >> Apps Script
![3](/media/3-appscript.png)
กดปุ่ม เพิ่ม สคริปต์
![4](/media/4-addscript.png)
## 3.Add Script
นำโค้ดชุดนี้ไปวาง แล้วบันทึก
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

#### 4.เปลี่ยนค่าตัวแปร xxx ทั้งหมด 3 จุด ดังนี้

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
ให้ล็อคอิน https://notify-bot.line.me/ แล้วสร้าง token ใหม่
![5](/media/5-linetoken.png)
Copy google sheet ID จากจุดนี้ ตามตัวอย่าง
![6](/media/6-sheetid.png)
Copy google sheet name จากจุดนี้ ตามตัวอย่าง
![7](/media/7-sheetname.png)
## 5.Run
บันทึก แล้วกด เรียกใช้ หากสำเร็จจะต้องไม่แสดง error ตามตัวอย่าง
![8](/media/8-run.png)
## 6.Trigger
เลือกแทบทางซ้าย ทริกเกอร์ แล้ว กดปุ่ม เพิ่มทริกเกอร์ใหม่
![9](/media/9-trigger.png)
เลือกการตั้งค่าทริกเกอร์ตามตัวอย่าง เพื่อให้ระบบทริกเกอร์ทุกนาที แล้วกดบันทึก เรียบร้อย
![10](/media/10-daily.png)
## ตัวอย่าง
ทดลอง กรอกบันทึกการแจ้งเตือน
![11](/media/11-addnew.png)
หากถึงเวลาแจ้งเตือน จะได้รับไลน์แจ้งเตือน ดังนี้
![12](/media/12-notify.png)