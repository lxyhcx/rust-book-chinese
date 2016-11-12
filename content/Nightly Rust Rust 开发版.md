# Rust开发版

> [nightly-rust.md](https://github.com/rust-lang/rust/blob/master/src/doc/book/nightly-rust.md)
> <br>
> commit 6ba952020fbc91bad64be1ea0650bfba52e6aab4

Rust提供了3种发行渠道：开发版（每日构建），beta版和稳定版。不稳定功能只在Rust开发版中可用。对于这个进程的更多细节，查看[作为可支付的稳定性](http://blog.rust-lang.org/2014/10/30/Stability.html)。

要安装Rust开发版，你可以使用`rustup.sh`：

```bash
$ curl -s https://static.rust-lang.org/rustup.sh | sh -s -- --channel=nightly
```

如果你担心使用`curl | sh`的[潜在不安全性](http://curlpipesh.tumblr.com)，请继续阅读并查看我们下面的免责声明。并且你也可以随意使用下面这个两步安装脚本以便可以检查我们的安装脚本：

```bash
$ curl -f -L https://static.rust-lang.org/rustup.sh -O
$ sh rustup.sh --channel=nightly
```

如果你用Windows，请直接下载[32位](https://static.rust-lang.org/dist/rust-nightly-i686-pc-windows-gnu.exe)或者[64位](https://static.rust-lang.org/dist/rust-nightly-x86_64-pc-windows-gnu.exe)安装包然后运行即可。

## 卸载

如果不幸的，你再也不想使用Rust了:(，当然这不要紧。也许Rust不是你的菜（原文：不是所有人都会认为什么语言非常好）。运行下面的卸载脚本：

```bash
$ sudo /usr/local/lib/rustlib/uninstall.sh
```

如果你使用Windows安装包进行安装的话，重新运行`.exe`文件，它会提供一个卸载选项。

你可以在任何时候重新运行脚本来更新Rust。在现在这个时间，你将会频繁更新Rust，因为Rust还未发布1.0版本，经常更新人们会认为你在使用最新版本的Rust。

不过这带来了另外一个问题（传说中的免责声明？）：一些同学确实有理由对我们让他们运行`curl | sudo sh`感到非常反感。他们理应如此。从根本上说，当你运行上面的脚本时，代表你相信是一些好人在维护Rust，他们不会黑了你的电脑做坏事。对此保持警觉是一个很好的天性。如果你是这些强迫症患者（大雾），请检阅以下文档，[从源码编译Rust](https://github.com/rust-lang/rust#building-from-source)或者[官方二进制文件下载](http://www.rust-lang.org/install.html)。

当然，我们需要提到官方支持的平台：

* Windows (7, 8, Server 2008 R2)
* Linux (2.6.18 or later, various distributions), x86 and x86-64
* OSX 10.7 (Lion) or greater, x86 and x86-64

Rust 在以上平台进行了广泛的测试，当然还在一些其他平台，比如Android。不过进行了越多测试的环境，越有可能正常工作。

最后，关于Windows。Rust将Windows作为第一级平台来发布，不过说实话，WIndows的集成体验并没有Linux/OS X那么好。我们正在改善它！如果有情况它不能工作了，这是一个bug。如果这种发生了，请让我知道。任何一次提交都在Windows下进行了测试，就像其它平台一样。

如果你已安装Rust，你可以打开一个Shell，然后输入：

```bash
$ rustc --version
```

你应该看到版本号，提交的hash值，提交时间和构建时间：

```bash
rustc 1.0.0-nightly (f11f3e7ba 2015-01-04) (built 2015-01-06)
```

如果你做到了，那么Rust已经正确安装！此处应有掌声！

如果你遇到什么错误，有几个你可以获取帮助的地方。最简单的是通过[Mibbit](http://chat.mibbit.com/?server=irc.mozilla.org&channel=%23rust)访问[Rust IRC频道 irc.mozilla.org](irc://irc.mozilla.org/#rust)。点击上面的链接，你就可以与其它Rustaceans（简单理解为Ruster吧）聊天，我们会帮助你。其它的地方有[the /r/rust subreddit](http://www.reddit.com/r/rust)和[Stack Overflow](http://stackoverflow.com/questions/tagged/rust)。
