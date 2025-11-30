# ハンズオン用メールサーバー 構築スクリプト

## 構築手順

1. 手元のPCにansible-playbook / passlibをインストール
2. Ubuntu 24.04の仮想マシンを1台用意
3. preparationディレクトリ直下に「hosts」という名前で以下の内容のファイルを作成

    ```plain
    [server]
    {仮想マシンのIPアドレス}

    [server:vars]
    ansible_ssh_user="{ansibleを実行するユーザー名}"
    ansible_sudo_pass="{sudo権限を実行するためのパスワード}"
    ansible_ssh_private_key_file="{仮想マシンにアクセスする秘密鍵のパス}"
    password="{一般ユーザーのパスワード}"
    trainer_password="{trainerユーザー(メール受信用)のパスワード}"
    ```

    例:

    ```plain
    [server]
    163.43.113.65

    [server:vars]
    ansible_ssh_user="ubuntu"
    ansible_sudo_pass="password"
    ansible_ssh_private_key_file="~/.ssh/id_ed25519"
    password="sample_password"
    trainer_password="another_sample_password"
    ```

4. preparationディレクトリ直下で以下のコマンドを実行

    ```bash
    ansible-playbook -i hosts -v playbook.yaml
    ```

## 受信環境を整える

ご使用のメールクライアントに、以下のように設定を加えて下さい。  
Thunderbird等、ログイン用のユーザー名が個別に設定できるメールクライアントを推奨します。

### 受信メールサーバー

- プロトコル: IMAP
- サーバー: 構築したサーバーのIPアドレス
- ポート: 143
- 暗号化方式: なし
- 認証方式: 通常のパスワード認証
- ユーザー名: trainer
- パスワード: hostsで設定した`trainer_password`

### 送信メールサーバー

- サーバー: 構築したサーバーのIPアドレス
- ポート: 587
- 暗号化方式: なし
- 認証方式: 通常のパスワード認証
- ユーザー名: trainer
- パスワード: hostsで設定した`trainer_password`
