###  physical cells
---
+ decap cell，去耦单元，这是一种特殊的Filler cell。
当电路中大量单元同时翻转时会导致冲放电瞬间电流增大，使得电路动态供电电压下降或地线电压升高，引起动态电压降，俗称IR-drop。
为了避免IR-drop对电路性能的影响，通常在电源和地线之间放置由MOS管构成的电容，这种电容被称为去耦电容或者去耦单元。
它的作用是在瞬态电流增大，电压下降时向电路补充电流以保持电源和地线之间的电压稳定，防止电源线的电压降和地线电压的升高。
Encounter中通过addFiller命令添加decap cell
+ Filler cell——通常是单元库中与逻辑无关的填充物，可以分为IO filler以及普通的std cell filler。
IO filler，通常是用来填充I/O 单元与I/O单元之间的空隙。
为了更好的完成power ring，也就是ESD之间的电源连接。
通常是在floorplan阶段时添加。使用addIoFiller命令。
Std cell filler，也是为了填充std cell之间的空隙。
主要是把扩散层连接起来满足DRC规则和设计需求，并形成power rails。
通常是在place之后，route之前添加，使用命令addFiller。
+ Well tap cell 一种物理单元，在floorplan stage摆放，主要防止CMOS器件的寄生闩锁效应（latch-up），有些标准单元的版图没有画阱接触孔和衬底接触孔，
而是把这两个孔做成单元，这样在做PR时可以节约面积，如果不加，那么会发生latch-up效应。
+ Endcap是一种特殊的标准单元。在后端物理设计中，除了与，非，或等一些常见的标准单元外，还有一些特殊的物理单元(physical cell)，
它们通常没有逻辑电路，不存在与netlist当中，但是对整个芯片的运行，稳定却起着举足轻重的作用。那Endcap cell就是其中一种，它俗称为拐角单元。
Endcap主要加在row的结尾（两边都要加），以及memory 或者其他block的周围包边
主要是为了避免PSE ( poly space effect）和OSE（od space effect）造成的影响，
不能让poly和OD周围太空，不对称，密度太低，因此通过加endcap来满足均匀的密度， 使得std cell 周围的环境一致。
+ Physical Net就是我们平常用到的power, ground, tie-hi，tie-lo。
这些net没有定义在netlist中，在Encounter中通过在global文件中以下两行导入：
setUserDataValue init_pwr_net {vdd_mpu}
setUserDataValue init_gnd_net {vss}
并且可以通过globalNetConnect把instance的pg pin或者1'b0/1'b1 pin连接到对应的global net上
+ spare cell：简单来说，就是每块地方洒一些类似SDFF，NAND，AND，XOR，INV等的备用cell, 为以后做function eco和metal eco用。
流片过程是先光刻base层和M1层的片子，这个是最贵的，这个需要一两个星期。这段时间，要是验证过程中发现了func和metal error，就改变M2以及以上金属层的连线，连接备用cell去修。代工厂再给你做M2以及以上金属层的片子。这样就可以不需要修改place 只改指定metal的routing就可以了
在Encounter中，采用specifySpareGate来定义spare cell
+ track: Track是指布线轨道，类似grid和row一样，可以规划绕线器的走线方案。信号线通常必须snap到track上。我们把track分为Pref track和Non-pref track。 也称为垂直和水平布线轨道。通常在design的tech lef中定义。Lef中与该层layer的direction相对应的就是pref track, 剩下的另外一个定义就是Non-pref track。
+ Row: 我们知道row是表征Floorplan横向排列的一个重要网格，它对std cell的摆放起着限制约束作用。那Encounter中其实有多种row类型，各自对应着不同的用法。下面我们就来讲讲它们各自的定义方法和作用。
我们首先得知道SITE单元属性的概念。SITE定义的是最小的布局单位.row也有自己的方向，通常相邻的row会相互abut并且flip,这样能够节省打电源线资源
+ cell status
  - Unplaced就是没有instance还没有place，还在右下角呢。
  - Placed就是该instance已经place好了，但是不稳定，接下来的步骤工具都可以去动它
  - Fixed就是相当于preplace住了，接下来步骤中，工具不能动它了，但是你自己还是可以去动它的.
  - Cover就是cover cell了，在这种状态下，连你自己也不能去动它了，一般做完一些super cmd后，会把整个top cell设定成cover cell. 防止自己的错误操作。
  - SoftFixed: 前面几种状态大家都很熟悉吧，但大家知道softFixed吗？softFixed是介于placed和fixed之间的一种状态，它代表着该状态下的instance在global place中不能被移动。但是在detail place中的legalization可以移动，optDesign中可以被upsize.
