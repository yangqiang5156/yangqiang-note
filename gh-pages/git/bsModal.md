### modal 中有需要初始化dom的组件，应该注意在show.bs.modal的回调函数中，否则，modal框中的插件在modal 显示后不能正确初始化显示

```javascript
//在modal框中使用select2插件
$('.detailSite_modal').on('show.bs.modal', function (e) {
    $(".siteSelect").select2({
        "placeholder": '搜索站点',
        "language": "zh-CN"
    });
});
```