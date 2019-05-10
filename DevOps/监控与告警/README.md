# 传统监控

Zabbix 是我们当时的选择，简单的配置页面，丰富的 agent 数据采集，支持短信、邮件及微信告警，在一个星期内，我们就完成了全站的基础监控。Zabbix 开箱即用的使用方式，适合初创型公司。即使是现在，Zabbix 还在线上运行，监控网络设备的运行状态。随着业务高速发展，研发的需求越来越复杂，同时也暴露出 Zabbix 的很多缺点。

- Zabbix 性能瓶颈，监控数据存储在 Mysql 中，随着监控数据越来越多，Zabbix 响应时间变慢。

- Zabbix 只支持 metric 类型监控，对于日志类监控，支持并不友好。

- Zabbix 监控大盘页面不美观，无法满足业务方定制的需求。

![](https://ww1.sinaimg.cn/large/007rAy9hgy1g0f16geqrtj30go08cq3i.jpg)

# 数据采集

## CAT

CAT (Central Application Tracking) 是基于 Java 开发的实时监控平台，主要包括移动端监控，应用侧监控，核心网络层监控，系统层监控等。

CAT 的优点是功能丰富，支持钉钉告警，95 线 99 线计算，可展示代码级别监控，在代码层故障定位提供了强有力的工具。

![](https://ww1.sinaimg.cn/large/007rAy9hgy1g0f16geqrtj30go08cq3i.jpg)

## LEPUS

Lepus(天兔) 数据库企业监控系统是一套由专业 DBA 针对互联网企业开发的一款强大的企业数据库监控管理系统。Lepus 后端采用 Python 语言开发，对于运维非常友好，可以很方便地作出一些个性化的修改。

Lepus 的优点是无需安装 agent，账号集中管理，适合作为数据库的 CMDB 使用。

![](https://ww1.sinaimg.cn/large/007rAy9hgy1g0f16geqrtj30go08cq3i.jpg)

## ELK 监控生态

ELK (Elasticsearch,Logstash,Kibana) 是 Elastic 公司提供的三个开源组件。在日常工作中，我们需要进行日志分析场景：直接对日志文件进行 grep、awk 等正则操作，获取我们想要的信息。在大规模的场景中，日志文件分布在不同的服务器上，且文件非常大，逐台操作性能非常低。比如 Nginx 日志，Mysql 慢查询日志，应用 log 日志等。ELK 提供一整套的解决方案，可以帮助我们快速全站查询。

下图是 Mysql 慢查询的截图，通过 Python 脚本，可以实时读取 Mysql 慢查询日志，并写入 ES，方便查看线上问题。

![](https://ww1.sinaimg.cn/large/007rAy9hgy1g0f3f3fcn3j30gm07qq3f.jpg)

下图是服务器的 dashboard，通过模糊匹配，可以快速查询相关服务器组的性能指标。

![](https://ww1.sinaimg.cn/large/007rAy9hgy1g0f3f3fcn3j30gm07qq3f.jpg)

## Open-Falcon

Open-Falcon 是小米开源的监控系统，灵活的数据采集，水平扩展能力以及高效的告警策略帮助我们快速监控 servers 的信息。在实际的环境中，我们仅采用了 falcon-agent、falcon-transfer 组件，帮助我们采集数据，具体的存储及展示由更专业的组件处理。

![](https://ww1.sinaimg.cn/large/007rAy9hgy1g0f3i634lsj30u00jwabs.jpg)

# 数据存储

Prometheus 提供了丰富的数据模型和查询语句，容易上手，很容易集成到现有的环境中，但是 Prometheus 的集群和 HA 架构并不成熟，需要额外的开发，并不适合。
InfluxDB 是在 Prometheus 之后才提出的，并且提供商业的伸缩和集群化服务，相比 Prometheus 的 metrics 存储，InfluxDB 还能处理事件类型的数据，对于大部分公司而言，商业化基本不会考虑。
OpenTSDB 是一个基于 Hadoop 和 Hbase 的分布式事件序列数据库，相比 Prometheus 和 InfluxDB，OpenTSDB 的横向扩缩容很容易 (需要有丰富的 Hadoop/HBase 维护经验), 同时官方 Open-falcon 支持 OpenTSDB。

关于数据展示的选型，在没有自研能力的情况下，Grafana 是不二选择。Grafana 的告警功能强大方便，同时支持钉钉，Webhook 等，满足公司所有的需求。与此同时，我们将 Grafana 和 Docker 技术结合，实现了 Grafana 高可用、自愈和无限扩展能力。

# AIOps

在数据采集阶段，保留了 Open-Faclon、CAT、 客户端 SDK、Logstash 等入口，通过 Kafka 进行汇聚，引入大数据实时计算平台 Flink，提炼 metric 指标，并最终入库。

![](https://ww1.sinaimg.cn/large/007rAy9hgy1g0f3i634lsj30u00jwabs.jpg)

在告警方面，开发了 Alert Manager 组件，对接多种告警渠道：钉钉、短信、微信等，并且自动创建 Jira，告警闭环。
