## 上传文件

### 文件 md5

> `npm i spark-md5`

-   H5 端

    ```javascript
    uni.chooseImage({
        count: 30, // 默认9
        sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
        sourceType: ['album'], // 从相册选择
        success: function (res) {
            let files = res.tempFiles[0]; // 以第一个为例，多图片上传自己遍历
            let reader = new FileReader();
            reader.readAsBinaryString(res.tempFile); // 读取文件
            reader.onload = function (e) {
                let _md5 = Sparkmd5.hashBinary(e.target.result); // 获得文件MD5
                console.log('_md5_$$$$$$', _md5);
            };
        },
    });
    ```

-   微信小程序端

    > 不支持 `new FileReader()`

    ```javascript
    uni.getFileSystemManager().readFile({
        filePath: file.url,
        success: (res) => {
            let spark = new Sparkmd5.ArrayBuffer();
            spark.append(res.data);
            let _md5 = spark.end(false);
            console.log('_md5_$$$$$$', _md5);
        },
    });
    ```
