---
title: Golang 处理emoji表情无法存入mysql utf8 字段
date: 2021-11-21 22:20:51
tags: Golang,mysql,utf8
---

# Golang 处理emoji表情无法存入mysql utf8 字段

通过代码把不符合规范的字符过滤掉
原理为遍历字符串字符，检查字符在utf8中的长度，因为mysql utf8 仅能处理最大3字节的字符
```
//FilterUtf8SizeGreaterThree mysql数据库utf8只能存储字节3的utf8数据，这里把不符合的字符过滤掉
func FilterUtf8SizeGreaterThree(content string) string {
	bf := bytes.NewBufferString("")
	for _, value := range content {
		if size := utf8.RuneLen(value); size <= 3 {
			bf.WriteRune(value)
		}
	}
	return bf.String()
}
```

还有一种办法是直接把数据库表或对应字段换成utf8mb4