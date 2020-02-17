# Scrapy

## 命令行工具

Scrapy项目的目录结构

```python
scrapy.cfg #scrapy 项目名称的定义
myproject/
    __init__.py
    items.py
    middlewares.py
    pipelines.py
    settings.py
    spiders/
        __init__.py
        spider1.py
        spider2.py
        ...
```

scrapy存在两种指令，在不同位置执行指令的行为可能不同：

1. 一种是只能从特定项目中运行，只作用于当前项目的指令。仅project指令
2. 另一种是全局指令，在任意目录都可以执行的指令。



仅project指令：

- [`startproject`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-startproject)
- [`genspider`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-genspider)
- [`settings`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-settings)
- [`runspider`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-runspider)
- [`shell`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-shell)
- [`fetch`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-fetch)
- [`view`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-view)
- [`version`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-version)



全局指令：

- [`crawl`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-crawl)
- [`check`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-check)
- [`list`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-list)
- [`edit`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-edit)
- [`parse`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-parse)
- [`bench`](https://www.osgeo.cn/scrapy/topics/commands.html#std:command-bench)



### 全局指令

####startproject:

​	创建一个名为 `project_name` 下 `project_dir` 目录。如果 `project_dir` 没有指定， `project_dir` 将与 `project_name` 

```shell
scrapy startproject project_name [project_dir] #project_dir可以指定项目创建的目录
```

#### genproject

​	在当前文件夹或当前项目的 `spiders` 文件夹（如果从项目内部调用）。这个 `<name>` 参数设置为spider的 `spider_name` ，同时 `<domain>` 用于生成 `allowed_domains` 和 `start_urls` 蜘蛛的属性。

```shell
scrapy genspider [-t template] <spider_name> <domain>
```

#### fetch

​	使用ScrapyDownloader下载给定的URL，并将内容写入标准输出。

```shell
scrapy fetch url
```

#### view

​	在你的默认浏览器中打开给定的URL，并以Scrapy spider获取到的形式展现。 有些时候spider获取到的页面和普通用户看到的并不相同，一些动态加载的内容是看不到的， 因此该命令可以用来检查spider所获取到的页面。

```shsell
scrapy view url
```

#### shell

​	以`scrapy shell`的方式打开给定的url

```shell
scrapy shell url
```



### 仅project指令

#### crawl

​	用于启动名称为`spider_name`的爬虫

```shell
scrapy crawl spider_name
```

#### check

​	用于检查代码合法性

```shell
scrapy check spider_name
```

#### list

​	用于列出项目中所有可用的spider

```shell
scrapy list
```

#### edit

​	用于编辑爬虫，使用中定义的编辑器编辑给定的蜘蛛 `EDITOR` 环境变量或（如果未设置） [`EDITOR`](https://www.osgeo.cn/scrapy/topics/settings.html#std:setting-EDITOR) 设置。

```shell
scrapy edit spider_name
```

#### parse

​	以当前项目的spider解析给定的url

```shell
scrapy parse url [option]
#--spider=SPIDER 指定spider解析
#--callback -c 使用指定的parse方法解析url
```

------

## Spider

​	**定义**：spider是定义一个特定站点（或一组站点）如何被抓取的类，包括如何执行抓取（即跟踪链接）以及如何从页面中提取结构化数据（即抓取项）。换言之，spider是为特定站点（或者在某些情况下，一组站点）定义爬行和解析页面的自定义行为的地方。

​	**爬虫的运行周期**：

1. 首先生成对第一个URL进行爬网的初始请求，然后指定一个回调函数，该函数使用从这些请求下载的响应进行调用，要执行的第一个请求是通过调用 [`start_requests()`](https://www.osgeo.cn/scrapy/topics/spiders.html#scrapy.spiders.Spider.start_requests) （默认）生成的方法 [`Request`](https://www.osgeo.cn/scrapy/topics/request-response.html#scrapy.http.Request) 对于中指定的URL [`start_urls`](https://www.osgeo.cn/scrapy/topics/spiders.html#scrapy.spiders.Spider.start_urls) 以及 [`parse`](https://www.osgeo.cn/scrapy/topics/spiders.html#scrapy.spiders.Spider.parse) 方法作为请求的回调函数。
2. 回调函数中，解析响应（网页）并返回提取数据的dict， [`Item`](https://www.osgeo.cn/scrapy/topics/items.html#scrapy.item.Item) 物体， [`Request`](https://www.osgeo.cn/scrapy/topics/request-response.html#scrapy.http.Request) 对象，或这些对象中的一个不可重复的对象。这些请求还将包含回调（可能相同），然后由scrappy下载，然后由指定的回调处理它们的响应。
3. 在回调函数中，解析页面内容，通常使用 [选择器](https://www.osgeo.cn/scrapy/topics/selectors.html#topics-selectors) （但您也可以使用beautifulsoup、lxml或任何您喜欢的机制）并使用解析的数据生成项。
4. 从spider返回的项目通常被持久化到数据库（在某些 [Item Pipeline](https://www.osgeo.cn/scrapy/topics/item-pipeline.html#topics-item-pipeline) ）或者使用 [Feed 导出](https://www.osgeo.cn/scrapy/topics/feed-exports.html#topics-feed-exports)

### Scrapy.spider

​	所有爬虫的顶层父类，所有scrapy爬虫必须继承该类，Scrapy.spider提供了一个默认值 [`start_requests()`](https://www.osgeo.cn/scrapy/topics/spiders.html#scrapy.spiders.Spider.start_requests) 从发送请求的实现 [`start_urls`](https://www.osgeo.cn/scrapy/topics/spiders.html#scrapy.spiders.Spider.start_urls) spider属性并调用spider的方法 `parse` 对于每个结果响应。

### 属性&方法

#### name

​	定义爬虫的名称的字符串。spider名称是scrappy定位（和实例化）spider的方式，因此它必须是唯一的。

####**allowed_domains**

​	设置allowed_domains的含义是过滤爬取的域名，在插件OffsiteMiddleware启用的情况下（默认是启用的），不在此允许范围内的域名就会被过滤，而不会进行爬取

####start_urls

​	爬虫进行爬取的起始页面

####start_requests()

​	调用爬虫开始爬取的方法，此方法返回一个iterable

####parse(response)

​	这是Scrapy在请求未指定回调时用来处理下载响应的默认回调。这个 `parse` 方法负责处理响应，并返回 爬取 的数据和/或更多的URL。