リアクティブ・マニフェスト
----------------------

[マニフェストにサインしよう(本家へ)](http://www.reactivemanifesto.org/)

## リアクティブに向かう必要性 The Need to Go Reactive

昨今、アプリケーションに求められる要件は劇的に変化してきました。わずか数年前、大きなアプリケーションは、数十のサーバー、何秒かの応答時間、何時間かのオフラインでの保守作業、ギガバイト級のデータから構成されていました。今日では、アプリケーションはモバイルデバイスから数千のマルチコアプロセッサが稼働するクラウドベースのクラスタまでの全ての環境にデプロイされます。ユーザーはミリ秒単位のレスポンス時間と100%の可用性を要求します。データの需要はペタバイト級に膨らんでいます。

当初は、Google や Twitter のような革新的なインターネットドリブンな企業の周辺において、多くの分野でこれらのアプリケーションの特徴が表出しています。金融や通信業界が新しい要求を満たすために新しいプラクティスを適用した先駆者であり、その他の業界が続いています。

新しい要求は、新しい技術を要求します。以前のソリューションは、管理されたサーバとコンテナに重点をおいていました。スケーリングは、より大きなサーバを購入しマルチスレッドによる並行実行処理によって成し遂げられました。追加のサーバは複雑で非効率で高価なプロプライエタリなソリューションを通じて追加されました。

しかし、今や新しいアーキテクチャが進化してきており、開発者が今日の要求を満たすコンセプトを作成したりアプリケーションを構築することができるようになってきました。私達は、これらを  *リアクティブ・アプリケーション* と呼びます。このアーキテクチャは、開発者に *イベント駆動で、スケーラブルで、回復力があり、インタラクティブな* システムを構築することを可能にします。 それは、リアルタイム感のある、高度にインタラクティブなユーザー体験を提供し、スケーラブルで回復力のあるアプリケーションのスタックに支えられ、マルチコアでクラウドコンピューティングのアーキテクチャにデプロイ可能です。リアクティブ・マニフェストは、 *リアクティブに向かう*ために必要なこれらの決定的に重要な特徴について記述します。

## リアクティブ・アプリケーション Reactive Applications

Merriam-Webster (訳注: オンライン辞書、[Merriam-Webster](http://www.merriam-webster.com/)) は、リアクティブを *"刺激に対して直ちに反応する"* と定義しています。すなわち、そのコンポーネントは、"アクティブ" であり、常にイベントを受け付けられるようになっています。この定義は、リアクティブ・アプリケーションの要諦を捉えています、システムに着目した時以下の様なことが言えます。

- *イベントに反応する*: イベント駆動という性質は、結果として高品質を可能にする
- *読み込みに反応する*: ひとりのユーザのパフォーマンスよりスケーラビリティに注力する
- *失敗に反応する*: すべてのレベルにおいて立ち直ることができる回復力のあるシステムを構築する
- *ユーザーに反応する*: 上記の特徴群をインタラクティブなユーザー体験に混ぜ合わせる	

これらは、どれもリアクティブ・アプリケーションの本質的な特性を表します。これらは互いに依存関係がある一方で、標準的なレイヤー化されたアプリケーションアーキテクチャで意味するところの階層のような関係ではありません。むしろ、技術スタック全体に適用するような設計の属性について述べているものです。

![fig. 1 The Reactive Traits](../images/stack.png)

これ以降、これら四つの本質をもっと深く見ていき、お互いにどのように関係するかを見ていきます。

## イベント駆動 Event-driven

### なぜ重要なのか

非同期通信をベースとしたアプリケーションは、純粋に動機的なメソッド呼び出しをベースとしたアプリケーションより、非常に素晴らしく *疎結合な* 様式を実装します。送信者と受信者は、イベントがどのように伝播されるかの詳細を留意することなく実装され、通信の内容に注力したインターフェースを可能にします。これにより拡張、進化、保守することが容易な実装となり、より優れた柔軟性を提供し、保守コストを低減することができます。

非同期通信における受信者は、イベントが発生するかメッセージを受領するまで休止したままであるため、イベント駆動アプローチはリソースを効率的に利用でき、多くの受信者がひとつのハードウェアスレッドを共有できます。重い負荷がかかっている状態においてもノンブロッキングなアプリケーションは、 ブロッキングの同期と原始的な通信に基づく伝統的なアプリケーションに比べて、 *より短い遅延とより高いスループット* を実現することができます。その結果、より低い運用時のコストと、より高い可用性、よりハッピーなエンドユーザーへつながります。

### キーとなる構成要素

イベント駆動のアプリケーションにおいては、コンポーネントは *イベント* - 事象を描く個々の情報のかけら - の生産と消費を通じてお互いに作用します。これらのイベントは、非同期でノンブロッキングな作法で送信したり受信したりします。イベント駆動システムは、 *プル* や *ポーリング* よりも *プッシュ* に頼る傾向があります。すなわち、消費者に継続的にデータを訪ねさせたり、データの到着を待たせたりしてリソースを浪費してしまう代わりに、可能な限り消費者に向かってデータをプッシュします。

- *非同期* イベントの送信 - *メッセージ授受とも呼ばれる* - は、アプリケーションが計画的に高い並行実行性を実現し、マルチコアのハードウェアを何も変更することなく活用できることを意味する。ひとつのCPU 上のどのコアにおいても、どのメッセージイベントを処理することができ、並列化の機会を劇的に増加させることとなる。
- *ノンブロッキング* は、不活性なコンポーネントはサスペンドされ、リソースが開放され、他のコンポーネントに利用されるため、アプリケーションがハードウェアの活用という点において非常に効率的であるということを意味する。

伝統的なサーバサイドのアーキテクチャは、単一スレッド上の共有された可変ステートとブロッキング操作に依っています。システムが要求の変更を満たすためにスケールしたいときに、それらの両方ともが困難を引き起こす要因となるでしょう。共有された可変ステートは同期を要求し、それは偶発的な複雑さと非決定性を生み出し、コードを理解しづらく保守しづらくします。ブロッキングによってスレッドをスリープさせることで、限られたリソースを使い切り、また、高いウェイクアップコストを招くことになります。

イベントの生成と処理とを切り離すことによって、実行時のプラットフォームは同期の詳細とスレッド間でどのようにイベントを振り分けるかを気遣うことができ、また、ビジネスワークフローのレベルまでプログラムの抽象化度を引き上げられます。スレッドやロックといった低レベルでの原始的なつまらないことにではなく、どのようにイベントがシステム内で伝播するかや、どのようにコンポーネント群が相互作用するかについて、考えを集中することができます。

イベント駆動システムは、コンポーネントやサブシステムの間を疎結合にします。このレベルの間接性は、あとで見るように、スケーラビリティと回復力の前提条件のひとつになります。コンポーネント間の複雑で強い密結合を取り除くことによって、イベント駆動アプリケーションは現存するアプリケーションに対しての影響を最小限に留めなら、拡張していくことができます。

アプリケーションが高い性能と大きなスケーラビリティの要求にさらされたとき、どこにボトルネックが発生するかを予測することは困難です。したがって、ソリューション全体が非同期でノンブロッキングになることが重要です。典型的な例において、UIにおけるユーザーリクエスト(ブラウザ、RESTクライアント、他)から、リクエスト解析とウェブレイヤーでの振り分け、ミドルウェアでのサービスコンポーネント、そして、キャッシングやデータベースへ下るところまで、イベント駆動な設計をする必要があります。もし、これらのレイヤーのうち、ひとつでも参加しないのなら、- たとえば、データベースへのブロッキングコール、共有された可変ステートへの依存、高価な同期的操作の呼び出しなど - そのとき、全体のパイプラインは行き詰まり、ユーザーは遅延の増加とスケーラビリティの減少に苦しむことになります。

アプリケーションは、 *徹底的にリアクティブ* でなければなりません。

一連のチェーンにおける最も弱い環を減らしていくことの必要性は、アムダールの法則がよく説明してくれます。ウィキペディアによれば、	

> 複数のプロセッサを使い並列計算によってプログラムの高速化を図る場合、そのプログラムの中で逐次的に実行しなければならない部分の時間によって高速化が制限される。例えば、1プロセッサでは20時間かかるプログラムがあり、その中の1時間かかる部分が並列化できないとする。したがって19時間ぶん（95%）は並列化できるが、どれだけプロセッサを追加して並列化していったとしても、そのプログラムの最小実行時間は1時間より短くならない。図にも示したように、この場合の高速化は20倍までが限界である。(訳注: [ウィキペディア - アムダールの法則](http://ja.wikipedia.org/wiki/%E3%82%A2%E3%83%A0%E3%83%80%E3%83%BC%E3%83%AB%E3%81%AE%E6%B3%95%E5%89%87))

![fig. 2 Amdahl's Law](../images/amdahl.png)

## スケーラブル Scalable

### なぜ重要なのか

Merriam-Webster によれば、スケーラブルという単語は、 *必要に応じて簡単に拡張したり改善したりする能力がある* と定義されています。スケーラブルなアプリケーションは、その用法に準じた拡張が可能です。これは、要求に応じてスケールアウト・スケールイン(ノードの追加・削除)ができるオプションによって、アプリケーションに弾力性を加えることで実現されます。加えて、このアーキテクチャでは、アプリケーションを再設計したり書きなおしたりすることなく、スケールアップ・スケールダウン(より多くの、またはより少ないCPUを持つノードを配置)することをも容易にします。弾力性は、クラウドコンピューティング環境におけるアプリケーション運用コストを最小限にし、仮想化された使った分だけ課金するモデルを可能にするという利点をもたらします。

スケーラビリティはリスクの管理にも貢献します。そのリスクとは、少なすぎるハードウェアによってユーザー負荷に追いつけなくなり不快や顧客の流出をもたらしてしまうこと、または、大きすぎるハードウェア - および多すぎる運用人員 - によって意味なくアイドルさせてしまい不要な支出を招くことです。スケーラブルなソリューションは、新しいハードウェアで可能になるようなことを活用できないアプリケーションで終わってしまうリスクも低減します。新たな可能性とは、次の10年で数千までは行かずとも数百のプロセッサーを積んだハードウェアのスレッドが見えており、そして、その可能性を活かし切るためには、アプリケーションには実にきめ細かくスケーラブルであることを求められます。

### キーとなる構成要素

非同期なメッセージ授受に基づいたイベント駆動システムはスケーラビリティの基盤を提供します。コンポーネントとサブシステムの間を疎結合にし、同じプログラミングモデルと同じ意味(セマンティクス)を保持しながら、複数のノード上にシステムをスケールアウトできるようになります。より多くのコンポーネントのインスタンスを追加することで、システムがイベントを処理する能力が増加します。実装の観点においては、マルチコアの活用によってスケールアップすることと、データセンターやクラスタのノードを増やすことによってスケールアウトすることに一切の違いがありません。アプリケーションの接続形態(トポロジー)は、アプリケーションに使用方法に対応した、設定や(と)適合可能な実行時のアルゴリズムによって表現されるような、デプロイ時の決定事項になります。これを位置透過性(ロケーション・トランスパレンシー、[location transparency](http://en.wikipedia.org/wiki/Location_transparency)) と呼びます。

重要なのは、透過的な分散コンピューティングや、分散オブジェクト、または RPC スタイルの通信を実装しようとすることではありません。- それは、挑戦され続け失敗され続けてきました。そうではなく、非同期なメッセージ授受を通じてプログラミングモデルに直接的に表現することで *ネットワークを受け入れる* ことが必要なのです。真のスケーラビリティは、私達が知るかぎり本質的に信頼できない([inherently unreliable](http://aphyr.com/posts/288-the-network-is-reliable)) ネットワークを横断(トラバース)するノード間の通信とともに、自然に分散コンピューティングを内包します。したがって、物事を"単純化しよう"とすることで漏れが生じてしまうような抽象化で覆い隠してしまうのではなく、プログラミングモデルに明示的にネットワークプログラミングの制約、トレードオフ、失敗シナリオを組み込むことが重要なのです。結果として、分散環境で発生するような典型的な問題を解決する共通の構成要素をカプセル化するプログラミングの道具を提供することも、同じように重要になります。それは、より高いレベルの信頼性を提供する合意の形成のメカニズムやメッセージングの抽象化のようなものです。

## 回復力のある Resilient

### なぜ重要なのか

アプリケーションのダウンは、ビジネスに起こりうつ最も痛手な問題のひとつです。通常、それが意味するところは、業務がただただストップし、収益を生むシステムに穴を開けてしまうことです。長い目で見た時には不満な顧客と悪い評判を生み出し、ビジネスを更に深刻に傷つけます。アプリケーションの回復力が、要件から全く無視される、または、一時しのぎな技術の据え付けで済ましてしまうことは驚くべきことです。これは、しばしば、粗すぎる道具を使って異なったレベルの粒度の問題として位置づけられてしまうことを意味します。一般的な技術として、実行時のサーバの故障から回復するためにサーバクラスタリングがあります。不運なことに、サーバーのフェイルオーバーは著しくコストが掛かり、また同時に危険でもあります。- 失敗の連鎖を生みクラスタ全体をダウンさせてしまう可能性があるためです。これは間違ったレベルの粒度の失敗のマネジメントであり、代わりにコンポーネントレベルの微細な回復力によって代替されるべきものです。

Merriam-Webster は、resilience を次のように定義しています。

- *元々の形に息を吹き返す、物質や物体の能力* 
- *困難から迅速に回復する能力*

リアクティブ・アプリケーションにおいて、回復力は後から考えるものではなく、当初からの設計の一部です。そのプログラミングモデルにおいて失敗をファーストクラスの構成物として扱うことは、失敗に反応し、失敗を管理するということを意味し、実行時に回復でき修復できることで失敗に対しての高い耐久力があるアプリケーションへと導きます。伝統的な失敗への対処では、小さな環境では防御的すぎて、大きな環境では挑戦的すぎるため、これを達成することができません。言い換えるなら、いつどこで起ころうと、例外を(ひとつひとつ)正しく対処するか、アプリケーションインスタンス全体にフェールオーバーを備えるか(の両極端)になってしまいます。

### キーとなる構成要素

*失敗を管理* するためには、他の健全なコンポーネントに波及しないよう *孤立させる* (isolate) 方法と、失敗のコンテキストの外側にある安全なポイントから管理されるように *観察する* (observe) 方法とが必要です。思いつくひとつのパターンは、(船の)隔壁パターン([bulkhead pattern](http://skife.org/architecture/fault-tolerance/2009/12/31/bulkheads.html)) です。図にあるように、ひとつが壊れても他が影響を受けないように安全な区画を立てることによってシステムを築くのです。これにより、古典的な問題である連鎖的な失敗 ([cascading failures](http://en.wikipedia.org/wiki/Cascading_failure)) を防ぐことができ、問題を孤立した領域で管理することができます。

![fig 3 Bulkheads](../images/tank.png)

スケーラビリティを可能にするイベント駆動モデルは、この失敗管理のモデルを実現するのに必須の資質を持ち合わせています。イベント駆動モデルにおける疎結合性は、失敗を、文脈とともに捕捉させ、メッセージとしてカプセル化し、エラーを分析しどのように応答するかを決定することができる他のコンポーネントに投げてしまえるような、完全に孤立化させたコンポーネントを提供します。

このアプローチは、ビジネスロジックをクリーンに保ち、予期せぬことへの対処から分離するシステム、失敗が、区画化され観察され管理され宣言的な方法で設定されるために明示的にモデル化されたシステム、それ自身を治癒し自動的に復活するシステムを作り出します。もし区画が階層的なやり方で構造化されているなら最も素晴らしく動作し、それは処理する力を持つレベルまで到達するまで、問題が上にエスカレートとする大きな企業のように振る舞います。

このモデルの美しさは、純粋にイベント駆動であり、リアクティブなコンポーネントと非同期なイベントにもとづいており、結果的に *位置透過性* を持つことにあります。実際のところ、このことはローカルのコンテキストの場合と同じような意味(セマンティクス)で分散環境で動作することを意味します。

## インタラクティブ Interactive

### なぜ重要なのか

インタラクティブなアプリケーションは、リアルタイムであり、魅力があり、リッチでコラボレーションに富んでいます。ビジネスは、インタラクティブな体験を通して顧客を歓迎し、顧客とのオープンで継続的な対話を作ります。これにより、顧客の能率は向上し、問題を解決し作業を完遂するための繋がっている感じと設備が整った感じを提供します。ひとつの例ですが、Google Docs は、作業している間、お互いの変更とコメントをライブで見ながらリアルタイムに協力して編集することを可能にます。
 
リアルタイムで意味ある情報に変換されたデータと相互作用することができるなら、ユーザーは力を与えられます。インタラクティブなアプリケーションは、どのインタフェースにも本来的に備わっているこの情報とコラボレートすることで、もっと効率的にもっと頻繁にコミュニケーションをとれるようになります。伝統的なページ全体の扱いからアイテムレベルにフィードバックを小さく増やしていくことでもっと深まっていきます。 例として、Eメールのシングル・ページのウェブクライアントがあります。遠い距離を超えた瞬時のソーシャルなインタラクションは、人々がどのように情報と向き合い、お互いにやりとりしていくかを根本的に変えていきます。たとえば、GitHub は、インタラクティブなブラウザベースのアプリケーションにより、"ソーシャル・コーディング" を通じて開発者のコラボレーションに革命を起こしつつあります。もちろん、Twitter は、ニュースが拡散する方法を根本的に変えてきました。

イベント駆動の基礎の上に構築することで、リアクティブ・アプリケーションはインタラクティブ性も同時にうまく備えます。スケーラビリティは、アプリケーションに人気が出てきた時にも、この属性を保ち続けることは必須です。回復力はユーザーが継続的にその機能を楽しみ続けることを確実にします。

### キーとなる構成要素

リアクティブ・アプリケーションは、観察可能なモデルとイベントストリーム、ステートフルなクライアントを利用します。	

観察可能なモデルは、状態が変化した時に他のコンポーネントがイベントを受け取ることを可能にします。これは、ユーザーとシステムの間にリアルタイムの接続を提供します。たとえば、複数のユーザーが同じデータセットに同時並行的に作業するとき、変更はリアクティブに彼らの間で双方向に同期されます。

イベント・ストリームは、この接続が構築されるための基本的な抽象化を形成します。 それらをリアクティブに保つことはブロッキングを避ける事を意味し、代わりに非同期でノンブロッキングな変換を可能にします。たとえば、リアルタイムデータのストリームは、解析されたデータの新たなストリームを生み出す鮮やかな射影を通して受け渡されます。

最もリアクティブなアプリケーションは、魅力あるユーザー体験を創りだすリッチウェブとモバイルクライアントです。これらのアプリケーションは、データが変更された時にリアルタイムでユーザーインタフェースを更新する機構を提供する観察可能なモデルのもと、クライアントサイドでロジックを実行し状態を保持します。Web Socket や サーバプッシュ型イベントのような技術は、イベントストリームに直接的に接続されるユーザーインタフェースを可能にし、イベント駆動システムはバックエンドからクライアントまで全体にわたってに広がることができます。これにより、リアクティブ・アプリケーションは、非同期やノンブロッキングなデータ移送によって、スケーラブルで回復力のある方法で、イベントをブラウザやモバイルアプリケーションにプッシュすることが可能になります。

この点を念頭に置くと、凝集性のある全体像を形作るために、四つの本質である *イベント駆動* と *スケーラブル*、 *回復力がある*、 *インタラクティブ* がどのように相互に関連しているかが明らかになります。

![fig 4. The Reactive Traits](../images/full-reactive.png)

## 結論 Conclusion

リアクティブ・アプリケーションは、ソフトウェア開発における幅広い現代の困難に向けてのバランスのとれたアプローチを示します。 *イベント駆動* のメッセージをベースとした基盤上に構築され、 *スケーラビリティ* と *回復力* を確かにするために必要な道具を提供します。そして、その上でリッチでリアルタイムなユーザー *インタラクション* を支えられるのです。私たちは、ここ何年かのうちに急速に増加しつつあるシステムたちがこの見取り図に追随してくれることを期待しています。

[マニフェストにサインしよう(本家へ)](http://www.reactivemanifesto.org/)

