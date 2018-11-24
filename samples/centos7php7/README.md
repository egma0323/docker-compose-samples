# docker-compse-samples
* CentOS 7.x
* Apache 2.4.6
* mysql 5.7
* PHP 7.1.x

## browser access
```
http://127.0.0.1/index.php
https://127.0.0.1/index.php
```

## mysql access
```
PS C:\> mysql -h localhost -P 33306 -u root -p
Enter password: password
```

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
* ビルド
    - シェルの実行 (phpcsやphpmdの結果ファイル出力先を作成する)
        - シェルスクリプト:mkdir -p ${WORKSPACE}/reports
    - Phingの呼び出し
        - ビルドファイル:/var/www/html/jenkins/phing/build.xml
* ビルド後の処理
    - Checkstyle警告の集計
        - 集計するファイル:reports/phpcs.xml
    - PMD警告の集計
        - 集計するファイル:reports/phpmd.xml
