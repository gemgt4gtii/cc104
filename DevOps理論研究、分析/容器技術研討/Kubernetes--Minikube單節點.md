# Kubernetes--[Minikube](https://github.com/kubernetes/minikube)單節點

# 總結:Minikube由於需求的資源較少，可以快速安裝佈署環境。
## 實作環境:ubuntu                                                                                                     **安裝工具:** Minikube、VirtualBox


#     [+Kbernetes--基本介紹](https://paper.dropbox.com/doc/Kbernetes-hmnILF0hqQuyfQKuvJ1qT)                                                                    
# 節點:
      Node也稱為Worker或Minion，是部署容器（工作負載）的單機器（或[虛擬機](https://zh.wikipedia.org/wiki/%E8%99%9A%E6%8B%9F%E6%9C%BA)）。集群中的每個節點都必須具備容器的運行環境（[runtime](https://zh.wikipedia.org/wiki/%E8%BF%90%E8%A1%8C%E6%97%B6)） ——比如 [Docker](https://zh.wikipedia.org/wiki/Docker)，以及下面提到的其他組件，以便與這些容器的網絡配置進行通信。---wiki
## 學習環境:
        [**Play with Kubernetes**](https://labs.play-with-k8s.com/)**--線上操作環境，有4個小時的時間，一旦時間一到就會銷毀。**
        [GCP雲服務](https://cloud.google.com/kubernetes-engine/?hl=zh-tw)--在一定的試用期間免費。
# **Minikube介紹:**                                                           Minikube 是一個輕量的 Kubernetes 方案，透過在本機上建立 VM 與佈署簡易叢集容器。Minikube 可以運行在 Linux、Max OS 與 Windows 作業系統上。Minikube CLI 提供基本的叢集操作練習環境，包含啟動（start）、停止（stop）、狀態（status）與刪除（delete）。
## [Minikube快速搭建K8S單節點環境](https://github.com/kubernetes/minikube)
# 01.到Kubernetes的GIitHub/**minikube****專案，點選****releases**
![](https://d2mxuefqeaa7sj.cloudfront.net/s_F7C1CD749DEE17DA67FFEDF669AEE6F09ACB7656E9B43D594209F30FAE0B7DB6_1551365841868_minikube001.png)

# 02.依作業環境下載檔案
![](https://d2mxuefqeaa7sj.cloudfront.net/s_F7C1CD749DEE17DA67FFEDF669AEE6F09ACB7656E9B43D594209F30FAE0B7DB6_1551365954964_image.png)

# 03.下載完後到目錄下
    #更改檔名
      mv minikube-linux-amd64 minikube
    #加入可執行權限
    chmod +x minikube
    #執行help，可以查看minikube的指令使用方式
    ./minikube --help
    #將minikube移至/usr/local/bin/，即可讓minikube當作指令使用。
    mv minikube /usr/local/bin/
![](https://d2mxuefqeaa7sj.cloudfront.net/s_F7C1CD749DEE17DA67FFEDF669AEE6F09ACB7656E9B43D594209F30FAE0B7DB6_1551366793682_image.png)

# 04.安裝[kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)命令列。
    sudo apt-get update && sudo apt-get install -y apt-transport-https
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
    sudo apt-get update
    sudo apt-get install -y kubectl
# 05.啟動minikube，當我們運行minikube，它會下載Minikube.ISO的檔案，在以VirtualBox安裝並建立一台虛擬機，這台虛擬機中會安裝所有K8s的組件並啟動服務。
    minikube start
![](https://d2mxuefqeaa7sj.cloudfront.net/s_F7C1CD749DEE17DA67FFEDF669AEE6F09ACB7656E9B43D594209F30FAE0B7DB6_1551372747169_image.png)

# 06 .啟動minikube dashboard(儀錶板)，會跳出web頁面。
    minikube dashboard
![](https://d2mxuefqeaa7sj.cloudfront.net/s_F7C1CD749DEE17DA67FFEDF669AEE6F09ACB7656E9B43D594209F30FAE0B7DB6_1551373104851_image.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_F7C1CD749DEE17DA67FFEDF669AEE6F09ACB7656E9B43D594209F30FAE0B7DB6_1551373268108_image.png)

# 07.查看叢集資訊
    kubectl cluster-info
![](https://d2mxuefqeaa7sj.cloudfront.net/s_F7C1CD749DEE17DA67FFEDF669AEE6F09ACB7656E9B43D594209F30FAE0B7DB6_1551414736541_image.png)


