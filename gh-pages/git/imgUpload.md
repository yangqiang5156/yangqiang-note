### 图片上传预览

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>h5上传图片处理</title>
    <style>
        .images {
            width: 100px;
            height: 30px;
            background-color: orange;
        }
    </style>
</head>

<body>
    
    <div class="images">点击上传图片</div>
        <img src="" alt="" class="finalIMG" style="border:1px solid orange;">
    </div>
    <script>
        window.onload = function() {
            var imgElem = document.querySelector(".images");
            var fileElem = document.createElement('input');
            fileElem.type = 'file';

            imgElem.addEventListener('click', function() {
                fileElem.click();
            });

            fileElem.addEventListener('change', function() {
                var file = this.files[0];
                var img = document.createElement("img");
                var canvas = document.createElement("canvas");
                var ctx = canvas.getContext("2d");
                img.src = window.URL.createObjectURL(file);
                img.onload = function() {
                    var MAX_WIDTH = 400;
                    var MAX_HEIGHT = 300;
                    var width = img.width;
                    var height = img.height;
                    //等比例缩放制作缩略图
                    if (width > height) {
                        if (width > MAX_WIDTH) {
                            height *= MAX_WIDTH / width;
                            width = MAX_WIDTH;
                        }
                    } else {
                        if (height > MAX_HEIGHT) {
                            width *= MAX_HEIGHT / height;
                            height = MAX_HEIGHT;
                        }
                    }
                    canvas.width = width;
                    canvas.height = height;
                    var ctx = canvas.getContext("2d");
                    ctx.drawImage(img, 0, 0, width, height);
                    var finalURL = canvas.toDataURL(); //base64
                    var imgshow = document.querySelector(".finalIMG");
                    imgshow.setAttribute('src', finalURL)

                    //上传缩略图
                    // canvas.toBlob(function(blob) {
                    //     var form = new FormData();
                    //     form.append('file', blob);
                    //     fetch('/api/upload', {
                    //         method: 'POST',
                    //         body: form
                    //     });
                    // });
                }
            });
        }


    </script>
</body>

</html>

```


```javascript
 //预览图片的几种方式
        //1.方式一
        var img = document.createElement('img');
        img.src = window.URL.createObjectURL(file);

        //2.方式二
        var img = document.createElement("img");
        var reader = new FileReader();
        reader.onload = function(e) {
            img.src = e.target.result;
        }
        reader.readAsDataURL(file);


        //图片上传预览
        //dom结构
        //<div>
        //    <input id="file_1" type="file" onchange="showPic(this,'pic')">
       //     <img src="" id="pic">
       // </div>

        function getFullPath(obj, imgId) {
            if (obj) {
                //Internet Explorer
                if (window.navigator.userAgent.indexOf("MSIE") >= 1) {
                    obj.select();
                    return document.selection.createRange().text;
                }
            
                //兼容chrome、火狐等，HTML5获取路径
                if (typeof FileReader != "undefined") {
                    var reader = new FileReader();
                    reader.onload = function(e) {
                        document.getElementById(imgId).src = e.target.result + "";
                    }
                    reader.readAsDataURL(obj.files[0]);
                } else if (browserVersion.indexOf("SAFARI") > -1) {
                    alert("暂时不支持Safari浏览器!");
                }

            }
        }

        function showPic(obj, imgId) {
            var fullPath = getFullPathDevice(obj, imgId);
            if (fullPath) {
                document.getElementById(imgId).src = fullPath + "";
            }
        }

```