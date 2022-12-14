## 1、下载vim最新版本

[[download : vim online](https://www.vim.org/download.php)]:

```
git clone https://github.com/vim/vim.git
```

```shell
cd src
make -j12
sudo make install
cd
sudo mv vim/ usr/local/lib
```

## 2、nodejs

[[下载 | Node.js (nodejs.org)](https://nodejs.org/zh-cn/download/)]:

```shell
 sudo mkdir -p /usr/local/lib/nodejs
 sudo tar -xJvf node-xxxx.tar.xz -C /usr/local/lib/nodejs
 sudo vim ~/.profile #添加export PATH=/usr/local/lib/nodejs/bin:$PATH
 
 source .profile
 
 sudo ln -s /usr/local/lib/nodejs/bin/node /usr/bin/node
 sudo ln -s /usr/local/lib/nodejs/bin/npm /usr/bin/npm
 sudo ln -s /usr/local/lib/nodejs/bin/npx /usr/bin/npx
```

查看是否成功安装

`$ node -v`

`$ npm version`

`$ npx -v`



## 3、clangd

下载源码解压到 /usr/local/lib中

添加环境变量





## 4、coc.nvim

`sudo npm i -g yarn`

```c
"安装coc.nvim
Plugin 'neoclide/coc.nvim'
```

如果安装失败: `yarn install`  



安装coc.nvim成功后

```c
#vim
:CocInstall coc-clangd  
```



## 5.clash

sudo vim /etc/systemd/system/clash.service

```shell
[Unit]
Description=Clash service
After=network.target

[Service]
Type=simple
User=lipei
ExecStart=/home/lipei/Clash/clash
Restart=on-failure
RestartPreventExitStatus=23

[Install]
WantedBy=multi-user.target
```



```shell
# 重载服务
sudo systemctl daemon-reload
# 开机启动
sudo systemctl enable clash
# 启动服务
sudo systemctl start clash
# 查看服务状态
sudo systemctl status clash
```

