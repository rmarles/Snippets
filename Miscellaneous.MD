# General Tips + Tricks

Back in my HPUX\Linux days I used to use tail -f on logfiles to monitor them as they grow. A friendly contact at Microsoft shared this trick with me.

```
Get-Content -Path "C:\scripts\test.txt" -Wait
```

---

For a while now, Microsoft has been making memorable URLs by shortening them via the “aka.ms” prefix, which I truly appreciate.
One of my favourites addresses a pet peeve of mine: people who ping me with a mere “hello”.
See more here: https://aka.ms/nohello

---

```
Start-Transcript -Path ($PSScriptRoot + "\" + ([io.fileinfo]$MyInvocation.MyCommand.Definition).BaseName + "_" + (get-date -f yyyy-MM-dd) + ".log")
```

---


