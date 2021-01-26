# 使用 dotfiles 来管理你的配置文件

> 可以实践结构思考力的方式组织

通过使用 dotfiles 管理我们的配置，可以使配置文件更加迁移以及复用。

想象一个场景：

1. 以前都是使用自己的笔记本工作，入职新公司要使用新的环境，怎么快速的搭建自己的 Shell 环境?
2. 大家提交的代码，总是会出现格式的冲突，如何让大家有使用同样一套配置？ 
3. 我有一些小的 Shell 的技巧想和大家分享，什么方式更好传承？

## Dotfiles 个人配置管理最佳方案

### Dotfiles 就是配置文件集合

我们在 linux, mac 开发的时候，在初始化环境的时候需要很多配置，例如：

- `zsh`
- `zsh-plugins` 
- `vim`
- `git`
-`tmux`
- `alacritty`
- `.i3`

### Dotfiles 通过 git 管理更高效 

1. 在根目录下创建 .dotfiles 目录

```bash
#
mkdir ~/.dotfiles
cd ~/.dotfiles && git init
```

2. 移动相关的点文件到目录中

```bash
mv ~/.zshrc ~/.dotfiles/
mv ~/.tmux.conf ~/.dotfiles/
mv ~/.vim ~/.dotfiles/
mv ~/.gitconfig ~/.dotfiles/
mv ~/.gitignore ~/.dotfiles/
```

3. 使用脚本重新 ln 到根目录

```
cd ~/.dotfiles
mkdir script && cd script
wget https://raw.githubusercontent.com/cizel/dotfiles/master/script/bootstrap.sh
cd ..
sh script/bootstrap.sh
```

4. 使用 github 同步配置

```
git remote add origin git@github.com:{username}/dotfiles.git
git add .
git push -u origin master
```


## Dotfiles 常用配置推荐

**驱动 bootstrap.sh**

```
#!/bin/bash

# this symlinks all the dotfiles (and .vim/) to ~/
# it also symlinks ~/bin for easy updating

# this is safe to run multiple times and will prompt you about anything unclear


# this is a messy edit of alrra's nice work here:
#   https://raw.githubusercontent.com/alrra/dotfiles/master/os/create_symbolic_links.sh
#   it should and needs to be improved to be less of a hack.



# jump down to line ~140 for the start.


#
# utils !!!
#


answer_is_yes() {
    [[ "$REPLY" =~ ^[Yy]$ ]] \
        && return 0 \
        || return 1
}

ask() {
    print_question "$1"
    read
}

ask_for_confirmation() {
    print_question "$1 (y/n) "
    read -n 1
    printf "\n"
}

ask_for_sudo() {

    # Ask for the administrator password upfront
    sudo -v

    # Update existing `sudo` time stamp until this script has finished
    # https://gist.github.com/cowboy/3118588
    while true; do
        sudo -n true
        sleep 60
        kill -0 "$$" || exit
    done &> /dev/null &

}

cmd_exists() {
    [ -x "$(command -v "$1")" ] \
        && printf 0 \
        || printf 1
}

execute() {
    $1 &> /dev/null
    print_result $? "${2:-$1}"
}

get_answer() {
    printf "$REPLY"
}

get_os() {

    declare -r OS_NAME="$(uname -s)"
    local os=""

    if [ "$OS_NAME" == "Darwin" ]; then
        os="osx"
    elif [ "$OS_NAME" == "Linux" ] && [ -e "/etc/lsb-release" ]; then
        os="ubuntu"
    fi

    printf "%s" "$os"

}

is_git_repository() {
    [ "$(git rev-parse &>/dev/null; printf $?)" -eq 0 ] \
        && return 0 \
        || return 1
}

mkd() {
    if [ -n "$1" ]; then
        if [ -e "$1" ]; then
            if [ ! -d "$1" ]; then
                print_error "$1 - a file with the same name already exists!"
            else
                print_success "$1"
            fi
        else
            execute "mkdir -p $1" "$1"
        fi
    fi
}

print_error() {
    # Print output in red
    printf "\e[0;31m  [✖] $1 $2\e[0m\n"
}

print_info() {
    # Print output in purple
    printf "\n\e[0;35m $1\e[0m\n\n"
}

print_question() {
    # Print output in yellow
    printf "\e[0;33m  [?] $1\e[0m"
}

print_result() {
    [ $1 -eq 0 ] \
        && print_success "$2" \
        || print_error "$2"

    [ "$3" == "true" ] && [ $1 -ne 0 ] \
        && exit
}

print_success() {
    # Print output in green
    printf "\e[0;32m  [✔] $1\e[0m\n"
}






#
# actual symlink stuff
#


# finds all .dotfiles in this folder
declare DOTFILES_PATH=$HOME/.dotfiles

declare -a FILES_TO_SYMLINK=$(find $DOTFILES_PATH -type f -maxdepth 1 -name ".*" -not -name .DS_Store -not -name .git -not -name .osx | sed -e 's|'$DOTFILES_PATH'/||' | sed -e 's|//|/|' | sed -e 's|\./\.|.|')
FILES_TO_SYMLINK="$FILES_TO_SYMLINK .vim .config .hammerspoon" # add in vim and the binaries


# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

init() {

    execute "git submodule update --init"

    # update .tmux.conf
    execute "ln -f $DOTFILES_PATH/.tmux/.tmux.conf $DOTFILES_PATH"
}

main() {

    local i=""
    local sourceFile=""
    local targetFile=""

    for i in ${FILES_TO_SYMLINK[@]}; do

        sourceFile="$DOTFILES_PATH/$i"
        targetFile="$HOME/$(printf "%s" "$i" | sed "s/.*\/\(.*\)/\1/g")"

        if [ -e "$targetFile" ]; then
            if [ "$(readlink "$targetFile")" != "$sourceFile" ]; then

                ask_for_confirmation "'$targetFile' already exists, do you want to overwrite it?"
                if answer_is_yes; then
                    rm -rf "$targetFile"
                    execute "ln -fs $sourceFile $targetFile" "$targetFile → $sourceFile"
                else
                    print_error "$targetFile → $sourceFile"
                fi

            else
                print_success "$targetFile → $sourceFile"
            fi
        else
            execute "ln -fs $sourceFile $targetFile" "$targetFile → $sourceFile"
        fi

    done

}

init
main
```

## 相关链接 

- [cizel dotfiles 配置](https://github.com/cizel/dotfiles)
