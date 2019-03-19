# Kubernetes--多節點環境(Vagrant)
總結:這份文件的流程是用Vagrant建立的k8s多節點環境，還有kubectl的命令列使用說明和bash的補全，config的設定。
有提供系統錯誤提示的解決方法，以便日後在碰到相同問題時不需要再翻找文件。

# 
# 實作環境:VMware-Ubuntu 18.04
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551447317096_.png)

  # 工具:
      - Vagrant
      - VirtualBox
      - kubectl
# 實作說明:**Vagrant+VirtualBox搭建K8S**多節點環境
## 官方GitHub:https://github.com/pires/kubernetes-vagrant-coreos-cluster
# 下載連結:
          [**Vagrant**](https://www.vagrantup.com/downloads.html)**(手動下載):**是一款用於構建及配置虛擬開發環境的軟件，主要使用Oracle的開源VirtualBox虛擬化系統，與Chef，Salt，Puppet等環境配置管理軟件搭配使用， 可以實行快速虛擬開發環境的構建。1.1以後的版本中開始VMware等虛擬化軟件，包括Amazon EC2之類服務器環境的對應。
    #ubuntu-安裝指令
    sudo apt install -y vagrant
    #VirtualBox安裝指令
    sudo apt install -y virtualbox
# 01.透過官方的GitHub下載到作業的環境。
    #git 下載 網址 命名
    git clone https://github.com/pires/kubernetes-vagrant-coreos-cluster.git vagrant-coreos
----------
# 02.到git clone下的目錄底下啟動**Vagrant建立虛機，它會下載需要創建的image，並且初始化虛機然後安裝所需要的軟體並啟動。**
    cd ~/Desktop/vagrant-coreos
    vagrant up
![#錯誤提示:需要在有Vagrantfile的目錄下執行](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551417247791_image.png)

    解決方法:
    #1.查看本地box列表：
      vargant box list
    #2.初始化環境：
      vagrant init
    #此時會產生.vagrant/和Vagrantfile
    #3.編輯Vagrantfile文件
    #4.vagrant up
![#錯誤提示:版本差異，透過apt安裝的vagrant版本為2.0.2，需要更新版本才能啟動(2.2.X)。](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551417573227_image.png)

    解決方法:
      #先移除舊的vagrant
    sudo apt remove -y vagrant
      #透過網站抓取檔案來源
    wget https://releases.hashicorp.com/vagrant/2.2.2/vagrant_2.2.2_x86_64.deb
      #安裝壓縮包
    sudo dpkg -i vagrant_2.2.2_x86_64.deb
      #查看目前版本
    vagrant -v
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551421004036_image.png)

----------
----------
## 成功執行
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551431951154_image.png)

# 03.查看虛擬機狀態，通過ssh進入節點(虛擬機)，然後輸入docker ps查看k8s的組件
    vagrant status
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551432159371_image.png)

    #vagrant 選項 主機名稱
    vagrant ssh master
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551432420834_image.png)

    #列出docker所運行的服務
    docker ps
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551432488301_image.png)

    #顯示執行中的節點
    kubectl get nodes
![補充:master主機主要負責去佈署其他截點的主機。](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551432690276_image.png)

# 04.到目前為止我們一次性地去建立了兩個節點master類似於控制中心(也就是大腦)，負責去佈署其他節點。
----------
# Kubectl 使用與介紹

指令補全，當我們輸入Kubectl的時候卻無法通過用TAB鍵顯示或補全可用的指令選項，我們可以通過Kubectl 的help來幫助我們去設定bash補全的功能

    #顯示 幫助
    kubectl completion -h
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551434900969_image.png)

    #依照環境將bash指令複製並執行(Linux系統)
    source <(kubectl completion bash)
    #輸入之後，就可以使用Tab鍵補齊指令
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551435258523_image.png)

----------
# 顯示clusters(群集)的狀態
    kubectl config view
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551442572600_image.png)

    輸入指令後會顯示我們目前的clusters，第一個clusters是我們從Vagrant的GitHub複製下來所建立的多節點clusters。
    第二個clusters則是我們minikube所建立的單節點clusters。
# 我們可以通過本機上K8s的config去修改clusters的參數，比如修改clusters的name，切換連接的clusters…等等。
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551443279083_image.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551444101760_tempsnip.png)

# 切換contexts
    #顯示目前的contexts
    kubectl config get-contexts
    #切換contexts。
    #指令 選項 contexts名字
    kubuctl config set current-context minikube
    #顯示目前節點
    kubectl get node
![*表示我們目前連接的contexts](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551444288716_image.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551444718214_image.png)

# 更改context的name
    vim ~/.kube/config
    將context的local修改然後儲存並離開
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551445060069_01.png)

    kubectl config get-contexts
    原本的local被修改為vagrant-coreos
![](https://d2mxuefqeaa7sj.cloudfront.net/s_785DEFD94F86BD0B5C99322548C77CAC8A740A3FDD24A57DD443F39EC2AB3D2C_1551445187670_image.png)


@Li B 

----------

下一篇:透過Google Cloud服務快速搭建多節點的K8S群集

