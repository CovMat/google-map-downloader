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
国内的地图服务均进行过GCJ02坐标系加密，因此这个工具对高德地图、高德卫星图与谷歌地图进行了GCJ02到WGS84坐标系的变换。谷歌卫星图则不需要进行变换。
此外，境内用户需确保能够正常使用谷歌地图网页版才能使用本工具下载谷歌地图。

提供如下参数运行脚本，即可下载 tif 格式地图:
```python
python downloader_gmtchina.py 起始经度 结束经度 起始纬度 结束纬度 地图层级 输出图片文件名 地图来源
```
地图层级取 0 - 22 之间的整数，值越高分辨率越大。地图来源目前可以选择以下四个:

=========== =========
代码         地图来源
=========== =========
amap        高德地图
amap_sat    高德卫星图
google      谷歌地图
google_sat  谷歌卫星图
=========== =========

以厦门市为例，使用如下参数下载四种地图:

```python
python downloader_gmtchina.py 118.055917 118.244753 24.399450 24.559724 12 google.tif google
python downloader_gmtchina.py 118.055917 118.244753 24.399450 24.559724 16 google_sat.tif google_sat
python downloader_gmtchina.py 118.055917 118.244753 24.399450 24.559724 12 amap.tif amap
python downloader_gmtchina.py 118.055917 118.244753 24.399450 24.559724 16 amap_sat.tif amap_sat
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
