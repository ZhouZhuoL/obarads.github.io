# A Review of Robot Learning for Manipulation: Challenges, Representations, and Algorithms

元の論文の公開ページ : [arxiv.org](https://arxiv.org/abs/1907.03146)  
提案モデルの実装 : [なし]()  
Github Issues : []()  

Note: 記事の見方や注意点については、[こちら](/)をご覧ください。
Note: 本記事は全翻訳に近いです。

## どんなもの?
##### ロボットマニピュレーションのために機械学習を使う研究について調査した。
- このサーベイでは、ロボットマニュピレーションの学習問題を形式化し、残りの研究課題を明確にする。

## 先行研究と比べてどこがすごいの? or 関連事項
##### 省略

## 技術や手法のキモはどこ? or 提案手法の詳細
- Note: 全翻訳ではありません。

### 1. 導入
##### ロボットマニピュレーションはロボット工学において中心的な存在である。
- ロボットマニピュレーションはロボット工学の可能性を得るための中心的存在である。
  - ここのロボットの定義はアクチュエーターを持ち、それを使用してworldに変化をもたらすことができることである。
  - [worldは現実世界とか現実の環境とかそういうニュアンス?]
- 環境を操作できる[(環境に介入できる)]ロボットは、病院、高齢者もしくは幼児の見守り、工場、宇宙、レストランなどのサービス産業および家庭に配備でき、とんでもないポテンシャルを持つ。
- 多種多様な状況や、食品の準備のような特定の状況にもある広範かつ非体系的な環境の変動は、ロボットが経験したことのない環境に対処できる必要があることを示している。
- 研究者はどうやってそのような環境でロボットが操作の仕方を学習していくかという問(研究)に注目している。
- この研究は以下のような多くの目的に及ぶ。
  - 人のデモンストレーションから個々の操作を学習する、
  - 高度なプランニングに対して適切な操作タスクの抽象的説明の学習、
  - オブジェクトとの相互作用によるオブジェクトの機能の発見。
- その一例として、著者らの研究を図2に示す。

![fig2](img/ARoRLfMCRaA/fig2.png)

##### 本論文の目的は2つある。
- この論文の目的は以下の２つである。
  - 既存の研究を単体の一貫したフレームワークへ統合し、ロボットマニピュレーション学習問題の形式化を行う。
  - ママニピュレーションのためのロボットの学習を行う研究の一部を説明する。
- こうすることで、これらの手法が適応されているマニピュレーション学習の問題の多様性を示すと同時に、残っている研究課題を特定できる。

##### 本論文は1章~8章で構成される。
- 本論文の構成は以下の通りになる。
  - 二章では、マニピュレーションの学習を行う上でのキーコンセプトについてサーベイし、それらの基本的な構造について提供する。
  - 三章では、殆どのマニピュレーション問題を把握し、マニピュレーション問題の形式化を行う。ここでは問題に不可欠な構造が含まれている[?]。レビューの残りでは、いくつかの技術的課題を取り扱う。
  - 四章では、状態空間を定義するため、学習の問題を考える。ここでは、ロボットは環境内の各オブジェクトの自由度と関連する状態特徴を発見しなければならない。
  - 五章では、ロボットの行動がタスクの状態にどのように影響するかを表す環境推移モデルを学習するためのアプローチを説明する。
  - 六章では、いくつかの目標を達成するためのモーター制御ポリシーをどうやって学習できるかに着目する。
  - 七章では、モータースキル[?]を特徴づけるためのアプローチについて説明する。
  - 最終章では、効果的なハイレベルの学習、プランニング、転移[?]を可能にするための手順と状態の抽象化を学習する方法についてサーベイする。

### 2. マニピュレーションのための学習に関する一般的な概念
- マニピュレーション学習問題を形式化するに当たって、はじめに内部の骨組みについて説明する。

#### 2.1 物理系としてのマニピュレーション
##### 物理的な概念はマニュピレーション学習に多大な影響を与える。
- マニピュレーションは物理法則による影響を受け、それはマニピュレーション学習アルゴリズムに対して多大な影響を与える。
- 基本的な物理概念(重力など)は、マニピュレーションタスクにかなりの事前知識を与える。
- これらの概念は学習アルゴリズムにとって非常に重要であり、マニュピレーションの技能をより御しやすいものにしてくれる。

#### 2.2 マニピュレーションにおけるUnderactuation, Nonholonomic ConstraintsとModes
##### Underactuation[??]
- マニピュレーションタスクはほとんどの場合、劣駆動系として特徴づけられる。
  - 各々のロボットは、低レベルの行動領域を定義したアクチュエータのセットを持つ。
  - 例えば、8つの制御変数に、fully actuated 7-DoF armと1 DoF gripperをもたせる。
  - 不活性オブジェクト(Inanimate objects)は行動領域を拡大させない。ただし、環境内の全てのオブジェクトは独立した状態変数のセットを状態空間に提供するのである。
  - Hence, having even one free object in the environment will result in the state having more degrees of freedom than there are actu-ators, which means that the system is underactuated. [???]
  - アクチュエータとDoFの数の不一致はinanimate objectsが環境へ追加されるごとに増える。

- しかしながら、異なるオブジェクトの状態はマニピュレーションを介したロボットによって変わる。
- ロボットは最初にオブジェクトの状態を変更できる状態にしておき、期待されるマニピュレーションを適応する。
  - 例えば、オブジェクトと接触してからオブジェクトを押すというようなマニピュレーション。
- 

## どうやって有効だと検証した?
##### 省略

## 議論はある?
##### 省略

## 次に読むべき論文は?
##### なし

## 論文関連リンク
##### なし
1. [なし]()[1]

## 会議, 論文誌, etc.
##### なし

## 著者
##### Oliver Kroemer, Scott Niekum, George Konidaris

## 投稿日付(yyyy/MM/dd)
##### 2019/07/06

## コメント
##### あり
- Underactuationの項が不完全

## key-words
##### Survey, Robot, 導入

## status
##### 導入

## read
##### なし

## Citation
##### 未記入
