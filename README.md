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
[2020/07/31 2:33:47] Building host: startup suppressed: 'False', configuration suppressed: 'False', startup operation id: '43050b64-2648-4dde-acab-36715fdcb685'
[2020/07/31 2:33:47] Reading host configuration file 'C:\work\temp\loan-wizard\host.json'
[2020/07/31 2:33:47] Host configuration file read:
[2020/07/31 2:33:47] {
[2020/07/31 2:33:47]   "version": "2.0",
[2020/07/31 2:33:47]   "logging": {
[2020/07/31 2:33:47]     "applicationInsights": {
[2020/07/31 2:33:47]       "samplingSettings": {
[2020/07/31 2:33:47]         "isEnabled": true,
[2020/07/31 2:33:47]         "excludedTypes": "Request"
[2020/07/31 2:33:47]       }
[2020/07/31 2:33:47]     }
[2020/07/31 2:33:47]   },
[2020/07/31 2:33:47]   "extensionBundle": {
[2020/07/31 2:33:47]     "id": "Microsoft.Azure.Functions.ExtensionBundle",
[2020/07/31 2:33:47]     "version": "[1.*, 2.0.0)"
[2020/07/31 2:33:47]   }
[2020/07/31 2:33:47] }
[2020/07/31 2:33:48] FUNCTIONS_WORKER_RUNTIME set to node. Skipping WorkerConfig for language:java
[2020/07/31 2:33:48] FUNCTIONS_WORKER_RUNTIME set to node. Skipping WorkerConfig for language:powershell
[2020/07/31 2:33:48] FUNCTIONS_WORKER_RUNTIME set to node. Skipping WorkerConfig for language:python
[2020/07/31 2:33:48] Loading functions metadata
[2020/07/31 2:33:48] Reading functions metadata
[2020/07/31 2:33:48] 1 functions found
[2020/07/31 2:33:48] 1 functions loaded
[2020/07/31 2:33:48] Looking for extension bundle Microsoft.Azure.Functions.ExtensionBundle at C:\Users\kiyot\AppData\Local\Temp\Functions\ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle
[2020/07/31 2:33:48] Found a matching extension bundle at C:\Users\kiyot\AppData\Local\Temp\Functions\ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle\1.3.0
[2020/07/31 2:33:48] Fetching information on versions of extension bundle Microsoft.Azure.Functions.ExtensionBundle available on https://functionscdn.azureedge.net/public/ExtensionBundles/Microsoft.Azure.Functions.ExtensionBundle/index.json
[2020/07/31 2:33:48] Skipping bundle download since it already exists at path C:\Users\kiyot\AppData\Local\Temp\Functions\ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle\1.3.0
[2020/07/31 2:33:48] Loading extension bundle from C:\Users\kiyot\AppData\Local\Temp\Functions\ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle\1.3.0
[2020/07/31 2:33:48] FUNCTIONS_WORKER_RUNTIME set to node. Skipping WorkerConfig for language:java
[2020/07/31 2:33:48] FUNCTIONS_WORKER_RUNTIME set to node. Skipping WorkerConfig for language:powershell
[2020/07/31 2:33:48] FUNCTIONS_WORKER_RUNTIME set to node. Skipping WorkerConfig for language:python
[2020/07/31 2:33:49] Initializing Warmup Extension.
[2020/07/31 2:33:49] Initializing Host. OperationId: '43050b64-2648-4dde-acab-36715fdcb685'.
[2020/07/31 2:33:49] Host initialization: ConsecutiveErrors=0, StartupCount=1, OperationId=43050b64-2648-4dde-acab-36715fdcb685
[2020/07/31 2:33:49] LoggerFilterOptions
[2020/07/31 2:33:49] {
[2020/07/31 2:33:49]   "MinLevel": "None",
[2020/07/31 2:33:49]   "Rules": [
[2020/07/31 2:33:49]     {
[2020/07/31 2:33:49]       "ProviderName": null,
[2020/07/31 2:33:49]       "CategoryName": null,
[2020/07/31 2:33:49]       "LogLevel": null,
[2020/07/31 2:33:49]       "Filter": "<AddFilter>b__0"
[2020/07/31 2:33:49]     },
[2020/07/31 2:33:49]     {
[2020/07/31 2:33:49]       "ProviderName": "Microsoft.Azure.WebJobs.Script.WebHost.Diagnostics.SystemLoggerProvider",
[2020/07/31 2:33:49]       "CategoryName": null,
[2020/07/31 2:33:49]       "LogLevel": "None",
[2020/07/31 2:33:49]       "Filter": null
[2020/07/31 2:33:49]     },
[2020/07/31 2:33:49]     {
[2020/07/31 2:33:49]       "ProviderName": "Microsoft.Azure.WebJobs.Script.WebHost.Diagnostics.SystemLoggerProvider",
[2020/07/31 2:33:49]       "CategoryName": null,
[2020/07/31 2:33:49]       "LogLevel": null,
[2020/07/31 2:33:49]       "Filter": "<AddFilter>b__0"
[2020/07/31 2:33:49]     }
[2020/07/31 2:33:49]   ]
[2020/07/31 2:33:49] }
[2020/07/31 2:33:49] FunctionResultAggregatorOptions
[2020/07/31 2:33:49] {
[2020/07/31 2:33:49]   "BatchSize": 1000,
[2020/07/31 2:33:49]   "FlushTimeout": "00:00:30",
[2020/07/31 2:33:49]   "IsEnabled": true
[2020/07/31 2:33:49] }
[2020/07/31 2:33:49] SingletonOptions
[2020/07/31 2:33:49] {
[2020/07/31 2:33:49]   "LockPeriod": "00:00:15",
[2020/07/31 2:33:49]   "ListenerLockPeriod": "00:00:15",
[2020/07/31 2:33:49]   "LockAcquisitionTimeout": "10675199.02:48:05.4775807",
[2020/07/31 2:33:49]   "LockAcquisitionPollingInterval": "00:00:05",
[2020/07/31 2:33:49]   "ListenerLockRecoveryPollingInterval": "00:01:00"
[2020/07/31 2:33:49] }
[2020/07/31 2:33:49] HttpOptions
[2020/07/31 2:33:49] {
[2020/07/31 2:33:49]   "DynamicThrottlesEnabled": false,
[2020/07/31 2:33:49]   "MaxConcurrentRequests": -1,
[2020/07/31 2:33:49]   "MaxOutstandingRequests": -1,
[2020/07/31 2:33:49]   "RoutePrefix": "api"
[2020/07/31 2:33:49] }
[2020/07/31 2:33:49] Starting JobHost
[2020/07/31 2:33:49] Starting Host (HostId=desktope69dh4o-1627058275, InstanceId=1e7aa778-ff1d-43a2-9580-3955ab93102f, Version=3.0.13901.0, ProcessId=18352, AppDomainId=1, InDebugMode=False, InDiagnosticMode=False, FunctionsExtensionVersion=(null))
[2020/07/31 2:33:49] Loading functions metadata
[2020/07/31 2:33:49] 1 functions loaded
[2020/07/31 2:33:49] Starting worker process:node  "C:\Users\kiyot\AppData\Roaming\npm\node_modules\azure-functions-core-tools\bin\workers\node\dist/src/nodejsWorker.js" --host 127.0.0.1 --port 52599 --workerId a5cf738b-4514-4de4-b4fc-68dfb68a35f1 --requestId 1ee1adcb-58d7-4f71-a5ca-1224c67a5320 --grpcMaxMessageLength 2147483647
[2020/07/31 2:33:49] node process with Id=11284 started
[2020/07/31 2:33:49] Generating 1 job function(s)
[2020/07/31 2:33:49] Found the following functions:
[2020/07/31 2:33:49] Host.Functions.simple-interest
[2020/07/31 2:33:49]
[2020/07/31 2:33:49] Initializing function HTTP routes
[2020/07/31 2:33:49] Mapped function route 'api/simple-interest' [get,post] to 'simple-interest'
[2020/07/31 2:33:49]
[2020/07/31 2:33:49] Host initialized (127ms)
[2020/07/31 2:33:49] Host started (141ms)
[2020/07/31 2:33:49] Job host started
Hosting environment: Production
Content root path: C:\work\temp\loan-wizard
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        simple-interest: [GET,POST] http://localhost:7071/api/simple-interest

[2020/07/31 2:33:49] Worker a5cf738b-4514-4de4-b4fc-68dfb68a35f1 connecting on 127.0.0.1:52599
[2020/07/31 2:33:57] Executing HTTP request: {
[2020/07/31 2:33:57]   "requestId": "87cba19a-8c14-44e5-a7d0-af390f69b35a",
[2020/07/31 2:33:57]   "method": "GET",
[2020/07/31 2:33:57]   "uri": "/api/simple-interest"
[2020/07/31 2:33:57] }
[2020/07/31 2:33:57] Executing 'Functions.simple-interest' (Reason='This function was programmatically called via the host APIs.', Id=4e6f5192-5ccb-4344-b07c-7d0afa906ab2)
[2020/07/31 2:33:58] Executed 'Functions.simple-interest' (Succeeded, Id=4e6f5192-5ccb-4344-b07c-7d0afa906ab2)
[2020/07/31 2:33:58] Executed HTTP request: {
[2020/07/31 2:33:58]   "requestId": "87cba19a-8c14-44e5-a7d0-af390f69b35a",
[2020/07/31 2:33:58]   "method": "GET",
[2020/07/31 2:33:58]   "uri": "/api/simple-interest",
[2020/07/31 2:33:58]   "identities": [
[2020/07/31 2:33:58]     {
[2020/07/31 2:33:58]       "type": "WebJobsAuthLevel",
[2020/07/31 2:33:58]       "level": "Admin"
[2020/07/31 2:33:58]     }
[2020/07/31 2:33:58]   ],
[2020/07/31 2:33:58]   "status": 400,
[2020/07/31 2:33:58]   "duration": 834
[2020/07/31 2:33:58] }
[2020/07/31 2:33:58] Executing HTTP request: {
[2020/07/31 2:33:58]   "requestId": "5b44cbfd-563c-479e-b7cf-24b0bd5401da",
[2020/07/31 2:33:58]   "method": "GET",
[2020/07/31 2:33:58]   "uri": "/favicon.ico"
[2020/07/31 2:33:58] }
[2020/07/31 2:33:58] Executed HTTP request: {
[2020/07/31 2:33:58]   "requestId": "5b44cbfd-563c-479e-b7cf-24b0bd5401da",
[2020/07/31 2:33:58]   "method": "GET",
[2020/07/31 2:33:58]   "uri": "/favicon.ico",
[2020/07/31 2:33:58]   "identities": [],
[2020/07/31 2:33:58]   "status": 404,
[2020/07/31 2:33:58]   "duration": 153
[2020/07/31 2:33:58] }
[2020/07/31 2:34:53] Executing HTTP request: {
[2020/07/31 2:34:53]   "requestId": "9c3014e9-be32-4bab-8e08-b3e118363f48",
[2020/07/31 2:34:53]   "method": "GET",
[2020/07/31 2:34:53]   "uri": "/api/simple-interest"
[2020/07/31 2:34:53] }
[2020/07/31 2:34:53] Executing 'Functions.simple-interest' (Reason='This function was programmatically called via the host APIs.', Id=824b6d88-d5b1-452f-88f1-ba1677d3d0b2)
[2020/07/31 2:34:53] Executed 'Functions.simple-interest' (Succeeded, Id=824b6d88-d5b1-452f-88f1-ba1677d3d0b2)
[2020/07/31 2:34:53] Executed HTTP request: {
[2020/07/31 2:34:53]   "requestId": "9c3014e9-be32-4bab-8e08-b3e118363f48",
[2020/07/31 2:34:53]   "method": "GET",
[2020/07/31 2:34:53]   "uri": "/api/simple-interest",
[2020/07/31 2:34:53]   "identities": [
[2020/07/31 2:34:53]     {
[2020/07/31 2:34:53]       "type": "WebJobsAuthLevel",
[2020/07/31 2:34:53]       "level": "Admin"
[2020/07/31 2:34:53]     }
[2020/07/31 2:34:53]   ],
[2020/07/31 2:34:53]   "status": 400,
[2020/07/31 2:34:53]   "duration": 22
[2020/07/31 2:34:53] }
[2020/07/31 2:35:12] Executing HTTP request: {
[2020/07/31 2:35:12]   "requestId": "1395fba2-6268-40e5-a923-2d8bfddd0b9c",
[2020/07/31 2:35:12]   "method": "GET",
[2020/07/31 2:35:12]   "uri": "/api/simple-interest"
[2020/07/31 2:35:12] }
[2020/07/31 2:35:12] Executing 'Functions.simple-interest' (Reason='This function was programmatically called via the host APIs.', Id=ec008ced-865e-4ef1-b3f0-119700165edb)
[2020/07/31 2:35:12] Executed 'Functions.simple-interest' (Succeeded, Id=ec008ced-865e-4ef1-b3f0-119700165edb)
[2020/07/31 2:35:12] Executed HTTP request: {
[2020/07/31 2:35:12]   "requestId": "1395fba2-6268-40e5-a923-2d8bfddd0b9c",
[2020/07/31 2:35:12]   "method": "GET",
[2020/07/31 2:35:12]   "uri": "/api/simple-interest",
[2020/07/31 2:35:12]   "identities": [
[2020/07/31 2:35:12]     {
[2020/07/31 2:35:12]       "type": "WebJobsAuthLevel",
[2020/07/31 2:35:12]       "level": "Admin"
[2020/07/31 2:35:12]     }
[2020/07/31 2:35:12]   ],
[2020/07/31 2:35:12]   "status": 400,
[2020/07/31 2:35:12]   "duration": 44
[2020/07/31 2:35:12] }
[2020/07/31 2:35:27] Executing HTTP request: {
[2020/07/31 2:35:27]   "requestId": "7e440029-546a-4378-bb78-30b704e18c4a",
[2020/07/31 2:35:27]   "method": "GET",
[2020/07/31 2:35:27]   "uri": "/api/simple-interest"
[2020/07/31 2:35:27] }
[2020/07/31 2:35:27] Executing 'Functions.simple-interest' (Reason='This function was programmatically called via the host APIs.', Id=7eb560c6-9e69-4c7f-a17d-a524a5a7d0b1)
[2020/07/31 2:35:27] Executed 'Functions.simple-interest' (Succeeded, Id=7eb560c6-9e69-4c7f-a17d-a524a5a7d0b1)
[2020/07/31 2:35:27] Executed HTTP request: {
[2020/07/31 2:35:27]   "requestId": "7e440029-546a-4378-bb78-30b704e18c4a",
[2020/07/31 2:35:27]   "method": "GET",
[2020/07/31 2:35:27]   "uri": "/api/simple-interest",
[2020/07/31 2:35:27]   "identities": [
[2020/07/31 2:35:27]     {
[2020/07/31 2:35:27]       "type": "WebJobsAuthLevel",
[2020/07/31 2:35:27]       "level": "Admin"
[2020/07/31 2:35:27]     }
[2020/07/31 2:35:27]   ],
[2020/07/31 2:35:27]   "status": 200,
[2020/07/31 2:35:27]   "duration": 40
[2020/07/31 2:35:27] }
[2020/07/31 2:36:09] Executing HTTP request: {
[2020/07/31 2:36:09]   "requestId": "a7b1fb44-9163-4c9c-ae82-af6e58c8afd7",
[2020/07/31 2:36:09]   "uri": "/api/simple-interest"
[2020/07/31 2:36:09] }
[2020/07/31 2:36:09] Executing 'Functions.simple-interest' (Reason='This function was programmatically called via the host APIs.', Id=22cf8200-f972-4ce3-9865-e9146c57a4c2)
[2020/07/31 2:36:09] Executed 'Functions.simple-interest' (Succeeded, Id=22cf8200-f972-4ce3-9865-e9146c57a4c2)
[2020/07/31 2:36:09] Executed HTTP request: {
[2020/07/31 2:36:09]   "requestId": "a7b1fb44-9163-4c9c-ae82-af6e58c8afd7",
[2020/07/31 2:36:09]   "method": "GET",
[2020/07/31 2:36:09]   "uri": "/api/simple-interest",
[2020/07/31 2:36:09]   "identities": [
[2020/07/31 2:36:09]     {
[2020/07/31 2:36:09]       "type": "WebJobsAuthLevel",
[2020/07/31 2:36:09]       "level": "Admin"
[2020/07/31 2:36:09]     }
[2020/07/31 2:36:09]   ],
[2020/07/31 2:36:09]   "status": 200,
[2020/07/31 2:36:09]   "duration": 19
[2020/07/31 2:36:09] }
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


