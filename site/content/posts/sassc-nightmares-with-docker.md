+++
date = "2020-02-04T00:00:00Z"
tags = ["docker", "rails", "sassc", "rubygems", "fargate"]
title = "Sassc nightmares with Docker"
toc = false
+++
We run our infra on AWS Fargate. This means docker images. Our docker image is 
based on debian buster packaged with Ruby 2.6.5.

Of late, we've had crashes on and off everytime the container boots up. They all 
have the same symptom: Rails tries to load the `sassc` gem and it barfs!

The biggest problem is the unpredictability of the crash. It might happen.

It might not!

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Dear team that built the sassC gem, you guys are giving every docker operator nightmares. It crashes sometimes, it doesn’t crash sometimes.<br><br>Please fix this...</p>&mdash; Ashwin Murali (@cruisemaniac) <a href="https://twitter.com/cruisemaniac/status/1224733311748063232?ref_src=twsrc%5Etfw">February 4, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


If you run rails and make use of the `sassc` gem which you most probably will do,
you might run into a problem when the gem is loaded:

```
/vendor/bundle/ruby/2.6.0/gems/ffi-1.11.1/lib/ffi/library.rb:112: [BUG] Illegal instruction at 0x00007f759d834a15
```
You'll see a nasty long thread dump and your app will fail to start.

This is caused by the 2.2.1 version of sassc where the portability between 
processor architectures was removed post version 2.1.0.


There's two ways to fix this:

* Downgrade your gem from 2.2.1 to 2.1.0 until the team releases an official patch - 
The [bug][1] is still open and has been reported across multiple ruby versions.

* Install the gem manually like so:
```
gem install sassc -- --disable-march-tune-native
```
The above step will cause a manual compile of the underlying libraries for the 
gem and the problem goes away.

Happy rubying!

[1]: https://github.com/sass/sassc-ruby/issues/146
