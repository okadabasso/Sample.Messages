# Message Resource build Sample

Sample project to create message resource assembly and each satellite assemblies.


## create resource text in text format(not xml format).

create  resource text like this...

```Common.txt
; in japanese culture
ERR_INPUT_TOO_LONG = {0} 入力文字が長すぎます。{1}文字まで
ERR_INVALID_FORMAT = {0} 入力形式が違います。

ERR_INVALID_RANGE = {0} 範囲を超えています。{1} から {2}
ERR_REQUIRED_MISSING = {0} 必須入力です。
;
; 例外時のメッセージ
EXCEPTION_DEFAULT_MESSAGE = アプリケーション例外が発生しました。
EXCEPTION_INVALID_ARG_LENGTH = 入力引数の長さが不正です。
EXCEPTION_INVALID_ARG_CHARACTERS = 入力引数に認識できない文字が含まれています。

MSG_RESULT_NO_DATA = 検索条件に一致するものが見つかりませんでした。

```

create localized resource text like this...

```Common.en.txt
; in english culture
ERR_INPUT_TOO_LONG = {0} input too long.max {1} characters
ERR_INVALID_FORMAT = {0} invalid format.

ERR_INVALID_RANGE = {0} invalid range. {1} to {2}
ERR_REQUIRED_MISSING = {0} is required.
;
; 例外時のメッセージ
EXCEPTION_DEFAULT_MESSAGE = application exception occured
EXCEPTION_INVALID_ARG_LENGTH = invalid argument length
EXCEPTION_INVALID_ARG_CHARACTERS = invalid argument characters

MSG_RESULT_NO_DATA = data not found.
```

## edit msbuild project file

see Sample.Messages.csproj file.

### add another resource text file

If you want to add another resource text file, add file and add entry to  ItemGroup/ResourceText
like this.

```Foo.txt
; in japanese culture
ErrorMessage = new message

```

```Sample.Messages.csproj
<ResourceText Include="Foo.txt" />
```

you can access to new message class as "Sample.Messages.Foo".



namespace, version, assembly name ,etc... are sample. define as you want to define.

## build resource assemblies

do this

```
> msbuild Sample.Messages.csproj /t:Build
```

now, you can find resource assembly and localized satellite assemblies in directory named "bin" .


## use from application

* add reference bin/Sample.Message.dll
* add satellite assemblies as "file" to project.
* make satellite assemblies to copy always to output path.


then do like this in C#.

``` 
Console.WriteLine(Sample.Messages.Common.MSG_RESULT_NO_DATA);
```

## switch culture

you can access localized resource text like this.

``` 
CultureInfo ci = new CultureInfo("en"); // in english culture
Thread.CurrentThread.CurrentCulture = ci;
Thread.CurrentThread.CurrentUICulture = ci;

Console.WriteLine(Sample.Messages.Common.MSG_RESULT_NO_DATA);
```

or...

``` 
CultureInfo ci = new CultureInfo("en"); // in english culture
Sample.Messages.Common.Culture = ci; // this is "Global", be carefull to use.

Console.WriteLine(Sample.Messages.Common.MSG_RESULT_NO_DATA);
```

## remarks

In this sample, I use "MsBuildCommunityTasks" for "AssemblyInfo" task or others.
You can get  MsBuildCommunityTasks from https://github.com/loresoft/msbuildtasks .
