# Project Illuminated Fediverse
## 目標
* Twitterの置き換え
* timesへのパイプ
* できるだけ管理に手間をかけないような仕組み
* 可搬性のあるサーバー構成にできるような仕組み

### Twitterの置き換え
* 信用問題の爆発が回を重ねるごとに大きくなっている

### times
現状、以下のDiscordサーバーにアロケーションされたtimesを置き換えたい。より明示的な定義をするのであれば、当該インスタンスをSSOTとしたい。
* ▓▓▓▓鯖
* ▓▓▓▓っ？
* ▓▓▓▓はうす
* ▓▓開▓鯖

### できるだけ管理に手間をかけないような仕組み
* ルール整備も含めたソフト・ハード面での仕組み
* 自分以外の招待を原則許可しない
  * 他人ができることを制限もしくは許可するというのは、それだけで管理コストを負うことになるので避けたい
  * 他人の招待を許可すると[電気通信事業法](https://elaws.e-gov.go.jp/document?lawid=359AC0000000086)の適用対象となり得る。これは管理コストを増大させるので避けたい。
    * これに限らず、法律の議論については別の項で取り扱う。

### 可搬性のあるサーバー構成にできるような仕組み
* docker-composeなどで仮想化されたイメージが予め用意されているのが好ましい
  * ze▓▓▓▓-c▓▓▓dを見ればそのメリットは一目瞭然。
    * @R▓▓▓▓Kのように実装ソフトウェアがext4のデスクリプタを食べ尽くすということが起きたとしても、影響がコンテナ内に抑えられる (と思われる)。

### 日本法における議論
現行のサーバーはJapan ▓▓▓▓▓▓▓ (▓▓▓▓▓) にあるという▓▓▓▓▓▓の記述を信じるのであれば、準拠するべきは日本法である。

#### 電気通信事業法
[条文](https://elaws.e-gov.go.jp/document?lawid=359AC0000000086)

以下、この節において電気通信事業法を単に「法」と略する。

まず、法第九条及び第十条第一項では、「電気通信事業」を行う場合、総務大臣に届け出が必要であることを述べている：

> （電気通信事業の登録）
> 
> 第九条　電気通信事業を営もうとする者は、総務大臣の登録を受けなければならない。ただし、次に掲げる場合は、この限りでない。
> 
> 第十条　前条の登録を受けようとする者は、総務省令で定めるところにより、次の事項を記載した申請書を総務大臣に提出しなければならない。

「電気通信事業」は法第二条第四項において定義されている：

> （定義）
> 
> 第二条　この法律において、次の各号に掲げる用語の意義は、当該各号に定めるところによる。
> 
> 一　電気通信　有線、無線その他の電磁的方式により、符号、音響又は影像を送り、伝え、又は受けることをいう。
> 
> 二　電気通信設備　電気通信を行うための機械、器具、線路その他の電気的設備をいう。
> 
> 三　電気通信役務　電気通信設備を用いて他人の通信を媒介し、その他電気通信設備を他人の通信の用に供することをいう。
> 
> 四　電気通信事業　電気通信役務を他人の需要に応ずるために提供する事業（放送法（昭和二十五年法律第百三十二号）第百十八条第一項に規定する放送局設備供給役務に係る事業を除く。）をいう。
> 
> 五　電気通信事業者　電気通信事業を営むことについて、第九条の登録を受けた者及び第十六条第一項の規定による届出をした者をいう。
> 
> 六　電気通信業務　電気通信事業者の行う電気通信役務の提供の業務をいう。

同条第一項から第四項によれば、
「有線、無線その他の電磁的方式により、符号、音響又は影像を送り、伝え、又は受ける」ための「機械、器具、線路その他の電気的設備」を用いて「他人の通信を媒介し、その他電気通信設備を他人の通信の用に供する」場合で、かつ「他人の需要に応ずるために提供する事業」である場合に「電気通信事業」とされると読める。

したがって、シングルユーザーインスタンスの場合は「他人の需要に応ずるために提供する事業」ではないので、法による届け出を行わないことができると結論づけた。

### 刑法
[条文](https://elaws.e-gov.go.jp/document?lawid=140AC0000000045)

ここでは (よく議論を呼ぶ) 刑法第百七十五条を議論する。

> （わいせつ物頒布等）
> 
> 第百七十五条　わいせつな文書、図画、電磁的記録に係る記録媒体その他の物を頒布し、又は公然と陳列した者は、二年以下の懲役若しくは二百五十万円以下の罰金若しくは科料に処し、又は懲役及び罰金を併科する。電気通信の送信によりわいせつな電磁的記録その他の記録を頒布した者も、同様とする。
> 
> ２　有償で頒布する目的で、前項の物を所持し、又は同項の電磁的記録を保管した者も、同項と同様とする。

今の所そのようなわいせつ物頒布としてみなされるような画像が流入するような原因はないが、もしある場合は対策を講じなければならない。すなわち、投稿の非表示化である。これは投稿を流入させないようにする措置ではなく、投稿を流入させるが、該当の投稿はインスタンスのアカウント認証無しでアクセスできないようにステータスコード[`451`](https://developer.mozilla.org/ja/docs/Web/HTTP/Status/451)を返すなど、ソフト面での措置を講じることである。

投稿を非表示化することによって、私が刑法第百七十五条において規定される「公然と陳列した者」としてみなされる可能性は限りなく低くなると考えられる。

また、場合によっては (メディアキャッシュの受け入れ拒否がなかった頃にPawooが受けていたように) インスタンスごとブロックすることも措置に入れることも検討する。

将来的にマルチユーザーにする場合は、この条文に違反するリスクが払拭できないので利用規約で制限する必要があると考える。

#### プロバイダー責任制限法

### 外国法の議論
#### DMCA
米国でホストされているわけではないので対象外だと結論づけた。将来的に対応するべきときが来たら改めて考える。

#### GDPR
きちんと対応すると非常にややこしい(少なくとも、そういう印象を受けた)ので、EU諸国からのアクセスを禁止するべきかもしれない。

## 要件定義
各項の末尾にある「要求度」は以下の通り。
* **MUST**: 絶対に妥協できない。当てはまらなければ使わない。
* **MUST NOT**: 絶対に妥協できない。当てはまるなら使わない。
* **SHOULD**: 妥協できるが、あったほうが好ましい。
* **SHOULD NOT**: 妥協できるが、無いほうが好ましい。

### バックエンド
#### バックエンドの機能要件
* シングルユーザーでも動く (MUST)
* 他のソフトウェアと連携が容易 (MUST)
* サーバー外部とのデータ交換形式にRPC (Protocol Buffers, Thrift, gRPC, tRPC, ...) を使わないことができる (MUST)
* 投稿の閲覧権限を管理できる (MUST)
  * 投稿の閲覧権限を外部サーバーに応じてかえることができる (SHOULD)
    * 投稿の閲覧権限をアカウントに応じて変えることができる (SHOULD)
* DiscordへWebhookを通じて連携することができる (SHOULD)
* 必要がない機能を無効にできる (SHOULD)
* テレメトリがある場合は無効にできる (MUST)
* 「インスタンスのリスト」へサーバードメインを送信する機能を持っている場合は、その機能を無効にできる (MUST)
* ActivityPub対応 (SHOULD)
* ユーザー作成を制限できる (MUST)

#### バックエンドの非機能要件
* 各エンドポイントはレスポンスが500ミリ秒以内に返せること (SHOULD)
* 最小構成のサーバーはメモリ使用量が1ギビバイト以下で動かせること (SHOULD)
* 最小構成のサーバーはストレージ使用量が1ギビバイト以下で動かせること (SHOULD)
* 最小構成のサーバーは起動時間が15秒以下であること (SHOULD)
  * ここでの「起動時間」とは、コマンドによる立ち上げを行ってからサーバーがリクエストを受け付けられるようになるまでに消費する時間である
* ライセンスがコピーレフト条項が入ったものではない (SHOULD NOT)
  * AGPLv3 (一番多い)
  * GPLv3
  * MPL
* リリース用チャンネルにマージされるまでに単体テストが最低1回は行われていること (SHOULD)
* リリース用チャンネルにマージされるまでに結合テストが最低1回は行われていること (SHOULD)
* 前2項のテストが正常に成功した場合のみマージが行われていること (SHOULD)

### フロントエンド
#### フロントエンドの機能要件
* 複数アカウントをTweetdeckのように同時閲覧することが可能 (SHOULD)
* リモートフォローへの動線がわかりやすい (SHOULD)
  * misskeyの公式フロントエンドは「検索」というラベルがリモートフォローへつながる動線であることが直感的ではない

