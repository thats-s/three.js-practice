# カメラの制御方法（座標制御）

3Dには視点を制御するカメラという概念があり、カメラの動かし方を抑えれば表現の自由度が上がる。

## 地球を中心としてカメラを回転させる

[このサンプル](camera_basic_earth.html)では地球を中心として**カメラが円周上を自動的に移動**する。  
カメラ位置の設定は`camera`オブジェクトの`position`プロパティに数値を代入する。

```JavaScript
// ラジアンに変換する
const radian = rot * Math.PI / 180;
// 角度に応じてカメラの位置を設定
camera.position.x = 1000 * Math.sin(radian);
camera.position.z = 1000 * Math.cos(radian);
```

カメラは常に中央を見るようにしたいので、`camera`オブジェクトの`lookAt()`メソッドを使って原点座標`(0, 0, 0)`を指定。  
`lookAt()`メソッドはどの位置からでも指定した座標に強制的に向かせることができる。

```JavaScript
// 原点方向を見つめる
camera.lookAt(new THREE.Vector3(0, 0, 0));
```

動きの演出については、フレーム毎に衛星の配置角度を0.5度ずつ加算し、それをカメラの座標に変換。カメラの座標は三角関数（`sin`と`cos`）を使って、角度から求める。  
※1000という値は円の半径

```JavaScript
view.camera.x = 円周の半径 * Math.sin(角度 * Math.PI / 180);
view.camera.z = 円周の半径 * Math.cos(角度 * Math.PI / 180);
```

## マウスの座標に応じて回転させる

[このサンプル](camera_mouse_x.html)では地球を中心として、**マウスの横の移動に対してカメラが移動**する。  
**マウスの位置に応じてカメラの位置を制御する**スクリプトはこちら。

```JavaScript
let rot = 0; // 角度
let mouseX = 0; // マウス座標

// マウス座標はマウスが動いた時のみ取得できる
document.addEventListener("mousemove", (event) => {
  mouseX = event.pageX;
});

tick();

// 毎フレーム時に実行されるループイベントです
function tick() {
  // マウスの位置に応じて角度を設定
  // マウスのX座標がステージの幅の何%の位置にあるか調べてそれを360度で乗算する
  const targetRot = (mouseX / window.innerWidth) * 360;
  // イージングの公式を用いて滑らかにする
  // 値 += (目標値 - 現在の値) * 減速値
  rot += (targetRot - rot) * 0.02;

  // ラジアンに変換する
  const radian = rot * Math.PI / 180;
  // 角度に応じてカメラの位置を設定
  camera.position.x = 1000 * Math.sin(radian);
  camera.position.z = 1000 * Math.cos(radian);
  // 原点方向を見つめる
  camera.lookAt(new THREE.Vector3(0, 0, 0));

  // レンダリング
  renderer.render(scene, camera);

  requestAnimationFrame(tick);
}
```

ポイントとしては、**角度の算出方法をステージの幅の何%の位置にマウスがあるかを計算で求め、それを角度に反映しているところ。**

```JavaScript
// マウスの位置に応じて角度を設定
// マウスのX座標がステージの幅の何%の位置にあるか調べてそれを360度で乗算する
const targetRot = (mouseX / window.innerWidth) * 360;
```
