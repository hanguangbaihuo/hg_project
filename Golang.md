### golang ###

#### 安装 golang ###

    https://stackoverflow.com/questions/12843063/install-go-with-brew-and-running-the-gotour

#### 开发环境设置 ####

    1. 设置 GOROOT: 系统文件所在文件夹
    2. 设置 GOPATH: 项目文件所在文件夹

    ../Go    <<--- GOROOT 指向的位置
    --src                 <<--- Go 语言自带的源代码
    --pkg                 <<--- 编译的中间文件放在此文件夹
    --bin                 <<--- 编译的目标文件放在此文件夹
    ../MyWorks  <<--- GOPATH 指向的位置
        --src                 <<--- 项目源代码放置在此文件夹。!!!警告：一个常犯的错误是把 GOPATH 指向此处!!!
            --HelloWorld      <<--- 我们项目源代码所在的文件夹。!!!警告：一个常犯的错误是把 GOPATH 指向此处!!!
            --vendor          <<--- 第三方开源代码文件夹
                --github.com
                    --...
        --pkg                 <<--- 编译的中间文件放在此文件夹，Go编译器自动生成此文件夹
        --bin                 <<--- 编译的目标文件放在此文件夹，Go编译器自动生成此文件夹

#### Go 依赖管理 dep ####

    1 install dep
        https://golang.github.io/dep/docs/installation.html
    2 install 可视化工具
        brew install graphviz

    go 语言的依赖管理目前做的并不好,设置很糟糕,从 go1.9以后有了官方版本的 dep
    https://github.com/golang/dep
    1. 初始化项目
        dep init -v
    2. 查看依赖状态
        dep status
    3. 添加依赖
        dep ensure
            四种需要运行的情况
            1. 添加一个新的依赖 dep ensure -add
            2. 更新依赖 dep ensure -update
            3. 同步项目里面的依赖
            4. 同步 Goplg.toml 里面的依赖
    4. 依赖检查
        dep check
    5. 可视化依赖查看
        https://golang.github.io/dep/docs/daily-dep.html
        (Mac)
        dep status -dot | dot -T png | open -f -a /Applications/Preview.app

    Note:
      dep 和 iris 之间的依赖有 bug,解决方法: https://github.com/kataras/iris/issues/1143
      先下载 go get github.com/kataras/iris
      然后下载两个文件:
        wget https://raw.githubusercontent.com/kataras/iris/5bdbffebc8a4a525a9ec8f9d6425fc22f615f03c/Gopkg.toml
        wget https://raw.githubusercontent.com/kataras/iris/5bdbffebc8a4a525a9ec8f9d6425fc22f615f03c/Gopkg.lock
        

#### Go 开发工具 ####

    1. gin
        https://github.com/codegangsta/gin
        go get github.com/codegangsta/gin
        gin 是一个可以自动测试项目文件变动,自动重启 go 运行的工具,类似 django 的 runserver
        gin -i run main.go  # 注意使用-i 选项，否则可能造成第二次运行出错。
    2. debug 工具:
        dlv debug main.go
        https://github.com/go-delve/delve
        go get -u github.com/go-delve/delve/cmd/dlv
        dlv debug main.go
