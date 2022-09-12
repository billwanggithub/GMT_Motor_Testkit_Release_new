###### tags: `GMT TestKit`
# GMT Motor Test Kit 使用手冊
*update: 2022.09.07*

:::spoiler **目錄**
[toc]
:::


---
## ==APP Download Link==
[v0.1.1.2:arrow_left:](https://github.com/billwanggithub/GMT_Motor_Testkit_Release_new/blob/af6635a7fe844a9f562c7dd09a362aa49682ee64/APP/motor_testkit_v0.1.1.2.7z)

---
## ==Features==
* Cycle by Cycle RPM recording
* Integrated Power Supply Control
* Current Meaurement by build-in ADC
* Histogram for startup test result
* Error Capturing on startup test
* Realtime plot of RPM and current
* 
---
## ==Startup test==
### 測試參數定義
#### `Pole Pairs`
>![](https://i.imgur.com/fnBMmBt.png) 提供FG頻率換算RPM
> 
> :bulb:RPM計算公式如下
> 
> $RPM = \frac{FG\ Frequency(Hz) * 60}{Pole\ Pairs}$


#### `Duty On/Off Stuff`
>![](https://i.imgur.com/Dl11QYt.png) ![](https://i.imgur.com/raYEeix.png)
![](https://i.imgur.com/uHWvBxo.png)

#### `Power On/OFF Stuff`
>![](https://i.imgur.com/9zpcNCV.png) ![](https://i.imgur.com/j8vmImd.png)
>![](https://i.imgur.com/5ZqlplM.png)


#### `RPM Limit`
> ![](https://i.imgur.com/EKyjU9n.png)
> ![](https://i.imgur.com/mEK9Bub.png)

#### `Max. 1st FG Time/Start Stall Time`
>
> :bulb:Start Stall Time
啟動時超過Stall Time時,尚未看到第一個FG變化,會停止本次測試跳到下一次測試

>:bulb:Max. 1st FG Time
啟動時若第一個FG變化超過Max. 1st FG Time,不會停止本次測試,若error有勾選會記錄

![](https://i.imgur.com/cyUqiir.png) ![](https://i.imgur.com/TiLXdhE.png)

#### `Running RPM error`
>![](https://i.imgur.com/n0QpK0X.png) ![](https://i.imgur.com/KYS0d4f.png)

#### `Auto Stable Test`

>__Note:__ 有Enable時,達成[穩定測試條件](https://hackmd.io/VTtx3Je1R2-bkXGfu2KUkg?both#%E8%BD%89%E9%80%9F%E7%A9%A9%E5%AE%9A%E6%A2%9D%E4%BB%B6)時會停止本次測試跳到下一次測試
> 
>![](https://i.imgur.com/EzyQMVk.png)
##### 誤差計算方式
> ![](https://i.imgur.com/J2hMtd2.png)
##### 轉速穩定條件

> :bulb: **在設定的時間(Time)內的RPM的平均值與此時間內的最大RPM的誤差(error1)或最小RPM的誤差(error2)均要小於設定值(error)**


#### `Snapshot`
>![](https://i.imgur.com/2nVasXZ.png)
>
>:bulb: 每次測試RPM曲線儲存成png圖檔

---
### Basic Test
![](https://i.imgur.com/7TfHqti.png) ![](https://i.imgur.com/Xl44vz5.png)

---
### Sweep Test
![](https://i.imgur.com/TWn70KX.png) 
- 設定ON/OFF Mode ![](https://i.imgur.com/napFBTa.png) 
- 設定Duty及Power測試參數 ![](https://i.imgur.com/XHmgBh0.png)

:::success
ON/OFF Mode
* Duty
```csharp
foreach (power in power list)
{
    foreach (duty in duty list)
    {
        //Max. Test Time是否超過?
        //Enable Auto Test Enabled => RPM是否穩定?
        //紀錄RPM,電流,電壓,Duty..
    }
}
```
* Voltage
```csharp
foreach (duty in duty list)
{
    foreach (power in power list)
    {
        //Max. Test Time是否超過?
        //Enable Auto Test Enabled => RPM是否穩定?
        //紀錄RPM,電流,電壓,Duty..
    }
}
```
:::

---
### Lookup table Test
* 先做一次 Fancurve test
    * 設定 Sweep type: Duty or Power 
    * 只要勾選UP即可.設定每一步的測試時間(Sampling time )    
    ![](https://i.imgur.com/K8tINN7.png)     
    ![](https://i.imgur.com/hCEapPA.png)
    * 觀察每個step轉速是否穩定,視需要加減Sampling Time
    ![](https://i.imgur.com/vuFzvpM.png)
    * 結果會存成 fancurve***.csv
    ![](https://i.imgur.com/qGSVDZW.png)


* 切換到Startup Test. 勾選 Lookup table
    ![](https://i.imgur.com/bTkGvNa.png)

* 切換到Lookup Table. 載入fan curve table.
    ![](https://i.imgur.com/zNSrK0J.png)
* 設定Final RPM與Target RPM的±誤差範圍
    > ***Target RPM相關測試參數***
        ![](https://i.imgur.com/HrPJDKx.png)
* 點擊測試按鈕
    ![](https://i.imgur.com/qGjQ4xr.png)




---
## ==Fancurve test==
![](https://i.imgur.com/SSGZIJM.png)

### **Sweep Type**
:::success
#### Duty
```csharp
foreach (power in power list)
{
    foreach (duty in duty list)
    {
        Delay(sampling time)
        //紀錄RPM,電流,電壓,Duty..
    }
}
```
#### Voltage
```csharp
foreach (duty in duty list)
{
    foreach (power in power list)
    {
        Delay(sampling time)
        //紀錄RPM,電流,電壓,Duty..
    }
}
```
:::

---
## ==Rolling Test==
![](https://i.imgur.com/f5G9b1k.png)
![](https://i.imgur.com/uJqWdzV.png)

:::info
:bulb: RPM Window Size: RPM視窗內的RPM點數
:::

### Simple
![](https://i.imgur.com/smw37ON.png)

### Duty Jump
![](https://i.imgur.com/Itwdotn.png)

### Voltage Jump
![](https://i.imgur.com/a8zYVGn.png)

---
## ==Others==
:bulb: Startup Test Histogram可儲存成csv格式
![](https://i.imgur.com/obRklLE.png)

:bulb:　使用 CTRL+ALT+Z 截圖到 Clipboard
> 把鼠標移動到你要的圖上 再按CTRL+ALT+Z

:bulb:　Startup Test測試表格相關功能
![](https://i.imgur.com/HPA9EgF.png)



---
## ==Firmware更新==
//TBD

---
## ==TODO==
- 電流紀錄 Option:Buildin ADC or Power Supply
- 雙風扇量測

---

## ==Release Notes==
- v0.1.1.2, 220912
    - 修正rolling jump datalog 同步問題
- v0.1.1.1, 220907
    - 修正rolling duty jump時無data log問題
- v0.1.1.0, 220905
    - 加入在測試前調整PWM   Duty是否輸出的選項
- v0.1.0.11, 220809
    - 加入 rolling duty/voltage datalog
    - 修正 rolling duty/voltage datatable null value問題
- v01010, 220726
     - 修正Rolling mode PWM frequency輸入問題
- v0109, 220725
    - show warning if file is locked when loading startup test log 
    - copy fancurve file to startup test if enable lookup table
- v0108, 220722
    - rpm plot 圖例改成靠上水平排列
- v0107, 220721
    - 使用 power Supply 電流給fancurve/startup datalog表格
    - 修正 final rpm reading 有時候會0問題
    - 使用 CTRL+ALT+Z 截圖到 Clipboard
- v0106, 220720
    - 修正running rpm rising/falling error 顯示問題
    - startup增加error description
    - 修正Normal Trigger 有時候會不見問題
---
Feel free to ping us for questions:
- :mailbox: bill.wang@gmt.com.tw
