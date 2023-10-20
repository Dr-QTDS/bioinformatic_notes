[在Linux服务器中安装网页版Rstudio | 了尘兰若的小坑 (liaochenlanruo.fun)](https://liaochenlanruo.fun/post/ea71.html#%E8%BF%9C%E7%A8%8B%E7%99%BB%E5%BD%95rstudio)

[ r语言 服务器网页版ide RStudio Server 简介_whatday的博客-CSDN博客_r语言网页版](https://blog.csdn.net/whatday/article/details/114516277)

# 安装R语言

以Ubuntu为例

[The Comprehensive R Archive Network (tsinghua.edu.cn)](https://mirrors.tuna.tsinghua.edu.cn/CRAN/)

Run these lines (if `root`, remove `sudo`) to tell Ubuntu about the R binaries at CRAN

``` bash
# update indices
sudo apt update -qq
# install two helper packages we need
sudo apt install --no-install-recommends software-properties-common dirmngr
# add the signing key (by Michael Rutter) for these repos
# To verify key, run gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc 
# Fingerprint: E298A3A825C0D65DFD57CBB651716619E084DAB9
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
# add the R 4.0 repo from CRAN -- adjust 'focal' to 'groovy' or 'bionic' as needed
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
```

在上面的脚本中，使用了命令 `lsb_release -cs` 来识别当前使用的是哪个版本的ubuntu

运行下端代码，以安装R语言及其依赖

```
sudo apt install --no-install-recommends r-base
```

# 安装R studio server

[RStudio Server - Posit](https://posit.co/download/rstudio-server/)

以Ubuntu 22 为例

运行下面命令

```
sudo apt-get install gdebi-core
wget https://download2.rstudio.org/server/jammy/amd64/rstudio-server-2023.06.2-561-amd64.deb
sudo gdebi rstudio-server-2023.06.2-561-amd64.deb
```

# 开放端口

R studio server 默认使用8787端口

```
sudo ufw allow from xxx.xxx.xxx.xxx/24 to any port 8787 proto tcp
```