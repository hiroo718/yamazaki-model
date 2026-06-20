# Yamazaki Model Visualizer

### Interactive Tire Contact Force Simulator — Brush & Hertz Model

**Author:** Dr. Hiroo Yamazaki

- First release: May 9, 2026
- Current version: v2.0 (June 20, 2026)

---

The Yamazaki Model aims to bridge automotive tire dynamics and railway adhesion theory through a common rolling-contact formulation.

The Yamazaki Model Visualizer is an open-source Executable Engineering Model (EEM) for tire-road and wheel-rail contact mechanics.

Yamazaki Model Visualizer は、タイヤ・路面および車輪・レール間接触力学を対象としたオープンソース実行可能工学モデル（EEM）です。

ブラウザ上でタイヤ接触力モデルをリアルタイム可視化するスタンドアロンツールです。インストールやサーバー構築は不要です。

A self-contained, zero-dependency browser tool for real-time visualization of tire contact mechanics.

---

## 概要 / Overview

本ツールは **山崎モデル（Yamazaki Model）** を実装したインタラクティブビジュアライザです。

Brush Model（ブラシモデル）、Hertz 楕円接触圧力、および速度依存動摩擦モデルを統合し、接触楕円内の粘着／すべり領域、接線力分布、およびトラクション特性をリアルタイムに計算・可視化します。

This tool implements the Yamazaki tire friction model by integrating the Brush Model, Hertzian elliptical contact pressure, and velocity-dependent dynamic friction.

It renders adhesion/slip zone maps, tangential force distributions, and traction characteristic curves in real time.

---

## 特徴 / Features

- リアルタイム計算・可視化
- 粘着／すべり領域の表示
- 接線力分布のヒートマップ表示
- Hertz 接触圧分布の可視化
- 縦・横スリップ連成解析
- 速度依存摩擦（Stribeck 効果）
- スタンドアロン動作（依存ライブラリ不要）

---

## 使い方 / Usage

### ステップ 0 — ブラウザで直接開く（インストール不要）

👉 https://hiroo718.github.io/yamazaki-model/Yamazaki_Model_Visualizer_Tire.html

---

### ステップ 1 — リポジトリをクローン

Repository:

```text
https://github.com/hiroo718/yamazaki-model
```

```bash
git clone https://github.com/hiroo718/yamazaki-model.git
cd yamazaki-model
```

---

### ステップ 2 — HTMLファイルをブラウザで開く

#### macOS

```bash
open Yamazaki_Model_Visualizer_Tire.html
```

#### Windows

```cmd
start Yamazaki_Model_Visualizer_Tire.html
```

#### Linux

```bash
xdg-open Yamazaki_Model_Visualizer_Tire.html
```

> [!NOTE]
>
> サーバーや依存ライブラリのインストールは不要です。
>
> Chrome / Firefox / Safari / Edge の最新版で動作します。
>
> No server or library installation is required.

---

## 操作パネル / Control Panel

左側のコントロールスライダーで以下のパラメータをリアルタイムに変更できます。

| パラメータ | 説明 | 範囲 |
| --- | --- | --- |
| **$s_x$** | 縦スリップ率 | $0 \sim 1.0$ |
| **$s_y$** | 横スリップ率 | $0 \sim 1.0$ |
| **$\mu_s$** | 静摩擦係数 | $0.50 \sim 1.50$ |
| **$\alpha$** | 動摩擦比 $\mu_\infty / \mu_0$ | $0 \sim 1.0$ |
| **$\beta$** | Stribeck 減衰率 $[\mathrm{s/m}]$ | $0 \sim 0.01$ |

---

## 表示パネル / Visualization Panels

```text
┌──────────────────┬────────────────┬────────────────┐
│  粘着/すべり域   │ 接線力 |f| 分布 │ 圧力分布 p(x,y)│
│ Adhesion / Slip  │ Tangential |f| │ Hertz Pressure │
├──────────────────┴────────────────┴────────────────┤
│         トラクション特性 Fx/Fz, Fy/Fz             │
│          (sx sweep / sy sweep 切替)               │
└────────────────────────────────────────────────────┘
```

### 粘着／すべり域

- 青：粘着域（弾性変形支配）
- 赤：すべり域（摩擦飽和）

### 接線力分布

接線力の大きさ

$$
|f|=\sqrt{f_x^2+f_y^2}
$$

をヒートマップ表示します。

### 圧力分布

Hertz 楕円接触理論に基づく法線圧力分布を表示します。

### トラクション特性

縦スリップ率または横スリップ率を掃引した際の

$$
F_x/F_z
$$

および

$$
F_y/F_z
$$

を表示します。

---

## モデルの理論 / Model Theory

### 1. Hertz 楕円接触圧力

タイヤと路面の接触形状を楕円形と仮定します。

$$
\frac{x^2}{a^2}+\frac{y^2}{b^2}\le1
$$

法線圧力分布は次式で与えられます。

$$
p(x,y)=
\frac{3F_z}{2\pi ab}
\sqrt{
1-\frac{x^2}{a^2}-\frac{y^2}{b^2}
}
$$

---

### 2. Brush Model — 弾性せん断応力

接触境界までの距離を

$$
x_e(y)=a\sqrt{1-\left(\frac{y}{b}\right)^2}
$$

$$
y_e(x)=b\sqrt{1-\left(\frac{x}{a}\right)^2}
$$

と定義します。

弾性せん断応力は、

$$
f_{x,e}(x,y) = K_x s_x\frac{x_e(y)-x}{2x_e(y)}
$$

$$
f_{y,e}(x,y) = K_y s_y\frac{y_e(x)-y}{2y_e(x)}
$$

で与えられます。

---

### 3. 粘着・すべり判定

接線応力の大きさを

$$
|f_e| = \sqrt{f_{x,e}^2+f_{y,e}^2}
$$

とすると、

$$
|f_e|>\mu_s p(x,y)
$$

の場合に滑り状態へ遷移します。

滑り状態では、

$$
\mathbf{f} = \mu_d(w)p(x,y)\frac{\mathbf{f}_e}{|f_e|}
$$

とします。

---

### 4. 速度依存動摩擦係数

動摩擦係数は Stribeck 型モデルを用いて、

$$
\mu_d(w) = \mu_s\left[(1-\alpha)e^{-\beta w}+\alpha\right]
$$

と表現します。

相対すべり速度は

$$
w=v_0\sqrt{s_x^2+s_y^2}
$$

です。

---

### 5. 合力の計算

接触領域全体で積分して、

$$
F_x = \iint_{\Omega}f_x(x,y)\,dA
$$

$$
F_y = \iint_{\Omega}f_y(x,y)\,dA
$$

を求めます。

表示値は無次元化された

$$
F_x/F_z
$$

および

$$
F_y/F_z
$$

です。

---

## 固定パラメータ / Fixed Parameters

| 記号 | 値 | 説明 |
| --- | ---: | --- |
| $a$ | 100 mm | 接触楕円半長軸 |
| $b$ | 121.5 mm | 接触楕円半短軸 |
| $F_z$ | 3923 N | 垂直荷重 |
| $v_0$ | 20 km/h | 車速 |
| $K_x$ | 0.33 MPa | 縦方向ブラシ剛性 |
| $K_y$ | 0.66 MPa | 横方向ブラシ剛性 |
| $N, M$ | 60 × 60 | 計算メッシュ数 |

---

## ファイル構成 / Repository Structure

```text
yamazaki-model/
├── README.md
├── Yamazaki_Model_Visualizer_Tire.html
├── archive/
│   └── Yamazaki_Model_Visualizer_Tire_v1.html
└── CITATION.cff
```

---

## Version History

### v2.0 (June 20, 2026)

- Replaced the shear modulus formulation with brush stiffness parameters $K_x$ and $K_y$
- Aligned the browser implementation with the Python reference model (`cond = 4`)
- Updated default parameters
- Removed experimental comparison figures
- Revised the README documentation

### v1.0 (May 9, 2026)

- Initial browser implementation
- Simplified brush model using shear modulus $G$

Older versions are available in the `archive/` directory.

---

## AI Assistance

The visualization interface and software implementation were developed with the assistance of AI systems.

AI assistance was used for code generation, documentation support, and interface design.

The theoretical formulation and validation of the model remain the responsibility of the author.

---

## 引用 / Citation

本ツールを研究、論文、または学会発表等に使用する場合は、`CITATION.cff` を参照して引用してください。

If you use this tool in your research, please cite it using the metadata in `CITATION.cff`.

---

## Contact

For questions or feedback:

- Email: yamazaki.hiroo@gmail.com
- GitHub Discussions: https://github.com/hiroo718/yamazaki-model/discussions

---

## 参考文献 / References

1. Yamazaki, H., Nagai, M., and Kamada, T., *A Study of Adhesion Force Model for Wheel Slip Prevention Control*, JSME International Journal, Series C, Vol. 47, No. 2, 2004, pp. 496–501.

2. Yamazaki, H., *Adhesion Model Based on Hertz Contact Theory Using Coefficient of Dynamic Friction*, Transactions of the Japan Society of Mechanical Engineers, Series C, Vol. 76, No. 770, 2010, pp. 82–87.

3. Abe, M., *Vehicle Handling Dynamics*, Butterworth-Heinemann, 2015.

4. Polach, O., *Creep Forces in Simulations of Traction Vehicles Running on Adhesion Limit*, Wear, Vol. 258, 2005, pp. 992–1000.

5. Shabana, A. A., Zaazaa, K. E., and Sugiyama, H., *Railroad Vehicle Dynamics*, CRC Press, 2008.

6. Ohyama, T., RTRI Report, Vol. 1, No. 2, 1987.

7. Kalker, J. J., *Survey of Wheel-Rail Rolling Contact Theory*, Vehicle System Dynamics, 1979.
