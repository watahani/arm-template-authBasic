# Key Vault のテスト

KeyVault と VM のリージョンが異なるとうまくいかないよ！！！
順にコマンドを入力すればできるようになっています。そのはず。

### portal と同じ証明書の表記をする

参考 : [証明書を使用するように IIS を構成します。](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/tutorial-secure-web-server)

事前に KeyVault 名を `$kv = "aaa"` として入力しておく必要があるよ。
```
az keyvault certificate list --vault-name $kv --query "[].{Name:name,Thum:x509ThumbprintHex,State:attributes.enabled,Expire:attributes.expires}" -o table
```

### ここから、VM の証明書ストアにぶち込む

KeyVault の ID を取得
```
$vaultid = az keyvault show -n $kv --query "id"
```

az keyvault certificate list 出力の `$certName = ` タブを `-n` の引数に渡して詳細を得る。

```
az keyvault certificate show --vault-name $kv -n $certName
```
以下のように `$secreturl` 変数に入れる。
```
$secreturl = az keyvault certificate show --vault-name $kv -n $certName --query "sid"
```

#### 以下のコマンドから、ID を変数に入れる。

どの VM か知らないので、以下の VM 一覧から、ID を set する -> `$vmid = ` 
```
az vm list -o table --query "[].{Name:name,rg:resourceGroup,ID:id}" -o table
```

### 割り当てるコマンド

以下で完了！
```
$vmid =
az vm secret add --keyvault $vaultid --certificate $secreturl --ids $vmid
```
KeyVault と VM のリージョンが違うとできないよ！

> PS D:\github\github-portal\github-portal> az vm secret add --keyvault $vaultid --certificate $kvurl --ids $targetvmids
> Deployment failed. Correlation ID: 798d11b6-c5ae-4b1e-bea2-55313759261d. The Key Vault https://rymiyaba-acme-bdgl.vault.azure.net/secrets/test-rymiyaba-com/d582e855a92a488fb6a7c0ef69b33823 is located in location southcentralus, which is different from the location of the VM, japaneast. The VM and Key Vault need to be located within the same region.
