# 单字符匹配

| 字符 | 功能                                  |
| ---- | ------------------------------------- |
| .    | 匹配任意一个字符,除了\n, \.匹配点本身 |
| []   | 匹配[]中列举的字符                    |
| \d   | 匹配数字 0-9                          |
| \D   | 匹配非数字                            |
| \s   | 匹配空白,空格,tab 键                  |
| \S   | 匹配非空白                            |
| \w   | 匹配单词字符,a-z,A-Z,0-9,\_           |
| \W   | 匹配非单词字符                        |

# 数量匹配

| 字符  | 功能                                 |
| ----- | ------------------------------------ |
| \*    | 匹配前一个规则的字符出现 0 至无数次  |
| +     | 匹配前一个规则的字符出现 1 至无数次  |
| ?     | 匹配前一个规则的字符出现 0 次或 1 次 |
| {m}   | 匹配前一个规则的字符出现 m 次        |
| {m,}  | 匹配前一个规则的字符出现至少 m 次    |
| {m,n} | 匹配前一个规则的字符出现 m 到 n 次   |

# 边界匹配

| 字符 | 功能               |
| ---- | ------------------ |
| ^    | 匹配字符串开头     |
| $    | 匹配字符串结尾     |
| \b   | 匹配一个单词的边界 |
| \B   | 匹配非单词的边界   |

# 分组匹配

| 字符 | 功能                     |
| ---- | ------------------------ |
| \|   | 匹配左右任意一个表达式   |
| ()   | 将括号中字符作为一个分组 |