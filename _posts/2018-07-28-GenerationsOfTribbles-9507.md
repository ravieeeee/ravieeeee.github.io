---
layout: post
title: "9507번) Generations of Tribbles"
date:   2018-07-28
categories: [Algorithm]
icon: icon-java
---

# 문제
꿍은 군대에서 진짜 할짓이 없다. 그래서 꿍만의 피보나치를 만들어보려고 한다. 기존의 피보나치는 너무 단순해서 꿍은 좀더 복잡한 피보나치를 만들어보고자 한다. 그래서 다음과 같은 피보나치를 만들었다. 꿍만의 피보나치 함수가 koong(n)이라고 할 때,

* n < 2 : 1

* n = 2 : 2

* n = 3 : 4

* n > 3 : koong(n − 1) + koong(n − 2) + koong(n − 3) + koong(n − 4) 이다.

여러분도 꿍 피보나치를 구해보아라. [baekjoon](https://www.acmicpc.net/problem/9507)

<br>

# 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.math.BigInteger;

public class GenerationsOfTribbles_9507 {
    static BigInteger[] dp;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int t = Integer.parseInt(br.readLine());
        StringBuilder sb = new StringBuilder();
        dp = new BigInteger[68];
        dp[0] = BigInteger.ONE;
        dp[1] = BigInteger.ONE;
        dp[2] = BigInteger.valueOf(2);
        dp[3] = BigInteger.valueOf(4);

        while (t-- > 0) {
            int n = Integer.parseInt(br.readLine());
            sb.append(koong(n) + "\n");
        }

        System.out.println(sb.deleteCharAt(sb.length() - 1));
    }

    static BigInteger koong(int n) {
        if (dp[n] != null) return dp[n];

        return dp[n] = koong(n - 1).add(koong(n - 2)).add(koong(n - 3)).add(koong(n - 4));
    }
}
```

<br>

# 풀이
개꿀!
