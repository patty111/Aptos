# Aptos 

[![hackmd-github-sync-badge](https://hackmd.io/Af2-Uhx-RT-PfGL_hhjakQ/badge)](https://hackmd.io/Af2-Uhx-RT-PfGL_hhjakQ)

### 環境及套件介紹
#### <font color="blue">VScode</font>
比較其他開發環境
* 延伸模組
* 多種語言
* Emmet（糾正錯誤、快速編寫...）

#### <font color="blue">Linux作業系統</font>
* 相同等級作業系統 Window, MacOs 等等...
* 開放原始碼作業系統（允許任何人自由地使用、複製、研究及以任何方式來改動軟體）
* 更好的穩定性及完善的網絡功能

#### <font color="blue">WSL（Windows 子系統 Linux 版）</font>
* 在 VS Code 使用 WSL 作為完整開發環境
* 可讓開發人員直接執行且不需修改 GNU/Linux 環境 (包括大部分的命令列工具、公用程式和應用程式)
* WSL2提供與WSL1相同的使用者體驗，並增加檔案系統效能、新增完整的系統呼叫相容性
* 在 WSL 2 上開始使用 Docker 遠端容器 : https://learn.microsoft.com/zh-tw/windows/wsl/tutorials/wsl-containers

#### <font color="blue">Docker</font>
1.特點及優點
* 不同作業系統上實現虛擬化，可以利用系統執行不同程式
* 使用Linux構建
* 快速部屬：幾行指令就可啟用服務
* 啟動速度快、效能高
* 節省硬碟空間

2.差異（圖源：AWS）
* VM:虛擬出整個硬體環境CPU、作業系統、Memory Size，並打包在VM Image中
* Docker:透過Docker Engine 來管理和配置映像(image)並產生容器(Container)
![](https://i.imgur.com/O871HLh.png =500x250)
3.比較
![](https://i.imgur.com/2MTIV1T.png =500x150)

#### <font color="blue">SDK(軟件開發工具包) 及 API(應用程序協議接口 or 應用程式介面)</font> 
* Software Development Kit (SDK) 可理解為把功能包裝好的軟件包
* Application Programming Interface (API) 允許兩個應用程序相互通信的軟件中介
* 可將SDK比喻為杯子，API比喻為吸管

#### <font color="blue">Aptos Typescript SDK 及 Aptos REST API</font>
* RESTful API 也稱為REST API
* Representational State Transfer (REST) 為對API施加條件的一種軟體架構，遵循 REST 架構風格的 API 稱為 REST API 。 
* Aptos Node API 是客戶端應用程序與 Aptos 區塊鏈交互的 RESTful(REST) API。
* Typescript/Python/Rust SDK

#### <font color="green">環境搭建</font>
1. 下載Docker
2. 下載Ubuntu，使用此終端機視窗遠端操作VScode
3. 在VScode擴充套件安裝Remote Development (除了包含Remote - SSH 和 Dev Containers 擴充功能，還有WSL 擴充功能)
4. 確認Remote Development已下載好讓Ubuntu開啟時可使用遠端
5. 確認Docker在VScode左側，並且確認Docker已連線(需開啟)

### 第二篇 
開箱第一個範例程式碼
範例程式做到5件事情
1.初始化REST跟Aptos水龍頭
2.在本地創造兩個帳號 Alice 跟 Bob
3.從水龍頭給 Alice 100000000 個token
4.Alice 轉1000個token給 Bob 由 Alice 承擔手續費(54100個Token)
5.Alice 再轉1000個token給 Bob 一樣由 Alice 承擔手續費(54100個Token)


### move語言  
1.Move語言出現前是什麼樣子？
* move語言由來-Libra blockchain
* 注重資產管理
* aptos、sui等鏈

2.move語言有什麼特性？
* 強型別語言、著重安全性
* 以太坊常見攻擊out of bounds access、數據洩露等
* 解決辦法強制執行訪問的範圍限制
* 關鍵特性為resource type(借用linear logic概念)，「resource只能使用一次，使用後即消耗」
* move resource特性「不會無故消失與複製」及「只適用交易移動」

3.move與solidity資產的表現形式？
* solidity-ERC20帳本形式
* move由物件實體包裹資產


4.move與solidity blockchain global state的差別？
* solidity由contract address儲存。
* move則是由address分別儲存資產與實作邏輯的模組。

5.move module與script

* script：動作包裝成數個步驟開放給其他用戶執行。

* script程式碼：transfer（withdraw+deposit）

```
script{
	use Sender::Coins;
	
	fun transfer(acc:singer,recipient:address,amount){
		let coins= Coins::withdraw(&acc,amount);
		Coins::deposit(recipient,coins);
	}

}

```
* module：是存儲於鏈上類似智能合約的程式碼，協助部署智能合約或補齊功能。

* module程式碼
1. 部署module
1. 與別人的module、api互動
1. 如何定義與處理resource資產
1. 如何操作resource資產
```

//1.
module Sender::Coins{

//2.
    use Std::Signer;
//3.
    struct Coins has store{val:u64}
    
    struct Balance has key{
		coins:Coins
    }
//4.
    public fun mint (val:u64)Coins{
	let new_coin = Coin{val}
	    new_coin
    }

}

```

https://drive.google.com/file/d/1eBJooLLfa2OhwDtD2GkYmeytuw2Vp2I2/view?usp=sharing


### 智能合約  
#### 智能合約是甚麼?
- 區塊鏈上運行的程式
- 可以分開<font color="blue">智能</font>和<font color="green">合約</font>進行理解

<font color="green">合約</font>
: 雙方處理加密貨幣的協議(規則)

<font color="blue">智能</font>
: 由區塊鏈(Aptos)管理和實現加密貨幣合約的功能
#### 智能合約的特色?
- 沒有第三方擔任合約監測者
- 合約自動化
- 合約權限最大化
- 去中心化
- 透明
- 不可逆轉
#### 智能合約和一般程式的差異?
- 整合金流容易
- 發佈時需要手續費
- 發佈後不能修改
- 不需耍伺服器維持
#### 智能合約的優點?
- 成本低
- 交易效率高
- 更能發揮合約的作用
- 安全性高
#### 智能合約的缺點?
- 不易修改
- 難上手
- 沒法律規管
- ~~交易風險大~~(但Aptos的特色是安全)
### 第3篇（附上aptos cli 簡介)  
##### 使用move module 進入aptos智能合約開發的第一步
* 編譯器aptos CLI
* 在aptos上建立一個帳號
* 編譯和測試智能合約
* 發佈智能合約
* 和合約互動
---
#### CLI
*用來編譯move語言，來建立合約*

因為Docker Image裡已經有Aptos CLI所以我們可以直接跳過下載這個步驟
#### 建立帳號
1. 先將Docker Desktop開啟
2. 啟動aptos開發環境
```
docker run --rm -it maxmilian/aptos-sdk:latest
```
3. 初始化

```
aptos init
```
4. 新帳號devnet 會先空投 500000 Octas 代幣到這個帳號中
```
aptos account fund-with-faucet --account default
```
#### 編譯和測試智能合約
編譯
```
cd aptos-core/aptos-move/move-examples/hello_blockchain
aptos move compile --named-addresses hello_blockchain=default
```
測試
```
aptos move test --named-addresses hello_blockchain=default
```
#### 發佈智能合約
```
aptos move publish --named-addresses hello_blockchain=default
```
#### 寫入資料到智能合約
透過set_message函式
```
aptos move run \
  --function-id 'default::message::set_message' \
  --args 'string:hello, blockchain'
```

### 第3篇原始碼分析        [source code](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/move-examples/hello_blockchain/sources/hello_blockchain.move)
#### struct 宣告
`MessageHolder`儲存了 $message$ 訊息----也就是合約內容，和一個紀錄 $message$ 是否被修改的事件
`MessageChangeEvent` 用來記錄 $message$ 訊息改變前與後的內容。並經由`emit_event()` 發送通知回傳。 
```move=
    struct MessageHolder has key {
        message: string::String,
        message_change_events: event::EventHandle<MessageChangeEvent>,
    }
    //<:!:resource
    struct MessageChangeEvent has drop, store {
        from_message: string::String,
        to_message: string::String,
    }
```

#### 函式

傳入一個地址，並return存在該地址上`MessageHolder`中的 $message$
```move!
public fun get_message(addr: address): string::String acquires MessageHolder
``` 
---

傳入一個 $signer$ 參數和一個 $string$ 參數。$signer$ 如果沒有`MessageHolder`，則會建一個新的 `MessageHolder`並設定$message$訊息。如已存在`MessageHolder`，則會更新 $message$ 並發送`MessageChangeEvent`
```move!
public entry fun set_message(account: signer, message: string::String) acquires MessageHolder
```
---
用以測試 `set_message()`功能，傳入一個 $signer$ 參數，為 $signer$ 建立一個測試帳號。先設定裡面的 $message$ 內容再用 `get_message()` 檢查是否成功，如失敗則跳出錯誤訊息。

```move!
public entry fun sender_can_set_message(account: signer) acquires MessageHolder
```
---




### 影片  

套件及環境介紹
https://www.youtube.com/watch?v=AavfzWoO0MI

第二篇
https://youtu.be/tvkGs0puA80

Move語言
https://youtu.be/Jsv13PDrBLY

智能合約
https://youtu.be/g9dEXP41CWk

第三篇
https://youtu.be/M0dUNR5KPJ0

原始碼分析
https://youtu.be/hJo36Z7C_zk
