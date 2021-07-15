---
categories:
- posts
comments: true
date: "2013-04-05T00:00:00Z"
description: A few tips on improving productivity while at a shell prompt.
title: A handy Unix shell tip
---
<p>As a configuration management and release engineer, the vast entirety of my work is on *nix - Red Hat primarily and the odd Solaris installation.</p>

<p>While working on the command line with that black window and white text is bliss, I’m growing ever more conscious of the way I work - working smart.</p>

<p>Here’s a tip that I picked up recently and it has definitely improved my speed at one particularly mundane task - creating folder structures.</p>

<p>Assume the following folder structure that needs to be created:</p>

<pre><code>
folderA
|
--------folderB
        |
        --------folderB1
--------folderC
        |
        --------folderC1
</code></pre>

<p>The regular scenario would have you going:</p>

<pre><code>
shell$ mkdir folderA
shell$ cd folderA
folderA$ mkdir folderB
folderA$ cd folderB
folderB$ mkdir folderB1
folderB$ cd ..
folderA$ mkdir folderC
folderA$ cd folderC
folderC$ mkdir folderC1
</code></pre>

<p>Grossly inefficient.</p>

<p>With UNIX, there’s always a better way at this -</p>

<p>use the <code>-p (--parents)</code> parameter with the <code>mkdir</code> command and you’re done in a split second.</p>

<p>Like so:</p>

<pre><code>mkdir -p folderA/{folderB/folderB1,folderC/folderC1}</code></pre>

<p>The -p parameter creates parent folder structures as it sees them and ignores if they already exist.</p>

<p>The curly braces expand the list of comma separated values as individual parameters for the mkdir command. A point to note here is tt include a space between the comma and the next word.</p>

<p>Pretty useful tip to create destination folder tree structures via a shell script. mkdir should be universally supported on all distributions that conform to the UNIX specification - that means most of them in today’s time.</p>
