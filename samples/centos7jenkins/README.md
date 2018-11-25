# docker-compse-samples
* CentOS 7.x
* PHP 7.1.x

## jenkins sample 
* phpcs
* phpmd

### jenkins access
```
http://127.0.0.1:8080/
```

### jenkins install plugins
* 初回アクセス時のセットアップで*Install suggested plugins*を選択 
* Phing plugin
* Checkstyle Plug-in
* PMD Plug-in

### ジョブ設定
* フリースタイル・プロジェクトのビルド
* ソースコード管理
    - リポジトリURL：https://hogehoge
    - 認証情報：必要に応じて
    - ビルドするブランチ：*/master
* ビルド
    - シェルの実行 (phpcsやphpmdの結果ファイル出力先を作成する)
        - シェルスクリプト:mkdir -p ${WORKSPACE}/reports
    - Phingの呼び出し
        - ビルドファイル:/settings/phing/build.xml
* ビルド後の処理
    - Checkstyle警告の集計
        - 集計するファイル:reports/phpcs.xml
    - PMD警告の集計
        - 集計するファイル:reports/phpmd.xml
