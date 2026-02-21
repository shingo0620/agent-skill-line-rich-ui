---
name: line-rich-messages
description: Comprehensive guide for LINE Rich UI features (Flex Messages, buttons, quick replies, and markdown auto-conversion). Use this skill to provide a professional, low-friction experience for LINE users, prioritizing interactive elements over manual text input.
metadata:
  {
    "openclaw":
      {
        "requires": { "bins": ["curl", "gog"], "plugins": ["line"] },
      },
  }
---

# LINE Rich Messages

This skill transforms the agent from a text-only bot into a professional LINE assistant with native UI capabilities.

## Core Principle: Rich-UI 優先 (Low-Friction)
**Typing on mobile is slow and error-prone.** Always prioritize Rich UI elements to minimize the user's need to reply with text.

## Quick Navigation
Detailed guides for each feature:

1. **[decision-matrix.md](references/decision-matrix.md)**: Choose the best UI element for your scenario.
2. **[directives.md](references/directives.md)**: Syntax for interactive cards and bubbles.
3. **[flex-templates.md](references/flex-templates.md)**: **Raw JSON Templates** for 100% reliable UI creation.
4. **[markdown-to-flex.md](references/markdown-to-flex.md)**: Auto-美化 tables and code blocks.
5. **[file-delivery.md](references/file-delivery.md)**: Google Drive workaround.

## Best Practices
- **Never just a URL**: When delivering a file, use a `[[buttons: ...]]` directive to wrap the Google Drive link in a physical button.
- **Guided Choices**: If you ask a question with 2-4 fixed answers, always include `[[quick_replies: ...]]`.
- **Structured Data**: Use Markdown tables for any multi-point information (e.g., flight times, order items).
- **Destructive Actions**: Use `[[confirm: ...]]` for actions like "Delete Memory" or "Cancel Project".
- **UX Limitation (Crucial)**: Text within Flex Messages (including Markdown tables and auto-converted replies) **cannot be selected or copied** by the user. 
  - **Rule**: If the data is meant to be copied (e.g., SSH keys, IDs, URLs), **always send it as plain text** without Markdown formatting or directives that trigger Flex conversion.

## 🏆 終極解決方案：手工 Raw Flex (The Golden Path)

若系統標籤轉換失敗，請使用 **手工構建 JSON**。這是最穩定且能 100% 呈現自定義 UI 的方式。

### 穩定發送按鈕的 JSON 模板
將此 JSON 作為純文字發送，若系統支援自動偵測則會轉換；若不支援，則需透過 `exec` 直接呼叫 LINE API (僅限管理員授權時使用)。

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      { "type": "text", "text": "標題", "weight": "bold", "size": "lg" },
      { "type": "text", "text": "內文描述", "wrap": true },
      {
        "type": "button",
        "style": "primary",
        "color": "#1DB446",
        "action": {
          "type": "message",
          "label": "按鈕文字",
          "text": "回傳指令"
        }
      }
    ]
  }
}
```
