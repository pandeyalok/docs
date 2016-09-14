---

copyright:
 years: 2015, 2016

---

# 關於 {{site.data.keyword.mobilepushshort}}
{: #overview-push}
前次更新：2016 年 8 月 16 日
{: .last-updated}

IBM {{site.data.keyword.mobilepushshort}} 是一項服務，可用來將通知傳送給 iOS 及 Android 裝置。通知可以使用標籤，以所有應用程式使用者或一組特定使用者及裝置為目標。您可以管理裝置、標籤及訂閱。您也可以使用 SDK（軟體開發套件）及「具象狀態傳輸 (REST)」應用程式介面 (API) 來進一步開發用戶端應用程式。 

{{site.data.keyword.mobilepushshort}} 也可以當作「Bluemix 專用」服務使用。如需將 {{site.data.keyword.mobilepushshort}} 當作專用服務的相關資訊，請參閱[專用服務](../../dedicated/index.html)。請注意，{{site.data.keyword.mobilepushshort}} 監視標籤不會顯示分析資料。

{{site.data.keyword.mobilepushshort}} Service 現在已啟用 OpenWhisk。如需相關資訊，請參閱 [OpenWhisk](../../openwhisk/index.html)。


## {{site.data.keyword.mobilepushshort}} Service 程序
{: #overview_push_process}

行動用戶端可以訂閱及登錄 {{site.data.keyword.mobilepushshort}} Service。啟動時，行動應用程式會自行登錄及訂閱 {{site.data.keyword.mobilepushshort}} Service。通知會先分派到 Apple Push Notification Service (APNs) 或 Google Cloud Messaging (GCM) 伺服器，然後傳送給已登錄的行動用戶端。

![推送概觀](images/overview.jpg)


###行動應用程式
{: mobile-applications}

啟動時，行動應用程式會自行登錄及訂閱 {{site.data.keyword.mobilepushshort}} Service 以接收通知。

###後端應用程式
{: backend-applications}

後端應用程式可以位於內部部署中，也可以位於公用雲端中。後端應用程式將會使用 {{site.data.keyword.mobilepushshort}} Service，以將環境定義相關通知傳送給行動使用者。不需要後端應用程式，也可以維護與管理用於傳送推送通知的行動裝置及使用者資訊。反之，後端應用程式可以使用 {{site.data.keyword.mobilepushshort}} Service。

###應用程式後端擁有者
{: app-backend-owner}

應用程式後端擁有者會建立可組合 {{site.data.keyword.mobilepushshort}} Service 實例的行動後端應用程式。應用程式後端擁有者也會配置及設定 {{site.data.keyword.mobilepushshort}} Service，以符合使用此服務的後端應用程式以及為 {{site.data.keyword.mobilepushshort}} 目標的行動應用程式。

###{{site.data.keyword.mobilepushshort}} Service
{: push-notification-service}

{{site.data.keyword.mobilepushshort}} Service 會管理用於登錄通知之裝置的所有相關資訊。此服務讓應用程式不必處理將通知傳送給異質行動平台的技術詳細資料，全都在其內進行處理。

###閘道
{: gateways}

行動裝置平台專用雲端服務（例如 Google Cloud Messaging (GCM) 或 Apple Push Notification Service (APNs)）使用 {{site.data.keyword.mobilepushshort}} Service 將通知分派給行動應用程式。

###推送安全
{: push-security}

{{site.data.keyword.mobilepushshort}} API 透過兩種類型的密碼來保護 - i) appSecret ii) clientSecret。'appSecret' 會保護一般由後端應用程式所呼叫的 API，例如，傳送 {{site.data.keyword.mobilepushshort}} 的 API、配置設定的 API。'clientSecret' 會保護一般由行動用戶端應用程式所呼叫的 API。目前，只有一個 API 與使用需要此 'clientSecret' 的關聯 UserId 登錄裝置有關。從行動用戶端呼叫的所有其他 API 都不需要 clientSecret。連結應用程式與 {{site.data.keyword.mobilepushshort}} Service 時，會將 'appSecret' 及 'clientSecret' 配置給每個服務實例。如需如何傳遞密碼以及針對哪些 API 傳遞的相關資訊，請參閱 ReST API 文件。

## {{site.data.keyword.mobilepushshort}} 類型
{: #overview-push-types}

###播送
{: broadcast}

當行動應用程式向 {{site.data.keyword.mobilepushshort}} Service 登錄自己後，就可以開始接收播送。播送通知是訊息，以針對 {{site.data.keyword.mobilepushshort}} Service 安裝及配置應用程式的所有裝置為目標。任何已啟用 {{site.data.keyword.mobilepushshort}} 的應用程式預設都會啟用播送通知。啟用 {{site.data.keyword.mobilepushshort}} Service 的應用程式都會有 Push.ALL 標籤的預先定義訂閱，伺服器會使用此標籤將通知訊息播送給所有裝置。若要傳送使用 Push REST API 的播送通知，請確定在公佈至訊息資源時，"target" 是空的 JSON 檔案。

###標籤型通知
{: tag-based-notifications}

標籤通知是指以所有訂閱特定標籤之裝置為目標的訊息。標籤型通知容許根據主旨區域或主題分段通知。只有在通知是所感興趣的主旨或主題時，通知收件者才能選擇接收通知。因此，標籤型通知提供一種方法來分段收件者。此特性會啟用定義標籤後根據標籤來傳送及接收訊息的能力。訊息只會以訂閱標籤的裝置為目標。您必須先建立應用程式的標籤，並設定標籤訂閱，然後起始標籤型通知。若要傳送使用 REST API 的標籤型通知，請確定在公佈至訊息資源時提供 "tagNames"。

###單點播送通知
{: unicast-notifications}

單點播送通知是指以特定裝置或使用者為目標的訊息。以裝置為目標的單點播送通知不需要任何額外的設定，而且預設會在應用程式啟用 {{site.data.keyword.mobilepushshort}} 時予以啟用。

不過，以使用者為目標的單點播送通知需要：

- 在登錄行動裝置的推送通知時，關聯使用者 ID 與裝置。  

- 傳遞將後端應用程式連結至 {{site.data.keyword.mobilepushshort}} Service 時所配置的 'clientSecret'，以授權這類使用者 ID 登錄。 

一般而言，行動應用程式會先執行鑑別週期，在此鑑別週期，會針對鑑別服務[（例如 Mobile Client Access）](https://console.ng.bluemix.net/docs/services/mobileaccess/index.html)鑑別行動應用程式使用者。鑑別成功時，已鑑別使用者 ID 接著會與 clientSecret 一起傳遞至「推送裝置登錄 API」。clientSecret 的存在只會強制執行「使用者 ID」與行動裝置登錄的已授權關聯。
若要透過 REST API 傳送單點播送通知，請確定在公佈至訊息資源時已提供 deviceId 或 userId。

###平台型通知
{: platform-based-notifications}

可以設定通知的目標以送達特定的裝置平台。例如，可以將通知只傳送給所有 Android 使用者。若要傳送使用 REST API 的平台型通知，請確定在公佈至訊息資源時已提供目標平台。請將平台指定為陣列。支援的平台如下：
* A (Apple)
* G (Google)

## {{site.data.keyword.mobilepushshort}} 訊息大小
{: #push-message-size}

{{site.data.keyword.mobilepushshort}} 訊息的有效負載大小取決於中介。 

###iOS
{: ios-message-size}

若為 iOS 8 以及更新版本，接受的大小上限為 2 KB。Apple Push Notification Service 不會傳送超過此限制的通知。

###Android
{: android-message-size}

Android 平台上沒有任何限制。