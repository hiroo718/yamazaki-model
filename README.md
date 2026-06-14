# Yamazaki Model Visualizer

### Interactive Tire Contact Force Simulator — Brush & Hertz Model

**Author:** Dr. Hiroo Yamazaki

---

Yamazaki Model Visualizer is an open-source **Executable Engineering Model (EEM)** for tire-road and wheel-rail contact mechanics.

Yamazaki Model Visualizer は、タイヤ・路面および車輪・レール間接触力学を対象としたオープンソース実行可能工学モデル（Executable Engineering Model, EEM）です。

ブラウザ上でタイヤ接触力モデルをリアルタイム可視化するスタンドアロンツールです。インストールやサーバー構築は不要です。

A self-contained, zero-dependency browser tool for real-time visualization of tire contact mechanics.

---

## Demo

<p align="center">
  <img src="figure1.gif" alt="Yamazaki Model Visualizer Demo" width="900">
</p>

*Figure: Fitting characteristics of the Yamazaki Model (solid line) against Sakai Tire experimental data (dots).*

---

## Live Demo

ブラウザで直接実行できます。

👉 **https://hiroo718.github.io/yamazaki-model/Yamazaki_Model_Visualizer_Tire.html**

### Features

- No installation required
- No server required
- Runs entirely in the browser
- Chrome / Firefox / Safari / Edge compatible
- Real-time visualization of adhesion and slip behavior
- Interactive parameter adjustment

---

## Overview

本ツールは **Yamazaki Model（山崎モデル）** を実装したインタラクティブビジュアライザです。

以下を統合しています。

- Brush Model
- Hertz Elliptical Contact Theory
- Velocity-dependent Dynamic Friction (Stribeck Effect)

リアルタイムに以下を計算・描画します。

- Adhesion / Slip Zone Distribution
- Tangential Force Distribution
- Hertz Contact Pressure Distribution
- Traction Characteristic Curves

The model has been validated against experimental tire data and provides accurate fitting of traction characteristics.

---

## Usage

### Option 1 — Run Online

ブラウザで直接実行：

👉 **https://hiroo718.github.io/yamazaki-model/Yamazaki_Model_Visualizer_Tire.html**

---

### Option 2 — Run Locally

リポジトリを取得します。

```bash
git clone https://github.com/hiroo718/yamazaki-model.git
cd yamazaki-model
```

HTMLファイルをブラウザで開きます。

**macOS**

```bash
open Yamazaki_Model_Visualizer_Tire.html
```

**Windows**

```cmd
start Yamazaki_Model_Visualizer_Tire.html
```

**Linux**

```bash
xdg-open Yamazaki_Model_Visualizer_Tire.html
```

> [!NOTE]
> サーバーや外部ライブラリは不要です。
> 最新版の Chrome, Firefox, Safari, Edge で動作します。

---

## Control Parameters

左側のコントロールパネルから以下のパラメータを変更できます。

| Parameter | Description | Range |
|------------|------------|--------|
| **sx** | Longitudinal slip ratio | 0.0 – 1.0 |
| **sy** | Lateral slip ratio | 0.0 – 1.0 |
| **μs** | Static friction coefficient | 0.10 – 0.60 |
| **α** | Dynamic friction ratio μ∞/μ0 | 0.0 – 1.0 |
| **β** | Stribeck decay coefficient | 0.01 – 0.30 |

---

## Visualization Panels

```text
┌──────────────────┬────────────────┬────────────────┐
│ Adhesion / Slip  │ Tangential |f| │ Hertz p(x,y)   │
├──────────────────┴────────────────┴────────────────┤
│      Traction Characteristics Fx/Fz, Fy/Fz         │
└────────────────────────────────────────────────────┘
```

### Adhesion / Slip Zone

- Blue = Adhesion region
- Red = Slip region

粘着域とすべり域の遷移を可視化します。

### Tangential Force Distribution

接線力

\[
|f|=\sqrt{f_x^2+f_y^2}
\]

の分布を表示します。

### Hertz Pressure Distribution

接触楕円内の法線圧力

\[
p(x,y)
\]

を表示します。

### Traction Characteristics

無次元化力

\[
F_x/F_z,\qquad F_y/F_z
\]

をスリップ率に対して描画します。

---

## Model Theory

### Hertz Elliptical Contact

接触楕円

\[
\frac{x^2}{a^2}+\frac{y^2}{b^2}\le1
\]

法線圧力分布

\[
p(x,y)
=
\frac{3F_z}{2\pi ab}
\sqrt{
1-\frac{x^2}{a^2}-\frac{y^2}{b^2}
}
\]

---

### Brush Model

縦方向

\[
f_{x,e}
=
G s_x (x+x_e)
\]

横方向

\[
f_{y,e}
=
G s_y (y+y_e)
\]

---

### Slip Criterion

\[
\sqrt{
f_{x,e}^2+f_{y,e}^2
}
>
\mu_s p(x,y)
\]

---

### Velocity-Dependent Dynamic Friction

\[
\mu_d(w)
=
\mu_s
\left[
(1-\alpha)e^{-\beta w}
+\alpha
\right]
\]

局所すべり速度

\[
w
=
\sqrt{s_x^2+s_y^2}
\,v_0
\]

---

### Resultant Forces

\[
\frac{F_x}{F_z}
=
\frac{1}{F_z}
\iint f_x(x,y)\,dx\,dy
\]

\[
\frac{F_y}{F_z}
=
\frac{1}{F_z}
\iint f_y(x,y)\,dx\,dy
\]

---

## Fixed Parameters

| Symbol | Value | Description |
|----------|----------|----------|
| a | 50 mm | Semi-major axis |
| b | 74 mm | Semi-minor axis |
| Fz | 4448 N | Normal load |
| G | 10 MPa | Rubber shear modulus |
| v0 | 80 km/h | Vehicle speed |
| Mesh | 60 × 60 | Computational grid |

---

## Repository Structure

```text
yamazaki-model/
├── README.md
├── figure1.gif
├── Yamazaki_Model_Visualizer_Tire.html
└── CITATION.cff
```

---

## Citation

If you use this software in research, publications, or presentations, please cite the metadata provided in:

```text
CITATION.cff
```

---

## AI Assistance

The visualization was developed with assistance from AI tools.
Discussions regarding the theoretical formulation of the lateral force model were also supported by AI systems.

---

## References

1. Yamazaki, H., Nagai, M. and Kamada, T. (2004). *A Study of Adhesion Force Model for Wheel Slip Prevention Control*. JSME International Journal Series C, Vol.47, No.2, pp.496–501.

2. Yamazaki, H. (2010). *Adhesion Model Based on Hertz Contact Theory Using Coefficient of Dynamic Friction*. Transactions of the Japan Society of Mechanical Engineers Series C, Vol.76, No.770, pp.82–87.

3. Abe, M. (2015). *Vehicle Handling Dynamics*. Butterworth-Heinemann.

4. Polach, O. (2005). *Creep Forces in Simulations of Traction Vehicles Running on Adhesion Limit*. Wear, Vol.258, pp.992–1000.

5. Shabana, A.A., Zaazaa, K.E., Sugiyama, H. (2008). *Railroad Vehicle Dynamics: A Computational Approach*. CRC Press.

6. Ohyama, T. (1987). *Study on Influences of Contact Condition between Wheel and Rail on Adhesion Force and Its Improvement at Higher Speeds*. RTRI Report.

7. Kalker, J.J. (1979). *Survey of Wheel-Rail Rolling Contact Theory*. Vehicle System Dynamics, Vol.5, pp.317–358.

---

## Contact

Questions, comments, suggestions, and collaboration inquiries are welcome.

GitHub Discussions:

https://github.com/hiroo718/yamazaki-model/discussions

Email:

yamazaki.hiroo@gmail.com
