```markdown
# Yamazaki Model Visualizer
### Interactive Tire Contact Force Simulator — Brush & Hertz Model

**Author:** Dr. Hiroo Yamazaki — *May 9th 2026*

---

Yamazaki Model Visualizer is an open-source Executable Engineering Model (EEM) for tire-road and wheel-rail contact mechanics.
Yamazaki Model Visualizer は、タイヤ・路面および車輪・レール間接触力学を対象としたオープンソース実行可能工学モデル（EEM）です。

ブラウザ上でタイヤ接触力モデルをリアルタイム可視化するスタンドアロンツールです。インストール・サーバー不要。
A self-contained, zero-dependency browser tool for real-time visualization of tire contact mechanics.

## 概要 / Overview

本ツールは **山崎モデル** を実装したインタラクティブビジュアライザです。Brush Model（ブラシモデル）、Hertz（ヘルツ）楕円接触圧力、および速度依存動摩擦を統合し、接触楕円内の粘着/すべり域分布・接線力分布・トラクション特性曲線をリアルタイムに計算・描画します。

This tool implements the Yamazaki tire friction model, integrating the Brush Model, Hertz elliptical contact pressure, and velocity-dependent dynamic friction. It renders adhesion/slip zone maps, tangential force distributions, and traction characteristic curves in real time.

<div class="model-reference-graph" style="text-align: center; margin: 20px 0;">
    <h3>モデル検証データの例 (Yamazaki Model vs Sakai Tire)</h3>
    <img src="images/sakai_vs_yamazaki.png" alt="Yamazaki Model vs Sakai Tire Graph" style="max-width: 100%; height: auto; border: 1px solid #ccc; border-radius: 4px;">
    <p style="font-size: 0.9em; color: #555;">図：Sakai Tireの実測値（●）に対するYamazakiモデル（実線）のフィッティング特性</p>
</div>
---

## 使い方 / Usage
### ステップ 0 — ブラウザで直接開く（インストール不要）

👉 **https://hiroo718.github.io/yamazaki-model/Yamazaki_Model_Visualizer_Tire.html**

### ステップ 1 — リポジトリをクローン
```bash
git clone [https://github.com/hiroo718/yamazaki-model.git](https://github.com/hiroo718/yamazaki-model.git)
cd yamazaki-model

```

### ステップ 2 — HTMLファイルをブラウザで開く

* **macOS:** `open Yamazaki_Model_Visualizer_Tire.html`
* **Windows:** `start Yamazaki_Model_Visualizer_Tire.html`
* **Linux:** `xdg-open Yamazaki_Model_Visualizer_Tire.html`

> [!NOTE]
> サーバーや依存ライブラリのインストールは一切不要です。Chrome / Firefox / Safari / Edge の最新版で即座に動作します。
> No server or library installation required. Works in any modern browser.

---

## 操作パネル / Control Panel

左側のコントロールスライダーで以下のパラメータをリアルタイムに変更可能です。

| スライダー | 説明 | 範囲 |
| --- | --- | --- |
| **$s_x$**（縦スリップ率） | 加減速方向のスリップ。$0$ = 純転がり、$1$ = 完全ロック | $0 \sim 1.0$ |
| **$s_y$**（横スリップ率） | 横力方向のスリップ（コーナリング相当） | $0 \sim 1.0$ |
| **$\mu_s$**（静摩擦係数） | 粘着限界を決める最大摩擦係数 | $0.10 \sim 0.60$ |
| **$\alpha$（alfa）** | 速度無限大での動摩擦比 $\mu_\infty / \mu_0$ | $0 \sim 1.0$ |
| **$\beta$（beta）** | 動摩擦の速度依存減衰率（Stribeck効果の傾き） | $0.01 \sim 0.30$ |

---

## 表示パネル / Visualization Panels

```text
┌──────────────────┬────────────────┬────────────────┐
│  粘着/すべり域   │  接線力|f|分布   │  圧力分布p(x,y) │
│  Adhesion/Slip   │  Tangential |f| │  Hertz p(x,y)  │
├──────────────────┴────────────────┴────────────────┤
│          トラクション特性曲線 Fx/Fz, Fy/Fz         │
│          (sx sweep / sy sweep タブ切替)            │
└────────────────────────────────────────────────────┘

```

* **粘着/すべり域 (Adhesion/Slip Zone)**
青 = 粘着域（弾性変形支配）、赤 = すべり域（摩擦力飽和）。スリップ率増加に伴う境界の遷移を直感的に把握できます。
* **接線力 $|f|$ 分布 (Tangential Force Distribution)**
$\sqrt{f_x^2 + f_y^2}$ の空間分布をヒートマップ表示。応力の集中箇所を確認できます。
* **圧力分布 $p(x,y)$ (Normal Pressure Distribution)**
Hertz 楕円接触に基づく法線力分布。中心ほど高圧になります。
* **トラクション特性 (Traction Characteristics)**
$s_x$ または $s_y$ を $0 \to 1$ までスイープした際の無次元化発生力 $F_x/F_z$, $F_y/F_z$ 曲線を表示。

---

## モデルの理論 / Model Theory

### 1. Hertz 楕円接触圧力

タイヤと路面の接触形状を楕円形と仮定します。

$$\frac{x^2}{a^2} + \frac{y^2}{b^2} \le 1$$

接触楕円内における法線圧力分布 $p(x,y)$ は、Hertzの接触理論に従い以下のように定式化されます。

$$p(x,y) = \frac{3F_z}{2\pi ab} \sqrt{1 - \frac{x^2}{a^2} - \frac{y^2}{b^2}}$$

### 2. Brush Model — 弾性せん断応力

粘着域（Adhesion zone）では、トレッドゴムの接触点が接触楕円前端から後端へ移動する間に、スリップ率に応じた弾性変形（ひずみ）が蓄積されます。

$$f_{x,e}(x,y) = G \cdot s_x \left(x + x_e(y)\right)$$

$$f_{y,e}(x,y) = G \cdot s_y \left(y + y_e(x)\right)$$

ここで、
$x_e(y) = a\sqrt{1-(y/b)^2}$
および
$y_e(x) = b\sqrt{1-(x/a)^2}$
は接触楕円の前端座標を表し、$G$ はタイヤゴムのせん断弾性率です。

### 3. 粘着・すべり判定

各メッシュ点において、蓄積された弾性応力が静摩擦限界を超えるかどうかで粘着・すべりの状態を判定します。

$$\text{すべり域 (Slip Zone Condition):} \quad \sqrt{f_{x,e}^2 + f_{y,e}^2} > \mu_s \cdot p(x,y)$$

すべり域に達した座標では、接線力の大きさは動摩擦力によって制限され、その方向は弾性変形方向（$\mathbf{f}_e$）を維持します。

$$\mathbf{f}_{\text{slip}} = \mu_d(w) \cdot p(x,y) \cdot \frac{\mathbf{f}_e}{|\mathbf{f}_e|}$$

### 4. 速度依存動摩擦係数

動摩擦係数 $\mu_d$ は、トレッドゴムと路面の局所的なすべり速度 $w$ に依存して低下します（Stribeck 効果のモデル化）。

$$\mu_d(w) = \mu_s \left[ (1-\alpha)e^{-\beta w} + \alpha \right]$$

ここで、局所すべり速度 $w$ は車両速度 $v_0$ と合成スリップ率から以下のように計算されます。

$$w = \sqrt{s_x^2 + s_y^2} \cdot v_0$$

* $\alpha = 1.0$ のとき、Coulomb 摩擦（速度非依存：$\mu_d = \mu_s$）となります。
* $\alpha < 1.0$ のとき、摩擦係数はすべり速度の増加とともに減衰し、最終的に $\alpha\mu_s$（すなわち $\mu_\infty$）へと漸近します。

### 5. 合力の計算

接触面全体で発生している局所接線力を積分し、タイヤ全体に作用する無次元化された合力を求めます。

$$\frac{F_x}{F_z} = \frac{1}{F_z} \iint_{\Omega} f_x(x,y) \,dx\,dy, \qquad \frac{F_y}{F_z} = \frac{1}{F_z} \iint_{\Omega} f_y(x,y) \,dx\,dy$$

---

## 固定パラメータ / Fixed Parameters

| 記号 | 値 | 説明 / Description |
| --- | --- | --- |
| **$a$** | $50\text{ mm}$ | 接触楕円 半長軸（進行方向） / Semi-major axis (longitudinal) |
| **$b$** | $74\text{ mm}$ | 接触楕円 半短軸（横方向） / Semi-minor axis (lateral) |
| **$F_z$** | $\approx 4448\text{ N}$ | 垂直荷重（$1815\text{ kg}$ 車両の1輪分相当：$1815g/4$） / Normal load |
| **$G$** | $10\text{ MPa}$ | タイヤゴムの横せん断弾性係数 / Shear modulus of tire rubber |
| **$v_0$** | $80\text{ km/h}$ | 基準車速 / Vehicle speed |
| **$N, M$** | $60 \times 60$ | 計算メッシュ数（解像度とリアルタイム性のトレードオフ） / Mesh resolution |

---

## ファイル構成 / Repository Structure

```text
yamazaki-model/
├── Yamazaki_Model_Visualizer_Tire.html   # メインビジュアライザ (HTML5/JS) / Main visualizer
└── CITATION.cff                           # 引用情報 / Citation metadata

```
## AI Assistance
The visualization was developed with the assistance of AIs and theoretical formulation of lateral direction was discussed with AIs, as well.

---

## 引用 / Citation

本ツールを研究、論文、または学会発表等に使用する場合は、以下のメタデータ（または `CITATION.cff`）を参照して引用してください。

*If you use this tool in your research, please cite it using the metadata in `CITATION.cff`.*

## Contact
For questions or feedback, feel free to open a [Discussion](https://github.com/hiroo718/yamazaki-model/discussions) or contact: yamazaki.hiroo@gmail.com

---

## 参考文献 / References
1. Yamazaki, H., Nagai, M. and Kamada, T., A Study of Adhesion Force Model for Wheel Slip Prevention Control, JSME International Journal, Series C, Vol. 47, No. 2, (2004), pp.496-501.
[J-STAGE](https://www.jstage.jst.go.jp/article/jsmec/47/2/47_2_496/_article)
2. 山崎大生. 動摩擦係数を考慮した 3 次元接触理論に基づく粘着力のモデル化. 日本機械学会論文集（C編）, 76巻770号, (2010), pp.82-87.
Yamazaki, H. Adhesion Model Based on Hertz Contact Theory Using Coefficient of Dynamic Friction. Transactions of the Japan Society of Mechanical Engineers, Series C, Vol. 76, No. 770, (2010), 82-87 (In Japanese)
[J-STAGE](https://www.jstage.jst.go.jp/article/kikaic1979/76/770/76_KJ00006661717/_pdf/-char/ja)
3. Masato Abe, VEHICLE HANDLING DYNAMICS, Butterworth-Heinemann, (2015), p.28-39.
4. O. Polach, Creep forces in simulations of traction vehicles running on adhesion limit, Wear, Vol.258, (2005), pp.992–1000.
5. Ahmed A. Shabana, Khaled E. Zaazaa and Hiroyuki Sugiyama, Railroad Vehicle Dynamics A Computational Approach, CRC Press, (2008), pp.133-159.
6.  Ohyama, T., Study on Influences of Contact Condition between Wheel and Rail on Adhesion Force and Its Improvement at Higher Speeds, RTRI REPORT, (in Japanese), Vol.1, No.2 (1987), pp.11–22, 34–35, Railway Technical Research Institute.
7. Kalker, J.J., Survey of Wheel-Rail Rolling Contact Theory, Vehicle System Dynamics 5, (1979), pp.317–358.
```

```
