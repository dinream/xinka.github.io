_when_：有几种状态 composing、has_menu、paging

_accept_：控制接受的按键 minus、equal,、period、comma、bracketleft、bracketright

_send_：控制动作 Page_Up、Page_Down、Escape(清空输入码)

`# # 快捷键# - { when: has_menu, accept: semicolon, send: 2 } # ":" (分号)选择第 2 个候选词# - { when: has_menu, accept: apostrophe, send: 3 } # "'" (引号)选择第 3 个候选词# - { when: composing, accept: Shift+Tab, send: Shift+Left } # "Shift+Tab" 键向左选拼音分词# - { when: composing, accept: Control+a, send: Home } # "Control+a" 光标移至首# - { when: composing, accept: Control+e, send: End } # "Control+e" 光标移至尾# - { when: composing, accept: Control+g, send: Escape } # "Control+g" 清码# - { when: composing, accept: Return, send: Escape } # "Return" 回车清码# - { when: always, accept: Control+Shift+1, select: .next } # 切换输入方案# - { when: always, accept: Control+Shift+2, toggle: ascii_mode } # 中/英文切换# - { when: always, accept: Control+Shift+3, toggle: full_shape } # 全角/半角切换# - { when: always, accept: Control+Shift+4, toggle: simplification } # 繁简体切换# - { when: always, accept: Control+Shift+5, toggle: extended_charset } # 通用/增广切换（显示生僻字）# - { when: composing, accept: Control+b, send: Left } # "Control+b" 移动光标# - { when: composing, accept: Control+f, send: Right } # "Control+f" 向右选择候选词# - { when: composing, accept: Control+h, send: BackSpace } # "Control+h" 删除输入码`

https://gist.github.com/lotem/2316704