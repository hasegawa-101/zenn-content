---
title: "テスト時にreact-modalが原因で発生するエラー解消方法"
emoji: "🤫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ "react", "テスト"]
published: false
---

現在react-modalを使うことはあまりないですが、備忘録も兼ねて。

## 対象となるモーダルのコード

```typescript jsx
import React from "react";
import Modal from "react-modal";

export const exampleModal = () => {
  Modal.setAppElement("#root"); // ルートとなる要素を指定
  
  const [isOpen, setIsOpen] = React.useState(false);
  
  return (
    <>
      <button onClick={() => setIsOpen(true)}>Open Modal</button>
      <Modal
        isOpen={isOpen}
        onRequestClose={() => {
          setIsOpen(false);
        }}
      >
      <h2>Example Modal</h2>
      <button onClick={() => setIsOpen(false)}>Close Modal</button>
    </Modal>
    </>
  );
};
```

Jest、もしくはVitestでreact-modalを使用したモーダルのテストをする際に、以下のようなエラーが発生するはずです。

```bash
Error: react-modal: No elements were found for selector #root.
```

## 解消方法

テストコードに以下のコードを追加することで解消できます。

```typescript
beforeAll(() => {
    const app = document.createElement("div")
    app.id = "root" // 指定しているルートとなる要素を作成
  
    document.body.appendChild(app)
  });
```

