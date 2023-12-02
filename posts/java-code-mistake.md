---
title: Java コード入門の誤り（indexOf／lastIndexOfメソッド）
description: hkのエンジニアブログ。Java コード入門というサイトの誤り（indexOf／lastIndexOfメソッド）の指摘とその検証のブログです。
draft: ture
date: 2022-11-12
updated: 2022-11-12
tags:
  - java
---

## はじめに

記載の誤りを見つけたので、検証とともに記載。  
誤りは<a href="http://java-code.jp" target="_blank">Java コード入門</a>というサイト。  
こちらの<a href="http://java-code.jp/187" target="_blank">indexOf／lastIndexOf メソッド</a>の記載に誤りがあります。

## 本題

> lastIndexOf メソッドでは、引数 index は、末尾から数えた検索開始位置を表します。

問題は上記の部分です。  
<a href="https://docs.oracle.com/javase/jp/8/docs/api/java/lang/String.html#lastIndexOf-java.lang.String-int-" target="_blank">Java SE8 の API 仕様（日本語）</a>では以下のように記載されています。

> 検索は指定されたインデックスから開始され、先頭方向に行われる

つまり、indexOf/lastIndexOf ともに第 2 引数で指定されたインデックスは 1 文字目を 0 として、どこから検索を始めるかを示しています。ちなみに上記サイトのサンプルコードは合ってます。

```java
String target = "あいうえおかきくけこ";
System.out.println(target.lastIndexOf("お", 1));// (A)：-1
System.out.println(target.lastIndexOf("お", 6));// (B)：4
```

Java コード入門の記載の通りであれば A の結果は「け」から検索を始めて「く」→「き」→「か」→「お」となり-1 の結果にはならないはずです。
第二引数の index は先頭からであり、以下のようなことをしているイメージを持っていただければと思います。

```java
String target = "あいうえおかきくけこ";
System.out.println(target.substring(0, 1).lastIndexOf("お"));// (A)：-1
System.out.println(target.substring(0, 6).lastIndexOf("お"));// (B)：4
```
