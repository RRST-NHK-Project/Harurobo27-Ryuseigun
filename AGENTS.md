# AGENTS.md

AIコーディングエージェント向けの作業ガイド。人間向けの説明は [README.md](README.md) と [docs/](docs/README.md) を参照。

## プロジェクト概要

関西春ロボコン2027 立命館大学 **流星群** 用の ROS 2 ワークスペース（`~/ros2_ws/src` に clone する前提）。
Ubuntu 24.04 + ROS 2 Jazzy。パッケージ構成とノードの起動方法は README.md の 4・5 章を参照。

`ros2can` / `serial_bridge` / `microcontroller-ws` / `rrst-console` / `virtual_ps4` は **サブモジュール**。
サブモジュール内を変更する場合はそのリポジトリ側の AGENTS.md / README.md（例: `ros2can/AGENTS.md`）に従う。

## コマンド

```bash
# ワークスペースルート（~/ros2_ws）で
colcon build --symlink-install
colcon build --symlink-install --packages-select <パッケージ名>
source install/setup.bash
```

## Git の管理

**作業を始める前と、コミット・プッシュする前に必ずブランチの状態を確認する。**

```bash
git status                     # 今いるブランチ、未コミットの変更、untracked ファイル
git branch -vv                 # ローカルブランチと追跡先、ahead / behind
git fetch && git status -sb     # リモートに対して遅れていないか
git submodule status            # 各サブモジュールのコミットと状態
```

- `main` は動作確認済みの安定版。**`main` に直接コミットしない。** 作業用ブランチを切ってから作業する。
  `main` 上で作業を頼まれた場合は、ブランチを切るか作業者に確認する。
- 作業者の未コミットの変更が残っている場合は、勝手に stash / reset / checkout で消さない。まず作業者に伝える。
- リモートより遅れている（behind）場合は、作業前に作業者へ伝え、pull するかを確認する。
- `git submodule status` の行頭が `-`（未初期化）なら `git submodule update --init --recursive` が必要。
  `+`（記録と違うコミット）や detached HEAD の場合は、意図したものか作業者に確認する。
- サブモジュールの中を変更したときは、**サブモジュール側でコミット・プッシュしてから**、親リポジトリでサブモジュールの参照を更新してコミットする。順番を逆にすると他の人が clone したときに参照先が見つからない。
- コミット・プッシュ・ブランチ削除・force push は作業者に頼まれたときだけ行う。

## 記録を残す

作業者には **定期的に作業記録を `docs/` に残すことを勧める。** 区切りのいいところ（ノードを足した、トピックや
パラメータを変えた、実機で動いた / 動かなかった、その日の作業を終える）で、記録を書くよう声をかける。

- 書く内容のめやすは [docs/README.md](docs/README.md) の「書くときのめやす」に従う
  （ノードの役割、トピック名と型、パラメータと既定値、起動コマンド、ハマりどころ）。
- 作業者に頼まれたら、その回の変更内容から記録の下書きを作ってよい。実機で確認していないことは「未検証」と明記する。
- トピック名・型・パラメータを変えたのに記録が古いままになっていたら、更新を提案する。

## その他の注意

- `docs/slides/` の pptx は講習資料。頼まれない限り編集しない。
- `docs/answers/` は講習課題の解答例で `COLCON_IGNORE` 済み。ビルド対象ではない。
- ビルドや実機での確認ができなかった場合は、その旨をはっきり伝える。
