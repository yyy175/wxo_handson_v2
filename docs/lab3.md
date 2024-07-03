# Lab4 Automation

このLabでは、特定の基準に依存したアクションを実行する方法について学びます。  
例えば、ローン申請者の収入やクレジット・スコアに応じてローンを承認したい場合にはルール・ベースでの意思決定を行うことが有用です。  
watsonx Orchestrateはルール・エンジン機能を提供するため、意思決定を実装してスキルとして呼び出すことによりこの判断を行うことが可能です。

## 前提条件
 1. watsonx Orchestrateの環境にアクセスできること。  
 [https://dl.watson-orchestrate.ibm.com/](https://dl.watson-orchestrate.ibm.com/) 　　 
 2. IBM-idを用いてログイン可能であること。
 3. Automationsを用いて自動化を作成する権限があること。(Builder権限)


## watsonx Orchestrateへのアクセス

 1. watsonx Orchestrateにログインします。
 2. IBM-idを入力して、**Continue** ボタンをクリックします。
 3. 複数テナントに所属する場合には、テナント選択画面が表示されるので、適切なテナント名を選択してください。  
![alt text](lab1_images/image.png)

## Automationの作成
このセクションでは、Automationを作成します。

1. Skill studioをクリックします。もしくは、左上のメニューから**Skill studio**を選択してもOKです。  
![alt text](image-82.png)

2. 右上にある**Create skill**をクリックし、**Automation**をクリックします。
![alt text](image-83.png)

6. Automationの新規作成のダイアログが表示されるので、**あなたのイニシャル_Lending_Services**という名前を入力し、**Create**をクリックしてください。
![alt text](image-84.png)

7. 以下のようなダイアログが表示されます。意思決定、ワークフロー、生成AIに関する処理を実装することが可能です。今回は意思決定に関するスキルを実装するため、**Decision**をクリックします。    
![alt text](image-85.png)

8. 次にDecisionのモデルを選択します。3つのモデルを選択可能ですが、今回はDecision Modelを作成します。Decision ModelはDMN(Decision Model Notation)と呼ばれる標準の形式で意思決定をモデル化することが可能です。  
![alt text](image-86.png)

9. Decisionの名前を指定します。**あなたのイニシャル Personal Loan**という名前を指定して、**Create**ボタンをクリックしてください。  
![alt text](image-87.png)

10. 次のようなDMN形式のダイアグラムが表示されます。緑のノード(Inputノード)はルールによって使用されるデータを表現します。  青いノード(意思決定ノード)は意思決定のステップを表現します。意思決定ノードにはそのステップで実行されるルールが含まれます。それぞれの意思決定ノードは部分的な意思決定の結果を出力します。複雑な意思決定には多くの意思決定ノードが含まれ、一つの意思決定ノードの出力が他の意思決定ノードの入力フローとして動作します。  
![alt text](image-88.png)

## Input ノードの実装
このセクションでは、Inputノードの定義をしていきます。

1. Inputノードをクリックし、右側に表示される編集欄から、Node nameを**applicant name**に変更してください。  
![alt text](lab3_images/image-7.png)

2. Input Nodeの追加は2通りあります。
    - ①左側にあるパレットからAdd Input Nodeをクリックし、新規にInputノードを追加し、Decisionノードに接続する方法
    - ②Decisionノードにカーソルを持ってくると、decision nodeの上部にパレットが表示されるので、Inputノードをクリックし追加する方法
    ![alt text](lab3_images/image-8.png)  
    ![alt text](image-89.png)

3. 追加されたノードをクリックし、右側の編集欄から、Node nameを**income**に、Output typeを**Integer**にしてください。
![alt text](lab3_images/image-9.png)

(左パレットでInputノードで追加した場合)
incomeノード上にマウス・カーソルを動かすと、アイコンが表示されるので、**Connect to another node**をクリックし、decision nodeに接続してください。(もしくは、DecisionノードのメニューからAdd Inputを選択した場合には自動的に接続されます)  
![alt text](lab3_images/image-10.png)  

5. 1-4の手順を繰り返し、以下の表に従って新規ノードを追加してください。  

|Node name|Node type|
|---------|---------|
|employed|boolean|
|SSN|string|
|loan purpose|string|
|credit score|integer|

6. 意思決定ダイアグラムは以下の様になるはずです。  
![alt text](lab3_images/image-11.png)  


## ルールの作成
次に、意思決定ノードの中で実行されるルールをいくつか作成します。ルールは構文形式と意思決定表の形式で記述することが可能です。(日本語を用いることも可能ですが、現在のバージョンではサポートされていないため英語での記述になります。)

1. 初めに作成するのは、**デフォルト・ルール**です。
デフォルト・ルールは、承認の意思決定の結果を初期化するために用います。
その他のルールには**ローンを却下する場合の条件**を指定します。
デフォルトのルールを作成するには、Decisionノードを選択し、デフォルトではDetailsタブになっているため**Logicタブ**をクリックしてから右上に表示される＋ボタンをクリックし、一番下に表示されるDefault ruleを選択します。  
![alt text](lab3_images/image-12.png)

2. 次のような意思決定エディタが表示されるので、string placeholder(<a string\>と表示されている場所)をクリックしてドロップダウンからstringを選択し、**approved**と入力してください。
![alt text](lab3_images/image-15.png)

3. ルールは以下の様になっているはずです。    
![alt text](lab3_images/image-14.png)

4. 次に、**クレジット・スコアが600以下の申請者の申請はすべて却下するルール**を追加します。Default ruleを選択したときと同様に右上にある+ボタンをクリックして、**Business Rule**を選択します。
ルールの名前を**decline low credit score**として、Select the criteria for your ruleでcredit scoreを選択してから**create**をクリックしてください。   
![alt text](lab3_images/image-17.png)

5. ルールのテンプレートを以下の様に修正します。なお、Ctrl+Space(Macの場合はControl+Space)でコード補完が可能です。  
![alt text](lab3_images/image-18.png)

6. (オプション)例えば、無職の場合(employed is false)にローンを却下するなど、入力された値によってローンが却下されるようないくつかのルールを追加してみてください。
手順は、右上にある+ボタンをクリックして、**Business Rule**を選択します。
ルールの名前を**decline unemployed**として、Select the criteria for your ruleで**employed**にチェックをしてから**create**をクリックしてください。
(デフォルトで、ローンの申請は承認され、ルールは却下する理由を判定することに注意してください。)  
![alt text](image-90.png)
ルールのテンプレートを以下の様に修正します。なお、Ctrl+Space(Macの場合はControl+Space)でコード補完が可能です。
![alt text](lab3_images/image-65.png)

## 意思決定のテスト
作成した意思決定はテスト・データを指定してテストすることができます。

 1. 右上の**Preview**アイコンをクリックしてください。  
 ![alt text](lab3_images/image-21.png)

 2. **Add test data set**をクリックしてテスト・データを追加します。  
 ![alt text](lab3_images/image-22.png)

 3. 左側に各項目の入力画面が表示されるので、＋をクリックして入力してから**pewview**ボタンをクリックして意思決定の出力を確認してください。例えば、creditScoreに500を入力して実行した場合、却下されるはずです。  
![alt text](lab3_images/image-23.png)

## 意思決定サービスのデプロイ
正しく動作することが確認できたら、スキルとして呼び出せるようにサービスをデプロイします。  

1. Back to "XX Personal Loan"をクリックしてください。  
![alt text](lab3_images/image-24.png)

2. **Operations**タブをクリックしてください。スキルとして呼び出されるオペレーションを定義します。  
![alt text](lab3_images/image-25.png)

3. **Create operation**ボタンをクリックしてください。  
![alt text](lab3_images/image-26.png)

4. オペレーションの設定画面が表示されるので、Operation nameを**XXPersonalLoan**、Componentとして XX Personal Loanを選択してから**save**をクリックしてください。この操作によってスキルとして呼び出されるオペレーションが作成されます。  
![alt text](lab3_images/image-27.png)

5. 次にページ右上にある**Share changes**タブをクリックします。  
![alt text](lab3_images/image-28.png)

6. **Share**ボタンをクリックしてください。ダイアログが表示されるので**Share**をクリックします。この操作により、リポジトリに変更が反映されます。  
![alt text](lab3_images/image-29.png)

7. 左上の←をクリックしてLending Serviceのレベルに戻り、**History**タブをクリックしてください。  
![alt text](lab3_images/image-30.png)

8. 右側の+アイコンをクリックし、バージョンを作成します。  
![alt text](lab3_images/image-31.png)

9. バージョンの名前としてnameの欄に**1.0.0**と入力し、**Create**をクリックしてください。   　　
![alt text](lab3_images/image-32.png)

10. バージョンが作成されたら、**Publish**タブをクリックし、作成されたバージョンを展開し、**Publish**をクリックしてください。ダイアログが表示されるので**Publish**をクリックします。  
![alt text](lab3_images/image-33.png)

11. 意思決定サービスがパブリッシュされ、実行可能になりました。このサービスに紐づいたスキルがOrchestrate上に作成されます。左上のメニューから**Skills studio**を選択してから、**Skills**タブをクリックし、スキルの一覧を表示させます。  
![alt text](lab3_images/image-34.png)

12. 作成した意思決定のスキルがReady to Publishの状態で表示されているはずです。
![alt text](lab3_images/image-35.png)

## スキルのエンハンス
スキルのエンハンスを行い、各入力パラメータの表示形式や振る舞いを変更してみます。以下の様に変更します。変更はスキルのエンハンス画面から実施することが可能です。
また、オプションですが、スキルが定義されているOpenAPI定義を直接変更することでより細かい変更が可能です。 

|入力パラメータ|振る舞い|
|-----|---|
|income|必須|
|loan purpose|選択肢|
|credit score|説明を追加|
|SSN|非表示|
|employed|default value:true|


1. スキルの右側のメニューを展開し、**Enhance this skill**を選択します。  
![alt text](lab3_images/image-36.png)

2. **Input**タブをクリックし、incomeの**Required**にチェックを入れてください。  
![alt text](lab3_images/image-37.png)

3. loanPurposeの**Options**を選択し、**medical emergency**,**consolidate debt**,**home improvement**の3つのOptionを設定してください。  
![alt text](lab3_images/image-39.png)

4. creditScoreを選択し説明に**Provide the Experian credit score**を入力してください。  
![alt text](lab3_images/image-40.png)

5. **Publish**をクリックして変更を保存します。
![alt text](lab3_images/image-41.png)

## (オプション)openAPIで直接編集し、細かな変更を行う

5. **Save as draft**をクリックして変更を保存します。  
![alt text](lab3_images/image-41.png)

6. スキル一覧画面に戻り、**Export this skill**を選択し、スキルをエクスポートし、ローカルにjsonファイルを保存します。  
![alt text](lab3_images/image-42.png)

7. メモ帳やエディタなどで保存したファイルを開きます。(実際のファイルは改行が含まれません、必要に応じてお使いのエディタでフォーマットしてください。)  各項目はx-ibmプロパティによって設定されます。x-ibmプロパティの詳細については[Understanding x-properties](https://www.ibm.com/docs/en/watson-orchestrate?topic=skills-understanding-x-properties)を参照してください。

8. **employed**は以下の様に設定されています。**,"default": "true"**となるようにファイルを編集してください。（カンマを含めることを忘れないでください） 
```
    "employed": {
                "type": "boolean",
                "x-ibm-order": 3,
                "x-ibm-multiline": "false",
                "default": "true"
                },
```
9. 同様にSSNの部分を修正し、以下の様に **,"x-ibm-show": "false"**を追加してください。
```
    "SSN": {
            "type": "string",
            "x-ibm-order": 4,
            "x-ibm-multiline": "false",
            "x-ibm-show": "false"
            },
```

10. ファイルを保存します。

11. Skills and appsの画面で**Add skills**をクリックします。  
![alt text](lab3_images/image-43.png)

12. **From a file**をクリックし、先ほど保存したファイルをドラッグ＆ドロップするか、クリックして選択してください。  
![alt text](lab3_images/image-44.png)

13. ファイルの検証が正しく行われたことを確認し**Next**をクリックします。エラーが表示された場合には、正しく編集されていることを確認してください。  
![alt text](lab3_images/image-45.png)

14. 既に存在するスキルをインポートするため、警告が表示されますが、スキルのチェックボックスにチェックをいれ、**Add**をクリックしてスキルを上書きします。  
![alt text](lab3_images/image-46.png)

15. スキルの右側のメニューから**Enhance this skill**をクリックしてからスキルを**Publish**してください。  
![alt text](lab3_images/image-47.png)

## 作成したローン審査のスキルを使用してみよう！

16. 左側のメニューから**Skill catalog**を選択します。  
![alt text](lab3_images/image-48.png)

17. **あなたのイニシャル_Lending_Service**検索欄に記入して検索し選択し、**Add**をクリックしてスキルを追加します。
![alt text](lab3_images/image-49.png)  
![alt text](lab3_images/image-50.png)

18. 左側のメニューからChatを表示し、チャットの入力欄に、**あなたのイニシャルPersonalLoan**と入力してみてください。 スキルのフォームが表示され、先ほど行ったカスタマイズが正しく行われていることを確認します。 
![alt text](lab3_images/image-52.png)

19. 値を入力し、正しい結果が返ってくることを確認してください。

## お疲れさまでした！
このハンズオンでは、Automation Builderを用いて意思決定サービスを実装し、スキルの入力フォームをカスタマイズしてから、チャット画面から呼び出しました。意思決定サービスを用いることで、AIが苦手な根拠のある正確な判断を実行することができます。　

## さらに オプション　意思決定表の実装
ルールを記述する際には今回のハンズオンで実施したような構文形式のルールの他に、意思決定表による実装も可能です。意思決定表はIf-Thenの組み合わせを表形式で表現するため、多数の条件を表形式で管理したい場合などに有効です。また、エクセルの表などをコピー＆ペーストすることも可能なため、より業務ユーザーが管理しやすいというメリットもあります。このオプションでは、先ほど実装した意思決定に、意思決定表形式の新たなルールを追加します。

1. Automation Builderで編集するDecisionのDiagramを開きます。

2. Diagram上で**Decision**をクリックし、右側のプロパティ画面より、Logicタブをクリックし、**+**アイコンをクリックしてください。  
![alt text](lab3_images/image-53.png)

3. 意思決定表を作成するため、**Decision table**を選択してください。  
![alt text](lab3_images/image-54.png)

4. 今回は、収入(income)に応じた条件をテーブルとして定義してみましょう。そこで、Nameとして"Income table"という名前を入力し、使用する条件として**income**を選択し、Createをクリックしてください。  
![alt text](lab3_images/image-55.png)

5. 次のようなテーブルが表示されます。incomeのminとmaxがカラムとして入力可能になっており、それらの条件を満たす場合のDecisionの値をDecision列に記入することが可能です。  
![alt text](lab3_images/image-56.png)

6. income列にカーソルを合わせると、次の様に、このカラムで定義されている条件文を確認することが可能です。  
![alt text](lab3_images/image-57.png)

7. 条件列のルールは、変更することも可能です。income列のヘッダーを右クリックし、**Define column**を選択してください。以下のようなダイアログが表示されます。ここで、条件をルール・言語を用いて記述することが可能ですが、今回は修正を行わないため、**Cancel**をクリックして閉じてください。   
![alt text](lab3_images/image-58.png)

8. アクション列のルールも確認してみましょう。アクション列のヘッダーにカーソルを合わせると、カラムに入力した値がdecisionにセットされるという事が分かります。アクション・ルールも先ほどと同様に右クリックして編集することも可能です。意思決定表は条件列がAND条件で評価され、trueの場合にアクション列に定義されたアクションが実行されます。各条件行は個別に評価されます。   
![alt text](lab3_images/image-59.png)

9. 次に、incomeが0から10000の場合はdeclinedとなるルールを追加してみましょう。1行目のminに**0**,maxに**10000**,Decisionに"declined"を入力してください。  
![alt text](lab3_images/image-66.png)

9. 次にincomeが10000以上の場合にはapproveとなるルールを追加してみましょう。maxに大きな値を入れることも可能ですが、表の記入形式を変更することも可能です。2行目を右クリックして、**change operator** > **≧**を選択してください。  
![alt text](lab3_images/image-61.png)

10. 条件列に**10000**を、アクション列に**approved**を入力してください。  
![alt text](lab3_images/image-67.png)

11. 作成したルールのテストをしてみましょう。画面右のプレビューアイコンをクリックしてテスト画面を開き、creditScoreが高く、employedがtrueであっても、incomeが10000以下の場合にローンが却下されることを確認してください。
![alt text](lab3_images/image-68.png)