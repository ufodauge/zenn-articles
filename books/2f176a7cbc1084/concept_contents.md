---
title: Typst の文法 - コンテンツ環境
---

## コンテンツ環境

コンテンツ環境は、ユーザーがデフォルトで使用できる環境です。執筆画面で適当に文字を記述すれば、ある程度はそのまま PDF に反映されます。改行や強調文字、箇条書きなどは特別な記法があります。
また、`[ ... ]` で囲われた環境もコンテンツ環境です。

Typst で注意すべき点として、**改行**と**段落改行**が異なる事が挙げられます。段落改行は**空行**で表し、その名の通り段落を作ります。一方単なる改行は `\` で、段落を作りません。

下記では、主要なものに関して、 Latex での記法と、（分かる人向けになりますが）マークダウン記法をコメントで併記して比較しています。完全な互換ではありませんが、理解のためとしてご容赦ください（あと正直 Latex あんまり知りませんごめんなさい）。

### 強調文字

```typst
*強調文字*

// latex: \textbf{強調文字}
// md:    **強調文字**
```

### 斜体文字

```typst
_斜体文字_

// latex: \textit{斜体文字}
// md:    *斜体文字*
```

:::message alert
2023 年 7 月 5 日現在、斜体文字は日本語に対して適用されないようです。Latex でも同じだったような気もしますが…
:::

### リンク

latex ではいずれもパッケージの取り込みが必要です。

```typst
https://example.com
#link("https://example.com")[代替文字]

// latex: \url{https://example.com}
//        \href{https://example.com}{代替文字}
//
// md:    https://example.com
//        [代替文字](https://example.com)
```

### 見出し文字

```typst
= 見出し文字
== 見出し文字
=== ...

// latex: \section{見出し文字}
//        \subsection{見出し文字}
//        \subsubsection, \paragraph, \subparagraph
//
// md:    # 見出し文字
//        ## 見出し文字
//        ### ...
```

### 箇条書き

```typst
- 箇条書き
- ...

// latex: \begin{itemize}
//          \item 箇条書き
//          \item ...
//        \end{itemize}
//
// md:    * 箇条書き
//        * ...
```

### 通し番号つき箇条書き

```typst
+ 通し番号つき箇条書き
+ ...

// latex: \begin{enumerate}
//          \item 通し番号つき箇条書き
//          \item ...
//        \end{enumerate}
//
// md:    1. 通し番号つき箇条書き
//        1. ...
```

### 数式

比較が難しいので、 Katex や MathJax 記法との比較とします。

```typst
$ x != 1 $  (数式)
$x != 1$  (インライン数式)

// $$x \ne 1$$ (数式)
// $x \ne 1$ (インライン数式)
```

数式自体の文法等は次の項で説明します。

---

以降は特に比較を行わず、 Typst に関して記述します。

### ページ内リンク

ページ内リンクは下記のようにして対応付を行います。ただし、リンクが張れるものは、

- 番号付き見出し
- 番号付き数式
- 図
- 脚注
- 参考文献

に限られています。
見出しや数式を番号付きにするには、 `#set heading(numbering: "1.")` のような記述を予めしておく必要があります。詳しくは後ほど。

```typst
// 見出しや数式を番号付きにする
#set heading(numbering: "1.")
#set math.equation(numbering: "(1)")

// リンク元になる見出しなどのあとに
// ラベルを配置する
= heading <label>
$ O(n) = 2^n $ <eq>

// リンク
@label
@eq
```

### 生テキスト

マークダウン記法と一緒です。

バッククォート 1 つで対象文字を囲めば、インラインの生テキストになります。バッククォート 3 つ（` ``` `）で囲めば、改行も可能なコードブロックになります。また、` ```lua ... ``` ` のように、最初のバッククォート 3 つの直後に言語名をかけば、その言語のシンタックスハイライトがされます。

![コードブロック](/images/2f176a7cbc1084/codeblock.png)

### 単語リスト

単語の定義をリスト形式で表示する際に使用します。 `/ [Word]: [Description]` を開業して並べていくと、（デフォルトでは）単語の部分が太字表記になり、その右に単語の説明が入ります。

```typst
/ 単語: 単語の説明
/ 単語: 単語の説明
...
```

### コメント

今更ですがコメントは下記のとおりです。

```typst
// コメント
/*
  コメント
 */
```

### 関数、変数

`#変数名` や、 `#関数名(引数)` で、関数や変数を呼び出すことができます。

組み込みの関数・変数では、以下のようなものがあります。

```typst
// 組み込み変数
#emoji.face.fear \    // == 😱
#sym.arrow.filled.b \ // == ⬇
#math.exists \        // == ∃

// 組み込み関数
#lorem(30) 
#linebreak()
#strong([strong]) \
#raw("raw\n  text") \
#list(
  [Foundations],
  [Calculate],
  [Construct],
  [Data Loading]
)
```

組み込み変数の3つを見てもらえれば分かる人にはわかると思いますが、オブジェクトに対して**ピリオド（.）**をつければ、そのオブジェクトのプロパティや関数などにアクセスできます。JavaScript などでおなじみの記法ですね。

関数の方に関しては、いくつかしっかり触れておくことがあります。

`#linebreak()` は、そのまま **改行** をするための関数です。改行は `\ ` でもできますが、関数からも呼び出せるわけです。

`#strong()` は、コンテンツ（環境）を引数にとり、対象のコンテンツ（環境）を強調文字にします。これも `* ... *` で出来ます。

勘の良い方はわかると思いますが、今まで見てきた `\` や `* ... *` は、**関数を呼び出すためのマクロ**のようなものです。更に勘の言い方はわかると思いますが、関数で呼び出せることで、引数を更に追加し、スタイルを変更したりすることも出来ます。

例えば、先の `#strong()` では、フォントの太さ（weight）を変えることが出来ます。

```typst
#strong(
  delta: 100,
  [strong]
)
```

また、引数にコンテンツ（環境）を与えるときは、`#関数名[コンテンツ]`のように記述が可能です。

```typst
#strong[strong]
*strong*
```

自分で関数を定義する場合など、詳しいことはコード環境の項で説明します。
