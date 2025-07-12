# Bitz2DGeo

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Bitz2DGeoは、BitzLabsエコシステム内で、CADデータのような正確な2D幾何学情報とトポロジーを扱うための専門ライブラリです。BREP (Boundary Representation) モデルに基づいています。

**注意: このリポジトリはBitzLabsの第2目標で本格的に開発されます。**

## 主な機能

-   **`Geo2DAST`の定義**: Vertex, Edge, Face, Loopといったトポロジー情報を含む、高精度な2DジオメトリモデルのためのAST。曲線(B-スプライン、ベジェ)や、レイヤー、グループ化もサポートします。
-   **パーサー**: DXFなどの標準的なCADフォーマットを解釈し、`Geo2DAST`を生成します。
-   **専用API**: プログラムコードから、幾何学的な制約を保ちながら図形を構築できます。
-   **ジオメトリエンジン**: `Geo2DAST`を解釈し、描画のための`LayoutAST`を生成します。また、面積計算やオフセット生成などの高度な幾何演算機能も提供します。

## ✅ 初期開発ToDoリスト

1.  **`Geo2DAST`の型定義**:
    *   `IGeometryNode`インターフェース(`IAstNode`継承)を定義。
    *   `Vertex`, `Edge` (LineGeometry, ArcGeometryなどを含む), `Face`, `Loop`の主要クラスの「殻」を作成。
2.  **専用API (ビルダー) の設計**:
    *   `Geo2DBuilder`クラスのインターフェースを設計。`AddVertex`, `AddLine`などのメソッドシグネチャを定義。
3.  **パーサーの骨格**:
    *   `DxfParser`クラスを作成し、`Parse(string dxfText)`メソッドの骨格を実装。
4.  **レイアウトエンジンのインターフェース**:
    *   `Geo2DLayoutEngine`クラスを作成し、`Render(IGeometryNode node)`メソッドを定義。

## このライブラリの位置づけ

このライブラリは、BitzLabsを単なる図表ツールから、プロフェッショナルな2D CADやテクニカルイラストレーションの領域へと拡張します。`BitzDoc`と連携し、設計図や仕様書に精密な図面を埋め込むことを可能にします。

### 依存関係

-   `BitzAstCore`
-   `BitzParser`
-   `BitzLayout` (出力型として)
