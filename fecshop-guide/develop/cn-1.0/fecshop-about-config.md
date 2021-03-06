Fecshop 初始配置
================

> 当您手动安装好Linux 和FecShop的代码后，就可以进行配置了。


1、配置 fecshop app advanced

```
在common/main-local.php中配置mysql，mongodb，redis

```


```
<?php
return [
    'components' => [
        'db' => [
            'class' => 'yii\db\Connection',
            'dsn' => 'mysql:host=localhost;dbname=fecshop',  # Fecshop是mysql的数据库名字，您需要到mysql中建立一个数据库
            'username' => 'root',  # mysql的账户
            'password' => '123456', # mysql的密码
            'charset' => 'utf8',
        ],
        'mongodb' => [
            'class' => 'yii\mongodb\Connection',
            # 有账户的配置，username是用户名，password是密码，fecshop是数据库
            //'dsn' => 'mongodb://username:password@localhost:27017/fecshop',
            # 无账户的配置，fecshop是数据库
            'dsn' => 'mongodb://127.0.0.1:27017/fecshop',
            # 复制集 如果您的mongodb是复制集（大站），可以使用下面的复制集的配置方式。
            //'dsn' => 'mongodb://10.10.10.252:10001/fecshop,mongodb://10.10.10.252:10002/fecshop,mongodb://10.10.10.252:10004/fecshop?replicaSet=terry&readPreference=primaryPreferred',
        ],


```

mongodb默认是没有密码的，您可以将mongodb的端口在iptables添加信任和端口封闭即可
保证安全，当然，您也可以配置mongodb的用户名和密码。


4、配置环境

4.1 添加host

打开C:\Windows\System32\drivers\etc\hosts，添加如下代码（如果是其他IP，将
127.0.0.1 替换成其他IP即可。）：

```
127.0.0.1       rock.fecshoptest.com
127.0.0.1       my.fecshoptest.com
127.0.0.1       appadmin.fecshoptest.com
127.0.0.1       appfront.fecshoptest.com
127.0.0.1       appfront.fecshoptest.es
127.0.0.1       apphtml5.fecshoptest.com
127.0.0.1       appapi.fecshoptest.com
127.0.0.1       appserver.fecshoptest.com
127.0.0.1       img.fecshoptest.com		#appimage/common
127.0.0.1       img2.fecshoptest.com	#appimage/appadmin
127.0.0.1       img3.fecshoptest.com	#appimage/appfront
127.0.0.1       img4.fecshoptest.com	#appimage/apphtml5
127.0.0.1       img5.fecshoptest.com	#appimage/appserver
```


4.2、配置nginx

```
appfront.fecshoptest.com appfront.fecshoptest.es 指向 fecshop/appfront/web 
 
appadmin.fecshoptest.com 指向fecshop/appadmin/web

apphtml5.fecshoptest.com 指向fecshop/apphtml5/web

appapi.fecshoptest.com 	 指向fecshop/appapi/web

appserver.fecshoptest.com 指向fecshop/appserver/web

img.fecshoptest.com 	指向fecshop/appimage/common

img2.fecshoptest.com 	指向fecshop/appimage/appadmin

img3.fecshoptest.com 	指向fecshop/appimage/appfront

img4.fecshoptest.com 	指向fecshop/appimage/apphtml5

img5.fecshoptest.com 	指向fecshop/appimage/appserver

```

5、配置语言（可以先使用默认）：


在配置文件（：`@common\config\fecshop_local_services\FecshopLang.php`



6、配置货币（可以先使用默认）：


在文件：`@common\config\fecshop_local_services\Page.php`



7、配置store的域名和图片的域名，您可以和我下面的示例代码一致，


store在配置文件：`@app\config\fecshop_local_services\Store.php`

譬如我的代码(您可以和我的保持一致，相应域名已经在上面添加host)：

```
<?php
   return [
   'store' => [
		'class' => 'fecshop\services\Store',
		'stores' => [
			# 数据的key就是域名
			'appfront.fecshoptest.com' => [
				'language' 		=> 'en_US',   # 语言必须在上面第五步的fecshoplang中定义，否则将无法得到语言属性。
				'languageName' 	=> 'English', # 在添加store的时候，必须查看 添加的语言在 fecshoplang中是否定义。
				# 定义本地模板路径，用来重写fecshop的模板，或者开发新的模板文件。
				//'localThemeDir'	=> '@appfront/theme/terry/theme01',
				# 定义第三方模板路径，用来重写fecshop的模板，或者开发新的模板文件。
				'thirdThemeDir'	=> [],
				# 当前语言的默认货币，货币必须在上面第六步的配置中存在
				'currency' 		=> 'USD',
				'mobile'		=> [ # 当设备满足什么条件的时候，进行跳转。
					'enable'		=> true,
					'condition'		=> ['phone','tablet'], # phone 代表手机，tablet代表平板
					'redirectUrl' 	=> 'apphtml5.fecshoptest.com',	# 如果是移动设备访问进行域名跳转
				],
				# 第三方账号登录配置
				'thirdLogin' => [
					'facebook' =>[                       #fb api配置 ，fb可以一个app设置pc和手机两个域名 
						'facebook_app_id'     => '184963',
						'facebook_app_secret' => '2e097dd7139',
					],
					"google" => [                       #谷歌api visit https://code.google.com/apis/console to generate your google api
						'CLIENT_ID'  	 => '38037grhccontent.com',
						'CLIENT_SECRET'  => 'ei8RaoCHYm0rrwO',
					],
				]

				//'image'	=> [
				//	'domain' => 'img.appfront.fancyecommerce.com',
				//	'baseDir'=> '@appimage/appfront',
				//]
			],
			'appfront.fecshoptest.com/fr' => [
				'language' 		=> 'fr_FR',
				'languageName' 	=> 'Fran?ais',
				'localThemeDir'	=> '@appfront/theme/terry/theme01',
				'thirdThemeDir'	=> [],
				'currency' 		=> 'RMB',
				'mobile'		=> [
					'enable'			=> true,
					'condition'			=> ['phone'], # phone 代表手机，tablet代表平板。
					'redirectDomain' 	=> 'apphtml5.fecshoptest.com/fr', # 跳转后的url。
				],
			],
			'appfront.fecshoptest.es' => [
				'language' 		=> 'es_ES',
				'languageName' 	=> 'Espa?ol',
				'localThemeDir'	=> '@appfront/theme/terry/theme01',
				'thirdThemeDir'	=> [],
				'currency' 		=> 'USD',
				'mobile'		=> [
					'enable'		=> true,
					'condition'		=> ['tablet'],
					'redirectDomain' 	=> 'fecshop.apphtml5.es.fancyecommerce.com',	
				],
			],
			'appfront.fecshoptest.com/cn' => [
				'language' 		=> 'zh_CN',
				'languageName' 	=> '中文',
				'localThemeDir'	=> '@appfront/theme/terry/theme01',
				'thirdThemeDir'	=> [],
				'currency' 		=> 'RMB',
				'mobile'		=> [
					'enable'		=> false,
					'condition'		=> ['phone','tablet'],
					'redirectDomain' 	=> 'fecshop.apphtml5.fancyecommerce.com/cn',	
				],
			],
		],
		
	],
			
];
```

各个代码的具体含义，在注释中已经说明，关于第三方facebook和google登录，如何获取
CLIENT_ID，CLIENT_SECRET可以参看我的博文：
[ facebook login 申请 app_id 和 app_secret](http://blog.csdn.net/terry_water/article/details/55095721) ，
[ google login api 申请 CLIENT_SECRET 和 CLIENT_SECRET](http://blog.csdn.net/terry_water/article/details/55095209)

如果您的各个语言使用了子域名（譬如cn.fecshop.com , fr.fecshop.com 等），由于子域名之间session默认是不能共享的，为了更好的共享session，让语言切换后，
购物车和登录信息存在，您需要在入口文件index.php里面设置`session.cookie_domain`
，大约在index.php第3行找到代码

```
#ini_set('session.cookie_domain', '.fancyecommerce.com'); //初始化域名，
```

将前面的注释去掉，将`.fancyecommerce.com`替换成您的域名,
如果您使用的是 `www.fecshop.com/cn` , `www.fecshop.com/fr`,
这种方式，则不需要设置 `session.cookie_domain`,
如果您想使用后缀模式区分多语言，譬如添加it语言，您需要到@app/web中添加一个文件夹it，
然后在文件夹里面新建@app/web/it/index.php文件，并新建文件夹@app/web/it/assets，并设置可写,
目前fecshop只添加了cn和fr两个例子，你可以参考这两个。

7、图片域名配置文件：`@common\config\fecshop_local_services\Image.php`
,譬如我的代码(您可以和我的保持一致，相应域名已经在上面添加host)：

```
<?php
/**
 * FecShop file.
 * @link http://www.fecshop.com/
 * @copyright Copyright (c) 2016 FecShop Software LLC
 * @license http://www.fecshop.com/license/
 */
return [
	'image' => [
		'appbase'	=> [
			'appfront' => [
				'basedir' => '@appimage/appfront',
				'basedomain' => 'http://img3.fecshoptest.com',
			],
			'apphtml5' => [
				'basedir' => '@appimage/apphtml5',
				'basedomain' => 'http://img2.fecshoptest.com',
			],
			'appadmin' => [
				'basedir' => '@appimage/appadmin',
				'basedomain' => 'http://img2.fecshoptest.com',
			],
			'common' => [
				'basedir' => '@appimage/common',
				'basedomain' => 'http://img.fecshoptest.com',
			],
		],
	],
];
```

您可能会问，为什么要给图片配置域名，图片和网站使用一个域名不就可以吗？
原因：浏览器加载页面的时候，每一个域名加载的链接个数是有限制的，把
图片使用不同的域名，可以让图片独立加载，加快页面的加载。



8、配置是否强制复制assets到web目录，如果是开发环境，按照下面进行配置（可选配置，可以先不管这个）。

`@app/config/main.php`里面可以看到下面的配置


这个是yii2的知识范畴

```
'assetManager' => [
	'forceCopy' => true,
],
```

如果是线上， 将forceCopy设置成false `['forceCopy' => false]`

原因：本地开发经常修改css，因此，每次就需要yii的asset将css发布到web路径下面（这里的发布，就是yii2在初始化的时候就会强制把需要的css复制到
web/assets路径下面，譬如yii2封装的bootstrap前端框架），
如果是线上环境，这样非常耗费资源，因此，线上关闭即可，如果您发布了新的css文件，
那么您需要手动清空@app/web/assets下面的文件夹（如果线上访问量不大，开启也无所谓，呵呵。）。

9、导入数据库表(migrate)，在fecshop根目录执行下面的命令行

9.1、Yii2 migratge方式导入表。

mysql(导入mysql的表，数据，索引):

```
./yii migrate --interactive=0 --migrationPath=@fecshop/migrations/mysqldb
```

mongodb(导入mongodb的表，数据，索引):

```
./yii mongodb-migrate  --interactive=0 --migrationPath=@fecshop/migrations/mongodb
```

9.2、测试数据安装：

mongodb的示例数据存放路径为：

`./vendor/fancyecommerce/fecshop/migrations/mongodb-example-data/example_data.js`

可以通过mongodb的后台，或者通过php的rockmongo安装这些mongodb中的示例数据。

mongodb的示例数据产品图片比较大，没有放到版本库里面，你可以到百度云盘下载`appimage.zip`，下载地址为：`https://pan.baidu.com/s/1kVwRD2Z`
如果覆盖图片后，将appimage覆盖到根目录即可，覆盖后，如果发现产品图片没有出来，那么您需要清空 `appimage/common/media/catalog/product/cache/*`下面所有文件和文件夹，
清空浏览器图片缓存，重新刷新页面即可。

10、开启nginx  mysql  mongodb  php，你就可以访问本地配置的fecshop了。

11、如果是线上，需要开启一些脚本。

详细参看：[Fecshop 脚本介绍](http://www.fecshop.com/doc/fecshop-guide/instructions/cn-1.0/guide-fecshop_cron_script.html)


