# studyplan

4.18之前：主要学习了数据库优化(架构层次=>读写分离，负载均衡等，从sql语句层次,从数据库表的设计遵从范式，消除部分依赖，消除传递依赖，从分区分表层次=>垂直分表，横向分表key,hash，list,range前两项是求余算法，后两项是表达式算法,索引的应用,何时应该使用，不应该使用的地方,mysql慢查询,explain执行计划的使用,binlog三种规范格式等等),Memcache,Redis等Nosql学习与项目应用,了解AngularJs并没有应用到项目开发.
复习了javascript,jquery,php的基础,Linux基本命令复习,重新配置Linux下编程环境,开始在Linux下开发后续项目.
总结实习生面试不足之处.

未完成的事情：项目代码没有复习,没有进行算法训练,没有进行操作系统，网络，css3,html5,js面试习题训练。




4.18：  学习了负载均衡配置，nginx与upstream结合，nginx与keepalive结合，LVS配置与keepalive 结合，  虚拟服务器(Linux)与Master与Backup与从1主服务器和从2主服务器.
php实现mysql读写分离.理解反向代理.


未完成的事情：实验室论文没有进展,method还没开始写,荒废的c和c++还没有复习,论文代码没有进展.

4.19：  重读<<PHP高级程序设计_模式,框架与测试>> 加深理解 抽象类,接口的使用,异常的使用,复习了单例模式以及工厂模式,以及在php6中出现的static新的特性,反射类在众多接口中的使用,利用反射去查找插件等.
例如:利用RefectionClass的implementsInterface方法去检查哪些类去继承了某些插件,然后将他们装在数组里返回,然后利用迭代这个数组,然后利用每个反射得到的类(这些类都有我们想要的接口功能),利用他们的getMethod方法去检查,实现了这些接口中的有没有我们想要的方法,将有我们想要功能方法的类去检查他是否是一个静态函数,如果是静态就invoke(null)返回,如果不是就利用newInstance()去实现对象,然后invoke(stdClass),最后利用array_merge()方法将他们组合成一个数组.
PHPUnit,完成单元测试, reqiere_once('PHPUnit/Framework.php');
DemoTest extends PHPUnit_Framework_TestCase{
   
   $demo =  new Demo();
   $this->assertEquals(XX,$demo->XXXXX);//断言的使用
   $this->assertNotEquals(XX,$demo->XXXXX);

}
setUp()和tearDown()方法在每个测试执行的时候都会被调用,而且会在测试类的初始实例上被调用.所以设置代码不会在测试实行之间持久保存，而且不会泄露每个测试之间的状态.

一般调试php大多使用echo,var_dump等输出(exit,die)配合使用,或者用框架中的错误日志等等...利用Xdebug除了调试功能 还有助于确定那部分的代码运行很缓慢
SPL的使用:key,current,rewind,next,valid接口中的方法重新实现，观察者模式的使用
序列化的问题，非SPL的序列化魔术方法__sleep和__wakeup有一些问题,基类的私有成员访问受到了限制,通过Serializable消除这个限制.
spl_autoload_register();
对象标识符spl_object_hash();
对同一对象的引用 
class a{}
$instance = new a();
$reference = $instance;
他们的对象标识符是相同的.

实现递归目录，可以控制循环层次

<?php
 $path =".";
  $list1 = getdirlist($path,3);
  var_dump($list1);
  
  function getdirlist($path,$num){
          static $list = array();
          static $count=0;
          if(!is_dir($path)){
                 $list[]=$path;
         }else{          
                  $count++;
                  if($count<$num){
                          $handle = opendir($path);
                          while($name = readdir($handle)){
                                  if(!isset($list[$path])){
                                          $list[$path]='';
                                  }       
                                  if($name !='.' && $name !='..'){
                                           
                                         if(is_dir($path."/".$name)){
                                                 $list[$path][] = $name;
                                                  getdirlist($path."/".$name,$num);
                                       }else{  
                                                  $list[$path][] = $name;
                                          }       
                                 }       
                          }       
                }       
          }       

 return $list;
 }
?>




//操作复制目录
$path = '/usr/local/nginx/html';
$newpath = '/usr/local/nginx/html/d';
$result = mycopydir($path,$newpath);
var_dump($result);
function  mycopydir($source,$dest){
       $filelist =array();
        if(!is_dir($dest)){
                exit("目录不存在");
        }
      $filelist = getdirinfo($source);
        if(!$filelist){
                exit("source目录异常,请检查!");
        }
        $transfersource = trim($source,"/");
        $transfersource = explode('/',$transfersource);
        $i = count($transfersource) - 1;
        $currentdir = $transfersource[$i];
        foreach ($filelist as $dir => $file) {
                $newdir = str_replace($source,'' , $dir);
                $copydir = $dest."/".$currentdir.'/'.$newdir;
                if(!is_dir($copydir)){
                        if(!mymkdir($copydir)){
                                exit("无法创建目录$copydir,请检查权限");
                        }
                }
                foreach ($file as  $filename) {
                        if(!copy($dir."/".$filename,$copydir."/".$filename)){
                        exit("无法拷贝文件$dir/$filename,请检查权限");
                        }
                }
        }
 return true;
}

function mymkdir($path){
        $currentdir = '';
        $newpath = explode('/',$path);
        for ($i=0,$count=count($newpath); $i < $count; $i++) {
                $currentdir .= $newpath[$i].'/';
                if(!is_dir($currentdir)){
                        if(!mkdir($currentdir)){
                                return false;
                        }
                }
        }
        return true;
}


function getdirinfo($path){
        static $filelist=array();
        //echo $path;exit;
        if(is_dir($path)){
                $handle =  opendir($path);
                while($filename = readdir($handle)){
                        if($filename !="." && $filename !=".." && is_dir($path.'/'.$filename)){
                                getdirinfo($path.'/'.$filename);
                        }elseif(is_file($path.'/'.$filename)){
                                $filelist[$path][] = $filename;
                        }
                }
        }
      return $filelist;
}

文件锁都包含哪些?
php文件锁有两种：共享锁和排他锁，也就是读锁(LOCK_SH)和写锁(LOCK_EX) ,读的时候：
如果不想出现dirty数据，那么最好使用lock_sh共享锁。可以考虑以下三种情况：
1. 如果读的时候没有加共享锁，那么其他程序要写的话（不管这个写是加锁还是不加锁）都会立即写
成功。如果正好读了一半，然后被其他程序给写了，那么读的后一半就有可能跟前一半对不上（前一
半是修改前的，后一半是修改后的）
2. 如果读的时候加上了共享锁（因为只是读，没有必要使用排他锁），这个时候，其他程序开始写，
这个写程序没有使用锁，那么写程序会直接修改这个文件，也会导致前面一样的问题
3. 最理想的情况是，读的时候加锁(lock_sh),写的时候也进行加锁(lock_ex),这样写程序会等着读程序
完成之后才进行操作，而不会出现贸然操作的情况
写的时候：
如果多个写程序不加锁同时对文件进行操作，那么最后的数据有可能一部分是a程序写的，一部分是b
程序写的 如果写的时候加锁了，这个时候有其他的程序来读，那么他会读到什么东西呢？
1. 如果读程序没有申请共享锁，那么他会读到dirty的数据。比如写程序要写a,b,c三部分，写完a,这时
候读读到的是a，继续写b，这时候读读到的是ab，然后写c，这时候读到的是abc.
2. 如果读程序在之前申请了共享锁，那么读程序会等写程序将abc写完并释放锁之后才进行读。


在什么情况下需要用文件锁?
在会有并发写入的时候,或者读写同时操作的时候需要使用文件锁
如何使用文件锁?
flock — 轻便的咨询文件锁定
bool flock ( int $handle , int $operation [, int &$wouldblock ] )
flock() 操作的 handle 必须是一个已经打开的文件指针。 operation 可以是以下值之一：
1.要取得共享锁定（读取的程序），将 operation 设为 LOCK_SH（ PHP 4.0.1 以前的版本设置为
1）.
2.要取得独占锁定（写入的程序），将 operation 设为 LOCK_EX（ PHP 4.0.1 以前的版本中设置为
2）.
3.要释放锁定（无论共享或独占），将 operation 设为 LOCK_UN（ PHP 4.0.1 以前的版本中设置为
3）.
4.如果不希望 flock() 在锁定时堵塞，则给 operation 加上 LOCK_NB（ PHP 4.0.1 以前的版本中设置
为 4）. 




4.20  应付一家创业公司的需求学习了mongodb,11点才起来,学了2个多小时,在linux安装,建库,增删改查,基本操作,这时候终于都用上了Nosql三剑客...好了出发江安助教,晚上回来继续深入查询,游标操作,索引,备份与恢复,分片的使用^_^

show dbs;
show collections;
db.createCollection();
db.XXX.insert();
db.XXX.update()
db.XXX.drop();
db.XXX.remove();
{$set:{xx:xx}}
{$rename:{xx:xx}}
{$unset:{xx:xx}}
{$inc:{xx:xx}}
在update中第三个参数可以设置成{upsert:true/false}为true时等价于mysql的replace,{multi:true}代表匹配多行
db.xxx.find({gender:"m"},{_id:1,name:1})
http://www.cnblogs.com/zdz8207/p/3765579.html 
php 5.4中php-fpm 的重启、终止操作命令
重启nginx
/usr/local/nginx/sbin/nginx -s reload
mongoDB及其php扩展安装
http://blog.csdn.net/qmhball/article/details/8963763


4.21 下午又因为助教浪费了时间，由于阿里云到期了(低配的用起来就是XXX)，换了新浪sae重新弄了下配置,微信公共号换了下地址，重新弄了代码，匆匆赶回望江，继续mongodb, 看了游标操作,让我感觉到了这个数据库的强大之处..继续学习..实习+论文一再搁浅,我的内心是崩溃加焦虑的..

>for(var mycursor=db.bar.find({_id:{$lte:10}});mycursor.next();){ printjson(mycursor.next()) }

> var mycursor = db.bar.find({_id:{$lte:5}})
> mycursor.forEach(function(obj){printjson(obj)})
//分页的时候用
var mycursor = db.bar.find().skip(50);
mycursor.forEach(function(obj){
printjson(obj)
})
//不要随意使用toArray()
//会把所有的行立即以对象的方式组织在内存
//可以在取出少数几行的时候用这个功能
var mycursor = db.bar.find().skip(850).limit(10)
mycursor.toArray();

var mycursor = db.bar.find().skip(850).limit(10)
mycursor.forEach(function(obj){
printjson(obj.content)
})

//使用explain()类似mysql的执行计划(explain sql)
db.stu.find({sn:99}).explain();
//查询结果
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "test.stu",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"sn" : {
				"$eq" : 99
			}
		},
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"sn" : {
					"$eq" : 99
				}
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"serverInfo" : {
		"host" : "localhost.localdomain",
		"port" : 27017,
		"version" : "3.2.5",
		"gitVersion" : "34e65e5383f7ea1726332cb175b73077ec4a1b02"
	},
	"ok" : 1
}


//设置索引
 db.stu.ensureIndex({sn:1})

//再次查询

db.stu.find({sn:99}).explain();
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "test.stu",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"sn" : {
				"$eq" : 99
			}
		},
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"sn" : 1
				},
				"indexName" : "sn_1",
				"isMultiKey" : false,
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 1,
				"direction" : "forward",
				"indexBounds" : {
					"sn" : [
						"[99.0, 99.0]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"serverInfo" : {
		"host" : "localhost.localdomain",
		"port" : 27017,
		"version" : "3.2.5",
		"gitVersion" : "34e65e5383f7ea1726332cb175b73077ec4a1b02"
	},
	"ok" : 1
}
//查看索引
//要注意索引是建立在牺牲写入和更改等操作的基础上的
db.stu.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "test.stu"
	},
	{
		"v" : 1,
		"key" : {
			"sn" : 1
		},
		"name" : "sn_1",
		"ns" : "test.stu"
	}
]

//删除索引
>db.stu.dropIndex({name:-1})

//设置多列索引

>db.shop.ensureIndex({'spc.area':1})
//设置唯一索引
> db.shop.ensureIndex({'spc.area':1},{unique:true}}

//稀疏索引
> db.shop.ensureIndex({email:1},{sparse:true})
//会把数据中没有email列的忽略,而不是像普通索引将没有email列的当做null查询出来


//哈希索引对于顺序和范围查询都不如B-tree好，适用于数据散列分布的
db.XX.ensureIndex({XX:'hashed'})
//索引重建
db.xx.reIndexes();


//用户权限认证,注意mongo是角色跟随库的

use admin
switched to db admin
> db.auth('dba','123456')
1
> show dbs
admin  0.000GB
local  0.000GB
test   0.000GB
> use test
switched to db test
> db.createUser(
... {
... user:'test',
... pwd:'123456',
... roles:[
... {role:'readWrite',db:'test'}
... ]
... }
... )
Successfully added user: {
	"user" : "test",
	"roles" : [
		{
			"role" : "readWrite",
			"db" : "test"
		}
	]
}
> db.auth('test','123456')
1
> show dbs
admin  0.000GB
local  0.000GB
test   0.000GB
> show collections;
bar
shop
stu
user
> db.shop.find();
{ "_id" : ObjectId("5718b1b3efff6b9f31611b34"), "name" : "Nokia", "spc" : { "weight" : 120, "area" : "taiwan" } }
{ "_id" : ObjectId("5718b1c3efff6b9f31611b35"), "name" : "sanxing", "spc" : { "weight" : 220, "area" : "hanguo" } }
{ "_id" : ObjectId("5718b643efff6b9f31611b36"), "name" : "Nokia", "spc" : { "weight" : 120, "area" : "taiwan" } }
{ "_id" : ObjectId("5718b645efff6b9f31611b37"), "name" : "Nokia", "spc" : { "weight" : 120, "area" : "taiwan" } }
> 

 pkill -9 mongo
 ./bin/mongod --dbpath /home/m17 --logpath /home/mlog/m17.log --fork --auth 权限登录
//导出csv格式的数据 
 ./bin/mongoexport -d test -c stu -f sn,name -q '{sn:{$lte:10}}' --csv  -o ./test.stu
 //导入csv格式数据
 ./bin/mongoimport -d test -c brid4 --type csv --headerline --file ./test.stu //注意headerline！！！

 //导出json格式数据
  ./bin/mongoexport -d test -c stu -f sn,name -q '{sn:{$lte:10}}'  -o ./test.stu.json
//导入json数据
 [root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongoimport -d test -c animal --type json --file ./test.stu.json

//导出二进制文件
./bin/mongodump -d test //整个库,  -c tea 某个collection
//恢复二进制文件
./bin/mongorestore -d test --dir dump/test/


复习了session类重写 session_set_save_handle("sessRead","sessWrite","sessGC","sessDelete","sessBegin","sessEnd")
session会话技术,能否持久化(如果要持久需要配置2个地方,一个是cookie时间,一个是垃圾回收时间)
如果禁用了cookie，一般认为是不可以使用session的。但可以通过(url,post等传送)

4.22日   
replication的应用
[root@localhost mongodb-linux-x86_64-3.2.5]# pkill -9 mongo
[root@localhost mongodb-linux-x86_64-3.2.5]# mkdir /home/m17 /home/m18 /home/m19 /home/mlog
[root@localhost mongodb-linux-x86_64-3.2.5]# ls /home/m*
/home/m17:
/home/m18:
/home/m19:
/home/mlog:

[root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongod --dbpath /home/m17 --logpath /home/mlog/m17.log --fork --port 27017 --replSet rs2 --smallfiles
about to fork child process, waiting until server is ready for connections.
forked process: 67248
child process started successfully, parent exiting

[root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongod --dbpath /home/m18 --logpath /home/mlog/m18.log --fork --port 27018 --replSet rs2 --smallfiles
about to fork child process, waiting until server is ready for connections.
forked process: 67306
child process started successfully, parent exiting

[root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongod --dbpath /home/m19 --logpath /home/mlog/m19.log --fork --port 27019 --replSet rs2 --smallfiles
about to fork child process, waiting until server is ready for connections.
forked process: 67332
child process started successfully, parent exiting


> var rsconf = {     _id:'rs2',     members:     [         {_id:0,         host:'192.168.1.211:27017'         },         {_id:1,         host:'192.168.1.211:27018'         },         {_id:2,         host:'192.168.1.211:27019'         }     ] }

> printjson(rsconf)
{
	"_id" : "rs2",
	"members" : [
		{
			"_id" : 0,
			"host" : "192.168.1.211:27017"
		},
		{
			"_id" : 1,
			"host" : "192.168.1.211:27018"
		},
		{
			"_id" : 2,
			"host" : "192.168.1.211:27019"
		}
	]
}
> rs.initiate(rsconf)
{ "ok" : 1 }
rs2:OTHER>rs.status()
{
	"set" : "rs2",
	"date" : ISODate("2016-04-22T05:21:06.604Z"),
	"myState" : 1,
	"term" : NumberLong(1),
	"heartbeatIntervalMillis" : NumberLong(2000),
	"members" : [
		{
			"_id" : 0,
			"name" : "192.168.1.211:27017",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 1825,
			"optime" : {
				"ts" : Timestamp(1461302412, 2),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2016-04-22T05:20:12Z"),
			"infoMessage" : "could not find member to sync from",
			"electionTime" : Timestamp(1461302412, 1),
			"electionDate" : ISODate("2016-04-22T05:20:12Z"),
			"configVersion" : 1,
			"self" : true
		},
		{
			"_id" : 1,
			"name" : "192.168.1.211:27018",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 64,
			"optime" : {
				"ts" : Timestamp(1461302412, 2),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2016-04-22T05:20:12Z"),
			"lastHeartbeat" : ISODate("2016-04-22T05:21:06.364Z"),
			"lastHeartbeatRecv" : ISODate("2016-04-22T05:21:05.783Z"),
			"pingMs" : NumberLong(0),
			"syncingTo" : "192.168.1.211:27017",
			"configVersion" : 1
		},
		{
			"_id" : 2,
			"name" : "192.168.1.211:27019",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 64,
			"optime" : {
				"ts" : Timestamp(1461302412, 2),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2016-04-22T05:20:12Z"),
			"lastHeartbeat" : ISODate("2016-04-22T05:21:06.364Z"),
			"lastHeartbeatRecv" : ISODate("2016-04-22T05:21:05.783Z"),
			"pingMs" : NumberLong(0),
			"syncingTo" : "192.168.1.211:27017",
			"configVersion" : 1
		}
	],
	"ok" : 1
}

//删除
rs2:PRIMARY> rs.remove('192.168.1.211:27018');
//添加不起作用
rs.add('192.168.1.211:27018');
//不能多次初始化一个变量=>例如var rsconf ={};
如果删掉secondary可以使用rs.reconf(变量);重新配置
给primary赋值,secondary也可以查看到 但是要注意not master and slaveOk=false，使用rs.slaveOK()解决
rs2:PRIMARY> show dbs
local  0.000GB
rs2:PRIMARY> use test
switched to db test
rs2:PRIMARY> db.stu.insert({title:'hello'})
WriteResult({ "nInserted" : 1 })
rs2:PRIMARY> show dbs;
local  0.000GB
test   0.000GB
rs2:PRIMARY> show collections;
stu
rs2:PRIMARY> db.stu.find()
{ "_id" : ObjectId("5719b78b5bddae62f1ef164e"), "title" : "hello" }
rs2:PRIMARY> exit
bye
[root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongo --port 27018
MongoDB shell version: 3.2.5
connecting to: 127.0.0.1:27018/test


rs2:SECONDARY> use test
switched to db test
rs2:SECONDARY> db.stu.find()
Error: error: { "ok" : 0, "errmsg" : "not master and slaveOk=false", "code" : 13435 }
rs2:SECONDARY> rs.slaveok()
2016-04-21T22:35:30.497-0700 E QUERY    [thread1] TypeError: rs.slaveok is not a function :
@(shell):1:1

rs2:SECONDARY> rs.slaveOk();
rs2:SECONDARY> db.stu.find()
{ "_id" : ObjectId("5719b78b5bddae62f1ef164e"), "title" : "hello" }
rs2:SECONDARY> 

//replication基本配置完成 剩下的可以rs.help()查看帮助



简单的自动启动脚本
vim start.sh
#!/bin/bash
IP=192.168.1.211
NA=rs2

pkill -9 mongo
rm -rf /home/m*

mkdir -p /home/m17 /home/m18 /home/m19 /home/mlog

/usr/local/mongodb/mongodb-linux-x86_64-3.2.5/bin/mongod --dbpath /home/m17 --logpath /home/mlog/m17.log --port 27017 --fork --smallfiles --replSet ${NA}

/usr/local/mongodb/mongodb-linux-x86_64-3.2.5/bin/mongod --dbpath /home/m18 --logpath /home/mlog/m18.log --port 27018 --fork --smallfiles --replSet ${NA}

/usr/local/mongodb/mongodb-linux-x86_64-3.2.5/bin/mongod --dbpath /home/m19 --logpath /home/mlog/m19.log --port 27019 --fork --smallfiles --replSet ${NA}

/usr/local/mongodb/mongodb-linux-x86_64-3.2.5/bin/mongo <<EOF
use admin
var rsconf = {
_id:'rs2',
members:[
{_id:0,host:'${IP}:27017'},
{_id:1,host:'${IP}:27018'},
{_id:2,host:'${IP}:27019'}
]
}
rs.initiate(rsconf);
EOF

============================================================================================
mongos路由器  => configsvr => shard1 shard2 ....

configsvr不存储真正的数据,存储的是meta信息,即'某条数据在哪个片上'

mongos查询某条数据的时候,先要找configsvr,询问得到该数据在哪个shard上

分片要如下要素:
1:要有N(N>=2)个mongod服务做片节点
2.要有configsvr维护meta信息
3.要设定好数据的分片规则
4.要启动mongos做路由


未完成的事: 复习shell脚本  论文method 实验没跑  复习项目代码
=============================================================================================
[root@localhost mongodb-linux-x86_64-3.2.5]# pkill -9 mongo
[root@localhost mongodb-linux-x86_64-3.2.5]# rm -rf /home/m*
[root@localhost mongodb-linux-x86_64-3.2.5]# ps aux |grep 'mongo'
root      71396  0.0  0.0 112660   960 pts/2    R+   23:50   0:00 grep --color=auto mongo
[root@localhost mongodb-linux-x86_64-3.2.5]# mkdir -p /home/m17 /home/m18 /home/m20 /home/mlog
[root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongod --dbpath /home/m17/ --logpath /home/mlog/m17.log --fork --port 27017 --smallfiles
about to fork child process, waiting until server is ready for connections.
forked process: 71550
child process started successfully, parent exiting
[root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongod --dbpath /home/m18/ --logpath /home/mlog/m18.log --fork --port 27018 --smallfiles
about to fork child process, waiting until server is ready for connections.
forked process: 71587
child process started successfully, parent exiting
[root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongod --dbpath /home/m20/ --logpath /home/mlog/m20.log --fork --port 27020 --configsvr
about to fork child process, waiting until server is ready for connections.
forked process: 71854
child process started successfully, parent exiting
[root@localhost mongodb-linux-x86_64-3.2.5]# ./bin/mongos --logpath /home/mlog/m30.log --port 30000 --configdb 192.168.1.211:27020 --fork
2016-04-21T23:56:33.976-0700 W SHARDING [main] Running a sharded cluster with fewer than 3 config servers should only be done for testing purposes and is not recommended for production.
about to fork child process, waiting until server is ready for connections.
forked process: 71901
child process started successfully, parent exiting
[root@localhost mongodb-linux-x86_64-3.2.5]# 










