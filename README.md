# Jenkins Will Build Fail (過版失敗演示專案)

此專案為關貿網路（Tradevan）資訊服務部系統三課（T55）之 CI/CD 過版驗證演示專案。

## 專案目標
- 模擬在 Jenkins 執行 Pipeline 過版自動化建置與測試時發生中斷（Fail）。
- 觸發 Jenkins `post { failure { ... } }` 監聽。
- 透過 Zulip API 即時向關貿通訊的 **`#課-系統三課(T55)`** 頻道發送高優先級警報通知。
