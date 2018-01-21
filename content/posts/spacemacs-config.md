---
title: "Spacemacs 简单配置"
date: 2018-01-21T17:43:02+08:00
---

一直在找一个开箱即用，键位与 vim 相同，但也不缺少可扩展性的编辑器，为什么不直接使用 vim 呢，原因是没有花时间把配置折腾好，而且有时候在家写前端代码较少，想看看其他语言，这样配置出来的东西也没法在工作的时候使用。

昨天找到了 [spacemacs](http://spacemacs.org/) 这个 emacs 发行版，之前没用过 emacs ，感觉它功能太强大，作为编辑器的话也太复杂。不过这个 spacemacs 这套配置，把很多基本功能都涵盖了。而且最主要的是，它可以使用 vim 模式！也就是说在 emacs 复杂的功能上，我们可以用上 vim 的键位，而可能不太需要学习 emacs 那套五花八门复杂的键位了（然后发现 spacemacs 的键位也好多啊）。

spacemacs 最好的地方是当打开新的文件时，会根据文件类型自动下载相应的 layer (也就是对文件类型的支持库)，坑的地方是，天朝网络原因，下载巨慢，经常会失败。

#### 安装

在 mac 上要使用 spacemacs ，当然第一步是要安装 emacs 的，macOS 在最近的版本里都自带了 emacs 22.x ，但是这个版本太旧，spacemacs 是不支持的，并且如果要获取到最新的 emacs 版本，自然要自己下载了。

[emacs-mac](https://emacsformacosx.com/) 提供了最新的给 macOS 用的 emacs 版本，我们可以使用 Homebrew Cask 来安装：

```
brew cask install emacs-mac
```

然后按照 spacemacs 的文档安装 spacemacs 即可：

```
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
```

如果这时候用 GUI 启动 emacs 是没用问题的，因为 emacs 版本是刚才安装的 emacs-mac 了，但是在命令行内不行，这个 emacs 会使用 macOS 自带的 emacs ，导致无法使用 spacemacs，解决方法也很简单，把 emacs 命令 alias 到刚才安装的 emacs 上：

```
alias emacs="/Applications/Emacs.app/Contents/MacOS/Emacs -nw"
```

`-nw` 参数是用来在命令行启动的时候不适用 GUI 模式，如不需要则可以去掉。

#### 配 置

自己用编辑器需要的功能并不多，简单来说需要配置 spacemacs 的地方有，自动显示文件所在的项目目录，可以打开终端。下面的配置都是写在 .spacemacs 里的。

自动显示目录结构：

```
(add-hook 'after-init-hook #'neotree-toggle)
```

终端配置：

```
(shell :vairables
        shell-default-shell 'term
        shell-default-term-shell "bin/bash"
        shell-default-full-span nil
        shell-default-height 20
        shell-default-position 'bottom
)
```

其中 `shell-default-full-span nil` 是由于打开 neotree 后，宽度被改变，导致终端无法显示全宽，然后打不开，所以设置成 nil ，让终端可以正常显示。

spacemacs 默认不显示行号（这让人很不解），所以要修改 `dotspacemacs-line-nubmers t`，这样行号就正常了。

接下来是文件 tab 的显示，在网上搜索了一下，与 vim 或者 vscode 这样在视觉上显示 tab 的方式，貌似是 [workspace](http://spacemacs.org/doc/DOCUMENTATION.html#workspaces) 这个功能。

使用

```
SPC l w 2
```

可以创建第2个 workspace，之后就可以使用 gt 或者 gT 来切换下一个或上一个 workspace 了。

当然也有快捷键来切换不同的文件和项目：

查看当前所有 buffer:

```
SPC b b
```

切换到最近的 buffer:

```
SPC TAB
```

切换项目：

```
SPC p p
```

寻找项目内的文件：

```
SPC p f
```

还有个痛点是在终端内，没法复制内容到系统剪贴板，也不从系统剪贴板粘贴，这个功能在 GUI 模式下是正常的。参考 github 上的 (issue)[https://github.com/syl20bnr/spacemacs/issues/2222]，我们可以在 .spacemacs 的 user-config 内加上两段代码，即可在终端内也实现此功能。

```
(defun copy-to-clipboard ()
      "Copies selection to x-clipboard."
      (interactive)
      (if (display-graphic-p)
          (progn
            (message "Yanked region to x-clipboard!")
            (call-interactively 'clipboard-kill-ring-save)
            )
        (if (region-active-p)
            (progn
              (shell-command-on-region (region-beginning) (region-end) "pbpaste")
              (message "Yanked region to clipboard!")
              (deactivate-mark))
          (message "No region active; can't yank to clipboard!")))
      )

    (defun paste-from-clipboard ()
      "Pastes from x-clipboard."
      (interactive)
      (if (display-graphic-p)
          (progn
            (clipboard-yank)
            (message "graphics active")
            )
        (insert (shell-command-to-string "pbpaste"))
        )
      )
  (evil-leader/set-key "o y" 'copy-to-clipboard)
  (evil-leader/set-key "o p" 'paste-from-clipboard)
```

这样复制到剪贴版则用 `SPC o y`，粘贴用 `SPC o p`。到此，我们对一个编辑器需要的基本功能是完善了，其他自动补全这些代码上的事情，还没有试过，想必应该不需要太多配置既可使用了。

PS: 如果 spacemacs 在下载 package 的时候一直不成功，或者出现 Bad Request ，可以尝试用 emcas --insecure -nw 打开，不知道为什么 Let's Encrypt 的证书被不信任了。
