# 我的Linux生活日記 02-安裝Debian

今天讓我詳細介紹如何安裝Debian吧！老實說安裝Debian步驟真的超多的。我記得我第一次接觸Debian 10 安裝時，有點不知所措。因為步驟真的比Ubuntu 、CentOS 多好多，因此我在這邊簡單帶一下安裝步驟。

## Debian 簡介: 自由(free) 的信仰

說到Debian 這個發行版本，一定得提到 `free` 、 `none-free` 。首先這邊需要很嚴肅的跟大家說 `free` 這一詞是 `自由` 的意思，意指該軟體可以被世界上任何一個人使用、複製甚至販售，而不是 `免費` 的意思。`none-free` 就是不符合上述 free 的規範啦。所以從官方網站下載的一定是 free 的版本。

這個觀念很重要，如過是硬體有 intel 無線網卡，需要特別需要去找 [iwlwifi](https://wiki.debian.org/iwlwifi) 在安裝前準備。第一次安裝我舊碰到這個問題，可花費我兩三天的時間去處理。

另外我想介紹 Debian 的原因是: ~~他的安裝步驟很多~~ 它是我接觸過作業系統中，有多都是以Debian 為基底去做改良，例如：ubuntu、Proxmox VE、OpenSwitch OPX、kali。所以學習使用Debian 投資報酬率很高。(我覺的啦)

## 下載 Debain

* 官方網址: https://www.debian.org/

![https://ithelp.ithome.com.tw/upload/images/20220915/20128528sAaGVQRtVG.png](https://ithelp.ithome.com.tw/upload/images/20220915/20128528sAaGVQRtVG.png)

一進入官方網站，你會發現這個官網真陽春，真的好像是我會寫出的網站排版：HTML 加 CSS 簡單稍微排版。然而在首頁的右下方就出現一個大大個下載，提供你下載。

* : https://cdimage.debian.org/mirror/debian/dists/

如果想要下載之前版本的版本可以從這個網站下載，當然也可以下載Debain 衍生的發行板，如：Ubuntu、kali。

## 下載及準備驅動軟體

Debian 的官網入口有提供小型安裝檔，只有300多MB 而已，不過安裝過程當中建議連接網路，如果有安裝面環境需求時會需要從網路上下載套件。如果安裝的環境中，網路環境相當堪憂，我這邊建議去準備8GB 以上的隨身碟，並且到網路穩定的環境下載自己要的版本DVD/USB 版本。

我兩台電腦安裝的經驗當中，筆電我是下載DVD/USB 版本，選擇的當案是「[debian-live-10.9.0-amd64-gnome.iso](https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-10.9.0-amd64-gnome.iso)」，因為當時安裝的網路環境只有300kbps/s，相當慢。而自己組裝桌機時，是使用小型安裝檔。兩者安裝的流程其實是一樣，只差DVD/USB 版本只是預先將桌面環境下載下來而已。

關於驅動軟體，對於 Linux 不熟悉的人來說都有聽過一個傳聞，就是安裝Linux 需要自己另外去硬體廠商網站下載Linux 版本的驅動軟體。其實這個傳聞的聽說，我也是說聽說啦！是聽說以前的確需要特別去找，但是已我的經驗當中，絕大部分的驅動軟體都可以不用自行去下載，如果你是用Ubuntu自然更不會遇到這個問題。

但是當我安裝Debian 時卻碰到 筆電的無線網卡無法辨認的問題，後來我上網查筆電的規格後，發現無線網卡是Intel ，加上Debian 有將軟體類型區分 main、contrib、non-free、non-us，所以我必須特去找Intel的驅動軟體[iwlwifi](https://wiki.debian.org/iwlwifi)

我這邊簡單說明為什麼會有這樣的區分：主要是Debian 希望在這個平台上所有的軟體都是高純度的自由軟體，可以被自由使用的，但是現實當中一個軟體的誕生並非是開發者/團體絕對的無私的為軟體世界開發。說白一點，工程師也要吃飯，工程師後面也有老闆要賺錢向投資者交待，因此才會區分這些類別。

* main: Debian 最主要的系統組成成份。
* contrib: 本身是自由軟體，但是與其他非自由軟體有相依性。
* non-free: 非自由軟體。
* non-us: 這類大多是專利相關軟體。

關於安裝的細節步驟，我就不多做敘述。因為很多人寫過，而且在針對文書處理機安裝的過程中真的只是下一步，頂多打個帳號密碼，選擇想要的桌面環境就結束了。除非是真的要安裝伺服器主機才會去特別調整硬碟資雲分配......等等。

安裝詳細的步驟官方的最詳細，不然用關鍵字「安裝Debian」就會出現很多相關的處理方式，例如：[圖解 Debian 10（Buster）安裝步驟](https://kknews.cc/zh-tw/code/x65jrpo.html)

## 安裝 Debian

1. 安裝菜單

選擇第一個 `Graphical install` 圖形安裝，有提供滑鼠安裝，也比較平易近人。第二個項目 `Install` 你會看到Dos 年代的畫面風格，只能透過鍵盤安裝，不過很容易就上手的。

![https://ithelp.ithome.com.tw/upload/images/20220914/201285285h2wrJHDa2.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285285h2wrJHDa2.png)

2. 選擇語系

這邊我習慣使用英文語系，~~因為外國的月亮比較圓~~ 主要是未來發現不懂的操作，英文資源比較好找，如果是中文語系，需要先翻譯成英文，但是我英文能力普普啊! 為了能夠打中文，安裝完成後必須再安裝中文語系。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528dt9XsO2zWs.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528dt9XsO2zWs.png)

3. 選擇所在地區
   
生在台灣就選 Taiwan 呀！

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528zuCcKdcZZ4.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528zuCcKdcZZ4.png)

4. 選擇所在地區：其他

但是台灣在哪呢？怎摸跟世界地圖一樣難找！先選擇 `Other` 。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528VvaeMLkpAh.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528VvaeMLkpAh.png)

5. 選擇所在地區：亞洲

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528bRlfq3Uiye.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528bRlfq3Uiye.png)

6. 選擇所在地區：台灣

花費了三個步驟終於找到啦！但是在 Redhat 體系 或是 ubuntu 的安裝檔點點地圖就設定完成了。也許地圖選擇是有版權的吧(我猜)。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528qvnV53zzu7.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528qvnV53zzu7.png)

7. 選擇語系：en_US.UTF-8

記得要選擇 UTF8 唷！不然生在台灣會有很多 罕見字 (難字) 在未來會看不到。雖然 UTF8 還是會有中文字不到，例如：新北市坪林區大林里「?」(魚逮 ㄉㄞˋ)魚堀。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528dNDMYH6GrY.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528dNDMYH6GrY.png)

8. 選擇鍵盤配置

這邊選擇美式鍵盤

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528oInLbdA9MQ.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528oInLbdA9MQ.png)

9. 進行讀取安裝光碟

![https://ithelp.ithome.com.tw/upload/images/20220914/201285282pFoesgOYm.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285282pFoesgOYm.png)

10. 選擇網路

這邊就選擇可以上網的網路接孔吧。不用怕選錯，因為連不上網路還會跳回來這一步。

![https://ithelp.ithome.com.tw/upload/images/20220914/201285287LowFS6NG2.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285287LowFS6NG2.png)

11. 進行網路配置

系統會自動探索網際網路。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528d6Rkl8UTNW.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528d6Rkl8UTNW.png)

12. 輸入主機名稱

探索到網路世界後，接下來就是跟大家說我是誰。~~不然大家就會問你的名子~~

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528B4dAfbP8TK.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528B4dAfbP8TK.png)

13. 輸入網站

在這邊如果未來規劃是網站伺服器或是郵件伺服器，如：zimbra，可以在這個步驟填寫自己的Domain。不填沒有關係，安裝完成之後改 `/etc/hosts` 文件即可。如果是作為桌面使用，可以按下一步。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528UE8UvWn7p2.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528UE8UvWn7p2.png)

14. 輸入root密碼

強烈建議不填 root 密碼，讓 root 不允許登入。 ~~這樣資安稽核就可以安心通過一項了，或是導入GCB就少一項設定。~~

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528psVgKjdxSd.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528psVgKjdxSd.png)

15. 輸入使用者名稱

輸入你的大名。例如：`POLLOCHANG`。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528d0Fl85Wrcb.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528d0Fl85Wrcb.png)

16. 輸入使用者帳號

輸入你的小名。例如： `pollochang` 。~~不就大小寫差別？~~

對於linux 帳號命名這邊建議用`英文小寫`加`阿拉伯數字`就可以，主要是未來有安裝老舊的軟體有應用到linux帳號，大寫英文可能會作為該軟體的帳號，例如：[DB2](https://www.ibm.com/docs/zh-tw/db2/9.7?topic=requirements-db2-users-groups)。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528K2fblT8m0x.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528K2fblT8m0x.png)

17. 輸入使用者密碼

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528u9Xp6GNLTY.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528u9Xp6GNLTY.png)

18. 規劃硬碟

選擇第一項: `Guided- use entire disk`，如過是安裝在虛擬機上，可以選擇 `Guided- use entire disk and set up LVM` 未來空充硬碟會比較方便。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528vayFEQeogr.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528vayFEQeogr.png)

19. 選擇硬碟

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528PvxpQ26WNn.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528PvxpQ26WNn.png)

20. 選擇硬碟分割模式

這邊選擇第一項，讓電腦自動配置。~~這個年頭電腦除了會挑選花生，也會切硬碟唷！~~

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528j1yUgA5iqH.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528j1yUgA5iqH.png)

21. 規劃分割硬碟

電腦切割的結果，如果不喜歡也可以自己切。自己切記得 `swap` 要切出來，至少512MB ，不然之後可能會遇到很奇怪的問題。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528HAnGie0ATV.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528HAnGie0ATV.png)

22. 再次確認硬碟分割規劃

沒問題就按下 yes 。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528ZDCs4urgd4.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528ZDCs4urgd4.png)

23. 執行硬碟分割

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528cDjbVtkbOj.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528cDjbVtkbOj.png)

24. 詢問有其他安裝光碟

![https://ithelp.ithome.com.tw/upload/images/20220914/201285285djmWWJR5A.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285285djmWWJR5A.png)

25. 選擇 mirror 地理位置

根據你地理上最近的地區選擇吧。~~但是我地理不好啊！跟索隆一樣~~

![https://ithelp.ithome.com.tw/upload/images/20220914/201285287PLWFjVtLQ.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285287PLWFjVtLQ.png)

26. 選擇 mirror 網站

隨便選，開心就好。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528dpQJpQcViU.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528dpQJpQcViU.png)

27. 設定 Proxy

主機所在的網路環境沒有特殊要求就不用設。

![https://ithelp.ithome.com.tw/upload/images/20220914/201285281ECd7dIv6P.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285281ECd7dIv6P.png)

28. 設定 apt

![https://ithelp.ithome.com.tw/upload/images/20220914/201285287GNe8Ttua0.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285287GNe8Ttua0.png)

29. 安裝系統

![https://ithelp.ithome.com.tw/upload/images/20220914/201285288BazLqHuFt.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285288BazLqHuFt.png)

30. 詢問將安裝過程分享給Debian社群

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528MLMJnauKZC.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528MLMJnauKZC.png)

31. 詢問安裝桌面

一般我是習慣選擇 `Gnome`，如果要遠端桌面使用的話建議就 ~~不要裝！大家都透過SSH直接遠端~~ 選擇 `xfce`。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528C7nlAjbY90.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528C7nlAjbY90.png)

32. 安裝 boot loader

這步驟很重要，請選擇 yes!有一次我選擇 NO ，安裝完之後就傻傻的從頭安裝。

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528wDI5A24geu.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528wDI5A24geu.png)

33. 選擇磁區

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528dUqTR2GQlN.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528dUqTR2GQlN.png)

34. 安裝 boot loader 畫面

![https://ithelp.ithome.com.tw/upload/images/20220914/20128528xWeeyhqoyB.png](https://ithelp.ithome.com.tw/upload/images/20220914/20128528xWeeyhqoyB.png)

35. 安裝完畢畫面

經過最短路徑 35 個步驟終於安裝完啦！不得不說安裝 Debian 不看前人或官方的安裝指引，真的有種在走迷宮的感覺。

![https://ithelp.ithome.com.tw/upload/images/20220914/201285283nw5BPpQdI.png](https://ithelp.ithome.com.tw/upload/images/20220914/201285283nw5BPpQdI.png)

## 安裝問題處理

### Intel 無線網卡驅動程式

這是我安裝Debian 第一個遇到的問題，處理方式是將對別人處理好的[deb](http://ftp.br.debian.org/debian/pool/non-free/f/firmware-nonfree/firmware-iwlwifi_20210315-2_all.deb)檔，下載後放到令一個空白隨身碟中，記得需要把deb當放在隨身碟第一層目錄，不然會安裝驅動失敗。

在安裝的過程當中系統會提示你，有找到不認識的硬體設備，可以透過準備好的安裝檔案裝。這時你就可以把剛剛準備好的隨身碟依照指示安裝到電腦當中。

### nVidia 安裝驅動

這個問題是發生在我組裝電腦的上面。

安裝完電腦就可已正常啟動並且運作，但是常常運作到一半系統畫面就無緣無故當機。

通常電腦當機的原因有

1. 硬體問題
2. 驅動程式問題
3. 軟體問題

我的檢測方式是，當發生當機時，看看Linux 是不是可以透過其他電腦ping 檢測可以接通，如果可以代表CPU、RAM等等電腦計算功能都是正常運作，剩下就是螢幕上的顯示功能。

在我安裝組裝電腦時，沒有特別去安裝硬體官方網站上所推出的驅動程式，想說就都用公用的驅動應該沒有問題吧！反正我頂多就只是個高級文書人員而已。

後來我去下載險卡的驅動程式後，用到一半系統畫面突然死當的狀況就被解決了。

## 參考資料

[在哪裡可以找到 non-free （非自由的）光碟映像檔？](https://www.debian.org/CD/faq/#firmware)

[黑暗執行緒—Debian 與 Non-Free Firmware](https://blog.darkthread.net/blog/debian-non-free-firmware/)

[DB2 使用者及群組 (Linux 及 UNIX)](https://www.ibm.com/docs/zh-tw/db2/9.7?topic=requirements-db2-users-groups)

[OpenSwitch OPX system overview](https://github.com/open-switch/opx-docs/wiki/OpenSwitch-OPX-system-overview)

[5.3 以垂直的觀點來看 Debian 軟體的分佈：main、contrib、non-free、non-us ](https://docs.huihoo.com/gnu_linux/debian/tutorial/Debian-Install-Guide-5.html)

[Intel Wireless WiFi Link, Wireless-N, Advanced-N, Ultimate-N devices](https://wiki.debian.org/iwlwifi)

[取得 Debian](https://www.debian.org/distrib/index.zh-tw.html)

[Debian GNU/Linux 安裝手冊](https://www.debian.org/releases/sarge/i386/index.html.zh_TW)

[圖解 Debian 10（Buster）安裝步驟](https://kknews.cc/zh-tw/code/x65jrpo.html)