---
title: "Solve: There is no Brightness button on my new keyboard ヽ(°〇°)ﾉ"
datePublished: Sun Jun 06 2021 11:43:00 GMT+0000 (Coordinated Universal Time)
cuid: ckvp898i909vx4as19bml9adh
slug: solve-there-is-no-brightness-button-on-my-new-keyboard
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1636289054681/6ZH54H6cw.jpeg

---

As the title says, I bought a new keyboard for my laptop, unfamiliar with world of keyboards, I went with the safest option, approved by many, the Logitech G213 Prodigy. 

Much to my suprise, I was not able to control brightness with this new keyboard.
- No function keys were mapped to brightness settings
- There was no specific keys for that
- Even G Hub software had no System Action for brightness

So, the dev inside me said, lets tackle it with software!
<br>
By the help of stack-overflow and powershell docs, I wrote a simple powershell script, to adjust my brightness...
{% gist https://gist.github.com/ra101/c2060ade52280b4fcbe3291331caf1e0 %}

```powershell
.\brightness.ps1 <# No agrs will increase brightness (+10) #>
```
```powershell
.\brightness.ps1 desc <# desc as arg will decrease brightness (-10) #>
```

<br>
Now I compiled this file to `.exe`, to replicate that,

- First Install ps2exe module in powershell (as admin)
```powershell
Install-Module ps2exe
```

- Simply use this module to convert ps1 to exe
```
Invoke-ps2exe .\brightness.ps1 .\brightness.exe -noConsole
```

<br>
First I thought of using AutoHotKey, but i couldn't utilise any function key there. So, settled with G-Hub instead

Now, head over to G-Hub add this file as application, **twice** , one for +10 and other for -10, remember to add `desc` there and then change whatever Function keys you want map to brightness.

FYI: I used F6 key to change to G-shift mode. and F7 to increase brightness and F8 to decrease brightness in G-shift mode. No updation made in default mode.

<br>
Please comment, for any suggestion regarding this keyboard, or any wonderful hack you have in mind.