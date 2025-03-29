# Specfem3D 正演


**日期**：2025.3.9  
**处理者**：应恒成  

---

## 1. 建立三维模型
### 1.1 获取地形数据
- 从GMT下载地形数据，**earth_relief_01m.grd**，在终端中执行下面代码以获得所在区域的地形数据;
```bash
gmt grdcut earth_relief_01m.grd -Gregion.nc -R84/89/26/30
```

- 利用gmt将grd格式数据转化为txt;
```bash
gmt grd2xyz region.nc > region.txt
```

- 执行python脚本**step1_get_topo.py**读取region.txt中的高程数据;需要修改**step1_get_topo.py**中第一行的输入输出;


![fig 1](https://github.com/yhc1999mercy/markdown_pics/blob/main/fig1.png?raw=true)


![fig 2](https://github.com/yhc1999mercy/markdown_pics/blob/main/fig2.png?raw=true)
### 1.2 获取速度模型（以一维模型为例）
- 

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