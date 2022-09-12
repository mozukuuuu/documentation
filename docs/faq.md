---
description: よくある質問
---

# よくある質問

Amarok公開テストネットの「よくある質問」一覧です。

### 対応するチェーンは？

最新のリストは[Supported Chains](basics/chains.md)見ることができます。

### どのような資産に対応していますか？

資産の住所は、[配備先契約住所](https://docs.connext.network/developers/testing-against-testnet#deployed-contract-addresses)ご確認ください。

### canonical」「representation」「adopted」「local」アセットとはどういう意味ですか？

<figure><img src="https://docs.connext.network/img/faq/assets.png" alt=""><figcaption></figcaption></figure>

### トークンの正規の詳細を調べるにはどうしたらいいですか？

トークンの正規の domainId と tokenId は、`TokenRegistry` の[`getTokenId`](https://github.com/connext/nxtp/blob/3d0af2251b2d8d244d2617be6fb738c09a571022/packages/deployments/contracts/contracts/core/connext/helpers/TokenRegistry.sol#L176)関数を呼び出すことで取得できます。

例

* 興味のあるトークンは、Rinkeby上のTestERC20`(0x3FFc03F05D1869f493c7dbf913E636C6280e0ff9`) です。そのcanonical domainIdとtokenIdを把握したい。
* リンケージ上のトークンレジストリの契約アドレスは[こちらから](https://docs.connext.network/developers/testing-against-testnet#deployed-contract-addresses)ご確認ください。
*   `getTokenIdを`呼び出す（この例ではFoundryの`キャストを`使用してコントラクトから読み取る）

    ```
    cast call --chain rinkeby 0x1A3BA482D98CCB858AEacB3B839f952390099cE6 "getTokenId(address)(uint32,bytes32)" "0x3FFc03F05D1869f493c7dbf913E636C6280e0ff9" --rpc-url <rinkeby_rpc_url>.
    ```

    リターンです。

    ```
    3331 # 正規の domainId は Goerli0x00000000000026fe8a8f86511d678d031a022e48fff41c6a3e3b # 正規の bytes32 tokenId
    ```
*   Goerli上の正規のトークンのアドレスを取得するには、Goerliの`TokenRegistryの` `getLocalAddress`関数を呼び出します。

    ```
    cast call --chain goerli 0x51192fD98635FD32C2bfc0A2F4e362D864A4B8b1 "getLocalAddress(uint32,bytes32)(address)" "3331" "0x00000000000026fe8a8f86511d678d031a022e48fff41c6a3e3b" --rpc-url <goerli-rpc-url>。
    ```

    リターンです。

    ```
    0x7ea6eA49B0b0Ae9c5db7907d139D9Cd3439862a1 # 正規の TestERC20 のコントラクトアドレスを示す。
    ```

### 目的地側のターゲット関数をテストしたいだけの場合はどうすればいいのでしょうか？

トークン転送がない場合は、`transactingAssetId: address(0)` **と** `amount: 0`.を設定するだけです。

### AMBの契約に何か必要なものはありますか？

いいえ、AMBの契約を直接展開したり、やり取りしたりする必要はありません。

### 異なるdomainIdsを見つけるにはどうすればよいですか？

[ドメインIDを](https://docs.connext.network/developers/testing-against-testnet#domain-ids)参照してください。

### コネクストの契約はどこにある？

[展開された契約アドレス](https://docs.connext.network/developers/testing-against-testnet#deployed-contract-addresses)を参照してください。

### トークンをクロスチェーンに持ち出すにはどうしたらいいですか？

いくつかの手順がありますので、ぜひお声をおかけください。

* Connextチームがあなたのアセットをホワイトリストに登録します。（現時点では必須）
* 橋の向こうの資産の一部を移管するのです。
* 移行先のアセットが採用したアセットとamb風味のアセットを持つ場合（既存のプールがない、代表アセットがまだ存在しない）、Token Registryは新しいトークンを展開し、それを正規のものとしてセットします。

### calldataにサイズ制限はありますか？ <a href="#are-there-size-limits-to-calldata" id="are-there-size-limits-to-calldata"></a>
