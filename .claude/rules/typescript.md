---
paths:
  - "app/**/*.js"
  - "app/**/*.jsx"
  - "app/**/*.ts"
  - "app/**/*.tsx"
  - "app/**/*.vue"
  - "server/**/*.ts"
  - "shared/**/*.ts"
---

# Typescriptコーディング規約

- コンポーネント名はプロジェクト内でユニークにする
- シングルクォート、セミコロンあり、末尾カンマなし

## 命名規則

種類|規則|例
---|---|---
変数|ローワーキャメルケース|userCount
定数|アッパースネークケース|CATEGORY_LIST
関数|ローワーキャメルケース|updateData
イベントハンドラ|`on` + ローワーキャメルケース|onUpdateData

## 禁止事項

## 非推奨事項

下記のイベントはパフォーマンス懸念を引き起こす可能性があるため、非推奨となっています

- `scroll`イベント
- `resize`イベント
- `mousemove`イベント
- `touchmove`イベント
- `wheel`イベント

代わりに各種`Observer API`や`matchMedia API`を使用してください
代替手段がない場合は`requestAnimationFrame`などを利用して処理を間引くことでパフォーマンス低下を抑制してください
また`Passive Event Listener`（`addEventListener('wheel', onWheel, { passive: true })`）を使用してユーザーのアクションをブロックしないようにしてください
