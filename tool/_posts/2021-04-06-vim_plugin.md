---
title: vim插件
tags: vim
typora-root-url: ../..
---

[Vundle](http://github.com/VundleVim/Vundle.vim)

Vim 的插件管理器，可以很方便的管理 Vim 插件，包括安装、更新、卸载。

1. 安装 Vundle 到 Vim 的用户目录，`git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`

2. 配置插件，编辑 [.vimrc](https://gist.github.com/chaiyan0216/e1c52bca824299fbd48f48ff151cb73d) 文件，加入需要的插件，以下以 `Plugin` 开头的行即位需要添加的插件，其余为 Vundle 必须的脚本

   ```shell
   " vundle 环境设置开始
   filetype off
   set rtp+=~/.vim/bundle/Vundle.vim
   call vundle#begin()
   Plugin 'VundleVim/Vundle.vim'
   Plugin 'scrooloose/nerdtree'
   Plugin 'majutsushi/tagbar'
   Plugin 'scrooloose/syntastic'
   Plugin 'valloric/youcompleteme'
   Plugin 'morhetz/gruvbox'
   Plugin 'vim-airline/vim-airline'
   call vundle#end()
   filetype plugin indent on
   " vundle 环境设置结束
   ```

3. 启动 Vim，执行 `:PluginInstall` ；或者直接在命令行执行 `vim +PluginInstall +qall`

4. 安装完毕后，可以通过 `:h vundle` 查看帮助文档

5. 所有安装的插件都在 `~/.vim/bundle/` 路径下，可以比较直观的看出来装了哪些插件



### 推荐插件

- [NERDTree](https://github.com/preservim/nerdtree)

NERDTree 是 Vim 的文件系统浏览器，安装该插件后可以在 Vim 里进行文件系统的可视化浏览，效果非常好，可以非常方便的增删改文件。

![NERDTree](https://github.com/preservim/nerdtree/raw/master/screenshot.png)

#### 常用快捷键

```shell
# 光标
Ctrl + w + h		光标 focus 左侧树形目录
Ctrl + w + l		光标 focus 右侧文件窗口
Ctrl + w + w		光标 focus 左右切换

# 编辑 & 预览
o			打开文件，并跳转到编辑窗口
go		打开文件，但不跳转到编辑窗口
t			打开文件到新 tab 页，并跳转到编辑窗口
T			打开文件到新 tab 页，但不跳转到编辑窗口
i			水平分割打开文件
gi		水平分割打开文件，不跳转
s			竖直分割打开文件
gs		竖直分割打开文件，不跳转

# 操作
?			帮助菜单
m			打开系统操作菜单，可以复制、移动、删除、以系统 app 打开当前文件等
gt		后一个 tab
gT		前一个 tab
```

#### 有用的配置

以下配置放在 `.vimrc` 中。

```shell
# NERDTree 打开和关闭快捷键映射
nmap <F3> :NERDTreeToggle<CR>

# 进入 vim 就打开 NERDTree
autocmd vimenter * NERDTree

# 当 NERDTree 是最后一个窗口时，关闭 vim
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif

# 显示隐藏文件
let g:NERDTreeShowHidden=1
```



- [Tagbar](https://github.com/preservim/tagbar)

Tagbar 用来显示文件大纲，依赖 `Ctags` 

```shell
# Ctags 路径
let g:Tlist_Ctags_Cmd='/usr/local/Cellar/ctags/5.8_1/bin/ctags'
# 触发快捷键
nmap <F8> :TagbarToggle<CR>
```



- [Syntastic](https://github.com/vim-syntastic/syntastic)

语法检查，支持多种语言。

![Syntastic](https://github.com/vim-syntastic/syntastic/raw/master/_assets/screenshot_1.png)

官网推荐配置如下。

```shell
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*

let g:syntastic_always_populate_loc_list = 1
# 自动打开和关闭 error list
let g:syntastic_auto_loc_list = 1
# 打开文件时是否进行检查
let g:syntastic_check_on_open = 1
# 保存文件时是否进行检查
let g:syntastic_check_on_wq = 0
```



- [gruvbox](https://github.com/morhetz/gruvbox)

一款比较火的主题。

![Dark mode](https://camo.githubusercontent.com/a05028ef4dae5865098c508fc9f686b211f510198f07e6a5636734dbac618b30/687474703a2f2f692e696d6775722e636f6d2f476b496c38466e2e706e67)

```shell
colorscheme gruvbox
set background=dark
```



- [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)

代码补全大杀器，没怎么配置，因为依赖比较大，安装会慢很多。

![YouCompleteMe](https://camo.githubusercontent.com/3b874ea8b78bd8264d63ff63a60f0b777e790d880e175f7277e9a34b2139c618/68747470733a2f2f692e696d6775722e636f6d2f304f50346f6f642e676966)



- [vim-airline](https://github.com/vim-airline/vim-airline)

状态栏增强。

```shell
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'
```

