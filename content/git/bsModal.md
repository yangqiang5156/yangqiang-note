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

### modal 在html中的位置问题，modal div的父级元素不能有定位相关的属性，即父级元素不能有potition属性，否则会出现modal 的灰色遮罩层遮住下面的所有元素，导致无法操作modal和页面；

解决方法: 1.取消modal 父级元素的position属性，一般建议把modal作为body 的子元素；2.如果父元素必须采用position 属性，可通过js控制 在modal 弹出后将遮罩层的display设置为none;

```js
function showUserInfo(){
    $("#userInfo_modi").modal('show');
    $(".modal-backdrop").css('display','none');

}
```