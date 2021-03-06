.. glossary:

------------
用語集
------------

業界ではおなじみの概念や、Calmが使用する際に特定の意味を持つ用語が多数存在します。この Calm の用語集を参照して、発見と習得の方向付けを行ってください。詳細は追加のリソースを参照してください。

Nutanix Calm用語集
++++++++++++++++++++

.. glossary::

    アクション
        :term:`ブループリント` の中で、順次実行される :term:`タスク <タスク>` のセット。
        全てのブループリントに含まれる基本的なアクションは以下のようになる。作成、削除、停止、開始、再起動。
        開発者のロールは、追加のカスタムアクション(ランブックなど)、スケールインとスケールアウトのアクション、作成前と作成後のアクションを作成することができます。

    アプリケーション
        :term:`ブループリント` のデプロイメントインスタンスであり、トップレベルのCalmメニューアイコンでもあります。

    アプリケーションプロファイル
        開発者ロールは、 :term:`ブループリント` 変数とサービス作成定義のセットを :term:`アプリケーションプロファイル` にバンドルして名前を付けることができます。
        オペレータロールは、デプロイメントの選択のためにブループリントの起動時にアプリケーションプロファイルを選択することができます。
        例: サービスリソースのサイズ (小、中、大)、展開シナリオ (開発、ステージング、本番)。

    ブループリント
        :term:`アクション` と :term:`タスク` の :term:`依存性` をオーケストレーションした :term:`サービス` トポロジのアプリケーションモデル。
        ブループリントには、少なくとも一つの :term:`サービス` とデフォルトの :term:`アプリケーションプロファイル` が含まれていなければなりません。
        また、トップレベルの Calm メニューアイコンも含まれていなければなりません。

    ブラウンフィールド
        プロバイダからインポートされた :term:`既存のマシン` から構成されるブループリント。

    Calm
        Calmはアプリケーションライフサイクルマネージャであり、ビジネスガバナンス、アプリケーション、インフラストラクチャを単一の成果物である青写真としてモデル化するための自動化プラットフォームを提供します。
        現在、CalmはNutanix :term:`Prism Central` で提供され、ワンクリックで有効になります。

    依存関係
        サービス間の論理的なシーケンシャル接続で、サービスの基本的なアクションを一緒にオーケストレーションします。
        オレンジ色の円弧で示された :term:`タスク` (同じアクションのサービス間)の間に生成された、または開発者が指定したシーケンシャルな接続。

    epsilon
        Calmのワークフローエンジンであり、Life Cycle Managerによって制御される内部コンポーネントであり、 :term:`Prism Central` ファシリティでも使用されます。

    eScript (Epsilon Script)
        ローカルで制限されたPython環境を実行するための :term:`task` タイプ。

    既存のマシン
        :term:`task` で管理できる既存のサービス(Calm でプロビジョニングされていない)。

    Kubernetes
        コンテナ管理およびホスティングシステム、https://kubernetes.io を参照してください。

    マクロ
        :term:`task` と変数で使用できるCalmの組み込みまたはブループリントで指定された :term:`変数` 。

    マーケットプレイス
        Prism Centralで公開されているCalmのブループリントを、プロジェクトとロールで管理するエンドユーザー向けのセルフサービスポータルです。
        また、トップレベルのCalmメニューアイコンもあります。

    Prism Central
        Nutanixのコントロールプレーンで複数のNutanixクラスタを管理して高度な管理機能(:term:`Calm`, :term:`Flow` , :term:`Karbon` など) をウェブコンソールから利用できます。

    プロジェクト
        :term:`プロバイダ` にアクセスできる :term:`ロール` に関連付けられたユーザまたはグループのバンドル。リソースクォータを使用。

    プロバイダ
        :term:`サービス` を提供するインフラストラクチャホスト。

    ロール
        VMの起動、Calmのブループリントの作成など、ユーザーの権限を定義する一連のグループ。

    ランタイム
        ランタイムプロパティは、指定されたプロパティを :term:`ブループリント` やアクションの起動時に上書きすることを可能にします。

    シークレット
        クレデンシャルのパスワードまたはキー、 :term:`アプリケーションプロファイル` 、または秘密のプロパティで指定されたサービス変数。
        シークレットは開発者 :term:`ロール` によって指定され、ブループリントが保存されると盗用を防ぐために隠されます。
        ブループリントがエクスポートされるとシークレットは空白になり、 :term:`task` の出力ではシークレットは空白になります。

    サービス
        典型的には仮想マシン、既存のマシン、または :term:`Kubernetes` のPodのインスタンス。

    Substrate
        インフラストラクチャ :term:`プロバイダ` のインスタンス。アプリケーションを実行するための基盤環境。

    タスク
        :term:`アクション` の中の運用実行の個々のステージ。

    変数
        開発者によって静的に設定された :term:`ブループリント` のプロパティ。
        :term:`タスク` のマクロとして使用され、 :term:`ランタイム` プロパティで指定することで、ブループリントの起動時に利用者に設定を委譲することができます。

Nutanix製品
++++++++++++++++++++

.. glossary::

    Calm
        セルフサービスのアプリケーション中心の自動化と管理については、 :term:`Calm` と https://www.nutanix.com/products/calm を参照してください。
        現在、Calm は Nutanix :term:`Prism Central` で提供されており、ワンクリックで利用可能です。

    Era
        データベース・アズ・ア・サービス（DBaaS）の自動化とデータ管理については、https://www.nutanix.com/products/era を参照してください。

    Flow
        Nutanixクラスタサービスのマイクロセグメンテーション（分散ファイアウォール）セキュリティと管理
        https://www.nutanix.com/products/flow を参照してください。
        Flowは Nutanix :term:`Prism Central` で提供され、ワンクリックで有効になります。

    Karboin
        :term:`Kubernetes` のプラットフォーム・アズ・ア・サービス (PaaS)、マネージドコンテナの提供。
        https://www.nutanix.com/products/karbon を参照してください。
        現在、Karbon は Nutanix :term:`Prism Central` で提供され、ワンクリックで有効になっています。

    Prism Central
        :term:`Prism Central` や https://www.nutanix.com/products/prism を参照のこと。

.. _additional-resources:

追加のリソース
++++++++++++++++++++

#. [ブログ] `Calm Terminology <https://next.nutanix.com/blog-40/calm-terminology-actions-and-dependencies-33852>`_ `[Source] <https://github.com/MichaelHaigh/calm-blueprints/blob/master/DependencyTaskExample/README.md>`_
