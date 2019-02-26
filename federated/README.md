### Prerequisites

- Install Iroha python sdk
```shell
pip install iroha
```
### Create a domain, asset and default role in the BSMD network
```shell
python3 setup.py
```
The `setup.py` file creates a domain call **federated**, an asset call **fedcoin** and a default role call **user**. Domains are use to grup a set of users with common goals. For example, in this case all nodes will use the BSMD for participating in a Federated Learning algorithm, hence all nodes must be in the domain **federated**. A node can be part of different domains. **Assets** are virtual coins the **users** use for payments.

The `setup.py` file contain two function `send_transaction_and_print_status(transaction)` and `create_domain_and_asset()`. The first one is use for sending any transaction to the BSMD, while the second is use to configure the **domain**, default **role** and **assets**.

In the `create_domain_and_asset()` function modify the parameters `domain_id`, `default_role` and `asset_name` to setup a domain, default role and asset. 
```python
commands = [
	iroha.command('CreateDomain', domain_id='your_domain_name', default_role='your_defaul_role'),
	iroha.command('CreateAsset', asset_name='your_asset_name', domain_id='your_domain_name',precision=number_of_decimals)
	]
```
NOTE: `your_default_role` must be setting up first in the [genesis.block](/network/config/genesis.block) file. In this particular example we define the `user` role in the [genesis.block](/network/config/genesis.block) file as
```json
"createRole":{
	"roleName":"user",
	"permissions":[
		"can_add_signatory",
		"can_get_my_acc_ast",
		"can_get_my_acc_ast_txs",
		"can_get_my_acc_detail",
		"can_get_my_acc_txs",
		"can_get_my_account",
		"can_get_my_signatories",
		"can_get_my_txs",
		"can_grant_can_add_my_signatory",
		"can_grant_can_remove_my_signatory",
		"can_grant_can_set_my_account_detail",
		"can_grant_can_set_my_quorum",
		"can_grant_can_transfer_my_assets",
		"can_receive",
		"can_remove_signatory",
		"can_set_quorum",
		"can_transfer"
		]
	}
```
### Create a node
```shell
python3 node.py
```
The `node.py` file creates a node call **generator** with role **user** in the domain **federated** and a credit of 1000 **fedcoins**. 

To create nodes run the function `functions.create_account_user(name,user_public_key,domain,asset_qty)` with the following parameters:
- `name`: (string) Name of the node, each name in the BSMD need to be unique
- `user_public_key`: (string) public key of the node. The key is obtained with the function `IrohaCrypto.derive_public_key(user_private_key)`. NOTE: for this example the `user_private_key = b11224fedce8e8deebf4c03339f16790681f35ca09ccdc9785a7217394065af5`, but in production enviroments the private key must be obtained with the function`IrohaCrypto.private_key()`
- `domain`: (string) Name of the domain where the node is created. The domain must correspond to a domain created in the [Setup.py](Setup.py). In this example the domain name is **federated**
- `asset_qty`: (float) Initial assets of the node. Nodes can be created with a initial number of assets, for instance if a node wants to participate in the blockchain he can add some money to his account to make transactions. 

This node have some functionaties that are desribed next:

# Get balance 
```python
functions.get_balance(iroha,network,account_id,user_private_key)
```
This function returns a `json` with the assets quantyt of the node account. Example
```json

```


Only the owner and the **admin** can query the assets. The parameters are:
- `iroha`: Addres for connecting to a domain in the BSMD. The addres have the following form `Iroha(name@domain)`. In this example the addres is `Iroha(generator@federated)` 
- `network`: Phisical addres of one node runng the blockchain. The addres have the following form `IrohaGrpc(IP_of_the_node)`.  In this example the addres is `IrohaGrpc()` (or `IrohaGrpc(127.0.0.1)`) since the blockchain runs in our local machine
-`account_id`: (string) Id of the user in the domain. The account_id have the following form `name@domain`. In this example the account_id is `generator@federated`
-`user_private_key`: (string) Private key of the user