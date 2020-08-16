# Java プロジェクトの作り方（Gradle を使って）
Gradle を使って、ローカルに Java プロジェクトを作成する。

ここでは、以下の環境を想定している。
- Windows 10 Home
- Git for Windows をインストール済み
- Gradle をインストール済み

Gradle をインストールしていない場合は、以下のページを参照する。

[]()

Scoop を導入している場合は、Git for Windows はインストールされているはず。

## CLI を起動する
Gradle コマンドを実行するために、今回はコマンドラインインタフェース（CLI）として Git Bash を利用する。

Win キーを押して、`gitbash` と入力して、Git Bash を起動する。

## Gradle コマンド
まずは、ユーザデスクトップに移動する。
```console
cd /c/User/＜ユーザー名＞/Desktop
```

次にデスクトップにプロジェクト置き場とするディレクトリを作成する。
```console
mkdir ./first.gradle.java
```

ディレクトリ内に移動する。
```console
cd ./first.gradle.java
```

次のコマンドで Gradle プロジェクト作成をスタートする。
```console
gradle init
```
実行結果:
```
Starting a Gradle Daemon (subsequent builds will be faster)

Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4]
```

作成するプロジェクトのタイプには「2: application」を選択する。
実行結果: 
```
Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Swift
Enter selection (default: Java) [1..5]
```

実装する言語には「3: Java」を選択する。
実行結果: 
```
Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2]
```

Gradle のビルドスクリプト記述言語には「1: Groovy」を選択する。
実行結果: 
```
Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit 4) [1..4]
```

テスティングフレームワークには「JUnit Jupiter」を選択する。
これで Junit のバージョン 5 以上を利用できる。

実行結果: 
```
Project name (default: first.gradle.java):
```

プロジェクト名を求められたら、ブランクで Enter キーを押下。
これで、デフォルトの `first.gradle.java` がプロジェクト名に利用される。

実行結果: 
```
Source package (default: first.gradle.java): 
```

今回はソースパッケージ名に `com.github.fs5013.furi.sutao` と入力してみる。

実行結果: 
```
> Task :init
Get more help with your project: https://docs.gradle.org/6.6/userguide/tutorial_java_projects.html

BUILD SUCCESSFUL in 12m 27s
2 actionable tasks: 2 executed
```

## VSCode でコードを確認する

VSCode で `first.gradle.java` フォルダを開く。すると、以下の通知メッセージが表示される（Language Support for Java の設定を変更していない場合）。

```
The workspace contains Java projects, would you like to import them?

Source: Language Support for Java(TM) by Red Hat
```

「YES」ボタンを押して、Java サーバを使えるようにする。

## 補完機能の確認
`src` フォルダ配下に生成された `App.java` フォルダを開いて、補完機能が効くことを確認する。

main メソッド内に `sysout` と入力してみる。
補完メニューで補完候補リストが、入力位置の脇に表示されれば、補完機能が効いている。

## コンパイラが働いているかの確認
わざと間違ったコードを記述して、コンパイラに間違いを指摘されるかを確認する。

例えば、以下のコードを main メソッド内に記述してみる。
```java
int num = "number";
```

赤い波線が表示されれば、コンパイラが働いている。

## コードの実行
Gradle をインストールしているユーザ向けには、以下のコマンドで Java プロジェクトを実行できる。

```console
gradle run
```
実行結果:
```
> Task :run
Hello world.

BUILD SUCCESSFUL in 1s
2 actionable tasks: 1 executed, 1 up-to-date
```

Gradle がローカルにインストールできていないユーザ向けには、以下のコマンドで実行バッチを起動することができる。

```console
./gradlew run
```
実行結果:
```
> Task :run
Hello world.

BUILD SUCCESSFUL in 3s
2 actionable tasks: 2 executed
```

## テストの実行
`gradle init` ではテストコードも生成される。

`src` ディレクトリ配下には `test` ディレクトリが生成されていて、この配下にテストコードが生成されている。

```java
/*
 * This Java source file was generated by the Gradle 'init' task.
 */
package com.github.fs5013.furi.sutao;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class AppTest {
    @Test void appHasAGreeting() {
        App classUnderTest = new App();
        assertNotNull(classUnderTest.getGreeting(), "app should have a greeting");
    }
}
```

次のコマンドでテストを実行する。
```console
gradle test
```
実行結果:
```
BUILD SUCCESSFUL in 2s
3 actionable tasks: 2 executed, 1 up-to-date
```

もちろん、こちらも Gradle をインストールしていなければ、`gradlew` バッチを実行することでテスト実行が可能。

```console 
./gradlew test
```

## テストが失敗することを確認する
テストコードを変更してみる。

変更前:
```java
assertNotNull(classUnderTest.getGreeting(), "app should have a greeting");
```

変更後: 
```java
assertEquals(classUnderTest.getGreeting(), "Hello new world.");
```

テストを再実行してみる。
```console
gradle test
```
実行結果:
```
> Task :test FAILED

com.github.fs5013.furi.sutao.AppTest > appHasAGreeting() FAILED
    org.opentest4j.AssertionFailedError at AppTest.java:12

1 test completed, 1 failed

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':test'.
> There were failing tests. See the report at: 
file:///C:/Users/＜ユーザ名＞/Desktop/first.gradle.java/build/reports/tests/test/index.html

* Try:
Run with --stacktrace option to get the stack trace. 
Run with --info or --debug option to get more log output. 
Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 2s
3 actionable tasks: 3 executed
```

想定通り、テストが失敗したことを確認できた。














