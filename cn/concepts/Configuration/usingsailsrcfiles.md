# 使用 .sailsrc 文档（Using .sailsrc Files）


除了设置应用程序的其他方法外，从 0.10 版开始，你可以在 `.sailsrc` 文档里为指定一个或多个应用程序的设置（感谢 Dominic Tarr 的优秀 [`rc` 模组](https://github.com/dominictarr/rc)）。`rc` 文档对于设置命令行和/或套用组件设置到所有执行在你电脑上的 Sails 应用程序最有用。

当 Sails 命令行界面执行一个指令时，它会先在当前目录和你的家目录（即 `~/.sailsrc`）（任何新建立的 Sails 应用程序附带的模板 `.sailsrc` 文档）寻找 `.sailsrc` 文档（JSON 或 [.ini](http://en.wikipedia.org/wiki/INI_file) 格式）。然后将它们合并到现有的组件设置。

> 其实，Sails 会从其它几个地方寻找 `.sailsrc` 文档（遵循 [rc 约定](https://github.com/dominictarr/rc#standards)）。你可以放置 `.sailsrc` 文档到这些路径。也就是说，你最好能遵循约定，放置公用 `.sailsrc` 文档的地方是你的家目录（即 `~/.sailsrc`）。




<docmeta name="uniqueID" value="sailsrc374211">
<docmeta name="displayName" value="Using `.sailsrc` Files">

