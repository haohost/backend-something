## optmization

placement/ route is not timing-drived, so there are still some optimization to do after placement and route.
+ fix timing, DRV and power
+ eg: PPA(Power, performance, area) measurement of a chip

### basic methods
+ what to do?
  - add buffer/double inv
  - resizeing
  - moving
  - pin swap
  - restruct circuits structure: simple cell -> complex cell
  - layer assignment: use higher layer to route, reduce RC
  
### prects
+ command: place_opt_design -opt
+ goal: DRV clean, setup timing clorse, no concern on hold violation
process
+ rc extraction and build timing graph
+ global optimization
  1. Netlist simplification: add/delete buffer, improve structure
  2. 



