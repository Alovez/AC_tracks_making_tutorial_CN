# 准备

1. 软件

    Assetto Corse
    
    Assetto Corse SDK
    
    3D 建模软件（Blender）

    图片编辑软件（Procreate/PS）

2. 素材

    一张赛道的俯视图（带测量数据）

3. 参考

        https://assettocorsamods.net/threads/build-your-first-track-basic-guide.12/

# 0. 流程梳理

1. 编辑配置信息
    * 地图基本信息
    * 表面（surface）配置信息
1. 制作模型
    * 参考物的选取
    * 建模
    * 贴图和纹理
2. 导出模型
    * 坐标轴的校准
    * 单位的校准
3. 导入AC SDK并编辑材质
    * 导入并预览FBX文件
    * 编辑材质
4. 导出地图文件
    * 保存配置
    * 导出地图
5. 试玩



# 1.  编辑配置信息


**在这一部分我们会介绍赛道的基本文件结构以及新建一个赛道。**

首先我们需要找到一个现存的赛道作为例子

在steam中右键 `AC -> 管理 -> 浏览本地文件`

打开 `content -> tracks`

这个目录中的子目录就是AC的赛道信息了，随便打开一个

这里我们介绍一下赛道信息的目录结构

`ai/` 里面是给游戏的ai设定的最佳路线，不是必要的，有机会可以单独出教程，这里略过

`data/` 这个目录是赛道的配置信息，是存在 *.ini 文件里的。 其中必须有的是 `surface.ini`, 这个文件定义了游戏引擎分配物理特性的设置，不同的物理材质分别写在用空行分隔的代码段中：

        [SURFACE_0]
        KEY=TARMAC-DRIFT // 物理材质名, 这个名字会影响建模时不同组件的名字
        FRICTION=1
        DAMPING=0
        WAV=       
        WAV_PITCH=0
        FF_EFFECT=NULL
        DIRT_ADDITIVE=0
        IS_VALID_TRACK=1
        BLACK_FLAG_TIME=0
        SIN_HEIGHT=0
        SIN_LENGTH=0
        IS_PITLANE=0
        VIBRATION_GAIN=0
        VIBRATION_LENGTH=0

各个参数的影响可以自行调整尝试，这里一定不能搞错的是 `KEY`。

在本教程中，我们只使用官方预定义的几个材质，在游戏根目录 `system -> data -> surfaces.ini`
中有 `ROAD`, `GRASS`, `KERB`, `SAND` 四种材质的定义， 分别对应了 正常行驶的柏油路，赛道外的草地，路肩， 赛道外的沙地

`skin/` 可以跳过

`ui/` 里面包含了赛道的缩略图和背景图还有一个包含了赛道名字，标签，地理信息等的json文件

`*.kn5` 是地图的模型文件

`map.png` 比赛UI中显示的小地图，可以先忽略

`models.ini` 模型的配置文件，可以先忽略


回到赛道目录，复制其中一个赛道的目录，起一个新的名字，保留文件结构，其他的都删掉


打开 `ui` 目录， 编辑其中的 `ui_track.json` 文件：
我新建这个赛道是以之前去的一个卡丁车场为原型的，这里我就按照这个赛道的基本信息填写了

    {
        "name": "SD Karting Raceway",
        "description": "Beijing SD Karting Raceway",
        "tags" : [""test track","China", "Beijing", "Karting"],
        "geotags": ["lat", "lon"],
        "country": "China",
        "city": "Beijing",
        "length": "500",
        "width": "15",
        "pitboxes": "10",
        "run": "SDKARTING"
    }

从网上下载一个赛道的照片，裁剪到355*200的大小，作为预览图 `preview.png`

按照赛道的轮廓画一个赛道的地图缩略图， 保存为 `outline.png`

会到新地图的根目录，进入 `data` 目录，只保留 `surface.ini` 其他的都删掉

编辑 `surface.ini` 将 `system -> data -> surfaces.ini` 的内容复制到 `surface.ini` 中

到此为止，前期的准备和新建赛道的工作就完成了。


# 2. 制作模型

在这一部分我们会根据参考图完成赛道的建模

本次教程中的赛道参考了 北京胜道博岳国际卡丁车场（化工路店）

首先第一步是获取赛道的参考图，以及一些基本的尺寸参数。

这里我用了Google Earth找到了该赛道的俯拍图，并且带着测距数据直接截屏作为参考了。

![测距数据直接截屏](./_Doc_pic/screenshots.jpg)


如果有条件也可以去获取赛道的详细参数，或者实地测量。本次教程的精度不高，这样的卫星地图就足够了。

对截图进行裁剪，使长边与地图上的测量线重合，方便我们在Blender中设置参考图的大小。

打开blender

![打开blender](./_Doc_pic/open_bender.jpg)

新建项目，清空项目

切换到顶视图

新建参考图

![新建参考图](./_Doc_pic/new_reference.png)

选择刚刚修剪好的图片

打开图片选项，将图片的大小调整到地图测量得到的值

![大小调整](./_Doc_pic/adjust_reference.png)

深度选择前置，透明度打开并且设置在0.3左右

![前置](./_Doc_pic/top_reference.png)

将游标设定在起点附近， 新建一个平面，进入编辑模式

![游标](./_Doc_pic/move_cursor.png)

![新建一个平面](./_Doc_pic/new_plane.png)

添加新的边，慢慢覆盖整条赛道

铺赛道的时候注意最好把路肩的部分也覆盖到

![铺赛道](./_Doc_pic/set_road.png)

将pits的区域也用面覆盖到

![pits](./_Doc_pic/include_pits.png)

对赛道部分进行环切，细分到可以覆盖路肩的宽度就可以了

![环切](./_Doc_pic/loop_cut.png)

用小刀工具对路肩附件的面进行切割

![小刀工具](./_Doc_pic/knife_cut.png)

将赛道，路肩，维修区分离成单独的对象

![分离](./_Doc_pic/separate.png)

这样赛道的模型就完成了

