# カメラの制御方法（OrbitControls）

Three.jsには**カメラの動きを自動的に制御する`OrbitControls`クラスが存在**する。

主な用途は以下の通り。

* 周回軌道を描くようにカメラを配置
* ポインター操作でカメラの配置やアングルを変更

## 導入方法

`OrbitControls.js`は、Three.jsライブラリの本体に含まれていないので注意。

```html
<script type="importmap">
  {
    "imports": {
      "three": "https://unpkg.com/three@0.152.2/build/three.module.js",
      "three/addons/": "https://unpkg.com/three@0.152.2/examples/jsm/"
    }
  }
</script>
```

`import`文で読み込む。

```html
<script type="module">
import * as THREE from "three";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";

// …
</script>
```

## 使い方

`OrbitControls`クラスのコンストラクターへ、カメラのインスタンスとDOM要素を引数として指定。これだけで自動的にマウスと連動してインタラクションが効く。

`OrbitControls`クラスの第2引数は、ポインター操作を受け付ける対象のDOM要素を指定する。（多くの場合`document.body`か`canvas`要素が妥当）

```JavaScript
// カメラを作成
const camera = new THREE.PerspectiveCamera(/*省略*/);
// カメラの初期座標を設定
camera.position.set(0, 0, 1000);

// カメラコントローラーを作成
const controls = new OrbitControls(camera, document.body);
```

マウス操作で以下のようにカメラを制御できる。

* オービット（周回軌道）：左ボタンでドラッグ
* ズーム：マウスホイール
* パン：右ボタンでドラッグ

## 滑らかにコントロールする

`OrbitControls`インスタンスの`enableDamping`や`dampingFactor`プロパティーを設定すると、ドラッグ時にカメラが滑らかに動く。

`enableDamping`や`dampingFactor`プロパティーを使う場合は、`requestAnimationFrame`内で`OrbitControls`インスタンスの`update`メソッドを呼び出す必要がある。

```JavaScript
// カメラを作成
const camera = new THREE.PerspectiveCamera(/*省略*/);
// カメラの初期座標を設定
camera.position.set(0, 0, 1000);

// カメラコントローラーを作成
const controls = new OrbitControls(camera, canvasElement);

// 滑らかにカメラコントローラーを制御する
controls.enableDamping = true;
controls.dampingFactor = 0.2;

tick();

// 毎フレーム時に実行されるループイベントです
function tick() {
  // カメラコントローラーを更新
  controls.update();

  // レンダリング
  renderer.render(scene, camera);

  requestAnimationFrame(tick);
}
```
