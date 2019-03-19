# Kubernetes—GCP應用

# 想法:

當我們在本地建立單節點或多節點的環境都會有一個缺點，那就是持久性，如果我們本地的主機關機，那環境就會出現中斷，如果把環境佈署在多台主機上，反而又需要龐大的資源，所以雲服務就是最好的解決辦法，既有高持久性也能降低成本，而Google的雲服務GCP也有提供K8s的單獨產品GKE，在這個Lab中我建立了三台虛擬機相連結成一個cluster。



# 簡介:
      Google Container Engine 是功能強大的叢集管理器和協調系統，可用於執行 Docker 容器。Container Engine 可將容器安排成叢集，並根據設定的需求 (例如 CPU 和記憶體) 自動加以管理。
        --[官方GKE使用及說明](https://gcpug-tw.gitbook.io/google-cloud-platform-in-practice/google-cloud-shang-de-yun-suan-fu-wu/container-engine)
# 實作流程:
               首先先開通GCP帳號，接著我們會建立三台的虛擬機(VM)，一台主機當master，兩     台當Node，再將兩台Node加入到master，建立一個雲服務的多節點環境，最後建立一個pod驗證cluster是否成功運作。
# 01.建立虛擬機(master,node01,node02)

點選左上角的瀏覽選單展開--Compute Engine--VM 執行個體--建立--設定虛擬機資源--建立SSH私鑰進行SSH訪問--點選建立--完成第一台(master)。

![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551771188290_01-0VM.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551771456652_01-1VM.png)

| 名稱                           | master                      |  |
| ---------------------------- | --------------------------- |  |
| 區域                           | 預設                          |  |
| Machine type<br>      (機器類型) | 預設                          |  |
| Boot disk<br>      (開機磁碟)    | 點選變更--         Centos7      |  |
| 身分及API存取權                    | 預設                          |  |
| Firewall<br>        (防火牆)    | 勾選--HTTP<br>          HTTPS |  |
|                              |                             |  |

## 
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551779652806_image.png)


建立ssh-key

    ssh-keygen
    #Enter file in which to save the (預設存取位置):可自行命名key名字
    路徑/自行命名的key名稱
    #密碼可以不用輸入
    #獲取金鑰碼
    cat .ssh\key-id
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551772164283_01-3VM-ssh-key.PNG)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551772435582_01-3VM-ssh-key-2-1.PNG)

## 將ssh-key的金鑰碼複製到-安全性-SSH金鑰貼上，左邊則會出現金鑰的用戶
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551773423987_01-3VM-ssh-key-3.PNG)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551773854842_01-4VM-OK.PNG)


依照流程再建立兩台虛擬機(node-01,node-02)。#金鑰都使用同一把。

## 
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551780901992_image.png)


測試連線


    ssh -i .ssh/key名字 用戶@虛擬機IP
![成功連線至master主機](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551774417001_image.png)

![成功連線至node-01主機](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551775318426_image.png)

![成功連線至node-02主機](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551775437578_image.png)

# 02.安裝K8s相關工具
## 建立腳本然後執行腳本--3台主機都要安裝。
    #腳本--安裝k8s工具還有防火牆設定。
    #/bin/sh
    
    # install some tools
    sudo yum install -y vim telnet bind-utils wget
    
    sudo bash -c 'cat <<EOF > /etc/yum.repos.d/kubernetes.repo
    [kubernetes]
    name=Kubernetes
    baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
    enabled=1
    gpgcheck=1
    repo_gpgcheck=1
    gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
    EOF'
    
    # install and start docker
    sudo yum install -y docker
    sudo systemctl enable docker && sudo systemctl start docker
    # Verify docker version is 1.12 and greater.
    
    sudo setenforce 0
    
    # install kubeadm, kubectl, and kubelet.
    sudo yum install -y kubelet kubeadm kubectl
    
    sudo bash -c 'cat <<EOF >  /etc/sysctl.d/k8s.conf
    net.bridge.bridge-nf-call-ip6tables = 1
    net.bridge.bridge-nf-call-iptables = 1
    net.ipv4.ip_forward=1
    EOF'
    sudo sysctl --system
    
    sudo systemctl stop firewalld
    sudo systemctl disable firewalld
    sudo swapoff -a
    sudo systemctl enable kubelet && sudo systemctl start kubelet
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551776190484_image.png)

![查看安裝的工具資訊](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551776470357_image.png)

# 03.建立叢集
## 我們將一台主機做初始化為管理主機建立K8s的apiserver並指定連線的IP。

#要做為管理的主機CPU一定要2核心

    #sudo kubeadm init 初始化
    #--pod-network-cidr 172.100.0.0/16 指定連結pod的IP，ip可自行指定，不可跟主機衝突
    #--apiserver-advertise-address 10.142.0.6 指定做為apiserver主機的IP
    #--apiserver-cert-extra-sans 34.73.36.152 指定做為對外連接的apiserver主機IP
    #-----------------------------------------------------------------------------------#
    
    sudo kubeadm init --pod-network-cidr 172.100.0.0/16 --apiserver-advertise-address 10.142.0.6 --apiserver-cert-extra-sans 34.73.36.152
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551780781411_image.png)

![初始化完成，](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551780822664_image.png)

## 在初始化完成後，系統會提示接下來的步驟。
    
    To start using your cluster, you need to run the following as a regular user:
    #建立資料夾
    mkdir -p $HOME/.kube
    #複製admin.conf到.kube/config
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    #將config的使用者變更為目前使用者
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551784399918_02-02.png)

    You should now deploy a pod network to the cluster.
    進入系統提示網址:https://kubernetes.io/docs/concepts/cluster-administration/addons/
    #安裝插件
    選擇要安裝的網路規範
     Weave Net 提供了在网络分组两端参与工作的网络和网络策略，并且不需要额外的数据库。
    #進入該規範後會有介紹和使用說明、安裝方式
    kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551784641721_02-02.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551785058954_02-03.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551785399224_02-04.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551785332812_image.png)


執行命令是否運作:

    kubectk get pod --all-namespaces
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551785513857_image.png)

## 將其它兩台主機(node)加入master主機
    sudo kubeadm join 10.142.0.6:6443 --token i6c7pc.j8dyk1mx2z0v2viy --discovery-token-ca-cert-hash sha256:96803293cd69900ff76b91a87ac746f80f2ded61ce0cd2c54047cbd3d769e414
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551785836808_02-05.png)


node-01

![#系統顯示:已經將此節點加入叢集](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551785961973_image.png)

## 在管理主機查看cluster狀態
    #查看當前節點
    kubectl get node
![#兩台主機(node)已加入cluster](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551786200326_image.png)

# 在管理主機上建立一個pod驗證cluster是否建立成功。
    kubectl create -f https://raw.githubusercontent.com/udemy-course/Kubernetes-CN/master/section3/namespace/nginx.yml
![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551786813341_image.png)


由此可以知道我們建立起來的pod在node-01這個節點上。

![](https://d2mxuefqeaa7sj.cloudfront.net/s_968247585D35B3D3A333AC5DD646A88ADFE439621091F6195E6504555C5565A0_1551786918290_image.png)


實作心得:在這個Lab中我用google的雲平台實作了用三台虛擬機建立了一個多節點的環境，比起傳統的本地搭建既快速又簡單，而K8s的單獨產品GKE則可以透過web的操作介面自動化建立cluster，並且快速的佈署容器、環境。
@Bing-hong L 

