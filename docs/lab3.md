# Technical Lab3

このLabでは、特定の基準に依存したアクションを実行する方法について学びます。  
例えば、ローン申請者の収入やクレジット・スコアに応じてローンを承認したい場合にはルール・ベースでの意思決定を行うことが有用です。  
watsonx Orchestrateはルール・エンジン機能を提供するため、意思決定を実装してスキルとして呼び出すことによりこの判断を行うことが可能です。

## 前提条件
 1. watsonx Orchestrateの環境にアクセスできること。  
 [https://dl.watson-orchestrate.ibm.com/](https://dl.watson-orchestrate.ibm.com/) 　　 
 2. IBM-idを用いてログイン可能であること。
 3. Automationsを用いて自動化を作成する権限があること。(Builder権限)


## watsonx Orchestrateへのアクセス
このセクションでは、watsonx Orchestrateの主な画面構成について学びます。

 1. watsonx Orchestrateにログインします。
 2. IBM-idを入力して、**Continue** ボタンをクリックします。
 3. 複数テナントに所属する場合には、テナント選択画面が表示されるので、適切なテナント名を選択してください。  
![alt text](lab1_images/image.png)

## Automationの作成

1. Automationsをクリックします。もしくは、左上のメニューから**Automations**を選択してもOKです。  
![alt text](lab3_images/image.png)

5. 右上の**Create automations**をクリックします。
![alt text](lab3_images/image-1.png)

6. Automationの作成ダイアログが表示されるので、**あなたのイニシャル_Lending_Services**という名前を入力し、**Create**をクリックしてください。
![alt text](lab3_images/image-2.png)

7. 以下のようなダイアログが表示されます。意思決定、ワークフロー、生成AIに関する処理を実装することが可能です。今回は意思決定に関するスキルを実装するため、**Decision**をクリックします。    
![alt text](lab3_images/image-3.png)

8. 次にDecisionのモデルを選択します。3つのモデルを選択可能ですが、今回はDecision Modelを作成します。Decision ModelはDMN(Decision Model Notation)と呼ばれる標準の形式で意思決定をモデル化することが可能です。  
![alt text](lab3_images/image-4.png)

9. Decisionの名前を指定します。**あなたのイニシャル Personal Loan**という名前を指定して、**Create**ボタンをクリックしてください。
![alt text](lab3_images/image-5.png)

10. 次のようなDMN形式のダイアグラムが表示されます。緑のノード(Inputノード)はルールによって使用される、データを表現します。青いノード(意思決定ノード)は意思決定のステップを表現します。意思決定ノードにはそのステップで実行されるルールが含まれます。それぞれの意思決定ノードは部分的な意思決定の結果を出力します。複雑な意思決定には多くの意思決定ノードが含まれ、一つの意思決定ノードの出力が他の意思決定ノードの入力フローとして動作します。  
![alt text](lab3_images/image-6.png)

## Input ノードの実装
watsonx Orchestrateには、プリビルド・スキルと呼ばれる1000以上の様々なスキルが付属し、すぐに使用することが可能ですが、以下の方法でスキルを追加することも可能です。：

- Open APIの定義ファイル(json/yaml)をインポートする。
- 既存スキル(ワークフローやRPA)のディスバリー
- スキルフローの作成
- Automation Builderによるスキルの実装

このLabでは、OpenAPI定義をインポートしてスキルをカタログに追加する方法について学びます。では、始めましょう！

1. まず[こちらのリンク](https://n28bf9mpmg.execute-api.us-west-2.amazonaws.com/default/hellowatonx)をクリックして、Hello world OpenAPIが利用可能であることを確認してください。 {"message":"Missing Authentication Token"}と表示されればOKです。このAPIはAWS上で動作するシンプルなAPIです。

2. [こちらのリンク](./files/helloworld-watsonx.yaml)を右クリックしてファイルに保存してください。このファイルは先ほどのAPIの定義情報です。

3. notepad, VSCode, vi/vimといったお好みのエディタでファイルを開き編集します。
```
openapi: 3.0.3
info:
  title: YourInitials-helloworld-watsonx
  description: Your Initials Hello world WatsonX
  version: 1.0.0
  x-ibm-annotations: true
  x-ibm-application-name: IBM Watsonx - Training
  x-ibm-application-id: watsonxai-YourInitials-training
  x-ibm-skill-type: imported
  x-ibm-application-icon: <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" 
servers:
  - url: https://n28bf9mpmg.execute-api.us-west-2.amazonaws.com/default
security:
  - passwordGrant: [ ]
paths:
  /hellowatsonx:
    post:
      summary: YourInitials Hello World WatsonX
      operationId: Hello-YourInitials-watsonx
```
4. 今回のハンズオンでは、複数の参加者が同一のファイルを読み込むため、ファイルの中の、x-ibm-application-id, title,summaryとoperationIdをユニークにする必要があります。これらの値にのYourIntialsの部分をあなたのイニシャルに置き換えてください。更新後のファイルは以下のようになるはずです。
```
openapi: 3.0.3
info:
  title: SH-helloworld-watsonx
  description: SH Hello world WatsonX
  version: 1.0.0
  x-ibm-annotations: true
  x-ibm-application-name: IBM Watsonx - Training
  x-ibm-application-id: watsonxai-SH-training
  x-ibm-skill-type: imported
  x-ibm-application-icon: <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" 
servers:
  - url: https://n28bf9mpmg.execute-api.us-west-2.amazonaws.com/default
security:
  - passwordGrant: [ ]
paths:
  /hellowatsonx:
    post:
      summary: SH Hello World WatsonX
      operationId: Hello-SH-watsonx
```

5. メニューからSkills and appsを選択します。  
![alt text](lab1_images/image-8.png)

6. Add Skillsボタンをクリックします。  
![alt text](lab1_images/image-9.png)

7. From a fileを選択してください。  
![alt text](lab1_images/image-11.png)

8. 先ほど保存したファイルをドラッグ＆ドロップするか、クリックして選択してください。
9. ファイルの検証が実施され、問題無ければ以下の様に表示されます。  
![alt text](lab1_images/image-12.png)

10. 右下のNextをクリックしてください。追加するAPIの内容が表示されます。
11. Hello World WatsonXのチェックボックスをチェックし、右下のAddボタンをクリックしてください。  
![alt text](lab1_images/image-13.png)

12. スキル一覧にインポートしたスキルが表示されるので、右側のメニューを展開し、**Enhance this skill**を選択してください。  
![alt text](lab1_images/image-14.png)

13. スキルの詳細が表示されます。スキルのEnhance画面から、スキルの入出力の表示、スキルを呼び出すためのフレーズ、watsonx Orchestrateが次に実行するスキルの提案を行うNext Best Skillsの設定などを行うことが可能です。  
![alt text](lab1_images/image-15.png)

14. **Input**タブをクリックしてください。Inputタブでは、スキルの入力について確認することができます。スキルの入力パラメータに**name**があり、必須のパラメータで無いことが分かります。  
![alt text](lab1_images/image-16.png)

15. **Output**タブをクリックしてください。Outputタブでは、スキルの出力について確認することが可能です。スキルの出力パラメータとして**greeting**が定義されていることが分かります。  
![alt text](lab1_images/image-18.png)

16. **Security**タブをクリックしてください。Securityタブでは、認証情報や、URLを確認することができます。また、接続のテストを行うことができます。  
![alt text](lab1_images/image-19.png)

17. 接続のテストを実行してみましょう。Usernameに test@acme.com、Passwordにtestを入力します。今回使用するAPIについては認証不要のため、どんな値を入力しても問題ありません。

18. Submitボタンをクリックし、Authentication Successfulと表示されることを確認します。  
![alt text](lab1_images/image-20.png)

20. **Phrases**タブをクリックしてください。Phrasesタブでは、スキルを呼び出す際に使用するフレーズを登録します。ユーザーがここに登録した例文にマッチした文章をチャットから入力することでこのスキルが呼び出されます。複数のスキルがマッチした場合にはwatsonx Orchestrateは実行するスキルを確認してきます。いくつかの例文を追加してみてください。  
![alt text](lab1_images/image-21.png)

21. **Next best skills**タブをクリックしてください。このタブでは、スキルを実行した後に次に実行すべきスキルの選択肢として表示される複数のスキルを定義することが可能です。
- Next best skillsは10個まで登録することが可能です。
- Next best skillsに追加した各スキルに対して明示的に変数のマッピングを行うことが可能です。  
![alt text](lab1_images/image-23.png)

22. **Publish**ボタンをクリックして、スキルをPublishしてください。  
![alt text](lab1_images/image-24.png)


## Personal Skillへのスキルの追加
ここまでの作業で、カタログにスキルを追加することができました。次のステップとしてPersonal Skillにカタログからスキルを追加します。

1. 左上のメニューから**Chat**を選択してチャット画面を表示します。  
![alt text](lab1_images/image-25.png)

2. 以下のような画面が表示されるはずです。  
![alt text](lab1_images/image-26.png)

3. 左下のAdd skills from the catalogをクリックします。検索フォームに先ほど作成したスキルの名前を入力してください。  
![alt text](lab1_images/image-27.png)

4. スキルを選択して、add skillをクリックしてスキルを追加します。  
![alt text](lab1_images/image-29.png)

5. 右上の**Connect App**をクリックしてアプリケーションと接続します。username,passwordに任意の値を入力し、Connect appボタンをクリックします。  
![alt text](lab1_images/image-31.png)

6. アプリケーションとの接続が完了しました！
- なお、接続はカタログからではなく、チャットからスキルを呼び出した際に実行することも可能です。  
![alt text](lab1_images/image-32.png)

7. メニューからチャット画面に戻ります。スキルが追加されていることが分かります。  
![alt text](lab1_images/image-33.png)

## スキルの動作確認
カタログから追加したスキルは以下の2つの方法で実行可能です:
- チャット画面下部に表示されているスキルをクリックする
- スキルをEnhanceした際に指定したフレーズ(watsonx Orchestrateがしたいされたフレーズを元に学習するため、完全一致する必要はありません)を入力する

1. チャットの入力欄に、**Hello World WatsonX**と入力してみてください。  
![alt text](lab1_images/image-34.png)

2. 先ほど追加したスキルが呼び出され、入力フォームが表示されます。  
![alt text](lab1_images/image-35.png)

3. 自分の名前を入力し、**Apply**ボタンをクリックしてください。結果が表示されるはずです。  
![alt text](lab1_images/image-36.png)

4. チャットの内容は右上のホウキアイコンをクリックすることで削除することが可能です。  
![alt text](lab1_images/image-37.png)  
![alt text](lab1_images/image-38.png)

## お疲れさまでした！
このハンズオンでは、watsonx Orchestrateにログインし、カスタムスキルを追加、EnhanceしてからPublishし、カタログに追加しました。そしてカタログからスキルを追加し、チャット画面から呼び出しました。　