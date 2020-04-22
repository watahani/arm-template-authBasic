# arm-template-authBasic

よく作るような環境のテンプレートを作成してみました。
リソースグループから、テンプレート作れるので、みんなもこれを参考にやってみよう！


[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fryuya-M%2farm-template-authBasic%2fmaster%2fvm-myenv-template-ver3.json)

### パラメータの補足

Location : 有効な場所を指定しないといけません。以下のコマンドなどで指定する際の文字列を確認してください。
```
az account list-locations --query "[].name"
```

Virtual Machine Prefix : VM の Prefix。Prefix-dc とか Prefix-adfs などの名前で作成される。
Prefix-proxy (Azure AD App Proxy を想定) があり、VM のマシン名は 15 字以下などの制限があるので、9 字以内にしてね。

Virtual Machine Size : 後から変更できます。

Dns Prefix : Public IP Address に対する DNS 名。DNSPrefix-VMName.<reagion>~~~ という DNS 名になります。
  
### デプロイされるイメージ

![](./armViewer.png)

## next step

以下のテンプレートを同じリージョンに Deploy しよう

https://github.com/shibayan/keyvault-acmebot/blob/master/README_ja.md
