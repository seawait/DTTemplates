# DTTemplates

DynamicTemplate扩展使用的模板文件

默认模板路径

* Windows : %HOMEPATH%/.vscode/templates
* Linux : ~/.vscode/templates
* Mac : ~/.vscode/templates

[参考模板](https://github.com/seawait/DTTemplates/blob/master/TEMPLATES.md)

## 模板字段

* 模板字段是模板内用于替代生成时动态变化的文本内容所指定的占位符.
* 分为**配置字段**,**程序字段**与**自定义字段**三种类型.
* 所有字段都可以设置显示的格式,
* 且同一字段每次出现可以设置不同的显示格式.
* 模板字段的格式为`{__fieldName__}`或者`{__fieldName__.flag}`
  * _fieldName表示字段名_
  * _flag表示字段显示格式_

字段显示格式:
>字段显示格式是针对代码中相同变量名存在多种命名格式而设置的一种折衷自动转化方案.(如大驼峰命名, 小驼峰命名,snakeCase...)

例如:

>模板中的`{__fieldName__}`调用时的替代内容是`myCustomField`,
那么模板中的`{__fieldName__.u}`自动替换为`MY_CUSTOM_FIELD`,
`{__fieldName__.d}`自动替换为`my.custom.field`.

目前支持转化的格式及对应的符号如下:*(大小写不敏感)*

* `c`: camelCaseFileName(小驼峰命名)
* `p` : PascalCaseFileName(大驼峰命名)
* `s` : snake_case_file_name
* `k` : kebab-case-file-name
* `d` : lower.dot.case.file.name(包名格式)
* `u` : UPPER_SNAKE_CASE_FILE_NAME
* 其它无效内容默认使用原文本替代

### Ⅰ. 配置字段

* 配置字段是替代内容通过配置文件设定的固定字段.
* 配置格式为`"dynamicTemplate.fields.{fieldName}":"{content}"`
  * _{fieldName}表示字段名_
  * _{content}表示替代内容_

例如:

* 字段:`{__author__}` => 对应替换为配置`"dynamicTemplate.fields.author"`的值
* 字段:`{__email__}`=> 对应替换为配置`"dynamicTemplate.fields.email"`的值

### Ⅱ. 程序字段

* 程序字段是替代内容由扩展内已实现的方法动态生成的固定字段.

例如:

* 字段:`{__date__}` => 对应替换为当前本地时间, 格式为`年-月-日 时:分:秒`

### Ⅲ. 自定义字段

* **自定义字段**是模板执行时提示**用户输入**替代内容的**动态字段**
* 每次调用对应模板时, 扩展程序会提前检索文件内出现的动态字段
* 合并相同项后再逐个提示用户输入替代内容.

## 模板命令

* 每个**模板文件**可以由多条**模板命令**组成.
* 每条**模板命令**由*命令名*及*命令参数*构成.
* **命令参数**可能会有*多个*, 也可能会有*多组*, 具体根据*命令类型*而定.
* *命令参数间*以及*命令间*都通过固定文本`//===ACMD===`*按行分隔*.
* 目前支持的**命令类型**有`Add`, `Insert`, `Open`.

下面详细介绍每个命令的配置及作用.

### Add

用于添加文件

例如:

* 添加文件`CMD_{__CommandName__.u}.ts`到`src/public/commands`目录.
* 第二个`//===ACMD===`后直到文件结尾或者下一个`//===ACMD===`分隔符前的文本为文件内容.

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

* 在`src/public/config/GCommandConfig.ts`里添加内容.
* 第一个参数为文件路径,基于项目根目录
* 第二个参数为文本在文件内的插入点位置标记文本(需独占单行),新增的文本插入到该文本上方.
* 第三个参数为插入的文本内容.
* 后续的参数同第二,第三个参数, 可配置多组, 以空参数结束. 用于在同个文件多处插入内容

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

* 打开文件`src/public/commands/CMD_{__CommandName__.u}.ts`
* 参数只有一个, 即需要打开并显示的文件路径(基于项目根目录).

```typescript
Open
//===ACMD===
src/public/commands/CMD_{__CommandName__.u}.ts
```
