[README.md](https://github.com/user-attachments/files/29622953/README.md)
# Gait Studio — 歩行フォーム解析

iPhone等で撮影した歩行動画を **ブラウザ内だけ** で解析し、歩行速度（推定）・ケイデンス・体幹の傾き・左右対称性などを数値化して、フォーム改善のヒントを返す単一ファイルのWebアプリです。姿勢推定は Google MediaPipe（`tasks-vision`）を CDN から読み込んで実行します。動画はサーバーに送信されません。

## GitHub Pages で公開する手順

`https://hideltoid.github.io/running-coach/` で公開する場合:

1. GitHub で **`running-coach`** という名前のリポジトリを作成する（Public）。
2. この `index.html` をリポジトリの**ルート**に置いて push する。
   ```bash
   git init
   git add index.html README.md
   git commit -m "Gait Studio v1"
   git branch -M main
   git remote add origin https://github.com/hideltoid/running-coach.git
   git push -u origin main
   ```
3. リポジトリの **Settings → Pages** を開く。
4. **Build and deployment → Source** を「Deploy from a branch」にし、**Branch** を `main` / `/ (root)` に設定して Save。
5. 数十秒〜数分後、`https://hideltoid.github.io/running-coach/` で公開されます。

> ファイル名が `index.html` であればトップページとして自動表示されます。追加設定は不要です。

## 使い方

1. 身長(cm)を入力し、撮影の向き（**横から**を推奨）を選ぶ。
2. 歩行動画（**MP4 / H.264**、5〜10秒、全身が枠に収まる横からの映像が最適）をドロップまたは選択。
3. エンジン読み込み → フレームごとに姿勢を追跡 → 指標とコーチングメモを表示。

## 精度を上げる撮影のコツ

- カメラを**三脚等で固定**し、被写体が画面を横切るように撮る（速度推定は横からの固定カメラ前提）。
- **全身**が常に枠に入るようにする（頭〜足）。
- 明るい場所、背景がごちゃつかない場所が望ましい。
- iPhoneは **設定 → カメラ → フォーマット → 「互換性優先」** にすると H.264 で保存され、どのブラウザでも再生・解析できます（`.mov`/HEVCは一部ブラウザで非対応）。

## 技術メモ

- 姿勢推定: `@mediapipe/tasks-vision@0.10.22`（`pose_landmarker_full`）。33関節ランドマーク＋3Dワールド座標。
- 速度は「身長 → ピクセル実寸換算」＋股関節中心の水平移動量から**推定**（絶対値ではなく概算）。
- ケイデンスは足関節の鉛直方向ピーク（接地）検出から算出。
- 体幹傾き・腕振り・上下動・膝可動域・左右対称性を併せて算出し、一般的な成人歩行の目安と比較。

## 注意

参考情報を提供するツールであり、医学的診断・治療方針の決定を目的としたものではありません。痛み・転倒不安・神経学的症状がある場合は医療専門職にご相談ください。
