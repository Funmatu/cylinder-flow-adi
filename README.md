# Flow Around a Cylinder (ADI Method)

円柱周りの非圧縮性粘性流れをWebブラウザ上で解く数値流体力学（CFD）シミュレーションです。
流れ関数-渦度法（Stream Function-Vorticity formulation）を用い、ADI法による高速かつ安定な計算を実現しています。

[👉 デモをブラウザで試す](https://funmatu.github.io/cylinder-flow-adi/)

## 概要

流体が円柱に衝突し、剥離してカルマン渦（Von Kármán vortex street）を形成する様子をリアルタイムで可視化します。
純粋なJavaScriptで実装されており、外部ライブラリには依存していません。



## 数学的モデル

### 支配方程式
2次元極座標系 $(r, \theta)$ における非圧縮性ナビエ・ストークス方程式を、**流れ関数-渦度法**を用いて記述しています。

1.  **渦度輸送方程式** (移流・拡散):
    $$\frac{\partial \omega}{\partial t} + (\mathbf{u} \cdot \nabla)\omega = \nu \nabla^2 \omega$$
2.  **流れ関数のポアソン方程式**:
    $$\nabla^2 \psi = -\omega$$

### 数値解法の特徴

* **空間離散化**:
    * 極座標グリッド $(r, \theta)$ を採用。
    * **不等間隔メッシュ**: `tanh`関数を用いた座標変換により、円柱表面付近の境界層を細かく解像しています。
* **時間発展 (ADI法)**:
    * **ADI (Alternating Direction Implicit) 法**を採用。$r$ 方向と $\theta$ 方向を半ステップごとに交互に陰的に解くことで、陽解法に比べて大きな時間刻みでも安定した計算が可能です。
    * 連立一次方程式は **TDMA (Thomas Algorithm)** で高速に解いています。
* **ポアソン方程式の解法**:
    * SOR法 (Successive Over-Relaxation) による反復解法。
* **境界条件**:
    * 壁面渦度: 2次精度の **Woodsの公式** を使用。

## 機能・操作方法

* **Reynolds Number (Re)**: レイノルズ数を変更し、流れの性質（層流、層流剥離、渦放出）の変化を観察できます。
* **Visualization**: 以下の物理量を切り替えて表示可能です。
    * Streamlines (流線)
    * Vorticity (渦度 - 渦の強さ)
    * Velocity Field (速度ベクトル)
    * Pressure (圧力場 - 圧力ポアソン方程式をSOR法で計算)
* **Solver**: ADI法（推奨）と陽解法（Explicit）の比較が可能です。

## 技術スタック
* HTML5 Canvas API (Rendering)
* Vanilla JavaScript (ES6+)

---
License: MIT
