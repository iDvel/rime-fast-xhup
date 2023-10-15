## Rime 输入法双拼加辅助码方案

Rime 输入法配置文件，小鹤双拼+小鹤形码辅助方案）。使用后打字几乎不需要翻页，且学习成本明显低于五笔等输入方案。

* 如果用户还不了解双拼输入方案，或者不熟悉 Rime 输入法软件，可以先查看
  [双拼与 Rime 输入法入门](intro.md) 。当然，在那之前，您也可以先往下翻，看看本项目能提供什么样的功能。

### 特性说明

* 小鹤双拼音码组合优化，采用 不对应汉字读音的字母组合 替换一些手指移动不和谐的字母组合键，实现左右手对侧击键
* 支持模糊音，网络上太多双拼方案模糊音配置都不生效，让我放弃了好几次，终于折腾出来了，造福平翘舌不分的朋友
* 过滤掉生僻字，lua 脚本实现，过滤掉一些不能正常显示的表情，其他实现方案貌似都有缺点
* 支持简拼，将常见成语、俗语、古诗首字母放入自定义短语实现，初始词频低，加了长词优选功能，提升在前三候选
* 加入了长词优选功能，常见英文单词和成语排在第一页，短单词(字母少于4个)不会置顶不会联想补全. 
  特长单词也不会出现第一页, 除非已经输入了这个长单词的前多个字母
* 支持多种常见格式的英文单词输入，如：`Tool`， `TOOL`， `tool`　
* 支持中英文输入方案中英混合输入自动添加空格, 特定符号(,.!?:)后面也会添加空格
* 支持英文单词「自造词」, 在单词末尾输入「`」符号, 下次就可以直接输入这个生词了
* 支持强制删词, 按下 `Ctrl+d` 触发, 立即生效; 词条降频, 按下 `Ctrl+j` 来触发, 将当前高亮词条放置到第4 位, 立即生效 
* 支持 macOS 下输入`jj`前缀的自定义短语，触发启动 App 应用

### 音码组合优化说明

* 原理类似 rime 模糊音， 只是我采用的是 **不对应汉字读音的字母组合**  , 或者某些字母组合的汉字是生僻字，会被挪用
* 用来替换的字母组合 和 被替换的字母组合 不会相互影响 正常输入，如若不喜欢或感觉有影响可自行删除调整
* 替换的字母有这些 `e--> q`，`f-->q` ，`t-->d` , `y-->w`,  `s-->b`
* ```yaml
      - derive/^(j)n/$1c/
      - derive/^([jyn])t/$1d/                             # t --> d
      - derive/^([bjmnx])p/$1e/                           # p --> e
      - derive/^([djnx])q/$1f/                            # q --> f
      - derive/^q([biklmnrsxy])$/f$1/
      - derive/^qq$/fz/
      - derive/^qu$/fs/
      - derive/^qp$/fe/
      - derive/^qt$/fd/
      - derive/^([td])p$/fc/                              # [td]p --> fc
      - derive/^p([okifuhjbnm])$/e$1/                     # pk --> ek
      - derive/^pc$/ef/                                   # pc --> ef
      - derive/^pg$/es/                                   # pg --> es
      - derive/^([jk])y$/$1w/                             # [jk]y --> [jk]w
      - derive/^t([wy])$/dw/                              # t[wy] --> dw
      - derive/^t([er])$/df/                              # t[er] --> df
      - derive/^tk$/rk/
      - derive/^rg$/rd/
      - derive/^xt$/xd/
      - derive/^b([fgnm])$/s$1/                           # b[fgnm] --> s[fgnm]
  ```

### 方案说明

* 方案名称为“小鹤快手”，它提供了小鹤双拼+形码辅助选字的输入方案，使用 `[` 引出辅助码。
  * 如果希望使用 `Tab` 键也能引出辅助码，可以在 `flypy_xhfast.custom.yaml` 文件里设置“tab引导辅助码”。默认的 `Tab`，`Shift+Tab` 键功能可以由 `Control+i`，`Control+o` 替代。
  * 如果希望直接输入辅助码而不需要使用符号引导，可以在同样的文件里设置“直接引导辅助码”。
  * 如果用户习惯使用自然码双拼而非小鹤双拼，可同样设置“改用自然码双拼”。若是其他双拼方案的用户，则可以考虑自行修改 `pinyin_switch.yaml` 中的拼写运算，将小鹤翻译成所需要的双拼（项目提供的是翻译成自然码的运算）。
* 允许纯双拼输入。使用形码辅助造词之后，下次可以直接使用双拼部分输入这个词组。
* 例如，输入“输入”这个词组，“输”可以是 `uu`, `uu[i`, `uu[ir`，“入”可以是 `ru`, `ru[p`, `ru[pd`，敲 3×3 种输入码都可以得到“输入”一词。
  * 由于作者只找到了常用字的形码部分码表，有的生僻一些的字没有形码，只能敲其双拼部分来输入。
  * 双形辅助码根据简体字字形给出，即使在繁体输入模式下也是如此。
* ~~码表基于朙月拼音码表修改得到，保留了对繁体字、含多音字词组的支持特性~~。
  ![小鹤辅助码](xhup.png)
  根据发音不难记忆。
  * 默认为单字（除了手动置顶的那些）显示其输入码，包括小鹤双拼、辅助码两部分。可以在 `flypy_xhfast.custom.yaml` 里设置“关闭单字编码提示”或“单字编码提示只显示辅助码部分”。
  * 若需要专门查某个字的输入码，可以直接在字典文件里搜索，或者利用下方“附加功能”里的 `oh` 前缀。
* 目前本项目已添加使用小鹤原版辅助码的码表（如涉及侵权，可联系项目作者删除）。如果用户更喜欢小鹤版本的双形，可以在 `flypy_xhfast.custom.yaml` 文件里设置“使用小鹤原版辅助码”。
  * 在[这个页面](https://xh.flypy.com/#/xyx)的尾部可以看到小鹤的双形拆分规则。另外，本项目只提供单字码，对词组简码有需求的用户建议直接使用官方的小鹤音形输入法。

### 文件说明

* 将这些文件放入 Rime 的用户目录下，重新部署（右键点击任务栏的 Rime 图标可见）即可。
  * librime 允许输入法指定用户文件夹的位置。用户文件夹的位置应使用绝对路径。请勿使用相对路径
  * 默认的用户目录：
  * `%APPDATA%\Rime`（Windows，小狼毫： 用户文件夹的默认路径为 %APPDATA%\Rime。也可以通过「开始菜单＼小狼毫输入法＼用户文件夹」打开。）;
  * `~/Library/Rime`（MacOS，鼠鬚管： 用户文件夹的路径为 ~/Library/Rime。也可以通过「系统输入法菜单／鼠鬚管／用户设定…」打开。)
  * ~~`~/.config/ibus/rime`~~
 
  * ibus-rime: `~/.config/ibus/rime`
  
    fcitx-rime: `~/.config/fcitx/rime`
    
    fcitx5-rime: `~/.local/share/fcitx5/rime/` （Linux； ~~根据前端类型，可能需要将 ibus 改成 fcitx 或 fcitx5~~）;
  * `/sdcard/rime`（Android）。
  * 如果某些同名文件已经存在，本节的后续部分可能有参考价值。否则，可以直接跳过这些文件内容说明，去看下一节“附加功能”。
  * 如果 Rime 老用户之前已有较多配置文件，希望在不混淆各类配置的同时试用本项目的完整功能（毕竟本项目文件较多），也可以考虑备份/重命名原来的用户目录，将本项目单独放在新建的用户目录下部署。
* 一些设置项需要通过修改文件内容实现。Windows（尤其是 7 及以下的版本）自带的记事本可能无法胜任 YAML 文件的编辑任务，推荐使用 VS Code，Sublime Text 等通用代码编辑器。其他桌面系统的默认文本编辑器原则上都可用。
  * 没有也不想安装代码编辑器的用户可以考虑使用 [在线 YAML 编辑器](https://codebeautify.org/yaml-editor-online)。
  * 此外，所有配置文件都应以 UTF-8 编码保存，YAML 文件还需要保持严格的缩进（只能用空格，不能用 Tab 符号）。
* `flypy_xhfast.schema.yaml` 和 `flypy_xhfast.dict.yaml` 为本方案的主要文件。`flypy_xhfast.custom.yaml` 提供了一些常用设置项。其余文件均用于附加功能。
* `luna_pinyin.custom.yaml` 为只使用全拼的用户提供，已经掌握双拼的用户无需保留该文件。
* `default.custom.yaml` 仅用于声明本方案的依赖方案。如果用户已经有同名的文件，并且其中设置了 `schema_list` 选项，可以直接将本项目同名文件的内容添加到该选项下，而不必使用项目提供的这一文件。
* `stroke.custom.yaml` 用于初始化“部件组字”功能，部署一次即可生效，后续部署不再必需（除非删除了部署生成的二进制文件）。这个文件会影响五笔画方案（stroke）的默认行为。如果用户已有同名文件，同样将内容添加进去即可，部署一次后可以将添加的内容删除。
* `rime.lua` 文件用于涉及 Lua 的相关功能。如果用户已经有同名的文件，可以将本项目文件的内容复制添加到原有文件之中，但是可能需要自行确认其中的变量名、函数名与原有的那些没有冲突。
* `scripts/flypy_dict_generator_new.py` 双拼词库转换程序，实现了汉字转小鹤双拼，简拼，小鹤音形码，支持输出 `yaml`字典和 `txt` 自定义短语文件，可指定词库内容追加输出，可指定初始词频，或默认源词库的词频。
* 鉴于部分用户使用的 Rime 版本没有自带 emoji 输入方案，本项目提供了 `emoji.schema.yaml` 和 `emoji.dict.yaml` 文件。如果用户已经有这两个文件，可以不使用本项目提供的版本，不过不排除 emoji 输入时的输入法行为会有所不同。
* `easy_en.schema.yaml` 和 `easy_en.dict.yaml` 为作者基于 [easy-en](https://github.com/BlindingDark/rime-easy-en) 项目的英文输入方案修改得到的版本。如果用户已经有这两个文件，可以不使用本项目提供的版本，但英文单词输入的行为应该会有不同。

### 附加功能

* 置顶字词与自定义词组：由 `custom_phrase.txt` 指定，用户可以根据自己的需要修改和添加。其中，自定义词组使用一些（小鹤双拼下）不对应汉字读音的字母组合作为词组开头，例如 `jf`。
* 鉴于使用辅助码之后很少需要翻页，通常只会用到候选的前几项，方案里设置了分号 `;` 用于输入次选，~~斜杠 `/`~~ 单引号 `'` 用于输入第三选项。
* 鉴于双引号比单引号常用，方案里交换了这两个的位置。敲 `'` 输入的是 `“”`，而敲 `"` 输入的是 `‘’`。
  * 如果希望恢复默认的引号输入方式，可以在 `flypy_xhfast.custom.yaml` 里设置“恢复默认引号”。

### 前缀模式

* ~~`oh` 前缀：小鹤双拼键位查询，该功能为双拼初学者提供。例如：敲 `ohian`，可看到韵母 `ian` 对应的按键是 `m`~~。
* ~~`od` 前缀：大写模式，可以输入连续的几个大写字母，不需要大写锁定/`Shift` 键。例如：敲 `odlgpl` 空格，即可输入“LGPL”。~~
* `oy` 前缀：单词模式，不仅可以敲完整的单词，也允许“简写”，省略掉除了首字母以外的所有元音字母（`aeiou`）。例如：敲 `ozelevation` 或者 `ozelvtn` 再加空格，即可输入“elevation”。
  * 该功能基于 [easy-en](https://github.com/BlindingDark/rime-easy-en) 项目，简写特性由 `easy_en.schema.yaml` 文件中设置的拼写运算实现。作者对字典文件进行了精简处理以加快部署速度。如果用户希望使用更完整的字典文件，而同时保留简写特性，可以尝试将 `easy_en.dict.yaml` 文件更换为原项目的版本。
  * 使用的一个小 tips：单词第一次输入时用简写，Rime 会将它的词频记录进用户词典。之后的输入只需要敲完整单词的前半部分，它作为输入过的单词就会排在靠前的位置。
* ~~`oe` 前缀：emoji 模式，注意这里需要按照英文输入。例如：敲 `oelaugh` 空格，即可输入“😂”。~~
* ~~`oq` 前缀：临时全拼模式，用于一些长的词组。例如：敲 `aptlbchqzh` 空格，即可输入“螳螂捕蝉黄雀在后”。~~
  * ~~可以设置“使用扩展词典”，例如加入一些常见古诗词，详见 `flypy_xhfast.custom.yaml` 里的说明。~~
* `oq` 前缀：部件组字模式（类似搜狗拼音的 u 拆字模式），其中部件按照小鹤双拼输入。例如，要输入“犇”字（它可以拆为“牛牛牛”），敲 `oqnqnqnq` 空格即可，并看到这个字的编码是 `bf[nn`。
  * 功能基于 [这篇文章](http://gerry.lamost.org/blog/?p=296003)。一些部件的读音可以参考 [搜狗U模式说明](https://pinyin.sogou.com/help.php?list=3&q=8)（记得要按小鹤双拼输入），此外还有“丶”要敲 `vu`，“廾”要敲 `gs` 等。
  * 如果用户觉得只用笔画比用部件方便，可以在 `flypy_xhfast.custom.yaml` 中设置“oq前缀改用五笔画输入”（即朙月拼音等输入方案下 `` ` `` 前缀对应的模式），用横竖撇捺折（hspnz，点按捺处理）输入汉字。例如：设置后敲 `oqhspn` 空格，即可输入“木”，并看到它的编码是 `mu[ub`。
  * 用户也可以在部件组字模式下同时启用五笔画输入，在 `chaizi_flypy.dict.yaml` 中搜索找到“部件组字模式下启用五笔画”，按要求取消注释即可。不需要修改 `flypy_xhfast.custom.yaml`。
    * 设置后使用的一个小 tips：一些部件的读音可以用五笔画现查，例如想输入“羿”，先用五笔画敲 `oqhps` 查到“廾”的读音为 `gong`（其实查到的是双拼加形方案下的输入码 `gs[hp`、`nm[hp`，读音只看双拼部分，多个读音通常取第一个，也可以都试一遍），于是用部件组字敲 `oqyugs` 即可输入“羿”。
* `/` 前缀：符号模式，具体见 Rime 的系统目录自带的 `symbols.yaml` 文件。例如：敲 `/jt` 按 3，即可输入箭头“←”。此外，为避免与候选项选择混淆，用于输入数字相关符号的 `/2`,`/3`,`/4` 分别用 `/er`,`/san`,`/si` 代替。
* `tf` 前缀：简易 LaTeX 公式。例如：敲 `tl<<f,ff>>ooc0` 空格，即可输入“`$\langle f,\phi\rangle\propto 0$`”。
  * 如果发现该功能无法使用，考虑检查所用的 Rime 框架是否支持 Lua，见 [下方的说明](#关于-lua-支持)。
  * 功能的实现在 `rime.lua` 文件，可以在里面看具体的简写设定，并根据自己的需要增添、删除、修改（语法应该不难理解）。
  * 简写由重复的字符触发，例如 `aa` 变成“`\alpha`”。如果重复的字符是 `jvo` 中的一个，需要接上后面的一个字符触发，例如 `jj;` 变成“`\mapsto`”。
  * 使用 `` ` `` 避免重复字符触发，例如敲 `,,bb` 得到“`\math\beta`”不是我们想要的，敲 ``,,b`b`` 则可以得到“`\mathbb`”。
  * 如果 `` ` `` 两侧的字符不一样，则变成空格。例如，敲 ``\to`0`` 得到“`\to 0`”。
  * 连续的两个 `` ` `` 始终按照一个空格处理。
  * 形如 `x±1` 的上下标较为常见，用 `oo` 接上 `a/s/d/f`（分别代表：上标+1，上标-1，下标+1，下标-1）中的一个，再接上一个字符即可触发。例如，`xoodn` 会变成“`x_{n+1}`”。
  * 在 `rime.lua` 里可以设置“启用特殊符号替换”，默认的替换规则是 `{}` 与 `[]` 互换，`()` 与 `;'` 互换，`_^` 与 `./` 互换，这使常用符号输入更为方便。替换规则可以自行修改。
    * 例如，现在敲 `f.[2n];x'` 可以得到“`f_{2n}(x)`”。
    * 注意这会影响原有的重复字符触发，例如原来 `..` 变成“`\cdot`”，现在是 `__` 变成它。
* 敲 `date`, `time` 可以输入当天的日期 时间, 需要 Lua 支持。

### 额外的键位

* 对 Rime 默认 Emacs 键位的一些补充：
  * `Control+m` 可以替代回车。例如，敲 `yyds` 之后按这个键，输入的就是“yyds”。上面提到过的 `as` 前缀是类似的功能。
  * `Control+w` 可以替代 `Control+退格`，为删一个字的码。例如，敲 `buk` 或者 `buke[dk` 之后，按这个键得到的都是 `bu`，可以继续敲后面的字。如果在词组输入时发现敲错了，可以用这个方式删掉最后的字。

  > Rime 自带的 Emacs 键位包括 `Control+[` 替代 `Esc`，取消当前输入；以及 `Control+h` 替代退格。另外，作者喜欢用 `Control` 键是因为在系统里配置了大写锁定和左 `Control` 交换，这样按起来很舒服。由于这是系统的配置而不是 Rime 的，本文件中没有说明其设置方式。
  >
* 词组的双拼部分输入完成后，可用 `` ` `` 键（Tab 上面那个）逐字追加辅助码。例如，想输入“林纳斯”（默认词库没这个词，但一开始并不知道），可以敲 ``lbnasi`a`sk`q``，这与直接敲 `lb[a na[sk si[q` 是等价的。
  * 这也能用于重码太多的词库已有词。例如，希望输入“适时”一词，敲 `uiui` 发现候选太多，补上最后一个字的形码后 `uiui[oc` 还是没在第一页看到它。此时按 `` ` ``，输入框成为 `ui[ 光标 ui[oc`。补充敲下第一个字的形码部分 `q`，然后按 `Control+e`（或者 `End`）把光标移动到最后，即可看到想要的“适时”一词出现在候选中。
  * 可以在 `flypy_xhfast.custom.yaml` 中将 `` ` `` 改成 `Tab`，`Control+Tab` 或 `]` 等键。
  * 如果不希望自动补充 `[` 符号，其实可以直接改按 `Control+i` 或 `Shift+Right` 移动光标，不必使用 `` ` `` 键。
  * 对于安卓 Trime 用户来说，可能还需要在 `trime.custom.yaml` 里加上这一句（放在 `patch:` 下，注意缩进）：
    ```yaml
    style/horizontal: false
    ```

    ←→ 方向键在 Trime 默认用来移动候选项，这一设定将它改成移动光标（和电脑版的默认行为一致），从而这个补充辅助码的快捷键才能正常工作。* 另外，如果用户不嫌麻烦的话，也可以在第一次按 `` ` `` 键前先按 `Home` 或 `Control+a`（Trime 默认的虚拟键盘中长按 `a` 也行）把光标移动到开头。这样就无需改动 Trime 方向键的功能。
  * 在开启了“直接引导辅助码”的条件下，由于音节组合方式可能出现歧义，这一功能不总能正常运行。
  * 技术层面而言，该功能可能需要 librime 1.6.0 或以上版本才能生效（可以检查一下用户目录下 `installation.yaml` 文件中 `rime_version` 项是多少；一部分 1.5.3 版本也能支持）。如果用户所用系统能获取的最新版 Rime 不满足条件，而自己又有相应能力，可以考虑手动从 [librime 项目](https://github.com/rime/librime/releases) 安装或自行编译。

### 关于 Lua 支持

* 小狼毫（Windows）和鼠须管（MacOS）的最新版本应该都支持 Lua 。
* Trime（Android）要在 [GitHub 页面](https://github.com/osfans/trime) 下载最新测试版（注意不是稳定版）。
  另外，Trime 自带的配置文件可能有缺失，此时可以考虑将电脑版 Rime 系统目录里的配置文件也复制到 Trime 的配置目录中，比如朙月拼音的方案文件和字典文件。
* 对于中州韵（Linux），据说 Arch Linux 源提供的 fcitx5-rime 可以在插件设置里开启 Lua 支持。
  * 其他发行版的用户可以考虑这个 [ibus-rime AppImage](https://github.com/hchunhui/build)。遇到调频失效等问题可以试着删除各 userdb、build、sync 文件夹重新部署/同步。如果这一问题反复出现，或者重启/部署/同步之后经常忘掉之前输入的词，可以尝试在 `flypy_xhfast.custom.yaml` 里开启“用户词典记录为文本格式”，或者看这个 AppImage 有没有发布新版本。
* iRime（iOS）没用过，谁试了或许可以告诉作者（据说这个启用配置文件夹要花钱，而这对使用本项目的配置是必需的）。

### 给进阶用户

这一 Rime 输入方案的制作主要利用了这些文档，希望对 Rime 进行更深入的个性化配置的用户可以参考：

* [GitHub-UserGuide](https://github.com/rime/home/wiki/UserGuide)（访问 GitHub 不稳定的可以用 [Gitee 版 UserGuide](https://gitee.com/lotem/rime-home/wikis/UserGuide)）。其中介绍的 Rime 基本用法适合新手查看，而最开始的“專題”一节还给出了若干有用的链接，供用户在个性化配置时查阅。
  * 其实 UserGuide 只是该项目 wiki 中的一个文件，在网页查看的时候，侧栏里可以看到 wiki 的其他文件，它们也有参考价值。
  * 例如，如果部署后发现没有达到预期的效果，可以考虑从侧栏里点进 `RimeWithSchemata`，按照其中“關於調試”一节下的说明，在日志文件里查找是否有部署出错的提示。
* [设定项详解](https://github.com/LEOYoon-Tsaw/Rime_collections/blob/master/Rime_description.md)。这一文档主要解释了方案文件中各个选项的含义，并且提供了一些配置的例子。

### 存在的问题

* 词库中的字是多音字，音码使用错误， 用户纠错后可解决，

  原因为采用开源的 `pypinyin`项目，存在词组中多音字返回了另外一个注音
* 字库中的形码错误， 因为采用开源的小鹤形码方案， 无法保证形码的准确性和字库音形码的全面性
