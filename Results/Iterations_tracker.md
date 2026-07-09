# ASIC Synthesis and Iteration Tracker

This file documents the iterative optimization runs and strategy sweeps performed during the OpenROAD/LibreLane ASIC design flow for the `fft_top` design.

---

## 1. Run Log (Iteration Tracker)

| Run # | Sweep Purpose | CLOCK_PERIOD (ns) | FP_CORE_UTIL (%) | PL_TARGET_DENSITY | SYNTH_STRATEGY | Notes on Change | Instance Count | Core Area (um2) | Utilization | Setup WNS (ns) | Setup TNS (ns) | Hold WNS (ns) | Hold TNS (ns) | Setup WS max corner (ns) | Max Slew Violations | Max Fanout Violations | Max Cap Violations | Route DRC Errors | Antenna Violations | Magic DRC Errors | KLayout DRC Errors | LVS Errors | Power Total (W) | Route Wirelength (um) | Pass/Fail |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | Baseline run (initial config) | 30 | - | - | AREA 0 | First clean run – establishing Baseline | 9132 | 57475.1 | 0.736002 | 0 | 0 | 0 | 0 | 16.358 | 1401 | 26 | 28 | 0 | 0 | 0 | 0 | 0 | 0.002064 | 113562 | PASS |
| **2** | | 10 | - | - | Delay 4 | Reduced the number of maxSlew violations and setup wns | 12899 | 68105.3 | 0.573082 | 0 | 0 | 0 | 0 | 1.97956145967242 | 599 | 24 | 0 | 0 | 0 | 0 | 0 | 0 | 0.00578680541366339 | 125040 | PASS |
| **3** | pin_config.cfg and core util of 65% addedafter verifying for lowe cases like 45%, 55%, etc | 10 | 65 | - | Delay 4 | Arranged the i/p and o/p pinsin order and attempted to makethe design more compact | 12853 | 68105.3 | 0.572917 | 0 | 0 | 0 | 0 | 1.91975329955517 | 530 | 15 | 0 | 0 | 0 | 0 | 0 | 0 | 0.00572399003431201 | 136806 | PASS |
| **4** | Added target density of 0.7after verifying for lowe cases Like 0.4,0.5, etc | 10 | 65 | 0.7 | DELAY 4 | Attempt to make the design morecompact by giving a target density to the algorithm | 12878 | 68105.3 | 0.572457 | 0 | 0 | 0 | 0 | 0.60447026872616 | 368 | 26 | 0 | 0 | 0 | 0 | 0 | 0 | 0.00573303503915668 | 84645 | PASS |
| **5** | Ran the same parameters but changed the synth strat to AREA 1 | 10 | 65 | 0.7 | AREA 1 | To compare the final parametersof AREA 1 and DELAY 4 | 9040 | 57365 | 0.733598 | 0 | 0 | 0 | 0 | 1.99741295823485 | 1165 | 34 | 0 | 0 | 0 | 0 | 0 | 0 | 0.00617261696606875 | 152731 | PASS |

---

## 2. Strategy Sweep (STAPrePNR)

> **Note:** This sweep was run with `--to OpenROAD.STAPrePNR`, meaning it stops after synthesis and pre-placement STA. No placement, routing, or physical DRC/LVS happens at this stage—so area, utilization, and route metrics are not available here. This sweep is meant to quickly compare how each `SYNTH_STRATEGY` affects cell count, pre-PnR power estimate, and pre-PnR timing/slew.

| Strategy | Run Tag | Instance Count | Instance Area (um2) | Power Internal (W) | Power Switching (W) | Power Leakage (W) | Power Total (W) | Setup WNS (ns) | Setup TNS (ns) | Setup WS worst-corner (ns) | Hold WNS (ns) | Hold TNS (ns) | Hold WS worst-corner (ns) | Max Slew Violations | Max Fanout Violations | Max Cap Violations | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **AREA 3** | AREA_3 | 4201 | 39392.7808 | 0.00156761857215315 | 0.000427576538641006 | 4.31791917776536E-08 | 0.00199523824267089 | 0 | 0 | 20.5366822693714 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 845 | 6 | 4 | |
| **AREA 2** | AREA_2 | 2227 | 28815.136 | 0.00114996882621199 | 7.61908304411918E-05 | 2.58400518760027E-08 | 0.0012261854717508 | 0 | 0 | 19.9992774668351 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 1544 | 75 | 18 | |
| **AREA 1** | AREA_1 | 2227 | 28825.1456 | 0.00114973424933851 | 7.63814532547258E-05 | 2.58098804550855E-08 | 0.00122614158317447 | 0 | 0 | 20.611438472715 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 1537 | 77 | 18 | |
| **AREA 0** | AREA_0 | 2228 | 28845.1648 | 0.00114979012869299 | 7.64341384638101E-05 | 2.59043329009501E-08 | 0.00122625019866973 | 0 | 0 | 20.6389542409355 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 1532 | 77 | 18 | |
| **DELAY 0** | DELAY_0 | 2655 | 33753.6224 | 0.00157912739086896 | 0.000259029533481225 | 3.28555138651154E-08 | 0.00183818978257477 | 0 | 0 | 13.9907946965533 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 4136 | 87 | 22 | |
| **DELAY 1** | DELAY_1 | 2625 | 32069.5072 | 0.00154897477477789 | 0.000222566784941591 | 2.78312093371369E-08 | 0.00177156936842948 | 0 | 0 | 13.3360863904528 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 3211 | 89 | 18 | |
| **DELAY 2** | DELAY_2 | 2753 | 33030.4288 | 0.00156582798808813 | 0.000227859083679505 | 2.88052248720305E-08 | 0.00179371587000787 | 0 | 0 | 13.3360863904528 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 3415 | 91 | 20 | |
| **DELAY 3** | DELAY_3 | 2619 | 33608.4832 | 0.00160732376389205 | 0.000262422254309058 | 3.37126522254039E-08 | 0.00186977977864444 | 0 | 0 | 14.5946974192532 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 3981 | 97 | 22 | |
| **DELAY 4** | DELAY_4 | 2944 | 34128.9824 | 0.00109683023765683 | 3.95623392250855E-05 | 4.2452484194655E-08 | 0.00113643507938832 | 0 | 0 | 20.9917014157231 | -0.0244970439756022 | -0.0244970439756022 | -0.0244970439756022 | 309 | 8 | 2 | Looks lie the best strat to implement because of lower number of slew violations and low total power |
