前言：

覆盖范围：普及组数学

1. 素性测试

   1. 整除

      对于整数 $n, m$，如果存在整数 $q$ 使得 $n = q \times m$，则称 $m$ 整除 $n$,记作 $m \mid n$。其中， $m$ 是 $n$ 的因数， $n$ 是 $m$ 的倍数。

      反之，记作 $m \nmid n$。

   2. 质数

      如果一个数 $n$ 的**正因数**只有 $1$ 和它自己，并且这个数不是 $1$。则称这样的数是质数。

   3. 素性测试，即判断一个数是不是质数。

      考虑暴力，一个数是质数指的是它的正因数只有 $1$ 和它自己，那么逆命题就是它一个不为 $1$ 和它自己的因数。所以枚举 $2 \sim n - 1$，如果在范围内存在一个数 $i$ 使得 $i \mid n$，这个数就不是质数。这种做法即 **试除法**。 $O(n)$。

      P1185 

      然而， $O(n)$ 的时间复杂度是无法通过本题的。

      一个数 $n$ 最多只有一个质因数大于等于 $\sqrt n$。因此我们可以将试除的范围缩小到 $\sqrt n$。

      思考：1.为什么试除的范围可以缩小？ 2. 如何证明？ 3.如何保证试除过程的精度不丢失？  4.是否存在更优的做法？

      1. 判断是不是质数，其实只需要判断是否存在质因数即可。因为任何非质数因数都可以进一步分解为质因数。‘

        A3042 学生自己做

      G1033

2. 质因数分解

   1. 质因数分解

      沿用优化过的试除法的思路。在 $2$ 至 $\sqrt n$ 之间枚举，如果当前的 $i \mid n$ 就不断地除 $i$，直到 $i \nmid n$。最后特判 $n \neq 1$ 的情况。时间复杂度 $O(\sqrt n)$。

      A2017

      TW1141

      思考：为什么这样是对的？是否存在更优的做法？

      G1063

   2. 唯一分解定理

      对于任意的整数 $n$，必然可以将其表示为 $n = p_1^{\alpha_1}p_2^{\alpha_2}\cdots p_k^{\alpha_k}$，其中， $p_1, p_2, \cdots, p_k$ 是质数。

      也就是说，任何一个数都可以表示为若干个素数的乘积。

      1. 证明 “一个数 $n$ 最多只有一个质因数大于等于 $\sqrt n$。”

         显然，要是存在两个不同的质因子 $p_1, p_2 \geq \sqrt n$， $p_1 \times p_2$ 必然大于 $n$，矛盾。

      2. 证明上文质因数分解的正确性

         由于循环从 $2 \in \mathbb{P}$ 开始，如果 $i \mid n$，它必然是一个质数因子，否则，它一定会它自己的一个质数因子在之前不断做除法的过程中去掉。这个思想非常的重要，我们在之后学习筛法时还会见到它。

      3. 唯一分解定理又称之为算数基本定理。我们在课上不做证明。感兴趣的同学可以自己尝试证明。

        G1043 （二分）
      
        G1157（难）

3. 筛法

   质数定理：$1 \sim n$ 内的质数数量 $\pi(n) \sim \frac{n}{\ln n}$。（涉及高等数学，依旧不证明）

   P1187

   1. 如果不断的对若干个数字做素性测试然后判断显然会超时。事实上，涉及到找质数的问题应该使用筛法。

      筛法的基本思想：筛去倍数。

   2. 朴素筛法

      思想：从 $2$ 开始枚举，对于当前的数字 $i$，将其在 $n$ 以内的倍数全部筛去。

      时间复杂度 $O(n \log n)$。

   3. 埃氏筛

      考虑在朴素筛法的基础上优化，类似于分解质因数的思想。对于所有的合数，我们一定能够用一个质数筛去它。所以将代码改为，对于当前的数字 $i$，如果它是质数，就将其在 $n$ 以内的倍数全部筛去。

      时间复杂度 $O (n \log \log n)$。由于证明难度较大，所以跳过。

   4. 线性筛（欧拉筛）

      即便我们只用质数筛数字，也会存在有些数字被重复筛的情况。因此考虑让每一个数字只被它的最小质因子筛去。

      时间复杂度 $O(n)$。

      思考：如何证明线性筛的正确性？

   筛法的用处不仅在于预处理质数。筛法的思想也经常用来预处理各种内容。

   G1094 ABC170D P1500 ABC172D

4. 最大公约数和最小公倍数

   1. GCD

      GCD，即最大公因数。比如说 $\gcd(3, 5) = 1$，$gcd(4, 6) = 2$。

      求最大公因数一般用辗转相除法（欧几里得算法）

      ```Pseudocode
      function gcd(a, b)
      	if b == 0
      		return a
      	return gcd(b, a % b)
      ```

      辗转相除法用到了 GCD 的几个性质：

      $\gcd(a, b) = \gcd(b, a)$

      $\gcd(a, 0) = a$

      $\gcd(a, b) = \gcd(b, a \bmod b)$

      前两个性质显然。考虑证明第三个性质

      设 $c = a \bmod b$，所以有 $a = kb + c$。设 $d \mid a$，$d \mid b$。则 $\frac{a}{d} = k\frac{b}{d} + \frac{c}{d}$，移项得到 $\frac{a}{d} - k\frac{b}{d} = \frac{c}{d}$。显然 $\frac{a}{d} - k\frac{b}{d}$ 是一个整数，因此 $\frac{c}{d}$ 也是一个整数，即 $d \mid c$。上面的内容证明了 $a$ 和 $b$ 的公因子也是$b$ 和 $a \bmod b$ 的公因子。

      反过来，设 $d \mid c$，$d \mid b$，有 $\frac{a}{d} = k \frac{b}{d} + \frac{c}{d}$。右边的式子显然是一个整数，因此 $\frac{a}{d}$ 也是一个整数，即 $d \mid a$。也就是说 $b$ 和 $a \bmod b$ 的公因子也是 $a$ 和 $b$ 的公因子。

      既然公因子相同，那么它们的最大公因数也相同。

      思考：时间复杂度是多少？ $O(\log \max(a, b))$。 

      A3090（自己做）

      另外，如果 $\gcd(a, b) = 1$，我们称 $a$ 和 $b$ 互质。

      观察到，相邻的两个整数总是互质的。此外，斐波那契数列的相邻两项也是互质的。

      内置的 gcd：

      如果你引用了 `<algorithm>` 库，那么你可以使用 `__gcd(a, b)` 来求 $a$ 和 $b$ 的 GCD。但是需要注意：我们一般不推荐使用这个函数，因为 `__gcd()` 不是标准库函数，而是 GCC 自行实现的函数。真实环境下最好使用手写的 GCD。

      另外，在 C++17 以后的标准中，你可以使用 `std::gcd(a, b)` 来求 GCD，但是 OI 比赛的版本限制在 C++14，GESP 的版本限制在 C++11。

      （注意，__gcd()可能无法处理负数，但是std::gcd()可以，其实只需要gcd(abs(a), abs(b)) 就可以了）。

      多个数的 GCD。

      如果要求多个数共同的 GCD，其实非常简单。只需要依次求序列的 GCD 就可以了。

      ```Pseudocode
      Input. An Array A consisting of n elements.
      Output. The GCD of all elements
      Method.
      g <- 0
      for i <- 1 to n
      	g <- gcd(g, a[i])
      ```

      GCD 的另一种求法：

      更损相减术：$\gcd(a, b) = \gcd(a - b, b)$。

      如果求多个数的 GCD，则有 $\gcd(a, b, c, \cdots) = \gcd(a, \gcd(b - a, c - a, \cdots))$。

      思考：1.如何证明？2.时间复杂度？$O(\log \max(a, b))$。 

      G1127

      

   2. LCM

      LCM，即最小公倍数。比如说 $\operatorname {lcm}(2, 3) = 6$，$\operatorname {lcm}(2, 4) = 4$。

      求 LCM 非常简单，有$\operatorname{lcm}(a, b) = \frac{a \times b}{\gcd(a, b)}$。

      考虑证明：（学生先证明五分钟）

      根据唯一分解定理，设 $a = p_1^{\alpha_1}p_2^{\alpha_2}\cdots p_k^{\alpha_k}$，$b = p_1^{\beta_1}p_2^{\beta_2}\cdots p_k^{\beta_k}$。

      那么 $\gcd(a, b) = p_1^{\min(\alpha_1, \beta_1)}p_2^{\min(\alpha_2, \beta_2)}\cdots p_k^{\min(\alpha_k, \beta_k)}$，$\operatorname{lcm}(a, b) = p_1^{\max(\alpha_1, \beta_1)}p_2^{\max(\alpha_2, \beta_2)}\cdots p_k^{\max(\alpha_k, \beta_k)}$

      $a \times b = p_1^{\alpha_1 + \beta_1}p_2^{\alpha_2 + \beta_2}\cdots p_k^{\alpha_k + \beta_k}$。

      显然， $\frac{a \times b}{\gcd(a, b)}$ 的结果就是去掉所有 $\alpha_i + \beta_i$ 中的 $\min(\alpha_i, \beta_i)$。因此得证。

      因此，在已经求得 GCD 的情况下，求 LCM 的时间复杂度是 $O(1)$。如果还未求得 GCD，那么需要先求 GCD，时间复杂度为 $O(\log \min(a, b))$。

      P1108（自己做）

      内置的 LCM

      在 C++17 中，提供函数 `std::lcm()`。依旧推荐参加 OI 比赛的同学们自行实现。

      求多个数的 LCM。

      需要注意，$\operatorname{lcm}(a, b) = \frac{a \times b}{\gcd(a, b)}$ 只适用于两个元素的情况，对于多个元素不适用。也就是说，形如 $\operatorname{lcm}(a, b, c, d, ...) = \frac{a \times b \times c \times d \times \cdots}{\gcd(a, b, c, d, ...)}$ 是不成立的。

      求多个数的 LCM 的方法类似于求多个数的 GCD，依旧是按照序列依次计算。

      ```pseudocode
      Input. An Array A consisting of n elements.
      Output. The LCM of all elements
      Method.
      l <- lcm(a[1], a[2])
      for i <- 3 to n
      	l <- lcm(l, a[i])
      ```

   TW4031

   ABC445E（难）

5. 快速幂

   快速幂本身其实属于位运算的部分。但是由于很多数论操作都依赖快速幂，因此在本节讲解。

   引入：如果要求 $a^b % P$，应该怎么做？

   对于 `cmath` 库中提供的 `pow()` 函数，它的返回值是一个 double；

   如果直接乘 $b$ 次 $a$，显然会超时。

   考虑对 $b$ 做二进制拆分，设 $b = 2^{k_1} + 2^{k_2} + 2^{k_3} + \cdots$，那么 $a^b = a^{2^{k_1}} \times a^{2^{k_2}} \times a^{2^{k_3}} \cdots$ 所以我们可以一边拆 $b$ 一边做乘法。

   ```pseudocode
   function quick_pow(a, b)
   	res <- 1
   	while b > 0
   		if b % 2 == 1
   			res <- res * a
   		a <- a * a
   		b <- b / 2
   	return res
   ```

   或者我们可以用位运算的方式表示

   ```pseudocode
   function quick_pow(a, b)
   	res <- 1
   	while b > 0
   		if b & 1 == 1
   			res <- res * a
   		a <- a * a
   		b <- b >> 1
   	return res
   ```

   显然，快速幂的时间复杂度取决于拆分 $b$ 的次数，也就是 $\log b$。

   快速幂不仅快，它的作用还在于在做幂运算的时候让结果对一个数取模，防止结果溢出。

   P1194

   由于快速幂的本质是做乘法运算。因此我们可以用 $(a \times b) \bmod P = a \bmod P \times b \bmod P$ 的性质做计算。

   ```pseudocode
   function quick_pow(a, b)
   	res <- 1
   	while b > 0
   		if b & 1 == 1
   			res <- res * a % P
   		a <- a * a % P
   		b <- b >> 1
     return res
   ```

   本质上，快速幂是基于**倍增思想**的算法。

   拓展：

   我们可以利用快速幂在做幂运算的时候防止结果溢出。

   实际上，我们在做乘法时，还可以使用 $(a + b) \bmod P = ((a \bmod P) + (b \mod P)) \bmod P$ 的性质，将乘法计算拆分为加法运算。这个算法叫做**龟速乘**，感兴趣的同学自行了解。

6. 作业：

   P1257

   CF2200E

   P1504 （+ DFS）
   
   A2067
   
   CF2216D（CF2215B）
   
   ABC445E（难）
