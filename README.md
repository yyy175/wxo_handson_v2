# WXOHandson

watsonx Orchestrateのハンズオンのためのリポジトリです。
[MkDocs](https://www.mkdocs.org/)と[material for MkDocs](https://squidfunk.github.io/mkdocs-material/)を使用しています。  
ビルド、デプロイを行うためには上記ツールをインストールすることが必要です。編集だけならVSCodeがあれば十分です。  
生成されたサイトは[https://pages.github.ibm.com/ba-techsales-jp/WXOHandson](https://pages.github.ibm.com/ba-techsales-jp/WXOHandson)にデプロイされます。


## 環境構築

### Gitのインストール

### MkDocsのインストール
 1. [こちら](https://www.mkdocs.org/user-guide/installation/)を参照してMkDocsをインストールします。Pythonのインストール後、以下のコマンドでインストール可能です。  
 ```
 pip install --upgrade pip
 python get-pip.py
 pip install mkdocs
 ```
 2. 以下のコマンドでインストールの確認が可能です。
 ```
 $ mkdocs --version
 mkdocs, version 1.2.0 from /usr/local/lib/python3.8/site-packages/mkdocs (Python 3.8)
 ```

### Material for MkDocsのインストール
以下のコマンドでMaterial for MkDocsをインストールします。
```
pip install mkdocs-material
```

## ビルド
```
mkdocs build
```

## github pagesへのデプロイ
以下のコマンドで、直接git hub pageへデプロイ可能です。gh-deployブランチにPushされたものが公開されます。  
```
mkdocs gh-deploy
```

## その他
 - 文字列の後に改行したい場合には文末にスペースを2つ入力してください。
