# Performance Counter Debugging

Windows' Performance Counters can enter a corrupt state from time to time. HASS.Agent will show this message:

![166555217-d549bc28-3cd5-487a-90ba-3305c63e1cbd](https://user-images.githubusercontent.com/81011038/166637029-8b1b6987-cc67-4020-9fc6-431add802c9d.png)

You can easily fix this with a few commands. 

First, open an elevated command prompt (search for `cmd` in Start, then hold `ctrl` and `shift` while you press enter) and copy & paste the following commands:

```
cd c:\windows\system32
lodctr /R
cd c:\windows\sysWOW64
lodctr /R
```

```
WINMGMT.EXE /RESYNCPERF
```

You can now close the command prompt. Open an elevated Powershell prompt (search for `powershell` and use the same trick as above) and enter the following commands:

```
Get-Service -Name "pla" | Restart-Service -Verbose
```

```
Get-Service -Name "winmgmt" | Restart-Service -Force -Verbose
```

Now start HASS.Agent, and the error should be gone. Chances are the satellite service crashed as well, so make sure to go to `Configuration` -> `Satellite Service` and click `start service`.