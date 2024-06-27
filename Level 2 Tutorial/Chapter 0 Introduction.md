# Chapter 1 Introduction
|Writer| Stan.Huang|
|--|--|
|Version| V1.0|
|Release| 20240624|
  
### 概述
在Level 1 Tutorial 的系列文件中，簡單的介紹專有名詞以及EtherCAT的通訊觀念。( 至少OSI 7 Layer 的觀念要會吧 )。

所以現在至少應該知道CoE重要的概念是OD( Object Dictionary )。
OD 的概念是來自CANOpen，但在EtherCAT，透過SyncManager，確保資料的一致性，基於這個變革，在PDO的處理上面，就不是單純的一個Objrct 對應一個Index，而要先經過PDO Assignment、再PDO Mapping。 

在Leve 2 這系列，的目標，是為了解釋應用面會碰到的疑惑，故會用到之前提到的OD 觀念，如果現在還在問` OD 是甚麼可以吃嗎?、SDO為什麼只有Index沒有ばんごう?`的小夥伴們。可能要仔細想想，是不是前面的說明文件，漏看了甚麼。

`應用面會碰到的疑惑`的意思，舉例來說:
**將網路線從Master接到Slave 時**:
1. 通訊是怎麼起來的。
2. 通訊起來後Master怎麼確認Slave的狀態。
3. Master 怎麼透過通訊去控制Slave 的狀態與行為。
4. 發生非預期錯誤時，如何偵錯與排錯。
5. EtherCAT 封包( Frame )，到底做了什麼事，我們可以從中獲得什麼有用資訊。
   
以上五點，都是實際應用中，會出現的問題。如果用到現在都沒有出現類似的疑惑，哪麼有可能，一是你只是單純使用EtherCAT Master的使用者，只要會使用OD 設定與獲取參數就可以了。
第二可能是一個把屎缺推給菜鳥，還假裝自己分配工作很公正的老屁股。
假設是第二種，建議先去臺大醫院看醫生，請他做完腦部斷層掃描後，開治療講幹話的特效藥。
因為有很大的可能，會在之後會對菜鳥說: `這很簡單啊...我以前做過..blah...blah`。
當發生說出這種幹話的時候，再吃藥就來不及了，因為小腦已經取代大腦，會成為自大+自卑的有機生物體，末期(巨嬰症)的時候會說出: 
`你們都是壞人，都不懂我的用心良苦..blah..blah (TAT)`。

---

以下將先從 EtherCAT Device Protocal 中State Machine 開始說明。 這是很重要的觀念，Master 接上Slave 時，是怎麼讓Slave開始動起來的。



