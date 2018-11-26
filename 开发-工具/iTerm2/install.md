### 下载iTerm2: http://www.iterm2.com/downloads.html
### 安装Oh My Bash

- 通过 cat /etc/shells 命令可以查看当前系统可以使用哪些shell
- 通过 echo $SHELL 命令可以查看我们当前正在使用的shell
- 如果当前的shell不是zsh，我们可以通过chsh -s /bin/zsh命令可以将shell切换为shell之zsh，终端重启之后即可生效。
- 安装Oh My ZSH

        sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    
- Oh My Zsh配置主题

    - 查看主题： https://github.com/robbyrussell/oh-my-zsh/wiki/Themes
    - 打开配置文件

        open .zshrc
    
    - 修改主题为agnoster

        1. 设置ZSH_THEME="agnoster"
        2. 安装字体 <a href="https://github.com/powerline/fonts">fonts from Vim-Powerline</a>
        3. 修改默认字体

            进入：iTerm2 > Profiles > Text > Font，修改字体为：Meslo LG S DZ Regular for Powerline。
        4. 设置打开iTerm偏好设置

            iTerm2 > preferences > keys > Hotkey
            设置自定义快捷键
        


### 使用solarized配色方案
   
两种下载方式：
1. <a href="https://ethanschoonover.com/solarized">直接下载</a>
2. git命令下载

        # 下载Solarized
        git clone git://github.com/altercation/solarized.git

        # 进入文件夹
        cd solarized/vim-colors-solarized/colors

        #下面可能要管理员权限
        sudo mkdir -p ~/.vim/colors
        sudo cp solarized.vim ~/.vim/colors/

        # 创建.vimrc文件
        sudo vim ~/.vimrc

        # 复制以下命令
        syntax enable
        set background=dark
        colorscheme solarized

        # 保存并退出
        :wq

3. 应用配色

    iTerm2 > perferences > profiles > colors > 选择solarized颜色



### 保存并退出vim命令

:wq
    