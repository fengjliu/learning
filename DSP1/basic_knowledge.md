# 系统传输函数和冲击响应概念
https://www.zhihu.com/question/34789389

# 由S参数计算传输函数并画出相位曲线
```
sparams = sparameters('filter1030.s2p')
tf=s2tf(sparams)
u=angle(tf)
plot(unwrap(u))
```

