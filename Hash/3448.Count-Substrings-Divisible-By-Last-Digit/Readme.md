### 3448.Count-Substrings-Divisible-By-Last-Digit

这题很容易想到能否用“前缀+Hash”的套路来做。

假设`yyyxxxi`被d除的余数是r，那么我们能否找出某一段后缀是能被d整除的呢？我们不禁会想，如果前缀`yyy`被d除的余数恰好也是r，那么是否意味着`xxxi`就是能被d整除的呢？其实并不是。`xxxi`能被d整除的充要条件其实是`yyy0000`和`yyyxxxi`对于d同余，而不是`yyy`和`yyyxxxi`对于d同余。

于是我们发现，要判定以i为结尾的substring是否能被d整除，我们在Hash里就不能查找有多少“原始的前缀”的余数，而是应该查找有多少“升级后的前缀”的余数。比如说，当我们考察`abcdi`时，我们在Hash里记录的其实应该是`a0000`,`ab000`,`abc00`,`abcd0`对于d余数。这样方便我们和`abcdi`对于d的余数进行对照。如果有同余的，则说明当前有对应一段后缀能被d整除。

`abcd0`对于d余数从哪里来，其实就是`abcd`对于d余数r，再做变化r*10%r.

`abc00`对于d余数从哪里来，其实就是`abc0`对于d余数r，再做变化得到`r*10%r`. 那么`abc0`对于d余数又从哪里来，其实就是`abc`对于d余数r，再做变化r*10%r. 

由此类推，我们只需要在每一轮，将Hash里存放的余数频次进行变化即可。比如说当前count里存放的是`a000`,`ab00`,`abc0`,`abcd`的余数频次，那么
```cpp
for (int r=0; r<k; r++)
  count_new[r*10%r] += count[r];
```
count_new里存放的就是`a0000`,`ab000`,`abc00`,`abcd0`的余数频次.

由此本题迎刃而解。我们循环10遍，对于d=0,1,2,..,9分别进行计算，逐位考察存在多少以当前字符结尾的substring能被d整除。采用上述的“同余前缀”的思路来计数。

