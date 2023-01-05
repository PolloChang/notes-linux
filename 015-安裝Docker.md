# 我的linux日常生活-15.安裝Docker

###### tags: `我的linux日常生活`

OS: Debian 10 Debian 11

## 初次安裝先移除舊相關套件

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

## 安裝

### 加入 gpg

```shell
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 安裝docker 引擎

```shell
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### 將使用者加入docker群組

```shell
sudo usermod -aG docker [userName]
```

### 安裝docker-compose

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
sudo ln -s /usr/bin/docker-compose /usr/local/bin/docker-compose
```

### 開機時自動啟動docker

```shell=
sudo systemctl enable docker
```

## 參考資料

[Install Docker Engine on Debian](https://docs.docker.com/engine/install/debian/)