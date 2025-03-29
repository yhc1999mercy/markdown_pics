# Specfem3D 正演


**日期**：2025.3.29  
**处理者**：应恒成  
**示例文件夹**：/home/work/scoped/work/truedata

---

## 1. 建立三维模型
### 1.1 获取地形数据
- 从GMT下载地形数据：在终端中执行下面代码以获得所在区域的地形数据 **earth_relief_01m.grd**;
```bash
gmt grdcut earth_relief_01m.grd -Gregion.nc -R84/89/26/30
```

- 利用gmt的**grd2xyz**命令将grd格式数据转化为txt;
```bash
gmt grd2xyz region.nc > region.txt
```

- 执行python脚本**step1_get_topo.py**读取region.txt中的高程数据;需要修改**step1_get_topo.py**中第一行的输入输出;

- 执行python脚本**step1_plot_topo.py**将region.txt中的高程数据可视化为地形图。

![fig 1](https://github.com/yhc1999mercy/markdown_pics/blob/main/images/fig1.png?raw=true)
<div style="text-align: center;"><i>fig1：region.txt</i></div>

![fig 2](https://github.com/yhc1999mercy/markdown_pics/blob/main/images/fig2.png?raw=true)
<div style="text-align: center;"><i>fig2：pure_topo.txt</i></div>

![fig 3](https://github.com/yhc1999mercy/markdown_pics/blob/main/images/topo_plot.png?raw=true)
<div style="text-align: center;"><i>fig3：topo_plot.png</i></div>

### 1.2 获取速度模型（以三维模型为例）
- 以青藏高原南部作为研究区域为例，纬度26.2-29.8°，经度84.0-88.8°,深度80km。执行python脚本**step2_extract_lat_lon.py**从CSRM1.0模型中提取研究区对应的三维地震波速度模型，获得extracted_data.txt。
- 使用excel编辑extracted_data.txt，将其转化为specfem3d可读的格式，例如在文件的头四行加上头段信息；将速度转为m/s；添加密度信息等。

![fig 4](https://github.com/yhc1999mercy/markdown_pics/blob/main/images/fig4.png?raw=true)
<div style="text-align: center;"><i>fig4：extracted_data.txt</i></div>

![fig 5](https://github.com/yhc1999mercy/markdown_pics/blob/main/images/fig5.png?raw=true)
<div style="text-align: center;"><i>fig5：tomography_model.xyz格式</i></div>

![fig 6](https://github.com/yhc1999mercy/markdown_pics/blob/main/images/fig6.png?raw=true)
<div style="text-align: center;"><i>fig6：tomography_model.xyz</i></div>
---

## 2. 数据来源
### 2.1 原始数据
| 数据源名称 | 获取方式 | 数据量 | 时间范围 | 字段说明 |
|------------|----------|--------|----------|----------|
|            |          |        |          |          |

### 2.2 数据快照
```python
# 数据加载示例代码
import pandas as pd
df = pd.read_csv('dataset.csv')
print(df.info())
```