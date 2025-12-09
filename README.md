# He-Tree Demo

這是一個使用 [he-tree-vue](https://github.com/phphe/he-tree) 套件的 Vue 3 專案，實作了可編輯的樹狀結構編輯器。

## 功能特色

- ✅ 四個欄位的節點編輯（變數名稱、欄位中文、資料型態、資料內容）
- ✅ 拖曳排序功能
- ✅ 新增/刪除節點
- ✅ 自動儲存（debounce）
- ✅ 樹狀結構視覺化（有連接線）

## 安裝與執行

```bash
# 安裝依賴
npm install

# 啟動開發伺服器
npm run dev

# 建置專案
npm run build
```

## 技術棧

- Vue 3
- @he-tree/vue
- Vite
- Axios

## 注意事項

API 端點預設為 `http://localhost:3001`，請根據實際需求調整 `src/App.vue` 中的 `API_BASE` 變數。
