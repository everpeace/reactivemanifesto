
## 注：まだ作業中です！

リアクティブ マニフェスト
----------------------

[マニフェストにサインしよう(本家へ)](http://www.reactivemanifesto.org/)

## リアクティブに向かう必要性 The Need to Go Reactive

_Application requirements have changed dramatically in recent years. Only a few years ago a large application had tens of servers, seconds of response time, hours of offline maintenance and gigabytes of data. Today applications are deployed on everything from mobile devices to cloud-based clusters running thousands of multicore processors. Users expect millisecond response times and 100% uptime. Data needs are expanding into the petabytes._

昨今、アプリケーションに求められる要件は劇的に変化してきました。わずか数年前、大きなアプリケーションは、数十のサーバー、何秒かの応答時間、何時間ものオフラインでのメンテナンス、ギガバイト級のデータから構成されていました。今日、アプリケーションは、モバイルデバイスから数千ものプロセッサが動作するクラウドベースのクラスタまでのすべてにデプロイされます。ユーザーはミリ秒単位のレスポンス時間と100%の可用性を要求します。データのニーズはペタバイト級に膨らんでいます。

_Initially the domain of innovative internet-driven companies like Google or Twitter, these application characteristics are surfacing in most industries. Finance and telecommunication were the first to adopt new practices to satisfy the new requirements and others have followed._

最初に、Google や　Twitter のような革新的なインターネット駆動の企業の領域、これらのアプリケーションの特徴が多くの業界で表出しています。金融や通信業界が、新しい要求を満たすために新しいプラクティスを適用した先駆者であり、その他の業界が続いています。

_New requirements demand new technologies. Previous solutions have emphasized managed servers and containers. Scaling was achieved through buying larger servers and concurrent processing via multi-threading. Additional servers were added through complex, inefficient and expensive proprietary solutions._

新しい要求は、新しい技術を要求します。以前のソリューションは、管理されたサーバとコンテナに重点をおいていました。スケーリングは、新しい大きなサーバを購入しマルチスレッドによる同時実行プロセスによって成し遂げられました。追加のサーバは複雑で非効率で高価なプロプライエタリなソリューションを通じて追加されました。

_But now a new architecture has evolved to let developers conceptualize and build applications that satisfy today’s demands. We call these *Reactive Applications*. This architecture allows developers to build systems that are *event-driven, scalable, resilient and interactive*: delivering highly interactive user experiences with a real-time feel, backed by a scalable and resilient application stack, ready to be deployed on multicore and cloud computing architectures. The Reactive Manifesto describes these critical traits which are needed for *going reactive*._

しかし、今や新しいアーキテクチャが進化してきており、開発者が今日の要求を満たすコンセプトを作成したりアプリケーションを構築することができるようになってきました。私達は、これらを  *リアクティブ・アプリケーション* を呼びます。このアーキテクチャは、開発者に *イベント駆動で、スケーラブルで、回復力があり、インタラクティブな* システムを構築することを可能にします。 それは、リアルタイム感のある高度にインタラクティブなユーザー体験を提供し、スケーラブルで回復力のあるアプリケーションのスタックがバックエンドにあり、マルチコアでクラウドコンピューティングアーキテクチャに配置可能です。リアクティブ・マニフェストは、*リアクティブに向かう*ために必要なこれらの決定的な特徴について記述します。


## リアクティブ・アプリケーション Reactive Applications

_Merriam-Webster defines reactive as *“readily responsive to a stimulus”*, i.e. its components are “active” and always ready to receive events. This definition captures the essence of reactive applications, focusing on systems that:_

Merriam-Webster (訳注: オンライン辞書、[Merriam-Webster](http://www.merriam-webster.com/) は、リアクティブを *"刺激に対して直ちに反応する"* と定義しています。すなわち、そのコンポーネントは、"アクティブ" であり、常にイベントを受け付けられるようになっています。この定義は、リアクティブ・アプリケーションの要諦を捉えています、システムにフォーカスした時：

- *react to events*: _the event-driven nature enables the following qualities_
- *react to load*: _focus on scalability rather than single-user performance_
- *react to failure*: _build resilient systems with the ability to recover at all levels_
- *react to users*: _combine the above traits for an interactive user experience_

- *イベントに反応*: イベント駆動という性質は、結果として高品質を可能にする
- *読み込みに反応*: ひとりのユーザのパフォーマンスよりスケーラビリティに注力する
- *失敗に反応*: すべてのレベルで回復することができる回復力のあるシステムを構築する
- *ユーザーに反応*: 上記の特徴をインタラクティブなユーザー体験に混ぜ合わせる	

_Each one of these is an essential characteristic of a reactive application. While there are dependencies between them, these traits are not like tiers in a standard layered application architecture sense. Instead they describe design properties that apply across the whole technology stack._

これらは、どれもリアクティブ・アプリケーションの本質的な特徴	です。これらは互いに依存関係がある一方で、標準的なレイヤー化されたアプリケーションアーキテクチャで意味するところの階層のような関係ではありません。むしろ、技術スタック全体に適用するような設計の属性について述べています。

![fig. 1 The Reactive Traits](../images/stack.png)

_In the following we will take a deeper look at each of the four qualities and see how they relate to each other._

以降、これらの本質をもっと深く見ていき、どのようにお互いに関係するかを見ていきます。

## イベント駆動 Event-driven

### なぜ重要なのか Why it is Important

_An application based on asynchronous communication implements a *loosely coupled* design, much better so than one based purely on synchronous method calls. The sender and recipient can be implemented without regards to the details of how the events are propagated, allowing the interfaces to focus on the content of the communication. This leads to an implementation which is easier to extend, evolve and maintain, giving you more flexibility and reducing maintenance cost._

非同期通信をベースとしたアプリケーションは、純粋に動機的なメソッドコールをベースとしたアプリケーションより、とてもよく *疎結合な* デザインを実装します。送信者と受信者は、イベントがどのように伝播されるかの詳細を留意することなく実装され、通信の内容に注力するインターフェースを可能にする。これにより拡張、進化、保守することが容易な実装となり、より多くの柔軟性を提供し、保守コストを低減することができます。

_Since the recipient of asynchronous communication can remain dormant until an event occurs or a message is received, an event-driven approach can make efficient use of existing resources, allowing large numbers of recipients to share a single hardware thread. A non-blocking application that is under heavy load can thus have *lower latency and higher throughput* than a traditional application based on blocking synchronization and communication primitives. This results in lower operational costs, increased utilization and happier end-users._

非同期通信における受信者は、イベントが発生するかメッセージを受領するまで休止したままであるため、イベント駆動アプローチはリソースを効率的利用でき、多くの受信者がひとつのハードウェアスレッドを共有できます。重い負荷がかかっているノンブロッキングなアプリケーションは、 原始的なブロッキングの同期と通信に基づく伝統的なアプリケーションに比べて、*より短い遅延とより高いスループット* を実現することができます。その結果、より低い運用時のコストと、高い利用性、よりハッピーなエンドユーザーへつながります。

### キーとなる構成要素 Key Building Blocks

_In an event-driven application the components interact with each other through the production and consumption of *events* — discrete pieces of information describing facts. These events are sent and received in an asynchronous and non-blocking fashion. Event-driven systems tend to rely on *push* rather than *pull* or *poll*, i.e. they push data towards consumers when it is available instead of wasting resources by having the consumers continually ask for or wait on the data._

イベント駆動のアプリケーションにおいては、コンポーネントは *イベント* - 事象を描く個々の情報のかけら - の生産と消費を通じてお互いに作用します。イベント駆動システムは、*プル* や *ポーリング* よりも *プッシュ* に依存する傾向があります。すなわち、消費者に継続的にデータを訪ねさせたり、データの到着を待たせてたりしてリソースを浪費してしまう代わりに、可能である限り消費者の方にデータをプッシュします。

- *Asynchronous*     _sending of events — also called *message-passing* — means that the application is highly concurrent by design and can make use of multicore hardware without changes. Any core within a CPU is able to process any message event, leading to a dramatic increase in opportunities for parallelization._
- *Non-blocking* _means that the application is very efficient in terms of hardware utilization since inactive components are suspended and their resources are released, to be used by other components._

- *非同期* イベントの送信 - *メッセージ受け渡しとも呼ばれる* - は、アプリケーションが生まれながらに高い同時実行性を実現し、マルチコアのハードウェアを何も変更することなく活用できることを意味する。CPU のどのコアにおいても、どのメッセージイベントを処理する小尾ができ、並列化の機会を劇的に増加させることとなる。
- *ノンブロッキング* は、不活性なコンポーネントはサスペンドされ、リソースが開放され、他のコンポーネントに利用されるため、ハードウェアの活用という点において、アプリケーションは非常に効率的であるということを意味する。

_Traditional server-side architectures rely on shared mutable state and blocking operations on a single thread. Both contribute to the difficulties encountered when scaling such a system to meet changing demands. Sharing mutable state requires synchronization, which introduces incidental complexity and non-determinism, making the program code hard to understand and maintain. Putting a thread to sleep by blocking uses up a finite resource and incurs a high wake-up cost._

伝統的なサーバーサイドのアーキテクチャは、単一スレッド上の共有のミュータブルステートとブロッキング操作に依っている。変わりうる要求を満たすためにシステムがスケールとき、それらの両方ともが困難に突き当たる(ToDo)。共有されたミュータブルステートは同期を要求し、それは偶発的な複雑さと非決定性を生み出し、プログラムコードを理解しづらく保守しづらくします。ブロッキングによってスレッドをスリープさせることは、限られたリソースを使い切り、高いウェイクアップコストを招くことになります。

_The decoupling of event generation and processing allows the runtime platform to take care of the synchronization details and how events are dispatched across threads, while the programming abstraction is raised to the level of business workflows. You think about how events propagate through your system and how components interact instead of fiddling around with low-level primitives such as threads and locks._

イベント生成と処理のデカップリングによって、ランタイムのプラットフォームは同期の詳細とスレッド間でどのようにイベントを振り分けるかを気遣うことができ、一方でプログラムの抽象化は、ビジネスワークフローのレベルまで引き上げられます。スレッドやロックと知った低レベルでの原始的なつまらないことにではなく、どのようにイベントがシステム内で伝播するか、どのようにコンポーネント群が相互に作用するかについて、あなたは考えることができます。

_Event-driven systems enable loose coupling between components and subsystems. This level of indirection is, as we will see, one of the prerequisites for scalability and resilience. By removing complex and strong dependencies between components, event-driven applications can be extended with minimal impact on the existing application._

イベント駆動システムは、コンポーネントとサブシステム間を、疎結合にします。このレベルの間接性は、あとで見るように、スケーラビリティと回復力の前提条件の1つです。コンポーネント間の複雑で密結合を取り除くことによって、イベント駆動アプリケーションは既に存在するアプリケーションに対しては最小限の影響に留めつつ拡張することができます。

When applications are stressed by requirements for high performance and large scalability it is difficult to predict where bottlenecks will arise. Therefore it is important that the entire solution is asynchronous and non-blocking. In a typical example this means that the design needs to be event-driven from the user request in the UI (in the browser, REST client or elsewhere) to the request parsing and dispatching in the web layer, to the service components in the middleware, through the caching and down to the database. If one of these layers does not participate — making blocking calls to the database, relying on shared mutable state, calling out to expensive synchronous operations — then the whole pipeline stalls and users will suffer through increased latency and reduced scalability.

アプリケーションが高い性能と大きなスケーラビリティの要求によってストレスされたとき(ToDo)、どこにボトルネックが発生するかを予測することは困難です。したがって、ソリューション全体が非同期でノンブロッキングになることが重要です。典型的な例において、UIにおけるユーザーリクエスト(ブラウザ、RESTクライアント、他)から、リクエスト解析とウェブレイヤーでの振り分け、ミドルウェアでのサービスコンポーネント、そして、キャッシングやデータベースへ下るところまで、イベント駆動なデザインである必要があります。もし、これらのレイヤーのうち、ひとつでも参加しないのなら、- たとえば、データベースへのブロッキングコール、共有可変状態(ミュータブルステート)への依存、高価な同期的操作の呼び出しなど - そのとき、全体のパイプラインは行き詰まり、ユーザーは遅延の増加とスケーラビリティの現象に苦しむことになります。

An application must be *reactive from top to bottom*.

アプリケーションは、 *徹底的にリアクティブ* でなければなりません。

The need for eliminating the weakest link in the chain is well illustrated by Amdahl’s Law, which according to Wikipedia is explained as:

一連の処理において最も弱いリンクを減らしていく必要性は、アムダールの法則としてよく描かれています。ウィキペディアによれば、	

> _The speedup of a program using multiple processors in parallel computing is limited by the sequential fraction of the program. For example, if 95% of the program can be parallelized, the theoretical maximum speedup using parallel computing would be 20× as shown in the diagram, no matter how many processors are used._

> 複数のプロセッサを使い並列計算によってプログラムの高速化を図る場合、そのプログラムの中で逐次的に実行しなければならない部分の時間によって高速化が制限される。例えば、1プロセッサでは20時間かかるプログラムがあり、その中の1時間かかる部分が並列化できないとする。したがって19時間ぶん（95%）は並列化できるが、どれだけプロセッサを追加して並列化していったとしても、そのプログラムの最小実行時間は1時間より短くならない。図にも示したように、この場合の高速化は20倍までが限界である。(訳注: [ウィキペディア - アムダールの法則](http://ja.wikipedia.org/wiki/%E3%82%A2%E3%83%A0%E3%83%80%E3%83%BC%E3%83%AB%E3%81%AE%E6%B3%95%E5%89%87))

![fig. 2 Amdahl's Law](../images/amdahl.png)

## スケーラブル Scalable

### なぜ重要なのか Why it is Important

_The word scalable is defined by Merriam-Webster as *“capable of being easily expanded or upgraded on demand”*. A scalable application is able to be expanded according to its usage. This can be achieved by adding elasticity to the application, the option of being able to scale out or in (add or remove nodes) on demand. In addition, the architecture makes it easy to scale up or down (deploying on a node with more or fewer CPUs) without redesigning or rewriting the application. Elasticity makes it possible to minimize the cost of operating applications in a cloud computing environment, allowing you to profit from its virtualized pay-for-what-you-use model._

Merriam-Webster によれば、スケーラブルという単語は、*必要に応じて簡単に拡張したり改善したりする能力がある* と定義されています。スケーラブルなアプリケーションは、その使用法によって拡張することができます(ToDo)。オンデマンドでスケールアウト・スケールイン(ノードの追加・削除)ができるオプションによって、アプリケーションに弾力性を加えることで達成されます。加えて、このアーキテクチャは、アプリケーションを再設計したり書きなおしたりすることなく、スケールアップ・スケールダウン(より多くの、またはより少ないCPUを持つノードを配置すること)をも容易にします。弾力性は、クラウドコンピューティング環境におけるアプリケーション運用コストを最小限にし、仮想化された使った分だけ課金するモデルを可能にするという利点をもたらします。

_Scalability also helps managing risk: providing too little hardware to keep up with user load leads to dissatisfaction and loss of customers, having too much hardware — and operations personnel — idling for no good reason results in unnecessary expense. A scalable solution also mitigates the risk of ending up with an application that is unable to make use of new hardware becoming available: we will see processors with hundreds, if not thousands of hardware threads within the next decade and utilizing their potential requires the application to be scalable at a very fine-grained level._

スケーラビリティはリスクの管理にも貢献します。そのリスクとは、少なすぎるハードウェアによってユーザー負荷に追いつけなくなり不快や顧客の流出をもたらしてしまうこと、または、大きすぎるハードウェア - および多すぎる運用人員 - によって意味なくアイドルさせてしまい不要な支出を招くことです。スケーラブルなソリューションは、新しいハードウェアで可能になるようなことを活用できないアプリケーションで終わってしまうリスクも低減します。新たな可能性とは、次の10年で数百のプロセッサー、たとえ、数千のハードウェアスレッドでないとしても、そしてその可能性を活用するためには、アプリケーションは実にきめ細かくスケーラブルであることを求めます。

### キーとなる構成要素 Key Building Blocks

_An event-driven system based on asynchronous message-passing provides the foundation for scalability. It enables loose coupling between components and subsystems and thus makes it possible to scale out the system onto multiple nodes while retaining the same programming model with the same semantics. Adding more instances of a component increases the system’s capacity to process events. In terms of implementation there is no difference between scaling up by utilizing multiple cores or scaling out by utilizing more nodes in a datacenter or cluster. The topology of the application becomes a deployment decision which is expressed through configuration and/or adaptive runtime algorithms responding to application usage. This is what we call [location transparency](http://en.wikipedia.org/wiki/Location_transparency)._

非同期なメッセージ授受に基づいたイベント駆動システムはスケーラビリティの基盤を提供します。コンポーネントとサブシステムの間を疎結合にし、同じプログラミングモデルと同じ文脈(セマンティクス)を保持しながら複数のノードにシステムをスケールアウトできるようになります。より多くのコンポーネントのインスタンスを追加することで、システムがイベントを処理する能力が増加します。実装の観点においては、マルチコアの活用によってスケールアップすること、データセンターやクラスタのノードを増やすことによってスケールアウトすることに一切の違いはありません。アプリケーションの接続形態(トポロジー)は、アプリケーションに使用方法に呼応しした設定や(と)適合可能な実行時のアルゴリズムによって表現されるような、デプロイ時の決定事項になります。これを位置透過(ロケーション・トランスパレンシー、[location transparency](http://en.wikipedia.org/wiki/Location_transparency)) と呼びます。

_It is important to understand that the goal is not to try to implement transparent distributed computing, distributed objects or RPC-style communication — this has been tried before and it has failed. Instead we need to *embrace the network* by representing it directly in the programming model through asynchronous message-passing. True scalability naturally involves distributed computing and with that inter-node communication which means traversing the network, that as we know is [inherently unreliable](http://aphyr.com/posts/288-the-network-is-reliable). It is therefore important to make the constraints, trade-offs and failure scenarios of network programming explicit in the programming model instead of hiding them behind leaky abstractions that try to “simplify” things. As a consequence it is equally important to provide programming tools which encapsulate common building blocks for solving the typical problems arising in a distributed environment — like mechanisms for achieving consensus or messaging abstractions which offer higher degrees of reliability._

重要なのは、透過的な分散コンピューティングや、分散オブジェクトまたは RPC スタイルのコミュニケーションを実装しようとすることではありません。- それは、挑戦され失敗され続けてきました。そうではなく、非同期なメッセージ授受を通じてプログラミングモデルに直接的に表現することで *ネットワークを受け入れる* ことが必要なのです。真のスケーラビリティは、本質的に信頼できない([inherently unreliable](http://aphyr.com/posts/288-the-network-is-reliable)) ネットワークを横断(トラバース)するノード間の通信とともに、自然に分散コンピューティングを内包します。したがって、物事を"単純化しよう"とすることで漏れがあるような抽象化で核してしまうのではなく、プログラミングモデルに明示的にネットワークプログラミングの制約、トレードオフ、失敗シナリオを組み込むことが重要です。結果として、分散環境で発生するような典型的な問題を解決する共通の構成要素をカプセル化するプログラミングツールを提供することも、同じように重要になります。それは、高いレベルの信頼性を提供するコンセンサスの達成やメッセージの抽象化のメカニズムのようなものです。

## 回復力のある Resilient

### なぜ重要なのか Why it is Important

_Application downtime is one of the most damaging issues that can occur to a business. The usual implication is that operations simply stop, leaving a hole in the revenue stream. In the long term it can also lead to unhappy customers and a poor reputation, which will hurt the business more severely. It is surprising that application resilience is a requirement that is largely ignored or retrofitted using ad-hoc techniques. This often means that it is addressed at the wrong level of granularity using tools that are too coarse-grained. A common technique uses application server clustering to recover from runtime and server failures. Unfortunately, server failover is extremely costly and also dangerous — potentially leading to cascading failures taking down a whole cluster. The reason is that this is the wrong level of granularity for failure management which should instead be addressed using fine-grained resilience on the component level._

アプリケーションの中断時間は、ビジネスに起こりうつ最も痛手な問題のひとつです。普通に暗示することは、業務がたやすくストップし、収益を生むシステムに穴を開けてしまうことになります。長い目で見た時、不満な顧客と悪い評判を生み出し、ビジネスをもっと深刻に傷つけてしまいます。アプリケーションの回復力が、要件から全く無視される、または、一時しのぎな技術の据え付けで済ましてしまうことは驚くべきことです。このことは、よく粗すぎる粒度のツールを使って、異なったレベルの粒度の問題として位置づけられてしまうことも意味します。一般的な技術として、実行時のサーバの故障から回復するためにサーバクラスタリングがあります。不運なことに、サーバーのフェイルオーバーは著しくコストが掛かり、また同時に危険でもあります。- 失敗の連鎖を生む可能性があり、クラスタ全体をダウンさせてしまいます。これは間違ったレベルの粒度の故障管理であり、代わりにコンポーネントレベルの微細な回復力によって代替されるべきものです。

Merriam-Webster defines resilience as:

Merriam-Webster は、resilience を次のように定義しています。

- *the ability of a substance or object to spring back into shape*
- *the capacity to recover quickly from difficulties*

- *元々の形に息を吹き返す、物質や物体の能力* 
- *困難から迅速に回復する能力*

_In a reactive application resilience is not an afterthought but part of the design from the beginning. Making failure a first class construct in the programming model provides the means to react to and manage it, which leads to applications that are highly tolerant to failure by being able to heal and repair themselves at runtime. Traditional fault handling cannot achieve this because it is defensive in the small and too aggressive in the large — you either handle exceptions right where and when they happen or you initiate a failover of the whole application instance._

リアクティブなアプリケーションにおいて、回復力は後から考えるものではなく、当初の設計の一部です。そのプログラミングモデルにおいて失敗をファーストクラスの構成物としてみなすことは、反射することと管理するという意味を与えます。実行時に回復でき修復できることで失敗に対しての高い耐久力があるアプリケーションへと導きます。伝統的な失敗への対処では、小さな環境では防御的すぎて、大きな環境では挑戦的すぎるため、これを達成することができません。- いつどこで起ころうと、例外を(ひとつひとつ)
正しく対処するか、アプリケーションインスタンス全体にフェールオーバーを備えるか(の両極端)になってしまいます。

### キーとなる構成要素 Key Building Blocks

_In order to *manage failure* we need a way to *isolate* it so it doesn’t spread to other healthy components, and to *observe* it so it can be managed from a safe point outside of the failed context. One pattern that comes to mind is the [bulkhead pattern](http://skife.org/architecture/fault-tolerance/2009/12/31/bulkheads.html), illustrated by the picture, in which a system is built up from safe compartments so that if one of them fails the other ones are not affected. This prevents the classic problem of [cascading failures](http://en.wikipedia.org/wiki/Cascading_failure) and allows the management of problems in isolation._

*失敗を管理* するためには、他の健全なコンポーネントに波及しないよう *孤立させる* (isolate) 方法と、失敗のコンテキストの外側にある安全なポイントから管理されるように *観察する* (observe) 方法とが必要です。思いつくひとつのパターンは、(船の)隔壁パターン([bulkhead pattern](http://skife.org/architecture/fault-tolerance/2009/12/31/bulkheads.html)) です。図にあるように、ひとつが壊れても他が影響を受けないように、安全な区画によってシステムを築くことです。これにより、古典的な問題である連鎖的な失敗 ([cascading failures](http://en.wikipedia.org/wiki/Cascading_failure)) を防ぐことができ、問題を孤立した領域で管理することができます。

![fig 3 Bulkheads](../images/tank.png)

_The event-driven model, which enables scalability, also has the necessary primitives to realize this model of failure management. The loose coupling in an event-driven model provides fully isolated components in which failures can be captured together with their context, encapsulated as messages, and sent off to other components that can inspect the error and decide how to respond._

スケーラビリティを可能にするイベント駆動モデルは、この失敗管理のモデルを実現する必須の(原始性？土台？)を持ち合わせています。イベント駆動モデルにおける疎結合性は、それらのコンテキストの中で一緒に捕える事が可能な失敗を完全に孤立化させるコンポーネントを提供し、メッセージとしてカプセル化し、他のコンポーネントに投げてしまい、sの中でエラーを分析し、どのように応答するかを決定することができます。

_This approach creates a system where business logic remains clean, separated from the handling of the unexpected, where failure is modeled explicitly in order to be compartmentalized, observed, managed and configured in a declarative way, and where the system can heal itself and recover automatically. It works best if the compartments are structured in a hierarchical fashion, much like a large corporation where a problem is escalated upwards until a level is reached which has the power to deal with it._

このアプローチは、ビジネスロジックをクリーンに保ち予期せぬ場所の対処から分離するシステム、失敗は、区画化し観察し管理し宣言的な方法で設定するために明示的にモデル化されたシステム、それ自身を治癒し自動的に復活するシステムを作り出します。もし、区画が階層的なやり方で構造化され、処理する力を持つレベルまで到達するまで、問題が上にエスカレートとする大きな企業のように素晴らしく動作します。

_The beauty of this model is that it is purely event-driven, based upon reactive components and asynchronous events and therefore *location transparent*. In practice this means that it works in a distributed environment with the same semantics as in a local context._

このモデルの美しさは、純粋にイベント駆動であり、リアクティブなコンポーネントと非同期なイベントにもとづいており、結果的に *位置透過* になります。実際のところ、ローカルのコンテキストであるのと同じ動作方法で分散環境で動作することを意味します。

## インタラクティブ Interactive

### なぜ重要なのか Why it is Important

Interactive applications are real-time, engaging, rich and collaborative. Businesses create an open and ongoing dialog with their customers by welcoming them through interactive experiences. This makes them more efficient, creates a feel of being connected and equipped to solve problems and accomplish tasks. One example is Google Docs which enables users to edit documents collaboratively, in real-time — allowing them to see each other’s edits and comments live, as they are made.

インタラクティブなアプリケーションは、リアルタイムであり、魅力があり、リッチでコラボレーションに富んでいます。ビジネスは、インタラクティブな体験を通して顧客を歓迎し、オープンで継続的な対話を作ります。これにより、ビジネスはより効率的になり、問題を解決しタスクを完遂するために繋がり続け備え付けられた感じを提供します。ひとつの例ですが、Google Docs は、ドキュメントをリアルタイムに協力して編集することを可能にます。- それらが作られたかのように(？)、お互いの編集とコメントを見ながらです。
 
Users are empowered when they can interact with data that is transformed into meaningful information in real-time. Interactive applications make collaboration on this information inherent in every interface so people communicate more effectively and more often; this is deepened further by increasing the feedback granularity from traditional whole-page behavior down to a per-item level, e.g. in a single-page email web-client. Instantaneous social interactions over large distances are radically transforming how people engage with information and with each other. For instance, GitHub is revolutionizing developer collaboration through “Social Coding” with an interactive browser-based application and of course Twitter has profoundly changed the way news are spread.

リアルタイムでデータが意味ある情報に変換されて相互作用する時、ユーザーは力付けられます(？)。インタラクティブなアプリケーションは、どのインタフェースにおいても引き継いでいくこの情報を作り出します。伝統的なページ全体の扱いからアイテムレベルにフィードバックを小さく増やしていくことでもっと深くに行きます。 例としてEメールのシングル・ページのウェブクライアントがあります。遠い距離を超えた瞬時のソーシャルなインタラクションは、人々がどのように情報と向き合い、お互いにやりとりしていくかを根本的に変えていきます。たとえば、GitHub は、インタラクティブなブラウザベースのアプリケーションにより、"Social Coding" を通じて開発者のコラボレーションに革命を起こしつつあります。もちろん、Twitter は、ニュースが拡散する方法を根本的に変えてきました。

Build upon an event-driven foundation, reactive applications are well equipped to be interactive. Scalability is necessary to retain this property when the application becomes popular, and resilience ensures that its users will continually enjoy its function.

イベント駆動の基礎の上に構築することで、リアクティブなアプリケーションは、インタラクティブ性も同時にうまく備えています。スケーラビリティは、アプリケーションに人気が出てきた時、この属性(？)を保つことは必須です。回復力はユーザーが継続的にその機能を楽しみ続けることを確かにします(？)。

### キーとなる構成要素 Key Building Blocks

Reactive applications use observable models, event streams and stateful clients.

リアクティブなアプリケーションは、観察可能なモデルとイベントストリーム、ステートフルなクライアントです。	

Observable models enable other components to receive events when state changes. This can provide a real-time connection between users and systems. For example, when multiple users work concurrently on the same dataset, changes can be reactively synchronized bi-directionally between them.

観察可能なモデルは、他のコンポーネントが状態が変化した時にイベントを受け取ることを可能にします。これは、ユーザーとシステムの間にリアルタイムの接続を提供します。たとえば、複数のユーザーが同じデータセットに同時並行的に作用するとき、変更は、リアクティブにそれらの間で双方向に同期することができます。

Event streams form the basic abstraction on which this connection is built. Keeping them reactive means avoiding blocking and instead allowing asynchronous and non-blocking transformations. For example a stream of real-time data could be passed through a live projection that produces a new stream of analyzed data.

イベント・ストリームは、この接続が構築されたところに基本的な抽象性を形作る(？) それらをリアクティブに保つことはブロッキングを避ける事を意味し、代わりに非同期でノンブロッキングな変換を可能にします。たとえば、リアルタイムデータのストリームは、解析されたデータの新たなストリームを生み出す生の投影(？)を通じて受け渡されます。

Most reactive applications have rich web and mobile clients which create an engaging user experience. These applications execute logic and store state on the client-side in which observable models provide a mechanism to update user interfaces in real-time when data changes. Technologies like WebSockets or Server-Sent Events enable user interfaces to be connected directly with event streams so the event-driven system extends all the way from the back-end to the client. This allows reactive applications to push events to browser and mobile applications in a scalable and resilient way by using asynchronous and non-blocking data transfer.

もっともリアクティブなアプリケーションは、魅力あるユーザー体験を創りだすリッチなウェブとモバイルクライアントです。これらのアプリケーションは、観察可能なモデルがデータが変更された時にリアルタイムでユーザーインタフェースを更新する機甲を提供するクライアントサイドで、ロジックを実行し状態を保持する(？)。Web Socket や サーバプッシュ型イベントのような技術は、イベントストリームに直接的に接続されるユーザーインタフェースを可能にし、イベント駆動システムは、バックエンドからクライアントまで全ての道筋に広げることができます。これにより、リアクティブなアプリケーションは、非同期やノンブロッキングなデータ移送によってスケーラブルで回復力のある方法でイベントをブラウザやモバイルアプリケーションにプッシュすることが可能になります。

With this in mind it becomes apparent how the four qualities *event-driven*, *scalable*, *resilient* and *interactive* are interconnected to form a cohesive whole:

この点を念頭に置くと、四つの本質、*イベント駆動* と *スケーラブル*、*回復力がある*、*インタラクティブ* は凝集性のある全体(？)から相互接続されることが明らかになってきます。

![fig 4. The Reactive Traits](../images/full-reactive.png)

## 結論 Conclusion

Reactive applications represent a balanced approach to addressing a wide range of contemporary challenges in software development. Building on an *event-driven*, message-based foundation, they provide the tools needed to ensure *scalability* and *resilience*. On top of this they support rich, real-time user *interactions*. We expect that a rapidly increasing number of systems will follow this blueprint in the years ahead.

リアクティブなアプリケーションは、ソフトウェア開発における幅広い現代の困難に向けてのバランスのとれたアプローチを象徴するものです。*イベント駆動* のメッセージを基礎とした基盤の上に構築することで、*スケーラビリティ* と *回復力* を確かにするの必要なツールを提供します。この基板の上に、それらはリッチでリアルタイムなユーザーとの *インタラクション* を支援します。私たちは、急速に増加するシステムがこの見取り図に従っていき何年も先に進んでいくことを期待しています。

[マニフェストにサインしよう(本家へ)](http://www.reactivemanifesto.org/)

## 訳メモ

* event-driven は、すべて「イベント駆動」にしました。イベントドリブンでもよかったのかもしれません。
* リアクティブは、敢えてカタカナのままにしました。もし、訳すとしたら反応的ないしは反射的でしょうか。反応的アプリケーション、反応的な設計 ・・・。
* デプロイと配置は微妙に雰囲気で使い分けました。
* ノンブロッキングには適切な日本語はなく、また、普及した用語であるとみなしました。
* レジリエントは、カタカナのままで使うことも考えましたが、それほど一般的ではないため”回復力の/がある”としました。

デカップリング