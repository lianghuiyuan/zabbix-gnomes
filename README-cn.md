引用： [Zabbix-gnomes：Zabbix 命令行工具套件](https://cloud.tencent.com/developer/article/1578783)

Zabbix-gnomes 是一组 Zabbix 的开源脚本工具集合，它使用 Python 对 Zabbix API 的进行了分装，使得日常的大部分操作可以通过命令行完成，非常方便。最新的 Zabbix-gnomes 代码可以在 Github 上获取到。

所有 zabbix-gnomes 相关的工具，都可以用 -h/–help 调用获得帮助。

API工具：

zapi.py – 交互式 Zabbix API客户端。

相关历史：

zgethistory.py – 从历史记录获取一个itemid的值。

zhinvswitcher.py – 在主机( 群组组) 上切换 inv。

zgetinventory.py – 以CSV格式打印主机清单。

zhostupdater.py – 更新主机属性。

zhitemfinder.py – 查找主机上的项目。

zgethistory.py – 从历史记录( 不支持趋势) 获取项值。

zhgraphfinder.py – 查找在Zabbix主机上配置的图形。

zgetgraph.py – 从Zabbix前端( 需要用户前端访问) 下载一个图形. PNG 并保存它。

zghostfinder.py – 查找hostgroup中的成员主机。

zhostfinder.py – 根据搜索字符串在Zabbix中查找主机。

zhostupdater.py – 更新主机属性。

zhproxyfinder.py – 为Zabbix主机查找配置的代理。

ztmplimport.py – 将 xml 模板导入 Zabbix。

zhtmplfinder.py – 查找Zabbix主机的链接模板。

zthostfinder.py – 查找链接到模板的主机。

zthtmllinker.py – 将主机( 群组组)的链接链接到模板列表。

zthtmlunlink.py – 将主机( 群组组) 与模板列表断开。

zhtrigfinder.py – 在主机上查找触发器。

ztrigswitcher.py – 将触发器切换为已经启用或者discabled状态。

zhostupdater.py – 更新主机属性。

zeventfinder.py – 基于过滤器(。包含 tail -f 模式) 查找事件。

zgetevent.py – 获取eventIds的详细信息，包括和ack警报操作。

zeventacker.py – 基于eventIds确认事件。

配置 Zabbix-gnomes

这些程序可以使用 .ini 风格的配置文件，来获取所需的API连接信息。它的配置文件默认为 $HOME/.zbx.conf，样例如下：

```
[Zabbix API]
username=debian
password=Debian-CN-Debian.cn
api=http://aws-zabbix-1/zabbix/
no_verify=true
```
将 no_verify 设置为 true 将在使用 https 时禁用 tls/ssl 证书验证。

脚本依赖 pyzabbix、pillow 模块：

```
pip install pyzabbix pillow
```

### 用法样例
将主机 www.debian.cn 的一个月的CPU负载数据保存为 ~/jan.png，具体时间跨度是 2019年01月至1月31日：
```
graphid=$(./zhgraphfinder.py -e p-hsg-mysql-2 | grep 'CPU load' | cut -d ':' -f 1)
./zgetgraph.py -s $(date --date 'jan 1 2019' +%s) -t 2678400 -f ~/jan.png $graphid
```
输出的图形如下：



使用 zproxyfinder.py 在 zabbix_sender 脚本中使用恰当的 Zabbix 代理，
```
zabbix_sender -k $ITEMKEY -o $ITEMVALUE -s $HOSTNAME -z $(zhproxyfinder.py $HOSTNAME)
```
