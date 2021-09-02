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

```console
lsusb
```


* Provision sim-programmer VM. Note that 1) the `Vagrantfile` requires to activate USB port passthrough to detect the USB smart card reader and 2) we embed the VENDOR and PRODUCT IDs of the USB smart card reader.

```console
VENDOR=Oxaaaa PRODUCT=0xbbbb vagrant up --provider=libvirt vagrant up --provider=libvirt
```

* Go to the VM:

```console
vagrant ssh
```

* Scan the USB port for a smart card reader:

```console
pcsc_scan
```

* Get latest sysmocom sw and read/program the SIM:

```console
pySim-read.py -p0
Using PC/SC reader interface
Reading ...
Autodetected card type: sysmoISIM-SJA2
ICCID: 8988211000000529894
IMSI: 901700000052989
GID1: ffffffffffffffffffff
GID2: fffffffffffffffffff
```

## Troubleshooting

* Submit the SIM card type to the database if it does not exist. The first `pcsc_scan` output:

```console
Possibly identified card (using /root/.cache/smartcard_list.txt):
	NONE

Your card is not present in the database.
Please submit your unknown card at:
https://smartcard-atr.apdu.fr/parse?ATR=3B9F96801F878031E073FE211B674A4C753034054BA9
http://ludovic.rousseau.free.fr/softwares/pcsc-tools/smartcard_list.txt
```

* After submitting the unknown card as indicated and attempting again:

```console
pcsc_scan
Possibly identified card (using /root/.cache/smartcard_list.txt):
3B 9F 96 80 1F 87 80 31 E0 73 FE 21 1B 67 4A 4C 75 30 34 05 4B A9
	Test Card (Telecommunication)
```

* Assure polkitd guarantess acces to any user. See [here](https://access.redhat.com/blogs/766093/posts/1976313)
