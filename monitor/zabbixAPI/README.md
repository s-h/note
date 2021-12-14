pyzabbix以及zabbix api笔记

***连接zabbix

    from pyzabbix import ZabbixAPI
    zapi = ZabbixAPI("http://127.0.0.1/zabbix")
    zapi.login("Admin", "zabbix")

***获得主机

    zapi.host.get(filter={"available":[1, 2]})
    # zabbix agent 可用性
    #   0-unknown;
    #   1-available;
    #   2-unavailable.

    # snmp_available snmp可用性
    #   0-unknown;
    #   1-available;
    #   2-unavailable.

***按照模板查询主机

    zapi.host.get(output="hostid",selectParentTemplates=["templateid","name"], templateids=10001)
    #https://www.zabbix.com/documentation/3.4/manual/api/reference/host/get

***获取告警

    for i in  zapi.trigger.get(only_true=1, skipDependent=1, monitored=1, active=1, output='extend', expandDescription=1, selectHosts=['host']):
    print(i['hosts'][0]['host'], i['description'], i['priority'], i['lastchange'])
    # priority 告警级别
    #   0-未分类
    #   1-信息
    #   2-告警
    #   3-一般严重
    #   4-严重
    #   5-灾难
