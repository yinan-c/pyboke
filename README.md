# PyBoke

Static Blog Generator (极简博客生成器)

- 使用过程极简
- 功能极简，代码极简

- 文章文件名、图片文件名由用户设定，只允许使用 0-9, a-z, A-Z, _(下划线), -(短横线), .(点)。
- 文章文件名、图片文件名相当于 ID

## 创建一个新博客


1. `mkdir my-blog` (新建一个空文件夹)
2. `cd my-blog` (进入空文件夹内)
3. `boke init` (如果检测到文件夹不是空的，会拒绝初始化)

然后可以在当前文件内看到以下文件与文件夹：

- articles (文章文件 (markdown 格式, .md 后缀名) 请放在这里)
- articles/pics (供 markdown 文件使用的本地图片)
- articles/metadata (与 markdown 文件一一对应的 toml 文件)
- drafts (待发布的草稿放在这里)
- output (程序生成的 HTML, RSS 等文件将会输出到该文件夹)
- templates (Jinja2模板 与 CSS文件)
- boke.toml (博客名称、作者名称等等)

请用文本编辑器打开 boke.toml 填写博客名称、作者名称等。
（用 Jinja2 生成 toml?）

建议尽量少用图片，本程序未为大量图片做优化。

## 添加文章

- 文件后缀必须是 ".md", 文件内容必须采用 Markdown 格式, 必须采用 utf-8 编码。
- 文件名只能使用 0-9, a-z, A-Z, _(下划线), -(短横线), .(点)。
- 把 md 文件放入 articles 文件夹，执行 `boke render articles/filename`,
  会在 **articles/metadata** 文件夹里生成 TOML 文件，
  在 **output** 文件夹生成 HTML 文件。
- 其中, TOML 里有文章的标题、作者、创建日期、修改日期等信息。
- 大多数情况下你都可以忘记 articles/metadata 里的 toml 文件，不需要修改它。

## 修改文章内容

- 直接修改 articles 里的 md 文件。
- 然后执行 `boke render articles/filename`, 即可自动更新指定文件的 toml 和 html
- 该命令与前面所述 "添加文章" 的命令是一样的，作用都是渲染 toml 和 html

## 批量处理

- 执行 `boke render -all` 可自动检查全部文件是否被修改过，如有则更新其 toml 和 html
  (如有新文章，也会自动生成 toml 和 html)。

## 强制渲染

使用前述的 `boke render` 命令时，如果文章内容无变化，会自动忽略。  
也就是说，如果只修改 toml 的内容，不修改 markdown 文件的内容，就不会触发渲染处理。

因此，如果想在不修改文章内容的情况下，修改文章的作者或日期，就需要强制渲染。

- `boke render -force articles/filename` 强制渲染指定的一篇文章。
- `boke render --title-index` 强制渲染全部标题索引（在发现未触发更新时使用）
- `boke render -force -all` 强制渲全部文章。

大多数情况下不需要强制渲染，但有一种情况：修改了 blog.toml 里的博客名称、作者名称
等信息后，需要执行 `boke render -force -all` 强制渲全部文章。

## 删除文章

有两种方法：

1. 直接删除 articles 文件夹里的文件，然后执行 `boke render -all`
2. 使用命令 `boke delete articles/filename`

## 草稿

如上文所示，添加文章只需要把 markdown 文件放进 articles 文件夹即可，
操作非常简单明瞭。

但考虑到有时文章写到一半不想发布，需要一个存放草稿的地方，因此提供了一个
drafts 文件夹，草稿可以放在这里。（但其实草稿放在硬盘里的其它文件夹也行，
这个 drafts 文件夹只是提供方便，并非强制要求）

### `boke new drafts/abc.md`

该命令用来新建一个 markdown 文件，并确保其采用 UTF-8 编码。  
这个命令也只是提供方便，你完全可以不使用该命令，而是自己用其它方法创建文件。

### `boke post drafts/abc.md`

该命令的作用是把 drafts 文件夹里的指定文件移动到 articles 文件夹里，并对其执行渲染
(生成 toml 和 html 文件)。

这个命令也只是提供方便，你完全可以不使用该命令，而是自己移动文件，然后自己执行
`boke render` 命令。

### 提醒：小心覆盖文件

在终端用 `cp` 命令复制文件到 articles 文件夹时，如有同名文件，会直接覆盖。

因此建议用 `boke post` 命令，可以防止覆盖。  
(注意: 由于涉及文件移动, `boke post` 只能用来发布 drafts 文件夹里的文章。)

## Themes (主题)

- 主题名称只能由英文字母组成，不分大小写，不可包含空格。
