#AdwordsAPI初探
    AdWords Query Language 
        - SQL-like language
        - defining reports with an object hierarchy

## AdwordsAPI在做什麼
- 允許app和adwords platform互動，加強管理Adwords的帳號和活動，　　
    能做到的大部分Adword網站可做到的功能
- 大致事項如下
    - 自動化管理帳號
    - 自定產生報表
    - 廣告的決策依據參考資料
    - 和企業自身平台整合功能

![](http://image.slidesharecdn.com/howadwordsmapsintoadwordsapi-150415135752-conversion-gate02/95/how-adwords-ui-maps-into-adwords-api-5-638.jpg?cb=1430129626)    

## AdwordsAPI使用大略
- 註冊
    - 創建 AdWords manager account (和adword帳戶串聯可在access的時候簡化驗證) 
    - 完善資料及聯絡方式
    - 確認是否正常運作token連接狀況
- 使用api呼叫
    - 執行環境參數設定
        - Request a developer token.
            - developer token 通過的話 就算管理帳戶沒連結到adword帳戶也能操作
        - Create test accounts. 
            - 熟悉AdWords web interface
            - 在登入測試帳號時新增的帳戶都都自動設定為測試帳號
            - 測試帳號不影響你進行中的廣告也不用費用，report中無數據
        - Get a client library.
            - JAVA. NET PYTHON PHP PERL RUBY 依選擇開發的語言導入library
        - Set up authentication via OAuth2.
            - 讓自己的app可以使用api
            - OAuth2 client ID etc 身分驗證及歸納專案至Google DEVELOPER
        - 實作自動更新OAuth2 access的機制
    - api areas
        - Account management
        - Campaign management
        - Error handling
        - Optimization
        - Reporting
        - Targeting
        - example
            - 如販賣雨傘，結合氣象資料及設定策略，進行自動調整廣告活動的進行。
    - api call sturcture
        - 透過　Request headers辨識身分　及　Response headers回傳要求報告做溝通
- obj methods services
    - 每個AdWords Account 可以被視為一個Obj
    - 每個AdWords Account 底下可以有很多Campaigns
    - 每個Campaigns 底下可以有很多有很多AdGroup
    - 每個AdGroup 有多個 AdGoupAds & AdGroupCriteria
        - AdGroupAd 分 ads(正投放的廣告)&criterion(廣告觸發的規則)
## Account

- Managing Accounts 
    - CustomerService 
        - 提供取得你的帳戶資訊
    - ManagedCustomerService
        - 創建編輯帳戶
        - 比CustomerService能存取更多欄位
        - 取得帳戶之間的關聯關係
- Budget Order Service(BOS) 
    - api可以存取資訊
    - api可以創建編輯一個budgetOrder
    - api可以把各個Client account的order整併至同一個付款帳號同一份帳單
    - api不能創建或編輯帳號

## Campaigns
- Overview
    - 決定Campaign types (廣告類型出現的領域)
        - api支援的Campaign types
            - Search Network with Display Select
            - Search Network only (備註1)
            - Display Network only (備註2)
            - Shopping
    - 利用api創建一個Campaigns 最好的方式可參考library中Basic Operations
    - 設定一連串相關工作
        - Add a budget.
        - Set a bidding strategy.
        - Create mobile app or Shopping campaigns.
        - Target campaigns based on location.
        - Remarket
            - Remarket to users who have previously visited your site or who are in your CRM databases.
        - Create draft and experimental campaigns to test different settings and scenarios.
- 概述相關項目
    - Campaign Budgets
        - 透過api決定預算分享(移除)給各個活動,
        - 追蹤預算、取得資訊
    - bidding
        - 所有參數透過以下兩個物件設定
            - BiddingStrategyConfiguration object,
            - SharedBiddingStrategy object, 
    - Shopping Campaigns
        - 設定在使用者點擊前提供更多商品細節資訊
            - ex: 在你的商品圖附近添加title price etc.
    - Campaign Drafts and Experiments
        - 依這套機制降低實驗測試活動到正式之間的管理轉移成本(copying data, setup, etc.)
    - Targeting
        - local targeting
            - api允許設定廣告投放(排除)區塊(國家城市地域單位...)
            - 取得區塊報告
        - Generating Targeting Ideas
            - 取出歷史資料，透過特定條件進行評估過濾
        - Remarketing and Audience Targeting
            - 設定條件名單進行在行銷策略
        - Conflicting Negative Keywords and Shared Sets
            - 取出負面、正面關鍵字組，辨認兩者衝突，刪除衝突的負面關鍵字產出報告
- 備註
    - (1)The Search network : Google search result pages, other Google sites like Maps and Shopping, and partnering search sites.
    - (2)The Display network : like YouTube, Blogger, and Gmail, plus thousands of partnering websites across the internet.
    - (3)BiddingStrategyConfiguration object: for standard strategies at the campaign, ad group, and keyword levels.
    - (4)SharedBiddingStrategy object: for portfolio strategies. This object defines a bidding type, scheme, and zero or more bids for the entity.
## Ads
- Overview
    - AdWords ads可以出現在Search network和Display network
    - adtype
        - ExpandedTextAd,textAd,ImageAd,etc.
    - 可使用api
        - 編輯擴充的資訊
        - 自動調整響應式的內容
            - 格式、尺寸etc.
        - Upgraded URLs
        - 檢查ads內容及反應牴觸政策規定
    - 已有template可靈活創建廣告
## Reporting 
- Overview
    - api有2個主要步驟產生報告
        - 創建report definition.
            - 用xml or AWOL格式定義一些參數(report名稱,資訊的種類,etc.)
        - 附上report definition在HTTP POST request並傳送至AdWords server.
