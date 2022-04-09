# 筆記型電腦 調整

## 筆記型電腦插入滑鼠裝置自動關閉滑鼠板 Gnome

```shell
gsettings set org.gnome.desktop.peripherals.touchpad send-events disabled-on-external-mouse
```

## 電源管理 tlp

```bash
sudo apt install tlp tlp-rdw
sudo tlp start
sudo tlp-stat -s
sudo systemctl enable tlp
```

## 參考資料

[GNOME: How To Disable The Touchpad When A Mouse Is Plugged In And While Typing](https://www.linuxuprising.com/2021/06/gnome-how-to-disable-touchpad-when.html)