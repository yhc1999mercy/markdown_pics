# Specfem3D 正演


**日期**：2025.3.29  
**处理者**：应恒成  
**示例文件夹**：/home/work/scoped/work/xmesh_tomo

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

- 执行python脚本**step1_get_topo.py**读取region.txt中的高程数据;需要修改**step1_get_topo.py**中第一行的输入输出;得到的文件就是在建立mesh时用到的interface数据；

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

### 1.3 构建mesh
- 修改./DATA/meshfem3D_files目录下的**Mesh_Par_file**
>**需要留意的参数**
>>1、是否压制UTM投影
>>```
>>UTM_PROJECTION_ZONE             = 1
>>SUPPRESS_UTM_PROJECTION         = .true.
>>```
>>2、是否使用Doubling layers
>>```
>># Regular/irregular mesh
>>USE_REGULAR_MESH                = .true.
>># Only for irregular meshes, number of doubling layers and their position
>>NDOUBLINGS                      = 1
>># NZ_DOUBLING_1 is the parameter to set up if there is only one doubling layer
>># (more doubling entries can be added if needed to match NDOUBLINGS value)
>>NZ_DOUBLING_1                   = 10
>>NZ_DOUBLING_2                   = 0
>>```
>>3、使用tomography定义materials的写法（分两种）
>>>3.1 每个深度数据点数量相同/整个模型使用一个tomography **本文档以此为例**
>>>```
>>># number of materials
>>>NMATERIALS                      = 1
>>># define the different materials in the model as:
>>># #material_id  #rho  #vp  #vs  #Q_Kappa  #Q_mu  #anisotropy_flag  #domain_id
>>>#     Q_Kappa          : Q_Kappa attenuation quality factor
>>>#     Q_mu             : Q_mu attenuation quality factor
>>>#     anisotropy_flag  : 0 = no anisotropy / 1,2,... check the implementation in file aniso_model.f90
>>>#     domain_id        : 1 = acoustic / 2 = elastic / 3 = poroelastic
>>>
>>>-1 tomography elastic tomography_model.xyz 0 2
>>>```
>>>3.2 不同深度范围的数据点数量不同/整个模型使用多个tomography
>>>```
>>># number of materials
>>>NMATERIALS                      = 2
>>># define the different materials in the model as:
>>># #material_id  #rho  #vp  #vs  #Q_Kappa  #Q_mu  #anisotropy_flag  #domain_id
>>>#     Q_Kappa          : Q_Kappa attenuation quality factor
>>>#     Q_mu             : Q_mu attenuation quality factor
>>>#     anisotropy_flag  : 0 = no anisotropy / 1,2,... check the implementation in file aniso_model.f90
>>>#     domain_id        : 1 = acoustic / 2 = elastic / 3 = poroelastic
>>>
>>>-1 tomography elastic tomography_model_1.xyz 0 2
>>>-2 tomography elastic tomography_model_2.xyz 0 2
>>>```
- 修改./DATA/meshfem3D_files目录下的**interfaces.dat**
- 配置完成后运行xmeshfem构建mesh
```bash
mpirun -np 16 ./bin/xmeshfem
```




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