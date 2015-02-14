### RSS Factory

***

demo:<https://diy-devz.rhcloud.com>

RSS Factory 是用于生成 微博 微信公众号 知乎日报 RSS 的Web APP。  

部署在Openshift diy tornado环境上，环境搭建参考:  
<http://blog.ricoxie.com/2014/04/29/deploy-tornado-on-openshift-diy/>  

<del>另外还依赖BeautifulSoup</del>

模版引擎改为jinja2, html解析改为lxml

新增memcahed缓存，加速访问微信，知乎 rss  
Openshift安装memcached参考  
<http://www.blackglory.me/openshift-install-wordpress-memcached/>

启动memcached  
$OPENSHIFT_DATA_DIR/bin/memcached -l $OPENSHIFT_DIY_IP -p 15211 -d  
获取pid用于停止服务  
ps -ef|grep memcached  

还需要安装  
$OPENSHIFT_DATA_DIR/bin/pip install python-memcached

写这个APP是为了学习tornado异步请求，tornado没有用多线程实现并发，而是用事件循环来处理，所以这里主要用到了异步http client，同时fetch多个url也不会太慢

2015.02.05 搜狗微信公众号已启用反爬虫，我在代码中已添加针对反爬虫的措施，但是不保证一定有效，间歇性的500是不可避免的

2015.02.14 更新了定时任务，每6个小时更新一次cookie，获取10个有效cookie，微信公众�号api随机使用可用cookie，基本解决搜狗反爬虫问题
