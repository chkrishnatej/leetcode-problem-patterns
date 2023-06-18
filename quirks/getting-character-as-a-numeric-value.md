# Getting Character as a numeric value

Sometimes when we need to get the numeric character into a numeric value, we might be using

```java
char ch = '9';
int num = Character.getNumericValue(ch);
```

But if we are working on something like stack-based problems where we convert the String `3[a20[c]]` to `charArray` , then the 20 will be divided into two characters like `2|0`. The value of 20 is missing and the `.getNumericValue(ch)` does not work here.&#x20;

```java
String s = "20";
char[] ch = ['2','0'];
int num = 0;

while (ch.isEmpty()) {
    num = num * 10 + ch - '0';
}
```

1st iteration --> (0 \* 10) + (2 - '0') = 0 + 2 = 2

2nd iteration --> (2 \* 10) + (0 - '0') = 20 + 0 = 20

**What does **_**`ch-'0'`**_ **even mean?**

> It is doing ASCII calculations. Every character has an ASCII value associated with it,`0` character ASCII value is 48 and `2` character ASCII value is 50. So when we perform the operation `ch-'0'`, what it is doing is `50-48`, i.e 2.

This particular trick is used in the following questions:

{% embed url="https://leetcode.com/problems/decode-string/description/" %}
