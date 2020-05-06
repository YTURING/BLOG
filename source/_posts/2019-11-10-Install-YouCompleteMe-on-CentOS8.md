---
title: Install YouCompleteMe on CentOS8
date: 2019-11-10 19:48:42
categories:
- [Linux]
tags:
- [Vim]
---
能用Vim搭建自己的IDE吗？当然可以。
接下来记录一下自己是怎么在CentOS8上安装YouCompleteMe的以及遇到的问题，希望能帮到遇到相似问题的。
1. 首先安装预备环境  

        sudo dnf install cmake gcc-c++ make git python3-devel  //CentOS系
        sudo apt install build-essential cmake git python3-dev  //Debian系

2. 然后安装[Vundle](https://github.com/VundleVim/Vundle.vim),Vundle是Vim的插件管理器。  
  
        git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim  

3. 接下来  `vim ~/.vimrc` ,写入如下内容：  
          
        set backspace=2
        set nu
        set nocompatible              " be iMproved, required
        filetype off                  " required
        set shiftwidth=4
        set expandtab
        set softtabstop=4
        set t_Co=256
        "syntax on
        highlight PMenu ctermfg=14 ctermbg=235 guifg=#ff005f  guibg=black
        highlight PMenuSel ctermfg=231 ctermbg=235 guifg=#ff005f   guibg=black
        " set the runtime path to include Vundle and initialize
        set rtp+=~/.vim/bundle/Vundle.vim
        call vundle#begin()
        " alternatively, pass a path where Vundle should install plugins
        "call vundle#begin('~/some/path/here')
        
        " let Vundle manage Vundle, required
        Plugin 'VundleVim/Vundle.vim'
        
        " The following are examples of different formats supported.
        " Keep Plugin commands between vundle#begin/end.
        " plugin on GitHub repo
        Plugin 'tpope/vim-fugitive'
        " plugin from http://vim-scripts.org/vim/scripts.html
        Plugin 'L9'
        " Git plugin not hosted on GitHub
        Plugin 'git://git.wincent.com/command-t.git'
        " git repos on your local machine (i.e. when working on your own plugin)
        "Plugin 'file:///home/gmarik/path/to/plugin'
        " The sparkup vim script is in a subdirectory of this repo called vim.
        " Pass the path to set the runtimepath properly.
        Plugin 'Valloric/YouCompleteMe'
        Plugin 'Raimondi/delimitMate'
        Plugin 'scrooloose/syntastic'
        Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
        " Install L9 and avoid a Naming conflict if you've already installed a
        " different version somewhere else.
        
        " All of your Plugins must be added before the following line
        call vundle#end()            " required
        filetype plugin indent on    " required
        " To ignore plugin indent changes, instead use:
        "filetype plugin on
        "
        " Brief help
        " :PluginList       - lists configured plugins
        " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
        " :PluginSearch foo - searches for foo; append `!` to refresh local cache
        " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
        nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR>
        "let g:ycm_key_invoke_completion = '<c-j>'
        let g:jedi#force_py_version=3.6.8
        let g:ycm_python_binary_path = '/usr/bin/python3'
        let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/.ycm_extra_conf.py'
        
        set completeopt=longest,menu
        autocmd InsertLeave * if pumvisible() == 0|pclose|endif
        inoremap <expr> <CR>       pumvisible() ? "\<C-y>" : "\<CR>"
        "inoremap <expr> <Down>     pumvisible() ? "\<C-n>" : "\<Down>"
        "inoremap <expr> <Up>       pumvisible() ? "\<C-p>" : "\<Up>"
        "inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
        "inoremap <expr> <PageUp>   pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"  
        let g:ycm_key_list_select_completion = ['<Down>']
        let g:ycm_key_list_previous_completion = ['<Up>']
        "let g:ycm_confirm_extra_conf=0
        let g:ycm_collect_identifiers_from_tags_files=1
        let g:ycm_min_num_of_chars_for_completion=2
        let g:ycm_cache_omnifunc=0
        let g:ycm_seed_identifiers_with_syntax=1
        nnoremap <F5> :YcmForceCompileAndDiagnostics<CR>
        inoremap <leader><leader> <C-x><C-o>
        let g:ycm_complete_in_comments = 1
        let g:ycm_complete_in_strings = 1
        let g:ycm_collect_identifiers_from_comments_and_strings = 0  
        hi MatchParen ctermbg=62  guibg=lightblue  
 
4. 上面是一些自动补全及颜色的配置项，我个人的喜欢的配置，可以自行修改。然后在命令行输入 `vim` ，进入vim的命令行模式以后输入 `:PluginInstall` ,即可进入插件安装，有些插件可能对国内网络不是很友好，可开启命令行代理再进入安装。  
5. 最后是 YouCompleteMe 的安装，  
  
        cd ~/.vim/bundle/YouCompleteMe  
        python3 ./install.py --clang-completer --ts-completer --go-completer  
        cp ~/.vim/bundle/YouCompleteMe/third_party/ycmd/examples/.ycm_extra_conf.py   ~/.vim/bundle/YouCompleteMe/  
        --clang-completer 是 C 、C++、Python等语言的补全，--ts-complete 是JavaScript、TypeScript的补全,--go-completer是Go语言的补全。  
        在安装的时候你可能会遇到这样的问题：  

        go: golang.org/x/sync@v0.0.0-20190423024810-112230192c58: unrecognized import path "golang.org/x/sync" (https fetch: Get https://golang.org/x/sync?go-get=1: dial tcp 216.239.37.1:443: connect: connection refused)
        go: golang.org/x/net@v0.0.0-20190620200207-3b0461eec859: unrecognized import path "golang.org/x/        net" (https fetch: Get https://golang.org/x/net?go-get=1: dial tcp 216.239.37.1:443: connect:         connection refused)
        go: golang.org/x/xerrors@v0.0.0-20190717185122-a985d3407aa7: unrecognized import path "golang.org/        x/xerrors" (https fetch: Get https://golang.org/x/xerrors?go-get=1: dial tcp 216.239.37.1:443:         connect: connection refused)
        go: error loading module requirements  
          
        许多人说开代理就可以解决，但是我开了代理还是报错，最后试出这样可以：  

        git clone https://github.com/golang/sync.git   ~/.vim/bundle/YouCompleteMe/third_party/ycmd/third_party/go/src/golang.org/x/  
        git clone https://github.com/golang/net.git   ~/.vim/bundle/YouCompleteMe/third_party/ycmd/third_party/go/src/golang.org/x/  
        git clone https://github.com/golang/xerrors.git   ~/.vim/bundle/YouCompleteMe/third_party/ycmd/third_party/go/src/golang.org/x/  
        export GOPATH=/root/.vim/bundle/YouCompleteMe/third_party/ycmd/third_party/go  
        python3 ./install.py --clang-completer --ts-completer --go-completer  
        cp ~/.vim/bundle/YouCompleteMe/third_party/ycmd/examples/.ycm_extra_conf.py   ~/.vim/bundle/YouCompleteMe/  
结束啦，感谢那些为vim的使用体验提升不断努力的大佬们！

