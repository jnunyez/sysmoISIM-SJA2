# sysmoISIM-SJA2

This repo details how to program sysmoISIM-SJA2 cards.

## Background


* **Enable the provisioning/reprovisioning of the SIM card by the user**. We opt for a modality where the SIM can be reprogrammed for the different lab setups. The sysmoISIM-SJA2 pack comes pre-provisioned SIM cards with identities (IMSI, ICCID, MSISDN). To change all this data you will furthermore receive the ADM1 key for every card, which can be used to fully change any identity or key data, as well as the content of any other file on the card. The identities and keys can be changed by the customer.

* **Regarding it usage on 5G-SA networks**s: SUCI required in 5G networks. The sysmoISIM-SJA2 support the USIM protocol for 3G and 4G but don't support the calculation of SUCI required in 5G networks. This is done by the Mobile Equipment (ME). For more information see the official manual [here](https://sysmocom.de/manuals/sysmousim-manual.pdf). But long story short: *The sysmoISIM-SJA2 supports "SUCI calculation by ME" using EF_SUCI_Calc_Info as per 3GPP TS 31.102 Section 5.3.47. It does not support "SUCI Calculation by USIM" as per section 5.3.48*.



## Hardware Needed

1. SIM Card sysmoISIM-SJA2

![SIM Card](./images/sim.png)

2. SIM Card USB Reader 

![SIM Reader](./images/reader.png)

## Software Needed

* [pysim](https://github.com/osmocom/pysim)


## Steps

TBA

## Troubleshooting

TBA
