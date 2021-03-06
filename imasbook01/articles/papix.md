# スコアを用いた<br>ライブの予習セットリスト自動生成の試み<br><span class="subtitle">〜Investigation of Automatically Generation of Set List for Preparation〜</span>
<p class="right">著:遠山 賢寿</p>

## はじめに

｢アイドルマスター シンデレラガールズ｣は, バンダイナムコエンターテインメントとCygamesが開発/運営するソーシャルゲームである.
2011年にリリースされて以降, 2014年には1stライブ｢THE IDOLM@STER CINDERELLA GIRLS 1stLIVE WONDERFUL M@GIC!!｣が開催され, また2015年にはアニメ化及びスマートフォン向けの音楽ゲーム｢アイドルマスター シンデレラガールズ スターライトステージ｣がリリース, そして2016年にはVR(仮想現実)に対応した｢アイドルマスター シンデレラガールズ ビューイングレボリューション｣が発売されるなどの歴史を経て, 2018年現在も多くのファンに愛されているコンテンツの1つである.

本作の特徴として, 登場するキャラクターの多さが挙げられる. 本作初出となるアイドルは総勢183名を数え, そのうちの半数近いキャラクターに担当声優が割り当てられている. 毎年開催されるライブ(いわゆる｢周年ライブ｣)などのライブイベントにおいては, キャラクターの声を担当する声優が出演し, ライブパフォーマンスを行う. 近年の周年ライブイベントにおいては, 1公演あたり15人〜20人ほどの声優が出演し, 公演時間にもよるが20曲前後の曲を披露することが多い.

他方, 本作によって提供された楽曲について見ると, 2018年6月末時点で190曲が公開されているという調査結果がある[1]. ライブイベントの開催にあたって, 披露される可能性が高い楽曲を集めた楽曲集, いわゆる｢予習セットリスト｣を構築するにあたっては, 各公演について15名〜20名前後の出演者と既存の190曲を軸にして, ライブのテーマや過去の傾向などを考慮しつつ組み立てる必要がある. 予習セットリストを効率よく構築することができれば, より多くの時間をライブイベントの準備に充てることができる.

<footer>\*1 : 天蛾 ｢アイドルマスターのオリジナル曲数を数えてみた・2018年6月｣ 天さんは今日もブルー, 2018年
http://blog.bellmega.blue/post/2000</footer>

そこで本稿では, 予習セットリストの効率的な構築を狙い, その第一歩として単純なスコアの算出による予習セットリストの自動生成を試みた.
実装したプログラムにより, ｢THE IDOLM@STER CINDERELLA GIRLS 6thLIVE MERRY-GO-ROUNDOME!!!｣におけるメットライフドーム公演の出演者と57曲の楽曲を元に予習セットリストの自動生成を行い, 11月10日のDAY1公演は84.6%, 11月11日のDAY2公演は70.5%の楽曲について, 実際に公演にて披露されたことを確認した.

## 提案手法

本稿で実装する予習セットリスト自動生成プログラムは, 3つのデータを元にして予習セットリストを構築する. 各公演における出演者データ, 楽曲データ, そして設定データである. まずはこれらのデータについて解説を行う.

### 出演者データ

出演者データについては, ｢THE IDOLM@STER CINDERELLA GIRLS 6thLIVE MERRY-GO-ROUNDOME!!!｣におけるメットライフドームのDAY1公演に出演した27名(一ノ瀬志希, 三村かな子, 佐藤心, 前川みく, 向井拓海, 城ヶ崎美嘉, 城ヶ崎莉嘉, 塩見周子, 多田李衣菜, 大和亜季, 大槻唯, 姫川友紀, 安部菜々, 宮本フレデリカ, 小早川紗枝, 島村卯月, 川島瑞樹, 木村夏樹, 本田未央, 松永涼, 棟方愛海, 渋谷凛, 白坂小梅, 相葉夕美, 藤本里奈, 速水奏, 難波笑美)及びDAY2公演に出演した27名(アナスタシア, 一ノ瀬志希, 二宮飛鳥, 佐々木千枝, 佐藤心, 双葉杏, 向井拓海, 城ヶ崎美嘉, 城ヶ崎莉嘉, 塩見周子, 安部菜々, 宮本フレデリカ, 小早川紗枝, 島村卯月, 新田美波, 早坂美玲, 木村夏樹, 本田未央, 橘ありす, 渋谷凛, 白坂小梅, 結城晴, 藤本里奈, 赤城みりあ, 速水奏, 道明寺歌鈴, 龍崎薫)を対象とした[2].

<footer>\*2 : バンダイナムコエンターテインメント ｢開催概要│THE IDOLM@STER CINDERELLA GIRLS 6thLIVE｣ THE IDOLM@STER OFFICIAL WEB, 2018年
https://idolmaster.jp/event/cinderella6th/information.php</footer>

### 楽曲データ

楽曲データについては, 今回は検証のため, 以下の57曲の楽曲を対象とした.

- ｢THE IDOLM@STER CINDERELLA GIRLS STARLIGHT MASTER｣シリーズ
    - ｢Snow Wings｣, ｢Tulip｣, ｢ハイファイ☆デイズ｣, ｢生存本能ヴァルキュリア｣, ｢純情Midnight伝説｣, ｢Love∞Destiny｣, ｢サマカニ!!｣, ｢BEYOND THE STARLIGHT｣, ｢ラブレター｣, ｢Jet to the Future｣, ｢あんきら!?狂想曲｣, ｢命燃やして恋せよ乙女｣, ｢Sweet Witches' Night 〜6人目はだぁれ〜｣, ｢情熱ファンファンファーレ｣, ｢桜の頃｣, ｢∀NSWER｣, ｢Nothing but You｣, ｢モーレツ★世直しギルティ!｣, ｢With Love｣, ｢リトルリドル｣, ｢Kawaii make MY day!｣, ｢双翼の独奏歌｣, ｢Twin☆くるっ★テール｣, ｢Trinity Field｣, ｢Happy New Yeah!｣, ｢美に入り彩を穿つ｣, ｢凸凹スピードスター｣, ｢クレイジークレイジー｣, ｢ガールズ・イン・ザ・フロンティア｣, ｢Pretty Liar｣
- ｢THE IDOLM@STER CINDERELLA GIRLS MASTER SEASONS!｣シリーズ
    - SUMMER!: ｢銀のイルカと熱い風｣, ｢とんでいっちゃいたいの｣, ｢夏恋 -NATSU KOI-｣, ｢CoCo夏夏夏 Holiday｣
    - AUTUMN!: ｢秋風に手を振って｣, ｢Halloween♥Code｣, ｢さよならアンドロメダ｣, ｢空想探査計画｣
    - WINTER!: ｢ツインテールの風｣, ｢White again｣, ｢Frost｣, ｢冬空プレシャス｣
    - SPRING!: ｢桜の風｣, ｢HARURUNRUN｣, ｢未完成の歴史｣, ｢Spring Screaming｣
- ｢シンデレラガールズ劇場｣エンディングテーマ
    - 第1期: ｢キラッ! 満開スマイル｣, ｢エチュードは1曲だけ｣, ｢SUN♡FLOWER｣
    - 第2期: ｢Blooming Days｣, ｢秋めいて Ding Dong Dang!｣, ｢Snow\*Love｣
    - 第3期: ｢いとしーさー♥｣, ｢なつっこ音頭｣, ｢さよならアロハ｣
- ライブ記念楽曲
    - ｢Starry-Go-Round｣

これらの楽曲は, 次のようなデータ構造で表される. なお, 本稿においては以下各種データ構造をJSONで表現するものとする.

```json
{
  "title": "楽曲のタイトル",
  "performed_by": ["歌唱者A", "歌唱者B", "歌唱者C", ...],
  "meta": {
    "is_winter": true
  }
}
```

`title`は楽曲のタイトルであり, `performed_by`はその楽曲のオリジナルメンバー(歌ったアイドル)を表す. また, `meta`については楽曲が持つ属性を格納する. 例えば上記の例では, `is_winter`というメタデータが`true`になっており, これは冬をテーマにした楽曲であることを示す.

メタデータについては, 今回は`is_spring`, `is_summer`, `is_autumn`, `is_winter`を用意した. これは, 昨今の｢THE IDOLM@STER CINDERELLA GIRLS MASTER SEASONS!｣シリーズ及び｢シンデレラガールズ劇場｣エンディングテーマにて四季をテーマにした楽曲が多かったこと, ｢THE IDOLM@STER CINDERELLA GIRLS 6thLIVE MERRY-GO-ROUNDOME!!!｣が合計4公演開催されることが決まっており, 各公演で1つの季節をテーマにする可能性があったことが理由である.

楽曲データの具体的な例として, ｢Trinity Field｣と｢未完成の歴史｣のデータを示す:

```json
{
  "title": "Trinity Field",
  "performed_by": ["北条加蓮", "渋谷凛", "神谷奈緒"]
}
```

```json
{
  "title": "未完成の歴史",
  "performed_by": ["二宮飛鳥", "藤原肇", "北条加蓮"],
  "meta": {
    "is_spring": true
  }
}
```

### 設定データ

設定データは, 出演者データ及び楽曲データから, スコアを求める際に利用するパラメータである. 今回は, 以下のパラメータを指定できることとした:

- `score_limit`
    - 結果として表示するスコアの下限値
- `complete_bonus`
    - 楽曲のオリジナルメンバー全員が公演に出演する際に適用されるスコア倍率
- `is_spring` / `is_summer` / `is_autumn` / `is_winter`
    - 公演ごとに設定されたテーマ(季節)と一致した時に適用されるスコア倍率

例えば, 次のような設定データが用意されていると仮定する:

```json
{
  "score_limit": 0.5,
  "complete_bonus": 2,
  "is_spring": 1.5,
  "is_summer": 0,
  "is_autumn": 0,
  "is_winter": 0
}
```

この場合, 楽曲のオリジナルメンバー全員が公演に出演する場合スコアが2倍, 楽曲が春をテーマにしたものであればスコアが1.5倍となる. また, `score_limit`は0.5なので, スコアが0.5以下のものは結果として出力されない. `is_summer`, `is_autumn`, `is_winter`の場合はスコアが0倍となるので, スコアは必ず0.5以下となり, 最終的な予習セットリストの結果において表示されることはない.

### スコアの算出と予習セットリストの表示

これらの出演者データ, 楽曲データ, そして設定データを元にして, 予習セットリストの候補となる楽曲のスコアを計算する. 各楽曲のスコアの初期値は1として, 以下の方法に従ってスコアを計算し, 予習セットリストを生成する.
なお, ここでは例として, 楽曲｢さよならアンドロメダ｣及び｢Tulip｣について, ｢THE IDOLM@STER CINDERELLA GIRLS 6thLIVE MERRY-GO-ROUNDOME!!!｣におけるメットライフドームDAY1公演の出演者によってスコアの計算例を示す. ｢さよならアンドロメダ｣及び｢Tulip｣の楽曲データは以下の通りである.

```json
{
  "title": "さよならアンドロメダ",
  "performed_by": ["渋谷凛", "森久保乃々", "大和亜季"],
  "meta": {
    "is_autumn": true
  }
}
```

```json
{
  "title": "Tulip",
  "performed_by": ["速水奏", "塩見周子", "城ヶ崎美嘉", "宮本フレデリカ", "一ノ瀬志希"]
}
```

また, 設定データについては以下の値を用いる.

```json
{
  "score_limit": 1,
  "complete_bonus": 2,
  "is_spring": 1.2,
  "is_summer": 1.2,
  "is_autumn": 1.2,
  "is_winter": 1.2
}
```

#### 出演率によるスコア計算

まず最初に, ある楽曲Aについてスコアを計算するとき, 対象とする公演Bにオリジナルメンバーが多く出演していれば多く出演しているほど(出演率が高いほど), スコアが高くなるようにスコアを計算する. 具体的には,

- 楽曲Aのオリジナルメンバーが公演Bに全員出演するとき, スコアに設定データの`complete_bonus`を乗算する
- そうでないとき, スコアに`公演Bに出演するオリジナルメンバーの数 / オリジナルメンバーの総数`を乗算する

例えば, ｢Tulip｣については, DAY1公演にオリジナルメンバーが全員出演する. 従って, 楽曲のスコア`1`に, 設定データの`complete_bonus`, すなわち`2`を乗算したものがスコアとなる.

一方, ｢さよならアンドロメダ｣であれば, オリジナルメンバーのうち, DAY1公演に渋谷凛及び大和亜季の2名が出演する. 従って, 楽曲のスコア(初期スコア)`1`に, `2 / 3`を乗算したものがスコアとなる.

#### メタデータによるスコア計算

次に, 楽曲ごとに付与されたメタデータによってスコアを計算する.
ある楽曲Aにメタデータが設定されている場合, 設定データで指定された値を用いてスコアを乗算する.

例えば｢Tulip｣はメタデータが設定されていないので, メタデータによるスコア計算は行わない. 一方｢さよならアンドロメダ｣は, `is_autumn`というメタデータが設定されているので, スコアに設定データの`is_autumn`すなわち`1.2`を乗算する.

以上をまとめると, ｢Tulip｣及び｢さよならアンドロメダ｣のスコアは次のような結果となる:

曲名 | 初期スコア | 出演率 | メタデータ | 最終スコア |
-- | -- | -- | -- | --
Tulip                | 1 | * 2 (`complete_bonus`) | -                   | 2.000
さよならアンドロメダ | 1 | * 2 / 3                | * 1.2 (`is_autumn`) | 0.800

### 予習セットリストの表示

対象となる全ての楽曲についてスコアの計算を行った後, 予習セットリストの表示を行う.
結果は, 設定データの`score_limit`より大きい値のスコアを持つ楽曲が予習セットリストに含まれる.
従って, ｢Tulip｣は最終的なスコアが`2.000`のため予習セットリストに含まれるが, ｢さよならアンドロメダ｣は`0.800`のため, 予習セットリストには含まれない.

## 結果

### DAY1公演 (メタデータ考慮なし)

DAY1公演について, 以下の設定データを元にスコアの計算を行った.

```json
{
  "score_limit": 0.5,
  "complete_bonus": 2,
  "is_spring": 1,
  "is_summer": 1,
  "is_autumn": 1,
  "is_winter": 1
}
```

結果は次の通り:

曲名 | score | 結果 | オリジナルメンバー | 出演者
-- | -- | -- | -- | --
Happy New Yeah! | 2.000 | ○ | 島村卯月, 渋谷凛, 本田未央, 佐藤心, 三村かな子 | 島村卯月, 渋谷凛, 本田未央, 佐藤心, 三村かな子
Jet to the Future | 2.000 | ○ | 多田李衣菜, 木村夏樹 | 多田李衣菜, 木村夏樹
Tulip | 2.000 | ○ | 速水奏, 塩見周子, 城ヶ崎美嘉, 宮本フレデリカ, 一ノ瀬志希 | 速水奏, 塩見周子, 城ヶ崎美嘉, 宮本フレデリカ, 一ノ瀬志希
Twin☆くるっ★テール | 2.000 | ○ | 城ヶ崎美嘉, 城ヶ崎莉嘉 | 城ヶ崎美嘉, 城ヶ崎莉嘉
とんでいっちゃいたいの | 2.000 | ✕  | 一ノ瀬志希, 三村かな子, 宮本フレデリカ | 一ノ瀬志希, 三村かな子, 宮本フレデリカ
クレイジークレイジー | 2.000 | ○ | 宮本フレデリカ, 一ノ瀬志希 | 宮本フレデリカ, 一ノ瀬志希
凸凹スピードスター | 2.000 | ○ | 佐藤心, 安部菜々 | 佐藤心, 安部菜々
純情Midnight伝説 | 2.000 | ○ | 向井拓海, 藤本里奈, 松永涼, 大和亜季, 木村夏樹 | 向井拓海, 藤本里奈, 松永涼, 大和亜季, 木村夏樹
美に入り彩を穿つ | 2.000 | ○ | 小早川紗枝, 塩見周子 | 小早川紗枝, 塩見周子
SnowWings | 0.800 | ✕ | 島村卯月, 渋谷凛, 本田未央, 大槻唯, 上条春菜 | 島村卯月, 渋谷凛, 本田未央, 大槻唯
Halloween♥Code | 0.667 | ✕ | 安部菜々, 乙倉悠貴, 大和亜季 | 安部菜々, 大和亜季
White again | 0.667 | ✕ | 小早川紗枝, 櫻井桃華, 島村卯月 | 小早川紗枝, 島村卯月
さよならアンドロメダ | 0.667 | ✕ | 渋谷凛, 森久保乃々, 大和亜季 | 渋谷凛, 大和亜季
ツインテールの風 | 0.667 | ✕ | 城ヶ崎美嘉, 小日向美穂, 速水奏 | 城ヶ崎美嘉, 速水奏
冬空プレシャス | 0.667 | ✕ | 片桐早苗, 難波笑美, 姫川友紀 | 難波笑美, 姫川友紀
夏恋 -NATSU KOI- | 0.667 | ✕ | 塩見周子, 橘ありす, 松永涼 | 塩見周子, 松永涼
秋風に手を振って | 0.667 | ✕ | 相葉夕美, 多田李衣菜, 中野有香 | 相葉夕美, 多田李衣菜
BEYOND THE STARLIGHT | 0.600 | ✕ | 城ヶ崎莉嘉, 緒方智絵里, 北条加蓮, 川島瑞樹, 大槻唯 | 城ヶ崎莉嘉, 川島瑞樹, 大槻唯
SUN♡FLOWER | 0.600 | ✕ | 本田未央, 城ヶ崎美嘉, 片桐早苗, 諸星きらり, 佐藤心 | 本田未央, 城ヶ崎美嘉, 佐藤心
Starry-Go-Round | 0.600 | ○ | 前川みく, 大槻唯, アナスタシア, 姫川友紀, 二宮飛鳥 | 前川みく, 大槻唯, 姫川友紀
さよならアロハ | 0.600 | ✕ | 木村夏樹, 十時愛梨, 中野有香, 藤本里奈, 宮本フレデリカ | 木村夏樹, 藤本里奈, 宮本フレデリカ
ガールズ・イン・ザ・フロンティア | 0.600 | ✕ | 渋谷凛, 早坂美玲, 木村夏樹, 小日向美穂, 塩見周子 | 渋谷凛, 木村夏樹, 塩見周子

自動的に構築した予習セットリストの20曲中, 9曲が実際に公演中に披露された. 適合率は45.0%であった.

### DAY1公演 (メタデータ考慮あり)

DAY1公演において, 本公演においては四季がテーマとなっており, DAY1公演は"春"をテーマとしていたことがわかった. これを元に, 設定データを以下のようにしてスコア計算を行った.

```json
{
  "score_limit": 0.5,
  "complete_bonus": 2,
  "is_spring": 1.75,
  "is_summer": 0,
  "is_autumn": 0,
  "is_winter": 0
}
```

曲名 | score | 結果 | オリジナルメンバー | 出演者
-- | -- | -- | -- | --
Happy New Yeah! | 2.000 | ○ | 島村卯月, 渋谷凛, 本田未央, 佐藤心, 三村かな子 | 島村卯月, 渋谷凛, 本田未央, 佐藤心, 三村かな子
Jet to the Future | 2.000 | ○ | 多田李衣菜, 木村夏樹 | 多田李衣菜, 木村夏樹
Tulip | 2.000 | ○ | 速水奏, 塩見周子, 城ヶ崎美嘉, 宮本フレデリカ, 一ノ瀬志希 | 速水奏, 塩見周子, 城ヶ崎美嘉, 宮本フレデリカ, 一ノ瀬志希
Twin☆くるっ★テール | 2.000 | ○ | 城ヶ崎美嘉, 城ヶ崎莉嘉 | 城ヶ崎美嘉, 城ヶ崎莉嘉
クレイジークレイジー | 2.000 | ○ | 宮本フレデリカ, 一ノ瀬志希 | 宮本フレデリカ, 一ノ瀬志希
凸凹スピードスター | 2.000 | ○ | 佐藤心, 安部菜々 | 佐藤心, 安部菜々
純情Midnight伝説 | 2.000 | ○ | 向井拓海, 藤本里奈, 松永涼, 大和亜季, 木村夏樹 | 向井拓海, 藤本里奈, 松永涼, 大和亜季, 木村夏樹
美に入り彩を穿つ | 2.000 | ○ | 小早川紗枝, 塩見周子 | 小早川紗枝, 塩見周子
BEYOND THE STARLIGHT | 0.600 | ✕ | 城ヶ崎莉嘉, 緒方智絵里, 北条加蓮, 川島瑞樹, 大槻唯 | 城ヶ崎莉嘉, 川島瑞樹, 大槻唯
Starry-Go-Round | 0.600 | ○ | 前川みく, 大槻唯, アナスタシア, 姫川友紀, 二宮飛鳥 | 前川みく, 大槻唯, 姫川友紀
ガールズ・イン・ザ・フロンティア | 0.600 | ✕ | 渋谷凛, 早坂美玲, 木村夏樹, 小日向美穂, 塩見周子 | 渋谷凛, 木村夏樹, 塩見周子
HARURUNRUN | 0.583 | ○ | 関裕美, 水本ゆかり, 棟方愛海 | 棟方愛海
Spring Screaming | 0.583 | ✕ | 喜多見柚, 本田未央, 龍崎薫 | 本田未央

自動的に構築した予習セットリストの13曲中, 11曲が実際に公演中に披露された. 適合率は84.6%であった.

### DAY2公演 (メタデータ考慮あり)

DAY1公演の結果より, DAY2公演は"夏"をテーマになることが予想された(想定される四季の順としては, "春夏秋冬"ないしは｢THE IDOLM@STER CINDERELLA GIRLS MASTER SEASONS!｣シリーズのリリース順, すなわち"夏秋冬春"のいずれかの可能性が高い). これを元に, 設定データを以下のようにしてスコア計算を行った.

```json
{
  "score_limit": 0.5,
  "complete_bonus": 2,
  "is_spring": 0,
  "is_summer": 1.75,
  "is_autumn": 0,
  "is_winter": 0
}
```

曲名 | score | 結果 | オリジナルメンバー | 出演者
-- | -- | -- | -- | --
なつっこ音頭 | 3.500 | ○ | 赤城みりあ, 城ヶ崎莉嘉, 橘ありす, 結城晴, 龍崎薫 | 赤城みりあ, 城ヶ崎莉嘉, 橘ありす, 結城晴, 龍崎薫
Tulip | 2.000 | ○ | 速水奏, 塩見周子, 城ヶ崎美嘉, 宮本フレデリカ, 一ノ瀬志希 | 速水奏, 塩見周子, 城ヶ崎美嘉, 宮本フレデリカ, 一ノ瀬志希
Twin☆くるっ★テール | 2.000 | ○ | 城ヶ崎美嘉, 城ヶ崎莉嘉 | 城ヶ崎美嘉, 城ヶ崎莉嘉
クレイジークレイジー | 2.000 | ○ | 宮本フレデリカ, 一ノ瀬志希 | 宮本フレデリカ, 一ノ瀬志希
リトルリドル | 2.000 | ○ | 双葉杏, 城ヶ崎莉嘉, 二宮飛鳥, 白坂小梅, 早坂美玲 | 双葉杏, 城ヶ崎莉嘉, 二宮飛鳥, 白坂小梅, 早坂美玲
凸凹スピードスター | 2.000 | ○ | 佐藤心, 安部菜々 | 佐藤心, 安部菜々
美に入り彩を穿つ | 2.000 | ○ | 小早川紗枝, 塩見周子 | 小早川紗枝, 塩見周子
とんでいっちゃいたいの | 1.167 | ✕ | 一ノ瀬志希, 三村かな子, 宮本フレデリカ | 一ノ瀬志希, 宮本フレデリカ
夏恋 -NATSU KOI- | 1.167 | ✕ | 塩見周子, 橘ありす, 松永涼 | 塩見周子, 橘ありす
SUN♡FLOWER | 1.050 | ✕ | 本田未央, 城ヶ崎美嘉, 片桐早苗, 諸星きらり, 佐藤心 | 本田未央, 城ヶ崎美嘉, 佐藤心
さよならアロハ | 1.050 | ○ | 木村夏樹, 十時愛梨, 中野有香, 藤本里奈, 宮本フレデリカ | 木村夏樹, 藤本里奈, 宮本フレデリカ
Happy New Yeah! | 0.800 | ✕ | 島村卯月, 渋谷凛, 本田未央, 佐藤心, 三村かな子 | 島村卯月, 渋谷凛, 本田未央, 佐藤心
ガールズ・イン・ザ・フロンティア | 0.800 | ○ | 渋谷凛, 早坂美玲, 木村夏樹, 小日向美穂, 塩見周子 | 渋谷凛, 早坂美玲, 木村夏樹, 塩見周子
ハイファイ☆デイズ | 0.600 | ○ | 佐々木千枝, 櫻井桃華, 市原仁奈, 龍崎薫, 赤城みりあ | 佐々木千枝, 龍崎薫, 赤城みりあ
純情Midnight伝説 | 0.600 | ✕ | 向井拓海, 藤本里奈, 松永涼, 大和亜季, 木村夏樹 | 向井拓海, 藤本里奈, 木村夏樹
CoCo夏夏夏 Holiday | 0.583 | ○ | 上田鈴帆, 佐藤心, 十時愛梨 | 佐藤心
銀のイルカと熱い風 | 0.583 | ○ | 大槻唯, 緒方智絵里, 新田美波 | 新田美波

自動的に構築した予習セットリスト17曲中, 12曲が実際に公演中に披露された. 適合率は70.5%であった.

## 今後の展望

今回は, いわゆるグループ曲を中心に予習セットリストを構築している. 今後は, ソロ曲についても楽曲データの整備を行い予習セットリストを構築できるようにすることが求められる.
一方で, メタデータの拡充によって精度を向上することも考えられる. 考慮できるメタデータとしては, 初出年からの差(最近出た曲ほどより高スコアとなる), 過去のライブイベントにおける披露の有無(初披露の場合スコアが高くなる), 特定公演でオリジナルメンバーが揃う場合によりスコアを向上させる, などのアイデアがあるであろう.
また, これらのメタデータについて, 過去のライブイベントの出演者及びセットリストを元にして, 適切なメタデータの倍率を学習させるというアプローチも有効かもしれない.

## まとめ

｢THE IDOLM@STER CINDERELLA GIRLS 6thLIVE MERRY-GO-ROUNDOME!!!｣におけるメットライフドーム公演のDAY1公演及びDAY2公演について, 一部の楽曲を対象としてスコア算出に基づいた予習セットリストの自動構築を試みた.
結果として, 生成した予習セットリストに含まれた楽曲のうち, DAY1公演について84.6%, DAY2公演について70.5%の楽曲が, 実際に公演中に披露されたことを確認した.
なお, 本稿のために実装したプログラムは, プログラミング言語Perl[3]を利用して実装しており, 近日GitHub[4]にて公開することを検討している.

<footer>\*3 : https://www.perl.org/</footer>
<footer>\*4 : https://github.com/</footer>
