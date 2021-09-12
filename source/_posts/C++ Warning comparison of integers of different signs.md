---
title: C++ Warning comparison of integers of different signs
date: 2021-09-12 20:37:50
tags: C++
---
## 问题代码
```c++
std::string str = "abc"
for (int i = 0; i < str.length(); i++) {
    ...
}

// Warning: comparison of integers of different signs: ...
```

## 问题原因
我们定义的`i`是 integer 类型的数，而`str.length()`返回的是 unsigned int 类型的数。虽然在很多比较宽容的编译器下可以直接通过，但是还是会存在问题的。  
例如：  
```c++
unsigned int a = UINT_MAX; // 0xFFFFFFFFU == 4,294,967,295 
signed int b = a; // 0xFFFFFFFF == -1

for (int i = 0; i < b; ++i)
{
    // the loop will have zero iterations because i < b is always false!
}
```

## 解决方法
在比较时将`int`型转为`unsigned int`型，并判断是否为负（整型数已经溢出而未达到符号整型）。
```c++
unsigned int a = UINT_MAX; // 0xFFFFFFFFU == 4,294,967,295 

for (int i = 0; i < 0 || (unsigned)i < a; ++i)
{
    // The loop will have UINT_MAX iterations
}
```

***
参考：https://stackoverflow.com/questions/8350971/comparison-of-integers-of-different-signs-warning-with-xcode