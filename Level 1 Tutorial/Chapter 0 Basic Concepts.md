# Chapter 0 Basic Concepts
### Preface( Points ):
討論EtherCAT是一個很大範圍的項目，涵蓋通訊、工控應用、理論架構。
故，先以參與EtherCAT 的三位角色的，對於使用EtherCAT的心中目標，以實際的角度做為說明的切入點。

- [x]  從終端使用者( 工控設備開發商 )來說
- [x]  從模組( Slave: 工控模組開發商 )來說
- [x]  從EtherCAT Master來說 

#### 從終端使用者( 工控設備開發商 )來說。
心中的目標:
* **通訊**: 設備穩定運作( 不要斷線 )、能保證通訊的`即時性`。
* **操作**: 非預期錯誤可以快速偵錯與排除。
* **持續發展**: 使用上容易入手( 程式易寫、配線容易 )。

故上述三點的目的映射到EtherCAT 的功能特色。
1. `穩定運作( 不要斷線 )、能保證通訊即時性。`
    商用的EtherCAT Master 都提供斷線重連以及Redundancy 的功能，以確保運作時的強健性。

2. `非預期錯誤可以快速偵錯與排除`
    * 商用的EtherCAT Master 都提供了診斷功能以及Log紀錄，可以提供給User 進行排錯。
    <br>
    * EtherCAT 自身提供Error Counter Register，用於排錯。
1. `使用上容易入手`
   * 商用的EtherCAT Master 都提供常用於PLC的 ST 語法以及易於操作的Function Block，以便橋接工控領域原本的開發者到EtherCAT 領域。
  <br>
   * 最簡單的接線方式最就是網路線直接串接，一個串一個，就可以實現多模組( Motor 、IO )的分散式控制。並不需要另外購入有軸數上限的運動控制卡或有點數限制的I/O處理卡。
  <br>
   * 終端使用者只需要模組的功能的啟用與資料的讀取屬於放在哪個暫存器的Index，針對模組的站號( Device ID or Alias Address)，並對Index 讀寫數值，則可進行模組的操作與監測。

因此，EtherCAT 確實是以這樣的目標進行發展的。

#### 從模組( Slave: 工控模組開發商 )來說。
心中的目標:
* **通訊**: 通訊都由ESC晶片搞定，開發人員可專心在模組功能的研發，減少對通訊關注的時間。
* **操作**: 模組功能的MCU與ESC 以 I2C方式通訊，專注在本身開發的功能就可以了。
* **持續發展**: ESC供應商的晶片升級，總體升級。不影響目前專注的本業。
  
因此，有了EtherCAT Slave Controller ( ESC )廠商的幫助，確實是讓工控模組開發商以這樣的目標進行發展。

#### 從EtherCAT Master來說。
心中的目標:
* 終端使用者心中的目標可以達成。
* 模組(Slave)的開發商可以照ETG規範進行開發，不然兩者之間，對於協同合作的行為，不照規範走，容易出問題。
  
因此，EtherCAT Master 的相關人員( RD、AE )都要為了前兩者的心中目標而努力。舉例來說: 斷線了，可能的原因來自以下三點，但發生問題時，就會先問Master端發生什麼事了。Master 首當其衝，因此:

###### Master端就是屎缺。
---
斷線可能的因素(簡單概述):
1. Master => 本身在設計上的架構就爛。
2. Slave => 電路、或選用晶片太爛，功能設計時不照ETG規範走。
3. 環境
   * 大小電配線混雜，沒有接地造成雜訊、通訊線材的品質不佳( 可能性較高 )。<br>
   * 設備開發商活在美索不達米亞的漢摩拉比時期，只能給36~42K，所以碩士年輕人跑光光，喪失開發能力但保留幹話能力的老鯛魚跳下來寫程式，但又不熟悉EtherCAT，還以為就是Ethernet 寫錯字而已。( 造成斷線的可能性較低，有0.87% )。

### EtherCAT Master
上述的重點，簡而言之: 
##### Master端 = 屎缺、碰到的人都衰小。

基於這個客觀條件，加上我不負責任的分析，EtherCAT的推廣速度，不如工控界老牌分散式通訊( ModbusRTU、CANOpen )的箇中原由，就是
很吃重Master端的技術能力跟Supported能力。單靠一個沒經驗的白紙，就要對方在短時間內，上知天文、下肢無力的通曉工控中生硬的理論觀念，不太現實。

  
### The Abilities for using EtherCAT master that need to know first:

(標題英文我想的，文法是錯的，爽就好，勿戰 )

EtherCAT 的觀念並不難懂，一個具有即時性的封包，在Cycle Time 內，封包一去一回，告知Slave 的工作，並讀取各站Slave 的資料，Slave 則是收到資料之後開始各自工作。

在實際應用上( 單純User端的使用 )，只要知道如何設定Cycle Time、同步模式、PDO、SDO即可，並不需要特別的知識。

但如果要排錯、分析封包乃至於原理，對於Supported 單位來說，EtherCAT 難懂的地方在於，在搞懂EtherCAT 之前需先具備以下能力，才有機會避開學習路上相似名詞造成的混淆:

* **需要對 通訊觀念有一定的基礎**。
  * 能理解 OSI 7 Layer
  * Protocal 對應到OSI 7 Layer 的理解力。
* **需要對 Protocal的觀念有一定基礎**。
  * CANOpen。
  * Package Format。
* **需要對 硬體架構方塊圖的閱讀能力**。
  * 例如: Slave ESC Function Block。
  * Hardware register for what。
* **需要對 工控領域有一定的應用經驗**。
  * What Real Time System.
  * Redundency
  * 工控模組( Server Drive、DIO/AIO )。

---  
### 小結:
沒有上述基礎的話就算了，因為本系列文章就是要補足前兩項的觀念所寫的。
希望能對世界和平有所幫助。

後續文章會繞著一個觀念展開，就是通訊。
從通訊開始，直到EtherCAT 封包結束。能把為什麼討論EtherCAT 的人都習慣用火車作舉例。
的這件事可以說清楚。

就完成這一系列供初學者學習的文章。

---

#### Reference:
[ Watasi no keiken desu. ]