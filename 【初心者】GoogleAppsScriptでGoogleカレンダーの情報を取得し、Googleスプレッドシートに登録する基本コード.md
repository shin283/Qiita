
## シートの説明
* Configシート
    * カレンダーデータ取得に必要な情報が記載されているシート
* Dataシート
    * 取得したカレンダーデータを登録するシート

#### Configシート
以下のようにカレンダー取得に必要な情報を登録する。
B列の情報を利用します。

|  | A | B | C |
|:-:|:-:|:-:|:-:|
| 1 | id（データ取得する対象） | abc＠gmail.com | |
| 2 | startTime（いつから） | 2020/05/01 | |
| 3 | endTime（いつまで） | 2020/05/31 | |


## コード
```javascript
function getCalList() {

  // get Config
  var sheet = SpreadsheetApp.getActive().getSheetByName('Config');

  // get id(mail address)
  var id = sheet.getRange('B1').getValue();

  // get startdate
  var startTime = sheet.getRange('B2').getValue();

  // get enddate
  var endTime = sheet.getRange('B3').getValue();


  // get Output sheet
  var outputsheet = SpreadsheetApp.getActive().getSheetByName('Data');

  
  // get Calendar events.
  // 参考）https://tonari-it.com/gas-calendar-getevents/
  var cal = CalendarApp.getCalendarById(id);
  var events = cal.getEvents(startTime, endTime);
  
  // 取得したカレンダーデータの展開（レコード化）
  var eventList = [];
  for(const event of events){
    eventrecord = [
      event.getTitle(),
      event.getDescription(),
      event.getStartTime(),
      event.getEndTime()
    ];
    eventList.push(eventrecord);
    console.log(event.getTitle());
  }
  
  outputsheet.getRange(2, 1, eventList.length, eventList[0].length).setValues(eventList);
  
}
```

## 参考
[Google Apps Scriptで特定月のカレンダーのイベント情報を取得する](https://tonari-it.com/gas-calendar-getevents/)
