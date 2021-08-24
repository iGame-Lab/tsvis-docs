# 可视化后端启动选项

目前TS-VIS公开了三个子命令`runserver`, `test`, `migrate`，下面将分别介绍这三个子命令及其参数

## `runserver`

该命令是启动TS-VIS后端的命令，如果用户在不指定子命令的情况下，该命令会被默认执行。

其中包含的选项有

- `--logdir`，必选参数，该参数指定可视化日志所在的文件夹，并在启动后会监听该文件夹中文件的变化。

!!! tip 注意
    若指定的文件夹不存在则TS-VIS会自动退出

- `--port`，可选参数，该参数指定TS-VIS服务在哪个端口启动。该参数的默认值为9898，也就是说，默认情况下TS-VIS会启动在9898端口。

## `test`

该命令运行TS-VIS单元测试，其包含的选项有

- `--testdir`，必须参数，该参数指定单元测试使用日志所在的文件夹，并在启动后会监听该文件夹中文件的变化。

- `testcase`，必须参数，该参数指定运行单元测试的模块名

## `migrate`

该命令是为了初始化Django服务使用的，若未运行该命令，启动TS-VIS时会得到Django的警告

```
You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
```

该警告不影响TS-VIS的正常工作，若需要移除该警告则运行一次该命令即可。

## 查看版本和提交号

- `--version`, `-v`

TS-VIS还提供了查看版本号的选项，该选项只在未指定子命令的情况下可用，如

```
$ tsvis -v
```

## 查看命令行参数帮助

- `--help`, `-h`

用户可以通过该选项在控制台查看命令行参数的帮助说明。
