UPnP Device Architecture 2.0
===================
原文: [http://upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v2.0.pdf](http://upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v2.0.pdf)

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
  
UDAは"見えないネットワーキング"と
ベンダによる多岐に渡る"デバイスカテゴリ"のデバイス探索機能により
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
  
UPnPにおける"ユニバーサル"とは”デバイスドライバレス”という意味であり
デバイスドライバの代わりに一般的なプロトコルを使用します。
  
UPnPネットワーキングはデバイスの仕様とは独立した実装です。
UPnPデバイスはどのOSのどんなプログラミング言語によっても実装が可能です。
  
*UPnPのアーキテクチャはアプリケーションのAPIデザインに関して指定や制約をしません。*
OSベンダはエンドユーザの要望にあった形でAPIを作成すべきです。
  
***UPnP Forum***
  
UPnP Forumは異なるベンダのデバイスやPCとの接続性の維持に努める組織です。
UPnP Forumは機種間相互接続性の推進のためXMLベースのデバイスのスキーマ情報を標準化します。
  
UPnP Forumはデバイス間の相互接続性を一様に検証しそれを認証することを推進するため
複数の産業にまたがる企業によって構成されています。
  
UPnP Forumは"UPnP Logo Program"によりデバイス検証し認証するプロセスを司り
UPnP対応の保証をその他のUPnPデバイスのベンダに周知します。
  
UPnPデバイス認証のプロセスはベンダの開発者にも、会費を支払っているメンバーにもオープンであり
認証されたデバイスがUPnP機能に対応しているという情報についてもオープンです。
詳しくは[http://www.upnp.org](http://www.upnp.org.)を参照してください。
  
UPnP Forumは各方面の専門分野にワーキングコミッティをおいています。
ワーキングコミッティではデバイス標準を作成したりサンプル実装を作成したり
適切なテストスイートを作成したりします。
  
このドキュメントではそれらのワーキングコミッティ分野における技術的な見地について示します。
UPnPベンダは"UPnP Logo Program"によってその技術的な適合性を発信することにより
相互接続性に対する信頼を得ることができます。
*このLogo Programに関わらず、ベンダはUDAに沿って
形式張った標準的な認証申請なしにデバイスを開発することも可能です。
もしベンダが非公式でUPnPデバイスを開発した場合はワーキングコミッティのやり方とは
違った方式で技術的な実装をしていることになります。*
    
***In this document***
  
UPnPデバイスアーキテクチャ（aka DCPフレームワーク） はControllerまたは
ControlPointまたはデバイスによってお互いに通信されます。
"Discovery","Description","Control","Eventing","Presentation"のため
UDAは以下の通りのプロトコルスタックを使用しています。
  
<Fig.1>
  
最も上位のレイヤ"UPnP Vendor"はデバイスのベンダ依存情報のみ含まれています。
その下位レイヤ"UPnP Forum"はベンダ情報を補完するための情報が含まれます。
これらの情報は"SSDP","GENA",マルチキャストイベントプロトコル,その他文書化された
UPnPのプロトコルによって伝送されます。
  
SSDPはユニキャストまたはマルチキャストUDPによって伝送されます。
マルチキャストイベントはマルチキャストUDPによって伝送されます。
GENAはHTTPによって伝送されます。
結局のところ、これらのメッセージはすべてIPプロトコルの上に成り立ちます。
残すところ、このドキュメントが示すのはこれらのプロトコルがどういうフォーマットで
どういうメッセージで利用されているか、ということです。





