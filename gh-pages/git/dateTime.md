
+ jquery.datetimepicker.js

```js
//年月日  2017-06-01
$('#datetimepicker1').datetimepicker({
    format: 'Y-m-d',
    formatDate: 'Y-m-d',
    timepicker: false
});
//时分 12:00
$('#datetimepicker2').datetimepicker({
    datepicker:false,
    format:'H:i',
    value:'00:00'
});


```