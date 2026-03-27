# Note 工作流程（Standard 流程）

呢個 repo 用嚟保存課堂筆記內容，並同時提供：
1. **手機友善 HTML 閱讀版**
2. **PDF 存檔版**
3. **GitHub + Vercel 發布**

網站：
- Production: https://note-omega-nine.vercel.app
- Repo: https://github.com/v2sdryan/note

---

## 目標

每次有新 MP3 / YouTube / 課堂內容時，統一用同一條 pipeline：

**內容來源 → 整理重點 → 生成 HTML → 生成 PDF → 放入 note repo → GitHub → Vercel**

---

## 標準流程

### Step 1：取得內容
支援來源：
1. YouTube link
2. MP3 / 音訊
3. 文字稿 / transcript

### Step 2：整理內容
輸出要求：
1. 繁體中文（香港）
2. 課堂筆記風格
3. 有 H1 / H2 / 小節分段
4. 有 highlight 重點字眼
5. 各大 topic 用不同顏色

### Step 3：生成 HTML 講義
輸出到：
- `narration/youtube-handout-*.html`（工作區草稿）
- `note/notes/*.html`（網站閱讀版）

網站閱讀版原則：
1. 手機優先（mobile-first）
2. 避免 PDF viewer 問題
3. 以 HTML 作主閱讀體驗

### Step 4：生成 PDF
**標準引擎：LibreOffice headless**

原因：
1. 中文 render 比 browser print 穩定
2. 不用人手匯出
3. 可自動化

建議流程：
1. HTML 轉 DOCX
2. 用 `soffice --headless --convert-to pdf` 轉 PDF

範例：
```bash
textutil -convert docx narration/source.html -output narration/output.docx
soffice --headless --convert-to pdf --outdir note/pdfs narration/output.docx
```

### Step 5：放入網站
每篇 note 應包括：
1. 標題
2. 短 summary
3. HTML 閱讀頁
4. PDF 檔案

目前首頁每張卡有 3 個動作：
1. **閱讀 HTML**
2. **打開 PDF**
3. **下載 PDF**

### Step 6：發布
```bash
cd note
git add .
git commit -m "feat: add new note"
git push
vercel --prod --yes
```

---

## 命名規則

### HTML 頁面
用英文 slug：
- `notes/auto-research.html`
- `notes/ai-coding-frameworks.html`

### PDF 檔名
用清楚中文名：
- `自我優化AI_AutoResearch_課堂筆記.pdf`
- `AI_Coding框架比較_課堂筆記.pdf`

### 卡片標題
要求：
1. 短
2. 清楚
3. 適合手機閱讀

---

## 內容風格規則

1. 全部用繁體中文（香港）
2. 不顯示來源 footer / file path
3. 不在網站顯示多餘技術資訊
4. 重點內容要易掃讀
5. 大 topic 顏色一致，小 topic 跟同色系

---

## 推薦結構

每篇筆記建議包含：
1. 核心觀點
2. 主要概念拆解
3. 案例 / 應用場景
4. 結論 / 帶走重點

---

## 已知原則

1. **HTML 係主閱讀版**
2. **PDF 係存檔版**
3. 手機上先優先保證 HTML 體驗
4. PDF 若有兼容問題，先修正流程再上傳

---

## 之後新增筆記時的簡化指令

當有新內容時，照以下做：
1. 取得 transcript
2. 生成同風格 HTML
3. 轉 DOCX
4. LibreOffice 匯出 PDF
5. 更新 `index.html`
6. push + deploy

---

## 備註

如果之後要再升級，可以加：
1. 搜尋功能
2. tag/filter
3. 自動化腳本（新增 note 一鍵完成）
4. 封面圖 / thumbnail
