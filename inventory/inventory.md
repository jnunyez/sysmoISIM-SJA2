# Homelab

# Dallas Lab

sysmoISIM-SJA2 2FF/3FF/4FF SIM ISIM (10-pack) (901-70 MCC-MNC (with ADM keys))

* SIM number 1 (adm=95140700) 

Originally:

IMSI: 901700000051620
MCC: 901
MNC: 70

We change the MCC, MNC and IMSI:

```
./pySim-prog.py -p 0 --type sysmoISIM-SJA2 --pin-adm=95140700 --mcc=001 --mnc=01 --imsi=001010000000011  --iccid=8988211000000516206 --opc=3BA26CF58EC654C0CCEA6A5B60195D9E --ki=DB2D6E313F6C842555B934DC37151FD5
Using PC/SC reader interface
Ready for Programming: Insert card now (or CTRL-C to cancel)
Generated card parameters :
 > Name     : Magic
 > SMSP     : e1ffffffffffffffffffffffff0581005155f5ffffffffffff000000
 > ICCID    : 8988211000000516206
 > MCC/MNC  : 001/01
 > IMSI     : 001010000000011
 > Ki       : DB2D6E313F6C842555B934DC37151FD5
 > OPC      : 3BA26CF58EC654C0CCEA6A5B60195D9E
 > ACC      : None
 > ADM1(hex): 3935313430373030
 > OPMODE   : None
Programming ...
Warning: Programming of the ICCID is not implemented for this type of card.
Programming successful: Remove card from reader
```

* SIM number 2 (adm=11528839)

Originally:

IMSI: 901700000051621
MCC: 901
MNC: 70

We change the MCC, MNC and IMSI:

```
./pySim-prog.py -p 0 --type sysmoISIM-SJA2 --pin-adm=11528839 --mcc=001 --mnc=01 --imsi=001010000000012 --iccid=8988211000000516214 --opc=456B1522AAD06137D8914B01F3D8C9A8 --ki=D4F2D74B3D2EF2884F30219AED254C1F
Using PC/SC reader interface
Ready for Programming: Insert card now (or CTRL-C to cancel)
Generated card parameters :
 > Name     : Magic
 > SMSP     : e1ffffffffffffffffffffffff0581005155f5ffffffffffff000000
 > ICCID    : 8988211000000516214
 > MCC/MNC  : 001/01
 > IMSI     : 001010000000012
 > Ki       : D4F2D74B3D2EF2884F30219AED254C1F
 > OPC      : 456B1522AAD06137D8914B01F3D8C9A8
 > ACC      : None
 > ADM1(hex): 3131353238383339
 > OPMODE   : None
Programming ...
Warning: Programming of the ICCID is not implemented for this type of card.
Programming successful: Remove card from reader
```

# Boston Lab
