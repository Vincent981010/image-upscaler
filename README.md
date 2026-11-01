# SharpUp 圖片降噪放大

在瀏覽器中直接運算的圖片降噪與放大工具。所有處理都在你的裝置上完成，圖片不會上傳到任何伺服器。

## 功能

- 拖曳或選擇圖片，預覽原圖與結果對照
- 風格：插畫、插畫（掃描）、照片、插畫（輕量快速）
- 降噪：無 / 低 / 中 / 高 / 最高
- 放大：不放大 / 1.6x / 2x / 4x
- 輸出格式：PNG / WebP
- 優先使用 WebGPU 加速，不支援時自動改用 WebAssembly（CPU）

## 專案結構

```
index.html
README.md
onnx_models/
  cunet/art/
  swin_unet/art/
  swin_unet/art_scan/
  swin_unet/photo/
```

`index.html` 預設從 `onnx_models/` 載入模型。若要更改路徑，修改 `index.html` 中的 `MODEL_BASE` 常數即可。

## 部署到 GitHub Pages

1. 建立 Public repository，將 `index.html`、`README.md` 與 `onnx_models` 資料夾上傳到根目錄。
2. 到 **Settings → Pages**，Source 選 **Deploy from a branch**，Branch 選 `main`、資料夾選 `/ (root)`，按 Save。
3. 等待一兩分鐘，即可在 `https://<你的帳號>.github.io/<repository 名稱>/` 開啟。

注意：

- GitHub 單一檔案上限為 100 MB，整個站台建議不超過 1 GB。目前每個模型約 5～19 MB，沒有問題；不需要的模型可以刪除以縮小 repository。
- 必須透過 `http(s)://` 開啟頁面。直接雙擊 `index.html`（`file://`）會因瀏覽器限制而無法載入模型。本機測試可在專案資料夾執行 `python3 -m http.server`，再開啟 `http://localhost:8000`。
- 執行環境 `onnxruntime-web` 由 jsDelivr CDN 載入，使用者需要連網。

## 模型對應

| 介面選項 | 載入的檔案 |
| --- | --- |
| 降噪「無」+ 放大 2x | `scale2x.onnx` |
| 降噪「無」+ 放大 4x | `scale4x.onnx` |
| 降噪 低～最高 + 放大 | `noise0`～`noise3` + `_scale2x` / `_scale4x` |
| 只降噪、不放大 | `noise0`～`noise3.onnx` |
| 1.6x | 先以 2x 模型放大，再縮小到 1.6x |
| 輕量快速 + 4x | 連續執行兩次 2x |

## 使用提示

- 降噪只在圖片確實有雜訊（例如 JPEG 壓縮痕跡）時使用，否則可能造成反效果。
- 圖片以 128×128 的小塊逐塊處理，大圖耗時較長，請看進度條。
- 手機瀏覽器記憶體有限，4x 放大大圖可能失敗，建議使用電腦。
- 處理失敗時，畫面會顯示錯誤訊息，可依訊息檢查模型路徑是否正確。

## 致謝與授權

模型與相關程式來自開源專案 [nagadomi/nunif](https://github.com/nagadomi/nunif)（MIT License）。使用或散布時請依該專案的授權條款保留原作者的版權聲明。
