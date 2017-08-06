UPnP Device Architecture 2.0
===================
原文: [http://upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v2.0.pdf](http://upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v2.0.pdf)  
本翻訳文は私の私的な目的で記載しております。本翻訳文を利用したことによる一切の責任は利用者にあります。 
本翻訳文はコピーレフトですが、原文はUPnP Forumが著作権を持ちますので、流布に関しては原文の定める規定に従ってください。
  
Introduction
===================

***What is UPnP Technology?***
  
UPnPはPC端末や無線端末、その他IT製品に対して様々なP2P接続性を確保する技術です。
  
UPnPはアドホックネットワークや管理されないネットワーク
（例えば、家庭、SOHO、あるいはインターネットに接続された公衆LAN）
に対して簡易で便利で標準的な通信を提供します。
  
UPnPはコントロール通信・データ通信にてTCP/IPやWeb技術を使った
デバイス間の互換性の高い近接通信により分散オープンなネットワーキングを可能にします。
  
UPnP Device Architecture (UDA) はプラグ&プレイの最もシンプルなモデルです。
  
UDAは見えないネットワーキングと
ベンダによる多岐に渡るデバイスカテゴリのデバイス探索機能により
”Zero Configuration”のサポートを目指します。
  
UDAではデバイスが動的にネットワークにjoin、
自動でIPアドレスを取得、デバイス機能の伝達を行い
さらに他のデバイスの探索・機能探索を行うことができます。
よって、各デバイスはそのデバイスの状態に関わらず
いつでもネットワークから離脱することができるようになります。
  
UPnPは一般的なインターネットプロトコルを使います。
(例:IP,TCP,UDP,HTTP,XML)  
  
一般的なインターネット同様HTTPにより通信し拡張XMLによりデータ送受信します。
  
UDAでインターネットプロトコルを使うことは
既存の家庭用・SOHO用ネットワークとの親和性があり
現代のマルチベンダ環境における機種間相互接続性が高いため推奨されます。
  
UPnPのアーキテクチャはそのような環境をカバーできるように設計されました。
  
さらにはブリッジングを使用することにより経済的にor技術的にor古くから
TCP/IPスタックを使って居ないデバイスもカバーできます。
  
UPnPにおけるユニバーサルとは”デバイスドライバレス”という意味であり
デバイスドライバの代わりに一般的なプロトコルを使用します。
  
UPnPネットワーキングはデバイスの仕様とは独立した実装です。
UPnPデバイスはどのOSのどんなプログラミング言語によっても実装が可能です。
  
**UPnPのアーキテクチャはアプリケーションのAPIデザインに関して指定や制約をしません。**
OSベンダはエンドユーザの要望にあった形でAPIを作成すべきです。
  
***UPnP Forum***
  
UPnP Forumは異なるベンダのデバイスやPCとの接続性の維持に努める組織です。
UPnP Forumは機種間相互接続性の推進のためXMLベースのデバイスのスキーマ情報を標準化します。
  
UPnP Forumはデバイス間の相互接続性を一様に検証しそれを認証することを推進するため
複数の産業にまたがる企業によって構成されています。
  
UPnP ForumはUPnP Logo Programによりデバイス検証し認証するプロセスを司り
UPnP対応の保証をその他のUPnPデバイスのベンダに周知します。
  
UPnPデバイス認証のプロセスはベンダの開発者にも、会費を支払っているメンバーにもオープンであり
認証されたデバイスがUPnP機能に対応しているという情報についてもオープンです。
詳しくは[http://www.upnp.org](http://www.upnp.org.)を参照してください。
  
UPnP Forumは各方面の専門分野にワーキングコミッティをおいています。
ワーキングコミッティではデバイス標準を作成したりサンプル実装を作成したり
適切なテストスイートを作成したりします。
  
このドキュメントではそれらのワーキングコミッティ分野における技術的な見地について示します。
UPnPベンダはUPnP Logo Programによってその技術的な適合性を発信することにより
相互接続性に対する信頼を得ることができます。
**このLogo Programに関わらず、ベンダはUDAに沿って
形式張った標準的な認証申請なしにデバイスを開発することも可能です。**
もしベンダが非公式でUPnPデバイスを開発した場合はワーキングコミッティのやり方とは
違った方式で技術的な実装をしていることになります。
    
***In this document***
  
UPnPデバイスアーキテクチャ（aka. DCPフレームワーク） はControllerまたは
ControlPointまたはDeviceによってお互いに通信されます。
Discovery,Description,Control,Eventing,Presentationのため
UDAは以下の通りのプロトコルスタックを使用しています。
  
<TODO: Fig.1の画像をはる。でも、常識。>
  
最も上位のレイヤUPnP Vendorはデバイスのベンダ依存情報のみ含まれています。
その下位レイヤUPnP Forumはベンダ情報を補完するための情報が含まれます。
これらの情報はSSDP,GENA,マルチキャストイベントプロトコル,その他文書化された
UPnPのプロトコルによって伝送されます。
  
SSDPはユニキャストまたはマルチキャストUDPによって伝送されます。
マルチキャストイベントはマルチキャストUDPによって伝送されます。
GENAはHTTPによって伝送されます。
結局のところ、これらのメッセージはすべてIPプロトコルの上に成り立ちます。
残すところ、このドキュメントが示すのはこれらのプロトコルがどういうフォーマットで
どういうメッセージで利用されているか、ということです。
  
UPnPアーキテクチャには二つの大きな分類があります。
それはControlled Devices(＝デバイス)とControl Pointsです。
Controlled Deviceはサーバとしての役割を果たし
Control Pointsからのリクエストに対してレスポンスを返します。
  
Controlled DevicesとControl Pointsは様々なプラットフォーム
（パソコン、組み込み端末）にて実装することができ
同じネットワークエンドポイントにおいて同時並行的に動作することが可能です。
※このドキュメントはIPv4の世界に限定しています。IPv6は別途考慮が必要です。  
  
UPnPの基本はIPアドレッシングです。IPv4環境においてはControlled Devices,Control Pointsは
DHCPクライアント機能を持っており、ネットワークへの初回接続時にはDHCPサーバを探索します。
  
もしDHCPサーバが提供されている（＝管理されたネットワーク）場合は
Control PointはDHCPにより払い出されたIPアドレスを使うことになります。
もしDHCPサーバが提供されていいな（＝管理されていないネットワーク）場合は
Control PointはAuto IPを使うことになります。
  
Auto IPは予約アドレスの中からControlled DevicesControl Pointsが
アドレスを選択し、DHCPがある環境・ない環境どちらの場合もシームレスな動作をするように
実装が定義されています。
  
もしDHCPトランザクションの中でデバイスがDomainNameを取得した
場合、デバイスは以降のUPnPネットワーキングにおいてドメイン名を使用します。
DomainNameが取得されない場合はAuto IPまたはDHCPで配布されたIPを使用します。
  
通常UPnPネットワーキングは物理的・論理的に重複しないようなアドレッシングスキームを
するため、複雑な設計がされています。
  
UPnPデバイスは複数のネットワークインターフェイスを持ち得ます。
また、一つのインターフェースにつき2個以上のIPアドレスを持ち得ます。
そのような構成の場合は同じUPnPネットワークに一つのUPnPデバイスが複数のIPアドレスを
持っている状態となり、ネットワーク上に複数回Control Pointとして表示される可能性があります。
そのような状態の場合それらのデバイスはマルチホーム(複数NWへ所属すること)となります。
本ドキュメントにおいてUPnP-enabled interfaceとはUPnP networkに所属する
IPアドレスがアサインされたインターフェースのことをさします。
マルチホームなUPnPデバイスについては本ドキュメントにおいてはカバーされます。
しかし、一般原則として、UPnPの通信はincomingのインターフェースが
イコールoutgoingのインターフェースとなります。
  
今、UPnPデバイスに対してIPアドレスが付与されたと仮定します。
  
UPnPネットワーキングのStep1はDiscoveryです。
  
デバイスがネットワークに追加された際、UPnP Discoveryプロトコルはデバイスに対して
Control Pointを広告することをします。同様に、Control Pointがネットワークに
追加された場合はUPnP DiscoveryプロトコルはControl Pointにネットワークの探索をさせます。
どちらの場合も情報交換内容は通信に不可欠な情報と少々のデバイス機能についてのみです。
(e.g. Device Type ID, より深いデバイス情報へのポインタ情報)
後述のDiscoveryセクションではいかにしてデバイスが広告をするのか、
いかにしてControl Pointは探索をするのか、DiscoveryMessageのフォーマットの詳細について示します。
  
UPnPネットワーキングのStep2はDescriptionです。
  
Control Pointがデバイスを探索し発見した時点では、Control Pointはデバイスに関して
非常に小さな情報しか持ち得ません。Control Pointがデバイスの各種機能に関して
詳細な情報を入手するorデバイスと通信を開始するにあたり、Control Pointはデバイスからの
Discovery Messageに含まれて居たURLからデバイスのDescriptionを入手しようとします。
デバイスの中には論理的な複数のデバイスが存在するかもしれません。
それと同等の複数の機能ユニットやService単位を持っているかもしれません。
デバイスのUPnP DescriptionはXMLの中に記述されており、そこには
型番・シリアル番号・製造者名やベンダのホームページのURLなどのベンダ固有の情報が含まれます。
UPnP Descriptionにはそのデバイスに埋め込まれた論理デバイスやServiceのリスト情報が含まれます。
（大抵の場合これはControl,Eventing,Presentation用のURLである）
Serviceそれぞれに対して、DescriptionはServiceを返すための
コマンドやアクションと引数・パラメタのリストが含まれています。
Descriptionの中にはServiceのステート情報などの変数のリストも含まれます。
そこにはデータタイプやデータレンジ、イベントシグナルがService固有の表現でなされています。
後述のDiscoveryセクションではいかにしてデバイスがDescriptionの表現をするのかと
Control PointはいかにしてそれらのDescription情報を取得するのかということを述べます。
  
UPnPネットワーキングのStep3はDescriptionです。
  
Control PointがデバイスのDescriptionから取得された後
Control PointはデバイスのServiceに対してアクションを送ることができます。
そのためにはデバイスは適切なコントロールメッセージをデバイスのDescriptionに記載の
コントロールURLに送る必要があります。コントロールメッセージはSOAPを使用してXMLで送受信されます。
ファンクションコールのようにコントロールメッセージのレスポンスにアクションに対応した
返り値をServiceが返します。
アクションの対応によってはServiceの実行状態を示す変数は変動します。
後述のControlセクションではコントロールメッセージのフォーマット、状態変数(state)、
アクションの説明について述べます。 
  
UPnPネットワーキングのStep4はEventingです。
  
UPnP Descriptionは実行時のService状態(state)を示す変数のリストと
Serviceが応答するアクションのリストを表示します。
Serviceはこの状態に関するアップデートをしControlPointは更新情報を自動で受信します。
ServiceはEvent Messageを送信する子でアップデート情報を配布します。
Event Messageは一つ以上のEvent変数の名前を保持しており、それらの現在の値を含んでいます。
これらのメッセージはXMLによって伝送されます。
特にControlPointが最初に情報を取得する際には特殊なEvent Messageが配布されます。
このメッセージには全てのEvent名と値を含みService状態モデルの初期化をさせます。
複数のControl Pointをサポートするために、いかなるアクションに基づく影響について
全てのControl Pointで統一した状態をキープするようにEventingは設計されて居ます。
それゆえ、全てのEvent受信者はEvent変数についてのアップデートに関してもれなくEvent Messageを受信します。
（それがたとえリクエストしたアクションのレスポンスだろうが、Serviceモデルが変わろうが。）
マルチキャストEventingはEvent Messageを代表するものです。
マルチキャストEventingを利用することでControlPointはServiceのアップデートを
明示的なセッションの確立なしに受信することができるようになる。
この方式のEventingはUPnP通信に限らず
複数のControlPointに対して複数のユーザが存在する場合に
不特定のユーザに対してControlPointを通知をする際に便利であり、
そのあとで複数のデバイスはそれぞれ複数のControlPointを各々取得することができます。
マルチキャストEventingはUDPをトランスポート層に使っているため
根本的に信頼度は低いです。疎通確率を上げるためにマルチキャストEventing通信の
再送制御オプションが後述されています。
UPnP Working CommitteeはEventがマルチキャストEventかどうかを定義すべきです。
後述のEventingセクションではユニキャストとマルチキャストの両方のEvent Messageの
フォーマットについて述べます。
  
UPnPネットワーキングのStep5はPresentationです。
  
もしデバイスがPresentaion用のURLを持って居た場合、そのURLをブラウザで開くことで
ControlPointはページの内容を取得できます。
そのページでは場合によってはユーザはデバイスをコントロールしたりデバイスのステータス情報を参照することができます。
参照できる情報や操作できる情報の度合いについてはそのデバイスとデバイスの提示するページの内容に依存します。
後述のPresentationセクションではPresentationページから情報を入手するプロトコルについて述べます。
  
***Audience***
  
本文書の読者はUPnPデバイスやControlPointベンダ、UPnP Forum、UPnPプロトコルの技術情報を知る必要がある人です。
本文書はHTTP/TCP/UDP/IPファミリープロトコルに対して一定の知見があることを想定しており
本文書はそれらの技術に関してあえて説明をしません。
また、本文書の大多数の読者はXMLに対して見識がないことを想定していますが
本文書はXMLチュートリアルではないので、XMLに関する問題についてはUDAの中核にて解決することとします。
本文書はプログラミング・スクリプティングに関して見識を持った読者を想定していません。
  
***Conformance Terminology***
  
本文書では、実装基準を以下のように定義します。
  
Required (or shall or mandatory).
  
UDAに必要な基本的な実装です。以後、shall notやPROHIBITEDは禁止を意味します。
  
Recommended (or should).
  
UDAですべき実装です。概ね低コストで実装可能な機能的な実装です。
もしRecommendedな機能を実装した場合はガイドラインに従った実装をしているかどうか
認可するテストをその分実施する必要をすべきです。
Recommended機能の一部は将来的にRequiredな機能になり得ます。
以降、should notという表現は「実装に制限はないが非推奨を意味します。
  
Allowed
  
UDAで実装する必要も推奨されてもない実装です。
しかし、もし実装をした場合はガイドラインに準拠していることを保証すべきです。
Allowed機能の一部は将来的にRequiredな機能になることはありません。
  
DEPRECATED
  
UDAにおいて仕様上記載のあった機能であっても今後は後方互換性のためだけの実装です。
この実装をしたところで現在・今後の実装に対してエラーなど何かしらの影響はありません。
仕様上の問題で後方互換性が必要になるケースがありますが、その仕様も今後実装すべきではありません。
  
**略語**
 
<TODO: Table1 - Acronymsの画像を貼る。でもだいたいわかるし・・・>
  
**用語**
  
- action
  
Serviceによって公開されたコマンドのことです。一つ以上の引数・帰り値があります。
詳細は"Description"と"Control"の章を参照してください。
  
- argument
  
actionのパラメタです。
詳細は"Description"と"Control"の章を参照してください。

- control point
  
control pointはデバイス・ServiceのDescriptionを取得しactionをServiceに対して実施し
Serviceのstateをpollingし、ServiceからEventを受信します。
  
- device
  
物理デバイス、コンテナです。論理・物理問わず、単数・複数問いません。
デバイスはネットワークに対して自身の存在を広告します。
詳細は"Discovery"と"Description"の章を参照してください。
  
- device description
  
UPnP Template Languageで表現された論理デバイスの詳細な定義情報です。XMLで表現されます。
UPNP Device Templateに基づきUPnPベンダによって記述されます。
情報としては製造者識別子、品番、シリアル番号、URL (Control URL, Eventing URL, Presentation URL)が含まれます。
詳細は"Description"の章を参照してください。

- device type
  
UPnP Forum working Committeeによりアサインされた標準のデバイス種別識別子が
urn:schemas-upnp-org:device: で示されます。UPnP Device Templateと一対一対応です。
UPnPベンダはdevice typeを拡張することができます。
それらの拡張device typeはurn:_domain-name_:device: に続くベンダが決定した
識別子にて示されます。_domain-name_の部分はベンダのドメイン名になります。
詳細は"Description"の章を参照してください。
  
- event
  
Serviceによって公開されたstateが一つ上の変化をした際の通知です。
詳細は"Eventing"の章を参照してください。
  
- GENA
  
General Event Notification Architectureの略です。Eventの受信と通知に関するプロトコルセットです。
詳細は"Eventing"の章を参照してください。
  
- publisher
  
Eventの発信者です。大抵の場合はdevice serviceです。
詳細は"Eventing"の章を参照してください。
  
- root device
  
内包されていない論理デバイスを指します。
詳細は"Description"の章を参照してください。
  
- service
  
論理機能ユニットです。Controlの最小単位です。
state変数を使って物理デバイスの状態やactionを提供します。
  
- service description
  
論理的なServiceの定義です。UPnP Template LanguageでXMLによって記述されます。
UPnP Service Templateに基づいて、UPnPベンダによって情報は埋められます。
  
- SOAP
  
Simple Object Access Protocolの略です。
XMLで記述したコマンドセットをHTTPにより伝送することで実施する
リモートプロシージャコール（RPC）の仕組みの一つです。
詳細は"Control"の章を参照してください。
  
- SSDP
  
Simple Service Discovery Protocolの略です。
マルチキャストによるディスカバリ・サーチメカニズムをHTTP over UDPによって可能とします。
詳細は"Discovery"の章を参照してください。
  
- state variable
  
物理デバイスの看板です。Serviceによって公開されます。
name, data type, default value(optional), constaints value(optional),
その変数が変わった時に発するEvent Trigger(optional)を示します。
詳細は"Description"と"Discovery"の章を参照してください。
  
- subscriber
  
Event Messaveの受信者です。通常はControlPointです。
詳細は"Eventing"の章を参照してください。
  
- UPnP Device Template
  
device type, 内臓デバイス, 必要なServiceを定義したテンプレートです。
UPnP Forum Workiing Commmitteeの策定したUPnP Device Schemaに基づいたXMLにて表記されます。
Standard Device Typeと一対一関係にあります。
詳細は"Description"の章を参照してください。
  
- UPnP Service Template
  
Action名とActionのパラメタ、state変数、それらの変数のプロパティを定義したテンプレートです。
UPnP Forum Workiing Commmitteeの策定したUPnP Service Schemaに基づいたXMLにて表記されます。
Standard Service Typeと一対一関係にあります。
詳細は"Description"の章を参照してください。
  
- UPnP Device Schema
  
UPnP Device/Service Templateの要素を定義します。
UPnP Device Architectureの策定したXML Schemaに基づいたXMLにて表記されます。
詳細は"Description"の章を参照してください。
  
- Vendor Domain Name
  
ベンダ固有のドメイン名です。ベンダによって保持され
ICANNによって公式認可された一意な登録名でなければなりません。
ベンダはドメイン名を常に最新の状態に保持する必要があります。
ドメイン名の長さはUDAによって定められたドメイン名の条件にマッチするように調整をすべきです。
  
- その他リファレンス
  
RFC 2710, Multicast Listener Discovery for IPv6. 
Available at: [http://www.ietf.org/rfc/rfc2710.txt](http://www.ietf.org/rfc/rfc2710.txt)  
RFC 2616, HTTP: Hypertext Transfer Protocol 1.1.
Available at: [http://www.ietf.org/rfc/rfc2616.txt](http://www.ietf.org/rfc/rfc2616.txt)  
RFC 2279, UTF-8, a transformation format of ISO 10646 (character encoding).
Available at: [http://www.ietf.org/rfc/rfc2279.txt](http://www.ietf.org/rfc/rfc2279.txt)  
XML, Extensible Markup Language. W3C recommendation.
Available at: [http://www.w3.org/XML/](http://www.w3.org/XML/)  
DEVICEPROTECTION, UPnP Device Protection specification.
Available at [http://upnp.org/specs/gw/UPnP-gw-DeviceProtection-v1-Service.pdf](http://upnp.org/specs/gw/UPnP-gw-DeviceProtection-v1-Service.pdf)  
  
***0 Addressing***
  
AddressingはUPnPネットワーキングのStep0です。AddressingによりデバイスとControlPointは
ネットワークアドレスを取得します。AddressingはStep1のDiscoveryを可能にし
COntrol Pointがデバイスを探索することができます。Step2のDescriptionで
デバイスの機能をControl Pointが学習し、Step3でControl Pointはデバイスに対して
Commandを発行し、Step4でControl Pointはデバイスの状態変化をEventingによってリッスンし
Step5のPresentationでControl PointはデバイスのUIを表示することができます。
  
UPnPネットワーキングの根幹はIPアドレッシングです。
UPnPデバイスまたはControl PointはIPv4シングルスタックかIPv4/v6のデュアルスタックを
サポートすることができます。本文書（以降の項1〜5）ではIPv4実装を想定します。
本文書の付録にてIPv6の実装を補足します。
  
UPnPデバイスまたはControlPointは初回ネットワーク接続時は
自身はDHCPサーバとしては動かずDHCPクライアントとしてDHCPサーバを探索すべきです。
（もしDHCPサーバとして実装されている場合は自身の管理下のネットワークアドレスを
　自身のアドレスプールの範囲内で取得することができます。）
  
もしネットワークでDHCPサーバが稼働している場合はDHCPサーバからアサインされた
IPアドレスをデバイスは使用するべきです。
もしネットワークでDHCPサーバが稼働していない場合はAuto-IPアルゴリズムによって
取得したIPアドレスをデバイスは使用するべきです。
  
Auto-IP (RFC3927) はDHCPサーバが利用できない場合にリンクローカルIPアドレスの
範囲の中から決められた手順でIPアドレスを取得する手段です。
この技術によりデバイスがDHCP・非DHCPのネットワークを自由に往来することが可能となります。
本章ではAuto-IPの基本的な挙動について述べます。本章で以下に述べる点に関しては
詳細な情報がリファレンスドキュメントの方にも既に記載があります。
基本的にはリファレンスドキュメントの内容が優先されます。
  
***0.1 Determing Whether to user Auto-IP***
  
Auto-IPをサポートしアドレスを自動取得する設定になっているデバイスは
まずDHCPDISCOVERメッセージによりIPアドレスをリクエストの準備をします。
この時一定時間デバイスはDHCPOFFERを待ちますが、**その時間は実装依存で不定です。**
もしDHCPOFFERがその時間内に受け取れた場合はデバイスはそのままアドレス取得のプロセスに
移行するべきです。もし有効なDHCPOFFERが受け取れなかった場合は
デバイスはAuto-IPによって自動でIPアドレスを設定するべきです。
  
***0.2 Choosing an address***
  
Auto-IPでIPアドレスを決定するために、デバイスは**実装依存のアルゴリズム**で
169.254.0.0/16の範囲で使用します。169.254.0.xxxと159.254.255.xxxのアドレスは
予約アドレスのため、使用すべきではありません。
  
選択したアドレスは既に利用されているかどうかテストをすべきです。
もしアドレスが別のデバイスと重複してい他場合は別のアドレスを選択しテストをすることを**実装依存の回数**だけ試すべきです。
アドレス選択はアドレスを確保しようとする複数のデバイスと衝突を避けるようにランダマイズされるべきです。
全く同じタイミングでデバイスがアドレス重複を検知し連続的に取得しようとし
同じアルゴリズムを使ってお互いに衝突を検知し連続的にアドレス衝突を検知し続ける事態を避けるため
(169.254.1.0 - 169.254.254.255の範囲でアドレスを選択する)pseudo-algorismを使用すべきです。
このpseudo-algorismはデバイスのイーサネットのMACアドレスをseedにすべきです。
  
***0.3 Testing the address***

デバイスが選択したアドレスが既に使われているかどうか確認するためのテストのためARP Probeを使うべきです。
ARP ProbeとはARPリクエストにてデバイスの自身のMACアドレスを"SenderMAC"とし0.0.0.0を"SenderIP"として設定したものです。
デバイスは"同じIPアドレスを訴求するARP Probe"または"ARP Probeに対する応答"がないかARPをリッスンします。
もしそのようなARPパケットが見られた場合はデバイスは別のアドレスを使用すべきです。
ARP Probeはアドレスの重複を確実に避けるため、何度も行うことが許されています。
  
ARP Probeは2秒間隔に4回送ることが推奨されています。
  
無事リンクローカルアドレスを取得した場合
デバイスはgratuitous ARP(GARP)を2秒間隔で2回送信すべきです。
今回はARPの送信元IPに自身のIPアドレスを設定します。
GARPの目的は他のホストが同じIPを使った古いARPキャッシュを持たないようにするためです。
  
デバイスは保存可能なストレージを内蔵し一度使用した使ったIPアドレスを記憶し
次回ブート時の候補IPとして使うことが許されます。
これはIPアドレスの安定とアドレス衝突の可能性を減らします。
  
アドレス衝突検知の仕組みはアドレス取得フェイズだけでなく
ARP ProbeとARP Listeningのタイミングでも実施することが許されます。

アドレス衝突検知についてはデバイスがリンクローカルアドレスを使用する限り継続するプロセスとなります。
いつ何時でも、デバイスが自分自身の持つIPアドレスが送信元IPになっておりなおかつ自身の持つMACアドレスと
異なる送信元MACアドレスとなっているARPパケットを受信した場合、デバイスはアドレス衝突検知をしたと認識し
以下のa)またはb)のどちらかの方法で反応すべきです。
  
a) 新しいリンクローカルIPアドレスをすぐさま取得する  
b) デバイスがActiveなTCPコネクションを保持していたり、その他の要因で今のIPを使用し続けたいというケースで
　 その他の衝突ARPパケットを直近(e.g. 過去10秒間)で検知していない場合は
　 衝突ARPパケットを受信した時間間隔を計算し自分自身のIPとMACを送信元としたGARPをブロードキャストし
　 一度だけ自身のIPアドレスを守る試行をすることができます。
　 しかし、もし衝突ARPパケットが直近（e.g. 10秒間）で繰り返し受信された場合はデバイスはすぐさま
　 新しいIPアドレスを取得する必要があります。
  
デバイスはa)及びb)に記載の通り、衝突ARPパケットに応答すべきであり、衝突ARPパケットを無視すべきでありません。
デバイスはもし新しくIPアドレスを取得した場合は以前使用していたIPアドレスの広告をやめ、新しいIPアドレスを広告すべきです。
  
デバイスがAuto-IPでIPアドレスを正常に設定できたあとは
Auto-IPの範囲の送信元IPを持つ後続する全てのARPパケット（リクエスト・レスポンス）については
タイムリーなアドレス衝突検知のためリンクユニキャストではなくリンクブロードキャストで実施するべきです。
  
-





