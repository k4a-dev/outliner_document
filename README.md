# Twirliner

- [デモ](https://twirliner.k4a.me/demo?id0=root)

## コンセプト

- 純粋な２ペイン式
- 純粋なアウトライナ × 複数のビュー
- キーボードファースト

## なぜ作ったか

アウトライナーのすべてを行で表す簡潔さと構造操作の自由度に惚れ込み、情報整理やタスク管理のツールとして毎日使用している。

しかし、既存のアウトライナーにはそれぞれ痒いところに手が届かない部分が存在する。  
不満を感じて他のアウトライナーを探すという行動を何度か繰り返すたびに、どうせなら自分で作ってみようと思い作成に乗り出した。

## 概要

コンセプトに記載した 3 点の達成を目標とした。

### 1. 純粋な 2 ペイン式

今時のテキストエディタでは当たり前ではあるが、とても重要な機能である。

アウトライナーは通常の文章より行を区切るため、縦方向に長いツリーが展開される。
情報の書き出し中は問題ないが、内容を整理する段階では全体の見通しが悪くなる。
このツリーを左右並列に並べることで、縦に長い情報をや離れた位置の情報を気軽に整理することができる。

既存のアウトライナーには以下の様な製品が多い

- 1 ペインでしか開けない
- サイドペインを開けるが、あくまで「メイン」と「サイド」の関係で、両方対等な操作ができない

ブラウザで操作ができるアウトライナーでは 2 つのウィンドウを並べることで視認性は確保できるが、ウィンドウ間のドラッグアンドドロップは実現できない。
2 ペインをネイティブでサポートすることで、ペイン間のドラッグアンドドロップを実現させる。

### 2. 純粋なアウトライナー × 複数のビュー

アウトライナーはすべてが行という制約があるからこそ構造化された情報の作成が容易となる。
テキスト（改行を容認する）形式や、アウトライナー構造ではない特殊な入力形式などを実装すると、特定の場面では便利だが、操作の一貫性がなくなりアウトライナーとしての利点を殺すこととなる。

しかし行が積み重なった形式だけでは整理しにくい情報も存在する。タスク管理にはカンバン形式が、2 軸の情報整理にはテーブル形式が有用であることは否定できない。

上記 2 つの事柄を矛盾なく解決するために、目的に合った複数の表示形式（ビュー）の切り替えを純粋なアウトライナー構造を保ったまま実装する。

### 3. キーボードファースト

アウトライナーは、行をキーボードで自由自在に動かして情報構造を柔軟に変化させられる点に大きな魅力がある。
マウスを挟まなければ実現できない操作があると、情報の書き出しにブレーキを書けることとなる。

Twirliner では、ペイン間のアイテム移動を除きすべての操作をマウスだけで実現させることを目指した。

## 名前の由来

アウトライナーは一度情報を書き出して終わりではなく、書き出した情報構造をどんどん変化させて情報整理を行う。

頭の中や実際に書き出した情報を「Twirl(くるくると回す)」ようなソフトウェアにしたいという願いを込めて「Twirl」を選んだ。
他にも同様の単語があるが、「Outliner」の「l」をそのままつなげられること、語感が近い「Twilight（薄明り）」が未完成の情報を扱うアウトライナーというツールに似合っていることからこの単語を採用した。

## 機能

### 基本機能

- ノードの移動
- ノードの展開、収納
- ノードへのズームイン、ズームアウト
- ノードのコピー、カット、ペースト
- ノードのドラッグアンドドロップでの移動
- 左右 2 ペインの表示
  - 2 ペイン間のアイテム移動（ドラッグアンドドロップ）
- 改行されたテキストからネストを作成してペースト
- 検索
- 内部リンク
- 完了状態の切り替え
- タスク状態の切り替え
- バックリンク、ミラーリンク
  - バックリンク：内部リンクを張られた側が張った側の情報へのリンクを保持する
  - ミラーリンク：内部リンク先の情報を複製して表示する

### 応用機能

- カンバン、グリッド、スクラップ形式での表示、操作
- マークダウン＋一部独自記法でのスタイリング([詳細](https://twirliner.k4a.me/shortcuts))
- 複数形式での出力
  - plain text
  - opml
  - html
  - json(debug)
- ダーク、ライトモード切り替え
- バックリンク、バレット、ボーダー表示非表示の切り替え

## システム構成

### フロントエンド

- 開発言語
  - javascript(Typescript)
- フレームワーク
  - Next.js (React)
- ホスティング
  - Netlify

### バックエンド

- 開発言語
  - javascript(Typescript)
- サーバ環境
  - Node.js + Express
- ホスティング
  - Heroku

### 認証

- IDasS
  - Auth0

### ~~データベース~~

- DBMS
  - Postgres(Heroku)
- ORM
  - Prisma

※運用の都合上、現在はインメモリで動作

## 開発の工夫

React を代表とする仮想 DOM は、javascript のオブジェクトから DOM を作成する宣言的な UI 作成を実現している。
これは非常に優れた概念で、バグを生みにくく変更に強いシステム開発が可能であることは間違いない。

しかし、アウトライナーのように多重で深く、再描画が多い構造には不向きな点があると感じた。  
※私の知識が乏しいため、宣言的な書き方だけで実現できる可能性はあります。

ネストが深い要素を更新する場合を考える。
Outliner コンポーネントでは `ItemType` の配列を State として保持し、この配列からアウトライン構造を作成する。

```typescript
type ItemType = { id: string; content: string; children: string[] }
```

```typescript
const [itemList, setItemList] = useState<ItemType[]>([
  { id: '0', content: 'root', children: ['0-0', '0-1'] },
  { id: '0-0', content: '1階層目:0', children: ['0-0-0'] },
  { id: '0-0-0', content: '2階層目:0', children: ['0-0-0-0'] },
  { id: '0-0-0-0', content: '3階層目:0', children: ['0-0-0-0-0'] },
  { id: '0-0-0-0-0', content: '4階層目:0', children: [] },
  { id: '0-1', content: '1階層目:1', children: [''] },
])
```

上記の配列から以下の様な構造が作成される

```markdown
- root(0)
  - 1 階層目:0(0-0)
    - 2 階層目:0(0-0-0)
      - 3 階層目:0(0-0-0-0)
        - 4 階層目:0(0-0-0-0-0)
  - 1 階層目:1(0-1)
```

ここで`4階層目:0(0-0-0-0-0)`の DOM を更新する時、Stete ではない配列の値を書き換えるだけであれば、`splice`等を使用して個別に変更をすれば問題はない。

```typescript
itemList.splice(4, 1, { id: '0-0-0-0-0', content: '更新', children: [] })
```

しかし、React に対して状態の更新を通知するためにはオブジェクト自体を更新する必要があるため、全体の更新が必要となる。

```typescript
setItemList(itemList.map((item) => (item.id === '0-0-0-0-0' ? { ...item, content: '更新' } : item)))
```

これにより DOM が書き換えられるが、配列全体を書き換えたので全体に再描画が発生する。`React.memo`によって更新された要素以外の描画の「再計算」は抑止できるが、再描画自体は発生する。アウトライナーでは数百行以上の要素を何度も更新することもあるため、これでは操作するたびに再計算待ちのラグが発生する。

命令的な UI 操作であれば対象の要素を`queryselector`等で取得して削除や更新を行うため、再描画のコストは限定的となる。

この問題点を宣言的な仕組みのみで解決することは困難だったため、`handlerMap`という仕組みを導入することで解決した。

各行（アイテム）のコンポーネントマウント時に、描画に使用する State の Setter を全体からアクセスできる Map に[格納](https://github.com/k4a-dev/outliner/blob/24083e2ed9a7cb1a73a29aaa6838271c55c99b77/src/components/outliner/3_block/itemBlock.tsx#L154)する。
他のコンポーネントからの操作によって情報に更新があった場合は、handlerMap 経由で更新を[トリガー](https://github.com/k4a-dev/outliner/blob/24083e2ed9a7cb1a73a29aaa6838271c55c99b77/src/hooks/useItemUpdator.tsx#L125)することで、特定のコンポーネントのみを更新することができる。

この仕組みによって再描画を本当に更新があった要素のみに限定することができたため、かなりのパフォーマンス向上を実現できた。
React 本来の更新手順ではないため、何らかの問題が発生する可能性があるが、今のところ特に問題は発生していない。

## 課題

2 ペイン式以外は既存のアウトライナーに対して特に優位性のある機能は実現できなかった。

通常使用に問題ないレベルの操作感を実現できたのは良かったが、データの整合性の担保という点で信頼性に掛けるため自分でも普段使いのツールにすることはできなかった。

認証、データベースの知識や経験が浅く、何とか動作させることはできたが技術としては習得できていない。
たとえ深く学べたとしても認証やデータベースを専門とすることは難しいように感じた。

テストドリブンで開発をしてみようと思ったが、開発経験が乏しいため必要な機能の洗い出しを事前にするのが難しく、結局テストは中途半端な実装となった。また UI(React Component)部分のテストは全く実装することができなかった。

## その他

- [キーボード等のショートカット](https://twirliner.k4a.me/shortcuts)
