# re:invent 2019 SageMaker周辺のアップデート

re:invent 2019で以下のアップデートが発表された。

* [Amazon SageMaker Studio｜機械学習のための初の統合開発環境](https://aws.amazon.com/jp/blogs/news/amazon-sagemaker-studio-the-first-fully-integrated-development-environment-for-machine-learning/)
* [Amazon SageMaker Processing ｜フルマネージドなデータ加工とモデル評価](https://aws.amazon.com/jp/blogs/news/amazon-sagemaker-processin-fully-managed-data-processing-and-model-evaluation/)
* [Amazon SageMaker Autopilot｜高品質な機械学習モデルをフルコントロールかつ視覚的に自動生成](https://aws.amazon.com/jp/blogs/news/amazon-sagemaker-autopilot-fully-managed-automatic-machine-learning/)
* [Amazon SageMaker Experiments｜機械学習モデルの整理、追跡、比較、評価](https://aws.amazon.com/jp/blogs/news/amazon-sagemaker-experiments-organize-track-and-compare-your-machine-learning-trainings/)
* [Amazon SageMaker Debugger｜機械学習モデルのデバッガ](https://aws.amazon.com/jp/blogs/news/amazon-sagemaker-debugger-debug-your-machine-learning-models/)
* [Amazon SageMaker Model Monitor｜機械学習モデルのためのフルマネージドな自動監視](https://aws.amazon.com/jp/blogs/news/amazon-sagemaker-model-monitor-fully-managed-automatic-monitoring-for-your-machine-learning-models/)
* [Amazon SageMaker Operators for Kubernetes のご紹介](https://aws.amazon.com/jp/blogs/news/introducing-amazon-sagemaker-operators-for-kubernetes/)

cf. https://aws-seminar.smktg.jp/public/seminar/view/892

今まで、SageMakerをはじめとしたクラウドサービスの機械学習基盤（GCPのAI Platform, Azure Machine Learning）は  
notebook, モデル管理, デプロイの機能は個別に提供されているものの  
よく見てみると微妙に統合されていなくて（各機能の連携が弱く一元的に管理できない、など）イマイチ使いづらかった印象。

SageMakerはre:invent 2019の発表で頭ひとつ抜きん出た感じがする。  
特に↑の連携という意味だとSageMaker Studioが使いやすくしてくれている。


# なんで機械学習基盤が必要か

https://aws.amazon.com/jp/blogs/news/introducing-amazon-sagemaker-operators-for-kubernetes/
より抜粋。

```
機械学習は、ただモデルを構築すればよいわけではありません。ML ワークフローには、データのソーシングと準備、機械学習モデルの構築、構築したモデルのトレーニングと評価、その本番環境へのデプロイ、本稼働後の継続的なモニタリングというプロセスが含まれます。モジュラー式のフルマネージドサービスである Amazon SageMaker を利用すると、データサイエンティストおよび開発者は、より迅速にモデルの構築、トレーニング、デプロイ、メンテナンスといったタスクを完了することができます。
```

# SageMakerについて

## 無料枠

```
AWS 無料利用枠の一環として、Amazon SageMaker の使用を無料で開始できます。
Amazon SageMaker を使用したことがない場合は、最初の 2 か月間、モデル構築のためのノートブック利用に t2.medium または t3.medium インスタンスを 1 か月あたり 250 時間、トレーニングに m4.xlarge または m5.xlarge インスタンスを 50 時間、リアルタイム推論とバッチ変換用の機械学習モデルのデプロイに m4.xlarge または m5.xlarge インスタンスを追加で 125 時間、無料でご利用いただけます。
無料利用枠は、初めて SageMaker リソースを作成した最初の月から始まります。
```

らしい。


## SageMaker Studio

中身としては単にJupyter Labのカスタマイズ版っぽい。
ただ、SageMakerを利用するうえで必要なモジュールが同じUIに統合されていて必要十分な印象。

### 既存のnotebooksとの違い

基本的には今後はStudioを使っていけば良いはず。

* Jupyter notebook用のインスタンス起動が速い（確かに既存のnotebookでは起動・停止が遅い。起動に数分待たされたりする）
* 協同可能とうたっており、多分ユーザ管理が楽（AWSコンソール上でユーザを追加できたりする）


### SageMaker最初に使うとき

https://qiita.com/SatoshiGachiFujimoto/items/eecb66f5d57cb50324c7
を参考にしてセットアップ

### SageMaker Studioを起動

* 「ユーザーの割り当て」でユーザを追加すると、そのユーザでSageMaker Studioへのリンクが有効になる。
* Studioの起動時、特にインスタンスタイプは指定しなかった。
    + 起動後、Jupyter Labが使えるようになる。
        - 起動直後は、以下な感じ。
        ```
bash-4.2$ free
             total       used       free     shared    buffers     cached
Mem:       3978236    3852820     125416        524       2176    2904620
-/+ buffers/cache:     946024    3032212
Swap:            0          0          0
bash-4.2$ nproc
2
        ```
    + SageMaker notebookは、起動時にインスタンスタイプを指定する必要がある。


### その他

* jupyter lab上のメニューでshutdownをすると接続できなくなってしまった...
    + 30分くらいしたら復旧した。notebookとかも残っていた。
    + [フォーラムで報告した](https://forums.aws.amazon.com/message.jspa?messageID=927816#927816)
