---
title: "Intro to Emoji URLs"
datePublished: Fri Nov 05 2021 09:06:07 GMT+0000 (Coordinated Universal Time)
cuid: ckvp88si909vs4as11ckw17pr
slug: intro-to-emoji-urls
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1636289033995/GOmieQuhQ.jpeg

---

I was looking for catchy domain names, And I came across emoji domains like [📙.ws](http://📙.ws/) or [i❤️.ws
](http://i❤️.ws/)

Well, I knew what my task of day is going to be. Below is the report of my 3 hour investigation into Emoji Domains.
<br>
### How are they possible?

To create standardization in Internet hostnames every domain name is encoded into punycode, a LDH (letters, digits, and hyphens) subset of ASCII, by browsers before URL encoding it. pseudo code for the flow can written as:
```
final_url = url.encode(
    punycode.encode(domain.name) + '/rest of the path'
)
```
Now, by only using LDH, other ASCII char are converted into `xn--` form, some examples are:
- `ü` -> `xn--tda`
- `人` -> `xn--gmq`
- `😀` -> `xn--e28h`

links: [punycoder.com](punycoder.com) | [urlenc.com](urlenc.com)
<br>
### Why are they so rare?

With the restriction of LDH many hosting services also add the rule that `-` must be surrounded by characters to decrease changes of scamming or exploits therefore making `--` illegal. 

Click [Here](https://www.20i.com/support/domain-names/domain-name-restrictions) to know more.
<br>

**Loop-Hole: According to [wikipedia](https://en.wikipedia.org/wiki/Emoji_domain#Availability_and_registration), `As of April 2021, there are eleven top-level domains for which registration is possible: .uz, .cf, .ga, .gq, .ml, .tk, .st, .fm, .to, .kz and .ws`**
<br>
### Lets get one!

*I noticed `.tk`, and I remembered long ago they were giving redirector domain names for free that too without signup. I checked back, sadly that was not the case anymore, Although the fees was not that much, But I am not willing to pay for a detour in my evening.*

*Okay if we can't have domain name, lets get a subdomain, 
I already have a [InfinityFree](infinityfree.net) account, So lets try there, but it looked at `--` and said `no latin letters`, So, I looked around and found some websites but I had to input my card info, so i backed out.*

**Finally, [ProFreeHost](https://profreehost.com) was my saviour, I quickly setup an account, found how to redirect the url**, only thing left was to think of a domain name, After ton of soul searching, there it was on my youtube feed, `Lightyear | Teaser Trailer`. **So I went to infinity and beyond and created my website:
[2️⃣♾️➕🚀.unaux.com](http://2️⃣♾️➕🚀.unaux.com/)**
<br>

If you make/made your own Emoji Website, comment below and share with us as well.

