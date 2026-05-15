# Yamazaki Model Visualizer — ABS Edition
### Interactive Wheel Slide Protection Simulator

**Author:** Dr. Hiroo Yamazaki — *May 16 2026*

---

ブラウザ上でABSまたはWSPシミュレーション結果をリアルタイム可視化するスタンドアロンツールです。インストール・サーバー不要。
A self-contained, zero-dependency browser tool for real-time visualization of wheel slide protection (ABS/WSP) simulation results.

---

## 概要 / Overview

本ツールは **山崎モデル** をベースに、ABS（アンチロックブレーキ）/ WSP（車輪滑走防止装置）のシミュレーション結果をアニメーション表示するインタラクティブビジュアライザです。外部CSVファイルから時系列データを読み込み、接触楕円内の粘着/すべり域分布・接線力分布をリアルタイムに描画します。横すべり $s_y$ をスライダーで独立に制御することで、制動中の前後力とコーナリング力の分布を直感的に把握できます。

This tool provides an interactive animation of wheel slide protection (ABS/WSP) simulation results based on the Yamazaki tire friction model. It loads time-series data from an external CSV file and renders adhesion/slip zone maps and tangential force distributions in real time. The lateral slip $s_y$ can be independently controlled via a slider, enabling intuitive understanding of cornering force effects during braking.

---

## 使い方 / Usage

### ステップ 0 — ブラウザで直接開く（インストール不要）

👉 **https://hiroo718.github.io/yamazaki-model/Yamazaki_Model_Visualizer_ABS.html**

### ステップ 1 — リポジトリをクローン

```bash
git clone https://github.com/hiroo718/yamazaki-model.git
cd yamazaki-model
```

### ステップ 2 — HTMLファイルをブラウザで開く

* **macOS:** `open Yamazaki_Model_Visualizer_ABS.html`
* **Windows:** `start Yamazaki_Model_Visualizer_ABS.html`
* **Linux:** `xdg-open Yamazaki_Model_Visualizer_ABS.html`

> [!NOTE]
> サーバーや依存ライブラリのインストールは一切不要です。Chrome / Firefox / Safari / Edge の最新版で即座に動作します。
> No server or library installation required. Works in any modern browser.

---

## CSVファイルの読み込み / Loading CSV Data

左パネルの **📂 CSVファイルを読み込む** からABSやWSPのシミュレーション結果を読み込めます。

### CSVフォーマット

```
time, V, slip, BC
0.01, 80.0, 0.00069, 23.49
0.03, 79.9954, 0.00335, 70.47
...
```

| 列 | 説明 | 単位 |
|---|---|---|
| **time** | 時刻 | s |
| **V** | 車両速度 | km/h |
| **slip** | 縦スリップ率 $s_x$ | — |
| **BC** | ブレーキシリンダ圧力 | kPa |

> [!NOTE]
> 車輪速度 $V_w$ はCSVに含まれなくても、$V_w = V \times (1 - s_x)$ から自動計算されます。5列形式（time, V, Vw, slip, BC）にも対応しています。
> Wheel speed $V_w$ is automatically computed as $V_w = V \times (1 - s_x)$ if not included. 5-column format (time, V, Vw, slip, BC) is also supported.

CSVを読み込むとデフォルトのサンプルデータが差し替わり、アニメーションがリセットされます。

---

## 操作パネル / Control Panel

| コントロール | 説明 |
|---|---|
| **📂 CSVファイルを読み込む** | 外部CSVからシミュレーション結果を読み込む |
| **▶ 再生 / ■ 停止 / ↺ リセット** | アニメーションの制御 |
| **⏺ 録画開始 / ⏹ 保存** | アニメーションをwebm動画として保存 |
| **再生速度** | 1x〜10x の再生速度調整 |
| **横すべり $s_y$** | コーナリング相当の横スリップ率をリアルタイムに調整（0〜1.0） |

---

## 表示パネル / Visualization Panels

```text
┌──────────────────┬────────────────┬────────────────┐
│  車両速度        │  車輪速度      │  BC圧          │
│  V [km/h]        │  Vw [km/h]     │  BC [kPa]      │
├──────────────────┬────────────────┬────────────────┤
│  粘着/すべり域   │  接線力|f|分布  │  圧力分布p(x,y)│
│  Adhesion/Slip   │  Tangential |f| │  Hertz p(x,y)  │
├──────────────────┴────────────────┴────────────────┤
│          ABS/WSP 時系列応答 (V, Vw, BC)            │
└────────────────────────────────────────────────────┘
```

* **車両速度 / 車輪速度 / BC圧メーター** — 現在の時刻における各量をアナログメーターで表示
* **粘着/すべり域** — 青 = 粘着域、赤 = すべり域。制動力の飽和状況をリアルタイムに把握
* **接線力 $|f|$ 分布** — $\sqrt{f_x^2 + f_y^2}$ のヒートマップ表示
* **圧力分布 $p(x,y)$** — Hertz 楕円接触に基づく法線力分布
* **ABS/WSP 時系列応答** — $V$（車両速度）、$V_w$（車輪速度）、BC圧の時刻歴を同時表示

---

## 固定パラメータ / Fixed Parameters

本ツールでは以下のパラメータが固定値として設定されています。

| 記号 | 値 | 説明 / Description |
|---|---|---|
| **$a$** | $50\text{ mm}$ | 接触楕円 半長軸（進行方向） / Semi-major axis (longitudinal) |
| **$b$** | $74\text{ mm}$ | 接触楕円 半短軸（横方向） / Semi-minor axis (lateral) |
| **$F_z$** | $\approx 4448\text{ N}$ | 垂直荷重（$1815\text{ kg}$ 車両の1輪分） / Normal load |
| **$G$** | $10\text{ MPa}$ | タイヤゴムの横せん断弾性係数 / Shear modulus |
| **$v_0$** | $80\text{ km/h}$ | 基準車速 / Reference vehicle speed |
| **$\mu_s$** | $0.30$ | 静摩擦係数 / Static friction coefficient |
| **$\alpha$** | $0.20$ | 速度無限大での動摩擦比 / Dynamic friction ratio at infinite speed |
| **$\beta$** | $0.05$ | 動摩擦の速度依存減衰率 / Velocity-dependent decay rate |
| **$N, M$** | $60 \times 60$ | 計算メッシュ数 / Mesh resolution |

---

## モデルの理論 / Model Theory

接触力の計算には **山崎モデル**（Brush Model + Hertz 楕円接触 + 速度依存動摩擦）を使用しています。
詳細な理論については
https://github.com/hiroo718/yamazaki-model
の [`README.md`](./README.md) を参照してください。

For the theoretical background of the contact force model (Brush Model, Hertz contact, velocity-dependent friction),
please refer to [`README.md`](./README.md) in https://github.com/hiroo718/yamazaki-model.

---

## 引用 / Citation

本ツールを研究、論文、または学会発表等に使用する場合は、`CITATION.cff` を参照して引用してください。

*If you use this tool in your research, please cite it using the metadata in `CITATION.cff`.*

---

## Contact

For questions or feedback, feel free to open a [Discussion](https://github.com/hiroo718/yamazaki-model/discussions) or contact: yamazaki.hiroo@gmail.com
