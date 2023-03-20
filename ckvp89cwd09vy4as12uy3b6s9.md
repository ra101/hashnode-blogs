---
title: "New Bash Alias: Git Out"
datePublished: Tue Apr 20 2021 16:59:55 GMT+0000 (Coordinated Universal Time)
cuid: ckvp89cwd09vy4as12uy3b6s9
slug: new-bash-alias-git-out
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1636289060415/v4rWENuNt.png

---

Up until now, we only had `git commit` and `git push`. But what about `git out`? If you know what it feels like to be left out, you should empathize with this command. Even though it is a part of this legendary trio, it is never actually used.

But not today, Not anymore!
Let's right the wrongs of this society!
So, if you are with me!

Open Your `.bashrc` File!

```Bash
nano ~/.bashrc
```

<br>

Add This Line! & Save it! (ctrl+x, y, return)

```bash
git() { if [[ $@ == "out" ]]; then command "exit"; else command git "$@"; fi; }
```

<br>

And Finally! Source it!

```bash
source ~/.bashrc
```



Good job! Be Proud! Because of you, there is one less lonely command!

PS: Feel free to share any meme command out there.