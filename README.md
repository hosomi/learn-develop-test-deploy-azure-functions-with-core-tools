# learn-develop-test-deploy-azure-functions-with-core-tools 

https://docs.microsoft.com/ja-jp/learn/modules/develop-test-deploy-azure-functions-with-core-tools/

## 1. 作業フォルダ作成＆関数の初期化

``コマンドは全て PowerShell です。``

```powershell
mkdir loan-wizard
cd loan-wizard
func init
```

## 2. 関数作成

```powershell
func new
```

トリガーの種類：  
| Http Triger を選択。  

ファンクション名：  
| simple-interest  


## 3. index.json の編集

``index.json``:

```javascript
module.exports = async function(context, req) {
  // Try to grab principal, rate and term from the query string and
  // parse them as numbers
  const principal = parseFloat(req.query.principal);
  const rate = parseFloat(req.query.rate);
  const term = parseFloat(req.query.term);

  if ([principal, rate, term].some(isNaN)) {
    // If any empty or non-numeric values, return a 400 response with an
    // error message
    context.res = {
      status: 400,
      body: "Please supply principal, rate and term in the query string"
    };
  } else {
    // Otherwise set the response body to the product of the three values
    context.res = { body: principal * rate * term };
  }
};
```

## 4. ローカルで関数の実行

```powershell
func start
```

```powershell
loan-wizard> func start

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

Azure Functions Core Tools (3.0.2630 Commit hash: beec61496e1c5de8aa4ba38d1884f7b48233a7ab)
Function Runtime Version: 3.0.13901.0

・・・省略

```

ブラウザで確認：  
引数無し：http://localhost:7071/api/simple-interest  
引数有り：http://localhost:7071/api/simple-interest?principal=5000&rate=.035&term=36  

　  
``func start`` を終了するには ``CTRL + C``

## 5. Azure に発行

リソースグループは既存のものストレージアカウント、ファンクションを作成します。

### 5.1 Azure にログイン

```powershell
az login
```

``ブラウザが開きますのでログインしてください。``


複数サブスクリプションがある場合はログイン後、切り替えてください。

``az account list --output table --all``:  

```powershell
> az account list --output table --all   
Name                    CloudName    SubscriptionId                        State    IsDefault
----------------------  -----------  ------------------------------------  -------  --------
A Subscription          AzureCloud   ***********************************1  Enabled  True
B Subscription          AzureCloud   ***********************************2  Enabled  False
```

切り替えたい SubscriptionId をコピーします。

``az account set --subscription <SubscriptionId>``:  

```powershell
az account set --subscription ***********************************2
```

B Subscription に切り替える例です。  
もう一度一覧を表示すると切り替わっていることが確認できます。  

```powershell
> az account list --output table --all   
Name                    CloudName    SubscriptionId                        State    IsDefault
----------------------  -----------  ------------------------------------  -------  --------
A Subscription          AzureCloud   ***********************************1  Enabled  False
B Subscription          AzureCloud   ***********************************2  Enabled  True
```

### 5.1 ストレージアカウントの作成

```powershell
az storage account create `
  --resource-group "<既存のリソースグループ名>" `
  --name "<一意の被らない名前で英小文字と数字のみの組合せ>" `
  --kind StorageV2 `
  --location centralus
```

### 5.2 ファンクションの作成

```powershell
az functionapp create `
  --resource-group "<既存のリソースグループ名>" `
  --name "<一意の被らない名前で英小文字と数字とのみの組合せ（一部の記号を使えます）>" `
  --storage-account "<前項 5.1 で作成したストレージのリソース名>" `
  --runtime node `
  --consumption-plan-location centralus
```

これで関数の枠のみ作成されます。


### 5.3 関数を発行する

```powershell
func azure functionapp publish <5.2 で作成したファンクションのリソース名>
```

バージョンの互換性で発行が失敗する場合は ``--force`` オプションを付けて実行してみてください。  

```powershell
func azure functionapp publish <5.2 で作成したファンクションのリソース名> --force
```


Invoke url の URL にブラウザからアクセスして確認します。
（引数を同じように変更してローカルで実行した結果と同じか確認します。）  



### 5.4 ライブ メトリック ストリーム

https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-monitoring?tabs=cmd#live-metrics-stream  


``func azure functionapp logstream <5.2 で作成したファンクションのリソース名>``:  



```powershell
> func azure functionapp logstream <5.2 で作成したファンクションのリソース名>
Retrieving Function App...
2020-07-31T03:36:31  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours. Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds). 
2020-07-31T03:36:55.120 [Information] Executing 'Functions.simple-interest' (Reason='This function was programmatically called via the host APIs.', Id=b33f2fc8-3449-4353-a2ba-17d7b0a00915)
2020-07-31T03:36:55.136 [Information] Executed 'Functions.simple-interest' (Succeeded, Id=b33f2fc8-3449-4353-a2ba-17d7b0a00915, Duration=21ms)
2020-07-31T03:36:55.120 [Information] Executing 'Functions.simple-interest' (Reason='This function was programmatically called via the host APIs.', Id=b33f2fc8-3449-4353-a2ba-17d7b0a00915)
2020-07-31T03:36:55.136 [Information] Executed 'Functions.simple-interest' (Succeeded, Id=b33f2fc8-3449-4353-a2ba-17d7b0a00915, Duration=21ms)
```

発行済みの関数をもう一度ブラウザから動作確認するとログが出力される。  


