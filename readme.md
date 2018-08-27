## 一. 说明

以下内容大部分引用Laravel China社区的文章 - [分享下团队的开发规范 ——《Laravel 项目开发规范》](https://link.jianshu.com?t=https%3A%2F%2Flaravel-china.org%2Ftopics%2F5599%2Fshare-the-development-specification-of-the-team-laravel-project-development-specification)。

相对而言，上面引用的文章的规范更加严格，但考虑到目前的情况，会适当地对一些规范进行更改和增删。

## 二. 目的

暂无

## 三. 优点

规范有一下优点：

> *   高效编码 - 避免了过多的选择造成的『决策时间』浪费；
> *   风格统一 - 最大程度统一了开发团队成员代码书写风格和思路，代码阅读起来如出一辙；
> *   减少错误 - 减小初级工程师的犯错几率。

## 四. 开发哲学

> *   DRY –「Don't Repeat Yourself」不写重复的逻辑代码；
> *   约定俗成 - 「Convention Over Configuration」，优先选择框架提倡的做法，不过度配置；
> *   KISS - 「Keep it Simple, Stupid」提倡简单易读的代码，不写高深、晦涩难懂的代码，**不过度设计**；
> *   主厨精选 - 让有经验的人来为你选择方案，不独创方案；
> *   官方提倡 - 优先选择官方推崇的方案。

## 五. 设计理念

以下是一些优秀的『程序设计理念』：

> *   MVC - Model, View, Controller ，以 MVC 为核心，严格控制 Controller 的可读性和代码行数；
> *   Restful - 利用『资源化概念』和标准的 HTTP 动词来组织你的程序；

在此规范中，我们会将使用这两套理念作为程序设计基础。这些设计理念为我们设计程序提供了依据，遵循这些理念，能让程序变得清晰易读。

## 六. 能愿动词

为了避免歧义，文档大量使用了「能愿动词」，对应的解释如下：

> *   必须（Must） - 只能这样子做，请无条件遵循，没有别的选项；
> *   绝不（Must Not）- 严令禁止，在任何情况下都不能这样做；
> *   应该（Should） - 强烈建议这样做，但是不强求；
> *   不应该（Should Not） - 强烈建议不这样做，但是不强求；
> *   可以（May） - 选择性高一点，在这个文档内，此词语使用较少；

## 七. 关于Laravel版本的选择

选择Laravel版本时，**应该** 优先选择LTS版本，除非有特殊原因，如生产服务器的PHP版本不是PHP7以上，而是PHP5.*，且为了稳定不升级到PHP7，那么 **可以** 考虑使用上一个版本的发行版。

比如Laravel 5.5是最新的LTS但是只支持PHP7以上，那么 **可以** 考虑使用Laravel 5.4。当然比较可以使用Laravel 5.1 LTS版本，但是该版本比较旧，没有新版本的一些新特性。

请使用以下命令来创建指定版本的 Laravel 项目：

   ```
   composer create-project laravel/laravel project-name --prefer-dist 5.1.*
   ```
   **绝不** 也禁止使用复制粘贴项目文件的方式来创建项目。

   ## 八. 环境说明

   一般情况下，一个项目 **应该** 有以下三个基本的项目环境：
   > *   Local - 开发环境
   > *   Staging - 线上测试环境，对应git的test分支
   > *   Production - 线上生产环境，对应git的master分支

   ## 九. git分支

   在创建git仓库后，建议最好分开三个分支

   > *   主分支 - master，对应开发环境
   > *   测试分支 - test，对应线上测试环境
   > *   开发分支 - develop，对应线上生产环境

   所有功能都是从develop分支新建分支，按功能模块命名

*   新的功能模块，使用 `features/功能名称` 来命名
*   修复bug，使用 `fix/bug名称` 来命名
*   功能开发后，合并到develop
*   开发测试通过后，将develop合并到test分支
*   测试环境功能测试通过后，将test合并到master

    ## 十. 配置信息与环境变量

    在 Laravel 中有以下几种方法：

1.  硬代码，直接写死。- ❌ 可维护性低
2.  写死在 `config/app.php` 文件中。 - ❌ 无法区分环境进行配置
3.  存储于 `.env` 文件中，使用 `env()` 方法直接读取。 - ❌ 虽然解决了环境变量问题但是不推荐
4.  存储在 `.env` 和 `config/app.php` 文件中，然后使用 `config()` 函数来读取。- ✅ 最佳实践

    第一种方法是最古老的方法，代码可维护性极低，一旦域名变更就只能全局替换。

    第二种方法无法区分环境，例如本地使用开发环境域名测试，线上才是正式的域名。

    第三种方法虽然解决了环境变量的问题，并且也具备一定的灵活性，但是不够灵活，假如你的网站流量巨大，需要配置几个域名，使其在加载静态资源时随机支配域名，这种做法就无法满足需求了。

    第四种方法既支持环境变量，又具备极高的灵活性，假如遇到同样的多域名随机问题，你只需要写一个辅助方法，然后在 `config/app.php` 中调用即可，不需要动到任何一行业务逻辑代码。

    代码示例

    `.env` 文件中设置：

    ```
    DOMAIN=018eighteen.test
    ```
   
    `config/app.php` 文件中设置：

    ```
    'domain' => env('DOMAIN', '018eighteen.com'),
    ```

    程序中两种获取 相同配置 的方法：

    1.  `env('DOMAIN')`
    2.  `config('app.domain')`

    在此统一规定：所有程序配置信息 **必须** 通过 `config()` 来读取，所有的 `.env` 配置信息 **必须** 通过 `config()` 来读取，**绝不** 在配置文件以外的范围使用 `env()`。

    这样做主要有以下几个优势：

    1.  定义分明，`config()` 是配置信息，`env()` 只是用来区分不同环境；
    2.  统一放置于 `config` 中还可以利用框架的 [配置信息缓存功能](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.5%2Finstallation%23configuration-caching) 来提高运行效率；
    3.  代码健壮性， `config()` 在 `env()` 之上多出来一个抽象层，会使代码更加健壮，更加灵活。

    ## 十一. 路由器

    #### 1. 路由闭包

    **绝不** 在路由配置文件里书写『闭包路由』或者其他业务逻辑代码，因为一旦使用将无法使用 [路由缓存](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.5%2Fcontrollers%23route-caching) 。

    路由器要保持干净整洁，**绝不** 放置除路由配置以外的其他程序逻辑。

    #### 2. Restful 路由

    **必须** 优先使用 Restful 路由，配合资源控制器使用，见 [文档](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.5%2Fcontrollers%23RESTful-%25E8%25B5%2584%25E6%25BA%2590%25E6%258E%25A7%25E5%2588%25B6%25E5%2599%25A8)。

    [](https://link.jianshu.com?t=https%3A%2F%2Ffsdhubcdn.phphub.org%2Fuploads%2Fimages%2F201705%2F19%2F1%2F09GHC72ygP.png)

    [](https://link.jianshu.com?t=https%3A%2F%2Ffsdhubcdn.phphub.org%2Fuploads%2Fimages%2F201705%2F19%2F1%2F09GHC72ygP.png)
    <div class="image-package">
    <div class="image-container" style="max-width: 700px; max-height: 393px; background-color: transparent;">
    <div class="image-container-fill" style="padding-bottom: 45.28%;"></div>
    <div class="image-view" data-width="868" data-height="393">![](//upload-images.jianshu.io/upload_images/10019056-3f6939ed07c43f15.png)</div>
    </div>
    [<div class="image-caption">file</div>](https://link.jianshu.com?t=https%3A%2F%2Ffsdhubcdn.phphub.org%2Fuploads%2Fimages%2F201705%2F19%2F1%2F09GHC72ygP.png)
    </div>

    超出 Restful 路由的，**应该** 模仿上图的方式来定义路由。

    #### 3. resource 方法正确使用

    一般资源路由定义：

    <pre class="hljs php">`Route::resource(<span class="hljs-string">'photos'</span>, <span class="hljs-string">'PhotosController'</span>);
    `</pre>

    等于以下路由定义：

    <pre class="hljs php">`Route::get(<span class="hljs-string">'/photos'</span>, <span class="hljs-string">'PhotosController@index'</span>)-&gt;name(<span class="hljs-string">'photos.index'</span>);
    Route::get(<span class="hljs-string">'/photos/create'</span>, <span class="hljs-string">'PhotosController@create'</span>)-&gt;name(<span class="hljs-string">'photos.create'</span>);
    Route::post(<span class="hljs-string">'/photos'</span>, <span class="hljs-string">'PhotosController@store'</span>)-&gt;name(<span class="hljs-string">'photos.store'</span>);
    Route::get(<span class="hljs-string">'/photos/{photo}'</span>, <span class="hljs-string">'PhotosController@show'</span>)-&gt;name(<span class="hljs-string">'photos.show'</span>);
    Route::get(<span class="hljs-string">'/photos/{photo}/edit'</span>, <span class="hljs-string">'PhotosController@edit'</span>)-&gt;name(<span class="hljs-string">'photos.edit'</span>);
    Route::put(<span class="hljs-string">'/photos/{photo}'</span>, <span class="hljs-string">'PhotosController@update'</span>)-&gt;name(<span class="hljs-string">'photos.update'</span>);
    Route::delete(<span class="hljs-string">'/photos/{photo}'</span>, <span class="hljs-string">'PhotosController@destroy'</span>)-&gt;name(<span class="hljs-string">'photos.destroy'</span>);
    `</pre>

    使用 `resource` 方法时，如果仅使用到部分路由，**必须** 使用 `only` 列出所有可用路由：

    <pre class="hljs php">`Route::resource(<span class="hljs-string">'photos'</span>, <span class="hljs-string">'PhotosController'</span>, [<span class="hljs-string">'only'</span> =&gt; [<span class="hljs-string">'index'</span>, <span class="hljs-string">'show'</span>]]);
    `</pre>

    **绝不** 使用 `except`，因为 `only` 相当于白名单，相对于 `except` 更加直观。路由使用白名单有利于养成『安全习惯』。

    #### 4. 单数 or 复数？

    资源路由路由 URI **必须** 使用复数形式，如：

*   `/photos/create`
*   `/photos/{photo}`

    错误的例子如：

*   `/photo/create`
*   `/photo/{photo}`

    #### 5. 路由命名

    除了 `resource` 资源路由以外，其他所有路由都 **必须** 使用 `name` 方法进行命名。

    **必须** 使用『资源前缀』作为命名规范，如下的 `users.follow`，资源前缀的值是 `users.`：

    <pre class="hljs php">`Route::post(<span class="hljs-string">'users/{id}/follow'</span>, <span class="hljs-string">'UsersController@follow'</span>)-&gt;name(<span class="hljs-string">'users.follow'</span>);
    `</pre>

    #### 6. 获取 URL

    获取 URL **必须** 遵循以下优先级：

1.  `$model-&gt;link()`
2.  `route` 方法
3.  `url` 方法

    在 Model 中创建 `link()` 方法：

    <pre class="hljs php">`<span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">link</span><span class="hljs-params">($params = [])</span>
    </span>{
        $params = array_merge([<span class="hljs-keyword">$this</span>-&gt;id], $params);
        <span class="hljs-keyword">return</span> route(<span class="hljs-string">'models.show'</span>, $params);
    }
    `</pre>

    所有单个模型数据链接使用：

    <pre class="hljs php">`$model-&gt;link();

    <span class="hljs-comment">// 或者添加参数</span>
    $model-&gt;link($params = [<span class="hljs-string">'source'</span> =&gt; <span class="hljs-string">'list'</span>])
    `</pre>

    『单个模型 URI』经常会发生变化，这样做将会让程序更加灵活。

    除了『单个模型 URI』，其他路由 **必须** 使用 `route` 来获取 URL（这也是目前使用次数最多的方法）：

    <pre class="hljs php">`$url = route(<span class="hljs-string">'profile'</span>, [<span class="hljs-string">'id'</span> =&gt; <span class="hljs-number">1</span>]);
    `</pre>

    无法使用 `route` 的情况下，**可以** 使用 `url` 方法来获取 URL：

    <pre class="hljs bash">`url(<span class="hljs-string">'profile'</span>, [1]);
    `</pre>

    ## 十二. 数据模型

    #### 1. 放置位置

    所有的数据模型文件，都 **必须** 存放在：`app/Models/` 文件夹中。

    命名空间：

    <pre class="hljs php">`<span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Models</span>;
    `</pre>

    #### 2. 使用基类

    所有的 **Eloquent 数据模型** 都 **必须** 继承统一的基类 `App/Models/Model`，此基类存放位置为 `/app/Models/Model.php`，内容参考以下：

    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span>

    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Models</span>;

    <span class="hljs-keyword">use</span> <span class="hljs-title">Illuminate</span>\<span class="hljs-title">Database</span>\<span class="hljs-title">Eloquent</span>\<span class="hljs-title">Model</span> <span class="hljs-title">as</span> <span class="hljs-title">EloquentModel</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Model</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">EloquentModel</span>
    </span>{
        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">scopeRecent</span><span class="hljs-params">($query)</span>
        </span>{
            <span class="hljs-keyword">return</span> $query-&gt;orderBy(<span class="hljs-string">'created_at'</span>, <span class="hljs-string">'desc'</span>);
        }
    }
    `</pre>

    以 Photo 数据模型作为例子继承 Model 基类：

    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span>

    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Models</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Photo</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">Model</span>
    </span>{
        <span class="hljs-keyword">protected</span> $fillable = [<span class="hljs-string">'id'</span>, <span class="hljs-string">'user_id'</span>];

        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">user</span><span class="hljs-params">()</span>
        </span>{
            <span class="hljs-keyword">return</span> <span class="hljs-keyword">$this</span>-&gt;belongsTo(User::class);
        }
    }
    `</pre>

    #### 3. 命名规范[#]

    数据模型相关的命名规范：

*   数据模型类名 `必须` 为「单数」, 如：`App\Models\Photo`
*   类文件名 `必须` 为「单数」，如：`app/Models/Photo.php`
*   数据库表名字 `必须` 为「复数」，多个单词情况下使用「[Snake Case](https://link.jianshu.com?t=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSnake_case)」 如：`photos`, `my_photos` （注：目前由于和其他团队合作开发，所以这一条规范暂时不硬性要求）
*   数据库表迁移名字 `必须` 为「复数」，如：`2014_08_08_234417_create_photos_table.php`
*   数据填充文件名 `必须` 为「复数」，如：`PhotosTableSeeder.php`
*   数据库字段名 `必须` 为「[Snake Case](https://link.jianshu.com?t=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSnake_case)」，如：`view_count`, `is_vip`
*   数据库表主键 `必须` 为「id」（注：这条规范一定要严格执行，避免像018server的`prouct`表一样出现`product_id`这样的主键）
*   数据库表外键 `必须` 为「resource_id」，如：`user_id`, `post_id`
*   数据模型变量 `必须` 为「resource_id」，如：`$user_id`, `$post_id`

    #### 4. 利用 Trait 来扩展数据模型

    有时候数据模型里的代码会变得很臃肿，**应该** 利用 Trait 来精简逻辑代码量，提高可读性，类似于 [Ruby China 源码](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fruby-china%2Fruby-china%2Fblob%2Fmaster%2Fapp%2Fmodels%2Ftopic.rb%23L11-L17)。

    > 借鉴于 Rails 的设计理念：「Fat Models, Skinny Controllers」。

    存放于文件夹：`app/Models/Traits` 文件夹中。

    #### 5. Repository

    在[分享下团队的开发规范 ——《Laravel 项目开发规范》](https://link.jianshu.com?t=https%3A%2F%2Flaravel-china.org%2Ftopics%2F5599%2Fshare-the-development-specification-of-the-team-laravel-project-development-specification) 提出不适用 `Repository` 模式进行开发，但是考虑到随着功能越来越多，不适用 `Repository` 会使得控制器越来越臃肿，有些代码也会不停地重复写，另外以后有可能需要编写单元测试，所以最后还是决定启用 `Repository` 。

    具体参照[为什么你应该使用 Repository](https://link.jianshu.com?t=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000003488038)

    #### 6. 关于 SQL 文件

*   **绝不** 使用命令行或者 PHPMyAdmin 直接创建索引或表。**必须** 使用 [数据库迁移](https://link.jianshu.com?t=http%3A%2F%2Flaravel-china.org%2Fdocs%2F5.5%2Fmigrations) 去创建表结构，并提交版本控制器中；
*   **绝不** 为了共享对数据库更改就直接导出 SQL，所有修改都 **必须** 使用 [数据库迁移](https://link.jianshu.com?t=http%3A%2F%2Flaravel-china.org%2Fdocs%2F5.5%2Fmigrations) ，并提交版本控制器中；
*   **绝不** 直接向数据库手动写入伪造的测试数据。**必须** 使用 [数据填充](https://link.jianshu.com?t=http%3A%2F%2Flaravel-china.org%2Fdocs%2F5.5%2Fseeding) 来插入假数据，并提交版本控制器中。

    考虑到可能会和其他团队合作开发，所以具体还是根据团队的协定而定。但是如果是自己团队开发的话，必须严格按照以上标准。

    ## 十三. 控制器

    #### 1. 资源控制器

    **必须** 优先使用 [Restful 资源控制器](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.5%2Fcontrollers%23restful-resource-controllers) 。

    #### 2. 单数 or 复数？

    **必须** 使用资源的复数形式，如：

    > *   类名：PhotosController
> *   文件名：PhotosController.php

    错误的例子：

    > *   类名：PhotoController
> *   文件名：PhotoController.php

    #### 3. 保持短小精炼

    **必须** 保持控制器文件代码行数最小化，还有可读性。

*   **不应该** 为「方法」书写注释，这要求方法取名要足够合理，不需要过多注释；
*   **应该** 为一些复杂的逻辑代码块书写注释，主要介绍产品逻辑 - `为什么要这么做。`；
*   **不应该** 在控制器中书写「私有方法」，控制器里 `应该` 只存放「路由动作方法」；
*   **绝不** 遗留「死方法」，就是没有用到的方法，控制器里的所有方法，都应该被使用到，否则应该删除；
*   **绝不** 在控制器里批量注释掉代码，无用的逻辑代码就必须清除掉。
*   **应该** 创建Service层建立对应的Service类，以实现控制器对应的逻辑，见下放关于Service

    ## 十四. Service

    在上面提过决定启用 `Repository` ，`Repository`主要是实现对model的增删改查。

    而 `Service` 则是介于 `Controller` 与 `Repository` 之间，是对 `Controller` 业务逻辑的实现，实现过程中，通过 `Repository` 来对数据进行操作。

    #### 1. 创建 `Service` 文件夹

    首先我们需要在 `app` 文件夹创建自己 `Service` 文件夹 `services`，然后文件夹的每一个文件都要设置相应的命名空间。

    #### 2. 创建对应的 `Service` 类

    `PostsController.php`

    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span> 
    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Controllers</span>;

    <span class="hljs-keyword">use</span> <span class="hljs-title">App</span>\<span class="hljs-title">Http</span>\<span class="hljs-title">Controllers</span>\<span class="hljs-title">Controller</span>;
    <span class="hljs-keyword">use</span> <span class="hljs-title">App</span>\<span class="hljs-title">Services</span>\<span class="hljs-title">PostsService</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">PostsController</span> <span class="hljs-title">extend</span> <span class="hljs-title">Controller</span></span>{

        <span class="hljs-keyword">private</span> $postsService;

        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">__construct</span> <span class="hljs-params">(PostsService $posts)</span> </span>{
            <span class="hljs-keyword">$this</span>-&gt;postsService = $posts;
        }

            <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">addArticle</span> <span class="hljs-params">(Request $request)</span> </span>{
                <span class="hljs-keyword">return</span> <span class="hljs-keyword">$this</span>-&gt;postsService-&gt;addArticle ($request-&gt;all());
            }

    }
    `</pre>

    `PostsService.php`：

    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span> 
    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Services</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">PostsService</span></span>{

            <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">addArticle</span> <span class="hljs-params">($data)</span> </span>{
                <span class="hljs-comment">//To add a article...</span>
            }

    }
    `</pre>

    ## 十五. 视图

    在不进行前后端分离的情况下，请使用视图

    #### 1. 优先使用 Blade

    视图文件 **必须** 优先考虑使用 `.blade.php` 后缀来指定使用 Blade 模板引擎。

    #### 2. 保持目录清晰

*   layouts - 页面布局文件 **必须** 放置于此目录下；
*   common - 存放页面通用元素；
*   pages - 简单的页面存放文件夹，如：about、contact 等；
*   resources - 对应 Restful 路由的资源路径名称，以 URI `photos/create` 为例，对应 `create.blade.php` 文件，存放在文件夹 `photos` 下。

    必须 避免在 `resources/views` 目录下直接放置视图文件。

    #### 3. 局部视图

    局部视图文件 **必须** 使用 `_` 前缀来命名，如：`photos/_upload_form.blade.php`

    #### 4. 视图命名要释义

    为了和 Restful 路由器和资源控制器保持一致，视图命名也 **必须** 使用资源视图的命名方式。以 `photos` 为例：

*   `photos/index.blade.php`

        *   内容列表视图
    *   对应路由器 `/photos`，命名 `photos.index`
    *   控制器方法 `PhotosController@index`
*   `photos/show.blade.php`

        *   单个内容视图
    *   对应路由器 `/photos/{id}`，命名 `photos.show`
    *   控制器方法 `PhotosController@show`
*   `photos/create.blade.php`

        *   内容创建视图
    *   对应路由器 `/photos/create`，命名 `photos.create`
    *   控制器方法 `PhotosController@create`
*   `photos/edit.blade.php`

        *   内容编辑的视图
    *   对应路由器 `/photos/edit`，命名 `photos.edit`
    *   控制器方法 `PhotosController@edit`

    #### 5. `create_and_edit` 视图

    很多情况下，创建和编辑视图里的页面结构接近相似，在这种情况下，**应该** 使用 `create_and_edit` 视图。以 `photos` 为例：

*   `PhotosController@create` - 对应视图：`/photos/create_and_edit.blade.php`
*   `PhotosController@edit` - 对应 视图：`/photos/create_and_edit.blade.php`

    这样一来，通常情况下，一个完整的 `photos` 资源对应的视图文件为以下：

    <pre class="hljs css">`├── <span class="hljs-selector-tag">photos</span>
    │   ├── <span class="hljs-selector-tag">create_and_edit</span><span class="hljs-selector-class">.blade</span><span class="hljs-selector-class">.php</span>
    │   ├── <span class="hljs-selector-tag">index</span><span class="hljs-selector-class">.blade</span><span class="hljs-selector-class">.php</span>
    │   └── <span class="hljs-selector-tag">show</span><span class="hljs-selector-class">.blade</span><span class="hljs-selector-class">.php</span>
    `</pre>

    ## 十六. 表单验证

    #### 1. 表单请求验证类

    **必须** 使用 [表单请求 - FormRequest 类](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.5%2Fvalidation%23form-request-validation) 来处理控制器里的表单验证。

    ### 2. 使用基类

    所有 FormRequest 表验证类 **必须** 继承 `app/Http/Requests/Request.php` 基类。基类文件如下：

    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span>

    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Http</span>\<span class="hljs-title">Requests</span>;

    <span class="hljs-keyword">use</span> <span class="hljs-title">Illuminate</span>\<span class="hljs-title">Foundation</span>\<span class="hljs-title">Http</span>\<span class="hljs-title">FormRequest</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Request</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">FormRequest</span>
    </span>{
        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">authorize</span><span class="hljs-params">()</span>
        </span>{
            <span class="hljs-comment">// Using policy for Authorization</span>
            <span class="hljs-keyword">return</span> <span class="hljs-keyword">true</span>;
        }
    }
    `</pre>

    #### 3. 验证类命名

    FormRequest 表验证类 **必须** 遵循 **资源路由** 方式进行命名，`photos` 对应 `app/Http/Requests/PhotoRequest.php` 。

    #### 4. 类文件参考

    FormRequest 表验证类文件请参考以下：

    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span>

    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Http</span>\<span class="hljs-title">Requests</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">PhotoRequest</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">Request</span>
    </span>{
        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">rules</span><span class="hljs-params">()</span>
        </span>{
            <span class="hljs-keyword">switch</span>(<span class="hljs-keyword">$this</span>-&gt;method())
            {
                <span class="hljs-comment">// CREATE</span>
                <span class="hljs-keyword">case</span> <span class="hljs-string">'POST'</span>:
                {
                    <span class="hljs-keyword">return</span> [
                        <span class="hljs-comment">// CREATE ROLES</span>
                    ];
                }
                <span class="hljs-comment">// UPDATE</span>
                <span class="hljs-keyword">case</span> <span class="hljs-string">'PUT'</span>:
                <span class="hljs-keyword">case</span> <span class="hljs-string">'PATCH'</span>:
                {
                    <span class="hljs-keyword">return</span> [
                        <span class="hljs-comment">// UPDATE ROLES</span>
                    ];
                }
                <span class="hljs-keyword">case</span> <span class="hljs-string">'GET'</span>:
                <span class="hljs-keyword">case</span> <span class="hljs-string">'DELETE'</span>:
                <span class="hljs-keyword">default</span>:
                {
                    <span class="hljs-keyword">return</span> [];
                };
            }
        }

        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">messages</span><span class="hljs-params">()</span>
        </span>{
            <span class="hljs-keyword">return</span> [
                <span class="hljs-comment">// Validation messages</span>
            ];
        }
    }
    `</pre>

    ## 十七. 数据填充

    #### 1. factory 辅助函数

    `必须` 使用 `factory` 方法来做数据填充，因为是框架提倡的，并且可以同时为测试代码服务。

    #### 2. 运行效率

    开发数据填充时，`必须` 特别注意 `php artisan db:seed` 的运行效率，否则随着项目的代码量越来越大，`db:seed` 的运行时间会变得越来越长，有些项目多达几分钟甚至几十分钟。

    原则是：

    > Keep it lighting speed.

    只有当 `db:seed` 运行起来很快的时候，才能完全利用数据填充工具带来的便利，而不是累赘。

    #### 4. 批量入库

    所有假数据入库操作，都 **必须** 是批量操作，配合 `factory` 使用以下方法：

    <pre class="hljs php">`$users = factory(User::class)-&gt;times(<span class="hljs-number">1000</span>)-&gt;make();
    User::insert($users-&gt;toArray());
    `</pre>

    以上只执行一条数据库语句，推荐阅读 [大批量假数据填充的正确方法](https://link.jianshu.com?t=https%3A%2F%2Flaravel-china.org%2Ftopics%2F2066%2Fthe-correct-method-for-filling-large-quantity-of-false-data)。

    ## 十八. Artisan 命令行

    所有的自定义命令，都 必须 有项目的命名空间。

    如：

    <pre class="hljs css">`<span class="hljs-selector-tag">php</span> <span class="hljs-selector-tag">artisan</span> <span class="hljs-selector-tag">phphub</span><span class="hljs-selector-pseudo">:clear-token</span>
    <span class="hljs-selector-tag">php</span> <span class="hljs-selector-tag">artisan</span> <span class="hljs-selector-tag">phphub</span><span class="hljs-selector-pseudo">:send-status-email</span>
    ...
    `</pre>

    错误的例子为：

    <pre class="hljs undefined">`php artisan clear-token
    php artisan send-status-email
    ...
    `</pre>

    ## 十九. 日期和时间

    **必须** 使用 [Carbon](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fbriannesbitt%2FCarbon) 来处理日期和时间相关的操作。

    Laravel 5.1 中文的 `diffForHumans` 可以使用 [jenssegers/date](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fjenssegers%2Fdate)。

    Laravel 5.3 及以上版本的 `diffForHumans`，只需要在 `config/app.php` 文件中配置 `locale` 选项即可 ：

    <pre class="hljs php">`<span class="hljs-string">'locale'</span> =&gt; <span class="hljs-string">'zh-CN'</span>,
    `</pre>

    ## 二十. 前端开发

    根据 [分享下团队的开发规范 ——《Laravel 项目开发规范》](https://link.jianshu.com?t=https%3A%2F%2Flaravel-china.org%2Ftopics%2F5599%2Fshare-the-development-specification-of-the-team-laravel-project-development-specification)，规范里这么写的：

*   **必须** 使用 Laravel 官方前端工具做前端开发自动化；
*   **必须** 保证页面只加载一个 `.css` 文件；
*   **必须** 保证页面只加载一个 `.js` 文件；
*   **必须** 为 `.css` 和 `.js` 增加 [版本控制](https://link.jianshu.com?t=http%3A%2F%2Flaravel-china.org%2Fdocs%2F5.5%2Felixir%23%25E7%2589%2588%25E6%259C%25AC%25E4%25B8%258E%25E7%25BC%2593%25E5%25AD%2598%25E6%25B8%2585%25E9%2599%25A4)；
*   **必须** 使用 [SASS](https://link.jianshu.com?t=http%3A%2F%2Flaravel-china.org%2Fdocs%2F5.5%2Felixir%23sass) 来书写 CSS 代码；

    但是考虑到目前团队的情况，在不前后端分离的情况下可以不执行以上规范。

    如果实行前后端分离，则前端必须使用vue脚手架，并生成静态文件，增加版本控制。

    ## 二十一. Laravel 安全实践

    #### 1. 说明

    没有绝对安全，只有相对安全。Laravel 相较于其他框架在安全方面已经做得很优秀，不过作为开发者，我们要在日常开发中对『安全』需怀着敬畏之心，积极培养自己的安全意识。以下是一些 Laravel 安全相关的规范。

    #### 2. 关闭 DEBUG

    Laravel Debug 开启时，会暴露很多能被黑客利用的服务器信息，所以，生产环境下请 **必须** 确保：

    <pre class="hljs bash">`APP_DEBUG=<span class="hljs-literal">false</span>
    `</pre>

    #### 3. XSS

    跨站脚本攻击（cross-site scripting，简称 XSS），具体危害体现在黑客能控制你网站页面，包括使用 JS 盗取 Cookie 等，关于 XSS 的介绍请前往 [IBM 文档库：跨站点脚本攻击深入解析](https://link.jianshu.com?t=https%3A%2F%2Fwww.ibm.com%2Fdeveloperworks%2Fcn%2Frational%2F08%2F0325_segal%2F) 。

    默认情况下，在无法保证用户提交内容是 100% 安全的情况下，**必须** 使用 Blade 模板引擎的 `{{ $content }}` 语法会对用户内容进行转义。

    Blade 的 `{!! $content !!}` 语法会直接对内容进行 **非转义** 输出，使用此语法时，**必须** 使用 [HTMLPurifier for Laravel 5](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fmewebstudio%2FPurifier) 来为用户输入内容进行过滤。使用方法参见： [使用 HTMLPurifier 来解决 Laravel 5 中的 XSS 跨站脚本攻击安全问题](https://link.jianshu.com?t=https%3A%2F%2Flaravel-china.org%2Farticles%2F4798%2Fthe-use-of-htmlpurifier-to-solve-the-xss-xss-attacks-of-security-problems-in-laravel)

    #### 4. SQL 注入

    Laravel 的 [查询构造器](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.3%2Fqueries) 和 [Eloquent](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.3%2Feloquent) 是基于 PHP 的 PDO，PDO 使用 `prepared` 来准备查询语句，保障了安全性。

    在使用 `raw()` 来编写复杂查询语句时，**必须** 使用数据绑定。

    错误的做法：

    <pre class="hljs php">`Route::get(<span class="hljs-string">'sql-injection'</span>, <span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">()</span> </span>{
        $name = <span class="hljs-string">"admin"</span>; <span class="hljs-comment">// 假设用户提交</span>
        $password = <span class="hljs-string">"xx' OR 1='1"</span>; <span class="hljs-comment">// // 假设用户提交</span>
        $result = DB::select(DB::raw(<span class="hljs-string">"SELECT * FROM users WHERE name ='$name' and password = '$password'"</span>));
        dd($result);
    });
    `</pre>

    以下是正确的做法，利用 [select 方法](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fapi%2F5.3%2FIlluminate%2FDatabase%2FConnectionInterface.html%23method_select) 的第二个参数做数据绑定：

    <pre class="hljs php">`Route::get(<span class="hljs-string">'sql-injection'</span>, <span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">()</span> </span>{
        $name = <span class="hljs-string">"admin"</span>; <span class="hljs-comment">// 假设用户提交</span>
        $password = <span class="hljs-string">"xx' OR 1='1"</span>; <span class="hljs-comment">// // 假设用户提交</span>
        $result = DB::select(
            DB::raw(<span class="hljs-string">"SELECT * FROM users WHERE name =:name and password = :password"</span>),
            [
                <span class="hljs-string">'name'</span> =&gt; $name,
                <span class="hljs-string">'password'</span> =&gt; $password,
            ]
        );
        dd($result);
    });
    `</pre>

    `DB` 类里的大部分执行 SQL 的函数都可传参第二个参数 `$bindings` ，详见：[API 文档](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fapi%2F5.3%2FIlluminate%2FDatabase%2FConnectionInterface.html) 。

    （注：建议最好直接使用Laravel的Eloquent ORM来对数据库进行操作）

    #### 5. 批量赋值

    Laravel 提供白名单和黑名单过滤（`$fillable` 和 `$guarded`），开发者 **应该** 清楚认识批量赋值安全威胁的情况下合理灵活地运用。

    批量赋值安全威胁，指的是用户可更新本来不应有权限更新的字段。举例，`users` 表里的 `is_admin` 字段是用来标识用户『是否是管理员』，某不怀好意的用户，更改了『修改个人资料』的表单，增加了一个字段：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">input</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"is_admin"</span> <span class="hljs-attr">value</span>=<span class="hljs-string">"1"</span> /&gt;</span>
    `</pre>

    这个时候如果你更新代码如下：

    <pre class="hljs php">`Auth::user()-&gt;update(Request::all());
    `</pre>

    此用户将获取到管理员权限。可以有很多种方法来避免这种情况出现，最简单的方法是通过设置 User 模型里的 `$guarded` 字段来避免：

    <pre class="hljs php">`<span class="hljs-keyword">protected</span> $guarded = [<span class="hljs-string">'id'</span>, <span class="hljs-string">'is_admin'</span>];
    `</pre>

    #### 6. CSRF

    CSRF 跨站请求伪造是 Web 应用中最常见的安全威胁之一，具体请见 [Wiki - 跨站请求伪造](https://link.jianshu.com?t=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E8%25B7%25A8%25E7%25AB%2599%25E8%25AF%25B7%25E6%25B1%2582%25E4%25BC%25AA%25E9%2580%25A0) 或者 [Web 应用程序常见漏洞 CSRF 的入侵检测与防范](https://link.jianshu.com?t=https%3A%2F%2Fwww.ibm.com%2Fdeveloperworks%2Fcn%2Frational%2Fr-cn-webcsrf%2F)。

    Laravel 默认对所有『非幂等的请求』强制使用 `VerifyCsrfToken` 中间件防护，需要开发者做的，是区分清楚什么时候该使用『非幂等的请求』。

    > 幂等请求指的是：'HEAD', 'GET', 'OPTIONS'，既无论你执行多少次重复的操作都不会给资源造成变更。

*   所有删除的动作，**必须** 使用 DELETE 作为请求方法；
*   所有对数据更新的动作，**必须** 使用 POST、PUT 或者 PATCH 请求方法。

    Laravel 自动为每一个被应用管理的有效用户会话生成一个 CSRF “令牌”，该令牌用于验证授权用户和发起请求者是否是同一个人。想要生成包含 CSRF 令牌的隐藏输入字段，可以使用帮助函数 csrf_field 来实现：

    <pre class="hljs xml">`<span class="php"><span class="hljs-meta">&lt;?php</span> <span class="hljs-keyword">echo</span> csrf_field(); <span class="hljs-meta">?&gt;</span></span>
    `</pre>

    辅助函数 csrf_field 会生成如下 HTML：

    <pre class="hljs bash">`&lt;input <span class="hljs-built_in">type</span>=<span class="hljs-string">"hidden"</span> name=<span class="hljs-string">"_token"</span> value=<span class="hljs-string">"&lt;?php echo csrf_token(); ?&gt;"</span>&gt;
    `</pre>

    当然还可以使用 Blade 模板引擎提供的方式：

    <pre class="hljs undefined">`{!! csrf_field() !!}
    `</pre>

    你不需要自己编写代码去验证 POST、PUT 或者 DELETE 请求的 CSRF 令牌，因为 Laravel 自带的 HTTP 中间件 VerifyCsrfToken 会为我们做这项工作：将请求中输入的 token 值和 Session 中的存储的 token 作对比来进行验证。

    **X-CSRF-Token**

    除了将 CSRF 令牌作为 POST 参数进行验证外，还可以通过设置 X-CSRF-Token 请求头来实现验证，VerifyCsrfToken 中间件会检查 X-CSRF-TOKEN 请求头，首先创建一个 meta 标签并将令牌保存到该 meta 标签：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"csrf-token"</span> <span class="hljs-attr">content</span>=<span class="hljs-string">"{{ csrf_token() }}"</span>&gt;</span>
    `</pre>

    然后在 js 库（如 jQuery）中添加该令牌到所有请求头，这为基于 AJAX 的应用提供了简单、方便的方式来避免 CSRF 攻击：

    <pre class="hljs javascript">`$.ajaxSetup({
        <span class="hljs-attr">headers</span>: {
            <span class="hljs-string">'X-CSRF-TOKEN'</span>: $(<span class="hljs-string">'meta[name="csrf-token"]'</span>).attr(<span class="hljs-string">'content'</span>)
        }
    });
    `</pre>

    ## 二十三. Laravel 程序优化

    #### 1. 配置信息缓存

    生产环境中的 **应该** 使用『配置信息缓存』来加速 Laravel 配置信息的读取。

    使用以下 Artisan 自带命令，把 `config` 文件夹里所有配置信息合并到一个文件里，减少运行时文件的载入数量：

    <pre class="hljs css">`<span class="hljs-selector-tag">php</span> <span class="hljs-selector-tag">artisan</span> <span class="hljs-selector-tag">config</span><span class="hljs-selector-pseudo">:cache</span>
    `</pre>

    缓存文件存放在 `bootstrap/cache/` 文件夹中。

    可以使用以下命令来取消配置信息缓存：

    <pre class="hljs css">`<span class="hljs-selector-tag">php</span> <span class="hljs-selector-tag">artisan</span> <span class="hljs-selector-tag">config</span><span class="hljs-selector-pseudo">:clear</span>
    `</pre>

    注意：配置信息缓存不会随着更新而自动重载，所以，开发时候建议关闭配置信息缓存，一般在生产环境中使用。可以配合 [Envoy 任务运行器](https://link.jianshu.com?t=https%3A%2F%2Fdoc.laravel-china.org%2Fdocs%2F5.5%2Fenvoy) 使用，在每次上线代码时执行 `config:clear` 命令。

    #### 2. 路由缓存

    生产环境中的 **应该** 使用『路由缓存』来加速 Laravel 的路由注册。

    路由缓存可以有效的提高路由器的注册效率，在大型应用程序中效果越加明显，可以使用以下命令：

    <pre class="hljs css">`<span class="hljs-selector-tag">php</span> <span class="hljs-selector-tag">artisan</span> <span class="hljs-selector-tag">route</span><span class="hljs-selector-pseudo">:cache</span>
    `</pre>

    缓存文件存放在 `bootstrap/cache/` 文件夹中。另外，路由缓存不支持路由匿名函数编写逻辑，详见：[文档 - 路由缓存](https://link.jianshu.com?t=https%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.5%2Fcontrollers%23route-caching)。

    可以使用下面命令清除路由缓存：

    <pre class="hljs css">`<span class="hljs-selector-tag">php</span> <span class="hljs-selector-tag">artisan</span> <span class="hljs-selector-tag">route</span><span class="hljs-selector-pseudo">:clear</span>
    `</pre>

    注意：路由缓存不会随着更新而自动重载，所以，开发时候建议关闭路由缓存，一般在生产环境中使用。可以配合 [Envoy 任务运行器](https://link.jianshu.com?t=https%3A%2F%2Fdoc.laravel-china.org%2Fdocs%2F5.5%2Fenvoy) 使用，在每次上线代码时执行 `route:clear` 命令。

    #### 3. 类映射加载优化

    `optimize` 命令把常用加载的类合并到一个文件里，通过减少文件的加载，来提高运行效率。生产环境中的 **应该**使用 optimize 命令来优化类的加载速度：

    <pre class="hljs undefined">`php artisan optimize --force
    `</pre>

    以上命令会在 `bootstrap/cache/` 文件夹中生成缓存文件。你可以可以通过修改 `config/compile.php` 文件来添加要合并的类。在 `production` 环境中，参数 `--force` 不需要指定，文件就会自动生成。

    要清除类映射加载优化，请运行以下命令：

    <pre class="hljs undefined">`php artisan clear-compiled
    `</pre>

    此命令会删除上面 `optimize` 生成的两个文件。

    注意：此命令要运行在 `php artisan config:cache` 后，因为 `optimize` 命令是根据配置信息（如：`config/app.php` 文件的 `providers` 数组）来生成文件的。

    #### 4. 自动加载优化

    此命令不止针对于 Laravel 程序，适用于所有使用 `composer` 来构建的程序。此命令会把 `PSR-0` 和 `PSR-4`转换为一个类映射表，来提高类的加载速度。

    <pre class="hljs undefined">`composer dumpautoload -o
    `</pre>
    > 注意：`php artisan optimize --force` 命令里已经做了这个操作。

    #### 5. 使用 Memcached 来存储会话

    每一个 Laravel 的请求，都会产生会话，修改会话的存储方式能有效提高程序效率。会话的配置文件是 `config/session.php`。生产环境中的 **必须** 使用 Memcached 或者 Redis 等专业的缓存软件来存储会话，**应该** 优先选择 Memcached（注：为了服务器方便管理，也可以用redis）：

    <pre class="hljs php">`<span class="hljs-string">'driver'</span> =&gt; <span class="hljs-string">'memcached'</span>,
    `</pre>

    #### 6. 使用专业缓存驱动器

    「缓存」是提高应用程序运行效率的法宝之一，Laravel 默认缓存驱动是 `file` 文件缓存，生产环境中的 **必须** 使用专业的缓存系统，如 Redis 或者 Memcached。**应该** 优先考虑 Redis。**应该** 避免使用数据库缓存。

    <pre class="hljs php">`<span class="hljs-string">'default'</span> =&gt; <span class="hljs-string">'redis'</span>,
    `</pre>

    #### 7. 数据库请求优化

    关联模型数据读取时 **必须** 使用 [延迟预加载](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.5%2Feloquent-relationships%23%25E5%25BB%25B6%25E8%25BF%259F%25E9%25A2%2584%25E5%258A%25A0%25E8%25BD%25BD) 和 [预加载](https://link.jianshu.com?t=http%3A%2F%2Fd.laravel-china.org%2Fdocs%2F5.5%2Feloquent-relationships%23%25E9%25A2%2584%25E5%258A%25A0%25E8%25BD%25BD) 。

    临近上线时 **必须** 使用 [Laravel Debugbar](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fbarryvdh%2Flaravel-debugbar) 或者 [Clockwork](https://link.jianshu.com?t=https%3A%2F%2Flaravel-china.org%2Ftopics%2F23) 留意每一个页面的总 SQL 请求条数，进行数据库请求调优。

    #### 8. 为数据集书写缓存逻辑

    **应该** 合理的使用 Laravel 提供的缓存层操作，把从数据库里面拿出来的数据集合进行缓存，减少数据库的压力，运行在内存上的专业缓存软件对数据的读取也远远快于数据库。

    <pre class="hljs php">`$hot_posts = Cache::remember(<span class="hljs-string">'posts.hot_posts'</span>, $minutes = <span class="hljs-number">30</span>, <span class="hljs-function"><span class="hljs-keyword">function</span><span class="hljs-params">()</span>
    </span>{
        <span class="hljs-keyword">return</span> Post::getHotPosts();
    });
    `</pre>

    `remember` 甚至连数据关联模型也都一并缓存了。

    #### 9. 使用即时编译器

    **可以** 使用 OpCache 进行优化。OpCache 都能轻轻松松的让你的应用程序在不用做任何修改的情况下，直接提高 50% 或者更高的性能，PHPhub 之前做个一个实验，具体请见：[使用 OpCache 提升 PHP 5.5+ 程序性能](https://link.jianshu.com?t=https%3A%2F%2Flaravel-china.org%2Ftopics%2F301)。

    ## 二十四. 项目文档编写规范

    #### 1. 说明

    每一个项目都 **必须** 包含一个 `readme.md` 文件，`readme` 里书写这个项目的简单信息。作用主要有两个，一个是团队新成员可从此文件中快速获悉项目大致情况，另一个是部署项目时可以作为参考。

    #### 2. 排版规范

    文档页面排版 **必须** 遵循 [中文文案排版指北](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fsparanoid%2Fchinese-copywriting-guidelines) ，在此基础上：

*   中文文档请使用全角标点符号；
*   **必须** 遵循 Markdown 语法，勿让代码显示错乱；
*   原文中的双引号（" "）请代换成中文的引号（『』符号怎么打出来见 [这里](https://link.jianshu.com?t=http%3A%2F%2Fzhihu.com%2Fquestion%2F19755746%2Fanswer%2F27233392)）。
*   所有的 「`加亮`」、「**加粗**」和「[链接]()」都需要在左右保持一个空格。

    #### 3. 行文规范

    `readme.md` 文档 **应该** 包含以下内容：

*   「项目概述」- 介绍说明项目的一些情况，类似于简单的产品说明，简单的功能描述，项目相关链接等，500 字以内；
*   「运行环境」- 运行环境说明，系统要求等信息；
*   「开发环境部署/安装」- 一步一步引导说明，保证项目新成员能最快速的，没有歧义的部署好开发环境；
*   「服务器架构说明」- 最好能有服务器架构图，从用户浏览器请求开始，包括后端缓存服务使用等都描述清楚（主要体现为软件的使用），配合「运行环境」区块内容，可作为线上环境部署的依据；
*   「代码上线」- 介绍代码上线流程，需要执行哪些步骤；
*   「扩展包说明」- 表格列出所有使用的扩展包，还有在哪些业务逻辑或者用例中使用了此扩展包；
*   「自定义 Artisan 命令列表」- 以表格形式罗列出所有自定义的命令，说明用途，指出调用场景；
*   「队列列表」- 以表格形式罗列出项目所有队列接口，说明用途，指出调用场景。

    范例见 [附录：readme-example.md](https://link.jianshu.com?t=https%3A%2F%2Ffsdhub.com%2Fbooks%2Flaravel-specification%2F523%2Freadme-examplemd)

    ## 一些额外补充

    #### 1. 一个方法做一件事情

    一个方法做一件事情，尽量为每一块逻辑用一个方法写起来，不要把全部逻辑放在同一个方法，不然很难维护，别人阅读起来也很吃力。

*   错误的例子：
    `PostsController.php`
    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span> 
    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Controllers</span>;

    <span class="hljs-keyword">use</span> <span class="hljs-title">App</span>\<span class="hljs-title">Http</span>\<span class="hljs-title">Controllers</span>\<span class="hljs-title">Controller</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">PostsController</span> <span class="hljs-title">extend</span> <span class="hljs-title">Controller</span> </span>{

        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">doSomething</span> <span class="hljs-params">(Request $request)</span> </span>{
            <span class="hljs-comment">//检查数据</span>
            <span class="hljs-comment">//...</span>

            <span class="hljs-comment">//查询文章是否存在</span>
            $article = Article::find($request-&gt;input(<span class="hljs-string">'id'</span>));
            <span class="hljs-keyword">if</span> (!$article) {
                <span class="hljs-comment">//...</span>
            }

            <span class="hljs-comment">//添加文章记录</span>
            <span class="hljs-comment">//...</span>

            <span class="hljs-comment">//添加用户文章发布日志记录</span>
            <span class="hljs-comment">//...</span>
        }    

    }
    `</pre>

*   正确的例子：
    `PostsController.php`
    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span> 
    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Controllers</span>;

    <span class="hljs-keyword">use</span> <span class="hljs-title">App</span>\<span class="hljs-title">Http</span>\<span class="hljs-title">Controllers</span>\<span class="hljs-title">Controller</span>;
    <span class="hljs-keyword">use</span> <span class="hljs-title">App</span>\<span class="hljs-title">Services</span>\<span class="hljs-title">PostsService</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">PostsController</span> <span class="hljs-title">extend</span> <span class="hljs-title">Controller</span> </span>{

        <span class="hljs-keyword">private</span> $postsService;

        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">__construct</span> <span class="hljs-params">(PostsService $posts)</span> </span>{
            <span class="hljs-keyword">$this</span>-&gt;postsService = $posts;
        }

        <span class="hljs-comment">/**
         * 添加文章
         */</span>
        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">doSomething</span> <span class="hljs-params">(Request $request)</span> </span>{
            <span class="hljs-keyword">try</span> {
                <span class="hljs-keyword">$this</span>-&gt;postsService-&gt;addArticle($request-&gt;input(<span class="hljs-string">'user_id'</span>), $request-&gt;input(<span class="hljs-string">'data'</span>));

                <span class="hljs-keyword">return</span> response()-&gt;json([...]);
            } <span class="hljs-keyword">catch</span> (\<span class="hljs-keyword">Exception</span> $e) {
                <span class="hljs-comment">//捕捉抛出的异常处理</span>
                <span class="hljs-comment">//...</span>
            }
        }

    }
    `</pre>

    `PostsService.php`

    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span>
    <span class="hljs-keyword">namespace</span> <span class="hljs-title">App</span>\<span class="hljs-title">Services</span>;

    <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">PostsService</span> </span>{

        <span class="hljs-comment">/**
         * 添加文章
         */</span>
        <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">addArticle</span> <span class="hljs-params">($userId, $data)</span> </span>{
            <span class="hljs-comment">//检查数据</span>
            $check = <span class="hljs-keyword">$this</span>-&gt;checkForAddArticle($userId, $data);

            <span class="hljs-comment">//添加文章记录</span>
            $article = <span class="hljs-keyword">$this</span>-&gt;doAddArticle($userId, $data);

            <span class="hljs-comment">//添加用户文章发布日志记录</span>
            $log = <span class="hljs-keyword">$this</span>-&gt;addUserPublishLog($userId, $article);

            <span class="hljs-comment">//...</span>
        }

        <span class="hljs-comment">/**
         * 检查数据
         */</span>
        <span class="hljs-keyword">private</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">checkForAddArticle</span> <span class="hljs-params">($userId, $data)</span> </span>{
            <span class="hljs-comment">//检查失败，抛出异常，由控制器catch</span>
            <span class="hljs-comment">//...</span>
        }

        <span class="hljs-comment">/**
         * 添加文章记录
         */</span>
        <span class="hljs-keyword">private</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">doAddArticle</span><span class="hljs-params">($userId, $data)</span> </span>{
            <span class="hljs-comment">//...</span>
        }

        <span class="hljs-comment">/**
         * 添加用户文章发布日志记录
         */</span>
        <span class="hljs-keyword">private</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">addUserPublishLog</span><span class="hljs-params">($userId, $article)</span> </span>{
            <span class="hljs-comment">//...</span>
        }

    }
    `</pre>

    #### 2. 尽量要多做注释说明

    尽量多做注释，这样别人也能看得懂，自己需要维护后者修复bug的时候，也比较好找

    #### 3. 将常用数值写入新建的配置文件中

    一般数据库会有一些状态位，如 `orders` 表有 `order_status` 字段用来记录订单状态

    <pre class="hljs php">``order_status` tinyint(<span class="hljs-number">2</span>) NOT <span class="hljs-keyword">NULL</span> <span class="hljs-keyword">DEFAULT</span> <span class="hljs-string">'0'</span> COMMENT <span class="hljs-string">'订单状态： 0未付款 1已支付  2待配送 3派送中 4座位使用中 5已完成 6已取消 7超时未付款 8待退款 9已退款'</span>
    `</pre>

    那么可以将这些值写入一个新建的配置文件，如 `config/params.php` 中

    <pre class="hljs php">`<span class="hljs-meta">&lt;?php</span>

    <span class="hljs-keyword">return</span> [
        <span class="hljs-string">'order_status'</span> =&gt; [
            <span class="hljs-number">1</span> =&gt; <span class="hljs-string">'unpaid'</span>,
            <span class="hljs-number">2</span> =&gt; <span class="hljs-string">'paid'</span>,
            <span class="hljs-number">3</span> =&gt; <span class="hljs-string">'wait_for_delivery'</span>,
            ...
            <span class="hljs-number">9</span> =&gt; <span class="hljs-string">'refunded'</span>
        ],

        <span class="hljs-comment">//other config</span>
    ];
    `</pre>

    然后通过用 `config()` 函数读取

    #### 4. 一个请求一个方法

    不要用一个方法执行两种类型的请求，get和post分别用不同的方法，不要通过如下去写

    <pre class="hljs php">`<span class="hljs-keyword">if</span> (!<span class="hljs-keyword">empty</span>($_POST)) {}
    `</pre>

    #### 5. 数据操作

    不要直接执行原生sql，使用laravel的creat、insert、update、save等方法，防止sql注入并且可以过滤掉非法数据，最好使用Laravel的Eloquent ORM。不要使用以下写法：

    <pre class="hljs cpp">`\DB::select(<span class="hljs-string">"SELECT * FROM users WHERE id = 1"</span>);

          </div>
        </div>