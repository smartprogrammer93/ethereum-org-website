---
title: スケーリング
description: 現在イーサリアムコミュニティにおいて開発中の様々なスケーリングのオプションを紹介する。
lang: ja
sidebarDepth: 3
---

## スケーリングの概要 {#scaling-overview}

イーサリアムのユーザー規模が拡大するにつれ、イーサリアムブロックチェーンの処理能力は限界に達しつつあります。 これにより、イーサリアムネットワークの使用コストが増加しているため、「スケーリングソリューション」の必要性が高まってきました。 スケーリングを達成するという目標の下で、異なるアプローチを用いた様々なソリューションが研究、テスト、および実装されています。

スケーラビリティの取り組みにおける主な目標は、分散化やセキュリティを犠牲にすることなく、トランザクション速度の向上（ファイナリティ到達までの時間短縮）ならびにトランザクションスループットの向上（毎秒あたりの処理件数の向上）を実現することです（詳細については、[イーサリアムのビジョン](/roadmap/vision/)をご覧ください）。 レイヤー 1 のイーサリアムブロックチェーンでは、需要が増化するとトランザクションが遅延し、[ガス価格](/developers/docs/gas/)が高騰します。 イーサリアムが有意義かつ大規模に利用されるようになるためには、ネットワークの速度とスループットを改善することが不可欠です。

速度とスループットが重要である一方で、スケーリングのソリューションはこれらの目標を、分散化とセキュリティという特性を失わずに実現しなければなりません。 中央集権化およびセキュリティの低下を防ぐためには、どんなユーザーでもノードとして参加できるように参入障壁を低くしておくことが重要です。

スケーリングの概念では、まずオンチェーンのスケーリングとオフチェーンのスケーリングに分けて考えることができます。

## 前提知識 {#prerequisites}

イーサリアムに関する基本的なトピックすべてをよく理解しておく必要があります。 スケーリング技術は、バトルテストの実績が少なく、引き続き研究、開発が続けている状態であるため、スケーリングソリューションの実装は上級者向けのテーマだと言えます。

## オンチェーンにおけるスケーリング {#on-chain-scaling}

この手法は、イーサリアムプロトコル（レイヤー 1 の[メインネット](/glossary/#mainnet)）の変更を必要とします。 この手法によるスケーリングは、現在シャーディングが主流になっています。

### シャーディング {#sharding}

シャーディングとは、負荷を分散するためにデータベースを水平的に分割するプロセスを指します。 イーサリアムにおけるシャーディングとは、「シャード」と呼ばれる新たなチェーンを作成することで、ネットワークの混雑を解消し、毎秒あたりのトランザクション数を増加させる手法を指します。 これにより、バリデータは、イーサリアムネットワーク全体に含まれるすべてのトランザクションを処理する必要がなくなるため、負荷が軽減されます。

[シャーディング](/upgrades/sharding/) について詳しく知る。

## オフチェーンにおけるスケーリング {#off-chain-scaling}

オフチェーンのスケーリングソリューションは、レイヤー 1 のメインネットとは別個に実装されるため、既存のイーサリアムプロトコルを変更する必要がありません。 「レイヤー 2」ソリューションと呼ばれる一部のソリューションでは、[オプティミスティックロールアップ](/developers/docs/scaling/optimistic-rollups/)、[ゼロ知識ロールアップ](/developers/docs/scaling/zk-rollups/)、あるいは[ステートチャンネル](/developers/docs/scaling/state-channels/)のように、セキュリティ保護につきレイヤー 1 のイーサリアムコンセンサスに依存するものもあります。 一方、[サイドチェーン](#sidechains)、[バリディアム](#validium)、あるいは[プラズマチェーン](#plasma)といったソリューションでは、メインネットとは別個にセキュリティを保証する様々な形式の新規チェーンを作成します。 後者のソリューションでは、メインネットとやりとりするものの、各ソリューションの目標に合わせて様々な方法でセキュリティを維持しようとします。

### レイヤー 2 のスケーリング {#layer-2-scaling}

オフチェーンのソリューションのうち、セキュリティをイーサリアムメインネットに依存するものをレイヤー 2 のスケーリングソリューションと呼びます。

レイヤー 2 とは、メインネットが提供する堅牢かつ分散型のセキュリティモデルを活用しつつ、イーサリアムメインネット（レイヤー 1）外でトランザクションを実行することでアプリケーションのスケーラビリティを実現しようとするソリューションの総称です。 ネットワークの混雑によりトランザクション速度が低下すると、一部の Dapp では優れたユーザー体験を提供できなくなります。 さらに、ネットワークが混雑すると、トランザクションの送信者が価格をつり上げることでガス価格が上昇します。 これにより、イーサリアムの使用コストが非常に高額になる可能性があります。

大部分のレイヤー 2 ソリューションは、1 台または複数のサーバークラスタで構成され、各サーバーはノード、バリデータ、オペレーター、シーケンサー、ブロック生成者といった名称で呼ばれます。 これらのレイヤー 2 のノードは、実装により、ノードを使用する個人、企業、または組織が実行する場合、サードパーティ事業者が実行する場合、あるいは多数の個人が参加するグループが実行する場合（メインネットの場合と同様）があります。 トランザクションは通常、レイヤー 1（メインネット）に直接送信されるのではなく、これらのレイヤー 2 のノードに送信されます。 一部のソリューションでは、トランザクションは送信先のレイヤー 2 のインスタンスにおいてバッチとしてグループ化されてからレイヤー 1 に固定されるため、その後はレイヤー 1 において確定され、改変できなくなります。 このプロセスが具体的にどう実行されるかは、様々なレイヤー 2 のテクノロジー／実装により大きく異なります。

個別のレイヤー 2 インスタンスは、多くのアプリケーションに解放され、共有される場合と、特定のプロジェクトによりデプロイされ、関連アプリケーションのみを専用でサポートする場合があります。

#### レイヤー 2 が必要な理由 {#why-is-layer-2-needed}

- 毎秒あたりのトランザクション数を増加させることで、ユーザー体験が向上し、イーサリアムメインネットの混雑を軽減できる。
- 複数のトランザクションを 1 つのトランザクションにロールアップしてメインネットに送信するため、ガス代が軽減でき、イーサリアムの包摂性が高まり、すべての人が利用できるようになる。
- スケーラビリティを実現するためのアップデートは、分散化やセキュリティを犠牲にしてはならないが、レイヤー 2 はイーサリアムの基盤に基づくネットワークである。
- アプリケーションごとに特化されたレイヤー 2 のネットワークでは、大規模な資産を取り扱う際に独自の効率性アップが実現できる。

[レイヤー 2 の詳細](/layer-2/)

#### ロールアップ {#rollups}

ロールアップとは、レイヤー 1 の外部でトランザクションを実行した上で、データをレイヤー 1 に送信し、そこでコンセンサスを得るという手段です。 トランザクションデータはレイヤー 1 のブロックに含まれるため、ロールアップの安全性はイーサリアムのネイティブセキュリティにより保証されます。

ロールアップには、異なるセキュリティモデルを採用した以下の 2 種類があります：

- **オプティミスティック・ロールアップ**: トランザクションはデフォルトで有効であると仮定し、チャレンジが提起された場合のみ[**不正証明**](/glossary/#fraud-proof)を通じて計算を実行します。 [オプティミスティック・ロールアップの詳細](/developers/docs/scaling/optimistic-rollups/)。
- **ゼロ知識ロールアップ**: オフチェーンで計算を実行し、[**有効性証明**](/glossary/#validity-proof)をチェーンに送信します。 [ゼロ知識ロールアップの詳細](/developers/docs/scaling/zk-rollups/)。

#### ステートチャンネル {#channels}

ステートチャネルでは、マルチシグのコントラクトを通じて、参加者はオフチェーンで迅速かつ自由に取引を行うことでき、その上でメインネット上でファイナリティを実現します。 これにより、ネットワークの混雑、手数料、および遅延を最小限に抑えることができます。 現在利用されているチャネルとしては、ステートチャネルとペイメントチャネルがあります。

[ステートチャネル](/developers/docs/scaling/state-channels/)について詳しく知る。

### サイドチェーン {#sidechains}

サイドチェーンは、EVM 互換の独立したブロックチェーンであり、メインネットと並行して実行されます。 イーサリアムとの互換性は双方向ブリッジで実現され、トランザクションは独自に選択したコンセンサスルールとブロックパラメータに基づいて実行されます。

[サイドチェーン](/developers/docs/scaling/sidechains/)について詳しく知る。

### プラズマ {#plasma}

プラズマチェーンとは、メインのイーサリアムチェーンにおいて固定された別個のブロックチェーンで、不正証明（[オプティミスティック・ロールアップ](/developers/docs/scaling/optimistic-rollups/)など）を用いて紛争を仲裁します。

[プラズマ](/developers/docs/scaling/plasma/)について詳しく知る。

### バリディアム {#validium}

バリディアムチェーンは、ゼロ知識ロールアップと同様に有効性証明を使用しますが、データはレイヤー 1 であるメインのイーサリアムチェーン上に保存されません。 これにより、バリディアムチェーンごとの毎秒あたりのトランザクション数は 1 万件に達し、複数のバリディアムチェーンを並行して実行することができます。

[バリディアム](/developers/docs/scaling/validium/)について詳しく知る。

## 様々なスケーリングソリューションが求められる理由とは？ {#why-do-we-need-these}

- 様々なソリューションは、イーサリアムネットワークにおける各部分の全般的な混雑を緩和できるだけでなく、単一障害点の発生を防ぐために役立ちます。
- 複数のソリューションは、その全体においてさらに効力を発揮します。 つまり、様々なソリューションが共存し、調和的に機能することで、トランザクションの速度／スループットを今後爆発的に向上させることが可能になります。
- すべてのソリューションにおいてイーサリアムのコンセンサス・アルゴリズムを直接活用する必要があるわけではなく、様々な代替手段によりその他の方法では得にくいメリットを得ることができます。
- 特定のスケーリング・ソリューションが[イーサリアムのビジョン](/roadmap/vision/)を完全に満たすことは不可能です。

## 映像で学びたい場合 {#visual-learner}

<YouTube id="BgCgauWVTs0" />

_この動画の説明では、「レイヤー 2」という用語をオフチェーンでのスケーリング・ソリューション全般を指すものとして使用していますが、本記事では、「レイヤー 2」につき、レイヤー 1（メインネット）のコンセンサスに依存してセキュリティを保護するオフチェーンソリューションを指す用語としてを用いています。_

<YouTube id="7pWxCklcNsU" />

## 参考文献 {#further-reading}

- [ロールアップを重視したイーサリアム・ロードマップ](https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698) _ヴィタリック・ブテリン作成。_
- [イーサリアムのレイヤー 2 スケーリング・ソリューションに関する最新のアナリティクス](https://www.l2beat.com/)
- [イーサリアムの様々なレイヤー 2 のスケーリングソリューションを評価する：比較のフレームワーク](https://medium.com/matter-labs/evaluating-ethereum-l2-scaling-solutions-a-comparison-framework-b6b2f410f955)
- [ロールアップに関する不完全ガイド](https://vitalik.eth.limo/general/2021/01/05/rollup.html)
- [イーサリアムを活用したゼロ知識ロールアップ：ワールドビーター](https://hackmd.io/@canti/rkUT0BD8K)
- [オプティミスティック・ロールアップ とゼロ知識ロールアップの比較](https://limechain.tech/blog/optimistic-rollups-vs-zk-rollups/)
- [ゼロ知識によるブロックチェーンのスケーラビリティ](https://ethworks.io/assets/download/zero-knowledge-blockchain-scaling-ethworks.pdf)
- [ロールアップとデータシャードを組み合わせる手段が、高スケーラビリティを実現する唯一のサステナブルなソリューションである理由](https://polynya.medium.com/why-rollups-data-shards-are-the-only-sustainable-solution-for-high-scalability-c9aabd6fbb48)
- [有意義なレイヤー 3 とはどのようなものか？](https://vitalik.eth.limo/general/2022/09/17/layer_3.html)

_イーサリアムを学ぶために利用したコミュニティリソースはありますか？ このページを編集して追加しましょう！_
