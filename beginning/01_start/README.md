# Three.jsの基本構造

## THREE.Sceneクラス

3Dの空間を表すクラス。3Dのオブジェクトはシーンに`add()`メソッドを利用して追加することで表示できる。

## THREE.PerspectiveCameraクラス

3D空間を撮影するカメラ。視点を制御するために使用。  
3Dのどの視点で撮影しているのかの情報が必要。

## THREE.WebGLRendererクラス

3D空間のレンダリングを行う。レンダリングとは、Three.jsで計算した3Dのオブジェクトを画面に表示すること。  
内部的にはThree.jsがWebGLのAPIを使って、GPUで作業を計算させ画面に表示させている。  
Three.jsでは`requestAnimationFrame`のタイミングにあわせてレンダリングを行うように設定する。
