## detail_place

+ 主要关注点：需要满足工艺厂定义的各种生产规则（DRC rule），主要为两点：Legalize instances和Local optimize
+ Legalize instances: standard cell placement violations
+ local optimize: wire length optmization, place和route息息相关
### refine placement：
1. Incremental NP：全称**Incremental numerical placement**，这个阶段只用在optimize的refinePlace
因为optimize会增加许多buffer，所以refinePlace会考虑density和timing的因素，给它们一个合理的位置。
2. Place Level Shifter
3. Dynamic Detail Placement:简称DDP，这个是refinePlace的主要阶段，
每个cell会经历三个过程：推（pushing），拉（wire length opt），挤（legalization）。这三者都统称为DDP
logv里面分别记录着这三步过程。我们这边所讲的DDP所指的DDP pushing，它的主要任务就是解掉std cell的overlap，把它们扔到合适的row上。
4. WireLength Opt:也称为Tweakage，这个阶段主要是在小范围内的调整，把连接关系紧密的std cell拉近，
降低wire length。主要通过snap std cell to row, cell的翻转以及交换等措施。
5. RTC:全称high pin density cell spread，这个阶段是20nm以下的advance node设计独有的一个步骤，
主要是在这些advance node的设计中，cell pin的density越来越高，这样会造成pin access的困扰。
6. Legalization:
这一阶段是最后清除前面步骤所产生的placement violation。确立std cell的最终位置。
### congestion Repair 
+  to fix the congeation violations, lots of cells are needed to move. use the commad `congRepair`
1. use trialRoute(earlyGlobal Route) to estimate the high congestion area in the current design. call `hot-spot` 
2. in the **hot-spot** area, tools use `pading` to enlarge the cell in width and lenght, and rerun global placement
3. run refine placement
4. rerun early routing to report congestion
5. end repaire

## Tips
### `checkPlace` : so many place violations, such as
  + Out of core
  + Not of region/fence
  + Off-grid: cell is not located in manufacturing grid
  + Overlap
  + Cell orientation: the cell is not accordance with lef file, MY etc
  + PlaceBlockage
  + Pin access: the pin routing is stopped by PG nets
  + Edge constraint:
