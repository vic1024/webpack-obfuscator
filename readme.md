# javascript-obfuscator plugin for Webpack

[![npm version](https://badge.fury.io/js/webpack-obfuscator.svg)](https://badge.fury.io/js/webpack-obfuscator)

### 起因
因为有项目使用的是webpack构建的，所以代码混淆需要使用到webpack-obfuscator，但其引用的javascript-obfuscator库版本是0.18.1，本人在打包编译的时候遇上了报错：

```javascript

Error: Cannot read property 'ecmaFeatures' of undefined

```
经过查询之后，javascript-obfuscator调用的espree库是4.0.0造成的，升级到5.0.1即可，所以使用他人修改好的库就可以使用了，如：
Domuska/javascript-obfuscator 或者  mikoim/javascript-obfuscator 都可以。

所以本人才Fork（webpack-obfuscator）项目，进行修改。


### 配置
```javascript

    // 在webpack的配置文件中引入
    const JavaScriptObfuscator = require('webpack-obfuscator')


    // 在plugins.push中增加配置

    // 高混淆-低性能
    new JavaScriptObfuscator({
      compact: true, // 压缩代码
      // controlFlowFlattening: true, // 是否启用控制流扁平化(降低1.5倍的运行速度)  这个会溢出
      controlFlowFlattening: false, // 是否启用控制流扁平化(降低1.5倍的运行速度)
      controlFlowFlatteningThreshold: 1, // 应用概率;在较大的代码库中，建议降低此值，因为大量的控制流转换可能会增加代码的大小并降低代码的速度。
      deadCodeInjection: true, // 随机的死代码块(增加了混淆代码的大小)
      deadCodeInjectionThreshold: 1, // 死代码块的影响概率
      debugProtection: true, // 此选项几乎不可能使用开发者工具的控制台选项卡
      debugProtectionInterval: true, // 如果选中，则会在“控制台”选项卡上使用间隔强制调试模式，从而更难使用“开发人员工具”的其他功能。
      disableConsoleOutput: true, // 通过用空函数替换它们来禁用console.log，console.info，console.error和console.warn。这使得调试器的使用更加困难。
      identifierNamesGenerator: 'hexadecimal', // 标识符的混淆方式 hexadecimal(十六进制) mangled(短标识符)
      log: false,
      renameGlobals: false, // 是否启用全局变量和函数名称的混淆
      rotateStringArray: true, // 通过固定和随机（在代码混淆时生成）的位置移动数组。这使得将删除的字符串的顺序与其原始位置相匹配变得更加困难。如果原始源代码不小，建议使用此选项，因为辅助函数可以引起注意。
      selfDefending: true, // 混淆后的代码,不能使用代码美化,同时需要配置 cpmpat:true;
      stringArray: true, // 删除字符串文字并将它们放在一个特殊的数组中
      stringArrayEncoding: 'rc4',
      stringArrayThreshold: 1,
      transformObjectKeys: false, // 这个设置为true会无法运行
      unicodeEscapeSequence: false // 允许启用/禁用字符串转换为unicode转义序列。Unicode转义序列大大增加了代码大小，并且可以轻松地将字符串恢复为原始视图。建议仅对小型源代码启用此选项。
    }, ['main.js'])

    // 中混淆-最佳性能
    new JavaScriptObfuscator({
      compact: true, // 压缩代码
      // controlFlowFlattening: true, // 是否启用控制流扁平化(降低1.5倍的运行速度) 这个会溢出
      controlFlowFlattening: false, // 是否启用控制流扁平化(降低1.5倍的运行速度)
      controlFlowFlatteningThreshold: 0.75, // 应用概率;在较大的代码库中，建议降低此值，因为大量的控制流转换可能会增加代码的大小并降低代码的速度。
      deadCodeInjection: true, // 随机的死代码块(增加了混淆代码的大小)
      deadCodeInjectionThreshold: 0.4, // 死代码块的影响概率
      debugProtection: false, // 此选项几乎不可能使用开发者工具的控制台选项卡
      debugProtectionInterval: false, // 如果选中，则会在“控制台”选项卡上使用间隔强制调试模式，从而更难使用“开发人员工具”的其他功能。
      disableConsoleOutput: true, // 通过用空函数替换它们来禁用console.log，console.info，console.error和console.warn。这使得调试器的使用更加困难。
      identifierNamesGenerator: 'hexadecimal', // 标识符的混淆方式 hexadecimal(十六进制) mangled(短标识符)
      log: false,
      renameGlobals: false, // 是否启用全局变量和函数名称的混淆
      rotateStringArray: true, // 通过固定和随机（在代码混淆时生成）的位置移动数组。这使得将删除的字符串的顺序与其原始位置相匹配变得更加困难。如果原始源代码不小，建议使用此选项，因为辅助函数可以引起注意。
      selfDefending: true, // 混淆后的代码,不能使用代码美化,同时需要配置 cpmpat:true;
      stringArray: true, // 删除字符串文字并将它们放在一个特殊的数组中
      stringArrayEncoding: 'base64',
      stringArrayThreshold: 0.75,
      transformObjectKeys: false, // 这个设置为true会无法运行
      unicodeEscapeSequence: false // 允许启用/禁用字符串转换为unicode转义序列。Unicode转义序列大大增加了代码大小，并且可以轻松地将字符串恢复为原始视图。建议仅对小型源代码启用此选项。
    }, ['main.js'])

    // 低混淆-高性能
    new JavaScriptObfuscator({
      compact: true, // 压缩代码
      controlFlowFlattening: false, // 是否启用控制流扁平化(降低1.5倍的运行速度)
      deadCodeInjection: false, // 随机的死代码块(增加了混淆代码的大小)
      debugProtection: false, // 此选项几乎不可能使用开发者工具的控制台选项卡
      debugProtectionInterval: false, // 如果选中，则会在“控制台”选项卡上使用间隔强制调试模式，从而更难使用“开发人员工具”的其他功能。
      disableConsoleOutput: true, // 通过用空函数替换它们来禁用console.log，console.info，console.error和console.warn。这使得调试器的使用更加困难。
      identifierNamesGenerator: 'hexadecimal', // 标识符的混淆方式 hexadecimal(十六进制) mangled(短标识符)
      log: false,
      renameGlobals: false, // 是否启用全局变量和函数名称的混淆
      rotateStringArray: true, // 通过固定和随机（在代码混淆时生成）的位置移动数组。这使得将删除的字符串的顺序与其原始位置相匹配变得更加困难。如果原始源代码不小，建议使用此选项，因为辅助函数可以引起注意。
      selfDefending: true, // 混淆后的代码,不能使用代码美化,同时需要配置 cpmpat:true;
      stringArray: true, // 删除字符串文字并将它们放在一个特殊的数组中
      stringArrayEncoding: false,
      stringArrayThreshold: 0.75,
      unicodeEscapeSequence: false // 允许启用/禁用字符串转换为unicode转义序列。Unicode转义序列大大增加了代码大小，并且可以轻松地将字符串恢复为原始视图。建议仅对小型源代码启用此选项。
    }, ['main.js'])

```

### Installation

Install the package with NPM and add it to your devDependencies:

`npm install --save-dev vic1024/webpack-obfuscator`

### Usage:

```javascript
var JavaScriptObfuscator = require('webpack-obfuscator');

// ...

// webpack plugins array
plugins: [
	new JavaScriptObfuscator ({
      rotateUnicodeArray: true
  }, ['excluded_bundle_name.js'])
],
```

### obfuscatorOptions
Type: `Object` Default: `null`

Options for [javascript-obfuscator](https://github.com/javascript-obfuscator/javascript-obfuscator). Should be passed exactly like described on their page.

**Warning:** right now plugin not supported `souceMap` and `souceMapMode` options!

### excludes
Type: `Array` or `String` Default: `[]`

Bundle name is output file name after webpack compilation. With multiple webpack entries you can set bundle name in `output` object with aliases `[name]` or `[id]`.

Syntax for excludes array is syntax for [multimatch](https://github.com/sindresorhus/multimatch) package. You can see examples on package page.

Few syntax examples: `['excluded_bundle_name.js', '**_bundle_name.js'] or 'excluded_bundle_name.js'`


Example:
```
// webpack.config.js
'use strict';

const JavaScriptObfuscator = require('webpack-obfuscator');

module.exports = {
    entry: {
        'abc': './test/input/index.js',
        'cde': './test/input/index1.js'
    },
    output: {
        path: 'dist',
        filename: '[name].js' // output: abc.js, cde.js
    },
    plugins: [
        new JavaScriptObfuscator({
            rotateUnicodeArray: true
        }, ['abc.js'])
    ]
};
```

Can be used to bypass obfuscation of some files.

### License
Copyright (C) 2017 [Timofey Kachalov](http://github.com/sanex3339).

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

  * Redistributions of source code must retain the above copyright
    notice, this list of conditions and the following disclaimer.
  * Redistributions in binary form must reproduce the above copyright
    notice, this list of conditions and the following disclaimer in the
    documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDER> BE LIABLE FOR ANY
DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF
THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
