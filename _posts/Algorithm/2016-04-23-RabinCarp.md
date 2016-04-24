---
title: Rabin-Carp算法
layout: post
category : [Algorithm]
tag : [string searching, hash]
---
{% include JB/setup %}

<h3>介绍</h3>
字符串匹配算法中，暴力匹配的时间复杂度为O(mn)，空间复杂度为O(1)。暴力匹配的时间主要消耗在当源串text与模式串pattern不匹配时，需要将匹配窗口右移一位，然后又从pattern的第0位开始匹配。<span class = "highlight-span">也就是说，它需要对pattern与text的每一位都进行匹配。</span>

![brute force]({{ site.url }}/assets/images/2016-04-23-bruteforce.png){: .center-image }

<span class = "figure-caption">图1. 暴力匹配</span>


Rabin-Carp算法优化了这个过程，它利用哈希函数计算pattern和当前text的匹配窗口所对应的哈希值，如果哈希值不同，则窗口直接右移一位；如果哈希值相同，再与pattern逐位匹配。

![Rabin-Carp]({{ site.url }}/assets/images/2016-04-23-Rabin-Carp.png){: .center-image }

<span class = "figure-caption">图2. Rabin-Carp算法</span>

<h3>Rabin指纹</h3>
Rabin-Carp中，利用Rabin指纹（Rabin fingerprint）计算字符串的哈希值，它是一个多项式：

<pre class="prettyprint lang-cpp">
f(str, x) = m<sub>0</sub> + m<sub>1</sub>x + ... + m<sub>n - 1</sub>x<sup>n - 1</sup>
</pre>

其中，m为字符的ASCII码值，m<sub>i</sub> = str[n - i - 1]，n = length(str)。比如，text = "bcabc"，pattern = "abc"，x = 101，那么pattern的哈希值为

<pre class="prettyprint lang-cpp">
f("abc", 101) = 'a' * 101<sup>2</sup> + 'b' * 101 + 'c' = 999,494  
</pre>

此外，如果已知text的一个匹配窗口的哈希值，也能快速计算其下一个匹配窗口的哈希值,

<pre class="prettyprint lang-cpp">
已知
f(text[0..2] = "bca", 101) = 'b' * 101<sup>2</sup> + 'c' * 101 + 'a' = 1,009,794
则
f(text[1..3] = "cab", 101) = (f(text[0..2] = "bca", 101) - 'b' * 101<sup>2</sup>) * 101 + 'b' = 1,019,794
</pre>

当字符串太大时，哈希值很有可能溢出，因此在实际应用中，我们可以将其取模，

<pre class="prettyprint lang-cpp">
f(str, x) = (m<sub>0</sub> + m<sub>1</sub>x + ... + m<sub>n - 1</sub>x<sup>n - 1</sup>) % mod
</pre>

<h3>实现</h3>

<pre class="prettyprint lang-cpp">
int strStr(string text, string pattern) 
{
    int n = text.size();
    int m = pattern.size();
    int mod = 11987;
    int base = 101;
    
    // preprocessing, calculate hash value of text[0..m) and pattern
    int hashOfpattern = 0;
    int hashOftext = 0;
    for(int i = 0;i < m && i < n; ++i)
    {
        hashOfpattern = (hashOfpattern * base + pattern[i]) % mod;
        hashOftext = (hashOftext * base + text[i]) % mod;
    }
    
    // e = pow(base, m - 1)
    int e = 1;
    for(int i = 0; i < m - 1; ++i)
        e = (e * base) % mod;
    
    // start to match    
    for(int i = 0; i <= n - m; ++i)
    {
        // text and pattern have the same hash value, then make a comparision one by one
        if(hashOftext == hashOfpattern)
        {
            // compare pattern with text[i..i + m - 1]
            int j = 0;
            int k = i;
            while(j < m && text[k] == pattern[j])
            {
                ++j, ++k;
            }
            // matched
            if(j == m)
                return i;
        }
        else if(i != m - n)
        {
            // calculate the next hash value
            hashOftext = (base * (hashOftext - text[i] * e) + text[i + m]) % mod;
            if(hashOftext < 0)
                hashOftext += mod;
        }
    }
    return -1;
}
</pre>


<h3>复杂度分析</h3>
假设源串text长度为n，模式串pattern长度为m，Rabin-Capin算法的时间复杂度为O(mn)，空间复杂度为O(1)。它在匹配前要计算哈希值，所以需要O(m)的预处理时间。理论上它并没有比暴力匹配好多少。但在实际使用中，Rabin-Carp算法的时间复杂度被认为是O(m + n)。因为最坏情况下，text所有的子串都要与pattern匹配，此时共有n - m + 1个子串，时间复杂度为O((n - m + 1)m)；然而一般来说，这种情况并不是常有的，所以我们假设需要匹配的期望子串数为c，则其期望时间复杂度为O(n + m - 1 + cm)，即为O(m + n)。

<h3>总结</h3>
Rabin-Carp算法效果一般，但它有以下特点：

1. 易于实现。
2. 能用于多模式匹配，如论文抄袭检测。


<h3>参考文献</h3>
1. [Rabin–Karp algorithm](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm)

2. [Rabin fingerprint](https://en.wikipedia.org/wiki/Rabin_fingerprint)

3. [Computer Algorithms: Rabin-Karp String Searching](http://www.stoimen.com/blog/2012/04/02/computer-algorithms-rabin-karp-string-searching/)

4. [图说Rabin-Karp字符串查找算法](http://www.ituring.com.cn/article/1759)
