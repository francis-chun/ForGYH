### 生成专属于自己的文件

```java
replace K=K*abs(rnormal(0,1))
replace L=L*abs(rnormal(0,1))
save ECN6540_Assignment_mydata.dta, replace
```

### 加载数据

```java
use ECN6540_Assignment_mydata.dta, clear 
```

### 公式变换

$$
Y=AL^\alpha K^{1-\alpha} =>lnY = lnA + \alpha lnL + (1-\alpha)lnK
$$

```java
gen lnY = log(Y)
gen lnL = log(L)
gen lnK = log(K)
```

### 线性回归

```java
reg lnY lnL lnK
```

![image-20201121103852214](.\image-20201121103852214.png)

### White Test

```java
//获取残差
predict e,res
//获取y_hat
predict yhat, xb
//生成残差值平方
gen e2 = e^2
//生成各个的参数组合
gen lnL2 = lnL^2
gen lnK2 = lnK^2
gen lnL_lnK = lnL * lnK
//回归
reg e2 lnL lnK lnL2 lnK2 lnL_lnK
di e(N)*e(r2) //11.547993
//计算卡方检验自由度为5，显著性水平为5%的临界值
di invchi2tail(5,0.95)  //1.1454762
    
//拒绝原假设
```

![image-20201121110641297](.\image-20201121110641297.png)
$$
H_0:\beta_0 =0 , H_1:\beta_0 \ne0\\t=\frac {\beta_0^{hat}-\beta_0}{se(\theta)}
$$

```java
di 12.35033/0.7500209
//查看t检验临界值
di invttail(87, 0.025) //1.9876083
```

