---
layout: post
title:  "Setting up KTF on Debian"
date:   2019-02-08 04:51:10 +0000
categories: Linux
---
Currently, I am :%s/solaris/linux/g , and looking for ways to kick the Linux tires. Enter the Kernel Test Frame (KTF)

The KTF allows you to run gtest style tests on modules loaded into the kernel.
The readme is pretty good but here is the process I followed for a clean Debian install.

First here are the packages I installed.

{% highlight bash %}
sudo apt-get install git
sudo apt-get install googletest
sudo apt-get install pkg-config
sudo apt-get install libnl-3-dev
sudo apt-get install autoreconf
sudo apt-get install dh-autoreconf
sudo apt-get install cmake
{% endhighlight %}

The version of gtest I installed did not seem new enough for KTF, so I built it from source.

{% highlight bash %}
git clone https://github.com/google/googletest.git
cd googletest
cmake
{% endhighlight %}

Then I got the KTF source.

{% highlight bash %}
mkdir ~/src
cd ~/src
git clone https://github.com/oracle/ktf.git
cd ktf
autoreconf
mkdir ~/build/$(uname -r)/ktf
cd ~/build/$(uname -r)/ktf
~/src/ktf/configure KVER=$(uname -r)
make
{% endhighlight %}

If like me you initially got the wrong version of gtest you will need to rerun the autoreconf command.

One hiccup, I got the error message

{% highlight bash %}
make[1]: Entering directory '/root/build/4.9.0-8-amd64/ktf/user'
  CXX      ktfrun.o
  CXXLD    ktfrun
/usr/bin/ld: cannot find -lnl-genl-3
collect2: error: ld returned 1 exit status
Makefile:432: recipe for target 'ktfrun' failed
make[1]: *** [ktfrun] Error 1
make[1]: Leaving directory '/root/build/4.9.0-8-amd64/ktf/user'
Makefile:414: recipe for target 'all-recursive' failed
make: *** [all-recursive] Error 1
{% endhighlight %}

I have libnl installed so wtf. A quick find showed the problem was miss named library.

{% highlight bash %}
ln -s /lib/x86_64-linux-gnu/libnl-genl-3.so.200 /lib/x86_64-linux-gnu/libnl-genl-2.so
{% endhighlight %}

So nothing to do with KTF build succeeded after this change.

You can the insmod, the "hello world" module and see the output on the console when the test is hit.

Kudos KTF team.

















[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
