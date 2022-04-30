# build Remmina

## 安裝編譯套件

```bash
sudo apt install build-essential git-core libssh-dev cmake libx11-dev libxext-dev libxinerama-dev  libxcursor-dev libxdamage-dev libxv-dev libxkbfile-dev libasound2-dev libcups2-dev libxml2 libxml2-dev libxrandr-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libxi-dev libavutil-dev libjson-glib-dev libavcodec-dev libxtst-dev libgtk-3-dev libgcrypt20-dev  libpulse-dev  libvte-2.91-dev libxkbfile-dev libtelepathy-glib-dev libjpeg-dev libgnutls28-dev libsecret-1-dev libavahi-ui-gtk3-dev libvncserver-dev libappindicator3-dev intltool libsecret-1-dev libwebkit2gtk-4.0-dev libsystemd-dev libsodium-dev libkf5wallet-dev libusb-1.0-0-dev libpcre2-dev python3-dev
```

As root, remove freerdp-x11 package and all packages containing the string remmina in the package name.

```bash
sudo apt purge "remmina*" "libfreerdp*" "libwinpr*" "freerdp*"
```

## freerdp 編譯安裝

### Create a new directory for development in your home directory, and cd into it

```bash
sudo mkdir /opt/remmina_devel
cd /opt/remmina_devel
```

### Download the latest source code of FreeRDP from its master branch

```bash
git clone --branch 2.0.0 https://github.com/FreeRDP/FreeRDP.git
cd FreeRDP
```

### Configure FreeRDP for compilation (don't forget to include -DWITH_PULSE=ON)

```bash
cmake -DCMAKE_BUILD_TYPE=Debug -DWITH_SSE2=ON -DWITH_CUPS=on -DWITH_PULSE=on -DCMAKE_INSTALL_PREFIX:PATH=/opt/remmina_devel/freerdp .
```

### Compile FreeRDP and install

```bash
make && sudo make install
```

### Make your system dynamic loader aware of the new libraries you installed.

```bash
echo /opt/remmina_devel/freerdp/lib | sudo tee /etc/ld.so.conf.d/freerdp_devel.conf > /dev/null
sudo ldconfig
```

### Create a symbolik link to the executable in /usr/local/bin

```bash
sudo ln -s /opt/remmina_devel/freerdp/bin/xfreerdp /usr/local/bin/
```

## remmina 編譯安裝

### Now clone remmina repository to your devel dir:

```bash
cd /opt/remmina_devel
git clone https://gitlab.com/Remmina/Remmina.git
```

### Configure Remmina for compilation

```bash
cd Remmina
cmake -DCMAKE_BUILD_TYPE=Debug -DWITH_KF5WALLET=on -DCMAKE_INSTALL_PREFIX:PATH=/opt/remmina_devel/remmina -DCMAKE_PREFIX_PATH=/opt/remmina_devel/freerdp --build=build .
```

### Compile remmina and install it

```bash
make && sudo make install
```

### Create a symbolik link to the the executable

```bash
sudo ln -s /opt/remmina_devel/remmina/bin/remmina /usr/local/bin/
```

### Run remmina

```bash
remmina
```

### 建立桌面應用程式清單

```bash
sudo tee /usr/share/applications/remmina.desktop<<EOF
[Desktop Entry]
Version=1.0
Type=Application
Name=remmina
Icon=/opt/remmina_devel/remmina/share/icons/hicolor/scalable/apps/org.remmina.Remmina.svg
Exec="/opt/remmina_devel/remmina/bin/remmina"
Comment=smartgit
Categories=Development;     
Terminal=false
EOF
```

## 參考資料

[Cmake is not able to find Python-libraries](https://stackoverflow.com/questions/24174394/cmake-is-not-able-to-find-python-libraries)