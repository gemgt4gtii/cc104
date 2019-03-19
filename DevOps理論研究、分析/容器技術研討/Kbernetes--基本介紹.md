# Kbernetes--基本介紹

# 結論:從網路各大文章統整出來的的K8s介紹，主要在介紹K8s的重點技術並精簡摘要，方便閱讀理解，在通過實作[+Kubernetes--Minikube單節點](https://paper.dropbox.com/doc/Kubernetes-Minikube-4exqa6GwaW3lixJbDjP7s) 和[+Kubernetes--多節點環境(Vagrant)](https://paper.dropbox.com/doc/Kubernetes-Vagrant-AdE8KgeQaH5ijjk3aK9LR) ，就會對K8s運行的流程有大概的認識。

分享一首好歌

https://www.youtube.com/watch?v=Ah_SN-2dTzY

# 簡介:
      **Kubernetes**（常簡稱為**K8s**）是用於自動部署、擴展和管理[容器化](https://zh.wikipedia.org/wiki/%E4%BD%9C%E6%A5%AD%E7%B3%BB%E7%B5%B1%E5%B1%A4%E8%99%9B%E6%93%AC%E5%8C%96)（containerized）應用程式的開源系統。
      虛擬化技術是建立於實體資源之上分割出來，容器技術的優點則是輕量化，移轉性強，無論任何作業環境，皆能正常運行容器化後的應用程序。                                                                       
# Kubernetes 基本概念
## Container（容器）
      Container是一種便攜式，輕量級的操作系統級虛擬化技術。它使用namespace隔離不同的軟件運行環境，並通過鏡像自包含軟件的運行環境，從而使得容器可以很方便的在任何地方運行。
      K8s的容器從屬於pod。在運行應用、庫及其依賴的微服務中，容器是最低層級的。通過綁定一個外部IP，容器可以被外網訪問。
## Node(節點)
      Node也稱為Worker或Minion，是部署容器（工作負載）的單機器（或[虛擬機](https://zh.wikipedia.org/wiki/%E8%99%9A%E6%8B%9F%E6%9C%BA)）。集群中的每個節點都必須具備容器的運行環境（**runtime:**例如:docker） ——比如 [Docker](https://zh.wikipedia.org/wiki/Docker)，以及下面提到的其他組件，以便與這些容器的網絡配置進行通信。---wiki
## Pod
      相當於群組的概念，而K8S調度的基本單位稱為Pod(圖一)。一個pod一般包含一個或多個容器，這樣可以保證它們一直位於主機上，並且可以共享資源。在Kubernetes中，所有對像都使用manifest（yaml或json）來定義(圖二)，我們可以透過撰寫yaml來定義Pod裡的Container。
      
      
![圖一，在圖中可以知道有單一節點(機器)裡分別有4個Pod。](https://d2mxuefqeaa7sj.cloudfront.net/s_4120CAC16C73539117306D90B2408DB24EEA70D85AC4C99B2B406742E92941E0_1551527169811_image.png)
![圖二，在圖中定義了兩個Container，nginx和busybox。](https://d2mxuefqeaa7sj.cloudfront.net/s_4120CAC16C73539117306D90B2408DB24EEA70D85AC4C99B2B406742E92941E0_1551526849114_image.png)

# Namespace            
        Kubernetes 提供了抽象的 Cluster (virtual cluster) 的概念，讓我們能根據專案不同、執行團隊不同，或是商業考量，將原本擁有實體資源的單一 Kubernetes Cluster ，劃分成幾個不同的`抽象的 Cluster (virtual cluster)`，也就是 Namespaces。
# Service
        是應用服務的抽象，通過labels為應用提供負載均衡和服務發現。匹配labels的Pod IP和端口列表組成端點，由kube-proxy負責將服務IP負載均衡到這些端點上。
        每個服務都會自動分配一個集群IP（僅在集群內部可訪問的虛擬地址）和DNS名稱，其他容器可以通過該地址或DNS來訪問服務，而不需要了解後端容器的運行。
# Label
        是識別 Kubernetes 對象的標籤，以 key/value 的方式附加到對像上，如:
    "release" : "stable"，"release" : "qa"
    "enviroment": "dev"，"enviroment": "production"
    "tier": "backend", "tier": "frontend"
    "department": "enginnerting", "department": "marketing", "department": "finance"

若Kubernetes Cluster中的 Pod 物件帶有標籤，由上面幾個 labels 我們可以清楚知道，他們的功用是什麼、層級是什麼，他們服務的對象是哪些部門的人。

# **Annotations**
          如果是沒有識別用途的標籤，Kubernetes 也提供了我們一個 Annotations 元件。以 Pod 為例，我們可以在Pod的 Annotations 紀錄該 Pod的發行時間，發行版本，聯絡人email等。Annotations 主要是方便開發者、以及系統管理者管理上的方便，不會直接被 Kubernetes使用。


# Deployment
          可以幫我們達成以下幾件事情：
            - 部署一個應用服務(application)
            - 協助 applications 升級到某個特定版本(快速更新)
            - 服務升級過程中做到無停機服務遷移(zero downtime deployment)
            - 可以Rollback到先前版本(還原)









































@Li B 

