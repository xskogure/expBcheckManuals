<style type="text/css">
h1 {
    padding-bottom: 0em;
    padding-left: 0.5em;
    line-height: 1.2;
    border-bottom: solid 2px lightblue !important;
    border-left: solid 30px lightblue !important;
}

h2 {
    padding-bottom: 0em;
    padding-left: 0.5em;
    border-left: solid 20px darkcyan !important;
    border-bottom: solid 2px darkcyan !important;
}

h3 {
    padding-bottom: 0em;
    padding-left: 0.5em;
    border-left: solid 10px greenyellow !important;
    border-bottom: solid 1px greenyellow !important;
}

div.notewern {
    padding: 5px;
    background-color: #D0FFFF !important;
    border-left: solid 3px lightcyan !important;
    font-weight: bold !important; 
    border-radius: 5px !important;
}

div.noteinfo {
    padding: 5px;
    background-color: #D0FFD0 !important;
    border-left: solid 3px lightgreen !important;
    border-radius: 5px !important;
}

pre.shell {
    padding: 5px;
    background-color: #000000 !important;
    border-left: solid 3px lightgray !important;
    color: #F0F0F0 !important;
    border-radius: 5px !important;
}
</style>

# 2024情報科学実験B 実験1 チェック確認

## 1.1 BNFチェック

必要ないです

## 1.2 字句解析チェック (**T01_21CTokenizerTest.java**)

下記に関する Test コードを実装します．

[ ] マイナス 数字 のテスト: **minusFactor()**
```
-100
```

下記に実装例 (**T01_21CTokenizerTest.java**) を示します．

```java:T01_21CTokenizerTest.java
public class T01_21CTokenizerTest {

    CTokenizerTestHelper helper = new CTokenizerTestHelper();

    @Test
    public void minusNumber() {
        String testString = "-100";
        CToken[] exceptedTokenList = {
            new CToken(CToken.TK_MINUS, 1, 1, "-"),        // それぞれ「字句(Token)の種類」「行番号」「先頭からの開始位置」「認識される文字列」
            new CToken(CToken.TK_NUM, 1, 2, "100"),
            new CToken(CToken.TK_EOF, 1, 5, "end_of_file") // end_of_file は下記のテスト全てに必ずいれること！
        };
        helper.acceptList(testString, exceptedTokenList);
    }
}
```


**下記の test を各自実装してください**．なお，下記の文字列は，``String testString`` に代入するためのフォーマットとなっています．``TestCToken.java`` でのテストの際は，余計な文字 (""や\n + 等) は削除して ``test.c`` に書き込むようにしてください．

[ ] PLUS MINUS の後ろに インラインコメント: **inlineCommnetWithFactor()**
```
+ - // plus minus 記号
```

[ ] インラインコメントの次の行に数式: **lineCommentPlusExpression()**
```
"// LINE_COMMENT\n" +
"13 + 7 + 2"
```

[ ] 数字とブロックコメントの繰り返し: **blockCommentWithFactor()** **(下に実装例あり)**
```
"/***/123/*/12/*/34/*/56/*/78    // 123 34 78 が出てくるはず"
```

```
@Test
public void blockCommentWithFactor() {
    String testString = "/***/123/*/12/*/34/*/56/*/78    // 123 34 78 が出てくるはず";
    CToken[] exceptedTokenList = {
        new CToken(CToken.TK_NUM, 1, 6, "123"),
        new CToken(CToken.TK_NUM, 1, 17, "34"),
        new CToken(CToken.TK_NUM, 1, 27, "78"),
        new CToken(CToken.TK_EOF, 1, 54, "end_of_file")
    };
    helper.acceptList(testString, exceptedTokenList);
}
```

[ ] 閉じていないブロックコメント: **blockCommentError() **
```
"/***         // 閉じていないコメントはEOFが出るはず"
```


[ ] ブロックコメント複数行のあとの数式: **blockCommentPlusExpression()** **``lineCommentPlusExpression()`` とほとんど同じになるはず（行番号だけ違う）**
```
"/* COMMENT_START AND COMMENT_LINE1\n" +
"   COMMENT_LINE2\n" +
"   COMMENT_LINE3\n" +
"   COMMENT_LINE4 AND COMMENT_END */\n" +
"13 + 7 + 2"
```

## 1.3 構文解析テスト
下記確認します

### 1.3.1 isFirst()テスト (**T01_31IsFirstTest.java**)

実装したクラスの isFirst()メソッドが正しく実装できているか確認します．

+ [ ] ExpressionSub の IsFirst チェック 
```
-1
```

### 1.3.2 parse()テスト (**T01_32ParseTest.java**)

+ [ ] 下記で適切な Error を出力できていること (特に 3- のエラーメッセージを正しく記載できているかチェックします)
```
1+
-3
3-
+
```

## 1.4 意味チェックテスト

実験1 では本テストは関係ないので省略．

## 1.5 コード生成テスト (**T01_51CodeGenTest.java**)

+ [ ] 下記の演算が正しくできていることをシミュレータで示してください

```
7-2
13-7-3
```

+ [ ] 下記で正しいコードが生成できていることを Expression で確認 (Program でのチェックはしなくて良い)

```
7-2
13-7-3
13+7-3
13+7+3 // これは元々できているので，足し算のみに影響が出ないことの確認
13+/*comment*/7+3
1+2-3+4-5   // 実装を間違えると，+4-5 が認識できずエラーとなる
```
