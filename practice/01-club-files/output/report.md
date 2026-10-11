# 社團檔案整理報告 (Club Files Organization Report)

## 1. 檔案總數與盤點概要
- **原始資料夾**：`practice/01-club-files/input`
- **檔案總數**：共 12 個文字檔案
- **輸出資料夾**：`practice/01-club-files/output`
- **操作原則**：完整保留原始 input 檔案，無任何刪除、覆蓋或內容更動；所有檔案皆建立分類副本。

---

## 2. 建議分類架構與對照
整理後歸納為四大分類資料夾：

| 分類資料夾 | 說明 | 收錄檔案 |
|---|---|---|
| `announcements/` | 對外公告、通知與文案宣傳 | `announcement.txt`, `announcement_copy.txt`, `poster_text.txt` |
| `logistics/` | 總務支出預算與器材物資盤點清單 | `budget_draft.txt`, `equipment_backup.txt`, `equipment_list.txt` |
| `meetings/` | 例行幹部會議紀錄與活動檢討回饋 | `meeting_notes.txt`, `feedback_questions.txt` |
| `proposals/` | 活動企劃方案、雨天備案與後續評估 | `proposal_final.txt`, `proposal_final2.txt`, `rain_plan.txt`, `next_steps.txt` |

---

## 3. 內容完全相同（重複）檔案分析
經由 SHA-256 雜湊值比對，發現以下兩組檔案內容 100% 相同：

1. **公告文件組**：
   - `announcement.txt`
   - `announcement_copy.txt`
   - **SHA-256**：`c19164b1054ebf52ed33bc5321c887985ea043a0b07db1e99f866986d2ccd584`
   - **處理方式**：兩份皆予以保留於 `announcements/`，不擅自刪除任一份，以供備份查驗。

2. **器材清單組**：
   - `equipment_list.txt`
   - `equipment_backup.txt`
   - **SHA-256**：`c21d53ba1f8d3ad06e1833539935120a5baca4d13b34a2aa98ce157ff4502fa3`
   - **處理方式**：兩份皆保留於 `logistics/`，記錄為疑似備份檔案。

---

## 4. 名稱相近但內容不同（版本差異）分析
特別注意企劃案檔案，名稱皆含有 "final"，但實質內容完全不同：

- **`proposal_final.txt`**：
  - 內容：Proposal v1（室外活動，30分鐘），狀態為尚未核准。
  - SHA-256：`c520147e4561c8799949ce873baa14c7d6d0fe2b6cec4beb2c8f87921b0345`
- **`proposal_final2.txt`**：
  - 內容：Proposal v2（室內活動，20分鐘），狀態為等待確認。
  - SHA-256：`6fcce7bc6e075ccf7fad659bd632a6840227cd015b2dce0da4884bb36fe9faa8`

**重要判定**：
不得僅因檔名包含 `final2` 或建立時間較晚，便自行認定其為定案版。兩者皆保留於 `proposals/`，交由社團幹部開會決議。

---

## 5. 待確認問題與人工作業清單
1. **企劃定案**：室外方案（30分鐘）與室內方案（20分鐘）尚未核定，且 `rain_plan.txt` 指出下雨需討論室內備案，需幹部會議確認選定哪一版。
2. **預算審核**：`budget_draft.txt` 明確載明「非核准支出（100虛構單位）」，仍待幹部大會或指導老師審核。
3. **單一資料來源維護**：`announcement_copy.txt` 與 `equipment_backup.txt` 宜在幹部確認後制定命名標準，避免多版本雜亂。

---

## 6. 實際做過的檢查
- [x] **原始檔案未變更**：檢查 `input/` 下 12 個檔案之修改時間與雜湊值，無任何異動或刪除。
- [x] **副本數量完整**：確認 `output/` 包含 12 個對應副本檔案，無遺漏。
- [x] **位元一致性檢驗**：抽查並全數驗證 12 對來源與目的檔案之 SHA-256 雜湊值 100% 吻合。
- [x] **清單完整性**：產生 `manifest.json`，涵蓋全部 12 筆來源相對路徑、目的相對路徑與歸納理由。

---

## 7. 還沒確認的部分
- 真實實體活動的最終決策（室內 vs 室外）。
- 經費與預算是否獲得實質批准。
- 未來社團共同共用雲端空間之檔案權限與歸檔標準規範。
