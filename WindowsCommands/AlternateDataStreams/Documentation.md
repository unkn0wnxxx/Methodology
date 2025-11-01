# Alternate Data Streams

can hide information within an file.

---


## Discovery that an file is hidden by ADS

Checking if file has streams

```
dir /r hm.txt
```

Or with PS

```
Get-Item -Path normal.txt -Stream *

```


```
touch file.txt
```

Nano automatically creates an stream to the file and we can store any content inside it.

```
nano file.txt:hidden
```
