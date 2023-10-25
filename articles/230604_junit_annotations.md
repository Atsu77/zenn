---
title: "JUnitのアノテーションについて" # 記事のタイトル
emoji: "😺" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["java", "Java", "junit", "JUnit"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# JUnit のアノテーションについて

## はじめに

各アノテーションがどのような役割を持っているかを説明します。

## アノテーション

### @Test

メソッドに付与することでテストメソッドとして認識される。

### @BeforeAll

クラス内のテストメソッドが実行される前に一度だけ実行される。

### @BeforeEach

クラス内のテストメソッドが実行される前に毎回実行される。

### @AfterEach

クラス内のテストメソッドが実行された後に毎回実行される。

### @AfterAll

クラス内のテストメソッドが実行された後に一度だけ実行される。

### @Disabled

テストメソッドを無効化する。

### サンプルコード

```java
import org.junit.jupiter.api.*;

public class Test {
    @BeforeAll
    static void beforeAll() {
        System.out.println("Before All");
    }

    @BeforeEach
    void beforeEach() {
        System.out.println("Before Each");
    }

    @AfterEach
    void afterEach() {
        System.out.println("After Each");
    }

    @AfterAll
    static void afterAll() {
        System.out.println("After All");
    }

    @Test
    void test1() {
        System.out.println("Test 1");
    }

    @Test
    void test2() {
        System.out.println("Test 2");
    }

    @Disabled
    @Test
    void test3() {
        System.out.println("Test 3");
    }
}
```

### 実行結果

```text
Before All
Before Each
Test 1
After Each
Before Each
Test 2
After Each
After All
```

## まとめ

JUnit のアノテーションについて説明しました。