UPnP Device Architecture 2.0
===================
原文: [http://upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v2.0.pdf](http://upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v2.0.pdf)  
  
本文は私的な目的で記載しデータのバックアップ用途で公開しております。
本文を利用したことによる一切の責任は利用者にあります。原文はUPnP Forumが著作権を持ちます。
原文にはdeviceやcontrol pointと言った、クライアント・サーバ的な意味合いのワードが散見されますが
その違いを日本語にする過程で明確にするとややこしいので両方”デバイス”で統一しています。
ところどころ要約をしている箇所もありますので、曲解については委細承知の方
  
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
  
- RFC 2710, Multicast Listener Discovery for IPv6. Available at: [http://www.ietf.org/rfc/rfc2710.txt](http://www.ietf.org/rfc/rfc2710.txt)  
- RFC 2616, HTTP: Hypertext Transfer Protocol 1.1. Available at: [http://www.ietf.org/rfc/rfc2616.txt](http://www.ietf.org/rfc/rfc2616.txt)  
- RFC 2279, UTF-8, a transformation format of ISO 10646 (character encoding). Available at: [http://www.ietf.org/rfc/rfc2279.txt](http://www.ietf.org/rfc/rfc2279.txt)  
- XML, Extensible Markup Language. W3C recommendation. Available at: [http://www.w3.org/XML/](http://www.w3.org/XML/)  
- DEVICEPROTECTION, UPnP Device Protection specification. Available at [http://upnp.org/specs/gw/UPnP-gw-DeviceProtection-v1-Service.pdf](http://upnp.org/specs/gw/UPnP-gw-DeviceProtection-v1-Service.pdf)  
  
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
169.254.0.0/16の範囲で使用します。169.254.0.xxxと169.254.255.xxxのアドレスは
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
  
***0.4 Forwarding Rules***
  
送信元IPまたは送信先IPが169.254.0.0/16の範囲となっているIPパケットは
あらゆるルータに対してフォワーディングされるべきではありません。
その代わりに送信者はARPによって宛先IPアドレスを解決し
同じリンクの中で直接パケットを送信すべきです。
Auto-IPのアドレス範囲が送信元となっており、マルチキャストアドレスが宛先となっている
IPデータグラム通信はローカルリンクから外に出るべきではありません。
デバイスは169.254.0.0/16のIPアドレスは常にリンクローカルに存在し
直接的なリーチャビリティがあるものとして取り扱って構いません。
  
169.254.0.0/16のアドレス範囲はそれ以上サブネット分割すべきではありません。
  
***0.5 Periodic checking for dynamic address availability***
  
デバイスはIPアドレスを自分で設定した後も定期的にDHCPサーバの存在をチェックすべきです。
これはDHCPDISCOVERMessageを送ることで実現できます。
チェックをどういう頻度で実施するかどうかは**実装依存**ですが
5分に一回チェックをすることがネットワーク帯域の観点と接続性の観点で
バランスを維持できると思われます。
もしDHCPOFFERをもらえた場合、デバイスは自動アドレス割り当てのフェイズに映ります。
一度DHCPアドレスがアサインされれば、デバイスはAuto-IPのIPアドレスを
リリースすることができます。しかし一定時間の間（**実装依存**）古いIPアドレスを
後方互換接続性確保のために保持しておくことも許されます。
新しいIPアドレスを獲得した場合、デバイスは、可能な限り
これまで大々的に広告していたIPアドレスをキャンセルし新しいIPアドレスを広告すべきです。
  
Discoveryの章では広告と広告のキャンセルについて述べます。
また、Eventingの章ではEventの消し方についても述べます。
  
複数のIPアドレスを持ったマルチホームのデバイスについてもIPアドレスを新しいものに
切り替えする場合は古いIPアドレスをキャンセル広告し新しいIPアドレスを広告すべきです。

Discoveryの章では広告と広告のキャンセル・アップデートについて述べます。
また、Eventingの章ではEvent受信者への影響についても述べます。
  
***0.6 Device naming and DNS interaction***
  
デバイスがネットワークアドレスを正常に取得できたら
そのネットワーク上において、そのIPアドレスでデバイスが参照されるかもしれません。
これはエンドユーザがデバイスを探し見つける際に必要となります。
このような場合はIPアドレスよりもヒューマンリーダブルなデバイス名の方が
人間にとっては親しみがあります。
もしデバイスがDHCPサーバに対してhostnameを提供しそれがDNSサーバに登録される場合は
(hostnameがユーザによって書き換えが可能であるという意味で)
そのリクエストされたhostnameが一意になっていることを確認しなければなりません。
多くの場合、デバイスはhostnameを提供せず、代わりにIPアドレスを入れたURLを提供します。
 
さらに言えば、hostnameはIPアドレスよりもstatic的です。
クライアントがhostnameでデバイスを参照している場合はそのデバイスの
IPアドレスの変動に関して何も意識する必要がありません。
  
デバイスのDNS名とIPアドレスを紐づけるためRFC2136に従い
手動もしくは自動でDNSのデータベースにインプットすることができます。
  
デバイスがDDNSアップデートに対応している場合はDNSサーバに対して
直接DNSレコードを追加することができますし、DHCPサーバに対してDHCPクライアントの
挙動をベースにDNSレコード登録させるように設定をすることも可能です。
  
***0.7 Name to IP address resolution***
  
DNS名で参照する他のデバイスとデバイスが通信をしようとした場合は
そのIPアドレスを見つける必要があります。
デバイスはRFC1034とRFC1035にもとづいて事前に設定されたDNSサーバから
そのDNS名に紐づいたIPアドレスを取得します。
デバイスはDNSサーバのリストを固定で事前に設定することができます。
または、デバイスはDHCPOFFERを受けた際
（もしくはDHCPOFFERを受けた後もDHCPINFORMを受けることにより）
動的にDNSサーバのリストをDHCPサーバから取得するように設定することも可能です。
  
***0.8 References***
  
- RFC1034, Domain Names - Concepts and Facilities. Available at: [http://www.ietf.org/rfc/rfc1034.txt](http://www.ietf.org/rfc/rfc1034.txt)
- RFC1035, Domain Names - Implementation and Specification. Available at: [http://www.ietf.org/rfc/rfc1035.txt](http://www.ietf.org/rfc/rfc1035.txt)
- RFC 2131, Dynamic Host Configuration Protocol. Available at: http://www.ietf.org/rfc/rfc2131.txt.
- RFC 2136, Dynamic Updates in the Domain Name System. Available at: http://www.ietf.org/rfc/rfc2136.txt.
- RFC 3927, Dynamic Configuration of IPv4 Link-Local Addresses. Available at: http://www.ietf.org/rfc/rfc3927.txt.

***1 Discovery***
  
"Disovery"はUPnPネットワーキングのStep1です。
DiscoveryはStep0(IPアドレッシング)でIPアドレスを取得した後にきます。
Discoveryによってデバイスはデバイスを検索することができます。
Discoveryはデバイスの機能性について調べるDescription(Step2)を可能にし
デバイスに対してコマンドを送ることができるControl(Step3)を可能にし
デバイスの状態変化を検知することができるEventing(Step4)を可能にし
デバイスのユーザーインターフェースを表示するPresentation(Step5)を可能にします。
  
DiscoveryはUPnPネットワーキングの最初のステップです。
デバイスがネットワークに追加された時、そのデバイスはServiceの広告を
そのネットワークに存在するデバイスに実施することができます。
同時にデバイスがネットワークに追加された場合、デバイスは同じネットワークに
存在するデバイスを探索することができます。
基本的に両方のケースにおいて交換されるMessageは
デバイスの代表的なサービスであったり、UUIDであったり
より詳細な情報を取得するためのポインタ情報であったり
デバイスの現在の実行状態のoptionalなパラメタだったり
最小限な情報のみとなります。
  
<TODO: Figure 1-1: - Discovery architectureの図をはる。だいたいわかるけど。>
  
もしデバイスが自分自身が新しいネットワークに追加された場合
DisoveryMessageを発信し、自身の持つ内臓デバイス及びServiceについて
数回マルチキャストで広告すべきです(初回広告)。
デバイスはマルチキャストアドレスをリッスンすることで新しい機能・サービスが
利用可能になったことを検出することができます。
マルチホームデバイスはUPnPが有効となっている全てのインターフェースにおいて
ディスカバリMessageをマルチキャストすべきです。
マルチホームデバイスはUPnPが有効になっている一つ以上のインターフェースにおいて
マルチキャストMessageを受信することが許されます。
  
デバイスがネットワークに追加された場合、
ディスカバリMessageをマルチキャストしサービスまたはデバイス
またはその両方をを検索することが許されます。
全てのデバイスはマルチキャストアドレスでリッスンし
もしそのrootデバイス・内臓デバイス・サービスがディスカバリMessageの検索範囲に
含まれている場合はそのMessageに応答すべきです。
  
加えて、デバイスは特定のIPの1900番ポート
（またはSEARCHPORT.UPNP.ORGヘッダに示されたポート番号※1900番ポートはこれに上書きされる）
に対してユニキャストでディスカバリMessageを送り、
特定のIPアドレスを持ったUPnPデバイス・サービスを検索することができます。
この動作はデバイスが（適切なポートでリッスンしている）UPnPデバイスであり
そのIPアドレスを既にしっているという想定です。
デバイスはユニキャスト検索を数々のアプリケーションを検索するのに使えます。
ユニキャスト検索は特定のデバイスの持つディスカバリ情報（e.g. UUID, URL）を
手っ取り早く確認するのに便利です。
全てのデバイスはリッスンしているポートでユニキャストで届いたディスカバリMessageについて
その検索範囲に含まれている場合はそのMessageに応答すべきです。
  
マルチホームデバイスはUPnPが有効な複数のインターフェースのうち一つ以上の
インターフェースからマルチキャストディスカバリMessageを出すことが許されます。
マルチホームデバイスは全てのUPnPが有効なインターフェースで通常の
マルチキャストアドレスでマルチキャストディスカバリMessageをリッスンすべきです。
マルチホームデバイスは1900番ポート（またはSEARCHPORT.UPNP.ORGヘッダにより指定されたポート）にて
ユニキャストディスカバリMessageをリッスンすべきです。
  
基本的にデバイス・rootデバイス・内臓デバイス・サービスは
ディスカバリMessageの検索範囲に存在する限り応答を返すべきです。
  
繰り返しになりますが、デバイスはディスカバリMessageを放出しているデバイスや
デバイス検索のためのディスカバリMessageに応答したデバイスを記憶することが許されます。
いずれの場合のケースであっても、デバイスがより多くの情報を他のデバイスから入手するには
"Description"クエリMessageを送る必要があります。
Descriptionの章ではDescription Messageについて詳しく述べます。
  
デバイスがネットワークから削除される際には、可能であれば迅速かつ効率的に
rootデバイス・デバイス・サービスが使えなくなることを広告するため
数回のマルチキャストディスカバリーMessageを送るべきです。
同様にデバイスのIPアドレスが変更となった場合は速やかに新しいIPアドレスを
使うということを広告すべきです。
    
マルチホームデバイスについてもインターフェース単位で同様に
利用不可になったことを広告すべきです。しかし、関係ないインターフェースで
広告してはいけません。
  
マルチホームデバイスはUPnPが有効なインターゲースがネットワークに追加されるたびに
BOOTID.UPNP.ORGフィールドの値を増加させてUpdateMessageとしてマルチキャストを送るべきです。
全てのUPnPインターフェースでUpdateMessageを送信した後は新しいBOOTID.UPNP.ORGヘッダフィールドを
持ったディスカバリMessageを送信するべきです。
  
マルチホームデバイスはデバイスが変更となった場合もすぐさまその情報を広告し
古いIPアドレスについては放棄の広告をすべきです。
そして、BOOTID.UPNP.ORGフィールドの値を増加させてUpdateMessageとして
数回マルチキャストを送った後に全てのUPnPインターフェースで新しいBOOTID.UPNP.ORGフィールドの値を
持ったディスカバリMessageを送るべきです。
  
最後に、もしマルチホームデバイスがUPnPが有効なインターフェースの
リンクダウンと復旧を検知した場合はBOOTID.UPNP.ORGの値を増加させ
リンクとは関係がないUPnPインターフェースに対して新しいBOOTID.UPNP.ORGの値で
数回のUpdateMessageをマルチキャストするべきです。
数回のUpdateMessageをマルチキャストし終わったあと、（リンクと関係があるか
ないかに関わらず）全てのUPnPインターフェースで新しいBOOTID.UPNP.ORGの値をもった
ディスカバリMessageを送るべきです。
  
ネットワークの輻輳を避けるため、それぞれのマルチキャストMessageのIPヘッダの
TTL(Time to Live)の値の初期値は2とし、それは状況に応じて設定変更可能とすべきです。
もしTTLが1よりも大きい場合、マルチキャストMessageを複数のルータを往来することが
可能となります。デバイスがnon-AutoIPなIPアドレスを使っている場合は
ルータに対してIGMP Join Messageを送ることでルータはマルチキャストを認識し
フォワードができるためです。（これはAuto-IPなアドレスを使っている場合は必要ありません。）
なぜなら、ルータはAuto-IPなアドレスのパケットをフォワーディングしないからです。)
  
Versioning: Discoveryは異なるバージョンのUPnPネットワーキングをするデバイス間の相互接続性に関して重要な責務を負っています。
UDAは*major.minor*という形の数字でバージョンを表現しています。数字が大きい方がより新しいバージョンです。
minorバージョンの数字が異なっている場合は相互互換性を保証すべきですが
majorバージョンの数字が異なっている場合は相互互換性は保証されません。
しかし、UDAの2.0はUDA1.1（ひいては1.x）に後方互換性があります。
（UDA2.0のデバイスがUDA1xのデバイスを認識出きるようにするためです）
UDA1.xのデバイスはUDA2.0のデバイスと共存することができますが
UDA1.xのデバイスはUDA2.0特有の機能へアクセスすることはできません。
UPnPのバージョン情報はDiscoveryMessage及びDescriptionMessageの中に含まれます。
DiscoveryMessageはデバイスがサポートするUPnPネットワーキングのバージョンを
SERVERヘッダ及びUSER-AGENTヘッダに保持しています
デバイスのサポートするバージョン情報はその他関係するDiscoveryMessageの中にも含まれています。
さらに、Descriptionドキュメントの中にも同じ情報が入っています。
SERVER及びUSERーAGENtヘッダはUPnPネットワークのバージョンを交換しデバイス間の
Eventingなどをするのに使われます。
  
本章の残りの項目ではSSDP(Simple Service Discovery Protocol)という名前で知られた
UPnPにおける探索プロトコルについて述べます。
具体的にはデバイスはどのように自身を広告し、広告キャンセルをするのかと
デバイスはいかにしてデバイスを探索し、デバイスは探索に対してどのように回答するのかを述べます。
  
***1.1 SSDP message format***
  
SSDPはRFC2616で記載されたHTTP1.1をヘッダのフォーマットとして一部使用しています。
いかし、それはHTTP1.1を完全にベースにしているのではなく、TCPではなくUDPを使ってますし
独自の処理規則を持っています。章1.1.xではSSDPMessageの一般的なフォーマットについて述べます

全てのSSDPMessageはRFC2616の"generic message"（章4.1）に基づいてフォーマットされているべきです。
SSDPMessageはstart-lineと数個のヘッダフィールドを持ちます。
SSDP Messageはbody部分を持つべきではありません。もしSSDPMessageでbody部が入っているものを
受信した場合、デバイスはそのbody部分を無視して構いません。
  
***1.1.1 SSDP Start-line***
  
それぞれのSSDPMessageはたった一つのStart-lineを持ちます。章1.2"Advertisement"及び
章1.3"Search"の中に記載されたフォーマットがSSDPMessageの全てです。
Start-lineはRFC2616の章5.1と章6.1に記載されています。
さらに言えば、Start-lineは以下の3つの内のどれかであるべきです。
  
- NOTIFY * HTTP/1.1\r\n
- M-SEARCH * HTTP/1.1\r\n
- HTTP/1.1 200 OK\r\n
  
ここで明らかにしておきたいのは、Start-lineの"HTTP/1.1"はSSDPが完全にHTTP1.1を
ベースに作成されているという訳ではないということです。あくまでこのStart-lineは
後方互換性のためだけに存在します。
  
***1.1.2 SSDP header fields***
  
SSDPMessageのヘッダフィールドはRFC2616の章4.2に従って
大文字小文字の区別のないHeaderフィールドと大文字小文字の区別があるValueフィールドが
コロン（:）によって分けられて記述されています。SSDPの場合は使えるValueフィールドを制限しています。
  
SSDP Headerの例  
HOST: 239.255.255.250:1900
  
***1.1.3 SSDP header field extensions***
  
UPnP Working committeeとUPnPベンダはSSDPMessageに任意のSSDPヘッダを追加することができます。
また、UPnP Forum Technical committeeが独自に定義しているヘッダも使うことができます。
(e.g. 章1.2"Advertisement"で定義された BOOTID.UPNP.ORG , CONFIGD.UPNP.ORG , NEXTBOOTID.UPNP.ORG , SEARCHPORT.UPNP.ORG ヘッダ)
ヘッダフィールドの名前衝突を防ぐため、ベンダは以下のフォーマットでベンダ固有の
ヘッダを定義することが可能です。
  
 field-name = token “.” domain-name  
  
domain-nameはVendor Domain Nameとすべきです。利用可能な非メタ文字情報に関してはRFC2616の
章2.2を確認してください。ベンダ固有のSSDPヘッダの例を以下に示します。
  
 myheader.philips.com: “some value”  
 myheader.sony.com: “other value”
  
***1.1.4 UUID format and recommended generation algorithms***
  
UPnP 2.0のデバイスはUUIDを以下のフォーマットに従って実装すべきです。
しかし、UPnP2.0デバイスはUPnP1.0形式のフォーマットで記述されたUUIDも受け入れるように実装するべきです。
  
UUIDは128bitの数字によって定義されます。
  
 UUID = 4 * <hexOctet> “-” 2 * <hexOctet> “-” 2 * <hexOctet> “-” 2 * <hexOctet> “-” 6 * <hexOctet>  
 hexOctet = <hexDigit> <hexDigit>  
 hexDigit = “0”|“1”|“2”|“3”|“4”|“5”|“6”|“7”|“8”|“9”|“a”|“b”|“c”|“d”|“e”|“f”|“A”|“B”|“C”|“D”|“E”|“F”  
  
以下は128bitのUUIDの例です。
  
 “2fac1234-31f8-11b4-a222-08002b34c003”  
  
UUIDは以下の条件を満たすならばどのような公式で生成しても構いません。
  
 a) 別のUUIDと滅多に被らない公式であること
 b) 必ず128bitの数字に落とし込める公式であること
 c) 時間によってUUIDが変わらず一意であること
  
また、以下の時間・MACベースのアルゴリズムで生成したUUIDを
不揮発メモリに焼き付ける手法が推奨されています。
  
[http://www.opengroup.org/onlinepubs/9629399/apdxa.htm](http://www.opengroup.org/onlinepubs/9629399/apdxa.htm)
  
***1.1.5 SSDP processing rules***
  
章1.1の"SSDP message format"に記載のフォーマットに従っていないSSDPメッセージを
受け取った場合、受信者はそのメッセージをドロップすべきです。
受信者はそのようなSSDPメッセージを解析し、相互運用性の確保に勉めることが許されます。
SSDPヘッダを解析する際、受信者は(章1.2"Advertisement"や章1.3"Search"に記載の)
全ての必須ヘッダについて先に解析しなければなりません。
また、それ以外の全てのヘッダに関しては受信しても無視することが許されます。
受信者は不明なヘッダを解析せず無視できるようにするべきです。
  
***1.2 Advertisement***
  
デバイスがネットワークに追加された際、デバイスは自身の持つサービスを別のデバイスに広告します。
それは239.255.255.250:1900というアドレス・ポートへのマルチキャストディスカバリメッセージにて行われます。
デバイスはネットワーク上に新たな機能が追加されたことをこのポートをリッスンすることで検出します。
すべての利用できる機能について広告するため、デバイスはrootデバイスや内蔵デバイス・サービスについてする
ディスカバリメッセージを数回マルチキャストすべきです。
それぞれのメッセージにはデバイスの持つ各々のサービス・デバイス特有の情報が含まれます。
広告されたメッセージは失効までの時間を含めるべきです。もしデバイスが引き続き利用可能であるなら
広告は（新しい失効時間で）サイド送信されるべきです。もしデバイスがもう利用可能でない場合は
デバイスは明示的に自身の実施した広告をキャンセルする広告をすべきです。
しかし、もしデバイスがそれをできない場合は広告は失効時間をもって失効します。
もしマルチホームデバイスがUPnPが有効な一部のインターフェースのみ利用不可となった場合は
すぐさまその影響があるUPnPが有効なインターフェースに対して明示的にキャンセルする広告を
すべきです。（ただし、影響を受けないUPnPインターフェースを覗きます。）
これができない場合はUPnPインターフェース・IPアドレスは過去に広告に示された失効時間で
失効します。加えて、以下のヘッダがこのドキュメントには示されています。
  BOOTID.UPNP.ORG, NEXTBOOTID.UPNP.ORG, CONFIGID.UPNP.ORG, SEARCHPORT.UPNP.ORG
  
BOOTID.UPNP.ORGはデバイスがネットワークに（再）参加し初回広告されるか（これをUPnP用語でrebootと言います。）、
UPnPが有効なデバイスが追加された際にインクリメントされるべきです。
もしデバイスが明示的にSSDPメッセージのBOOTID.UPNP.ORGによって変更を広告しなかった場合は
そのデバイスがネットワークに存在し続けている限り、同じBOOTID.UPNP.ORGフィールドの値を
使って広告メッセージやSearchメッセージ応答やアップデート広告や最終的なネットワーク離脱広告をすべきです。

デバイスはBOOTID.UPNP.ORGヘッダの値を解析することによってあるデバイスが
"reboot"によってステート情報をロストした（eventのセッションが消え、特定DCPのセッションも変更となった）ことを
検知することができます。
  
デバイスがBOOTID.UPNP.ORGヘッダの値の変更なしにIPアドレスを変更することができないため
BOOTID.UPNP.ORGヘッダの値はマルチホームデバイスのIPアドレス変化を検出することができます。
（通常、別のデバイスから見るとマルチホームデバイスのSSDPメッセージは同じUUID, BOOTID.UPNP.ORGとなるが
IPアドレスが変更となった場合、このBOOTID.UPNP.ORGヘッダの値は異なるものとなる。）
NEXTBOOTID.UPNP.ORGヘッダはマルチホームデバイスが新たなUPnPインターフェースを追加する際に
使おうとするBOOTID.UPNP.ORGヘッダの値を示します。  
CONFIGD.UPNP.ORGヘッダの値は現状のデバイスとサービスのセットを識別します。
デバイスはこの値をパースすることにより、新しいDescriptionクエリメッセージを送るべきかどうか識別します。
SEARCHPORT.UPNP.ORGヘッダの値はデバイスがユニキャストでM-SEARCHメッセージを送る際のポート番号を知ることができます。
デバイスはこのポート番号を使ってM-SEARCHメッセージをユニキャストすべきです。
これらのヘッダの詳細については後ほど説明されています。
  
***1.2.1 Advertisement protocols and standards***
  
広告を送るために、デバイスは以下に示すUPnPのプロトコルセットを全て使用します。
（これらのUPnPプロトコルスタックはこのドキュメント最初にリストされています。）
  
  <TODO: Figure 1-2: — Advertisement protocol stack の写真をはる>
  
高レイヤー層はディスカバリメッセージがベンダ固有の情報
（e.g. デバイスのDescriptionのURLやデバイス識別子）を含んでいます。
スタックの下の方ではUPnP Forum working committeeによって策定された
ベンダコンテンツの情報が供給されます（e.g. debvice type）。
このレイヤの上位にあるメッセージはUPnP固有のプロトコルで策定されこのドキュメントに定義されています。
順番に、SSDPメッセージはUDP over IPによって伝送されます。
参考のため、[]に囲まれた色は下記のディスカバリメッセージのどのプロトコルで特定のヘッダ・値が定義されているかを示します。
  
***1.2.2 Device available - NOTIFY with ssdp:alive***
  
デバイスがネットワークに追加された際、サービス・内蔵デバイス・ルートデバイスを広告する
マルチキャストディスカバリメッセージを送るべきです。
それらのディスカバリメッセージは4つの主要なコンポーネントを含めるべきです。
  
a) notification type (e.g. device type)：_NT_ (Notification Type)ヘッダによって送信されます。
  
b) composite identifier：USN (Unique Service Name)ヘッダによって送信されます。
  
c) デバイス(サービス)に関する詳細な情報を入手するURL：LOCATIONヘッダによって送信されます。
  
d) 広告キャッシュの有効期限：CACHE-CONTROLヘッダによって送信されます。
  
各機能に関して広告するためデバイスは多くのディスカバリメッセージをマルチキャストします。
特に、ルートデバイスは以下の項目をマルチキャストすべきです。
  
- ルートデバイスによるディスカバリメッセージ
  
  <Table 1-1 — Root device discovery messages の写真をはる>
  
- 内蔵デバイスによるディスカバリメッセージ
  
  <Table 1-2 — Embedded device discovery messagesの写真をはる>
  
- それぞれのデバイスごとのそれぞれのサービスタイプ
  
  <Table 1-3 — Service discovery messagesの写真をはる>  
  

もしルートデバイスが内蔵デバイス"d"を持ち内蔵サービス"s"を持ち
一つだけ"k"という固有のサービスがを持っていた場合
結果として3+2d+k個のリクエストが発生することになります。
もし特定のデバイスや特定の内蔵デバイスのインスタンスが複数のサービスタイプを含んでいる場合であっても
サービスタイプは一度だけ広告するだけで構いません（インスタンス毎にする必要はありません）。
ただし、二つのデバイスが同じサービスタイプであったとしてもそれらのサービスは
独立してアナウンスされるべきであることにご注意ください。
この広告により、利用したいデバイス達が利用するデバイスのすべての機能範囲について知ることができます。
これらの複数の広告メッセージに関しては連続する分概ね似通った広告有効期限（expiration times）を持つべきです。
その順番については重要ではありませんが、それらの連続する広告メッセージを
個別に更新したりキャンセルしたりすることは禁止されています。
完全な後方互換性のため、古いバージョンのタイプ値と合わせられた最新のUPnPデバイスタイプと
サービスタイプが実装に必要となります。デバイスは自身がサポートする最も新しいバージョンの
タイプ値をそれぞれのデバイスタイプ・サービスタイプについて広告すべきです。
例えば、もしデバイスがバージョン"2"のAudioサービスタイプをサポートしている場合は
たとえそれがバージョン"1"と互換性がありサポートしていたとしてもバージョン"2"の広告のみすべきです。
デバイスはそのデバイスの持つバージョン・サービスよりも新しいバージョンのデバイスと
通信することができます。これは後方互換性を実装要件としているためですが、使える機能は古い方のバージョンの機能のみとなります。
例えば、バージョン1の"Audio"サービスしか使えないデバイスがバージョン2の"Audio"サービスの広告を受信した場合
バージョン1のデバイスはバージョン2のデバイスを使用することができるでしょう。
  
適切な広告間隔の時間を設定することはネットワークトラフィックの最小化と
デバイスステータスの最新化のトレードオフをすることとなります。
*デバイスのステータスが最新化されていることが保証される1800秒*に近い
比較的短い時間間隔における広告をすると、ネットワークトラフィックを犠牲にする代わりに
デバイスステータスを最新化することができます。
比較的長い時間間隔（言うなれば、何日間というオーダー）で広告を実施した場合は
デバイスステータスが最新化されているかは妥協的になりますが
ネットワークトラフィックに関しては大きく削減することができます。
一般的にはデバイスベンダーはそのデバイスの使い方（使われ方）によって
最適な広告間隔を選択すべきです。
これは、
ネットワークにおいて短期的に使用されるデバイスについては短い間隔で広告し、
ネットワークにおいて長期的に使用されるデバイスにおいては長い間隔で広告する、
ということが想定されています。
無線デバイスのようなネットワークへの接続・切断が頻繁に行われるものは
短い広告間隔でデバイスの可用性を担保すべきです。
それぞれの広告間隔（初期広告～継続的な更新広告）はそれぞれ対応する時間となります。最初の広告はなるべく早く送信されるべきです。
継続的な更新広告はサービスのグループ単位で送信せず、時間単位で送信することが許されます。
  
グループ単位ではなく時間単位で継続的な更新広告をすることはネットワーク的な信頼性の向上につながります。
デバイスごとの広告送信間隔の増加によるトータルなネットワーク負荷を増やさずに済みます。
以下の２つの図は全体的な時間の中で広告間隔を引き伸ばすケース／しないケースを示したものです。

最初の図はデバイスがネットワークに初回接続しており初回広告をし、その後一定の間隔で更新広告をしていることを示しております。（広告間隔を引き伸ばさないケース）
二つ目の図はデバイスが広告間隔を引き伸ばすケースです。
  
 ＜Figure 1-3: — Initial and repeat announcements, no announcement spreadingの図を貼る＞
   
 ＜Figure 1-4: — Initial and repeat announcements, message spreading of repeat announcements の図を貼る＞
   
デバイスは（ある意味ネットワークストームのような）初回広告を送信する前にランダムなインターバルで（例えば0～100msで）待つべきです。
※このランダムなインターバルはデバイスが新しいIPを取得する際や新しいUPnPインターフェースをインストールした際にも適用されるべきです。
  
本質的にUDPは信頼性が無いため、*ディスカバリメッセージは一回以上、（例えば数百msの間隔を空けて）複数回送信されるべき*です。
*ネットワーク混雑を避けるため、3回よりも多くディスカバリメッセージを送るべきではありません*。
加えて、デバイスはCACHE-CONTROLヘッダに記載した失効期間よりも前に自身の広告をしなければなりません。

広告の失効より前に広告のロストに対するリカバリの機会を与えつつ
複数のデバイスの同時送信によるネットワークトラフィックの急増を避けるため
失効対策用の広告間隔は広告失効期間の二分の一よりも短いランダムに分散された間隔で実施することが推奨されます。
*UDPは（ある実装においては概ね512KByteくらい小さい）その長さによって分割されることにご注意ください。各メッセージは一つのUDPパケットの中に収まるべきです。*
3+2d+kのディスカバリメッセージが特定の順番で来る保証はどこにもありません。
  
マルチホームデバイスはそれぞれのUPnPが有効なインターフェースで上述の広告処理を実施スべきです。
それぞれのUPnPが有効なインターフェースで送信される広告はHOST,CACHE-CONTROL,LOCATIONヘッダを除き全て同じ値が含まれるべきです。
広告のHOSTヘッダフィールドの値は特定のIPプロトコル(IPv4,IPv6)インターフェースで使用される標準のマルチキャストアドレスにすべきです。
LOCATIONヘッダフィールドの値に記載されたURLは広告が送信されるインターフェースにリーチャビリティがあるべきです。
最後に、異なるインターフェースから送信される広告に関しては異なるCACHE-CONTROLヘッダの値を取ることが許され、つまりはそれぞれ異なる周期で広告することが許されます。
  
デバイスがネットワークに追加された際は以下のフォーマットに示すような
NOTIFYメソッドとNTSヘッダにssdp:aliveを持ったマルチキャストメッセージをデバイスは送信すべきです。

```
NOTIFY * HTTP/1.1
HOST: 239.255.255.250:1900
CACHE-CONTROL: max-age = _<seconds until advertisement expires>
LOCATION: <URL for UPnP description for root device>
NT: <notification type>
NTS: ssdp:alive
SERVER: <OS/version UPnP/2.0 product/version>
USN: <composite identifier for the advertisement>
BOOTID.UPNP.ORG: <number increased each time device sends an initial announce or an update message>
CONFIGID.UPNP.ORG: <number used for caching description information>
SEARCHPORT.UPNP.ORG: <number identifies port on which device responds to unicast M-SEARCH>
```
  
※NOTIFYメソッドのメッセージにはbodyデータはありません。しかし、メッセージは最終行が空行のヘッダフィールドをもつべきです。
  
IPパケットのTTLは標準2ですが、自由に設定できるようにすべきです。
以下のリストは上述のリクエストラインとヘッダリールドに関する詳細となります。
フィールド名はケースセンシティブではありません。全てのフィールド値は特に指定が無ければ全てケースセンシティブです。

*Request line*  
Shall be "NOTIFY * HTTP/1.1"  

- NOTIFY  
  イベント及び通達を送るメソッド
- \*  
  メッセージ共通。リソースの指定は無いことを示すため\*にすべきです。
- HTTP/1.1  
  HTTPバージョン。  

*Header fields*
  
- HOST  
  
必須。Internet Assigned Numbers Authority(IANA)によって予約されたSSDPのポートとマルチキャストアドレスが含まれる。
239.255.255.250:1900にすべきです。もしポート番号(":1900")が省略された場合
受信者側はデフォルトのSSDP通信ポートである1900であると暗黙的に推測すべきです。
  
- CACHE-CONTROL  
  
必須。フィールド値にInteger秒でその広告が有効期間を示すmax-ageディレクティブ("max-age=")が含まれているべきです。
この期間が終わった時、デバイスはそのデバイス（またはサービス）がもう利用できないことを認識すべきです。
その期間の間にデバイスが、あるサービスや埋め込みデバイス及びそのデバイスのサービスについて、
もしくはそれらのrootデバイスについて、広告を受け取った場合についてはデバイスは
そのデバイスの関連全てについてもまだ有効であると推測できます。
有効期間の値については上述に定義したような例外もあれど1800秒（30分）以上にすべきです。
実際にはUPnPベンダによって定義されます。max-age以外のディレクティブは含まれるべきではなく受信しても無視されるべきです。
  
- LOCATION  
  
必須。root deviceの持つUPnP descriptionへのURLが含まれます。
通常は管理されていないネットワークにおける名前解決よりもIPアドレスの文字列がhostとして支持されます。
実際にはUPnPベンダによって定義されます。RFC3986におけるSingle absolute URLとなります。
  
- NT  
  
必須。フィールド値はNotification Typeが含まれるべきです。それらは以下の内の一つであるべきです。
（Table 1-1."Root device discovery messages", Table 1-2."Embedded device discovery messages",
 Table 1-3."Service discovery messages"を参照）Single URIとなります。

    - upnp:rootdevice
        rootデバイスに一度だけ送信されます

    - uuid:device-UUID
        各デバイスに一度だけ送信されます。device-UUIDはUPnPベンダによって指定されます。
        UUIDの必須フォーマットは章1.1.4 "UUID format and recommended generation algorithms" を参照。

    - urn:schemas-upnp-org:device:deviceType:ver
        各デバイスに一度だけ送信されます。deviceTypeとverはUPnP Forum working committeeにて定義されます。
        verはデバイスタイプのバージョンを示します。

    - urn:schemas-upnp-org:service:serviceType:ver
        各デバイスに一度だけ送信されます。deviceTypeとverはUPnP Forum working committeeにて定義されます。
        verはデバイスタイプのバージョンを示します。

    - urn:domain-name:device:deviceType:ver
        各デバイスに一度だけ送信されます。domain-nameはVendor Domain Nameであり、deviceTypeとverはUPnPベンダによって定義されます。
        verはデバイスタイプのバージョンを示します。__Vendor Domain Nameに含まれるピリオド文字はRFC22141に従ってハイフンに変換されるべきです。

    - urn:domain-name:service:serviceType:ver
        各デバイスに一度だけ送信されます。domain-nameはVendor Domain Nameであり、serviceTypeとverはUPnPベンダによって定義されます。
        verはデバイスタイプのバージョンを示します。__Vendor Domain Nameに含まれるピリオド文字はRFC22141に従ってハイフンに変換されるべきです。
- NTS
    必須。フィールド値にはNotification Sub Typeが含まれます。概ね"ssdp:alive"になります。Single URIです。

- SERVER
    必須。UPnPベンダによって指定されます。フィールド値はHTTP/1.1で定義された"product tokens"で始まるべきです。
    
