# Technical Lab2

このLabでは、watsonxを用いたコンテンツ生成のカスタムスキルと、Outlookでメールを送信するプリビルドスキルを組み合わせて、スキルフローを作成します。

## 前提条件
 1. watsonx Orchestrateの環境にアクセスできること。
 2. IBM-idを用いてログイン可能であること。
 3. スキルの作成、追加を行う権限があること。
 4. watsonx のAPIキー (講師から配布されます)
 5. Outlook のemail IDと資格情報 (講師から配布されます)

## STEP1:コンテンツ生成のスキルをインポートしよう
このセクションでは、watsonx Orchestrateの主な画面構成について学びます。

 1. watsonx Orchestrateにログインします。
 2. watsonx-skill-fileをダウンロードします。リンクを**右クリック**して**名前を付けてリンクを保存**を選択すると、ご自身のPCに保存できます。または、ブラウザ上でjsonファイルを開いても構いません。
 3. お好みのエディター (VSCode、vi/vimなど) を用いてファイルを編集します。
 4. `YourName - watsonx skill for WxO` (4行目) を探し、YourName をご自身のイニシャルに変更します (例:山田太郎さんなら、`TY - watsonx sill for WxO`)。同様に、`Generate Content (YourName)` (22行目) も変更します。
 5. ファイルを保存します。
 6. watsonx Orchestrateの画面で**Skills and apps**→**Add skills**を選択します。
 ![alt text](lab2_images/image-1.png)

 7. **From a file**タブを選択します。
 ![alt text](lab2_images/image-2.png)

 8. 先ほど編集した`basic_watsonx1.json`という名前のファイルをご自身のPCから指定の場所にドラッグアンドドロップします。アップロードされると、watsonx Orchestrateはjsonファイルをインポートする前に検証します。**Next**をクリックします。
 ![alt text](lab2_images/image-3.png)

 9. インポートするスキルのチェックボックスにチェックを入れ、**Add**をクリックします。
 ![alt text](lab2_images/image-4.png)

 10. スキルのインポートに成功すると、以下のように表示されます。
 ![alt text](lab2_images/image-5.png)

 11. Skills and appsのページで、作成したスキルを検索します (例:Generate content (TY) )。スキルを公開するために、右端の ⁝ から**Enhance this skill**を選択します。
 ![alt text](lab2_images/image-6.png)

 12. 複数のタブがある画面が表示されます。左端の**Name**タブはスキルの名前を表しています (先ほどjsonファイルで変更した部分です)。
 ![alt text](lab2_images/image-7.png)

 13. **Input**タブをクリックします。required (必須) になっている欄となっていない欄があることが確認できます。
 ![alt text](lab2_images/image-8.png)

 14. 他のタブも同様に動作を確認できます。
 * **Output**タブは、スキルの出力を設定できます。スキルの実行結果をテキストや表の形式で出力することができます。。
 * **Security**タブは、スキルを実行するために必要な認証情報を設定できます。
 * **Next Best Skill**タブは、このスキルが使用された後に、次に行うべきスキルとしてwatsonx Orchestrate が提案するスキルを設定します。
 15. **Phrases**タブは、チャットからスキルを呼び出すためのフレーズを入力します。多くのフレーズを入力するほど、自然言語からスキルを判断する精度が向上します。
![alt text](lab2_images/image-9.png)

 16. フレーズのテキスト欄に、`generate content`を入力し**Enter(return)** を押します。
 **Note:** フレーズを入力するには**Enter(return)** を押さなければなりません。
 `content`,`test`などのフレーズを追加することも可能です。
 そして、**Publish**を押します。
 ![alt text](lab2_images/image-10.png)

 17. スキルが公開（publish）できたというメッセージが表示されます。
 ![alt text](lab2_images/image-11.png)

 18. スキルが正常に公開されました。

 19. **補足:** 先ほど公開したスキルをカタログから探して、ステータスを確認してみましょう。ステータスはどのようになっているでしょうか？ _____________

## Step2:新入社員向けの歓迎メールを生成して、スキルをテストしてみよう
このセクションでは、インポートしたスキルをテストする方法について説明します。 スキルをテストするには、カタログからスキルを追加して実行する必要があります。 このための手順は、以下のとおりです。

 1. 左上にあるメニュー (≣) をクリックし、**チャット**を選択して、チャット画面に移動します。
 ![alt text](lab2_images/image-12.png)

 2. **Add skills from the catalog**を選択し、前のステップで作成したスキルを選択します。
 ![alt text](lab2_images/image-13.png)

 3. 検索バーで、`generate content`を検索します。 先ほど作成したスキルをクリックします。**(名前)- WxO Bootcamp- watsonx skill for WxO**
 ![alt text](lab2_images/image-14.png)

 4. スキルを追加する前に、このスキルを接続するための API キーを指定し、スキルを使用するときに必要な出力を設定する必要があります。 この**API キー**は、セッション中に講師から提供されます。

 5. **Connect app**ボタンをクリックします。
 ![alt text](lab2_images/image-15.png)

 6. 提供された**API キー**を追加し、**Connect app**ボタンをクリックします。 指定された**API キー**が有効な場合は、画面に正常な通知が表示されます。
 ![alt text](lab2_images/image-16.png)

 7. 次に**Add skill**をクリックします。
 ![alt text](lab2_images/image-17.png)

 8. スキルが追加された (`Added`) というメッセージが表示されます。
 ![alt text](lab2_images/image-18.png)

 9. **Chat**画面に戻ります。これで、スキルが表示されます。
 ![alt text](lab2_images/image-19.png)

 10. スキルをテストするには、スキルをクリックします。 

 11. 前のステップで説明したように、input は必須フィールドになっているため入力する必要があります。 
 
 12. `create welcome email to new hires`という語句を入力します。
　![alt text](lab2_images/image-20.png)

 13. スキル実行の一環として、進行状況が視覚的に表示され、「..」というメッセージが表示されます。 最後に、新入社員を歓迎する E メールのコンテンツを生成します。
 ![alt text](lab2_images/image-21.png)

 14. 歓迎メールのコンテンツ生成をテストが完了しました。

## Step3:カタログからスキルを追加し、Outlookでメールを送ろう
Outlook でメールを送信するスキルは既にインポートされています。 このスキルを追加し、Microsoft Outlook に接続してテストする必要があります。 Outlook に接続してメールで送信できるようにするには資格情報が必要になります。 差出人として自身のメールアドレスと資格情報を使用することはできません。

 1. **Chat** に移動し **Add skills from the catalog** をクリックするか、左上のメニューに移動して **Skills catalog** を選択します。
 ![alt text](lab2_images/image-22.png)

 2. **Microsoft Outlook** を検索します。
 ![alt text](lab2_images/image-23.png)

 3. **Microsoft Outlook**をクリックし、**Send an email using Outlook** の中で**Add skill**を選択します。 これで、このスキルは**Added**として表示されます。
 ![alt text](lab2_images/image-24.png)

 4. **Chat**画面に移動すると (**Menu** -> **Chat**) 、このセクションで追加された 2 つのスキルが表示されます。
 ![alt text](lab2_images/image-25.png)

 5. これで、**Send emaill** スキルを追加するセクションは完了です。

## Step4:自身のメールアドレスへメールを送り、スキルをテストしてみよう
このステップでは、スキルを Microsoft Outlook に接続します。 この資格情報は、講師から提供されます。

 1. **Chat**画面に移動します。

 2. **Send email**スキルを選択します。右下にリンク切れのマークが表示されています。これは、スキルがどの E メール・システムにも接続されていないことを意味します。
 ![alt text](lab2_images/image-26.png)

 3. リンク切れの (緑色のボックスで囲んだ)マークをクリックします。 スキルが Microsoft Outlook に接続されていないことが通知されます。 **Connect app** をクリックして Microsoft Outlook に接続します。 
 ![alt text](lab2_images/image-29.png)

 4. **Type** にデフォルト値の **Non-admin user** を使用し、**Connect app** ボタンをクリックします。 これにより、Outlookに移動します。 講師から提供される資格情報を使用します (IBM のメールアドレスを使用して Outlook に接続することはできません) 。
 ![alt text](lab2_images/image-28.png)

 5. **!!! パスワードをブラウザーやパスワード・マネージャーに保存しないでください !!!**
 ![alt text](lab2_images/image-27.png)

 6. アプリが接続されたことが確認できます。
 ![alt text](lab2_images/image-30.png)

 7. スキルをテストするために **Chat** 画面に移動します。**Send an email** スキルをクリックします。

 8. **To:** の欄はご自身のメールアドレスを使用します。件名を **Subject** の欄に記入します。（例: `Test send email skill`）**Content** 欄はメール本文を入力します。（例: `Hello and welcome!`）**Apply** ボタンをクリックします。
 ![alt text](lab2_images/image-31.png)

 9. **The email was sent** というメッセージが表示されます。ご自身のメールボックスにメールが送信されているか確認してみてください。

 10. これで **Send email** スキルをテストするステップは完了です。

## Step5:2つのスキルを組み合わせてスキルフローを作成しよう
これまでは、個々のスキルをインポートし、追加し、テストしました。watsonx Orchestrate では、個々のスキルだけでなく、2つ以上のスキルを組み合わせて、スキルフローを作成することができます。 手順は以下のとおりです。

 1. **Menu** -> **BUild** -> **Skills** に移動します。

 2. **Ass skills** のドロップダウンリスト () から **Create an skill flow** を選択します。
 ![alt text](lab2_images/image-32.png)

 3. スキルフローに名前を付けるため、**鉛筆** アイコンをクリックします。例:`Generate content and send emai - YourName`、**YourName** は **TaroYamada** のようにすることをお勧めします。
 ![alt text](lab2_images/image-33.png)

 4. 名前を付け (フローの名前に自分の名前を含めて)、説明 (Description) を追加して保存します。
 ![alt text](lab2_images/image-34.png)

 5. スキルを追加するには、**+** をクリックします。**Generate content** を検索して、ご自身の **WxO Bootcamp - watsonx besic skill** を選択します。
 ![alt text](lab2_images/image-35.png)

 6. インポートさたスキルが表示されます。**Add skill** を選択します。
 ![alt text](lab2_images/image-36.png)

 7. このスキルがフローに追加されます。
 ![alt text](lab2_images/image-37.png)

 8. 追加したスキルの後にある **+** をクリックします。 **Send email** を検索します。
 ![alt text](lab2_images/image-38.png)

 9. **Microsoft Outlook** をクリックすると、**Microsoft Outlook** の下にグループ化されたすべてのスキルが表示されます。 **Send an email** の中の **Add Skill** をクリックします。
 ![alt text](lab2_images/image-39.png)

 10. これで、スキル・フローが作成されました。 以下のようになります。
 ![alt text](lab2_images/image-40.png)

 11. 生成されたコンテンツをメールの受信者に送信するために、最初のスキルの出力を 2 番目のスキルの入力にマップします。これを行うには、**Generate content (YourName)** をクリックします。
 ![alt text](lab2_images/image-41.png)
 Inputのパラメーターを確認してください。 他のスキルの出力をこれらのパラメーターにマップできます。 ここでは **text** の形式で出力されるものが 1 つあります。

 12. 次に、**Send an email** をクリックしてinput と OUtput のパラメーターを確認します。
 ![alt text](lab2_images/image-42.png)
 **INput** タブをクリックしてください。**body.Content** の欄をクリックすると、
 ![alt text](lab2_images/image-43.png)
 **注意:**

 13. 

 14. 
 ![alt text](lab2_images/image-44.png)

 15. 
 ![alt text](lab2_images/image-45.png)

 16. 
 ![alt text](lab2_images/image-46.png)

 17. 
 ![alt text](lab2_images/image-47.png)

 18. 
 ![alt text](lab2_images/image-48.png)

 19. 以上でこのステップは完了です。

## Step6:メールを生成し、自身のメールアドレスに送信して、スキルをテストしてみよう

