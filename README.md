# 谷歌与高德地图下载器

## 中文：  
[downloader_1.1](https://github.com/zhengjie9510/Google-Map-Downloader/blob/master/downloader_1.1.py)：  
    一个小工具，你只需要输入空间范围、地图缩放等级就可以实现Google地图的下载，并输出为TIFF格式，含空间坐标系。  
[downloader_1.2](https://github.com/zhengjie9510/Google-Map-Downloader/blob/master/downloader_1.2.py)：  
    是在1.1版本上的改进。由于python的多线程中存在GIL锁，导致python的多线程不能利用多核，考虑到现在的计算机是多核的，为了充分利用计算机的多核资源，提高下载速度，尝试利用多进程+多线程的方式来实现地图切片下载，最终速度得到极大提高。但该部分还没有实现进度条功能。  
[downloader_gmtchina.py](https://github.com/CovMat/google-map-downloader/blob/master/downloader_gmtchina.py)：  
    添加了高德地图下载功能 

## 指南/Guide
### 安装/Install
```python
conda install -c anaconda numpy pillow py-opencv
conda install -c conda-forge gdal 
```
### 使用/Use
```python
python downloader_gmtchina.py 起始经度 结束经度 起始纬度 结束纬度 地图层级 输出图片文件名 地图来源
```

## 问题/Issues
If you encounter the problem of Bad network link, you can change the HEADERS in the download function, and try again.
```python
def download(self,url):
        HEADERS = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.76 Safari/537.36'}
        header = ur.Request(url,headers=HEADERS)
        err=0
        while(err<3):
            try:
                data = ur.urlopen(header).read()
            except:
                err+=1
            else:
                return data
        raise Exception("Bad network link.")
```
