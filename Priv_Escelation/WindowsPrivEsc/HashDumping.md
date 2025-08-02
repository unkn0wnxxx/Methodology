# Hash Dumping and Pass-the-Hash

Once a meterpreter session is gained with NT Authority rights, we can dump the passwordhashes.

First we will utilize kiwi, which is an extension of meterpreter, based on Mimikatz.

load kiwi

lsa_dump_sam --> to get Administrator and all user NTLM Hashes.

hashdump --> to get the total hash LM Hash & NTLM Hash.


