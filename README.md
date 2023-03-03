# サバイバルTypeScript <!-- omit in toc -->
- [【１章】TypeScript のあらまし](#１章typescript-のあらまし)
  - [TS の特徴](#ts-の特徴)
  - [TS の誕生](#ts-の誕生)
  - [TypeScript とエコシステム](#typescript-とエコシステム)
    - [パッケージマネージャー](#パッケージマネージャー)
    - [モジュールバンドラー](#モジュールバンドラー)
    - [トランスパイラー](#トランスパイラー)
    - [Git フック](#git-フック)
  - [なぜ TypeScript を使うべきか](#なぜ-typescript-を使うべきか)
    - [古いJS環境を対象とした開発シーンでも最新の構文が使える](#古いjs環境を対象とした開発シーンでも最新の構文が使える)
  - [静的型付け](#静的型付け)
  - [ECMAScript](#ecmascript)
    - [JavaScriptとECMAScriptの関係](#javascriptとecmascriptの関係)
    - [ECMAScript とブラウザの仕様](#ecmascript-とブラウザの仕様)
    - [ECMAScript とブラウザの関係性](#ecmascript-とブラウザの関係性)
  - [TypeScript の射程](#typescript-の射程)
    - [フロントエンドとバックエンドの両方でTypeScriptを採用するメリット](#フロントエンドとバックエンドの両方でtypescriptを採用するメリット)
    - [デスクトップアプリケーション](#デスクトップアプリケーション)
    - [WebAssembly](#webassembly)
  - [TypeScript は何ではないか？](#typescript-は何ではないか)
    - [実行時の高速化・省メモリ化に影響しない](#実行時の高速化省メモリ化に影響しない)
    - [JavaScriptの仕様バグは修正しない](#javascriptの仕様バグは修正しない)
- [【2章】作って学ぶTypeScript](#2章作って学ぶtypescript)
  - [開発環境の準備](#開発環境の準備)
    - [Node.js とは](#nodejs-とは)
  - [簡単な関数を作ってみよう](#簡単な関数を作ってみよう)
    - [コンパイラを働かせる](#コンパイラを働かせる)
  - [Reactでいいねボタンを作ろう](#reactでいいねボタンを作ろう)
    - [Reactの3大特徴](#reactの3大特徴)
    - [tsxって何？TypeScriptの中にHTMLが書ける？](#tsxって何typescriptの中にhtmlが書ける)
    - [関数コンポーネント](#関数コンポーネント)
    - [JSXのセルフクロージング要素](#jsxのセルフクロージング要素)
    - [class属性は使わない？](#class属性は使わない)
# 【１章】TypeScript のあらまし
## TS の特徴
- TypeScriptで書かれたコードは純粋なJavaScriptにコンパイルされる
- 型推論がある
- オープンソース

## TS の誕生
- JavaScriptの文法を拡張するに留める「JavaScriptのスーパーセット言語」としての戦略を採用
- TypeScriptを導入したとしても、既存のJavaScript資産はそのまま活用

## TypeScript とエコシステム
### パッケージマネージャー
- npmやYarnでインストールされるライブラリは、npmjs.comにホスティング
- npmjs.comは中央集権型のレジストリ

### モジュールバンドラー
- 複数のJavaScriptファイルをひとつのファイルに結合するためのツール
- 複数のJavaScriptファイルに依存関係がある場合、それをそのままブラウザに読み込ませるには、慎重に読み込み順を指定しないと、アプリケーションが破壊数
- 多くのファイルからなるアプリケーションをモジュールバンドラーで1ファイルに

### トランスパイラー
- あるプログラミング言語でかかれたコードを、別の言語に変換するツール
- JavaScriptでは、新しいバージョンのJavaScriptから古いバージョンのJavaScriptに変換するトランスパイラーがある
  - Babelやswc
- TypeScriptのtsc(TypeScript compiler)もトランスパイラー
  - tscはTypeScriptからJavaScriptへの変換

### Git フック
- Gitにコミットするタイミングや、プッシュするタイミングに、何らかのプログラムを実行する機能
- JavaScriptの開発現場では、Gitコミット時に
  - TypeScriptのコンパイルで型チェック
  - リンターのチェックを起動
  - コードフォーマッターでコード整形
- Gitフックを手軽に利用できるようにするツール
  - husky
  - lint-staged

## なぜ TypeScript を使うべきか
### 古いJS環境を対象とした開発シーンでも最新の構文が使える
- TypeScriptは、そのような古いJS環境をターゲットとしたプロジェクトにも導入可能
- TypeScriptはコンパイル時にどのバージョンのJavaScriptにコンパイルするかを選べる
- TypeScriptはECMAScriptへの準拠を原則としており、ECMAScriptで採択された新機能はいち早く取り入れらる
- TypeScriptはESNextと言われる次のECMAScriptに導入される言語仕様も先取りして取り入れる
  - 未来のJavaScriptで使用可能になることがほぼ確定している言語仕様も先取り

※注意が必要なのは、新しいAPIの利用  
たとえば、ECMAScript 2015(ES6)で導入されたMapクラスやSetクラスを使ったコードをTypeScriptで書き、それをES5にコンパイルすると、構文エラーは起きないものの、MapクラスやSetクラスを使う部分は実行時エラーになります。これは、TypeScriptはあくまで新しい構文を古いJSコードに変換する仕事だけを担っているためです。もし、古い環境で新しいAPIを使う必要がある場合は、core-jsなどのポリフィルを併用することで解決することができます。

## 静的型付け
- 動的型付け: 実行時に変数の型が定まる言語
  - 型にまつわる問題はプログラムを実行してみないと発覚しない
- 静的型付け: コンパイル時に変数の型が定まる言語
  - 型にまつわる問題はプログラムを実行しなくても発見
- 型注釈: **あなた専属のコードレビュアであるコンパイラを育てるための投資**

## ECMAScript
### JavaScriptとECMAScriptの関係
- ECMAScriptはJavaScriptの仕様を定義したもの
  - 仕様＝決まりごと
  - ブラウザなどがJavaScriptを読み込んだときに、どのような文法を解釈しなければならないか、処理がどのように動くべきかといったことを決めたもの
- 歴史的には、JavaScriptを標準化するべく、ECMAScriptが策定
  - JavaScriptはNetscape社が開発した言語
  - Microsoft社もJScriptという名前で実装
  - Netscape社は、JavaScriptを標準化するために、国際規格策定団体Ecmaインターナショナルに依頼
  - Ecmaによって標準化されたJavaScriptは「ECMAScript」と呼ばれる
- 現在のECMAScriptは、JavaScriptの仕様という立場
- ECMAScriptは仕様なので、ECMAScriptというプログラムがあるわけではない
  - ダウンロードしたりインストールするものではないということ

### ECMAScript とブラウザの仕様
- ECMAScriptが決めるクライアントサイドJavaScriptの仕様は部分的
  - ECMAScriptが定める範囲は、言語の文法、構文の解釈方法、コアのAPIなど言語の中核部分
- JavaScriptのうちブラウザ仕様に関する部分は、HTML Living Standardが決める
- JavaScriptの機能の中でも、ECMAScriptとHTML Living Standardで役割分担があるものがある
  - ECMAScriptはモジュールの仕様を定める
    - importやexportの構文や、モジュール内部の仕様など
  - 一方、モジュールの具体的なロード方法はHTML Living Standardが定める

### ECMAScript とブラウザの関係性
- ブラウザの内部を分解すると、レンダリングエンジンやJavaScriptエンジンと呼ばれる部品の単位がある
- JavaScriptエンジンは、ECMAScriptを実装したモジュール
  - JavaScriptエンジンの主要なものは V8、SpiderMonkey、JavaScriptCoreがある
- レンダリングエンジンは、JavaScriptエンジンを組み込んだブラウザの表示機能を担うモジュール
  - 有名なレンダリングエンジンに、Blink、Gecko、WebKitがある
  - レンダリングエンジンはJavaScriptだけでなく、HTMLやCSSを解釈し、画面描画を総合的に行う
  - Google ChromeはBlinkを、SafariはWebKitを組み込んでいる
  - 同じブラウザブランドでも、iOS版のブラウザのレンダリングエンジンはWebKit
    - たとえば、Google ChromeはBlinkを採用しているが、iOS版のGoogle ChromeのレンダリングエンジンはWebKit
  - **iOSのレンダリングエンジンにWebKitだけが独占的に利用を許されているため**
- **エンジンを知ることは、開発したプログラムが実行される環境を把握することと同義**
- プログラムをテストするときも、ブラウザとエンジンの組み合わせが理解できていると、同じエンジンを採用しているブラウザはテストを省略できる場合もある
- iOSのWebKit独占の例のように、同じブランド名のブラウザでもエンジンが異なる場合がある
  - この場合、テストで網羅するブラウザを増やすといった意思決定が行えるようになる

## TypeScript の射程
### フロントエンドとバックエンドの両方でTypeScriptを採用するメリット
- モジュールを共有できる
  - 両方でTypeScriptを採用すると、一方で作ったTypeScriptのモジュールをもう一方で再利用可能

### デスクトップアプリケーション
- WindowsやmacOS、Linux向けにデスクトップアプリケーションを作る場合もTypeScriptが使える
- デスクトップアプリケーションをJavaScript、HTML、CSSの技術スタックで開発できる **Electron** を用いる
- TypeScript製の著名なアプリとしては、SlackやMicrosoft社のVS Code など

### WebAssembly
- WebAssembly(WASM)はブラウザで動くアセンブリー言語
  - WASMはJavaScriptよりも高速な処理が必要とされるところで用いられる
- WASMのプログラムはC言語やC++、Rustといったシステム言語で開発されることが多い
- TypeScriptでWASMを開発できるようにする試みも出てきている
  - その筆頭が AssemblyScript

## TypeScript は何ではないか？
### 実行時の高速化・省メモリ化に影響しない
- TypeScriptの実行時パフォーマンスはJavaScriptと同じ
  - TypeScriptのランタイムはない
    - TypeScriptを直接実行するエンジンがない
    - TypeScriptで書いたコードを実行するには、一度JavaScriptコードに変換する必要がある
    - TypeScriptのパフォーマンスは、**コンパイル後のJavaScriptがどうなっているかで決まる**
  - TypeScriptコンパイラは最適化しない
    - 一般的に「コンパイラ」には、次の3つの仕事があると言われる
      1. ソースコードを解析し、問題点をチェックする
      2. ソースコードを別の言語に変換する
      3. 最適化する
         - 実行速度が速くなるようにする
         - 少ないメモリで動くようにする
         - 少ない電力で済むようにする
         - 実行ファイルのサイズを小さくする
    - このうち、TypeScriptコンパイラがするのは上の2つ
    - 3つ目の最適化はしない
  - TypeScriptコンパイラがするのは**基本的に型を外すだけ**

### JavaScriptの仕様バグは修正しない
- JavaScriptには元々バグだったものが仕様に変わった例がある
  - たとえば、値の型を調べるtypeof演算子は、nullを渡すと"object"が返る
  - これはバグと考えられていたが、後方互換性のため修正されることなく仕様になった
- TypeScriptでも、こうした仕様バグは修正されていない
  - TypeScriptはあくまでJavaScriptに型をプラスした言語というスタンスだから

# 【2章】作って学ぶTypeScript
## 開発環境の準備
### Node.js とは
- JavaScriptをサーバーサイドで動かすことを目的に開発されたソフトウェア
  - Node.jsはGoogle ChromeのJavaScriptエンジンと同じ「V8」でJavaScriptを実行
  - 組み込みのAPIは異なる
- ブラウザはJavaScriptからHTMLを操作する必要があるのでDOM APIがある
- Node.jsには、サーバーサイドのさまざまなリソースを制御する必要がある
  - ファイルシステムにアクセスするAPIやHTTPサーバーを建てるためのAPI、プロセスを起動したり終了したりするAPIなどがある
- TypeScriptでの開発も、TypeScriptコンパイラ(tsc)を動かすのにNode.jsが必要

## 簡単な関数を作ってみよう
### コンパイラを働かせる
- コンパイラに与えるヒントのことを「型注釈(type annotation)」と言う
```
function increment(num: number) {
//                 ^^^^^^^^型注釈
  return num + 1;
}
```
- TypeScriptコンパイラのコマンドは`tsc`
```
tsc increment.ts
```

## Reactでいいねボタンを作ろう
### Reactの3大特徴
- 特徴その1: 仮想DOM
  - そもそもDOM(document object model)とは、HTMLをJavaScriptから参照・操作する仕組み
    - HTMLを文字列操作ではなく、オブジェクトとして処理できる
    - DOMはHTMLを操作するためのAPIのようなもの
  - 仮想DOMはリアルDOMのプロキシのようなもの
    - リアルDOMと比べて、状態管理上のバグを起こしにくい設計になっている
    - パフォーマンス面では描画処理の最適化もする
    - 仮想DOMに起こった変更はリアルDOMに伝わり画面に現れる
- 特徴その2: 宣言的UI
  - そもそも**命令的なコード**とは
    - 何かを表示したい場合でもどのように表示するかのhowの部分を細かく書く必要がある
    - 命令的なコード例
```
    const list = document.createElement("ul");
const apple = document.createElement("li");
apple.innerText = "リンゴ";
list.append(apple);
const orange = document.createElement("li");
orange.innerText = "オレンジ";
list.append(orange);
const grape = document.createElement("li");
grape.innerText = "ぶどう";
list.append(grape);
```
  - 宣言的なコード例
    - どのように表示するかの部分はなく、「このような表示になってほしい」という目標だけを書く
    - 実装の細部やアルゴリズムを気にしなくてよい
    - 「どんなUIにしたいか」の一点に集中してコードを書く
```
function Fruits() {
  return (
    <ul>
      <li>リンゴ</li>
      <li>オレンジ</li>
      <li>ぶどう</li>
    </ul>
  );
}
```
- 特徴その3: コンポーネントベース
  - コンポーネントというのはUIの部品のこと
    - 小さいもので言えばボタンや入力欄、より大きめの部品だとフォーム
    - さらに大きい部品ではページもコンポーネント
  - Reactには、小さいコンポーネントを組み合わせ、大きなアプリケーションを成すという思想がある
  - オープンソースのコンポーネントも数多く公開
    - カレンダーUIのような自力で作ると面倒なコンポーネントも種類豊富に公開されている

### tsxって何？TypeScriptの中にHTMLが書ける？
- HTMLに見える部分はJSXと言われるもの
  - JSXはJavaScriptを拡張した言語で、JavaScriptの中にXMLを直接書けるようにしたもの
- TypeScriptとJSXは本来無関係の言語ですが、開発者の利便性のために、TypeScriptでもJSXが書けるようになっている
- JSXを書いたJavaScriptファイルは拡張子を`.jsx`にする
  - 同様にTypeScriptファイルは`.tsx`にする

### 関数コンポーネント
- ReactのJSXでは、HTMLタグのdivやheaderが使えるだけでなく、自分で定義した関数もタグとして使うことができる
  - JSXを戻り値として返す関数だけがタグとして使える
- JSXを戻り値にする関数をReact用語で「関数コンポーネント」と言う

### JSXのセルフクロージング要素
- JSXはJavaScriptの拡張構文であり、厳密にはHTMLと異なるもの
- そのため、JSXにはHTMLとは異なる書き方や制約がある
- `<LikeButton />`のようにスラッシュをタグに含める書き方も、JSXならではの書き方
  - これはセルフクロージング要素(self-closing element)と呼ばれる
    - 自己閉じ要素、自己完結型要素と呼ばれることもある
  - `<LikeButton></LikeButton>`のように子要素などを持たない場合に、`<LikeButton />`のように末尾に`/`をつけることで、短く表現できる

### class属性は使わない？
- HTMLではCSSクラスを指定するのに`class`属性を用いる
- ここでは`className`属性にしている
  - これは初期のReactがDOMプロパティに直接値をセットしていた名残り
- DOMでは、HTMLの`class`属性が`className`プロパティになる
  - 現在は、ReactがDOMプロパティを直接セットすることがなくなったので、`className`属性に縛られる技術的理由はない
  - React開発陣は`class`属性への乗り換えは慎重
  - これまで作られたコンポーネントが動かなくなるかも知れないから
  - また、両方サポートする気もない
    - `class`と`className`のどちらもOKとなると混乱を招くから
