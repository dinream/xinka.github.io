[Markdown数学公式语法 - 简书 (jianshu.com)](https://www.jianshu.com/p/383e8149136c)

# 一、简单分类

### 1. 行内公式

将公式插入到本行内，符号：`$公式内容$`，如：`$xyz$`  
![xyz](https://math.jianshu.com/math?formula=xyz)

### 2. 行间公式

将公式插入到新的一行内，并且居中，符号：`$公式内容$`，如：`$$xyz$$`  
![xyz](https://math.jianshu.com/math?formula=xyz)

# 二、上标、下标与组合

### 1. 上标符号，符号：`^`，如：`$x^4$` ![x^4](https://math.jianshu.com/math?formula=x%5E4)

### 2. 下标符号，符号：`_`，如：`$x_1$` ![x_1](https://math.jianshu.com/math?formula=x_1)

### 3. 组合符号，符号：`{}`，如：![<sup>{16}_{8}O</sup>{2+}_{2}](https://math.jianshu.com/math?formula=%3Csup%3E%7B16%7D_%7B8%7DO%3C%2Fsup%3E%7B2%2B%7D_%7B2%7D)

默认情况下，上、下标符号仅仅对下一个组起作用。一个组即单个字符或者使用{}（大括号） 包裹起来的内容。如果使用`$10^10$`表示的是![10^10](https://math.jianshu.com/math?formula=10%5E10),而`$10^{10}$` 才可以表示为![10^{10}](https://math.jianshu.com/math?formula=10%5E%7B10%7D)。同时，大括号还能消除二义性，如：`$x^5^6$` 将得到一个错误，必须使用大括号来界定^的结合性，如:`${x^5}^6$`表示的![{x^5}^6](https://math.jianshu.com/math?formula=%7Bx%5E5%7D%5E6)：或者用`$x^{5^6}$`表示的![x^{5^6}](https://math.jianshu.com/math?formula=x%5E%7B5%5E6%7D)。

# 三、括号

### 1.小括号与方括号

用原始的( ) ，[ ] 即可，如`(2+3)[4+4]`可表示：![(2+3)[4+4]](https://math.jianshu.com/math?formula=(2%2B3)%5B4%2B4%5D)。  
使用\left(或\right)使符号大小与邻近的公式相适应（该语句适用于所有括号类型），如\left(\frac{x}{y}\right)可表示![\left(\frac{x}{y}\right)](https://math.jianshu.com/math?formula=%5Cleft(%5Cfrac%7Bx%7D%7By%7D%5Cright))

### 2. 大括号

由于大括号{} 被用于分组，因此需要使用{和}表示大括号，也可以使用\lbrace 和\rbrace来表示。如{ab}或\lbrace ab\rbrace表示:![\{ab\}](https://math.jianshu.com/math?formula=%5C%7Bab%5C%7D)

### 3. 尖括号

区分于小于号和大于号，使用\langle 和\rangle 表示左尖括号和右尖括号。如\langle x \rangle表示：![\langle x \rangle](https://math.jianshu.com/math?formula=%5Clangle%20x%20%5Crangle)

# 三、取整

### 1. 上取整

使用\lceil 和 \rceil 表示。 如，\lceil x \rceil表示为：![\lceil x \rceil](https://math.jianshu.com/math?formula=%5Clceil%20x%20%5Crceil)

### 2. 下取整

使用\lfloor 和 \rfloor 表示。如，\lfloor x \rfloor表示为:![\lfloor x \rfloor](https://math.jianshu.com/math?formula=%5Clfloor%20x%20%5Crfloor)

# 四、求和\积分\连乘

### 1.求和

`\sum` 用来表示求和符号，其下标表示求和下限，上标表示上限。如:  
`$\sum_{r=1}^n$`表示：![\sum_{r=1}^n](https://math.jianshu.com/math?formula=%5Csum_%7Br%3D1%7D%5En)

### 2. 积分

`\int` 用来表示积分符号，同样地，其上下标表示积分的上下限。如，`$\int_{r=1}^\infty$`表示:![\int_{r=1}^\infty](https://math.jianshu.com/math?formula=%5Cint_%7Br%3D1%7D%5E%5Cinfty)  
多重积分同样使用`\int` ，通过 i 的数量表示积分导数：  
如：  
`$\iint$` 表示为：![\iint](https://math.jianshu.com/math?formula=%5Ciint)  
`$\iiint$` 表示为：![\iiint](https://math.jianshu.com/math?formula=%5Ciiint)

### 3. 连乘

`$\prod {a+b}$` 表示：![\prod {a+b}](https://math.jianshu.com/math?formula=%5Cprod%20%7Ba%2Bb%7D)  
`$\prod_{i=1}^{K}$` 表示：![\prod_{i=1}^{K}](https://math.jianshu.com/math?formula=%5Cprod_%7Bi%3D1%7D%5E%7BK%7D)  
`$$\prod_{i=1}^{K}$$`表示（注意是行间公式）：![\prod_{i=1}^{K}](https://math.jianshu.com/math?formula=%5Cprod_%7Bi%3D1%7D%5E%7BK%7D)

### 4. 其他

与此类似的符号还有，  
`$\prod$` ：![\prod](https://math.jianshu.com/math?formula=%5Cprod)  
`$\bigcup$`：![\bigcup](https://math.jianshu.com/math?formula=%5Cbigcup)  
`$\bigcap$` ：![\bigcap](https://math.jianshu.com/math?formula=%5Cbigcap)  
`$arg\,\max_{c_k}$`：![arg\,\max_{c_k}](https://math.jianshu.com/math?formula=arg%5C%2C%5Cmax_%7Bc_k%7D)  
`$arg\,\min_{c_k}$`：![arg,\min_{c_k}](https://math.jianshu.com/math?formula=arg%2C%5Cmin_%7Bc_k%7D)  
`$\mathop {argmin}_{c_k}$`：![\mathop {argmin}_{c_k}](https://math.jianshu.com/math?formula=%5Cmathop%20%7Bargmin%7D_%7Bc_k%7D)  
`$\mathop {argmax}_{c_k}$`：![\mathop {argmax}_{c_k}](https://math.jianshu.com/math?formula=%5Cmathop%20%7Bargmax%7D_%7Bc_k%7D)  
`$\max_{c_k}$`：![\max_{c_k}](https://math.jianshu.com/math?formula=%5Cmax_%7Bc_k%7D)  
`$\min_{c_k}$`：![\min_{c_k}](https://math.jianshu.com/math?formula=%5Cmin_%7Bc_k%7D)

# 五、分式与根式

### 1. 分式

第一种，使用`\frac ab`，表示为:![\frac ab](https://math.jianshu.com/math?formula=%5Cfrac%20ab) ，`\frac`作用于其后的两个组a ，b ，结果为。如果你的分子或分母不是单个字符，请使用{…}来分组，比如`$\frac {a+c+1}{b+c+2}$`表示:![\frac {a+c+1}{b+c+2}](https://math.jianshu.com/math?formula=%5Cfrac%20%7Ba%2Bc%2B1%7D%7Bb%2Bc%2B2%7D)  
第二种，使用\over来分隔一个组的前后两部分，如`${a+1\over b+1}$`：![{a+1\over b+1}](https://math.jianshu.com/math?formula=%7Ba%2B1%5Cover%20b%2B1%7D)

### 2. 连分数

书写连分数表达式时，请使用`\cfrac`代替`\frac`或者`\over`两者效果对比如下：  
`\frac` 表示如下：

```ruby
 $$x=a_0 + \frac {1^2}{a_1 + \frac {2^2}{a_2 + \frac {3^2}{a_3 + \frac {4^2}{a_4 + ...}}}}$$
```

显示如下：  
![x=a_0 + \frac {1^2}{a_1 + \frac {2^2}{a_2 + \frac {3^2}{a_3 + \frac {4^2}{a_4 + ...}}}}](https://math.jianshu.com/math?formula=x%3Da_0%20%2B%20%5Cfrac%20%7B1%5E2%7D%7Ba_1%20%2B%20%5Cfrac%20%7B2%5E2%7D%7Ba_2%20%2B%20%5Cfrac%20%7B3%5E2%7D%7Ba_3%20%2B%20%5Cfrac%20%7B4%5E2%7D%7Ba_4%20%2B%20...%7D%7D%7D%7D)  
`\cfrac`表示如下：

```ruby
$$x=a_0 + \cfrac {1^2}{a_1 + \cfrac {2^2}{a_2 + \cfrac {3^2}{a_3 + \cfrac {4^2}{a_4 + ...}}}}$$
```

显示如下：  
![x=a_0 + \cfrac {1^2}{a_1 + \cfrac {2^2}{a_2 + \cfrac {3^2}{a_3 + \cfrac {4^2}{a_4 + ...}}}}](https://math.jianshu.com/math?formula=x%3Da_0%20%2B%20%5Ccfrac%20%7B1%5E2%7D%7Ba_1%20%2B%20%5Ccfrac%20%7B2%5E2%7D%7Ba_2%20%2B%20%5Ccfrac%20%7B3%5E2%7D%7Ba_3%20%2B%20%5Ccfrac%20%7B4%5E2%7D%7Ba_4%20%2B%20...%7D%7D%7D%7D)

### 3.根式

根式使用`\sqrt` 来表示。  
如开4次方：`$\sqrt[4]{\frac xy}$` 可表示：![\sqrt[4]{\frac xy}](https://math.jianshu.com/math?formula=%5Csqrt%5B4%5D%7B%5Cfrac%20xy%7D)  
开平方：`$\sqrt {a+b}$`可表示：![\sqrt {a+b}](https://math.jianshu.com/math?formula=%5Csqrt%20%7Ba%2Bb%7D)

# 六、多行表达式

### 1. 分类表达式

定义函数的时候经常需要分情况给出表达式，使用\begin{cases}…\end{cases} 。其中：  
使用`\\` 来分类，  
使用`&`指示需要对齐的位置，  
使用`\ +space`表示空格。  
如：

```ruby
$$
f(n)
\begin{cases}
\cfrac n2, &if\ n\ is\ even\\
3n + 1, &if\  n\ is\ odd
\end{cases}
$$
```

表示:  
![f(n) \begin{cases} \cfrac n2, &if\ n\ is\ even\\ 3n + 1, &if\ n\ is\ odd \end{cases}](https://math.jianshu.com/math?formula=f(n)%20%5Cbegin%7Bcases%7D%20%5Ccfrac%20n2%2C%20%26if%5C%20n%5C%20is%5C%20even%5C%5C%203n%20%2B%201%2C%20%26if%5C%20n%5C%20is%5C%20odd%20%5Cend%7Bcases%7D)  
以及:

```ruby
$$
L(Y,f(X)) =
\begin{cases}
0, & \text{Y = f(X)}  \\
1, & \text{Y $\neq$ f(X)}
\end{cases}
$$
```

表示:  
![L(Y,f(X)) = \begin{cases} 0, & \text{Y = f(X)} \\ 1, & \text{Y $\neq$ f(X)} \end{cases}](https://math.jianshu.com/math?formula=L(Y%2Cf(X))%20%3D%20%5Cbegin%7Bcases%7D%200%2C%20%26%20%5Ctext%7BY%20%3D%20f(X)%7D%20%5C%5C%201%2C%20%26%20%5Ctext%7BY%20%24%5Cneq%24%20f(X)%7D%20%5Cend%7Bcases%7D)

如果想分类之间的垂直间隔变大，可以使用`\\[2ex]`代替`\\`来分隔不同的情况。`(3ex,4ex` 也可以用，`1ex`相当于原始距离）。如下所示：

```ruby
$$
L(Y,f(X)) =
\begin{cases}
0, & \text{Y = f(X)} \\[5ex]
1, & \text{Y $\neq$ f(X)}
\end{cases}
$$
```

表示：  
![L(Y,f(X)) = \begin{cases} 0, & \text{Y = f(X)} \\[5ex] 1, & \text{Y $\neq$ f(X)} \end{cases}](https://math.jianshu.com/math?formula=L(Y%2Cf(X))%20%3D%20%5Cbegin%7Bcases%7D%200%2C%20%26%20%5Ctext%7BY%20%3D%20f(X)%7D%20%5C%5C%5B5ex%5D%201%2C%20%26%20%5Ctext%7BY%20%24%5Cneq%24%20f(X)%7D%20%5Cend%7Bcases%7D)

### 2. 多行表达式

有时候需要将一行公式分多行进行显示。

```cpp
$$
\begin{aligned}
\sqrt{37} & = \sqrt{\frac{73^2-1}{12^2}} \\
 & = \sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}} \\ 
 & = \sqrt{\frac{73^2}{12^2}}\sqrt{\frac{73^2-1}{73^2}} \\
 & = \frac{73}{12}\sqrt{1 - \frac{1}{73^2}} \\ 
 & \approx \frac{73}{12}\left(1 - \frac{1}{2\cdot73^2}\right)
\end{aligned}
$$
```

表示:  
![\begin{aligned} \sqrt{37} & = \sqrt{\frac{73^2-1}{12^2}} \\ & = \sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}} \\ & = \sqrt{\frac{73^2}{12^2}}\sqrt{\frac{73^2-1}{73^2}} \\ & = \frac{73}{12}\sqrt{1 - \frac{1}{73^2}} \\ & \approx \frac{73}{12}\left(1 - \frac{1}{2\cdot73^2}\right) \end{aligned}](https://math.jianshu.com/math?formula=%5Cbegin%7Baligned%7D%20%5Csqrt%7B37%7D%20%26%20%3D%20%5Csqrt%7B%5Cfrac%7B73%5E2-1%7D%7B12%5E2%7D%7D%20%5C%5C%20%26%20%3D%20%5Csqrt%7B%5Cfrac%7B73%5E2%7D%7B12%5E2%7D%5Ccdot%5Cfrac%7B73%5E2-1%7D%7B73%5E2%7D%7D%20%5C%5C%20%26%20%3D%20%5Csqrt%7B%5Cfrac%7B73%5E2%7D%7B12%5E2%7D%7D%5Csqrt%7B%5Cfrac%7B73%5E2-1%7D%7B73%5E2%7D%7D%20%5C%5C%20%26%20%3D%20%5Cfrac%7B73%7D%7B12%7D%5Csqrt%7B1%20-%20%5Cfrac%7B1%7D%7B73%5E2%7D%7D%20%5C%5C%20%26%20%5Capprox%20%5Cfrac%7B73%7D%7B12%7D%5Cleft(1%20-%20%5Cfrac%7B1%7D%7B2%5Ccdot73%5E2%7D%5Cright)%20%5Cend%7Baligned%7D)

```ruby
$$
\begin{aligned}
a&=b+c-d \\
&=e-f \\
&=i \\
\end{aligned}
$$
```

表示:  
![\begin{aligned} a&=b+c-d \\ &=e-f \\ &=i \\ \end{aligned}](https://math.jianshu.com/math?formula=%5Cbegin%7Baligned%7D%20a%26%3Db%2Bc-d%20%5C%5C%20%26%3De-f%20%5C%5C%20%26%3Di%20%5C%5C%20%5Cend%7Baligned%7D)

其中`begin{equation}` 表示开始方程，`end{equation}`表示方程结束；`begin{split}` 表示开始多行公式，`end{split}` 表示结束；公式中用`\\` 表示回车到下一行，`&` 表示对齐的位置。

# 七、方程组

使用\begin{array}...\end{array} 与\left \与\right 配合表示方程组,如:

```ruby
$$
\left \{ 
\begin{array}{c}
a_1x+b_1y+c_1z=d_1 \\ 
a_2x+b_2y+c_2z=d_2 \\ 
a_3x+b_3y+c_3z=d_3
\end{array}
\right.
$$
```

表示：  
![\left \{ \begin{array}{c} a_1x+b_1y+c_1z=d_1 \\ a_2x+b_2y+c_2z=d_2 \\ a_3x+b_3y+c_3z=d_3 \end{array} \right.](https://math.jianshu.com/math?formula=%5Cleft%20%5C%7B%20%5Cbegin%7Barray%7D%7Bc%7D%20a_1x%2Bb_1y%2Bc_1z%3Dd_1%20%5C%5C%20a_2x%2Bb_2y%2Bc_2z%3Dd_2%20%5C%5C%20a_3x%2Bb_3y%2Bc_3z%3Dd_3%20%5Cend%7Barray%7D%20%5Cright.)

注意：通常MathJax通过内部策略自己管理公式内部的空间，因此`a…b` 与`a…….b`（.表示空格）都会显示为`ab`。可以通过在`ab`间加入`\`,增加些许间隙，`\;`增加较宽的间隙，`\quad` 与`\qquad` 会增加更大的间隙。

# 八、特殊函数与符号

### 1. 三角函数

`$\sin x$` : ![\sin x](https://math.jianshu.com/math?formula=%5Csin%20x)  
`$\arctan x$` : ![\arctan x](https://math.jianshu.com/math?formula=%5Carctan%20x)

### 2.比较运算符

小于`(\lt )`：![(\lt )](https://math.jianshu.com/math?formula=(%5Clt%20))  
大于`(\gt )`：![(\gt](https://math.jianshu.com/math?formula=(%5Cgt)  
小于等于`(\le )`：![(\le )](https://math.jianshu.com/math?formula=(%5Cle%20))  
大于等于`(\ge )`：![(\ge )](https://math.jianshu.com/math?formula=(%5Cge%20))  
不等于`(\ne )` :![(\ne )](https://math.jianshu.com/math?formula=(%5Cne%20))  
可以在这些运算符前面加上`\not` ，如`\not\lt` : ![\not\lt](https://math.jianshu.com/math?formula=%5Cnot%5Clt)

### 3.集合关系与运算

并集`(\cup)`: ![(\cup)](https://math.jianshu.com/math?formula=(%5Ccup))  
交集`(\cap)`: ![(\cap)](https://math.jianshu.com/math?formula=(%5Ccap))  
差集`(\setminus)`:![(\setminus)](https://math.jianshu.com/math?formula=(%5Csetminus))  
子集`(\subset)`: ![(\subset)](https://math.jianshu.com/math?formula=(%5Csubset))  
子集`(\subseteq)`: ![(\subseteq)](https://math.jianshu.com/math?formula=(%5Csubseteq))  
非子集`(\subsetneq)`: ![(\subsetneq)](https://math.jianshu.com/math?formula=(%5Csubsetneq))  
父集`(\supset)`: ![(\supset)](https://math.jianshu.com/math?formula=(%5Csupset))  
属于`(\in)`: ![(\in)](https://math.jianshu.com/math?formula=(%5Cin))  
不属于`(\notin)`:![`(\notin)](https://math.jianshu.com/math?formula=%60(%5Cnotin))  
空集`(\emptyset)`: ![(\emptyset)](https://math.jianshu.com/math?formula=(%5Cemptyset))  
空`(\varnothing)`: ![(\varnothing)](https://math.jianshu.com/math?formula=(%5Cvarnothing))

### 4. 排列

`\binom{n+1}{2k}` : ![\binom{n+1}{2k}](https://math.jianshu.com/math?formula=%5Cbinom%7Bn%2B1%7D%7B2k%7D)  
`{n+1 \choose 2k}` : ![{n+1 \choose 2k}](https://math.jianshu.com/math?formula=%7Bn%2B1%20%5Cchoose%202k%7D)

### 5. 箭头

`(\to)`:![(\to)](https://math.jianshu.com/math?formula=(%5Cto))  
`(\rightarrow)`: ![(\rightarrow)](https://math.jianshu.com/math?formula=(%5Crightarrow))  
`(\leftarrow)`: ![(\leftarrow)](https://math.jianshu.com/math?formula=(%5Cleftarrow))  
`(\Rightarrow)`:![`(\Rightarrow)](https://math.jianshu.com/math?formula=%60(%5CRightarrow))  
`(\Leftarrow)`: ![(\Leftarrow)](https://math.jianshu.com/math?formula=(%5CLeftarrow))  
`(\mapsto)`: ![\mapsto)](https://math.jianshu.com/math?formula=%5Cmapsto))

### 6. 逻辑运算符

`(\land)`: ![(\land)](https://math.jianshu.com/math?formula=(%5Cland))  
`(\lor)`: ![(\lor)](https://math.jianshu.com/math?formula=(%5Clor))  
`(\lnot)`: ![(\lnot)](https://math.jianshu.com/math?formula=(%5Clnot))  
`(\forall)`: ![(\forall)](https://math.jianshu.com/math?formula=(%5Cforall))  
`(\exists)`: ![(\exists)](https://math.jianshu.com/math?formula=(%5Cexists))  
`(\top)`: ![(\top)](https://math.jianshu.com/math?formula=(%5Ctop))  
`(\bot)`: ![(\bot)](https://math.jianshu.com/math?formula=(%5Cbot))  
`(\vdash)`: ![(\vdash)](https://math.jianshu.com/math?formula=(%5Cvdash))  
`(\vDash)`:![(\vDash)](https://math.jianshu.com/math?formula=(%5CvDash))

### 7.操作符

`(\star)`: ![`(\star)](https://math.jianshu.com/math?formula=%60(%5Cstar))  
`(\ast)`: ![(\ast)](https://math.jianshu.com/math?formula=(%5Cast))  
`(\oplus)`: ![(\oplus)](https://math.jianshu.com/math?formula=(%5Coplus))  
`(\circ)`: ![(\circ)](https://math.jianshu.com/math?formula=(%5Ccirc))  
`(\bullet)`: ![(\bullet)](https://math.jianshu.com/math?formula=(%5Cbullet))

### 8.等于

`(\approx)`:![(\approx)](https://math.jianshu.com/math?formula=(%5Capprox))  
`(\sim)`: ![(\sim)](https://math.jianshu.com/math?formula=(%5Csim))  
`(\equiv)`: ![(\equiv)](https://math.jianshu.com/math?formula=(%5Cequiv))  
`(\prec)`: ![(\prec)](https://math.jianshu.com/math?formula=(%5Cprec))

### 9.范围

`(\infty)`:![(\infty)](https://math.jianshu.com/math?formula=(%5Cinfty))  
`(\aleph_o)`:![(\aleph_o)](https://math.jianshu.com/math?formula=(%5Caleph_o))  
`(\nabla)`: ![(\nabla)](https://math.jianshu.com/math?formula=(%5Cnabla))  
`(\Im)`: ![(\Im)](https://math.jianshu.com/math?formula=(%5CIm))  
`(\Re)`: ![(\Re)](https://math.jianshu.com/math?formula=(%5CRe))

### 10. 模运算

`(\pmod)`: ![`(\pmod)](https://math.jianshu.com/math?formula=%60(%5Cpmod))  
如a \equiv b \pmod n 表示为: ![a \equiv b \pmod n](https://math.jianshu.com/math?formula=a%20%5Cequiv%20b%20%5Cpmod%20n)

### 11. 点

`(\ldots)`: ![(\ldots)](https://math.jianshu.com/math?formula=(%5Cldots))  
`(\cdots)`: ![(\cdots)](https://math.jianshu.com/math?formula=(%5Ccdots))  
`(\cdot)`: ![(\cdot)](https://math.jianshu.com/math?formula=(%5Ccdot))  
其区别是点的位置不同，`\ldots` 位置稍低，`\cdots` 位置居中。

```ruby
$$
\begin{cases}
a_1+a_2+\ldots+a_n \\ 
a_1+a_2+\cdots+a_n \\
\end{cases}
$$
```

表示(注意两部分点的位置)：  
![\begin{cases} a_1+a_2+\ldots+a_n \\ a_1+a_2+\cdots+a_n \\ \end{cases}](https://math.jianshu.com/math?formula=%5Cbegin%7Bcases%7D%20a_1%2Ba_2%2B%5Cldots%2Ba_n%20%5C%5C%20a_1%2Ba_2%2B%5Ccdots%2Ba_n%20%5C%5C%20%5Cend%7Bcases%7D)

### 12.顶部符号

对于单字符，`\hat x`：![\hat x](https://math.jianshu.com/math?formula=%5Chat%20x)  
多字符可以使用`\widehat {xy}`：![\widehat {xy}](https://math.jianshu.com/math?formula=%5Cwidehat%20%7Bxy%7D)  
类似的还有`\overline x`: ![\overline x](https://math.jianshu.com/math?formula=%5Coverline%20x)  
矢量`\vec x`:![\vec x](https://math.jianshu.com/math?formula=%5Cvec%20x)  
向量`\overrightarrow {xy}`: ![\overrightarrow {xy}](https://math.jianshu.com/math?formula=%5Coverrightarrow%20%7Bxy%7D)  
`\dot x` : ![\dot x](https://math.jianshu.com/math?formula=%5Cdot%20x)  
`\ddot x`: ![\ddot x](https://math.jianshu.com/math?formula=%5Cddot%20x)  
`\dot {\dot x}`: ![\dot {\dot x}](https://math.jianshu.com/math?formula=%5Cdot%20%7B%5Cdot%20x%7D)

# 九、表格

使用`\begin{array}{列样式}…\end{array}`这样的形式来创建表格，列样式可以是`clr` 表示居中，左，右对齐，还可以使用`|`表示一条竖线。表格中各行使用\ 分隔，各列使用& 分隔。使用`\hline` 在本行前加入一条直线。 例如:

```ruby
$$
\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\
\hline
1 & 0.24 & 1 & 125 \\
2 & -1 & 189 & -8 \\
3 & -20 & 2000 & 1+10i \\
\end{array}
$$
```

得到：  
![\begin{array}{c|lcr} n & \text{Left} & \text{Center} & \text{Right} \\ \hline 1 & 0.24 & 1 & 125 \\ 2 & -1 & 189 & -8 \\ 3 & -20 & 2000 & 1+10i \\ \end{array}](https://math.jianshu.com/math?formula=%5Cbegin%7Barray%7D%7Bc%7Clcr%7D%20n%20%26%20%5Ctext%7BLeft%7D%20%26%20%5Ctext%7BCenter%7D%20%26%20%5Ctext%7BRight%7D%20%5C%5C%20%5Chline%201%20%26%200.24%20%26%201%20%26%20125%20%5C%5C%202%20%26%20-1%20%26%20189%20%26%20-8%20%5C%5C%203%20%26%20-20%20%26%202000%20%26%201%2B10i%20%5C%5C%20%5Cend%7Barray%7D)

# 十、汉字、字体与格式

1. 汉字形式，符号：`\mbox{}`，如：![V_{\mbox{初始}}](https://math.jianshu.com/math?formula=V_%7B%5Cmbox%7B%E5%88%9D%E5%A7%8B%7D%7D)
2. 字体控制，符号：`\displaystyle`，如：![\displaystyle \frac{x+y}{y+z}](https://math.jianshu.com/math?formula=%5Cdisplaystyle%20%5Cfrac%7Bx%2By%7D%7By%2Bz%7D)
3. 下划线符号，符号：`\underline`，如：![\underline{x+y}](https://math.jianshu.com/math?formula=%5Cunderline%7Bx%2By%7D)
4. 标签，符号`\tag{数字}`，如：![\tag{11}](https://math.jianshu.com/math?formula=%5Ctag%7B11%7D)
5. 上大括号，符号：`\overbrace{算式}`，如：![\overbrace{a+b+c+d}^{2.0}](https://math.jianshu.com/math?formula=%5Coverbrace%7Ba%2Bb%2Bc%2Bd%7D%5E%7B2.0%7D)
6. 下大括号，符号：`\underbrace{算式}`，如：![a+\underbrace{b+c}_{1.0}+d](https://math.jianshu.com/math?formula=a%2B%5Cunderbrace%7Bb%2Bc%7D_%7B1.0%7D%2Bd)
7. 上位符号，符号：`\stacrel{上位符号}{基位符号}`，如：![\vec{x}\stackrel{\mathrm{def}}{=}{x_1,\dots,x_n}](https://math.jianshu.com/math?formula=%5Cvec%7Bx%7D%5Cstackrel%7B%5Cmathrm%7Bdef%7D%7D%7B%3D%7D%7Bx_1%2C%5Cdots%2Cx_n%7D)

# 十一、占位符

1. 两个quad空格，符号：`\qquad`，如：![x \qquad y](https://math.jianshu.com/math?formula=x%20%5Cqquad%20y)
2. quad空格，符号：`\quad`，如：![x \quad y](https://math.jianshu.com/math?formula=x%20%5Cquad%20y)
3. 大空格，符号`\`，如：![x \ y](https://math.jianshu.com/math?formula=x%20%5C%20y)
4. 中空格，符号`\:`，如：![x : y](https://math.jianshu.com/math?formula=x%20%3A%20y)
5. 小空格，符号`\,`，如：![x , y](https://math.jianshu.com/math?formula=x%20%2C%20y)
6. 没有空格，符号``，如：![xy](https://math.jianshu.com/math?formula=xy)
7. 紧贴，符号`\!`，如：![x ! y](https://math.jianshu.com/math?formula=x%20!%20y)

# 十二、定界符与组合

1. 括号，符号：`（）\big(\big) \Big(\Big) \bigg(\bigg) \Bigg(\Bigg)`，如：![（）\big(\big) \Big(\Big) \bigg(\bigg) \Bigg(\Bigg)](https://math.jianshu.com/math?formula=%EF%BC%88%EF%BC%89%5Cbig(%5Cbig)%20%5CBig(%5CBig)%20%5Cbigg(%5Cbigg)%20%5CBigg(%5CBigg))
2. 中括号，符号：`[]`，如：![[x+y]](https://math.jianshu.com/math?formula=%5Bx%2By%5D)
3. 大括号，符号：`\{ \}`，如：![{x+y}](https://math.jianshu.com/math?formula=%7Bx%2By%7D)
4. 自适应括号，符号：`\left \right`，如：`$\left(x\right)$`，![\left(x\right)](https://math.jianshu.com/math?formula=%5Cleft(x%5Cright))
5. 组合公式，符号：`{上位公式 \choose 下位公式}`，如：![{n+1 \choose k}={n \choose k}+{n \choose k-1}](https://math.jianshu.com/math?formula=%7Bn%2B1%20%5Cchoose%20k%7D%3D%7Bn%20%5Cchoose%20k%7D%2B%7Bn%20%5Cchoose%20k-1%7D)
6. 组合公式，符号：`{上位公式 \atop 下位公式}`，如：![\sum_{k_0,k_1,\ldots>0 \atop k_0+k_1+\cdots=n}A_{k_0}A_{k_1}\cdots](https://math.jianshu.com/math?formula=%5Csum_%7Bk_0%2Ck_1%2C%5Cldots%3E0%20%5Catop%20k_0%2Bk_1%2B%5Ccdots%3Dn%7DA_%7Bk_0%7DA_%7Bk_1%7D%5Ccdots)

# 十三、四则运算

1. 加法运算，符号：`+`，如：![x+y=z](https://math.jianshu.com/math?formula=x%2By%3Dz)
2. 减法运算，符号：`-`，如：![x-y=z](https://math.jianshu.com/math?formula=x-y%3Dz)
3. 加减运算，符号：`\pm`，如：![x \pm y=z](https://math.jianshu.com/math?formula=x%20%5Cpm%20y%3Dz)
4. 减甲运算，符号：`\mp`，如：![x \mp y=z](https://math.jianshu.com/math?formula=x%20%5Cmp%20y%3Dz)
5. 乘法运算，符号：`\times`，如：![x \times y=z](https://math.jianshu.com/math?formula=x%20%5Ctimes%20y%3Dz)
6. 点乘运算，符号：`\cdot`，如：![x \cdot y=z](https://math.jianshu.com/math?formula=x%20%5Ccdot%20y%3Dz)
7. 星乘运算，符号：`\ast`，如：![x \ast y=z](https://math.jianshu.com/math?formula=x%20%5Cast%20y%3Dz)
8. 除法运算，符号：`\div`，如：![x \div y=z](https://math.jianshu.com/math?formula=x%20%5Cdiv%20y%3Dz)
9. 斜法运算，符号：`/`，如：![x/y=z](https://math.jianshu.com/math?formula=x%2Fy%3Dz)
10. 分式表示，符号：`\frac{分子}{分母}`，如：![\frac{x+y}{y+z}](https://math.jianshu.com/math?formula=%5Cfrac%7Bx%2By%7D%7By%2Bz%7D)
11. 分式表示，符号：`{分子} \voer {分母}`，如：![{x+y} \over {y+z}](https://math.jianshu.com/math?formula=%7Bx%2By%7D%20%5Cover%20%7By%2Bz%7D)
12. 绝对值表示，符号：`||`，如：![|x+y|](https://math.jianshu.com/math?formula=%7Cx%2By%7C)

# 十四、高级运算

1. 平均数运算，符号：`\overline{算式}`，如：![\overline{xyz}](https://math.jianshu.com/math?formula=%5Coverline%7Bxyz%7D)
2. 开二次方运算，符号：`\sqrt`，如：![\sqrt x](https://math.jianshu.com/math?formula=%5Csqrt%20x)
3. 开方运算，符号：`\sqrt[开方数]{被开方数}`，如：![\sqrt[3]{x+y}](https://math.jianshu.com/math?formula=%5Csqrt%5B3%5D%7Bx%2By%7D)
4. 对数运算，符号：`\log`，如：![\log(x)](https://math.jianshu.com/math?formula=%5Clog(x))
5. 极限运算，符号：`\lim`，如：![\lim^{x \to \infty}_{y \to 0}{\frac{x}{y}}](https://math.jianshu.com/math?formula=%5Clim%5E%7Bx%20%5Cto%20%5Cinfty%7D_%7By%20%5Cto%200%7D%7B%5Cfrac%7Bx%7D%7By%7D%7D)
6. 极限运算，符号：`\displaystyle \lim`，如：![\displaystyle \lim^{x \to \infty}_{y \to 0}{\frac{x}{y}}](https://math.jianshu.com/math?formula=%5Cdisplaystyle%20%5Clim%5E%7Bx%20%5Cto%20%5Cinfty%7D_%7By%20%5Cto%200%7D%7B%5Cfrac%7Bx%7D%7By%7D%7D)
7. 求和运算，符号：`\sum`，如：![\sum^{x \to \infty}_{y \to 0}{\frac{x}{y}}](https://math.jianshu.com/math?formula=%5Csum%5E%7Bx%20%5Cto%20%5Cinfty%7D_%7By%20%5Cto%200%7D%7B%5Cfrac%7Bx%7D%7By%7D%7D)
8. 求和运算，符号：`\displaystyle \sum`，如：![\displaystyle \sum^{x \to \infty}_{y \to 0}{\frac{x}{y}}](https://math.jianshu.com/math?formula=%5Cdisplaystyle%20%5Csum%5E%7Bx%20%5Cto%20%5Cinfty%7D_%7By%20%5Cto%200%7D%7B%5Cfrac%7Bx%7D%7By%7D%7D)
9. 积分运算，符号：`\int`，如：![\int^{\infty}_{0}{xdx}](https://math.jianshu.com/math?formula=%5Cint%5E%7B%5Cinfty%7D_%7B0%7D%7Bxdx%7D)
10. 积分运算，符号：`\displaystyle \int`，如：![\displaystyle \int^{\infty}_{0}{xdx}](https://math.jianshu.com/math?formula=%5Cdisplaystyle%20%5Cint%5E%7B%5Cinfty%7D_%7B0%7D%7Bxdx%7D)
11. 微分运算，符号：`\partial`，如：![\frac{\partial x}{\partial y}](https://math.jianshu.com/math?formula=%5Cfrac%7B%5Cpartial%20x%7D%7B%5Cpartial%20y%7D)
12. 矩阵表示，符号：`\begin{matrix} \end{matrix}`，如：![\left[ \begin{matrix} 1 &2 &\cdots &4\5 &6 &\cdots &8\\vdots &\vdots &\ddots &\vdots\13 &14 &\cdots &16\end{matrix} \right]](https://math.jianshu.com/math?formula=%5Cleft%5B%20%5Cbegin%7Bmatrix%7D%201%20%262%20%26%5Ccdots%20%264%5C5%20%266%20%26%5Ccdots%20%268%5C%5Cvdots%20%26%5Cvdots%20%26%5Cddots%20%26%5Cvdots%5C13%20%2614%20%26%5Ccdots%20%2616%5Cend%7Bmatrix%7D%20%5Cright%5D)

# 十五、 逻辑运算

1. 等于运算，符号：`=`，如：![x+y=z](https://math.jianshu.com/math?formula=x%2By%3Dz)
2. 大于运算，符号：`>`，如：![x+y>z](https://math.jianshu.com/math?formula=x%2By%3Ez)
3. 小于运算，符号：`<`，如：![x+y<z](https://math.jianshu.com/math?formula=x%2By%3Cz)
4. 大于等于运算，符号：`\geq`，如：![x+y \geq z](https://math.jianshu.com/math?formula=x%2By%20%5Cgeq%20z)
5. 小于等于运算，符号：`\leq`，如：![x+y \leq z](https://math.jianshu.com/math?formula=x%2By%20%5Cleq%20z)
6. 不等于运算，符号：`\neq`，如：![x+y \neq z](https://math.jianshu.com/math?formula=x%2By%20%5Cneq%20z)
7. 不大于等于运算，符号：`\ngeq`，如：![x+y \ngeq z](https://math.jianshu.com/math?formula=x%2By%20%5Cngeq%20z)
8. 不大于等于运算，符号：`\not\geq`，如：![x+y \not\geq z](https://math.jianshu.com/math?formula=x%2By%20%5Cnot%5Cgeq%20z)
9. 不小于等于运算，符号：`\nleq`，如：![x+y \nleq z](https://math.jianshu.com/math?formula=x%2By%20%5Cnleq%20z)
10. 不小于等于运算，符号：`\not\leq`，如：![x+y \not\leq z](https://math.jianshu.com/math?formula=x%2By%20%5Cnot%5Cleq%20z)
11. 约等于运算，符号：`\approx`，如：![x+y \approx z](https://math.jianshu.com/math?formula=x%2By%20%5Capprox%20z)
12. 恒定等于运算，符号：`\equiv`，如：![x+y \equiv z](https://math.jianshu.com/math?formula=x%2By%20%5Cequiv%20z)

# 十六、集合运算

1. 属于运算，符号：`\in`，如：![x \in y](https://math.jianshu.com/math?formula=x%20%5Cin%20y)
2. 不属于运算，符号：`\notin`，如：![x \notin y](https://math.jianshu.com/math?formula=x%20%5Cnotin%20y)
3. 不属于运算，符号：`\not\in`，如：![x \not\in y](https://math.jianshu.com/math?formula=x%20%5Cnot%5Cin%20y)
4. 子集运算，符号：`\subset`，如：![x \subset y](https://math.jianshu.com/math?formula=x%20%5Csubset%20y)
5. 子集运算，符号：`\supset`，如：![x \supset y](https://math.jianshu.com/math?formula=x%20%5Csupset%20y)
6. 真子集运算，符号：`\subseteq`，如：![x \subseteq y](https://math.jianshu.com/math?formula=x%20%5Csubseteq%20y)
7. 非真子集运算，符号：`\subsetneq`，如：![x \subsetneq y](https://math.jianshu.com/math?formula=x%20%5Csubsetneq%20y)
8. 真子集运算，符号：`\supseteq`，如：![x \supseteq y](https://math.jianshu.com/math?formula=x%20%5Csupseteq%20y)
9. 非真子集运算，符号：`\supsetneq`，如：![x \supsetneq y](https://math.jianshu.com/math?formula=x%20%5Csupsetneq%20y)
10. 非子集运算，符号：`\not\subset`，如：![x \not\subset y](https://math.jianshu.com/math?formula=x%20%5Cnot%5Csubset%20y)
11. 非子集运算，符号：`\not\supset`，如：![x \not\supset y](https://math.jianshu.com/math?formula=x%20%5Cnot%5Csupset%20y)
12. 并集运算，符号：`\cup`，如：![x \cup y](https://math.jianshu.com/math?formula=x%20%5Ccup%20y)
13. 交集运算，符号：`\cap`，如：![x \cap y](https://math.jianshu.com/math?formula=x%20%5Ccap%20y)
14. 差集运算，符号：`\setminus`，如：![x \setminus y](https://math.jianshu.com/math?formula=x%20%5Csetminus%20y)
15. 同或运算，符号：`\bigodot`，如：![x \bigodot y](https://math.jianshu.com/math?formula=x%20%5Cbigodot%20y)
16. 同与运算，符号：`\bigotimes`，如：![x \bigotimes y](https://math.jianshu.com/math?formula=x%20%5Cbigotimes%20y)
17. 实数集合，符号：`\mathbb{R}`，如：`\mathbb{R}`
18. 自然数集合，符号：`\mathbb{Z}`，如：`\mathbb{Z}`
19. 空集，符号：`\emptyset`，如：![\emptyset](https://math.jianshu.com/math?formula=%5Cemptyset)

# 十七、数学符号

1. 无穷，符号：`\infty`，如：![\infty](https://math.jianshu.com/math?formula=%5Cinfty)
2. 虚数，符号：`\imath`，如：![\imath](https://math.jianshu.com/math?formula=%5Cimath)
3. 虚数，符号：`\jmath`，如：![\jmath](https://math.jianshu.com/math?formula=%5Cjmath)
4. 数学符号，符号`\hat{a}`，如：![\hat{a}](https://math.jianshu.com/math?formula=%5Chat%7Ba%7D)
5. 数学符号，符号`\check{a}`，如：![\check{a}](https://math.jianshu.com/math?formula=%5Ccheck%7Ba%7D)
6. 数学符号，符号`\breve{a}`，如：![\breve{a}](https://math.jianshu.com/math?formula=%5Cbreve%7Ba%7D)
7. 数学符号，符号`\tilde{a}`，如：![\tilde{a}](https://math.jianshu.com/math?formula=%5Ctilde%7Ba%7D)
8. 数学符号，符号`\bar{a}`，如：![\bar{a}](https://math.jianshu.com/math?formula=%5Cbar%7Ba%7D)
9. 矢量符号，符号`\vec{a}`，如：![\vec{a}](https://math.jianshu.com/math?formula=%5Cvec%7Ba%7D)
10. 数学符号，符号`\acute{a}`，如：![\acute{a}](https://math.jianshu.com/math?formula=%5Cacute%7Ba%7D)
11. 数学符号，符号`\grave{a}`，如：![\grave{a}](https://math.jianshu.com/math?formula=%5Cgrave%7Ba%7D)
12. 数学符号，符号`\mathring{a}`，如：![\mathring{a}](https://math.jianshu.com/math?formula=%5Cmathring%7Ba%7D)
13. 一阶导数符号，符号`\dot{a}`，如：![\dot{a}](https://math.jianshu.com/math?formula=%5Cdot%7Ba%7D)
14. 二阶导数符号，符号`\ddot{a}`，如：![\ddot{a}](https://math.jianshu.com/math?formula=%5Cddot%7Ba%7D)
15. 上箭头，符号：`\uparrow`，如：![\uparrow](https://math.jianshu.com/math?formula=%5Cuparrow)
16. 上箭头，符号：`\Uparrow`，如：![\Uparrow](https://math.jianshu.com/math?formula=%5CUparrow)
17. 下箭头，符号：`\downarrow`，如：![\downarrow](https://math.jianshu.com/math?formula=%5Cdownarrow)
18. 下箭头，符号：`\Downarrow`，如：![\Downarrow](https://math.jianshu.com/math?formula=%5CDownarrow)
19. 左箭头，符号：`\leftarrow`，如：![\leftarrow](https://math.jianshu.com/math?formula=%5Cleftarrow)
20. 左箭头，符号：`\Leftarrow`，如：![\Leftarrow](https://math.jianshu.com/math?formula=%5CLeftarrow)
21. 右箭头，符号：`\rightarrow`，如：![\rightarrow](https://math.jianshu.com/math?formula=%5Crightarrow)
22. 右箭头，符号：`\Rightarrow`，如：![\Rightarrow](https://math.jianshu.com/math?formula=%5CRightarrow)
23. 底端对齐的省略号，符号：`\ldots`，如：![1,2,\ldots,n](https://math.jianshu.com/math?formula=1%2C2%2C%5Cldots%2Cn)
24. 中线对齐的省略号，符号：`\cdots`，如：![x_1^2 + x_2^2 + \cdots + x_n^2](https://math.jianshu.com/math?formula=x_1%5E2%20%2B%20x_2%5E2%20%2B%20%5Ccdots%20%2B%20x_n%5E2)
25. 竖直对齐的省略号，符号：`\vdots`，如：![\vdots](https://math.jianshu.com/math?formula=%5Cvdots)
26. 斜对齐的省略号，符号：`\ddots`，如：![\ddots](https://math.jianshu.com/math?formula=%5Cddots)

# 十八、矩阵

使用`\begin{matrix}…\end{matrix}`这样的形式来表示矩阵，在`\begin` 与`\end`之间加入矩阵中的元素即可。矩阵的行之间使用`\\`分隔，列之间使用`&`分隔，例如:

```ruby
$$
\begin{matrix}
1 & x & x^2 \\
1 & y & y^2 \\
1 & z & z^2 \\
\end{matrix}
$$
```

得到：  
![\begin{matrix} 1 & x & x^2 \\ 1 & y & y^2 \\ 1 & z & z^2 \\ \end{matrix}](https://math.jianshu.com/math?formula=%5Cbegin%7Bmatrix%7D%201%20%26%20x%20%26%20x%5E2%20%5C%5C%201%20%26%20y%20%26%20y%5E2%20%5C%5C%201%20%26%20z%20%26%20z%5E2%20%5C%5C%20%5Cend%7Bmatrix%7D)

### 1. 括号

如果要对矩阵加括号，可以像上文中提到的一样，使用`\left`与`\right` 配合表示括号符号。也可以使用特殊的matrix 。即替换`\begin{matrix}…\end{matrix} 中matrix 为pmatrix ，bmatrix ，Bmatrix ，vmatrix , Vmatrix` 。  
`pmatrix$\begin{pmatrix}1 & 2 \\ 3 & 4\\ \end{pmatrix}$`:pmatrix![\begin{pmatrix}1 & 2 \\ 3 & 4\\ \end{pmatrix}](https://math.jianshu.com/math?formula=%5Cbegin%7Bpmatrix%7D1%20%26%202%20%5C%5C%203%20%26%204%5C%5C%20%5Cend%7Bpmatrix%7D)  
`bmatrix$\begin{bmatrix}1 & 2 \\ 3 & 4\\ \end{bmatrix}$` :bmatrix![\begin{bmatrix}1 & 2 \\ 3 & 4\\ \end{bmatrix}](https://math.jianshu.com/math?formula=%5Cbegin%7Bbmatrix%7D1%20%26%202%20%5C%5C%203%20%26%204%5C%5C%20%5Cend%7Bbmatrix%7D)  
`Bmatrix$\begin{Bmatrix}1 & 2 \\ 3 & 4\\ \end{Bmatrix}$` :Bmatrix![\begin{Bmatrix}1 & 2 \\ 3 & 4\\ \end{Bmatrix}](https://math.jianshu.com/math?formula=%5Cbegin%7BBmatrix%7D1%20%26%202%20%5C%5C%203%20%26%204%5C%5C%20%5Cend%7BBmatrix%7D)  
`vmatrix$\begin{vmatrix}1 & 2 \\ 3 & 4\\ \end{vmatrix}$` :vmatrix![\begin{vmatrix}1 & 2 \\ 3 & 4\\ \end{vmatrix}](https://math.jianshu.com/math?formula=%5Cbegin%7Bvmatrix%7D1%20%26%202%20%5C%5C%203%20%26%204%5C%5C%20%5Cend%7Bvmatrix%7D)  
`Vmatrix$\begin{Vmatrix}1 & 2 \\ 3 & 4\\ \end{Vmatrix}$` :Vmatrix![\begin{Vmatrix}1 & 2 \\ 3 & 4\\ \end{Vmatrix}](https://math.jianshu.com/math?formula=%5Cbegin%7BVmatrix%7D1%20%26%202%20%5C%5C%203%20%26%204%5C%5C%20%5Cend%7BVmatrix%7D)  
元素省略:  
可以使用\cdots ：⋯，\ddots：⋱ ，\vdots：⋮ 来省略矩阵中的元素，如：

```ruby
$$
\begin{pmatrix}
1&a_1&a_1^2&\cdots&a_1^n\\
1&a_2&a_2^2&\cdots&a_2^n\\
\vdots&\vdots&\vdots&\ddots&\vdots\\
1&a_m&a_m^2&\cdots&a_m^n\\
\end{pmatrix}
$$

```

表示：  
![\begin{pmatrix} 1&a_1&a_1^2&\cdots&a_1^n\\ 1&a_2&a_2^2&\cdots&a_2^n\\ \vdots&\vdots&\vdots&\ddots&\vdots\\ 1&a_m&a_m^2&\cdots&a_m^n\\ \end{pmatrix}](https://math.jianshu.com/math?formula=%5Cbegin%7Bpmatrix%7D%201%26a_1%26a_1%5E2%26%5Ccdots%26a_1%5En%5C%5C%201%26a_2%26a_2%5E2%26%5Ccdots%26a_2%5En%5C%5C%20%5Cvdots%26%5Cvdots%26%5Cvdots%26%5Cddots%26%5Cvdots%5C%5C%201%26a_m%26a_m%5E2%26%5Ccdots%26a_m%5En%5C%5C%20%5Cend%7Bpmatrix%7D)

### 2. 增广矩阵

增广矩阵需要使用前面的表格中使用到的`\begin{array} ... \end{array}`来实现。

```swift
$$
\left[  \begin{array}  {c c | c} %这里的c表示数组中元素对其方式：c居中、r右对齐、l左对齐，竖线表示2、3列间插入竖线
1 & 2 & 3 \\
\hline %插入横线，如果去掉\hline就是增广矩阵
4 & 5 & 6
\end{array}  \right]
$$

```

显示为：  
![\left[ \begin{array} {c c | c} 1 & 2 & 3 \\ \hline 4 & 5 & 6 \end{array} \right]](https://math.jianshu.com/math?formula=%5Cleft%5B%20%5Cbegin%7Barray%7D%20%7Bc%20c%20%7C%20c%7D%201%20%26%202%20%26%203%20%5C%5C%20%5Chline%204%20%26%205%20%26%206%20%5Cend%7Barray%7D%20%5Cright%5D)

# 二十、公式标记与引用

使用`\tag{yourtag}`来标记公式，如`$$a=x^2-y^3\tag{1}$$`显示为：  
![a=x^2-y^3\tag{1}](https://math.jianshu.com/math?formula=a%3Dx%5E2-y%5E3%5Ctag%7B1%7D)

# 二十一、字体

### 1.黑板粗体字

此字体经常用来表示代表实数、整数、有理数、复数的大写字母。  
`$\mathbb ABCDEF$`：![\mathbb ABCDEF](https://math.jianshu.com/math?formula=%5Cmathbb%20ABCDEF)  
`$\Bbb ABCDEF$`：![\Bbb ABCDEF](https://math.jianshu.com/math?formula=%5CBbb%20ABCDEF)

### 3.黑体字

`$\mathbf ABCDEFGHIJKLMNOPQRSTUVWXYZ$`:![\mathbf ABCDEFGHIJKLMNOPQRSTUVWXYZ](https://math.jianshu.com/math?formula=%5Cmathbf%20ABCDEFGHIJKLMNOPQRSTUVWXYZ)  
`$\mathbf abcdefghijklmnopqrstuvwxyz$`:![\mathbf abcdefghijklmnopqrstuvwxyz](https://math.jianshu.com/math?formula=%5Cmathbf%20abcdefghijklmnopqrstuvwxyz)

### 3.打印机字体

`$\mathtt ABCDEFGHIJKLMNOPQRSTUVWXYZ$`:![\mathtt ABCDEFGHIJKLMNOPQRSTUVWXYZ](https://math.jianshu.com/math?formula=%5Cmathtt%20ABCDEFGHIJKLMNOPQRSTUVWXYZ)

# 二十二、希腊字母

| 字母  | 实现              | 字母  | 实现            |
| --- | --------------- | --- | ------------- |
| A   | `A`             | α   | `\alhpa`      |
| B   | `B`             | β   | `\beta`       |
| Γ   | `\Gamma`        | γ   | `\gamma`      |
| Δ   | `\Delta`        | δ   | `\delta`      |
| E   | `E`             | ϵ   | `\epsilon`    |
| Z   | `Z`             | ζ   | `\zeta`       |
| H   | `H`             | η   | `\eta`        |
| Θ   | `\Theta`        | θ   | `\theta`      |
| I   | `I`             | ι   | `\iota`       |
| K   | `K`             | κ   | `\kappa`      |
| Λ   | `\Lambda`       | λ   | `\lambda`     |
| M   | `M`             | μ   | `\mu`         |
| N   | `N`             | ν   | `\nu`         |
| Ξ   | `\Xi`           | ξ   | `\xi`         |
| O   | `O`             | ο   | `\omicron`    |
| Π   | `\Pi`           | π   | `\pi`         |
| P   | `P`             | ρ   | `\rho`        |
| Σ   | `\Sigma`        | σ   | `\sigma`      |
| T   | `T`             | τ   | `\tau`        |
| Υ   | `\Upsilon`      | υ   | `\upsilon`    |
| Φ   | `\Phi`          | ϕ   | `\phi`        |
| X   | `X`             | χ   | `\chi`        |
| Ψ   | `\Psi`          | ψ   | `\psi`        |
| Ω   | `\v`            | ω   | `\omega`      |
| ε   | `$\varepsilon$` | ϑ   | `$\vartheta$` |
| ϖ   | `$\varpi$`      | ϱ   | `$\varrho$`   |
| ς   | `$\varsigma$`   | φ   | `$\varphi$`   |

[https://blog.csdn.net/weixin_43159628/article/details/88237139](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fweixin_43159628%2Farticle%2Fdetails%2F88237139)  
[https://www.jianshu.com/p/08cbe54a5f33](https://www.jianshu.com/p/08cbe54a5f33)

  
  
作者：无剑_君  
链接：https://www.jianshu.com/p/383e8149136c  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。