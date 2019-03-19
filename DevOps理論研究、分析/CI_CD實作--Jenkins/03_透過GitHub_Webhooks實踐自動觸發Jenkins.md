# 03.透過GitHub Webhooks實踐自動觸發Jenkins
[+02.整合GitHub](https://paper.dropbox.com/doc/02.GitHub-baszg1NWKCyMMUtbrw8QI) 



# 由於需要 https ， 所以我們透過 ngrok 將內網轉換為外網


# ngrok
# 01.解壓縮ngrok後，到目錄下執行ngrok。                              指令: ngrok http 8080
![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551284865601_image.png)

# 02.執行程式後等待時間轉址。
![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551284939666_image.png)

# 03.回到Jenkins頁面點選組態。
![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551285153213_12.png)

# 04.找到建置觸發程序，並選擇 GitHub hook trigger for GITScm polling
![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551285275262_13.png)

# 05.到GitHub，選擇專案-點選Sttings-Webhooks-Add webhooks
![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551285450030_14.png)

# 06.Payload URL 就是填入 ngrok 幫你產生的 https 的網址，在網址後面補上 /`github-webhook`/，**Content type:application/json**
![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551286037097_15.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551286053593_16.png)

# 07.添加成功，而ngrok顯示GitHub-webhook已建立連接
![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551286542917_image.png)

![](https://d2mxuefqeaa7sj.cloudfront.net/s_801E36BCE3A3BA74D3C93201F7102DA49C884D8409520CF3714B86593ECD1E65_1551286570977_image.png)

# 08.總結:這一系列的設定為，只要我們git push後，github webhook就會觸發通知給Jenkins，而Jenkins就會自動交付。





https://ithelp.ithome.com.tw/articles/10193212


