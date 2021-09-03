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

* Connect USB smart card reader with the sysmoISIM-SJA2 in the SIM slot.

* Get vendor and product id of your USB smart card reader. To get vendor and product id run the following command from your host with USB smart card reader connected:
  ```shell
  lsusb
  ```

* Get pySim code to program and read SIMs
  ```shell
  get_pysim
  ```

* Provision sim-programmer VM with Vagrant. Note that the `Vagrantfile` requires to activate USB port passthrough to detect the USB smart card reader. To attach specific USB port we specify the vendor and product ids of the USB smart card reader. And we require to specify a high-speed USB controller for pysim to interact with the USB smart cart reader, as the default configuration only exposes a USB1.1 controller.
  ```shell
  VENDOR="0x0xbda" PRODUCT="0x0169" MODEL="nec-xhci" vagrant up --provider=libvirt
  ```

* Launch and access the VM:
  ```shell
  vagrant ssh
  ```

* Now inside the VM Scan the USB port for a smart card reader:
  ```shell
  pcsc_scan
  ```

* Get latest sysmocom sw and read/program the SIM:
  ```shell
  cd /vagrant/pysim
  pySim-read.py -p 0
  ```

## Troubleshooting

### USB reader detected but SIM card not detected

In case of problems detecting the SIM card. Submit the SIM card type to the database if it does not exist. This is indicated when running `pcsc_scan` :

```shell
Possibly identified card (using /root/.cache/smartcard_list.txt):
	NONE

Your card is not present in the database.
Please submit your unknown card at:
https://smartcard-atr.apdu.fr/parse?ATR=3B9F96801F878031E073FE211B674A4C753034054BA9
http://ludovic.rousseau.free.fr/softwares/pcsc-tools/smartcard_list.txt
```

After submitting the unknown card as indicated and attempting again:

```shell
pcsc_scan
Possibly identified card (using /root/.cache/smartcard_list.txt):
3B 9F 96 80 1F 87 80 31 E0 73 FE 21 1B 67 4A 4C 75 30 34 05 4B A9
	Test Card (Telecommunication)
```

### Only root can do `pcsc_scan`

Only root user can read the USB card reader. 

If this is the case, check polkitd has the proper policies. The default pcsc policy in `/usr/share/polkit-1/actions/org.debian.pcsc-lite.policy` in only allows admin users to query the pcsc agent. See [here](https://access.redhat.com/blogs/766093/posts/1976313). Workaround to allow any user access pcsc agent:

```bash
#replace use sed to change `authadmin` replaced by `yes` in file
#/usr/share/polkit-1/actions/org.debian.pcsc-lite.policy
sudo  sed -i 's/auth_admin/yes/g' /usr/share/polkit-1/actions/org.debian.pcsc-lite.policy 
#restart policies
sudo systemctl restart polkit
```

## Example

* SIM Test card with adm=70725779

```
./pySim-read.py -p 0
Using PC/SC reader interface
Reading ...
Autodetected card type: sysmoISIM-SJA2
ICCID: 8988211000000529894
IMSI: 901700000052989
GID1: ffffffffffffffffffff
GID2: ffffffffffffffffffff
SMSP: ffffffffffffffffffffffffffffffffffffffffffffffffe1ffffffffffffffffffffffff06815384090067ffffffffff000000
SPN: Not available
Show in HPLMN: False
Hide in OPLMN: False
PLMNsel: ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
PLMNwAcT:
	ffffff0000 # unused
	ffffff0000 # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffff0000 # unused

OPLMNwAcT:
	ffffff0000 # unused
	ffffff0000 # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffff0000 # unused

HPLMNAcT:
	ffffff0000 # unused
	ffffff0000 # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffffffff # unused
	ffffff0000 # unused

ACC: 0200
MSISDN: Not available
Administrative data: 00000002
	MS operation mode: normal
	Ciphering Indicator: disabled
SIM Service Table: ff33ffff3f003f0f300cf0c3f00000
	Service 1 - CHV1 disable function
	Service 2 - Abbreviated Dialling Numbers (ADN)
	Service 3 - Fixed Dialling Numbers (FDN)
	Service 4 - Short Message Storage (SMS)
	Service 5 - Advice of Charge (AoC)
	Service 6 - Capability Configuration Parameters (CCP)
	Service 7 - PLMN selector
	Service 8 - RFU
	Service 9 - MSISDN
	Service 10 - Extension1
	Service 13 - Last Number Dialled (LND)
	Service 14 - Cell Broadcast Message Identifier
	Service 17 - Service Provider Name
	Service 18 - Service Dialling Numbers (SDN)
	Service 19 - Extension3
	Service 20 - RFU
	Service 21 - VGCS Group Identifier List (EFVGCS and EFVGCSS)
	Service 22 - VBS Group Identifier List (EFVBS and EFVBSS)
	Service 23 - enhanced Multi-Level Precedence and Pre-emption Service
	Service 24 - Automatic Answer for eMLPP
	Service 25 - Data download via SMS-CB
	Service 26 - Data download via SMS-PP
	Service 27 - Menu selection
	Service 28 - Call control
	Service 29 - Proactive SIM
	Service 30 - Cell Broadcast Message Identifier Ranges
	Service 31 - Barred Dialling Numbers (BDN)
	Service 32 - Extension4
	Service 33 - De-personalization Control Keys
	Service 34 - Co-operative Network List
	Service 35 - Short Message Status Reports
	Service 36 - Network's indication of alerting in the MS
	Service 37 - Mobile Originated Short Message control by SIM
	Service 38 - GPRS
	Service 49 - MExE
	Service 50 - Reserved and shall be ignored
	Service 51 - PLMN Network Name
	Service 52 - Operator PLMN List
	Service 53 - Mailbox Dialling Numbers
	Service 54 - Message Waiting Indication Status
	Service 57 - Multimedia Messaging Service (MMS)
	Service 58 - Extension 8
	Service 59 - MMS User Connectivity Parameters

EHPLMN:
	09f107 # MCC: 901 MNC: 070
	ffffff # unused
	ffffff # unused
	ffffff # unused

USIM Service Table: beff9f9de73e0408400170730000002e00000000
	Service 2 - Fixed Dialling Numbers (FDN)
	Service 3 - Extension 2
	Service 4 - Service Dialling Numbers (SDN)
	Service 5 - Extension3
	Service 6 - Barred Dialling Numbers (BDN)
	Service 8 - Outgoing Call Information (OCI and OCT)
	Service 9 - Incoming Call Information (ICI and ICT)
	Service 10 - Short Message Storage (SMS)
	Service 11 - Short Message Status Reports (SMSR)
	Service 12 - Short Message Service Parameters (SMSP)
	Service 13 - Advice of Charge (AoC)
	Service 14 - Capability Configuration Parameters 2 (CCP2)
	Service 15 - Cell Broadcast Message Identifier
	Service 16 - Cell Broadcast Message Identifier Ranges
	Service 17 - Group Identifier Level 1
	Service 18 - Group Identifier Level 2
	Service 19 - Service Provider Name
	Service 20 - User controlled PLMN selector with Access Technology
	Service 21 - MSISDN
	Service 24 - Enhanced Multi-Level Precedence and Pre-emption Service
	Service 25 - Automatic Answer for eMLPP
	Service 27 - GSM Access
	Service 28 - Data download via SMS-PP
	Service 29 - Data download via SMS-CB
	Service 32 - RUN AT COMMAND command
	Service 33 - shall be set to 1
	Service 34 - Enabled Services Table
	Service 35 - APN Control List (ACL)
	Service 38 - GSM security context
	Service 39 - CPBCCH Information
	Service 40 - Investigation Scan
	Service 42 - Operator controlled PLMN selector with Access Technology
	Service 43 - HPLMN selector with Access Technology
	Service 44 - Extension 5
	Service 45 - PLMN Network Name
	Service 46 - Operator PLMN List
	Service 51 - Service Provider Display Information
	Service 60 - User Controlled PLMN selector for I-WLAN access
	Service 71 - Equivalent HPLMN
	Service 73 - Equivalent HPLMN Presentation Indication
	Service 85 - EPS Mobility Management Information
	Service 86 - Allowed CSG Lists and corresponding indications
	Service 87 - Call control on EPS PDN connection by USIM
	Service 89 - eCall Data
	Service 90 - Operator CSG Lists and corresponding indications
	Service 93 - Communication Control for IMS by USIM
	Service 94 - Extended Terminal Applications
	Service 95 - Support of UICC access to IMS
	Service 122 - 5GS Mobility Management Information
	Service 123 - 5G Security Parameters
	Service 124 - Subscription identifier privacy support
	Service 126 - UAC Access Identities support

ePDGId:
	Not available

ePDGSelection:
	ffffffffffff # unused
	ffffffffffff # unused
	ffffffffffff # unused
	ffffffffffff # unused

P-CSCF:
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available

Home Network Domain Name: Not available
IMS private user identity: Not available
IMS public user identity:
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available

UICC IARI:
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available
	Not available

ISIM Service Table: 190200
	Service 1 - P-CSCF address
	Service 4 - GBA-based Local Key Establishment Mechanism
	Service 5 - Support of P-CSCF discovery for IMS Local Break Out
	Service 10 - Support of UICC access to IMS

Done !
```

Now we change current values of MCC=901,MNC=70 to MCC=000,MNC=01. To program any field we need the enter the pin-adm value for that card (ping-adm). Output should look like:

```
./pySim-prog.py -p 0 --type sysmoISIM-SJA2 --pin-adm=70725779 --mcc=001 --mnc=01 --imsi=901700000052989  --iccid=8988211000000529894 --opc=2987D4644B7E7D87162C1BCB48AB7C38 --ki=06D6D6903001C2058E7C386E138EFBA5
Using PC/SC reader interface
Ready for Programming: Insert card now (or CTRL-C to cancel)
Generated card parameters :
 > Name     : Magic
 > SMSP     : e1ffffffffffffffffffffffff0581005155f5ffffffffffff000000
 > ICCID    : 8988211000000529894
 > MCC/MNC  : 001/01
 > IMSI     : 901700000052989
 > Ki       : 06D6D6903001C2058E7C386E138EFBA5
 > OPC      : 2987D4644B7E7D87162C1BCB48AB7C38
 > ACC      : None
 > ADM1(hex): 3730373235373739
 > OPMODE   : None
Programming ...
Warning: Programming of the ICCID is not implemented for this type of card.
Programming successful: Remove card from reader
```

Change the IMSI as well:

```
./pySim-prog.py -p 0 --type sysmoISIM-SJA2 --pin-adm=70725779 --mcc=001 --mnc=01 --imsi=001010000052989  --iccid=8988211000000529894 --opc=2987D4644B7E7D87162C1BCB48AB7C38 --ki=06D6D6903001C2058E7C386E138EFBA5
Using PC/SC reader interface
Ready for Programming: Insert card now (or CTRL-C to cancel)
Generated card parameters :
 > Name     : Magic
 > SMSP     : e1ffffffffffffffffffffffff0581005155f5ffffffffffff000000
 > ICCID    : 8988211000000529894
 > MCC/MNC  : 001/01
 > IMSI     : 001010000052989
 > Ki       : 06D6D6903001C2058E7C386E138EFBA5
 > OPC      : 2987D4644B7E7D87162C1BCB48AB7C38
 > ACC      : None
 > ADM1(hex): 3730373235373739
 > OPMODE   : None
Programming ...
Warning: Programming of the ICCID is not implemented for this type of card.
Programming successful: Remove card from reader
```