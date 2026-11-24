# **Harurobo27-Ryuseigun**

## 1. 概要
関西春ロボコン2027に出場する、立命館大学 **流星群** 用のリポジトリです。[rrst-ros2-ws](https://github.com/RRST-NHK-Project/rrst-ros2-ws) をforkして作成しています。

|  | 説明 | ビルド状況 |
|---|---|---|
| **main** | 安定版ブランチ（動作確認済み） | [![ROS 2 Jazzy Build](https://github.com/RRST-NHK-Project/Harurobo27-Ryuseigun/actions/workflows/main_jazzy_build_and_test.yml/badge.svg?branch=main)](https://github.com/RRST-NHK-Project/Harurobo27-Ryuseigun/actions/workflows/main_jazzy_build_and_test.yml) |

---

## 2. 動作環境

| 項目 | 内容 |
|:---|:---|
| OS | Ubuntu 24.04 LTS |
| ROS | ROS 2 Jazzy |
| メモリ | 16 GB 以上推奨 |



---

## 3. はじめに

### 3.1 ワークスペースの作成
作成済みの場合はスキップしてください。

    mkdir -p ~/ros2_ws/src


### 3.2 リポジトリのクローン

    cd ~/ros2_ws/src
    git clone git@github.com:RRST-NHK-Project/Harurobo27-Ryuseigun.git .
    git submodule update --init --recursive
 

**サブモジュールのロードを確認すること。**

### 3.3 ビルド

    cd ~/ros2_ws
    colcon build --symlink-install



### 3.4 環境の読み込み

    source ~/ros2_ws/install/setup.bash



---

## 4. パッケージ構成

### ROS 2 パッケージ

| パッケージ | ビルドタイプ | 説明 |
|:---|:---|:---|
| `common` | ament_cmake | ヘッダのみ。PID / P-D 制御、ros2can・serial_bridge 用のパケット操作クラス |
| `teleop_tools` | ament_cmake | ホスト側制御ノード |
| `ros2can_example` | ament_cmake | ros2can のサンプルプログラム |
| `cpp_pubsub` | ament_cmake | 講習用の Pub/Sub サンプル |
| `ros2can` | ament_python | CAN バスに対応した通信ノード（サブモジュール） |
| `serial_bridge` | ament_cmake | シリアル通信ノード（サブモジュール）。ros2can の前身 |

### 非 ROS パッケージ（サブモジュール）

`package.xml` を持たないため、colcon は自動的にスキップします。`COLCON_IGNORE` は不要です。

| ディレクトリ | 説明 |
|:---|:---|
| `microcontroller-ws` | マイコン側ファームウェア（STM32 / ESP32 / Pico / B-G431B-ESC1） |
| `rrst-console` | R1 / R2 統合 GUI |
| `virtual_ps4` | 仮想 PS4 コントローラー（キーボード版・GUI版） |

---

## 5. ノードの実行

| 用途 | コマンド |
|:---|:---|
| CAN 通信ノード | `ros2 launch ros2can ros2can.launch.py` |
| シリアル通信ノード | `ros2 launch serial_bridge serial_bridge.launch.py` |
| ホスト側制御（ros2can） | `ros2 run teleop_tools ros2can_host` |
| ホスト側制御（serial_bridge） | `ros2 run teleop_tools sb_host` |
| 通信テスト | `ros2 run teleop_tools sb_tester` |
| CubeMars 位置制御サンプル | `ros2 run ros2can_example cubemars_position_example` |
| サーボ動作サンプル | `ros2 run ros2can_example servo_sweep_example` |
| 仮想 PS4 コントローラー | `python3 src/virtual_ps4/virtual_ps4_gui.py` |

ros2can の設定は `src/ros2can/config/ros2can.yaml` で行います。仮想 PS4 コントローラーは初回のみ `src/virtual_ps4/setup_uinput.sh` の実行が必要です。

---

## 6. ドキュメント

`docs/slides/` に講習資料（pptx）を置いています。

- `ROS2_Jazzy_入門_01` ～ `05`（概要と用語 / ワークスペースとcolcon / パッケージの作成 / PubSub Python / PubSub C++）
- `Ubuntu・ROS2_基本コマンドガイド`
- `serial_bridge講習` / `ros2can講習` / `serial_bridgeからros2canへ_移行ガイド`
- `turtle_teleop講習` / `仮想PS4コントローラー_セットアップ`

`docs/answers/` には講習課題の解答例が入っています（`COLCON_IGNORE` 済み）。

---

## 7. クレジット
Developed by NHK Project, RRST, Ritsumeikan University, Japan (2024)
- 公式サイト: https://www.rrst.jp
- X (Twitter): https://x.com/RRST_BKC

![Logo](https://www.rrst.jp/img/logo.png)

---
