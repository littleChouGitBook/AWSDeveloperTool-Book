# AWS CodeBuild 使用 託管映像 安裝 PHP Extension 時的注意事項



使用 AWS 提供的託管映像，如果安裝 PHP Extension 會發生找不到的狀況 ，原因是因為 Amazon 託管映像安裝的 PHP 區域有特別改過路徑 

![](https://i.imgur.com/MOnZOry.png)

解決方案：
打開建置日誌，看看 log 內是否有 extension 路徑 （如下圖黃色標示處）有找到先複製下路徑

![](https://i.imgur.com/Dc2YZnQ.png)

並在 BuildSpec.yml build 階段中 複製以下語法，並將 **[實際安裝的 extension 路徑]** 貼上你剛剛的 extension 路徑上去即可完成

```
- php -i | grep "Loaded Configuration File"
- phpini=$(php -i | grep "Loaded Configuration File")
- phpinilocation=$(echo "$phpini" | cut -d' ' -f5)    
- "echo extension=[實際安裝的 extension 路徑] >> $phpinilocation"    
```

參考文件：
CodeBuild PHP Extension:<br />
https://forums.aws.amazon.com/thread.jspa?messageID=944697<br />
CodeBuild not installing PHP Package<br />
https://stackoverflow.com/questions/55974458/codebuild-not-installing-php-packages<br />