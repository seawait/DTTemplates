# DTTemplates

DynamicTemplate扩展使用的模板文件

## 默认模板路径

* Windows : %HOMEPATH%/.vscode/templates
* Linux : ~/.vscode/templates
* Mac : ~/.vscode/templates

## 模板字段

模板字段是模板内用于替代生成时动态变化的文本内容所指定的占位符.
分为配置字段,程序字段与自定义字段三种类型.所有字段都可以设置显示的格式,
且同一字段每次出现可以设置不同的显示格式.
模板字段的表现形式为`{__fieldName__}`或者`{__fieldName__.flag}`
(_fieldName表示字段名,flag表示字段显示格式_).

### 配置字段

配置字段是替代内容通过配置文件设定的固定字段.
配置格式为__"dynamicTemplate.fields.{fieldName}":"{content}"__
(_{fieldName}表示字段名_, _{content}表示替代内容_)

推荐设置:

* `{__author__}`(配置为`"dynamicTemplate.fields.author"`)
* `{__email__}`(配置为`"dynamicTemplate.fields.email"`)

### 程序字段

程序字段是替代内容由扩展内已实现的方法动态生成的固定字段.
当前可使用的字段有:

* `{__date__}`生成当前本地时间, 格式为`年-月-日 时:分:秒`

### 自定义字段

自定义字段是在模板执行时提示用户输入替代内容的动态字段.
每次调用对应模板时, 扩展程序会提前检索文件内出现的动态字段,
合并相同项后再逐个提示用户输入替代内容.

### 字段显示格式

字段显示格式是针对代码中相同变量名存在多种命名格式(如大驼峰命名,
小驼峰命名,snakeCase...)而设置的一种折衷自动转化方案.例如,
`{__fieldName__}`的替代内容是`myCustomField`,
如果在其它位置使用了`{__fieldName__.u}`, 则会自动替换为`MY_CUSTOM_FIELD`
如果在其它位置使用了`{__fieldName__.d}`, 则会自动替换为`my.custom.field`
目前支持的转化格式及对应的使用符号(大小写不敏感)如下:

* `c`: camelCaseFileName(小驼峰命名)
* `p` : PascalCaseFileName(大驼峰命名)
* `s` : snake_case_file_name
* `k` : kebab-case-file-name
* `d` : lower.dot.case.file.name(包名格式)
* `u` : UPPER_SNAKE_CASE_FILE_NAME
* 其它无效内容默认使用原文本替代

## 模板命令

* 每个模板文件可以由多条模板命令组成.
* 每条模板命令由命令名及命令参数构成.
* 命令参数可能会有多个, 也可能会有多组, 具体根据命令类型而定.
* 命令参数间以及命令间都通过固定文本`//===ACMD===`按行分隔.
* 目前支持的命令类型有`Add`, `Insert`, `Open`.
* 下面详细介绍每个命令的配置及作用:

### Add

用于添加文件

例如:
添加文件`CMD_{__CommandName__.u}.ts`到`src/public/commands`目录.
第二个`//===ACMD===`后直到文件结尾或者下一个`//===ACMD===`分隔符前的文本为文件内容.

```typescript

Add
//===ACMD===
src/public/commands/CMD_{__CommandName__.u}.ts
//===ACMD===
import { Command, MEvent } from '../lib/RbLeg';
/**
 * {__Comment__}
 * @Author : {__author__} ({__email__})
 * @Date   : {__date__}
 */
export class CMD_{__CommandName__.u} extends Command
{
    constructor()
    {
        super();
    }

    public execuse(e:MEvent):void
    {
    }
}

```

### Insert

用于往现有文件插入添加内容

例如:
在`src/public/config/GCommandConfig.ts`里添加内容.
第一个参数为文件路径,基于项目根目录
第二个参数为文本在文件内的插入点位置标记文本(需独占单行),新增的文本插入到该文本上方.
第三个参数为插入的文本内容.
后续的参数同第二,第三个参数, 可配置多组, 以空参数结束. 用于在同个文件多处插入内容

```typescript
Insert
//===ACMD===
src/public/config/GComandConfig.ts
//===ACMD===
        //ACMD insert0
//===ACMD===
        /** {__Comment__} */
        this.push(CommandEvent.CMD_{__CommandName__.u} ,CMD_{__CommandName__.u});

//===ACMD===
//ACMD insert1
//===ACMD===
import { CMD_{__CommandName__.u} } from '../commands/CMD_{__CommandName__.u}';

//===ACMD===
//===ACMD===
```

### Open

一般用于模板文件的最后, 打开并显示指定的文件.

例如:
打开文件`src/public/commands/CMD_{__CommandName__.u}.ts`
参数只有一个, 即需要打开并显示的文件路径(基于项目根目录).

```typescript
Open
//===ACMD===
src/public/commands/CMD_{__CommandName__.u}.ts
```
