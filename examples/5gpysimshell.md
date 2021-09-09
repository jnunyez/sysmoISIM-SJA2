# USIM config for 5G

This example is required if SUPI concealment is required by the 5G-Core. By defaut, sysmoISIM-SJA2 has Services 123 (5G Security Parameters) & 124 (Subscription identifier privacy support) are set by default but files to calculate SUCI are empty by default. These files need to be configured in order to search/connect for anything 5G-SA. Enter the pysimShell:

```shell
./pySim-shell.py -p 0
Using PC/SC reader interface
Autodetected card type: sysmoISIM-SJA2
AIDs on card:
 USIM: a0000000871002ffffffff8907090000
 ISIM: a0000000871004ffffffff8907090000
Welcome to pySim-shell!
```

Verify Admin Key since we want to write values on the SIM 

```shell
pySIM-shell (MF)> verify_adm 70725779
```

## Write SUCI_CalcInfo

By default 'EF_SUCI_Calc_Info' files are empty.

```shell
pySIM-shell (MF)> select ADF.USIM
pySIM-shell (MF/ADF.USIM)> select DF.5GS
pySIM-shell (MF/ADF.USIM/DF.5GS)> select EF.SUCI_Calc_Info 
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> read_binary_decoded 
missing Protection Scheme Identifier List data object tag
{}
```

Write the EF.SUCI_Calc_Info with testing public keys.

```shell
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> update_binary_decoded '{ "prot_scheme_id_list": [ {"priority": 0, "identifier": 2, "key_index": 1}, {"priority": 1, "identifier": 1, "key_index": 2}, {"priority": 2, "identifier": 0, "key_index": 0}], "hnet_pubkey_list": [ {"hnet_pubkey_identifier": 27, "hnet_pubkey": "0272DA71976234CE833A6907425867B82E074D44EF907DFB4B3E21C1C2256EBCD1"}, {"hnet_pubkey_identifier": 30, "hnet_pubkey": "5A8D38864820197C3394B92613B20B91633CBD897119273BF8E4A6F4EEC0A650"}]}'
```

Read EF_SUCI_Calc_Info in `json` format.

```shell
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> read_binary_decoded 
{
    "prot_scheme_id_list": [
        {
            "priority": 0,
            "identifier": 2,
            "key_index": 1
        },
        {
            "priority": 1,
            "identifier": 1,
            "key_index": 2
        },
        {
            "priority": 2,
            "identifier": 0,
            "key_index": 0
        }
    ],
    "hnet_pubkey_list": [
        {
            "hnet_pubkey_identifier": 27,
            "hnet_pubkey": "0272da71976234ce833a6907425867b82e074d44ef907dfb4b3e21c1c2256ebcd1"
        },
        {
            "hnet_pubkey_identifier": 30,
            "hnet_pubkey": "5a8d38864820197c3394b92613b20b91633cbd897119273bf8e4a6f4eec0a650"
        }
    ]
}
```

## Write Routing Indicator

As specified in [3GPPTS35.221](https://www.etsi.org/deliver/etsi_ts/123000_123099/123003/16.03.00_60/ts_123003v160300p.pdf) the Routing Indicator together with the Home Network Identifier (i.e., MCC and MNC) allows routing network signalling with SUCI to AUSF and UDM instances capable to server the subscriber. Read the Routing Indicator.

```shell
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> select EF.Routing_Indicator
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> read_binary_decoded 
{
    "raw": "ffffffff"
}
```


Write the Routing Indicator. 


```shell
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> update_binary ffffff71
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> read_binary
ffffff71
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> read_binary_decoded 
{
    "raw": "ffffff71"
}
```

## USIM Service Table

Assure that the USIM service table disable service 125 since SUCI calculation by USIM is not possible with sysmocom. A list of services can be found in [TS31.102](https://www.etsi.org/deliver/etsi_ts/131100_131199/131102/15.01.00_60/ts_131102v150100p.pdf)

```shell
pySIM-shell (MF/ADF.USIM/EF.UST)> read_binary_decoded 
[
    2,
    3,
    4,
    5,
    6,
    9,
    10,
    11,
    12,
    13,
    14,
    15,
    17,
    18,
    19,
    20,
    21,
    25,
    27,
    28,
    29,
    33,
    34,
    35,
    38,
    39,
    42,
    43,
    44,
    45,
    46,
    51,
    60,
    71,
    73,
    85,
    86,
    87,
    89,
    90,
    93,
    94,
    122,
    123,
    124,
	125,
    126
]
pySIM-shell (MF/ADF.USIM/EF.UST)> ust_service_deactivate 125
```