# タイ料理メニュー AR（京都武田病院内 タイ料理店）

メニュー表のQRを読む → **メニュー表全体**にカメラを向ける → その料理が**実物大の3D**で登場し、
**吹き出し＋音声で説明**します。実物大なので「どのくらいの量か・どんな料理か」がひと目で伝わります。

公開URL: **https://exe-kishimoto.github.io/takeda-thai-ar/**

---

## 📁 ファイル構成
| ファイル | 役割 | 公開に必要 |
|---|---|:---:|
| `index.html` | ARアプリ本体 | ✅ |
| `assets/gapao.glb` | 601 ガイパットガパオ（53MB→0.5MBに圧縮） | ✅ |
| `assets/kaomangai.glb` | 605 カオマンガイ（50MB→0.6MBに圧縮） | ✅ |
| `assets/targets.mind` | メニュー表マーカー（生成済み） | ✅ |
| `assets/menu.png` | マーカーの元画像（実行時は不要） | － |

公開時は `index.html` と `assets/` の3ファイル（gapao / kaomangai / targets.mind）でOK。

---

## 🍽 使い方（お客さま側）
1. メニューのQRを読む → 「▶ はじめる」→ カメラ許可
2. **メニュー表全体**にカメラを向ける
3. 実物大の料理が登場して説明を始めます
4. 下のタブで **料理を切替**（601 ガパオ / 605 カオマンガイ）
5. ドラッグ＝回転、ピンチ＝拡大、タップ＝説明、⏸自動回転

---

## 📏 実物大の合わせ方（重要）
ARの寸法は **メニュー表の印刷サイズ基準** です。印刷後に一度だけ合わせます。
1. 右上 ⚙️ →「実寸合わせ」
2. **メニュー表の印刷横幅(cm)** と **料理（皿）の実寸横幅(cm)** を入力
3. 「この実寸に合わせる」を押す → 実物大に自動調整

> 例：メニューをA3横（幅42cm）で印刷、皿の実寸が26cm → 自動で scale≈0.326。
> 決めた値を固定したい場合は `index.html` の `DISHES` 各料理の `scale` に書き写してください。

---

## 🔳 QRコードの作り方
```
node ../webar/make-qr.mjs https://公開URL/                # メニュー用（点けて料理を切替）
node ../webar/make-qr.mjs "https://公開URL/?dish=gapao"    # ガパオ直行QR
node ../webar/make-qr.mjs "https://公開URL/?dish=kaomangai" # カオマンガイ直行QR
```
`?dish=` を付けると、その料理を最初から表示します（各料理の横にそのQRを置けます）。

---

## ➕ 料理を追加するには
1. 料理の `.glb` を用意し `../webar` で圧縮（`npx @gltf-transform/cli optimize 入力.glb assets/xxx.glb --compress draco --texture-compress webp --texture-size 2048 --simplify true --simplify-error 0.001`）
2. `index.html` の `<a-assets>` に `<a-asset-item id="xxx" src="./assets/xxx.glb">` を追加
3. `DISHES` に `xxx: { asset:'#xxx', label:'...', halfH:(高さ/2), scale:0.3, lines:[...] }` を追加

マーカーはメニュー表全体なので、料理を増やしても作り直し不要です。

---

## 補足
- 音声は端末の音声合成（Web Speech API）。iOS Safari / Android Chrome の新しめで動作。
- モデルは料理のフォトグラメトリ。アニメーションはありません（回転して見せています）。
- セリフは `index.html` の `DISHES[...].lines` を書き換えて自由に変更できます。
