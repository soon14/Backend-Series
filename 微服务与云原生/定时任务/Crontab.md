# Crontab

> - [Linux 下的定时任务，Crontab 用法](http://www.cnblogs.com/b028/archive/2011/01/07/1930243.html)
> - [Cron 表达式生成器](http://www.pdtools.net/tools/becron.jsp)

要写个在 Linux 下定时更新系统的脚本，man crondtab 不甚详细，现将网络上的介绍列举如下：

crontab 是一个很方便的在 unix/linux 系统上定时(循环)执行某个任务的程序使用 cron 服务，用 service crond status 查看 cron 服务状态，如果没有启动则 service crond start 启动它，cron 服务是一个定时执行的服务，可以通过 crontab 命令添加或者编辑需要定时执行的任务：

crontab -u //设定某个用户的 cron 服务，一般 root 用户在执行这个命令的时候需要此参数
crontab -l //列出某个用户 cron 服务的详细内容
crontab -r //删除没个用户的 cron 服务
crontab -e //编辑某个用户的 cron 服务

比如说 root 查看自己的 cron 设置：crontab -u root -l

再例如，root 想删除 fred 的 cron 设置：crontab -u fred -r

在编辑 cron 服务时，编辑的内容有一些格式和约定，输入：crontab -u root -e

进入 vi 编辑模式，编辑的内容一定要符合下面的格式：_/1 _ \* \* \* ls >> /tmp/ls.txt

编辑/etc/crontab 文件，在末尾加上一行: 30 5 \* \* \* root init 6 这样就将系统配置为了每天早上 5 点 30 自动重新启动。

需要将 crond 设置为系统启动后自动启动的服务，可以在/etc/rc.d/rc.local 中，在末尾加上

service crond start

如果还需要在系统启动十加载其他服务，可以继续加上其他服务的启动命令。

比如: service mysqld start

基本用法:

1. crontab -l
   列出当前的 crontab 任务
2. crontab -d
   删除当前的 crontab 任务
3. crontab -e (solaris5.8 上面是 crontab -r)
   编辑一个 crontab 任务,ctrl_D 结束
4. crontab filename
   以 filename 做为 crontab 的任务列表文件并载入

crontab file 的格式:
crontab 文件中的行由 6 个字段组成，不同字段间用空格或 tab 键分隔。前 5 个字段指定命令要运行的时间
分钟 (0-59)
小时 (0-23)
日期 (1-31)
月份 (1-12)
星期几(0-6，其中 0 代表星期日)
第 6 个字段是一个要在适当时间执行的字符串
例子:
#MIN HOUR DAY MONTH DAYOFWEEK COMMAND #每天早上 6 点 10 分
10 6 \* \* _ date #每两个小时
0 _/2 \* \* _ date (solaris 5.8 似乎不支持此种写法) #晚上 11 点到早上 8 点之间每两个小时，早上 8 点
0 23-7/2，8 _ \* _ date #每个月的 4 号和每个礼拜的礼拜一到礼拜三的早上 11 点
0 11 4 _ mon-wed date
#1 月份日早上 4 点
0 4 1 jan \* date
