#USIM config for 5G

```
./pySim-shell.py -p 0
Using PC/SC reader interface
Autodetected card type: sysmoISIM-SJA2
AIDs on card:
 USIM: a0000000871002ffffffff8907090000
 ISIM: a0000000871004ffffffff8907090000
Welcome to pySim-shell!
```

Verify Admin Key since we want to write values on the SIM 

```
pySIM-shell (MF)> verify_adm 70725779
```

Calc Info

```
pySIM-shell (MF)> select ADF.USIM
pySIM-shell (MF/ADF.USIM)> select DF.5GS
pySIM-shell (MF/ADF.USIM/DF.5GS)> select EF.SUCI_Calc_Info 
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> read_binary_decoded 
missing Protection Scheme Identifier List data object tag
{}
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> update_binary_decoded '{ "prot_scheme_id_list": [ {"priority": 0, "identifier": 2, "key_index": 1}, {"priority": 1, "identifier": 1, "key_index": 2}, {"priority": 2, "identifier": 0, "key_index": 0}], "hnet_pubkey_list": [ {"hnet_pubkey_identifier": 27, "hnet_pubkey": "0272DA71976234CE833A6907425867B82E074D44EF907DFB4B3E21C1C2256EBCD1"}, {"hnet_pubkey_identifier": 30, "hnet_pubkey": "5A8D38864820197C3394B92613B20B91633CBD897119273BF8E4A6F4EEC0A650"}]}'
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
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> tree
EF.5GS3GPPLOCI            4f01 5S 3GP location information
EF.5GSN3GPPLOCI           4f02 5S 3GP location information
EF.5GS3GPPNSC             4f03 5GS 3GPP Access NAS Security Context
EF.5GSN3GPPNSC            4f04 5GS 3GPP Access NAS Security Context
EF.5GAUTHKEYS             4f05 5G authentication keys
EF.UAC_AIC                4f06 UAC Access Identities Configuration
EF.OPL5G                  6f08 5GS Operator PLMN List
EF.SUPI_NAI               4f09 SUPI as Network Access Identifier
EF.Routing_Indicator      4f0a Routing Indicator
EF.URSP                   4f0b UE Route Selector Policies per PLMN
EF.TN3GPPSNN              4f0c Trusted non-3GPP Serving network names list
```

pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> dir
MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info
3f00/a0000000871002/5fc0/4f07
 ..                    EF.5GSN3GPPNSC        EF.Routing_Indicator  
 DF.5GS                EF.5GAUTHKEYS         EF.URSP               
 EF.5GS3GPPLOCI        EF.UAC_AIC            EF.TN3GPPSNN          
 EF.5GSN3GPPLOCI       EF.OPL5G              
 EF.5GS3GPPNSC         EF.SUPI_NAI           
13 files
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.SUCI_Calc_Info)> select EF.Routing_Indicator
{
    "file_descriptor": {
        "shareable": true,
        "file_type": "working_ef",
        "structure": "transparent"
    },
    "file_identifier": "4F0A",
    "proprietary_info": {
        "proprietary_D0": "20",
        "proprietary_D2": "0F"
    },
    "life_cycle_status_int": "operational_activated",
    "security_attrib_ref_expanded": "6F0603",
    "file_size": 4,
    "short_file_id": "50"
}

pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> read_binary_decoded 
{
    "raw": "ffffffff"
}

pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> update_binary 
Hint:
  data  Data bytes (hex format) to write

pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> update_binary ffffff71
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> read_binary
ffffff71
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> read_binary_decoded 
{
    "raw": "ffffff71"
}
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> select EF.UST
EXCEPTION of type 'ValueError' occurred with message: 'Cannot select unknown file by name EF.UST, only hexadecimal 4 digit FID is allowed'
To enable full traceback, run the following command: 'set debug true'
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> dir
MF/ADF.USIM/DF.5GS/EF.Routing_Indicator
3f00/a0000000871002/5fc0/4f0a
 ..                 EF.5GS3GPPNSC      EF.SUCI_Calc_Info  EF.TN3GPPSNN       
 DF.5GS             EF.5GSN3GPPNSC     EF.OPL5G           
 EF.5GS3GPPLOCI     EF.5GAUTHKEYS      EF.SUPI_NAI        
 EF.5GSN3GPPLOCI    EF.UAC_AIC         EF.URSP            
13 files
pySIM-shell (MF/ADF.USIM/DF.5GS/EF.Routing_Indicator)> select ADF.USIM
{
    "file_descriptor": {
        "shareable": true,
        "file_type": "df",
        "structure": "no_info_given"
    },
    "df_name": "A0000000871002FFFFFFFF8907090000",
    "proprietary_info": {
        "uicc_characteristics": "71",
        "available_memory": 101640
    },
    "life_cycle_status_int": "operational_activated",
    "security_attrib_compact": "00",
    "pin_status_template_do": "90017083010183018183010A83010B"
}
pySIM-shell (MF/ADF.USIM)> dir
MF/ADF.USIM
3f00/a0000000871002
 ..                EF.SMS            EF.OPLMNwAcT      EF.MUK            
 MF                EF.MSISDN         EF.ARR            EF.GBANL          
 EF.LI             EF.SMSP           EF.NETPAR         EF.EHPLMN         
 EF.IMSI           EF.SMSS           EF.PNN            EF.EHPLMNPI       
 EF.Keys           EF.SDN            EF.OPL            EF.NAFKCA         
 EF.KeysPS         EF.EXT2           EF.MBDN           EF.SPNI           
 EF.PLMNwAcT       EF.EXT3           EF.MBI            EF.PNNI           
 EF.HPPLMN         EF.SMSR           EF.MWIS           EF.NCP-IP         
 EF.ACMmax         EF.ICI            EF.CFIS           EF.EPSLOCI        
 EF.UST            EF.OCI            EF.EXT7           EF.EPSNSC         
 EF.ACM            EF.ICT            EF.SPDI           EF.UFC            
 EF.GID1           EF.OCT            EF.MMSN           EF.NASCONFIG      
 EF.GID2           EF.EXT5           EF.EXT8           EF.PWS            
 EF.SPN            EF.CCP2           EF.MMSICP         EF.FDNURI         
 EF.PUCT           EF.eMLPP          EF.MMSUP          EF.BDNURI         
 EF.CBMI           EF.AAeM           EF.MMSUCP         EF.SDNURI         
 EF.ACC            EF.BDN            EF.NIA            EF.IPS            
 EF.FPLMN          EF.EXT4           EF.VGCS           EF.eDPDGId        
 EF.LOCI           EF.CMI            EF.VGCSS          EF.FromPreferred  
 EF.AD             EF.EST            EF.VBS            DF.WLAN           
 EF.CBMID          EF.ACL            EF.VBSS           DF.HNB            
 EF.ECC            EF.DCK            EF.VGCSCA         DF.ProSe          
 EF.CBMIR          EF.CNL            EF.VBCSCA         DF.5GS            
 EF.PSLOCI         EF.START-HFN      EF.GBABP          
 EF.FDN            EF.THRESHOLD      EF.MSK            
98 files
pySIM-shell (MF/ADF.USIM)> tree 
EF.LI                     6f05 Language Indication
EF.IMSI                   6f07 IMSI
EF.Keys                   6f08 Ciphering and Integrity Keys
EF.KeysPS                 6f09 Ciphering and Integrity Keys for PS domain
EF.PLMNwAcT               6f60 User controlled PLMN Selector with Access Technology
EF.HPPLMN                 6f31 Higher Priority PLMN search period
EF.ACMmax                 6f37 ACM maximum value
EF.UST                    6f38 USIM Service Table
EF.ACM                    6f39 Accumulated call meter
EF.GID1                   6f3e Group Identifier Level 1
EF.GID2                   6f3f Group Identifier Level 2
EF.SPN                    6f46 Service Provider Name
EF.PUCT                   6f41 Price per unit and currency table
EF.CBMI                   6f45 Cell Broadcast message identifier selection
EF.ACC                    6f78 Access Control Class
EF.FPLMN                  6f7b Forbidden PLMNs
EF.LOCI                   6f7e Location information
EF.AD                     6fad Administrative Data
EF.CBMID                  6f48 Cell Broadcast Message Identifier for Data Download
EF.ECC                    6fb7 Emergency Call Codes
EF.CBMIR                  6f50 Cell Broadcast message identifier range selection
EF.PSLOCI                 6f73 PS Location information
EF.FDN                    6f3b Fixed Dialling Numbers
EF.SMS                    6f3c Short messages
EF.MSISDN                 6f40 MSISDN
EF.SMSP                   6f42 Short message service parameters
EF.SMSS                   6f43 SMS status
EF.SDN                    6f49 Service Dialling Numbers
EF.EXT2                   6f4b Extension2 (FDN)
EF.EXT3                   6f4c Extension2 (SDN)
EF.SMSR                   6f47 SMS status reports
EF.ICI                    6f80 Incoming Call Information
EF.OCI                    6f81 Outgoing Call Information
EF.ICT                    6f82 Incoming Call Timer
EF.OCT                    6f83 Incoming Call Timer
EF.EXT5                   6f4e Extension5 (ICI/OCI/MSISDN)
EF.CCP2                   6f4f Capability Configuration Parameters 2
EF.eMLPP                  6fb5 enhanced Multi Level Pre-emption and Priority
EF.AAeM                   6fb6 Automatic Answer for eMLPP Service
EF.BDN                    6f4d Barred Dialling Numbers
EF.EXT4                   6f55 Extension4 (BDN/SSC)
EF.CMI                    6f58 Comparison Method Information
EF.EST                    6f56 Enabled Services Table
EF.ACL                    6f57 Access Point Name Control List
EF.DCK                    6f2c Depersonalisation Control Keys
EF.CNL                    6f32 Co-operative Network List
EF.START-HFN              6f5b Initialisation values for Hyperframe number
EF.THRESHOLD              6f5c Maximum value of START
EF.OPLMNwAcT              6f61 User controlled PLMN Selector with Access Technology
EF.ARR                    6f06 Access Rule Reference
EF.NETPAR                 6fc4 Network Parameters
EF.PNN                    6fc5 PLMN Network Name
EF.OPL                    6fc6 Operator PLMN List
EF.MBDN                   6fc7 Mailbox Dialling Numbers
EF.MBI                    6fc9 Mailbox Identifier
EF.MWIS                   6fca Message Waiting Indication Status
EF.CFIS                   6fcb Call Forwarding Indication Status
EF.EXT7                   6fcc Extension7 (CFIS)
EF.SPDI                   6fcd Service Provider Display Information
EF.MMSN                   6fce MMS Notification
EF.EXT8                   6fcf Extension8 (MMSN)
EF.MMSICP                 6fd0 MMS Issuer Connectivity Parameters
EF.MMSUP                  6fd1 MMS User Preferences
EF.MMSUCP                 6fd2 MMS User Connectivity Parameters
EF.NIA                    6f51 Network's Indication of Alerting
EF.VGCS                   6fb1 Voice Group Call Service
EF.VGCSS                  6fb2 Voice Group Call Service Status
EF.VBS                    6fb3 Voice Group Call Service
EF.VBSS                   6fb4 Voice Group Call Service Status
EF.VGCSCA                 6fd4 Voice Group Call Service Ciphering Algorithm
EF.VBCSCA                 6fd5 Voice Group Call Service Ciphering Algorithm
EF.GBABP                  6fd6 GBA Bootstrapping parameters
EF.MSK                    6fd7 MBMS Service Key List
EF.MUK                    6fd8 MBMS User Key
EF.GBANL                  6fda GBA NAF List
EF.EHPLMN                 6fd9 Equivalent HPLMN
EF.EHPLMNPI               6fdb Equivalent HPLMN Presentation Indication
EF.NAFKCA                 6fdd NAF Key Centre Address
EF.SPNI                   6fde Service Provider Name Icon
EF.PNNI                   6fdf PLMN Network Name Icon
EF.NCP-IP                 6fe2 Network Connectivity Parameters for USIM IP connections
EF.EPSLOCI                6fe3 EPS Location Information
EF.EPSNSC                 6fe4 EPS NAS Security Context
EF.UFC                    6fe6 USAT Facility Control
EF.NASCONFIG              6fe8 Non Access Stratum Configuration
EF.PWS                    6fec Public Warning System
EF.FDNURI                 6fed Fixed Dialling Numbers URI
EF.BDNURI                 6fee Barred Dialling Numbers URI
EF.SDNURI                 6fef Service Dialling Numbers URI
EF.IPS                    6ff1 IMEI(SV) Pairing Status
EF.eDPDGId                6ff3 Home ePDG Identifier
EF.FromPreferred          6ff7 From Preferred
DF.WLAN                   5f40 Files for WLAN purpose
  EF.Pseudo               4f41 Pseudonym
  EF.UPLMNWLAN            4f42 User controlled PLMN selector for I-WLAN Access
  EF.OPLMNWLAN            4f43 Operator controlled PLMN selector for I-WLAN Access
  EF.UWSIDL               4f44 User controlled WLAN Specific Identifier List
  EF.OWSIDL               4f45 Operator controlled WLAN Specific Identifier List
  EF.WRI                  4f46 WLAN Reauthentication Identity
  EF.HWSIDL               4f47 Home I-WLAN Specific Identifier List
  EF.WEHPLMNPI            4f48 I-WLAN Equivalent HPLMN Presentation Indication
  EF.WHPI                 4f49 I-WLAN HPLMN Priority Indication
  EF.WLRPLMN              4f4a I-WLAN Last Registered PLMN
  EF.HPLMNDAI             4f4b HPLMN Direct Access Indicator
DF.HNB                    5f50 Files for HomeNodeB purpose
  EF.ACSGL                4f01 Allowed CSG Lists
  EF.CSGTL                4f02 CSG Types
  EF.HNBN                 4f03 Home NodeB Name
  EF.OCSGL                4f04 Operator CSG Lists
  EF.OCSGT                4f05 Operator CSG Type
  EF.OHNBN                4f06 Operator Home NodeB Name
DF.ProSe                  5f90 Files for ProSe purpose
  EF.PROSE_MON            4f01 ProSe Monitoring Parameters
  EF.PROSE_ANN            4f02 ProSe Announcing Parameters
  EF.PROSEFUNC            4f03 HPLMN ProSe Function
  EF.PROSE_RADIO_COM      4f04 ProSe Direct Communication Radio Parameters
  EF.PROSE_RADIO_MON      4f05 ProSe Direct Discovery Monitoring Radio Parameters
  EF.PROSE_RADIO_ANN      4f06 ProSe Direct Discovery Announcing Radio Parameters
  EF.PROSE_POLICY         4f07 ProSe Policy Parameters
  EF.PROSE_PLMN           4f08 ProSe PLMN Parameters
  EF.PROSE_GC             4f09 ProSe Group Counter
  EF.PST                  4f10 ProSe Service Table
  EF.UIRC                 4f11 ProSe UsageInformationReportingConfiguration
  EF.PROSE_GM_DISCOVERY   4f12 ProSe Group Member Discovery Parameters
  EF.PROSE_RELAY          4f13 ProSe Relay Parameters
  EF.PROSE_RELAY_DISCOVER 4f14 ProSe Relay Discovery Parameters
DF.5GS                    5fc0 5GS related files
  EF.5GS3GPPLOCI          4f01 5S 3GP location information
  EF.5GSN3GPPLOCI         4f02 5S 3GP location information
  EF.5GS3GPPNSC           4f03 5GS 3GPP Access NAS Security Context
  EF.5GSN3GPPNSC          4f04 5GS 3GPP Access NAS Security Context
  EF.5GAUTHKEYS           4f05 5G authentication keys
  EF.UAC_AIC              4f06 UAC Access Identities Configuration
  EF.SUCI_Calc_Info       4f07 SUCI Calc Info
  EF.OPL5G                6f08 5GS Operator PLMN List
  EF.SUPI_NAI             4f09 SUPI as Network Access Identifier
  EF.Routing_Indicator    4f0a Routing Indicator
  EF.URSP                 4f0b UE Route Selector Policies per PLMN
  EF.TN3GPPSNN            4f0c Trusted non-3GPP Serving network names list
pySIM-shell (MF/ADF.USIM)> select EF.UST 
{
    "file_descriptor": {
        "shareable": true,
        "file_type": "working_ef",
        "structure": "transparent"
    },
    "file_identifier": "6F38",
    "proprietary_info": {
        "proprietary_D0": "20",
        "proprietary_D2": "0F"
    },
    "life_cycle_status_int": "operational_activated",
    "security_attrib_ref_expanded": "6F0603",
    "file_size": 20,
    "short_file_id": "20"
}
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
    126
]
pySIM-shell (MF/ADF.USIM/EF.UST)> ust_service_deactivate 125
pySIM-shell (MF/ADF.USIM/EF.UST)> help

Documented commands (use 'help -v' for verbose/'help <topic>' for details):

File-Specific Commands
======================
ust_service_activate  ust_service_deactivate

ISO7816 Commands
================
activate_file  deactivate_file  open_channel  unblock_chv
change_chv     disable_chv      select        verify_chv 
close_channel  enable_chv       status      

pySim Commands
==============
desc  dir  export  intro  reset  tree  verify_adm

Transparent EF Commands
=======================
edit_binary_decoded  read_binary_decoded  update_binary_decoded
read_binary          update_binary      

pySim-shell built-in commands
=============================
alias  help     macro  quit          run_script  shell    
edit   history  py     run_pyscript  set         shortcuts

pySIM-shell (MF/ADF.USIM/EF.UST)> quit
