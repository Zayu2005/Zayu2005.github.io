## 修改默认密码
1.修改SSH密码
````
$ passwd ctf
Changing password for ctf.
Current password:
passwd: Authentication token manipulation error
passwd: password unchanged
````

2.修改网站后台登录密码，完善网站的登录密码
3.修改数据库连接密码
```mysql
flush privileges #刷新
```
## 部署站点防御
1.查看是否有后门账号
2.关注是否运行了特殊进程
3.使用命令匹配一句话特
4.关闭不必要端口
5.D盾扫描删除预留后门文件
6.流量监控脚本部署
7.WAF脚本部署
```PHP
require_once('waf.php');
```
8.文件监控脚本部署

## 代码审计漏洞挖掘
1.利用seay进行代码审计
2.根据代码特点进行漏洞挖掘
3.根据漏洞进行代码修补

## 利用漏洞进行得分
1.通过出漏洞进行得分
2.利用不死马进行权限维持
3.反弹shell维持权限
