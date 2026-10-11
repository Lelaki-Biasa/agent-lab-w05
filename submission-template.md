# My lab evidence / 我的實作紀錄

Use a group code, not real names or student IDs in shared files. / 共用檔只寫組別代碼，不寫姓名或學號。

- Group code / 組別：G-01
- Tool / 工具：Antigravity (Gemini 3.6 Flash)
- Route / 路線：individual 個人
- Tasks completed / 完成題目：A, B, C, D
- Material / 素材：NDHU classroom tasks 東華課堂版
- For original-pack work: task number, author/source link and version / 原版實作：題號、作者來源連結與版本：不適用（使用東華課堂版）
- My role and what I checked / 我的角色與實際檢查：擔任操作者與驗收者。在 A 題檢查原檔未刪除與 SHA-256 雜湊一致性；在 B 題實際測試條件篩選邊界情況、中英文切換與無網路離線運作；在 C 題檢查資料清洗空列移除與負數/缺失保留；在 D 題主動審查並退回包含不可逆刪除與擅自補值的危險計畫。

## Scope and plan / 範圍與計畫

Allowed input and output folders / 可讀取與輸出的資料夾：
限制於 `practice/` 內各子資料夾，嚴禁碰觸外部目錄：
- A: 僅讀取 `practice/01-club-files/input`，僅輸出至 `practice/01-club-files/output`
- B: 僅讀取 `practice/02-campus-picker/activities.json`，僅輸出至 `practice/02-campus-picker/output/index.html`
- C: 僅讀取 `practice/03-equipment/equipment.json`，僅輸出至 `practice/03-equipment/output/`
- D: 僅閱讀 `practice/04-review/bad-plan.txt`，產出 `practice/04-review/my-rejection.md`

What I asked for / 原始需求：
要求 Agent 在執行任何寫入前提出明確計畫，保留所有原始檔案不刪除不覆蓋。A 題依內容而非檔名字尾分類並產出 manifest.json 與 report.md；B 題製作單檔離線無 CDN 的挑選器；D 題分析不可逆破壞行為並提出安全替代案。

What I checked before execution / 動手前我檢查了什麼：
1. 確認 git 指令與環境可用（git version 2.49.0）。
2. 確認工作目錄嚴格限定於各題專屬資料夾，無跨目錄危險操作。
3. 檢查原始 input 檔案之狀態與大小，確認未執行任何寫入前 output 不存在。
4. 確認計畫中沒有刪除檔案或修改原始 activities.json 的操作。

## Tests actually performed / 我真的做過的測試

| Test / 測試 | Expected / 預期 | Observed / 實際 | Evidence / 證據 |
|---|---|---|---|
| 1. B題邊界篩選測試（室外／15分鐘／中強度） | 顯示「沒有符合條件的活動」，不放寬條件、不偷改篩選，且失敗抽選不記入歷史紀錄 | 頁面紅字清晰顯示「沒有符合條件的活動」，篩選維持原設定，最近紀錄筆數未增加 | 在 Node 與瀏覽器環境執行，候選數量顯示 0 項，`getFilteredActivities()` 返回空陣列 `[]`，結果容器套用 `.no-match` 樣式 |
| 2. B題唯一定位測試（室外／30分鐘／中強度） | 每次抽選都必須且只能是 A09（在合適位置快走，30分鐘，戶外，中強度） | 連續點擊抽選 5 次，每次皆精準抽出 A09，無任何其他項目干擾 | 比對 `activities.json`，符合條件者僅 A09 一項；頁面正確顯示 `[A09]`、名稱、時間與強度 |
| 3. A題雜湊驗證與版本檢查 | `input/` 12 個檔案完全無異動；`output/` 各副本與原檔 SHA-256 完全吻合；`proposal_final.txt` 與 `proposal_final2.txt` 兩份皆完整保留 | 原檔修改時間未變；12 個副本雜湊 100% 吻合；兩份企劃案分別保留於 `proposals/`，manifest 與 report 皆正確記錄差異 | SHA-256 腳本檢驗全數通過；`manifest.json` 包含 12 筆對照紀錄 |

## One revision / 一次修改

Before / 原來的情況：
B 題第一版中，篩選條件下拉選單變更時，使用者無法預先得知是否有符合的活動；且點擊按鈕後若連續抽中相同項目時缺乏顯著視覺動態；無鍵盤操作快捷鍵。

Request / 我提出的修改：
1. 在篩選區新增即時計算的「符合條件的候選活動數量」提示，下拉變更時即時響應。
2. 增加鍵盤 Enter 快捷鍵，可直接在頁面上按 Enter 進行抽選。
3. 為地點與強度徽章增加語意化配色（室內藍、室外紫、低強度綠、中強度橘），並加入淡入動畫回饋。

After and retest / 修改後與重測結果：
修改後切換為「室外/15分/中」，即時提示「符合條件的候選活動：0 項」，按下 Enter 立即顯示無符合活動；切換為「室外/30分/中」，即時顯示「1 項」，按 Enter 順利抽得 A09。切換至英文時，提示文字同步更新為「Matching candidates: 1 items」，功能完整重測通過。

New requirement or defect? / 新需求還是原規格未做到？
新增需求（原規格功能均已達成，此為提升易用性、可及性與即時回饋之增強修改）。

## One rejection / 一次退回

Which action I reject and why / 退回哪個動作、為什麼：
退回 `bad-plan.txt` 中的「整理整個 Downloads、刪除重複檔、將 final2 當最新版、缺失值補合理值、自動公開成果」。原因：(1) 操作全 Downloads 具越權存取私人檔案風險；(2) 不可逆刪除重複檔案可能導致重要資料滅失；(3) final2 與 final 可能為不同方案草案，不能憑檔名認定定案；(4) 補猜測值會產生資料幻覺；(5) 自動公開違反安全審查原則。

An acceptable alternative / 可以怎麼改：
限定路徑於單一任務資料夾；原檔唯讀，以建立副本取代刪除；重複與衝突檔案保留於 report.md 供人工確認；缺失值標記 unknown 並輸出 issues.md；產出保留於本機 output/ 供使用者自行驗收，絕不自動外傳。

## Still unverified / 還沒驗證

What I cannot claim is complete / 哪些事不能說已完成：
1. 不能宣稱社團實體活動的最終方案已經拍板（室內 20 分鐘 vs 室外 30 分鐘仍需幹部會議正式決議）。
2. 不能宣稱 100 虛構單位的紙張預算已經獲得校方或指導老師核准撥款。
3. 不能宣稱挑選器的隨機演算法在數萬次抽取下具有統計學意義上的完美均勻機率分佈（本次僅驗證條件篩選與業務邏輯之正確性）。

For the fallback route, mark all prepared evidence as supplied simulation. / 備援路線請標明所有預生成證據來源，不能填成自己的Agent實跑。
本實作為使用 Antigravity Agent 於本機完整實際執行、測試與驗收，非填寫備援模擬資料。
