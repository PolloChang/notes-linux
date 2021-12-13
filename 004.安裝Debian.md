# 我的linux日常生活-4. 安裝Debian

###### tags: `我的linux日常生活`

我承認我略過電腦組裝的過程，因為這不是這系列的主題。

接下來我會這次安裝 Debian 的關鍵介紹給大家。


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

[5.3 以垂直的觀點來看 Debian 軟體的分佈：main、contrib、non-free、non-us ](https://docs.huihoo.com/gnu_linux/debian/tutorial/Debian-Install-Guide-5.html)

[Intel Wireless WiFi Link, Wireless-N, Advanced-N, Ultimate-N devices](https://wiki.debian.org/iwlwifi)

[取得 Debian](https://www.debian.org/distrib/index.zh-tw.html)

[Debian GNU/Linux 安裝手冊](https://www.debian.org/releases/sarge/i386/index.html.zh_TW)

[圖解 Debian 10（Buster）安裝步驟](https://kknews.cc/zh-tw/code/x65jrpo.html)