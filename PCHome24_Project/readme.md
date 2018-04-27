# 簡介
目標是進行每日爬蟲  
紀錄PCHome24H商城的商品價格 觀察價格波動  
所觀察的商品為 **DDR4桌上型 筆記型 8G及16G 記憶體**  

對以下4個網頁進行爬蟲
* [DDR4 桌上型 8G][1]
* [DDR4 桌上型 16G][2]
* [DDR4 筆記型 8G][3]
* [DDR4 筆記型 16G][4]

[1]:https://24h.pchome.com.tw/store/DRAC5L
[2]:https://24h.pchome.com.tw/store/DRAC6X
[3]:https://24h.pchome.com.tw/store/DRAC6Q
[4]:https://24h.pchome.com.tw/store/DRAC78

# 實作過程
-   **階段1**  
    原先預計使用pyquery套件進行爬蟲  
    實作時發現其PCHome網站 非靜態網站 pyquery套件無法爬取內容
  
    於是使用selenium套件來獲得載入動態後的DOM文件  
    再將其DOM文件套入pyquery做網頁爬蟲

-   **階段2**  
    使用regular expression 進行商品過濾  
    主要選取的品牌為 金士頓 美光 創見 威剛 宇瞻  

-   **階段3**  
    爬蟲後 讀取前一天紀錄 其記錄以csv檔案儲存  
    第一次爬蟲沒有建立先前的紀錄檔案 因此讀取時會發生錯誤  
    使用try except 進行例外處理  

-   **階段4**  
    得到想要的商品資訊後 使用sorted配合key參數  
    撰寫排序函數 將商品依照品牌 做簡易排序  
    最後將排序後的檔案寫入csv檔儲存

    注意在WIN微軟的環境時 讀或寫csv檔  
    請在open時加入參數 `newline=''`  
    不然可能會因為CRLF的問題 造成錯誤寫入  

    ```python
    open("Price_Record.csv",'w',newline='')
    ```

- **Final**  
    最後 在jupyter notebook的介面下  
    點選左上角 file 選download as  
    將其另存新檔為 .py  

    下載套件 pyinstaller  

        $ pip install pyinstaller

    安裝完成後  
    在CLI(command-line interface)或其他命令提示字元輸入以下指令  
    若沒有加入參數-F 則無法包裝為單一執行檔

        $ pyinstaller -F PCHome24_Project.py

    若希望執行檔有好看的圖示  
    將 .py 與 .ico 檔案放在同一個資料夾

        $ pyinstaller -F --icon=my.ico xxx.py

    將其編譯為exe執行檔之後  
    配合WIN的執行工具 每日排程  
    自動進行網頁爬蟲

    **注意**
    當我們要使用windows工作排程器執行python程式時  
    如果有涉及到輸入、輸出等與路徑有關的問題時  
    一定要額外指定程式的開始位置  
    不然windows工作排程器會以其所在位置`c:\windows\system32\`作為起始位置

    ref:[windows工作排程器執行python程式](https://beanobody.blogspot.tw/2017/05/windowspython.html?showComment=1524828732205)


