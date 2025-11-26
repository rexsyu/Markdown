graph TD
    %% 定義樣式
    classDef startend fill:#f9f,stroke:#333,stroke-width:2px;
    classDef proc fill:#e1f5fe,stroke:#0277bd,stroke-width:2px;
    classDef decision fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;
    classDef database fill:#e0f2f1,stroke:#00695c,stroke-width:2px;

    Start((程式開始)):::startend --> CheckData{是否有傳入<br>錯誤資料?}:::decision
    
    CheckData -- 否 (無資料) --> End((結束)):::startend
    CheckData -- 是 (有資料) --> QueryDB[查詢資料庫<br>當日最大單號]:::database
    
    QueryDB --> CheckDate{今日是否<br>已有單號?}:::decision
    
    CheckDate -- 否 (當日首筆) --> GenFirst[產生新單號<br>日期 + 0001]:::proc
    CheckDate -- 是 (已有單號) --> GenNext[產生新單號<br>日期 + 序號往上加 1]:::proc
    
    GenFirst --> SaveHead[預先存檔單號<br>防止號碼重複]:::database
    GenNext --> SaveHead
    
    SaveHead --> LoopData[逐筆整理資料<br>填入單號與詳細內容]:::proc
    
    LoopData --> SaveFinal[整批寫入資料庫<br>完成存檔]:::database
    
    SaveFinal --> End
