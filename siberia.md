## The low productivity in PFT 8 and 9 

</br>

### 1. Problem report

PFT 8 and 9 cannot sustain their LAI and die continuously from low productivity. 

|![](default_LAI_Max.png)|
|:--:|
|*Fig.1 Annual maximum LAI for 30-year simulation in Siberia. Each point reflects each pixel and the title of each small figure represents PFT. In PFT 8 and 9, points lined in maximum values imply that pixel has replanted vegetation, and it is shown the overall period. Color represents yearly maximum WSTRESS_MONTH*|
 
|![](sibera.30yr.die.png)|
|:---:|
|*Fig.2 Number of years when replanting occurred, per pixels for PFT8 (leaf) and PFT9 (right). Exact years are written on each pixel.*|

Even though the short-term simulation such as 30 years, more than half of the pixels showed replanting 1 to 6 times.

</br>

### 2. Parameter adjustment

The low LAI is caused by the lack of reserves for bud break, which is assumed to result from low productivity or high (water) stress. To test this hypothesis, 4 parameters were tested with PFT 9.

1) tau_leaf. Turn over time for the leaf. By increasing tau_leaf less carbon can be used to sustain leaf mass.
2) k_latosa. High k_latosa allows higher LAI which brings higher production.
3) psi_leaf. Leaf water potential. More negative psi_leaf generates a higher gradient between soil and leaf and would reduce water stress. 
4) Decoupling psi_root from psi_leaf. As psi_root (root water potential) is poorly measured, the current code set psi_root the same as psi_leaf but this can reduce the gradient for water pressure. Set psi_root more negative than psi_leaf would result in less water stress. 
**All tests have been done with ORC_trun r6681.**
</br>

Table 1. 4 parameters adjusted 
| Parameters | Default | Change |  Label|
|:-----------|:-------:|:-------:|:-:|
|tau_leaf| 180| 1000| tau|
|k_latosa| 4500| 9000| latosa|
|psi_leaf| -1.6| -3.0| leaf-3|
|psi_root| =psi_leaf|=2*psi_leaf|root

**Label: label used in figures to refer to each configration.*

|![](lai.compare.default.png)|
|:--:|
|*Fig. 3 LAI result from each parameter changes (See Table. 1).*|

No water stress was set by </br>
`` ENERGY_CONTROL=5, OK_GS_FEEDBACK=n``

* From the result with no stress (right-middle figure), we can know two things: 1. without water stress, there is no dieback; 2. LAI is not high enough considering there is no water stress. This implies the problem was caused by the combination of high water stress and low lai. 
* Doubled_k_latosa increased LAI and decreased times of dieback. It was already proven that current k_latosa is too low from the global run, the next tests were done with double k_latosa. 

|![](lai.compare.latosa.png)|
|:---:|
|*Fig. 4 Annual maximum LAI results from each parameter changes with high k_latosa (See Table. 1).*|

* The result with no water stress (right-middle figure) is closer to expected LAI than the default configuration with no water stress (Fig. 3, right-middle figure). 
* Both psi_leaf and psi_root did increase LAI, and the configuration added all changes showed the best result. 
</br>

### 3. Test for longer simulation
The configuration with double k_latosa, psi_leaf -3 and psi_root -6 showed the most favorable resutls, thus, the add-all configuration was tested for longer years, 90 years.

|![](add_up_LAI_Max.png)|
|:--:|
|*Fig. 5 Annual maximum LAI from 90-year Siberia run with double k_latosa, psi_leaf -3, and psi_root -6. PFT8 (up) and PFT9 (down)*|

|![](sibera.param.90yr.die.png)|
|:--:|
|*Fig. 6 Numbers and times when replanting occurred per pixels from the add-all configuration. PFT8 (left) and PFT9 (right)*|

LAI and times of dieback reduced after all parameter adjustment, but still half of the pixels experienced establishments within 90 years. This implies that the vegetation still couldn't resist to extreme years. It is possible that the problem the trunk currently has, low soil carbon, is related to poor growth. This test will be continued after solving that. 

</br>
</br>
*Regardless of the above parameters tests, it was discussed that how to make plants gradually build carbon reserves during the growing season. This was kept for further development later on. 


