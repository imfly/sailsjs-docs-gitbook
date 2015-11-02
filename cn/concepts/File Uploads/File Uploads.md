# 文档上传（File Uploads）

> TODO: Normalize/expand this section

### 例子

#### 产生一个 `api`
首先，我们需要替 serving/storing 产生一个新的 `api` 文档。用 sails 命令行工具执行此动作。

```sh

dude@littleDude:~/node/myApp$ sails generate api file

debug: Generated a new controller `file` at api/controllers/FileController.js!
debug: Generated a new model `File` at api/models/File.js!

info: REST API generated @ http://localhost:1337/file
info: and will be available the next time you run `sails lift`.

dude@littleDude:~/node/myApp$ 

```

#### 撰写控制器动作

让我们建立一个 `index` 动作来开始文档上传及 `upload` 动作来接收文档。

```javascript 

// myApp/api/controllers/FileController.js

module.exports = {

  index: function (req,res){

    res.writeHead(200, {'content-type': 'text/html'});
    res.end(
    '<form action="http://localhost:1337/file/upload" enctype="multipart/form-data" method="post">'+
    '<input type="text" name="title"><br>'+
    '<input type="file" name="avatar" multiple="multiple"><br>'+
    '<input type="submit" value="Upload">'+
    '</form>'
    )
  },
  upload: function  (req, res) {
    req.file('avatar').upload(function (err, files) {
      if (err)
        return res.serverError(err);

      return res.json({
        message: files.length + ' file(s) uploaded successfully!',
        files: files
      });
    });
  }

};
```

#### 它们去哪了？
使用默认的 `receiver`，上传的文档会在 `myApp/.tmp/uploads/` 目录。你可以在 `upload` 动作内做你想做的任何事情。

#### 上传到自定义文件夹
在上面的例子中，我们可以将文档上传到 .tmp/uploads。那么我们该如何设置为自定义文件夹，例如 `assets/images`。我们可以通过增加选项到上传功能来实现这一目标，如下所示：
```javascript

  var uploadPath = './assets/images';
  uploadFile.upload({ dirname: uploadPath },function onUploadComplete (err, files) {             
                                                                              
      if (err) 
        return res.serverError(err);

      return res.json({
        message: files.length + ' file(s) uploaded successfully!',
        path:uploadPath
        file:files
      });
  });
```

> 请查看 [Skipper 文件](https://github.com/balderdashy/skipper)取得更多资讯及其他可用的 `receivers` 清单！



<docmeta name="uniqueID" value="fileuploads72947">
<docmeta name="displayName" value="File Uploads">
