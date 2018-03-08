# 参考模板

## 新建GameEvent

|   字段名  | 说明 |
|:--------:|:-----|
|`{Comment}`|事件描述的注释内容|
|`{EventName}`|事件名(*小驼峰格式*)模板内会自动转换为大写常量格式|

调用结果:

|   操作   |*使用`{EventName}`以大写常量格式定义事件并以`{Comment}`内容注释*|
|:---------:|:--------|:----|
|文件|`src/public/event/GameEvent.ts`|
|位置标记|`//ACMD insert1`|

## 新建命令

|   字段名  | 说明 |
|:--------:|:-----|
|`{Comment}`|命令描述的注释内容|
|`{CommandName}`|命令名(*小驼峰格式*)|

调用结果:

|   操作   |*添加`CMD_{CommandName}.ts`文件*|
|:--------:|:--------|:----|
|文件夹|`src/public/commands`|
|**操作**|***导入类`CMD_{CommandName}`***|
|文件|`src/public/config/GCommandConfig.ts`|
|位置标记|`//ACMD insert1`|
|**操作**|***绑定`CMD_{CommandName}`类和命令事件并以`{Comment}`内容注释***|
|文件|`src/public/config/GCommandConfig.ts`|
|位置标记|`//ACMD insert0`|
|**操作**|***使用`CMD_{CommandName}`大写常量格式定义命令事件并以`{Comment}`内容注释***|
|文件|`src/public/event/CommandEvent.ts`|
|位置标记|`//ACMD insert0`|
|**操作**|***自动打开文件***|
|文件|`src/public/command/CMD_{CommandName}.ts`|

## 新建对话框

|   字段名  | 说明 |
|:--------:|:-----|
|`{Comment}`|对话框描述(*用于注释*)|
|`{DialogName}`|对话框名(*小驼峰格式*)|
|`{DialogFolderName}`|对话框文件夹名(*小驼峰格式*)|

调用结果:

|   操作   |*添加`{DialogName}Dlg.ts`文件*|
|:--------:|:--------|:----|
|文件夹|`src/dialog/{DialogFolderName}`|
|**操作**|***添加`{DialogName}DlgMediator.ts`文件***|
|文件夹|`src/dialog/{DialogFolderName}`|
|**操作**|***定义对话框调用事件`SHOW_{DialogName}_DLG`并以`{Comment}`注释***|
|文件|`src/public/event/GameEvent.ts`|
|位置标记|`//ACMD insert`|
|**操作**|***添加对话框配置***|
|文件|`src/public/config/DialogConfig.ts`|
|位置标记|`//ACMD insert`|
|**操作**|***添加对话框资源配置(资源名格式为`Fui{DialogName}Dlg.fui`)***|
|文件|`src/public/config/AssetConfig.ts`|
|位置标记|`//ACMD insert`|
|**操作**|***自动打开文件***|
|文件|`src/dialog/{DialogFolderName}/{DialogName}DlgMediator.ts`|