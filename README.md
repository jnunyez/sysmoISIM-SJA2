# sysmoISIM-SJA2

This repo details how to program sysmoISIM-SJA2 cards.

## Background

The sysmoISIM-SJA2 support the USIM protocol for 3G and 4G but don'tsupport the calculation of SUCI required in 5G networks. This is done by the Mobile Equipment (ME). 
*The sysmoISIM-SJA2 supports "SUCI calculation by ME" using EF_SUCI_Calc_Info as per 3GPP TS 31.102 Section 5.3.47. It does not support "SUCI Calculation by USIM" as per section 5.3.48*

For more information see the official manual [here](https://sysmocom.de/manuals/sysmousim-manual.pdf).

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
