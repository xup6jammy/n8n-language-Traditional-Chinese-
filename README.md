# 效果
1. 登入首頁 (英)
    <img width="1280" height="619" alt="n8n-英-登入" src="https://github.com/user-attachments/assets/3ee69334-325b-495d-8233-1aeb39e685b7" />
1. 登入首頁 (繁中)
    <img width="1280" height="619" alt="n8n-繁中-登入" src="https://github.com/user-attachments/assets/3597f693-d623-47e8-b281-b37b4d1c66aa" />
2. 概述(英)
    <img width="1280" height="619" alt="n8n-英-概述" src="https://github.com/user-attachments/assets/30b65384-8dc3-4608-8e84-494c8285bce6" />
2. 概述 (繁中)
    <img width="1280" height="619" alt="n8n-繁中-概述" src="https://github.com/user-attachments/assets/5239fee1-10fb-4044-8460-ec8103094b4f" />
3. 憑證 (英)
   <img width="1280" height="619" alt="n8n-英-變數" src="https://github.com/user-attachments/assets/6d79a093-afe4-46f3-8b38-fffd3c2d5797" />
3. 憑證 (繁中)
    <img width="1280" height="619" alt="n8n-繁中-變數" src="https://github.com/user-attachments/assets/00f9ef95-1552-4c86-b2ec-195d310d9049" />
4. 工作畫布1 (英)
    <img width="1280" height="619" alt="n8n-英-畫布" src="https://github.com/user-attachments/assets/7d5bd9d7-1617-4644-bede-489276984da7" />
4. 工作畫布1 (繁中)
    <img width="1280" height="619" alt="n8n-繁中-畫布" src="https://github.com/user-attachments/assets/25b93f53-7194-4b68-8675-78aae8b1d48d" />
5. 工作畫布2 (英)
    <img width="1280" height="619" alt="n8n-英-畫布-2" src="https://github.com/user-attachments/assets/c68ff934-88a9-40dd-a9b0-3c08fd7af7f1" />
5. 工作畫布2 (繁中)
    <img width="1280" height="619" alt="n8n-繁中-畫布-2" src="https://github.com/user-attachments/assets/39519527-c957-4bf0-b0ad-33a6e7587722" />
---
# n8n 中文化配置指南

## 步驟 1: 準備中文語言檔

確認 `packages/frontend/@n8n/i18n/src/locales/zh.json` 存在
- 如果沒有，從 `zh-CN.json` 複製一份並重命名為 `zh.json`

## 步驟 2: 修改前端 i18n 配置

**檔案位置:** `packages/frontend/@n8n/i18n/src/index.ts`

```typescript
import englishBaseText from './locales/en.json';
import chineseBaseText from './locales/zh.json';

export const i18nInstance = createI18n({
    legacy: false,
    locale: 'zh',           // 改為 'zh'
    fallbackLocale: 'en',
    messages: {
        en: englishBaseText,
        zh: chineseBaseText,  // 註冊 zh
    },
    warnHtmlMessage: false,
});
```

## 步驟 3: 修改前端 Store 預設語言

**檔案位置:** `packages/frontend/@n8n/stores/src/useRootStore.ts`

**第 41 行:**
```typescript
defaultLocale: 'zh',  // 改為 'zh'
```

## 步驟 4: 修改後端配置預設語言

**檔案位置:** `packages/@n8n/config/src/index.ts`

**第 186 行:**
```typescript
@Env('N8N_DEFAULT_LOCALE')
defaultLocale: string = 'zh';  // 改為 'zh'
```

## 步驟 5: 設定環境變數

**檔案位置:** `.env` (專案根目錄)

```env
N8N_DEFAULT_LOCALE=zh
N8N_PORT=5678
N8N_SECURE_COOKIE=false
```

## 步驟 6: 建立啟動批次檔 (選用)

**檔案位置:** `start-n8n-zhtw.bat` (專案根目錄)

```batch
@echo off
REM n8n 中文啟動腳本

echo 設置中文語言...
set N8N_DEFAULT_LOCALE=zh
set N8N_PORT=5678
set N8N_SECURE_COOKIE=false

echo 啟動 n8n...
cd /d "%~dp0"
pnpm dev

pause
```

## 步驟 7: 重新編譯相關套件

```bash
# 切換到 n8n 專案根目錄
cd <你的n8n專案路徑>
# 例如: cd C:\Users\<你的用戶名>\Desktop\n8n\n8n

# 編譯 i18n 套件
pnpm --filter @n8n/i18n build

# 編譯 config 套件
pnpm --filter @n8n/config build

# 編譯 stores 套件
pnpm --filter @n8n/stores build
```

## 步驟 8: 啟動 n8n

**方法一: 使用批次檔**
```cmd
start-n8n-zhtw.bat
```

**方法二: 手動啟動**
```cmd
set N8N_DEFAULT_LOCALE=zh
pnpm dev
```

## 步驟 9: 驗證

開啟瀏覽器訪問 `http://localhost:5678`

檢查介面是否顯示中文

---

## 關鍵要點

1. **必須使用 `zh` 而非 `zh-TW`**
2. **三層配置必須一致:**
   - 前端 i18n (`locale: 'zh'`)
   - 前端 Store (`defaultLocale: 'zh'`)
   - 後端 Config (`defaultLocale: string = 'zh'`)
3. **修改後必須重新編譯相關套件**
4. **環境變數 `N8N_DEFAULT_LOCALE=zh` 必須設定**
