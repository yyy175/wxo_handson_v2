# Technical Lab2

このLabでは、watsonxを用いたコンテンツ生成のカスタムスキルと、Outlookでメールを送信するプリビルドスキルを組み合わせて、スキルフローを作成します。

## 前提条件
 1. watsonx Orchestrate の環境にアクセスできること。
 2. IBM-id を用いてログイン可能であること。
 3. スキルの作成、追加を行う権限があること。
 4. Outlook のメールアドレスとパスワード (講師から配布されます)

## コンテンツ生成のスキルをインポートしよう
このセクションでは、watsonx Orchestrate の主な画面構成について学びます。

 1. watsonx Orchestrate にログインします。
 2. スキルを定義した [OpenAPI ファイル](./files/wxOハンズオン_BAM_generate_content.json)をダウンロードします。リンクを**右クリック**して**名前を付けてリンクを保存**を選択すると、ご自身の PC に保存できます。または、ブラウザ上で json ファイルを開いても構いません。
 3. お好みのエディター (VSCode、vi/vim など) を用いてファイルを編集します。
 4. `YourName-IBM BAM` (4行目) を探し、YourName をご自身の名前に変更します (例:山田太郎さんなら、`TaroYamada-IBM BAM`)。同様に、`YourName-generate content skill for wxO Handson` (8行目)、`YourName-generate content` (23行目) も変更します。
 5. ファイルを保存します。
 6. watsonx Orchestrateの画面で**Skills and apps**→**Add skills**を選択します。
 ![alt text](lab2_images/image-1.png)

 7. **From a file**タブを選択します。
 ![alt text](lab2_images/image-2.png)

 8. 先ほど編集した`wxOハンズオン - BAM generate content.json`という名前のファイルをご自身のPCから指定の場所にドラッグアンドドロップします。アップロードされると、watsonx Orchestrateはjsonファイルをインポートする前に検証します。**Next**をクリックします。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_1602_dl.watson-orchestrate.ibm.com.jpeg)

 9. インポートするスキルのチェックボックスにチェックを入れ、**Add**をクリックします。
 ![alt text](lab2_images/image-68.png)

 10. スキルのインポートに成功すると、以下のように表示されます。  
 ![alt text](lab2_images/image-5.png)

 11. Skills and appsのページで、作成したスキルを検索します (例:TaroYamada-generate content)。スキルを公開するために、右端の ⁝ から**Enhance this skill**を選択します。
 ![alt text](lab2_images/image-61.png)

 12. 複数のタブがある画面が表示されます。左端の**Name**タブはスキルの名前を表しています。 (先ほど、jsonファイルで変更した部分です)
 ![alt text](lab2_images/image-69.png)

 13. **Input**タブをクリックします。required (必須) の欄とそうでない欄があることが確認できます。この画面は、インポートしたスキルを実行する際の入力項目を表しています。  
 必須になっている項目の **Edit response** をクリックし、テキストを入力してみましょう。例：`新入社員を歓迎するメールを作成してください。`（スキルの実行時に例文として表示されます。）  
 ![alt text](lab2_images/image-80.png)

 14. 他のタブも同様に動作を確認できます。
    - **Output**タブは、スキルの出力を設定できます。スキルの実行結果を表やテキストの形式で出力することができます。
    - **Security**タブは、スキルを実行するために必要な認証情報を確認できます。
    - **Next Best Skill**タブは、このスキルが使用された後に、次に行うべきスキルとして watsonx Orchestrate が提案するスキルを設定できます。
 15. **Phrases**タブは、チャットからスキルを呼び出すためのフレーズを入力します。多くのフレーズを入力するほど、自然言語からスキルを判断する精度が向上します。
 ![alt text](lab2_images/image-71.png)

 16. フレーズの欄に、`generate content`を入力し**Enter(return)** を押します。  
 **注釈:** フレーズを入力するには**Enter(return)** を押さなければなりません。  
 `generate`,`content`などが含まれるフレーズを自由に追加することも可能です。また、**Auto-generate phrases** をクリックすると、生成AIが自動でフレーズを生成してくれます。  
 最後に、**Publish**を押します。
 ![alt text](lab2_images/image-72.png)

 17. スキルが公開（publish）できたというメッセージが表示されます。  
 ![alt text](lab2_images/image-65.png)

 18. スキルが正常に公開されました。

 19. **オプション:** 先ほど公開したスキルをカタログから探して、ステータスを確認してみましょう。ステータスはどのようになっているでしょうか？

## 新年度のご挨拶メールを生成して、スキルをテストしてみよう
このセクションでは、インポートしたスキルをテストする方法について説明します。 スキルをテストするには、カタログからスキルを追加して実行する必要があります。 このための手順は、以下のとおりです。

 1. 左上にあるメニュー (≣) をクリックし、**チャット**を選択して、チャット画面に移動します。
 ![alt text](lab2_images/image-12.png)

 2. **Add skills from the catalog** を選択し、前のステップで作成したスキルを選択します。  
 ![alt text](lab2_images/image-13.png)

 3. 検索バーで、`generate content`を検索します。 **YourName-generate content skill for wxO Handson** という名前のアプリケーションを開き、先ほど作成した**YourName-generate content** というスキルを開きます。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_162835_dl.watson-orchestrate.ibm.com.jpeg)

 4. スキルを追加する前に、このスキルを接続するための API キーを指定し、スキルを使用するときに必要な出力を設定する必要があります。 この**API キー**は、[IBM Research BAM](https://bam.res.ibm.com/) にアクセスして、APIキーをコピーします。
 ![alt text](lab2_images/スクリーンショット_26-5-2024_161036_bam.res.ibm.com.jpeg)

 5. スキルカタログの画面に戻り、**Connect app** ボタンをクリックします。  
 ![alt text](lab2_images/スクリーンショット_28-5-2024_163038_dl.watson-orchestrate.ibm.com.jpeg)

 6. コピーした**API キー**を貼り付け、APIキーの冒頭に **Bearer + 半角スペース** を追加します。**Connect app** ボタンをクリックします。 指**API キー**が接続されると、**Connected** と表示されます。
 ![alt text](lab2_images/image-73.png)

 7. 次に**Add skill** をクリックします。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_163731_dl.watson-orchestrate.ibm.com.jpeg)

 8. スキルが追加された (`Added`) というメッセージが表示されます。
 ![alt text](lab2_images/image-74.png)

 9. **Chat** 画面に戻ります。追加したスキルが表示されています。  
 ![alt text](lab2_images/スクリーンショット_28-5-2024_164033_dl.watson-orchestrate.ibm.com.jpeg)

 10. スキルをテストするには、先ほどフレーズとして登録した **generate content** をチャット欄に入力します。（該当のスキルをクリックしても実行できます）  
 ![alt text](lab2_images/image-79.png)

 11. 前のステップで説明したように、input は必須項目になっているため、必ず入力する必要があります。  
 input の欄に、`取引先のお客様に対する、新年度のご挨拶メールを作成してください。`と入力します。**Show all fields** は、その他のパラメータを確認し、設定することができます。  
 入力が完了したら、**Apply** をクリックします。  
 ![alt text](lab2_images/スクリーンショット_29-5-2024_121827_dl.watson-orchestrate.ibm.com.jpeg)

 12. スキルが実行されると、進行状況が表示され、`working on it` というメッセージが表示されます。 最後に、新年度のご挨拶メールのコンテンツが生成されます。  
 ![alt text](lab2_images/image-81.png)  

 13. 以上で、メールのコンテンツを生成するテストは完了です。

## カタログからスキルを追加し、Outlookでメールを送ろう
Outlook でメールを送信するスキルは、プリビルドスキルとして既に watsonx Orchestrate にインポートされています。  
このスキルを追加し、Microsoft Outlook に接続してテストする必要があります。  
Outlook に接続してメールを送信できるようにするには資格情報が必要になります。今回の環境では、差出人として、自身のメールアドレスと資格情報を使用することはできません。

 1. **Chat** に移動し **Add skills from the catalog** をクリックするか、左上のメニューに移動して **Skills catalog** を選択します。
 ![alt text](lab2_images/image-22.png)

 2. **Microsoft Outlook** を検索します。
 ![alt text](lab2_images/image-23.png)

 3. **Microsoft Outlook** をクリックし、**Send an email using Outlook** の中で **Add skill** を選択します。 これで、このスキルは **Added** として表示されます。  
 ![alt text](lab2_images/image-24.png)

 4. **Chat** 画面に移動すると (**Menu** -> **Chat**) 、このセクションで追加された 2 つのスキルが表示されます。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_17034_dl.watson-orchestrate.ibm.com.jpeg)

 5. これで、**Send emaill** スキルを追加するセクションは完了です。

## 自身のメールアドレスへメールを送り、スキルをテストしてみよう
このステップでは、スキルを Microsoft Outlook に接続します。 この資格情報は、講師から提供されます。

 1. **Chat** 画面に移動します。

 2. **Send email** スキルを選択します。右下にリンク切れのマークが表示されています。これは、スキルがどのメールシステムにも接続されていないことを意味します。  
 ![alt text](lab2_images/image-26.png)

 3. リンク切れの (緑色のボックスで囲んだ)マークをクリックします。 スキルが Microsoft Outlook に接続されていないことが通知されます。 **Connect app** をクリックして Microsoft Outlook に接続します。 
 ![alt text](lab2_images/image-29.png)

 4. **Type** はデフォルト値の **Non-admin user** を使用し、**Connect app** ボタンをクリックします。 これにより、Outlookに移動します。 講師から提供される資格情報を使用します。 (今回の環境では、IBM のメールアドレスを使用して Outlook に接続することはできません)   
 ![alt text](lab2_images/image-28.png)  

 5. **※パスワードは、ブラウザーやパスワード・マネージャーに保存しないようにご注意ください。**  
 ![alt text](lab2_images/image-27.png)

 6. アプリが接続されたことが確認できます。
 ![alt text](lab2_images/image-30.png)

 7. スキルをテストするために **Chat** 画面に移動します。**Send an email** スキルをクリックします。

 8. **To:** の欄はご自身のメールアドレスを使用します。**Subject** の欄に件名を記入します。（例: `Test send email skill`）**Content** 欄はメール本文を入力します。（例: `Hello and welcome!`）**Apply** ボタンをクリックします。
 ![alt text](lab2_images/image-31.png)

 9. `The email was sent` というメッセージが表示されます。ご自身のメールボックスにメールが送信されているか確認してみてください。

 10. これで **Send email** スキルをテストするステップは完了です。

## 2つのスキルを組み合わせてスキルフローを作成しよう
これまでは、個々のスキルのインポート、追加、テストを行いました。watsonx Orchestrate では、個々のスキルだけでなく、2つ以上のスキルを組み合わせて、スキルフローを作成することができます。 手順は以下のとおりです。

 1. **Menu** -> **Build** -> **Skills** に移動します。

 2. **Add skills** のドロップダウンリスト (∨) から **Create an skill flow** を選択します。  
 ![alt text](lab2_images/image-32.png)  

 3. **鉛筆** アイコンをクリックし、スキルフローに名前を付けます。(例: Generate content and send email - YourName)  
 **YourName** の部分は **TaroYamada** のようにすることをお勧めします。
 ![alt text](lab2_images/image-33.png)

 4. 自身の名前を含めてフローの名前を付けたら、説明 (Description) を追加して保存します。(例: TaroYamada's generate content and send email)  
 ![alt text](lab2_images/スクリーンショット_28-5-2024_17854_dl.watson-orchestrate.ibm.com.jpeg)

 5. スキルを追加するには、**+** をクリックします。**Generate content** を検索して、ご自身の **YourName-generate content skill for wxO Handson** を選択します。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_17160_dl.watson-orchestrate.ibm.com.jpeg)

 6. インポートされたスキルが表示されます。**Add skill** を選択します。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_171929_dl.watson-orchestrate.ibm.com.jpeg)

 7. このスキルがフローに追加されます。  
 ![alt text](lab2_images/image-76.png)

 8. 追加したスキルの後にある **+** をクリックします。 **Send email** を検索します。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_172315_dl.watson-orchestrate.ibm.com-1.jpeg)

 9. **Microsoft Outlook** をクリックすると、Outlook と連携して利用できるすべてのスキルが表示されます。 **Send an email** を探し **Add Skill** をクリックします。  
 ![alt text](lab2_images/image-39.png)

 10. これで、スキルフローが作成されました。全体像は以下のようになります。
 ![alt text](lab2_images/image-77.png)

 11. 生成されたコンテンツをメールで送信するために、1 つ目のスキルの出力を 2 つ目のスキルの入力にマップします。これを行うには、**YourName-generate content** をクリックします。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_172827_dl.watson-orchestrate.ibm.com-1.jpeg)
 Input のタブを開き、パラーメーターを確認します。 他のスキルの出力をこれらのパラメーターにマップすることも可能です。次に、Output のタブを開きます。ここでは **text** の形式で出力されるものが 1 つだけあります。

 12. 次に、**Send an email** をクリックしてinput と Output のパラメーターを確認します。  
 ![alt text](lab2_images/スクリーンショット_28-5-2024_174533_dl.watson-orchestrate.ibm.com.jpeg)
 **Input** タブをクリックします。**body.Content** の欄をクリックすると、**Available Mappings**の中に **YourName-generate content** スキルの出力が表示されます。**YourName-generate content"** をクリックすると、スキルの出力の一覧とコンテンツの型（text, numeric など）が表示されるので、**text** を選択します。☆
 ![alt text](lab2_images/スクリーンショット_28-5-2024_175139_dl.watson-orchestrate.ibm.com.jpeg)  
 **注意:** 必ず**ご自身の名前**が入ったスキルを選択してください。

 13. 他にも、追加で制御できるオプションがあります。(例：エンドユーザーから見えないようにInput欄を非表示にする、など)

 14. **Actions** -> **Save as draft** (2回目以降は **Save**) をクリックして、スキルフローを保存します。 以下のように正常に保存されたことが表示されたら、**Close** をクリックして編集画面を閉じます。  
 ![alt text](lab2_images/image-44.png)

 15. ご自身の名前を用いてスキルを検索します。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_183743_dl.watson-orchestrate.ibm.com.jpeg)

 16. **Enhance this skill** を選択します。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_183955_dl.watson-orchestrate.ibm.com.jpeg)

 17. watsonx Orchestrate がスキルを認識できるように、スキルを呼び出すフレーズを登録してトレーニングする必要があります。  
 **Phrases** タブをクリックし、空の欄に **Generate content and send email to YourName** (**YourName** はご自身の名前に変更してください。例: TaroYamada) を入力し、Enter を押します。（**必ず Enter を押す必要があります**）  
 任意でさらにフレーズを追加し、**Publish** ボタンをクリックします。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_184711_dl.watson-orchestrate.ibm.com.jpeg)

 18. スキルフローを公開すると、スキルが正常に公開されたというメッセージが表示されます。  
 ![alt text](lab2_images/image-78.png)  

 19. 以上でこのステップは完了です。

## メールを生成し、自身のメールアドレスに送信して、スキルをテストしてみよう
最後に、先ほど組み合わせたスキルフローをテストしてみます。

 1. **Menu** -> **Skills Catalog** に移動します。

 2. 先ほど作成したスキルフローが、スキルカタログから見られるようになります。検索欄にご自身の名前を入力します。
 ![alt text](lab2_images/スクリーンショット_28-5-2024_185344_dl.watson-orchestrate.ibm.com.jpeg)

 3. **Skill flows** をクリックすると、作成したスキルフローが表示されます。  
![alt text](lab2_images/スクリーンショット_28-5-2024_185541_dl.watson-orchestrate.ibm.com.jpeg) 
**注釈:** 複数のスキルが表示される場合は、ご自身の名前のスキルを選択してください。（必要に応じて検索機能を使ってください）

 4. **Add skill** をクリックし、自身のスキルセットに追加します。**Added** と表示されたら、**Chat** 画面に移動します。

 5. スキルをテストするには、以下のいずれかを実行します。
    1. スキルをクリックして実行します。
    ![alt text](lab2_images/image-51.png)

    2. スキルを Enhance する際に設定したフレーズの一部を入力します。例：**generate content**
    ![alt text](lab2_images/スクリーンショット_28-5-2024_19225_dl.watson-orchestrate.ibm.com.jpeg)  
    表示された候補から、適切な文章を選択します。もし選択した文章と複数のスキルが合致していたら、watsonx Orchestrate はそれらのスキルを候補として提示します。

 6. 最初は新年度のご挨拶メール文面を生成します。そのために、 **input** に適切な文章を入力します。(例: 取引先のお客様に対する、新年度のご挨拶メールを作成してください。)
 ![alt text](lab2_images/スクリーンショット_29-5-2024_122427_dl.watson-orchestrate.ibm.com.jpeg)

 7. **Apply** ボタンをクリックすると、watsonx Orchetrate は生成された文章を次のスキルの Content の欄にコピーします。宛先にご自身のメールアドレスを入力し、**Apply** ボタンをクリックします。
 ![alt text](lab2_images/スクリーンショット_29-5-2024_12277_dl.watson-orchestrate.ibm.com.jpeg)  
 ![alt text](lab2_images/スクリーンショット_29-5-2024_12295_dl.watson-orchestrate.ibm.com.jpeg)  
 ![alt text](lab2_images/スクリーンショット_29-5-2024_122959_dl.watson-orchestrate.ibm.com.jpeg)  
 
 8. メールが送信されたというメッセージが表示されます。  
 ![alt text](lab2_images/image-55.png)  

 9. メールが送られているか、ご自身のメールボックスを確認してください。

## お疲れさまでした！
以上で Lab2 は完了です。このラボでは、OpenAPI でスキルをインポートし、スキルを公開してテストを行いました。  
次に、プリビルドスキルを自身のスキルセットに追加し、テストを行いました。  
最後に、それら2つのスキルを組み合わせ、1 つ目のスキルの出力を 2 つ目のスキルの入力にマッピングして、スキルフローを作成しました。