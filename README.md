# cryptomz
distination of developers of cryptocorrencies in Afrika
# cryptomz
distination of developers of cryptocorrencies in Afrika

Documentation on Working CRYPTO-MZN and Digital Metical and Token


______________________________________________________________________________________________________
	
	1.	Instalação do Solana depois da Instalação do WSL e Linux
______________________________________________________________________________________________________	

 Executar no Servidor o seguinte comando:

		-----------------------------------------------------------------------------------------------
		curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
		-----------------------------------------------------------------------------------------------
 
 Isto vai instalar todo pacote da Solana & Dependências:
  I. Rust
    - cargo
	- clippy
	- rust-docs
	- rust-std
  II. 	Solana CLI
  III. 	Anchor CLI
  IV. 	Yarn
  V. 	Nodejs
  
  Ao terminar os pacotes instalados serão (a saída será):
--------------------------------------------------------------------------------------------  
|-----------------------------------------------------------------------------|
|  Installed Versions:														  |
| Rust: rustc 1.98.0 (88d9e12ae 2026-08-18)									  |
| Solana CLI: solana-cli 3.1.10 (src:7bc9c805; feat:1620780344, client:Agave) |
| Anchor CLI: anchor-cli 1.1.2												  |
| Surfpool CLI: Not installed												  |
| Node.js: Not installed													  |
| Yarn: Not installed														  |
|-----------------------------------------------------------------------------|
---------------------------------------------------------------------------------------------
  Obs:
  No caso de algum dos pacotes acima der algum erro de instalação pode instalar singularmente
  no caso do Yarn (use o comando: "sudo apt install yarn")
  no caso do Nodejs (use o comando: "sudo apt install nodejs")
 
 Note: para verificar se esta tudo nos conformes use o comando abaixo
 
 ---------------------------------------------------------------------------------------------
  rustc --version && solana --version && anchor --version && node --version && yarn --version
-----------------------------------------------------------------------------------------------

 Fazer restart do Terminal para que todas as instalações surtam efeito
 
 

______________________________________________________________________________________________________

	2.	Criar a Carteira 
______________________________________________________________________________________________________


	2.1	Mudar para a DEVNET (Que é a rede Publica para testes free da rede Solana)
	==============================================================================

 Usaremos o seguinte comando:
		---------------------------------
		solana config set --url devnet
		----------------------------------
		
A saída será algo parecido com isto:
--------------------------------------------------------------
-------------------------------------------------------------
| Config File: /home/ewertoneak/.config/solana/cli/config.yml|
| RPC URL: https://api.devnet.solana.com					 |
| WebSocket URL: wss://api.devnet.solana.com/ (computed)	 |
| Keypair Path: /home/ewertoneak/.config/solana/id.json		 |
| Commitment: confirmed										 |
-------------------------------------------------------------
-----------------------------------------------------------
  
	2.2	A seguir criaremos a nossa Wallet/Carteira na Rede Solana
	============================================================================================== 
usaremos o seguinte comando:
		--------------------------------------------------------
		solana-keygen new --outfile ~/.config/solana/devnet.json
		--------------------------------------------------------
		
		
A saída será algo parecido com isto:
------------------------------------
------------------------------------------------------------------------------------=
|   Generating a new keypair														|
|																					|
| For added security, enter a BIP39 passphrase										|
|																					|
| NOTE! This passphrase improves security of the recovery seed phrase NOT the		|
| keypair file itself, which is stored as insecure plain text						|
|																					|
| BIP39 Passphrase (empty for none):												|
| Enter same passphrase again:														|
|																					|
| Wrote new keypair to /home/ewertoneak/.config/solana/devnet.json					|
| ==================================================================================|
| pubkey: 4m3hTY7CMug5D9LUMmeAqZbavmicZQMMzTq4mt5epYXA								|
| ==================================================================================|
| Save this seed phrase and your BIP39 passphrase to recover your new keypair:		|
| pool hungry donkey accident trap message plastic under cost permit lens rally		|
====================================================================================|

Nota: Isso criará sua carteira e exibirá sua chave pública — salve-a em um local seguro.
----------------------------------------------------------------------------------------


	2.3 Ativaremos a Wallet/Carteira para definí-la como CLI da Solana (trazê-la a vida):
	============================================================================================== 
Estamos a informar ao nosso CLI que a partir daqui todas as modificações que fizermos devem ser nesta Wallet/Carteira

 Para tal usaremos o seguinte comando:
 		--------------------------------------------------------
		solana config set --keypair ~/.config/solana/devnet.json
		--------------------------------------------------------
		
		
A saída será algo parecido com isto:
------------------------------------
--------------------------------------------------------------
| Config File: /home/ewertoneak/.config/solana/cli/config.yml |
| RPC URL: https://api.devnet.solana.com					  |
| WebSocket URL: wss://api.devnet.solana.com/ (computed)	  |
| Keypair Path: /home/ewertoneak/.config/solana/devnet.json	  |
| Commitment: confirmed										  |
---------------------------------------------------------------

	2.4 Vamos verificar se todas as configurações estão conforme:
	============================================================================================== 

Para tal usaremos o seguinte comando:
 		------------------
		solana config get
		------------------


A saída será algo parecido com isto:
------------------------------------
----------------------------------------------------------------
| Config File: /home/ewertoneak/.config/solana/cli/config.yml	|
| RPC URL: https://api.devnet.solana.com						|
| WebSocket URL: wss://api.devnet.solana.com/ (computed)		|
| Keypair Path: /home/ewertoneak/.config/solana/devnet.json		|
| Commitment: confirmed											|
----------------------------------------------------------------|

	2.5 Faremos a seguir um teste para verificar a validade da nossa Wallet/Carteira
	============================================================================================== 

	Vamos pedir a Rede Solana que eles nos dêem um crédito (airdrop) para que possamos usar em testes a nossa nova Wallet/Carteira
	
Solicitando um pouco de Devnet SOL para testes com seguinte comando:
 		------------------
		solana airdrop 2
		------------------

A saída será algo parecido com isto:
------------------------------------
------------------------------------------------------------------------------|
Requesting airdrop of 2 SOL													  |	
Error: airdrop request failed. This can happen when the rate limit is reached.|
------------------------------------------------------------------------------|

 Neste caso não deu certo, o que pode significar que o limite de airdrops para novas Wallet/Carteira foi excedido
 
 Mas têm um outro método para poder obter esse saldo (airdrop) disponibilizado pela própria rede Solana para DEVNET/testnet
 ===========================================================================================================================
 --> aceda ao website https://faucet.solana.com/ no seu navegador de preferência
 --> troque a rede para DEVNET no canto superior esquerdo
 --> preencha o wallet address pelo seu endereço que pode buscar pelo comando: "solana address"
 --> ao lado do endereço na caixinha "Amount" troque para o valor 2.5
 --> clique no botão "Confirm Airdrop"
 --> feito isso volte a sua CLI (linha de comando ou servidor) e confirme a recepção do saldo pelo comando: "solana balance"
 --> confirme o saldo e vamos continuar
 
 
 ______________________________________________________________________________________________________

	2.	Criando nosso Crypto (TOken)
_______________________________________________________________________________________________________

	2.1	Vamos usar a configuração 'TOKEN 2022' que basicamente é uma tecnologia que permite metadados on-chain, 
 configuração de casas decimais e extensões preparadas para o futuro.
	===============================================================================================================
	
  Para tal usaremos o seguinte comando para a criação do nosso novo Token (Moeda):

	----------------------------------------------------------------------------------------------------------------
	spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb --enable-metadata --decimals 9
	----------------------------------------------------------------------------------------------------------------


A saída será algo parecido com isto:
------------------------------------
-----------------------------------------------------------------------------------------------------------------------------|	
Creating token Y2QcQnCPsU7cc52GF1X9X1vwbo4487MZ71bM3wAnTyU under program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb		 |
To initialize metadata inside the mint, please run `spl-token initialize-metadata Y2QcQnCPsU7cc52GF1X9X1vwbo4487MZ71bM3wAnTyU| 
<YOUR_TOKEN_NAME> <YOUR_TOKEN_SYMBOL> <YOUR_TOKEN_URI>`, and sign with the mint authority.									 |
																															 |
Address:  Y2QcQnCPsU7cc52GF1X9X1vwbo4487MZ71bM3wAnTyU																		 |
Decimals:  9																												 |
																															 |
Signature: 2Z6iX3x8SkMiqZEARL7TWPxRsaq6pyk35skeWiuv8K9BEa666cSg938HhTgRNHn3RBnw2nWaNYxbmo9V5Kr792hR							 |
-----------------------------------------------------------------------------------------------------------------------------|


	2.2 Criando a nossa Wallet/Carteira para poder guardar os nossos Tokens
	============================================================================================== 

	Feito isso passaremos a ter o Address/endereço emitido no comando anterior como o nosso Banco, isto é o "mint account" gerado
	pelo comando anterior passa a ser considerado quem gere todas as regras das contas Tokens futuras do nosso Token Principal,
	é o qual armazena por exemplo as casas decimais que nos ditam o limite de quanto pode cada conta guardar os tokens, por isso para 
	termos onde armazenar os nossos Tokens teremos que criar contas (filiadas) a esse endereço mint
	
 Para tal usaremos o seguinte comando:
 
 	----------------------------------------------------------------------------------------------------------------
	spl-token create-account <MINT_ADDRESS>
	----------------------------------------------------------------------------------------------------------------
	
  Onde substituiremos o (MINT_ADDRESS) pelo gerado na saída (criação do token)


A saída será algo parecido com isto:
------------------------------------
----------------------------------------------------------------------------------------------------|
Creating account tbJofoA2tGn5274bk9pLKyDQPGhMCf4mPiadyuWiDxo										|
																									|
Signature: 52WUT3JkepZpysCT4HwwqBaj7qq1beeCuhXvwg7ha2gxWzPUArA366uiaDx6X1drNnNUHB1cLggSyJXZhN3DkH95	|
----------------------------------------------------------------------------------------------------|

Isso cria uma conta de carteira capaz de armazenar seu novo token.

	
	2.3  A seguir adicionaremos saldo a nossa Wallet/Carteira (Mint the Address)
	============================================================================================== 

Adicionaremos o saldo ao nosso MINT_ADDRESS com seguinte comando:
 		------------------
		spl-token mint <MINT_ADDRESS> 1000000
		------------------

A saída será algo parecido com isto:
------------------------------------
----------------------------------------------------------------------------------------------------|
| Minting 18446744073.709553 tokens																	|
|  Token: Y2QcQnCPsU7cc52GF1X9X1vwbo4487MZ71bM3wAnTyU												|
|  Recipient: tbJofoA2tGn5274bk9pLKyDQPGhMCf4mPiadyuWiDxo											|
|																									|
|Signature: 3DpbtpLcsx1b21Umcsd7HNWfSyMXAsdhdG9icr95PBE32ni2V8pXHA5mqupiS1gKb5VLiXaF9kGuLV9UGhHdiS6z|
-----------------------------------------------------------------------------------------------------

	2.3  Vamos checar o nosso saldo a nossa Wallet/Carteira (Mint the Address)
	============================================================================================== 

Usaremos o seguinte comando para checar se o nosso saldo se encontra na Wallet/Carteira:

	------------------------------------------
		spl-token balance <MINT_ADDRESS>
	------------------------------------------

A saída será algo parecido com isto:
------------------------------------
---------------------------------------------------------------|
 spl-token balance Y2QcQnCPsU7cc52GF1X9X1vwbo4487MZ71bM3wAnTyU |
18446744073.709551615										   |
---------------------------------------------------------------|

	2.4  Vamos checar a nossa carteira e transacções no Solana Explorer (Navegador)
	============================================================================================== 
	
 para tal acederemos ao nosso navegador de preferência e usaremos o link abaixo, substituindo pela nosso endereço MINT_ADDRESS
 
		https://explorer.solana.com/address/<MINT_ADDRESS>?cluster=devnet 
	
	
Seu token não será exibido com seu nome, símbolo e imagem, pois ainda não adicionamos os metadados.

	Note: podemos a partir deste ponto importar a nossa Wallet/Carteira para uma Carteira existente usando a nossa chave privada:
		  ------------------------------------------------------------------------------------------------------------------------
		  --> Usaremos a Wallet/Carteira Phantom como exemplo:
			-> Para importar a chave privada, basta acessar o caminho onde o "par de chaves" (keypair) foi salvo
			   usando o comando "cat" seguido do caminho, por exemplo: ~/.config/solana/devnet.json
	 		   o comando geral será: cat ~/.config/solana/devnet.json
		O qual nos trará o seguinte como saída:
	[92,140,50,185,179,159,180,7,76,16,30,161,206,195,192,139,153,56,58,161,50,196,254,204,171,98,213,220,126,153,2,221,55,219,170,
	159,199,27,98,32,57,205,18,2,94,110,252,68,206,207,85,221,131,163,122,86,142,181,217,73,49,186,37,25]	
	
	     --> Feito isso vamos abrir o Phantom Wallet:
		   -> Adicionar nova conta
		   -> Importar uma Chave Privada (private key)
		   -> Copiar todo esse nr acima incluindo os parênteses rectos "[]"
		   -> dar um nome a nossa Conta e clicar importar, teremos assim todos dados na Phantom Wallet
		   
		Obs: Não nos esquecendo de ativar o modo teste na configuração do Phantom Wallet
		

    2.5 Vamos adicionar Metadados (Imagem, descrição) a nossa Crypto
	==============================================================================================
	
	--> Temos que criar um directório (pasta) no nosso servidor/repositório e nela 2 ficheiros
	   vai ser algo do género:
					metadata/
						|--- mytoken-logo.png
						|--- metadata.json
	
	A saída será algo assim:
------------------------------------
{
  "name": "MyToken Token",
  "symbol": "MTK",
  "description": "The official utility token of the MyToken project — an example Solana token used for this tutorial.",
  "image": "mytoken-logo.png",
  "external_url": "https://mytoken.io",
  "attributes": [
    { "trait_type": "Category", "value": "Utility" },
    { "trait_type": "Network", "value": "Solana Devnet" }
  ],
  "properties": {
    "files": [{ "uri": "mytoken-logo.png", "type": "image/png" }],
    "category": "image",
    "creators": [{ "address": "<YOUR_WALLET_ADDRESS>", "share": 100 }]
  }
}
----------------------------------------------

	--> De seguida deveremos fazer upload para o IPFS (pelo PINATA ou Storacha)
	Dá para hospedar os metadados do nosso token em qualquer gateway IPFS. 
	A seguir vamos usar duas opções simples distintas:
	
	





