## PFT 8 and 9 fail to keep LAI

### Problem

PFT 8 and 9 cannot sustain their LAI and died continuously. 

|![LAI per pfts](ORC_dev/default_LAI_Max.png)|
|:--:|
|*Fig.1 Annual maximum LAI for 30-year simulation in Siberia. PFT 8 and PFT 9 shows regenerations for overall period.*|

|![enter image description here](https://lh3.googleusercontent.com/lNEMh_eH2izVkPnHesL2XzW4fNaZamKqnJglgVyDpz0zZuZIJzY9j2hGIqUSZsAyO5udbSc0X3bZrpwIyXDAFXlhd8Trp5Xw074EI6o8nmZi1y8W8poXnIgi8dhrmeD5_yr8PKd9de93Kc_3nntA4Pj_Yi4SFVaoHr1oJo7WRRcgNZ9ncCXJhIRuYZ2DHK7TXqYlcWYIHdTjv2AeVz9UQJQAjV-hF6hmBpveRkPfFfTLGPywH6hXYbdP8DKdT1H8190tbeLbh3CWxHLLgPkyDSTxhpt7gbfYMy9ijnyOWAsAquuRBfgxtMNku8WHVDFzbi3t8DXnt2Gap3cHfblMZWiTMHwDUMAaghPR4cs1gMX8pBxSIAuMT7KLNugnDfIG9AdjjN6qTuIbauEDXUEvyWWMWdOEJDfjDAz-gSvscrb52ybiOz9kIsE-VfouWpNR8E49gXjwGTkVBtWYephpNgFJOVRa-0U4PJzUApH3kJE_ChNWiHaOqdg_9uC1KkEKhJ9_SckD-DwsT87wccLYTuAiTIvFlVmyMbAgmiXI511UyZLr_zur1CX-d7b5G77wlHPs3_i7ZmFpCLkd3IV4giNNZdZwEDD4O8dSXnc5ewD03tfJYflJoNIoK60KjqY0j1oeH5C3ZTApj-os0vdRSIqhrkPrCXBxGExoB03pyD3ao3K_67KaKZR_heAmeg=w2736-h1368-no)|
|:---:|
|*Fig.2 Numbers and times when replanting occurred, per pixels. PFT8 (left) and PFT9 (right)*|

The low LAI caused by the lack of reserves for bud break, which is assumed to be resulted from low productivity or high (water) stress. To test this hypothesis, 4 parameters were tested with PFT 9.
1) tau_leaf. By increasing tau_leaf less carbon can be used for sustain leaf mass.
2) k_latosa. High k_latosa allows higher LAI which brings higher production.
3) psi_leaf. More negative psi_leaf generates higher gradient between soil and leaf and would reduce water stress. 
4) Dcoupling psi_root from psi_leaf. As psi_root is poorly measured, current code set psi_root same as psi_leaf but this can reduce the gradient for water pressure. Set psi_root more negative than psi_leaf would result less water stress. 

| Parameters | Default | Change |  Label|
|:-----------|:-------:|:-------:|:-:|
|tau_leaf| 180| 1000| .tau|
|k_latosa| 4500| 9000| .latosa|
|psi_leaf| -1.6| -3.0| .leaf-3|
|psi_root| =psi_leaf|=2*psi_leaf|.root

**Label: label used in fig to refer to each configration.*

|![enter image description here](https://lh3.googleusercontent.com/oFJwQoUlt5dD0FEhO-eb4sqDOtUzwntGAUwNrJGYvC7nSQZNNtbFSkLPpX5q-xmMlCEBoMHzKeq20V3S5g_M_jpS5YpGdBvaVWuvRCSuyekPkw6OpCWcOjJQ0Z-8yX1z98LuhPb-2EAuV5bv4n5hIr6JjWh6d9yTQ5NMFRk-Ks0NlUFwJgX8bqg7V1UsuuCxtr9ktAseTZP_HzNWiap45H42K7j3oHtR7XsMNvYibQnGGtisXPMJ1CKZxWUPnzcss7wdf9_al2rvor0Q0mywFyIiSYdsvlx5YEeufqNTIH2m8sg251jwK5LCDKHTHx5KHVAbhN9dspDAGguQyZvW6zzfzivy3DiPW3X1n_LU474NGBZ4ymLFIh8gesLhlDHbPUXdAhFUr2eARWwmVUGBlBScTAf8yXivsbuoLDm0e4Yhe6wsxDkbcE2iI8EaYOONsierPSJTzld7JwscP-tpsXvGGRJyS5AYiQIi8CTHe9b5mDITZ4FOGIOjLn7CxO35sTZhvZFANizqlczzu2sN9jR0jSxfvqpNFS2CvHYkubRw-0pd9rHZ1XD3F8LYPNlbu-7VVSOKbRzbH9lL2Ypb8zGIOmv0kVqrl8ajTgjmP2dvEFUpJvSKPYkE3CorCOsHZ5u5DyRWZZ3-iHSmO_fBIBM8FOxhBYBq-COvU6mePMsq65v7lOKFAO5kT82Buw=w2194-h1628-no)|
|:--:|
|*Fig. 3 LAI result from each testcase. No water stress was checked with ENERGY_CONTROL=5 and OK_GS_FEEDBACK=n*|

From the result with no stress, we can notice two things: 1. without water stress, there is no dieback; 2. LAI is not high enough considering there is no water stress. This implies the problem was caused by the combination of high water stress and low lai.  It was already concluded that current k_latosa is too low from global run, next tests were done with double k_latosa. 

 |![enter image description here](https://lh3.googleusercontent.com/_7D-UH7luWm1NI1OB-96AZHkHTSxnJ5AfieMi4hqzVESXIAtxo5v49En-0PTElTHeqMi_B6cdewUolciziVpPYa5sXMDuSqiF_2WupmONXHJz19fw67ki5KTyOJmcR5vIpQ4-2DG3prS92gVlUfst8A6rpmcwEpQzh5CAOc-n15ZzdoTjq8BxzsivNn9JLW6y1ZVgumarES06TqAVCxU67kjdk0mHbTTfBJTZ2BFj6rwZOX0D4HrqNvZiNyEloT4xtZZJRccV6ssXAkE93IHhlkG5fSFgK-AZi0UQvtuNKgXmMQ8ecw4Tcuh1fzMiF7VfGjIDeErAIJJSlb-4yTspXiqVS4RviVCTkkx-am2RFCQ0-E9tTH8jpSB38nuEi_IYpKQT63GdfOXombU6bzHWR5Ydf_pfYRXhSpuOiUY3AC_Ye4rW3zrx0tvJ0Rk7GBubf9cvgY7zUw3DfHASAmySvHDZo7XWss-YYCMwmrQVYx2NfDX8iy1y_L0BiKgw1tj1AGBrPEzklr_w5F9X9hBdZivhoEVvwQnQSSVRnVrGihy4Mq1dZF0RcZmw0HEbA_hKPUJX5MT-RN0VSSnrS2qFQTByI5UVW1N-IZEeaiGH4_faramO7l8UFMQvZpK4YCEZB2aimcPAzG8HihuiEoyFF5inmHCXFZqDA8F_abGAckttHo9ph0r50kg5jsIdQ=w2194-h1628-no)|
 |:---:|
 |*Fig. 4 LAI result from each parameter changes with high k_latosa.*|

Result with no water stress is closer to expected LAI than the default configuration with no water stress (Fig. 3). Both psi_leaf and psi_root did increase LAI, and the configuration with all add-up showed the best result. Thus, all add-up configuration was tested for longer years, 90 years.

|![](https://lh3.googleusercontent.com/AyuUHrv3Lip1gqaEFO-McSGOe5z20q9bkOlV9tZiD5Agf7fBesNpBlk9eTRBiVNoXhYRDEoepbckuTa1RFuGUEGA23tXLGzuoP1WG6UfNlXUjG3TwWPmA72Ona-kKC-F0WmW8OtPCXM7wchrcH9iixPtTjoHNYW3RuZGn6-sOhq9u2y7cvYOGk1zIICuiXBPCI-OdeIqfXKv88Sx2K8KtoFPd6ji4eJMlOxKbNc6EIOSw_CI6Du2hll6rV2dIjtKSXNJZe1hmo-xCsITGMD1J-dUVeF1MSincEslHajVKr1Sxj97EH_QXYzXjzD6055tflSuUM8oBu9LfQHsx24Oktbu8ptMlP-aA3QNo-AsUiuuIM4EHtrujemNfG5Cfy89yUKj4Sa3_t1NtSvIhdfSrsd6hWCTFN3C7yS3lEj6eadExTeaR-2bceqVeJDXrFC3g7T9y1Za5gr0l9ZtVFwrLfAviG1urkVaKLdATe7cLA9Y8PZu3HuPlhfCZMwpOCQRzD3eIUdYX-_qPgPp266r45KPY0riLEUU4dqo1WEgQ-55SN51yvfqixzb4WnMsnlk-uefYCgv8c7V-A1E98yh6qWBHh9QrgOKf13_mojRkjwTjvECWAyPKxSQKmG54RqenzDdEwMWbCXdD6uv87FJzIqq9nxBA62XeILeAStQrBL7ZhUMqWrUO4W1AuMJWQ=w2194-h1628-no)|
|:--:|
|*Fig. 5 Anuual maximum LAI .*|


About reserves 

