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







