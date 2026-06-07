# WSL2_CheetSeet

# Windows 11 + WSL2 + Ubuntu 24.04 + Playwright 開発環境構築手順

## 目次

* [1. 初回セットアップ](#1-初回セットアップ)
* [2. Ubuntu基本設定](#2-ubuntu基本設定)
* [3. nvmインストール](#3-nvmインストール)
* [4. pnpmインストール](#4-pnpmインストール)
* [5. VS Code連携](#5-vs-code連携)
* [6. Playwright環境構築](#6-playwright環境構築)
* [7. 開発時のルール](#7-開発時のルール)
* [8. PC再起動後の利用方法](#8-pc再起動後の利用方法)
* [9. 開発終了時](#9-開発終了時)
* [10. アンインストール方法](#10-アンインストール方法)
* [11. よく使うコマンド一覧](#11-よく使うコマンド一覧)
* [12. トラブルシューティング](#12-トラブルシューティング)

---

# 1. 初回セットアップ

## 1.1 WSL機能有効化

管理者権限のPowerShellを起動

```powershell
Enable-WindowsOptionalFeature `
  -Online `
  -FeatureName Microsoft-Windows-Subsystem-Linux `
  -NoRestart
```

Windowsを再起動する。

---

## 1.2 WSL状態確認

```powershell
wsl --status
```

期待値

```text
既定のバージョン: 2
```

---

## 1.3 Ubuntuインストール

利用可能なディストリビューション確認

```powershell
wsl --list --online
```

Ubuntu 24.04をインストール

```powershell
wsl --install -d Ubuntu-24.04
```

---

## 1.4 Ubuntu初期設定

Ubuntu起動後

```text
Enter new UNIX username:
```

ユーザー名を入力

例

```text
hogehoge
```

続いてパスワードを設定する。

---

## 1.5 インストール確認

```powershell
wsl -l -v
```

期待値

```text
NAME            STATE           VERSION
Ubuntu-24.04    Stopped         2
```

---

# 2. Ubuntu基本設定

Ubuntu起動

```powershell
wsl
```

パッケージ更新

```bash
sudo apt update
sudo apt upgrade -y
```

開発ツールインストール

```bash
sudo apt install -y \
  curl \
  git \
  build-essential
```

---

# 3. nvmインストール

## 3.1 nvm導入

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
```

設定反映

```bash
source ~/.bashrc
```

確認

```bash
nvm --version
```

---

## 3.2 Node.jsインストール

LTS版インストール

```bash
nvm install --lts
```

デフォルト指定

```bash
nvm alias default lts/*
```

確認

```bash
node -v
npm -v
```

---

# 4. pnpmインストール

```bash
npm install -g pnpm
```

確認

```bash
pnpm -v
```

---

# 5. VS Code連携

## 5.1 VS Codeインストール

Windowsへ VS Code をインストール

https://code.visualstudio.com/

---

## 5.2 WSL拡張機能インストール

VS Codeの拡張機能から

```text
WSL
```

を検索

以下をインストール

```text
Remote - WSL
```

---

## 5.3 作業ディレクトリ作成

Ubuntuで

```bash
mkdir -p ~/workspace
```

---

## 5.4 VS Code起動

```bash
cd ~/workspace
code .
```

---

# 6. Playwright環境構築

## 6.1 プロジェクト作成

```bash
mkdir playwright-sample
cd playwright-sample
```

---

## 6.2 Playwrightインストール

### npm利用

```bash
npm init playwright@latest
```

### pnpm利用

```bash
pnpm create playwright
```

---

## 6.3 Playwright動作確認

```bash
npx playwright test
```

ブラウザ起動確認

```bash
npx playwright test --headed
```

---

# 7. 開発時のルール

プロジェクトは必ずLinux側に配置する。

推奨

```text
/home/<user>/workspace
```

例

```text
/home/hogehoge/workspace/my-project
```

非推奨

```text
/mnt/c/Users/hogehoge/...
```

理由

* Gitが遅くなる
* npm installが遅くなる
* Playwright実行が遅くなる
* WSLから警告が出る

---

# 8. PC再起動後の利用方法

## 8.1 Ubuntu起動

PowerShell

```powershell
wsl
```

またはスタートメニューから

```text
Ubuntu 24.04
```

を起動

---

## 8.2 作業ディレクトリへ移動

```bash
cd ~/workspace
```

---

## 8.3 Node確認

```bash
node -v
npm -v
```

---

## 8.4 VS Code起動

```bash
code .
```

または

```bash
cd ~/workspace/my-project
code .
```

---

## 8.5 Playwright実行

```bash
cd ~/workspace/my-project
npx playwright test
```

---

# 9. 開発終了時

## Ubuntuシェル終了

```bash
exit
```

---

## WSL完全停止

PowerShell

```powershell
wsl --shutdown
```

確認

```powershell
wsl -l -v
```

期待値

```text
STATE
Stopped
```

---

# 10. アンインストール方法

## Ubuntuのみ削除

インストール済み一覧確認

```powershell
wsl -l -v
```

Ubuntu削除

```powershell
wsl --unregister Ubuntu-24.04
```

注意

* Ubuntu内の全データが削除される
* Gitリポジトリも削除される

---

## WSL機能削除

管理者PowerShell

```powershell
Disable-WindowsOptionalFeature `
  -Online `
  -FeatureName Microsoft-Windows-Subsystem-Linux
```

仮想マシンプラットフォームも削除する場合

```powershell
Disable-WindowsOptionalFeature `
  -Online `
  -FeatureName VirtualMachinePlatform
```

Windows再起動

---

# 11. よく使うコマンド一覧

## WSL起動

```powershell
wsl
```

## Ubuntu一覧

```powershell
wsl -l -v
```

## WSL停止

```powershell
wsl --shutdown
```

## Node確認

```bash
node -v
npm -v
```

## Playwright実行

```bash
npx playwright test
```

## VS Code起動

```bash
code .
```

## Git確認

```bash
git --version
```

## 現在位置確認

```bash
pwd
```

## ホームディレクトリへ移動

```bash
cd ~
```

## 作業ディレクトリへ移動

```bash
cd ~/workspace
```


# 12. トラブルシューティング

## ケース1: Ubuntuをインストールしたのに `wsl -l -v` に何も表示されない

### 症状

```powershell
wsl -l -v
```

結果

```text
Linux 用 Windows サブシステムにインストールされているディストリビューションはありません。
```

### 確認1: WSL状態確認

```powershell
wsl --status
```

期待値

```text
既定のバージョン: 2
```

---

### 確認2: Linux用Windowsサブシステムの状態確認

```powershell
Get-WindowsOptionalFeature `
  -Online `
  -FeatureName Microsoft-Windows-Subsystem-Linux
```

期待値

```text
State : Enabled
```

問題発生時

```text
State : Disabled
```

---

### 原因

WSL2の仮想化基盤は有効だが、

```text
Linux 用 Windows サブシステム
(Microsoft-Windows-Subsystem-Linux)
```

機能が無効になっている。

そのためUbuntuを登録できない。

---

### 解決方法

管理者PowerShellで実行

```powershell
Enable-WindowsOptionalFeature `
  -Online `
  -FeatureName Microsoft-Windows-Subsystem-Linux `
  -NoRestart
```

Windowsを再起動する。

---

### 再確認

```powershell
Get-WindowsOptionalFeature `
  -Online `
  -FeatureName Microsoft-Windows-Subsystem-Linux
```

期待値

```text
State : Enabled
```

---

## ケース2: Ubuntuが本当にインストールされているか確認したい

確認

```powershell
Get-AppxPackage *Ubuntu*
```

Ubuntuがインストール済みなら表示される。

表示されない場合

```text
Ubuntuが未インストール
```

である。

---

### インストール

```powershell
wsl --install -d Ubuntu-24.04
```

---

## ケース3: VirtualMachinePlatformの確認

確認

```powershell
Get-WindowsOptionalFeature `
  -Online `
  -FeatureName VirtualMachinePlatform
```

期待値

```text
State : Enabled
```

---

### 無効の場合

```powershell
Enable-WindowsOptionalFeature `
  -Online `
  -FeatureName VirtualMachinePlatform `
  -NoRestart
```

Windows再起動。

---

## ケース4: Ubuntuはあるのに起動できない

確認

```powershell
wsl -l -v
```

起動

```powershell
wsl
```

または

```powershell
wsl -d Ubuntu-24.04
```

---

## ケース5: VS CodeからWSL接続できない

確認

```bash
code .
```

で起動できるか確認。

VS Code拡張機能

```text
Remote - WSL
```

がインストールされていることを確認。

---

## ケース6: Playwrightのブラウザが起動しない

ブラウザインストール

```bash
npx playwright install
```

依存ライブラリもインストール

```bash
npx playwright install-deps
```

---

## ケース7: Node.jsが見つからない

確認

```bash
node -v
```

エラー

```text
node: command not found
```

の場合

```bash
source ~/.bashrc
```

またはUbuntuを再起動。

確認

```bash
nvm current
```

---

## ケース8: 現在のWSL状態をまとめて確認

```powershell
wsl --status
wsl -l -v
```

```powershell
Get-WindowsOptionalFeature `
  -Online `
  -FeatureName Microsoft-Windows-Subsystem-Linux

Get-WindowsOptionalFeature `
  -Online `
  -FeatureName VirtualMachinePlatform
```

```powershell
Get-AppxPackage *Ubuntu*
```

この3つを確認すると、WSL関連の問題の大半を切り分けできる。
