---
title: "依存性注入(Dependency Injection)を理解する" # 記事のタイトル
emoji: "😀" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["di", "java", "spring", "springboot"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# 依存性注入(Dependency Injection)を理解する

## 依存性注入とは何か？

依存性注入とは、一つのオブジェクトが別のオブジェクトを活用する際に、そのオブジェクトを外部から供給する手法です。これにより、オブジェクト間の依存関係を解消することが可能となります。

## 依存性注入の利点

- テストの容易性が増す
- オブジェクト間の依存関係が明瞭になる
- オブジェクトの再利用性が向上する

## サンプルコードによる解説

ここでは、CarクラスとEngineクラスが存在する状況を考えます。CarクラスはEngineクラスを用いているため、CarはEngineに依存しているといえます。

### 依存性注入を適用しないケース

以下の例では、CarクラスのコンストラクタでEngineクラスのインスタンスを生成しています。これにより、Carクラスは特定のEngineクラス（この例では`V8`エンジン）に直接依存してしまいます。結果として、柔軟性が失われ、テストや再利用が難しくなります。

```java
class Car {
    private String name;
    private Engine engine;

    public Car(String name) {
        this.name = name;
        this.engine = new Engine("V8");
    }
}

class Engine {
    private String name;

    public Engine(String name) {
        this.name = name;
    }
}
```

### 依存性注入を適用するケース
依存性注入を適用した場合、CarクラスのコンストラクタはEngineクラスのインスタンスを引数として受け取り、Carクラスのインスタンス変数に代入します。これにより、Carクラスは特定のEngineクラスに直接依存しない形になります。つまり、Engineクラスの任意の具体的な実装をCarクラスに注入することが可能となります。

```java
class Car {
    private String name;
    private Engine engine;

    public Car(String name, Engine engine) {
      this.name = name;
      this.engine = engine;
    }
}

class Engine {
    private String name;

    public Engine(String name) {
      this.name = name;
    }
}
```
