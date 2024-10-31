# DockerでCドライブが逼迫した時の対処
ver1.0 : 2024.08.28  takasu


Dockerを使い続けているとCドライブの容量が足りなくなってくるため、削除する必要がある。Dockerデスクトップアプリ上で削除したり、pruneコマンドを実行しても、WSLはデータを蓄積し続けるため、WSLのデータを削除する。
[参考サイト](https://dev.classmethod.jp/articles/windows-virtual-disk-compression/)
## 整理する前に必ずDocker内のデータのバックアップをとっておくこと！
## まずはDocker内のデータを整理
1. Dockerのデスクトップアプリで、いらないコンテナ、image、volumeを削除する。
2. または、コマンドから一括削除する。詳しくは以下などを参考

   [dockerのpruneコマンドについて詳しい解説のサイト](https://zenn.dev/minedia/articles/2023-02-20-docker-416d4f98ea1b75)
3. 改善しなければ次の項目へ進む

## Dockerフォルダサイズを確認
1. WindowsのPowerShellを管理者権限で開く
2. 以下のスクリプトを実行し、サイズを確認。（ユーザー名は書き換える）
   ```
   $dockerPaths = @(
        "C:\ProgramData\Docker",
        "C:\Users\ユーザー名\AppData\Local\Docker",
        "C:\Users\ユーザー名\AppData\Roaming\Docker"
      )
   
      foreach ($path in $dockerPaths) {
        if (Test-Path $path) {
          $size = (Get-ChildItem $path -Recurse | Measure-Object -Property Length -Sum).Sum / 1GB
          [PSCustomObject]@{
            FolderPath = $path
            SizeGB = [math]::Round($size, 2)
          }
        }
      }
   ```
3. パスとサイズが表示されるので、大きすぎるデータがあれば、そのパスをコピーする
4. 「アプリと機能」→一番下にスクロールして「プログラムと機能」→「Windowsの機能の有効化または無効化」→「Hyper-V」にチェックしOK→PC再起動（Hyper-Vがすでにインストールされていればこの手順は不要）
5. PowerShellを管理者権限で開き、さきほど取得したパスを参照しつつ以下のコマンドでディレクトリに移動
   ```
   cd C:\Users\ユーザー名\AppData\Local\Docker\wsl\data
   ```
6. 以下のコマンドで仮想ディスクの圧縮
   ```
   Optimize-VHD -Path .\ext4.vhdx
   ```
7. Cドライブを確認し空きができていれば完了