# fish设置提示符
```sh
function fish_prompt -d "Write out the prompt"
  printf '%s%s%s>' (set_color $fish_color_cwd) (prompt_pwd) (set_color normal)
end
```
# fish设置环境变量
```sh
set -x PATH /opt/demo/bin /home/guest/bin $PATH
```
# 设置alias
```sh
abbr -a v nvim
```