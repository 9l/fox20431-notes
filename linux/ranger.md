# ranger

## 相关软件安装
```sh
sudo apt-get install caca-utils # img2txt 图片
sudo apt-get install highlight  # 代码高亮
sudo apt-get install ranger     # ranger 的主程序
sudo apt-get install atool　    # 存档预览
sudo apt-get install w3m        # html页面预览
sudo apt-get install poppler    # pdf预览
sudo apt-get install mediainfo  # 多媒体文件预览
sudo apt-get install catdoc     # doc预览
sudo apt-get install docx2txt   # docx预览
sudo apt-get install xlsx2csv   # xlsx预览
```

## 使用方法
略过

## 配置
```sh
ranger --copy-config=all
```
复制所有配置文件到`~/.config/ranger`

推荐配置

修改rc.conf，
```conf
# 避免文件过大，预览导致的卡顿
set preview_max_size 10240
```