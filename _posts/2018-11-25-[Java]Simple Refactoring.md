---
title: "[Java]Simple Refactoring"
date: 2018-11-25 14:26:28
categories: Java
permalink: /java/
---

# Simple Refactoring(Test-driven Development)

아래의 기준으로 만들어진 코드를, 좀 더 잘 코딩 된 형식으로 변경(Refactoring) 해보려 합니다.

* 쉼표나 콜론을 구분자로 가지는 문자열을 입력 받음
* 구분자를 기준으로 분리한 각 숫자의 합을 반환

# Usage

최초로 작성 된 코드 예제 입니다.
```java
public class StringCalculator {
    public static int splitAndSum(String text) {
        int result = 0;
        if (text == null || text.isEmpty()) {
            result = 0;
        } else {
            String[] values = text.split(",|:");
            for (String value : values) {
                result += Integer.parseInt(value);
            }
        }        
        return result;
    }
}
```
***
첫번째, 한 단계의 들여쓰기(indent)만 한다.

-> else 문 안에 있는 logic(sum)을 함수로 만들어, 가독성을 높힌다.


```java
public class StringCalculator {
    public static int splitAndSum(String text) {
        int result = 0;
        if (text == null || text.isEmpty()) {
            result = 0;
        } else {
            String[] values = text.split(",|:");
            result = sum(values);
        }
        return result;
    }

    private static int sum(String[] values) {
        int result = 0;
        for (String value : values) {
            result += Integer.parseInt(value);
        }
        return result;
    }
}
```
***
두번째, else를 하지 않는다.

-> return을 사용하여, else를 하지 않도록 한다.

```java
public class StringCalculator {
    public static int splitAndSum(String text) {
        if (text == null || text.isEmpty()) {
            return  0;
        }
        String[] values = text.split(",|:");
        return sum(values);
    }

    private static int sum(String[] values) {
        int result = 0;
        for (String value : values) {
            result += Integer.parseInt(value);
        }
        return result;
    }
}
```
***
세번째, 메서드가 한가지 일만 하게 한다.

-> parseInt와 sum을 각 메서드에서 하게 분리 한다.

```java
public class StringCalculator {
    public static int splitAndSum(String text) {
        if (text == null || text.isEmpty()) {
            return  0;
        }
        String[] values = text.split(",|:");
        int[] numbers = toInts(values);
        return sum(numbers);
    }

    private static int[] toInts(String[] values) {
        int[] result = new int[10];
        int i = 0;
        for (String value : values) {
            result[i++] = Integer.parseInt(value);
        }
        return  result;
    }

    private static int sum(int[] values) {
        int result = 0;
        for (int value : values) {
            result += value;
        }
        return result;
    }
}
```
***
네번째, 로컬 변수를 지양 한다.

```java
public class StringCalculator {
    public static int splitAndSum(String text) {
        if (text == null || text.isEmpty()) {
            return  0;
        }      
        return sum(toInts(text.split(",|:")));
    }

    private static int[] toInts(String[] values) {
        int[] result = new int[10];
        int i = 0;
        for (String value : values) {
            result[i++] = Integer.parseInt(value);
        }
        return  result;
    }

    private static int sum(int[] values) {
        int result = 0;
        for (int value : values) {
            result += value;
        }
        return result;
    }
}
```
***
다섯번째, Compose Method 패턴(추상화 정도가 같아야 함) 적용 한다.

-> null 체크 및 split 부분이 무엇을 의미하는지 추상화 한다.

```java
public class StringCalculator {
    public static int splitAndSum(String text) {
        if (isBlank(text)) {
            return  0;
        }
        return sum(toInts(split(text)));
    }

    private static boolean isBlank(String text) {
        if (text == null || text.isEmpty()) {
            return  true;
        }
        return false;
    }

    private static String[] split(String text) {
        return text.split(",|:");
    }

    private static int[] toInts(String[] values) {
        int[] result = new int[10];
        int i = 0;
        for (String value : values) {
            result[i++] = Integer.parseInt(value);
        }
        return  result;
    }

    private static int sum(int[] values) {
        int result = 0;
        for (int value : values) {
            result += value;
        }
        return result;
    }
}
```
___
# Reference
* https://www.youtube.com/watch?v=MRarMhbIdcQ
