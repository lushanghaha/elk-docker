# 使用 Docker ELK stack 收集 CentOS logs

針對 ssh 登入 log 做收集：`/var/log/sercure*`

## 包含以下套件

* Filebeat 5.2.1：
    * 做法會將 log 檔案 mount 進 Filebeat container
    * 用來監聽 `/mnt/log/*`
* Logstash 5.1.2：
    * 作為 indexer 的角色，將 log 們做分類 (各種 index)
* Elasticsearch 5.1.2：
    * 開啟 port 9200，供 Kibana 以及 Cerebro 使用
* Kibana 5.1.2：
    * 開啟 port 5601，供使用者瀏覽 web UI
* Cerebro 5.1.2：
    * 因 5.1.2 的 Elasticsearch 不支援 KOPF，故用此代替！
    * 開啟 port 9000，供使用者瀏覽 web UI
* [wait-for-it](https://github.com/vishnubob/wait-for-it)  
    * 為了讓 docker-compose 內的服務，可以等待相依的服務啟動的小工具

串接方式：Filebeat \> Logstash \> Elasticsearch \> Kibana  
註：ELK 映像檔來源為 Docker Hub

## 使用方式

為了順利啟動 Elasticsearch，官方建議將 virtual memory 增大。   
編輯 `/etc/sysctl.conf` 加上 `vm.max_map_count=262144` 後存檔。

啟動
```bash
$ docker-composer up --build --force-recreate
```
建立 Kibana visualizations
```bash
$ #尚未處理
```
結束
```
Ctrl + C
```
Docker 清除停止的 container
```
$ docker rm (docker ps --filter=status=exited --filter=status=created -q)
```
Docker 清除 untagged images
```
$ docker rmi (docker images -a --filter=dangling=true -q)
```
## 測試：收集 macOS system.log

macOS 要進 Preferences UI 設定
* File Sharing：```/private```
* Advanced：(我的設定)
    * CPUs：8
    * Memory：8.0 GB (記憶體不夠會掛喔)

## 待處理問題

* Filebeat 如果不用 root 啟動，要怎麼讀外部掛進去的 system log (權限不足)
    * Docker 官方表示在 container 用 root 不會太危險，所以我用 root 跑 filebeat
    * https://docs.docker.com/engine/security/security/
* 終極目標：把容易變動的設定都統一抽到 `docker-compose.yml` 內
