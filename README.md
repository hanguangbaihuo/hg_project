## 麻雀团队简介 ##

    隶属于北京汉光百货有限公司
    地址: 北京市西城区西单北大街176号
    网站首页: http://www.hanguangbaihuo.com/

### 团队使命 ###

    植根于零售行业. 旨在改造百货行业, 打造一个线上线下完全一体化百货服务体系.

### 产品 ###

    1. 汉光百货+(小程序)
        购物商城
    2. 导购工作台(小程序)
        为汉光百货2000多导购提供一个快捷,易用的 IT 服务平台
    3. 提货小程序(对接汉光百货线上服务和线下服务)


### 技术架构: 微服务 ###

    服务流量特点: 由于运营需要,流量分布非常的不均衡.并且成为一种常态.
    每天有一个流量的高峰期. 最高的秒级并发量可以达到 2000. 低峰时期并发量维持在30左右.
    服务采用弹性云计算

    基础云服务架构:
    1. kubernetes
    2. docker

    组件:
    1. 服务注册与发现: consul
        采用 consul-sync 同步 kubernetes 的 service 到 consul
    2. 服务网关: kong
        使用 kong-ingress-controller 同步 kubernetes 的 ingress
    3. 配置中心: cunsul
    4. 搜索引擎: elasticsearch + kibana
        运行在 kubernetes 的搜索引擎集群
    5. 日志: fluent-bit + es + kibana
    6. apm系统性能监控:
    7. 基础模块: gRPC

### 开发语言 ###

    python
    golang
    nodejs

### 数据库 ###

    1. MySql
    2. memcached
    3. Redis

### 测试 ###

    单元测试
    集成测试
