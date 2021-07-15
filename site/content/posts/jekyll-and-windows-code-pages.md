---
comments: true
date: "2013-12-07T03:30:00Z"
description: A quick write up on an error I faced recently when working with Jekyll
  on Windows 7 and its resolution
tags:
- tech
- jekyll
- windows
- errors
- character pages
title: Jekyll and Windows Code Pages
---
####Prelude
I recently moved my blog over to Jekyll! Thanks to a black friday deal over at [Digital Ocean][1], I got my hands on a vps that I dont have to pay for the next 10 months...

So, the first thing I did was to setup my almost dead in the corner blog using [Jekyll][2], something that I've been meaning to do for over a year now.. And unlike a lot of people using Jekyll on *nix and OSX based systems, I was forced to setup Ruby 1.9.3 on Windows 7 and play around with it...

####The WTF
So up comes `c:\work\cruisemaniac.com>jekyll serve` and bam!

~~~
Generating... ←[31m  Liquid Exception: incompatible character encodings: UTF-8 and IBM437 in _layouts/post.html←[0m
error: incompatible character encodings: UTF-8 and IBM437. Use --trace to view backtrace
~~~

####And…the enlightenment
A bit of googling on this error says something about character pages on MS-DOS not being upto speed on handling characters from more advanced character sets - \*cough\* unicode \*cough\*

Windows uses Code pages - different bunches of characters for different languages. And MS-DOS - the command prompt is still stuck with 7-bit ASCII as the default character set.

To check your currently active code page, fire up a command prompt and type in `chcp`.
My Windows installation uses English US as its primary language and the result of the `chcp` command is:
```Active code page: 437```

To switch character code pages, the magical words are ```chcp 65001```.

`65001` is the Microsoft registered code-page for `UTF-8`. And _all is well_!

~~~
C:\work\cruisemaniac.com>chcp 65001
Active code page: 65001

C:\work\cruisemaniac.com>jekyll serve -w
Configuration file: C:/work/cruisemaniac.com/_config.yml
            Source: C:/work/cruisemaniac.com
       Destination: C:/work/cruisemaniac.com/_site
      Generating... done.
 Auto-regeneration: enabled
    Server address: http://0.0.0.0:4000
  Server running... press ctrl-c to stop.
~~~

####And then some..
Some chow on the topic of code pages before I let you be:

* [Microsoft Code Pages][3]
* [Microsoft Code page Identifiers][4]

[1]: https://www.digitalocean.com/?refcode=0a35d10ab88a "Digital Ocean"
[2]: http://jekyllrb.com/ "Jekyll"
[3]: http://msdn.microsoft.com/en-us/library/windows/desktop/dd317752(v=vs.85).aspx "Microsoft Code Pages"
[4]: http://msdn.microsoft.com/en-us/library/windows/desktop/dd317756(v=vs.85).aspx "Microsoft Code Page Identifiers"
