# 輸出模式

當技能需要產出一致、高品質的輸出時，使用這些模式。

## 範本模式

為輸出格式提供範本。根據需求匹配嚴格程度。

**嚴格要求（如 API 回應或資料格式）：**

```markdown
## 報告結構

始終使用此確切的範本結構：

# [分析標題]

## 摘要
[關鍵發現的一段式概述]

## 主要發現
- 發現 1 及支持資料
- 發現 2 及支持資料
- 發現 3 及支持資料

## 建議
1. 具體可執行的建議
2. 具體可執行的建議
```

**彈性指導（當需要靈活調整時）：**

```markdown
## 報告結構

以下是合理的預設格式，但請依最佳判斷調整：

# [分析標題]

## 摘要
[概述]

## 主要發現
[根據你的發現調整區段]

## 建議
[根據具體情境調整]

根據具體的分析類型按需調整區段。
```

## 範例模式

當輸出品質取決於看到範例時，提供輸入/輸出配對：

```markdown
## 提交訊息格式

遵循以下範例生成提交訊息：

**範例 1：**
輸入：Added user authentication with JWT tokens
輸出：
```
feat(auth): implement JWT-based authentication

Add login endpoint and token validation middleware
```

**範例 2：**
輸入：Fixed bug where dates displayed incorrectly in reports
輸出：
```
fix(reports): correct date formatting in timezone conversion

Use UTC timestamps consistently across report generation
```

遵循此風格：type(scope): 簡短描述，然後是詳細說明。
```

範例比單純的描述更能幫助 Claude 理解期望的風格和細節程度。
