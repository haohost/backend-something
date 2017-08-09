###  physical cell
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
