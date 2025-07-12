# Bitz2DGeo

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Bitz2DGeoは、BitzLabsエコシステム内で、CADデータのような正確な2D幾何学情報とトポロジーを扱うための専門ライブラリです。BREP (Boundary Representation) モデルに基づいています。

## 主な機能

-   **`Geo2DAST`の定義**: Vertex, Edge, Face, Loopといったトポロジー情報を含む、高精度な2DジオメトリモデルのためのAST。曲線(B-スプライン、ベジェ)や、レイヤー、グループ化もサポートします。
-   **パーサー**: DXFなどの標準的なCADフォーマットを解釈し、`Geo2DAST`を生成します。
-   **専用API**: プログラムコードから、幾何学的な制約を保ちながら図形を構築できます。
-   **ジオメトリエンジン**: `Geo2DAST`を解釈し、描画のための`LayoutAST`を生成します。また、面積計算やオフセット生成などの高度な幾何演算機能も提供します。

## このライブラリの位置づけ

このライブラリは、BitzLabsを単なる図表ツールから、プロフェッショナルな2D CADやテクニカルイラストレーションの領域へと拡張します。`BitzDoc`と連携し、設計図や仕様書に精密な図面を埋め込むことを可能にします。

### 利用シナリオ

-   **DSL経由**: 仕様書にCADライクな簡略図（例: `rectangle (0,0) to (100,50)`) を記述する。
-   **API経由**: 製造業向けの設計ドキュメントや、複雑な図面をプログラムで自動生成する。

### 依存関係

-   `BitzAstCore`
-   `BitzParser`
-   `BitzLayout` (出力型として)

## **✅ 初期開発ToDoリスト**

1.  **`Geo2DAST`の型定義**:
    *   `IGeometryNode` インターフェースを作成（`IAstNode`を継承）。
    *   **基本要素**: `Vertex` (point: `{ x: double, y: double }`), `Edge` (`id`, `startVertexId`, `endVertexId`, `geometry`), `Face` (`id`, `outerLoopId`, `innerLoopIds`) のクラスを定義。
    *   **ジオメトリ**: `Edge`の`geometry`プロパティに、`LineGeometry`, `ArcGeometry` (center, radius, startAngle, endAngle), `BsplineGeometry` (controlPoints) などの型を定義。
    *   **トポロジー**: `Loop`クラス（`edgeIds`のリスト）、`Geo2DAST`ルートノードには`vertices`, `edges`, `faces`, `loops`といったコレクションを持たせる。
    *   **属性・スタイル**: `attributes`プロパティを各要素に持たせ、`style`オブジェクト (`fill`, `stroke`, `strokeWidth`, `lineType`など) を定義。

2.  **専用API (ビルダー) の設計**:
    *   `Geo2DBuilder` クラスを作成。
    *   `builder.AddVertex(x, y)`, `builder.AddLine(v1, v2, style)` のようなメソッドで図形を構築できるようにする。
    *   曲線や面を作成するメソッドも用意する。

3.  **パーサーの骨格**:
    *   `DxfParser` (または簡略版) クラスを作成。
    *   `public IGeometryNode Parse(string dxfText)` メソッドを定義。DXFフォーマットの基本的な構造（`LINE`, `CIRCLE`, `ARC`, `POLYLINE`などのエンティティ）を読み取り、`Geo2DAST`に変換するロジックを実装する。
    *   *注意*: DXFパーサーは非常に複雑になりがちなので、まずはごく簡単なサブセット（線、円弧程度）から始めるのが現実的です。

4.  **レイアウトエンジンのインターフェース**:
    *   `Geo2DLayoutEngine` クラスを作成。
    *   `public LayoutNode Render(IGeometryNode geometryNode)` メソッドを定義。
    *   **内部ロジック**:
        *   `Geo2DAST`の`Vertex`, `Edge`, `Face`などのノードを走査する。
        *   各ノードのジオメトリ定義（線、円弧、B-スプラインなど）に基づいて、`BitzLayout`の`Path`, `Line`, `Rect`などのプリミティブを生成する。
        *   `attributes` や `style` を`LayoutAST`のスタイルにマッピングする。
        *   トポロジー情報（接続関係）は、直接`LayoutAST`に持ち越す必要はないが、レイアウト計算（例: 接続線の描画）で活用する。
