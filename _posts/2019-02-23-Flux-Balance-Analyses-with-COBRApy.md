Here, I use a tool called COBRApy (https://github.com/opencobra/cobrapy) to simulate metabolic flux generations in an E. coli model. Most of the information provided here is already available at: https://cobrapy.readthedocs.io/en/stable/ 

I used few modules from the above document to show how to 1. read the model, 2. generate optimal fluxes, 3. knock reactions, 4. change growth media, 5. print out required fluxes using the jupyter notebook. 

```python
# Author: Viswanadham Sridhara
# Date: 23 February 2019
# Title: Flux Balance Analyses with COBRApy

```


```python
from __future__ import print_function
import cobra
import os
from os.path import join
```


```python
sbml_path = join("Data","iAF1260.xml.gz")
print(sbml_path)

```

    Data\iAF1260.xml.gz
    


```python
iAF1260_ecoli_model = cobra.io.read_sbml_model(sbml_path)
```


```python
# Here the objective function is biomass and optimize function calculates the fluxes to get the max biomass (this can be changed)
iAF1260_ecoli_model.optimize()
```




<strong><em>Optimal</em> solution with objective value 0.737</strong><br><div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fluxes</th>
      <th>reduced_costs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12DGR120tipp</th>
      <td>0.000000</td>
      <td>-0.010031</td>
    </tr>
    <tr>
      <th>12DGR140tipp</th>
      <td>0.000000</td>
      <td>-0.010031</td>
    </tr>
    <tr>
      <th>12DGR141tipp</th>
      <td>0.000000</td>
      <td>-0.016049</td>
    </tr>
    <tr>
      <th>12DGR160tipp</th>
      <td>0.000000</td>
      <td>-0.010031</td>
    </tr>
    <tr>
      <th>12DGR161tipp</th>
      <td>0.000000</td>
      <td>-0.018055</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>ZN2abcpp</th>
      <td>0.000000</td>
      <td>-0.008025</td>
    </tr>
    <tr>
      <th>ZN2t3pp</th>
      <td>0.000000</td>
      <td>-0.002006</td>
    </tr>
    <tr>
      <th>ZN2tpp</th>
      <td>0.002327</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>ZNabcpp</th>
      <td>0.000000</td>
      <td>-0.008025</td>
    </tr>
    <tr>
      <th>Zn2tex</th>
      <td>0.002327</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>2382 rows × 2 columns</p>
</div>




```python
iAF1260_ecoli_model.summary()
```

    IN FLUXES            OUT FLUXES    OBJECTIVES
    -------------------  ------------  ----------------------
    o2_e       16.3      h2o_e  37.2   BIOMASS_Ec_i...  0.737
    glc__D_e    8        co2_e  17.8
    nh4_e       7.94     h_e     6.77
    pi_e        0.708
    so4_e       0.184
    k_e         0.131
    mg2_e       0.00582
    fe2_e       0.00556
    fe3_e       0.00523
    ca2_e       0.00349
    cl_e        0.00349
    cobalt2_e   0.00233
    cu2_e       0.00233
    mn2_e       0.00233
    mobd_e      0.00233
    zn2_e       0.00233
    


```python
# remove nitrogen source and look at Biomass objective function
# here the only nitrogen source seems to be nh4_e.
iAF1260_ecoli_model.reactions.NH4tex.knock_out()
```


```python
iAF1260_ecoli_model.optimize()
```




<strong><em>Optimal</em> solution with objective value 0.000</strong><br><div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fluxes</th>
      <th>reduced_costs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12DGR120tipp</th>
      <td>0.000000e+00</td>
      <td>3.330669e-16</td>
    </tr>
    <tr>
      <th>12DGR140tipp</th>
      <td>0.000000e+00</td>
      <td>3.330669e-16</td>
    </tr>
    <tr>
      <th>12DGR141tipp</th>
      <td>0.000000e+00</td>
      <td>6.106227e-16</td>
    </tr>
    <tr>
      <th>12DGR160tipp</th>
      <td>0.000000e+00</td>
      <td>3.330669e-16</td>
    </tr>
    <tr>
      <th>12DGR161tipp</th>
      <td>0.000000e+00</td>
      <td>6.106227e-16</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>ZN2abcpp</th>
      <td>0.000000e+00</td>
      <td>2.220446e-16</td>
    </tr>
    <tr>
      <th>ZN2t3pp</th>
      <td>0.000000e+00</td>
      <td>5.551115e-17</td>
    </tr>
    <tr>
      <th>ZN2tpp</th>
      <td>7.906134e-18</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>ZNabcpp</th>
      <td>0.000000e+00</td>
      <td>2.220446e-16</td>
    </tr>
    <tr>
      <th>Zn2tex</th>
      <td>7.906134e-18</td>
      <td>0.000000e+00</td>
    </tr>
  </tbody>
</table>
<p>2382 rows × 2 columns</p>
</div>




```python
# get the original model 
iAF1260_ecoli_model = cobra.io.read_sbml_model(sbml_path)


```


```python
# change glucose source fluxes to different values and see how it affects the objective function
iAF1260_ecoli_model.medium
```




    {'EX_ca2_e': 999999.0,
     'EX_cbl1_e': 0.01,
     'EX_cl_e': 999999.0,
     'EX_co2_e': 999999.0,
     'EX_cobalt2_e': 999999.0,
     'EX_cu2_e': 999999.0,
     'EX_fe2_e': 999999.0,
     'EX_fe3_e': 999999.0,
     'EX_glc__D_e': 8.0,
     'EX_h2o_e': 999999.0,
     'EX_h_e': 999999.0,
     'EX_k_e': 999999.0,
     'EX_mg2_e': 999999.0,
     'EX_mn2_e': 999999.0,
     'EX_mobd_e': 999999.0,
     'EX_na1_e': 999999.0,
     'EX_nh4_e': 999999.0,
     'EX_o2_e': 18.5,
     'EX_pi_e': 999999.0,
     'EX_so4_e': 999999.0,
     'EX_tungs_e': 999999.0,
     'EX_zn2_e': 999999.0}




```python
# currently EX_glc__D_e is at 8, change it to 20 (Instead of knocking out NH4tex, this is another way of changing the source)
# copy the mediums, change the values and put the medium back in the model (there might be simpler way)
medium = iAF1260_ecoli_model.medium
medium["EX_glc__D_e"] = 20
iAF1260_ecoli_model.medium = medium
```


```python
iAF1260_ecoli_model.optimize()
```




<strong><em>Optimal</em> solution with objective value 1.218</strong><br><div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fluxes</th>
      <th>reduced_costs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12DGR120tipp</th>
      <td>0.000000</td>
      <td>-0.024236</td>
    </tr>
    <tr>
      <th>12DGR140tipp</th>
      <td>0.000000</td>
      <td>-0.024236</td>
    </tr>
    <tr>
      <th>12DGR141tipp</th>
      <td>0.000000</td>
      <td>-0.043625</td>
    </tr>
    <tr>
      <th>12DGR160tipp</th>
      <td>0.000000</td>
      <td>-0.024236</td>
    </tr>
    <tr>
      <th>12DGR161tipp</th>
      <td>0.000000</td>
      <td>-0.043625</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>ZN2abcpp</th>
      <td>0.000000</td>
      <td>-0.019389</td>
    </tr>
    <tr>
      <th>ZN2t3pp</th>
      <td>0.000000</td>
      <td>-0.004847</td>
    </tr>
    <tr>
      <th>ZN2tpp</th>
      <td>0.003846</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>ZNabcpp</th>
      <td>0.000000</td>
      <td>-0.019389</td>
    </tr>
    <tr>
      <th>Zn2tex</th>
      <td>0.003846</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>2382 rows × 2 columns</p>
</div>




```python
iAF1260_ecoli_model.summary()
```

    IN FLUXES            OUT FLUXES           OBJECTIVES
    -------------------  -------------------  ---------------------
    glc__D_e   20        h_e       53         BIOMASS_Ec_i...  1.22
    o2_e       18.5      h2o_e     41.6
    nh4_e      13.1      for_e     23.1
    pi_e        1.17     ac_e      18.7
    so4_e       0.305    co2_e      9.53
    k_e         0.216    glyclt_e   0.000815
    mg2_e       0.00961
    fe2_e       0.0092
    fe3_e       0.00865
    ca2_e       0.00577
    cl_e        0.00577
    cobalt2_e   0.00385
    cu2_e       0.00385
    mn2_e       0.00385
    mobd_e      0.00385
    zn2_e       0.00385
    


```python
# Repeat the above by changing it to -20 (see the negative sign)
medium["EX_glc__D_e"] = -20
iAF1260_ecoli_model.medium = medium
```


```python
iAF1260_ecoli_model.optimize()
```

    cobra\util\solver.py:416 UserWarning: solver status is 'infeasible'
    




<strong><em>infeasible</em> solution</strong>




```python
# Back to original value of 8
medium["EX_glc__D_e"] = 8
iAF1260_ecoli_model.medium = medium
```


```python
iAF1260_ecoli_model.optimize()
```




<strong><em>Optimal</em> solution with objective value 0.737</strong><br><div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fluxes</th>
      <th>reduced_costs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12DGR120tipp</th>
      <td>0.000000</td>
      <td>-0.010031</td>
    </tr>
    <tr>
      <th>12DGR140tipp</th>
      <td>0.000000</td>
      <td>-0.010031</td>
    </tr>
    <tr>
      <th>12DGR141tipp</th>
      <td>0.000000</td>
      <td>-0.018055</td>
    </tr>
    <tr>
      <th>12DGR160tipp</th>
      <td>0.000000</td>
      <td>-0.010031</td>
    </tr>
    <tr>
      <th>12DGR161tipp</th>
      <td>0.000000</td>
      <td>-0.018055</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>ZN2abcpp</th>
      <td>0.000000</td>
      <td>-0.008025</td>
    </tr>
    <tr>
      <th>ZN2t3pp</th>
      <td>0.000000</td>
      <td>-0.002006</td>
    </tr>
    <tr>
      <th>ZN2tpp</th>
      <td>0.002327</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>ZNabcpp</th>
      <td>0.000000</td>
      <td>-0.008025</td>
    </tr>
    <tr>
      <th>Zn2tex</th>
      <td>0.002327</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>2382 rows × 2 columns</p>
</div>




```python
# Get a sense of energy production and related features
iAF1260_ecoli_model.metabolites.atp_c.summary()

```

    PRODUCING REACTIONS -- ATP C10H12N5O13P3 (atp_c)
    ------------------------------------------------
    %      FLUX  RXN ID      REACTION
    ---  ------  ----------  --------------------------------------------------
    72%  52      ATPS4rpp    adp_c + 4.0 h_p + pi_c <=> atp_c + h2o_c + 3.0 h_c
    18%  13.1    PGK         3pg_c + atp_c <=> 13dpg_c + adp_c
    5%    3.34   SUCOAS      atp_c + coa_c + succ_c <=> adp_c + pi_c + succoa_c
    4%    2.72   PPK         atp_c + pi_c <=> adp_c + ppi_c
    1%    1.03   PYK         adp_c + h_c + pep_c --> atp_c + pyr_c
    
    CONSUMING REACTIONS -- ATP C10H12N5O13P3 (atp_c)
    ------------------------------------------------
    %      FLUX  RXN ID      REACTION
    ---  ------  ----------  --------------------------------------------------
    61%  44.2    BIOMASS...  0.000223 10fthf_c + 0.000223 2ohph_c + 0.5137 a...
    12%   8.39   ATPM        atp_c + h2o_c --> adp_c + h_c + pi_c
    9%    6.19   PFK         atp_c + f6p_c --> adp_c + fdp_c + h_c
    3%    1.99   NDPK1       atp_c + gdp_c <=> adp_c + gtp_c
    2%    1.78   ACCOAC      accoa_c + atp_c + hco3_c --> adp_c + h_c + malc...
    2%    1.33   GLNS        atp_c + glu__L_c + nh4_c --> adp_c + gln__L_c +...
    1%    0.788  ASPK        asp__L_c + atp_c <=> 4pasp_c + adp_c
    1%    0.687  R15BPK      atp_c + r15bp_c --> adp_c + prpp_c
    1%    0.687  R1PK        atp_c + r1p_c --> adp_c + h_c + r15bp_c
    


```python
# Instead of maximizing biomass, we can change the objective function to maximize ATPM
iAF1260_ecoli_model.objective = "ATPM"
iAF1260_ecoli_model.reactions.get_by_id("ATPM").upper_bound


```




    8.39




```python
iAF1260_ecoli_model.reactions.get_by_id("ATPM").lower_bound
```




    8.39




```python
# Looks like the ATPM upper bound and lower bound is fixed at 8.39.
# If we run the model now, the optimum calculated will be same.
# INSTEAD, change the upper and lower bounds to different values and see the optimum with objective function of ATPM

iAF1260_ecoli_model.reactions.get_by_id("ATPM").upper_bound = 1000
iAF1260_ecoli_model.reactions.get_by_id("ATPM").lower_bound = -1000
```


```python
iAF1260_ecoli_model.reactions.get_by_id("ATPM").upper_bound
```




    1000




```python
iAF1260_ecoli_model.reactions.get_by_id("ATPM").lower_bound
```




    -1000




```python
iAF1260_ecoli_model.optimize()
```




<strong><em>Optimal</em> solution with objective value 95.813</strong><br><div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fluxes</th>
      <th>reduced_costs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12DGR120tipp</th>
      <td>0.0</td>
      <td>-2.5</td>
    </tr>
    <tr>
      <th>12DGR140tipp</th>
      <td>0.0</td>
      <td>-2.5</td>
    </tr>
    <tr>
      <th>12DGR141tipp</th>
      <td>0.0</td>
      <td>-4.5</td>
    </tr>
    <tr>
      <th>12DGR160tipp</th>
      <td>0.0</td>
      <td>-2.5</td>
    </tr>
    <tr>
      <th>12DGR161tipp</th>
      <td>0.0</td>
      <td>-4.5</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>ZN2abcpp</th>
      <td>0.0</td>
      <td>-2.0</td>
    </tr>
    <tr>
      <th>ZN2t3pp</th>
      <td>0.0</td>
      <td>-0.5</td>
    </tr>
    <tr>
      <th>ZN2tpp</th>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>ZNabcpp</th>
      <td>0.0</td>
      <td>-2.0</td>
    </tr>
    <tr>
      <th>Zn2tex</th>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>2382 rows × 2 columns</p>
</div>




```python
# Looks like the ATPM value went from 9 all the way to 95 (based on constraints on other reactions)
# Summary of atp reactions with this new objective function is
iAF1260_ecoli_model.metabolites.atp_c.summary()
```

    PRODUCING REACTIONS -- ATP C10H12N5O13P3 (atp_c)
    ------------------------------------------------
    %      FLUX  RXN ID    REACTION
    ---  ------  --------  --------------------------------------------------
    61%   63.8   ATPS4rpp  adp_c + 4.0 h_p + pi_c <=> atp_c + h2o_c + 3.0 h_c
    15%   16     PGK       3pg_c + atp_c <=> 13dpg_c + adp_c
    14%   14.8   ACKr      ac_c + atp_c <=> actp_c + adp_c
    8%     8     PYK       adp_c + h_c + pep_c --> atp_c + pyr_c
    1%     1.25  SUCOAS    atp_c + coa_c + succ_c <=> adp_c + pi_c + succoa_c
    
    CONSUMING REACTIONS -- ATP C10H12N5O13P3 (atp_c)
    ------------------------------------------------
    %      FLUX  RXN ID    REACTION
    ---  ------  --------  --------------------------------------------------
    92%   95.8   ATPM      atp_c + h2o_c <=> adp_c + h_c + pi_c
    8%     8     PFK       atp_c + f6p_c --> adp_c + fdp_c + h_c
    


```python

```
