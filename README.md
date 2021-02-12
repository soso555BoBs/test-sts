# 環境構築

## 環境
- macOS Big Sur
- STS 4
    - Maven 3.6.3
    - JRE 15.0.1
- Spring Boot 1.2.4
    - thymeleaf 2.1.4
- Git

## やったこと
1. STS 4 をダウンロード
    - https://spring.io/tools
2. インストール
3. 日本語化
    - https://mergedoc.osdn.jp/
    - 「Pleiades All in One ダウンロード」ではなく「Pleiades プラグイン・ダウンロード」

# プロジェクト作成

## 本来のやり方（？）
- https://b1tblog.com/2019/12/18/spring-boot1/
- 要は、STS4 で以下手順でプロジェクト作成していれば問題なかった
    - メニューバー > File > New > Spring Starter Project
- ただ、今回は Maven プロジェクトを作成して pom.xml をイチから追記していった
    - https://qiita.com/s_hino/items/42575eb30fbbe34226b0

## やったこと
1. [メニューバー] > [ファイル] > [新規] > [Mavenプロジェクト]
2. [シンプルなプロジェクトの作成（アーキタイプ選択のスキップ）] にチェックし [次へ]
3. 以下情報を入力し作成
    - グループID：com.example
    - アーティファクトID：test-sts2
4. 作成されたプロジェクト内の pom.xml に以下を追記
    - 各項目の詳細については是非ググってほしい

```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.2</version>
</parent>

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

5. プロジェクトを右クリックし、以下を実行
    - Maven→プロジェクトの更新→OK
    - 実行→Maven install
6. src/main/java 内に Example.java を作成

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) {
        SpringApplication.run(Example.class, args);
    }

}
```

7. プロジェクトを右クリックし、以下を実行
    - 実行→SpringBootApp
8. 起動確認
9. 以下にアクセス
    - http://localhost:8080/

## 心に留めておきたいこと（抜粋）

### SpringBootのすごいところ
1. 記述量が少なくて済む
    - Javaのプログラムは得てしてxml地獄やライブラリの輪廻に巻き込まれがちですが、SpringBootはこの辺がごっそりそぎ落とされているのが本当に楽ちんです。
2. 実装がとにかく楽
    - HTMLファイルのままJSP化できるThymeleaf、ソースの反映を高速に行ってくれるSpringLoaded、アプリケーションの状態をWebAPI経由で取得できるSprintBootActuator（localhost:8080/envみたいに使える）等のSpring選りすぐり便利ツールがまとめられています。Springのpomが部品ごとに記述するとしたら、SpringBootでは機能ごとにオールインワンでライブラリをpomで受け取ることができます。もちろん部品単品でもpomれます。

### SpringBootのここに注意
1. 細かいバージョン指定が面倒
    - SpringBootライブラリでは原則としてその時点で最新のライブラリを取得します。
    - springMVCだけ古いバージョンを使いたいなという場合には別途pomる必要がある上、最新版は依存関係ライブラリから除去できません。
    - 依存関係ライブラリも肥大化しがちです。
2. warファイルではなくjarファイルができる
    - ビルドするとjarファイルができます。これは注意しないといけません。
    - SpringBootにはtomcat8が組み込まれていてそのままの状態で動かせるのがSpringBootの強みですが、Tomcatが入っている既存のLinux環境で稼働させたい場合にはちょっと面倒なことになります。
    - 場合によってはjarファイルと一度分解してtomcat8を抜き取ってまたwarで固めて・・・という手順を踏むことになるわけですが、それではSpringBootの楽ちんでスピーディなところを丸々殺してしまうことになるわけでちょっと考え物です。
3. SpringFrameworkの理解が必要
    - いろいろ簡略化されてはいますが結局のところ仕組みはSpringなわけです。
    - Spring未経験者はそれなりに学習コストが必要だと思います。
    - Java初心者ともなると、少々厳しいと思います。

# Git 管理
1. [Git] パースペクティブを開く
2. ターミナルにて、プロジェクトフォルダにて git init
3. ローカルリポジトリーを追加
    - 新規に作成したプロジェクトフォルダを選択
