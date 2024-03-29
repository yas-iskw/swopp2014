# 組込みシステム向け メモリアクセス志向によるタスクの CPU コア割当て機構 #
## intro ##
* 組み込みでは応答性が重要
* ハードウェアのオーバーヘッドの存在が無視できない
> * アムダールの法則
> * バス調停、キャッシュ同期
* ユーザー空間は分離、カーネル空間は共有
* カーネル空間のキャッシュ同期をなんとかする
* 近年のアーキテクチャはキャッシュが共有
> * コア間の同期をしなくてよくなる
* 処理待ち合わせによる遅延が懸念される
* 頻繁なコンテキストスイッチによるオーバーヘッド
* カーネルのロードバランス処理


# Xeon Phi搭載計算機における DMA・MMIO 併用型 CPU 間データ通信機構 #
## intro ##
* マルチコア、メニーコア混在型計算機
* それぞれのメモリは互いにアクセス可能
* 提案はPVASとかいうの
* 同一仮想アドレス空間で複数のプロセスを実行するタスクモデル
> * 元々は単一のメニーコアCPUへ適用するモデル
> * multiple~はメニーコアと空間を共有するプログラム実行基盤
> * データ転送方式はMMIOとかDMA
* MMIO
> * リモートメモリ直接アクセス
> * CPU命令レベルで等価的に低遅延でアクセス可能
> * 大容量データ転送には不向き
* DMA
> * リモートメモリへデータ転送
* 64byte程度までだったら直接リモートメモリアクセスがいい
* 課題としてはメモリ空間をコヒーレントに見せつつ、大容量データ転送も効率よくおこなうこと
> * hostからxion phiへのwriteは高帯域通信可能らしい
> * readを考えなければならない
> * 転送量によって方式を動的?に変えるような基盤

## proposal ##
* 高速read方式→OSにヒント？を送ることで対象領域を高速にread可能
* 高速read方式の検討事項
> * 対象領域の扱い
> * データ転送方式関連
* 対象領域の扱い
> * アプリケーションプログラムがOSへ明示的に通知
> * 
* データ転送方式
> * MMIO方式を併用してDMA転送のアクセス遅延時間を隠蔽
> > * DMA転送中はMMIO
> > * 完了後はなんだっけ


## evaluation ##
* DMA転送の併用により、色々オーバーヘッドを考慮してもMMIOが早くなった



# 新しいタスクモデルによるMPIノード内通信の高性能化 #
## intro ##
* メニーコア環境では1論理コアあたりのメモリ量はかなり限られている
> * 節約しないと
* 共有メモリ方式
> * MPIプロセス間で共有メモリを作成
> * 2度でま
* カーネル支援方式
> * OSカーネルにメッセージの送受信を任せる
> * オーバーヘッドある
* メモリマッピング方式
> * 1度のメモリコピーだけでよい
> * システムコールは最初だけでよい(カーネル方式と違う)
> * カーネル空間で多くのメモリを消費してしまう


## evaluation ##
* NAS parallel benchmarks使ってる
* PVAS使った奴と普通のやつを比較



# 光サーキットの補助による低遅延性及びトポロジ内包性・分割生を持つネットワーク
## intro ##
* トポロジーによってスパコンに投げられたアプリケーションのパフォーマンスが低下してしまうかも
* 電気スイッチ間への光サーキットの挿入を提案している
* torus,hypoercubeなど
> * 局所性ある
> * ノード間遅延性性能?悪い
* 一様なランダムトポロジ
> * 全体での遅延性能良い
> * 局所性無し(トポロジ分割しにくい)
* 解決策としてランダムトポロジの局所化を推している

## proposal ##
* 局所化ランダム
> * torusの各次元ランダム入れ替え
> * 1次元目でランダム、2次元目でランダム…といった感じ


## evaluation ##



## proposal(本物) ##
* 要求された論理トポロジに対して物理トポロジの中で同じ部分があったらそれを提供する
* 欠けがあるかもなのでそれを補うのが光サーキットスイッチ?
* 並列実行する前に静的に(たぶん)光サーキットスイッチを用いて再構成を行う
* 局所化ランダムに光サーキットスイッチを足す
* どこに入れるか、みたいなのはGAを使って探すみたい

## evaluation(本物) ##



# 40Gbpsを目指した高効率TCPプロトコル
## intro ##
* TCPが10Gとかになるとしんどい
* 今では10GもTCPでもだいじょぶ
> * 次は40
* 輻輳制御も色々
> * 遅延ベース
> * 損失ベース
* TCP Reno
> * ウィンドウサイズを線形増加
* scalable TCP
> * 躯体息ネットワークで性能を引き出す
> * ウィンドウサイズを指数関数的に増加させる
> * 輻輳が結構起きてしまう
* TCP Hybla
* CUBIC TCP
> * 現在のlinuxの標準
* これらは3種類に分けられる


# 規則・不規則切り替え可能な再構成可能3DNoC #
## intro ##
* NoCは現状ではfat-treeとか2次元トポロジとかが中心
* スモールワールド・ネットワーク(ランダムなショートカットを入れたネットワーク)が色々使われている
* ロングレンジリンク(2次元メッシュで遠いリンクを繋ぐ)
> * 面積増加してしまう
> * トラフィックに対して入れるのでgeneralな解ではない
* 3次元のNoCにスモールワールド・ネットワーク入れたやつがある
> * 現状では完全に固定になってる
* 今フィギュラブルスイッチを使ってる
* n=2では対トーラス比で平均グラフ長を最大24.5%


# tenderにおけるプロセス間通信データ域に特化したプロセス間通信の設計 #
## intro ##
* tenderとかいうOS(おりじなる？)での話


# 多重 OS 構成によるカーネルのライブアップデート手法の 提案 #
## intro ##
* seamless kernel updateが関連研究
* 現在のやりかたではもうコレ以上早くならないので、