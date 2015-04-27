title: '[hexo 3.0.0-beta.1] hexo 原始碼閱讀 01'
date: 2015-01-15 16:53:37
tags:
- hexo
- hxojs
categories:
- 筆記
- hexo
---

最近 Tommy 大發佈了 [hexo](https://github.com/hexojs/hexo) 3.0.0 的 beta1 版本，那麼就藉機來讀一下新版本 hexo 的架構，之後也好開發新東西。

<!-- more -->

## 檔案架構

	./
	├── assets/
	├── bin/
	│	└── hexo
	├── lib/
	│	├── box/
	│	├── cli/
	│	├── extend/
	│	├── hexo/
	│	├── models/
	│	├── plugins/
	│	└── theme/
	├── test/
	│	├── fixtures/
	│	├── scripts/
	│	├── util/
	│	└── index.js
	├── .gitignore
	├── .gitmodules
	├── .jshintrc
	├── .npmignore
	├── .travis.yml
	├── LICENSE
	├── README.md
	├── appveyor.yml
	├── gulpfile.js
	└── package.json

## hexo 主程式 ./bin/hexo

可以看到主程式如下：

``` js ./bin/hexo
#!/usr/bin/env node
var args = require('minimist')(process.argv.slice(2));
require('../lib/cli/init')(args);
```

估計是解析參數後，由 `./lib/cli/init` 來初始化。在 `./lib/cli/init.js` 前幾行宣告：

``` js ./lib/cli/init.js
var Hexo = require('../hexo');
var pathFn = require('path');
var fs = require('hexo-fs');
var chalk = require('chalk');
```

可能 hexo 的主體已經打包成 `./lib/hexo`。

## hexo 本體 ./lib/hexo

### ./lib/hexo/index.js

`./lib/hexo/index.js` 定義的是 Hexo 物件，前面的宣告：

``` js ./lib/hexo/index.js
var util = require('hexo-util');
var Promise = require('bluebird');
var pathFn = require('path');
var tildify = require('tildify');
var Database = require('warehouse');
var _ = require('lodash');
var chalk = require('chalk');
var EventEmitter = require('events').EventEmitter;
var fs = require('hexo-fs');
var Module = require('module');
var vm = require('vm');
var pkg = require('../../package.json');
var createLogger = require('./create_logger');
var extend = require('../extend');
var Render = require('./render');
var registerModels = require('./register_models');
var Post = require('./post');
var Scaffold = require('./scaffold');
var Source = require('./source');
var Router = require('./router');
var defaultConfig = require('./default_config');
```

在第 54 行到 64 行初始化 hexo.extend：

``` js ./lib/hexo/index.js
  this.extend = {
    console: new extend.Console(),
    deployer: new extend.Deployer(),
    filter: new extend.Filter(),
    generator: new extend.Generator(),
    helper: new extend.Helper(),
    migrator: new extend.Migrator(),
    processor: new extend.Processor(),
    renderer: new extend.Renderer(),
    tag: new extend.Tag()
  };
```

我們可以知道 hexo 的 plugins 有 9 種 (配合[文件](http://hexo.io/docs/plugins.html))：

* console：用來操控 hexo 的介面
* depolyer：將 blog 佈署在各種系統上
* filter：用來修蓋特定內容的工具 (ex. excerpt 等)
* generator：用來生成靜態文件 (ex. RSS, sitemap 等)
* helper：寫 theme 時，用來快速插入特定內容的工具
* migrator：從別的 blog 系統搬來 hexo 的工具
* processor：處理 `source` 資料夾的原始檔案
* render：用來處理特定文件 (ex. ejs, stylus, jade 等)
* tag：用作快速插入特定內容的工具 (ex. bootstrap, multimedia 等)

而在 `Hexo.prototype.init` 中，第 116 行到 122 行：

``` js ./lib/hexo/index.js
  // Load internal plugins
  require('../plugins/console')(this);
  require('../plugins/filter')(this);
  require('../plugins/generator')(this);
  require('../plugins/helper')(this);
  require('../plugins/processor')(this);
  require('../plugins/renderer')(this);
  require('../plugins/tag')(this);
```

載入 hexo 本身擁有的 plugin。

## hexo 本身擁有的 plugins

### 內建 plugin 架構

	./lib/plugins/
	├── console/
	│	├── clean.js
	│	├── config.js
	│	├── deploy.js
	│	├── generate.js
	│	├── help.js
	│	├── index.js
	│	├── init.js
	│	├── migrate.js
	│	├── new.js
	│	├── publish.js
	│	├── render.js
	│	└── version.js
	├── filter/
	│	├── after_post_render/
	│	│	├── excerpt.js
	│	│	├── external_link.js
	│	│	└── index.js
	│	├── before_post_render/
	│	│	├── backtick_code_block.js
	│	│	├── index.js
	│	│	└── titlecase.js
	│	├── index.js
	│	├── new_post_path.js
	│	└── post_permalink.js
	├── generator/
	│	├── asset.js
	│	├── index.js
	│	├── page.js
	│	└── post.js
	├── helper/
	│	├── css.js
	│	├── date.js
	│	├── favicon_tag.js
	│	├── feed_tag.js
	│	├── format.js
	│	├── fragment_cache.js
	│	├── gravatar.js
	│	├── image_tag.js
	│	├── index.js
	│	├── is.js
	│	├── js.js
	│	├── link_to.js
	│	├── list_archives.js
	│	├── list_categories.js
	│	├── list_posts.js
	│	├── list_tags.js
	│	├── mail_to.js
	│	├── markdown.js
	│	├── number_format.js
	│	├── open_graph.js
	│	├── paginator.js
	│	├── partial.js
	│	├── render.js
	│	├── search_form.js
	│	├── tagcloud.js
	│	├── toc.js
	│	└── url.js
	├── processor/
	│	├── asset.js
	│	├── common.js
	│	├── index.js
	│	└── post.js
	├── renderer/
	│	├── html.js
	│	├── index.js
	│	├── json.js
	│	├── swig.js
	│	└── yaml.js
	└── tag/
		├── asset_img.js
		├── asset_link.js
		├── asset_path.js
		├── blockquote.js
		├── code.js
		├── gist.js
		├── iframe.js
		├── img.js
		├── include_code.js
		├── index.js
		├── jsfiddle.js
		├── link.js
		├── post_link.js
		├── post_path.js
		├── pullquote.js
		├── raw.js
		├── vimeo.js
		└── youtube.js

### console

也就是 hexo 的指令，在 `./lib/plugins/console/index.js` 中逐一註冊各指令，其中第 2 行：

``` js ./lib/plugins/console/index.js
  var console = ctx.extend.console;
```

`ctx` 從 `./lib/hexo/index.js` 的 ` require('../plugins/console')(this);` 進來時是 `Hexo` 本身，我們可以從 `./lib/plugins/console/index.js` 看到內建的指令有：

* clean
* config
* deploy
* generate
* help
* init
* list
* migrate
* new
* publish
* render
* version

從[文件](http://hexo.io/docs/plugins.html#Console)雖然可以知道大概的註冊方法，但是設定 `option` 還是得從 `./lib/hexo/index.js` 第 55 行 `    console: new extend.Console(),` 開始找起。

### filter

在 `./lib/plugins/filter/index.js` 有：

``` js ./lib/plugins/filter/index.js
  require('./after_post_render')(ctx);
  require('./before_post_render')(ctx);
```

遞迴註冊 filter。內建的 filter：

* new_post_path
* post_permalink
* after_post_render：在 md 檔被 render 後執行
	* excerpt
	* external_link
* before_post_render：在 md 檔被 render 前執行
	* backtick_code_block
	* titlecase

### generator

* asset
* page
* post

### helper

在 `./date.js` 內：

* date
* date_xml
* time
* full_date
* time_tag
* moment

在 `./format.js` 內：

* strip_html
* trim
* titlecase
* word_wrap
* truncate

在 `./is.js` 內：

* is_current
* is_home
* is_post
* is_archive
* is_year
* is_month
* is_category
* is_tag

在 `./tagcloud.js` 內：

* tagcloud
* tag_cloud (同上)

在 `./url.js` 內：

* relative_url
* url_for

連結工具：

* css
* js
* link_to
* mail_to
* image_tag
* favicon_tag
* feed_tag

list 系列：

* list_archives
* list_categories
* list_tags
* list_posts

其他：

* search_form
* fragment_cache (傳入參數 ctx)
* gravatar
* open_graph
* number_format
* paginator
* partial (傳入參數 ctx)
* markdown
* render (傳入參數 ctx)
* toc

### processor

* asset
* post

### renderer

根據[文件](http://hexo.io/docs/plugins.html#Renderer)，註冊方式有四個參數：

1. 輸入檔案格式
2. 輸出檔案格式
3. render 函數
4. 是否同步

直接貼出 `./lib/plugins/renderer/index.js`：

``` js ./lib/plugins/renderer/index.js
  var renderer = ctx.extend.renderer;
  var html = require('./html');
  renderer.register('htm', 'html', html, true);
  renderer.register('html', 'html', html, true);
  renderer.register('json', 'json', require('./json'), true);
  renderer.register('swig', 'html', require('./swig'), true);
  var yml = require('./yaml');
  renderer.register('yml', 'json', yml, true);
  renderer.register('yaml', 'json', yml, true);
```

### tag

類似就不整理了。