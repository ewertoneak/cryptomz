# 🪙 Guia de Criação de Token na Solana usando O Token existente "Crypto MZN" como Exemplo desenvolvido pelo Moçambicano Mr. Robot Ewertoneak 

Este tutorial passo a passo demonstra como configurar o ambiente de desenvolvimento, criar uma carteira na rede de testes (Devnet), emitir o seu próprio token usando o **Token-2022** e associar metadados a ele.

---

## 🛠 1. Instalação da CLI da Solana (WSL / Linux)

Execute o seguinte comando no terminal do seu servidor ou ambiente Linux/WSL para instalar o pacote completo da Solana e as suas dependências básicas:

```bash
curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
```

### Pacotes Instalados
A instalação irá configurar automaticamente as seguintes ferramentas:
1. **Rust** (`cargo`, `clippy`, `rust-docs`, `rust-std`)
2. **Solana CLI**
3. **Anchor CLI**
4. **Yarn**
5. **Node.js**

Ao terminar, a saída esperada deverá ser semelhante a esta:

```text
-----------------------------------------------------------------------------
 Installed Versions:
 Rust: rustc 1.98.0 (88d9e12ae 2026-08-18)
 Solana CLI: solana-cli 3.1.10 (src:7bc9c805; feat:1620780344, client:Agave)
 Anchor CLI: anchor-cli 1.1.2
 Surfpool CLI: Not installed
 Node.js: Not installed
 Yarn: Not installed
-----------------------------------------------------------------------------
```

> 💡 **Nota:** Se o **Yarn** ou o **Node.js** apresentarem erros ou não forem instalados por padrão, instale-os individualmente usando:
> ```bash
> sudo apt install yarn
> sudo apt install nodejs
> ```

Para verificar se todas as ferramentas foram instaladas com sucesso, execute:
```bash
rustc --version && solana --version && anchor --version && node --version && yarn --version
```

⚠️ **Importante:** Reinicie o seu terminal para que todas as variáveis de ambiente e instalações surtam efeito.

---

## 💳 2. Configuração e Criação da Carteira

### 2.1 Alterar para a rede DEVNET
A **Devnet** é a rede pública de testes gratuita da Solana. Altere o ambiente da CLI executando:

```bash
solana config set --url devnet
```

*Saída esperada:*
```text
Config File: /home/usuario/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /home/usuario/.config/solana/id.json
Commitment: confirmed
```

### 2.2 Criar uma Nova Carteira
Crie o par de chaves que servirá como a sua carteira de testes:

```bash
solana-keygen new --outfile ~/.config/solana/devnet.json
```

*Saída esperada:*
```text
Generating a new keypair

For added security, enter a BIP39 passphrase

NOTE! This passphrase improves security of the recovery seed phrase NOT the
keypair file itself, which is stored as insecure plain text

BIP39 Passphrase (empty for none): 
Enter same passphrase again: 

Wrote new keypair to /home/usuario/.config/solana/devnet.json
==================================================================================
pubkey: 4m3hTY7C...EXEMPLO_DE_CHAVE_PUBLICA...4mt5epYXA
==================================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
pool hungry donkey accident trap message plastic under cost permit lens rally
==================================================================================
```
> ⚠️ **Aviso:** Guarde a sua frase semente (*seed phrase*) e a sua chave pública num local seguro.

### 2.3 Ativar a Carteira Criada na CLI
Defina esta nova carteira como a conta padrão para todas as operações seguintes da CLI:

```bash
solana config set --keypair ~/.config/solana/devnet.json
```

### 2.4 Verificar as Configurações Atuais
Certifique-se de que a CLI está a apontar para o ficheiro e rede corretos:

```bash
solana config get
```

*Saída esperada:*
```text
Config File: /home/usuario/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /home/usuario/.config/solana/devnet.json
Commitment: confirmed
```

### 2.5 Obter Saldo de Testes (Airdrop)
Para interagir com a rede, precisamos de fundos de teste (SOL). Tente solicitar fundos diretamente via terminal:

```bash
solana airdrop 2
```

Se receber uma mensagem de erro devido a limites de requisição excedidos:
```text
Error: airdrop request failed. This can happen when the rate limit is reached.
```

#### Método Alternativo (Faucets Web):
1. Aceda ao site oficial do Faucet: https://faucet.solana.com/
2. Altere a rede para **DEVNET** no canto superior esquerdo.
3. Obtenha o endereço da sua carteira no terminal usando o comando: `solana address`
4. Cole o endereço no campo, mude o valor (*Amount*) para `2.5` e clique em **Confirm Airdrop**.
5. No terminal, valide se os fundos chegaram utilizando o comando: `solana balance`

---

## 🪙 3. Criação do Token (Criptomoeda)

### 3.1 Emitir o Token utilizando a extensão Token-2022
Utilizaremos o padrão **Token-2022**, que permite metadados nativos *on-chain* e melhorias de escalabilidade futuras.

```bash
spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb --enable-metadata --decimals 9
```

*Saída esperada:*
```text
Creating token Y2QcQnCP...ENDERECO_DO_TOKEN...bM3wAnTyU under program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb
To initialize metadata inside the mint, please run `spl-token initialize-metadata Y2QcQnCP...ENDERECO_DO_TOKEN...bM3wAnTyU <YOUR_TOKEN_NAME> <YOUR_TOKEN_SYMBOL> <YOUR_TOKEN_URI>`, and sign with the mint authority.

Address:  Y2QcQnCP...ENDERECO_DO_TOKEN...bM3wAnTyU
Decimals:  9

Signature: 2Z6iX3x8...ASSINATURA_DA_TRANSACAO...9V5Kr792hR
```
> 📌 **Importante:** O endereço gerado acima é a sua **Mint Account** (o contrato que gere as regras do seu token). Copie-o.

### 3.2 Criar uma Conta de Token Associada (Associated Token Account)
Para que a sua carteira principal possa guardar ou receber este novo token, é necessário criar uma conta subordinada ao endereço do token criado.

```bash
spl-token create-account <MINT_ADDRESS>
```
*(Substitua `<MINT_ADDRESS>` pelo endereço do token gerado no passo anterior)*

*Saída esperada:*
```text
Creating account tbJofoA2...CONTA_ASSOCIADA...uWiDxo

Signature: 52WUT3Jk...ASSINATURA...hN3DkH95
```

### 3.3 Emitir Unidades do Token (Minting)
Agora, vamos gerar saldo inicial de tokens e enviá-los para a conta associada que acabou de criar:

```bash
spl-token mint <MINT_ADDRESS> 1000000
```

*Saída esperada:*
```text
Minting 1000000 tokens
  Token: Y2QcQnCP...ENDERECO_DO_TOKEN...bM3wAnTyU
  Recipient: tbJofoA2...CONTA_ASSOCIADA...uWiDxo

Signature: 3DpbtpLc...ASSINATURA...GhHdiS6z
```

### 3.4 Verificar o Saldo de Tokens
Pode confirmar a quantidade total de tokens gerados com o comando:

```bash
spl-token balance <MINT_ADDRESS>
```

### 3.5 Explorar a Transação na Web
Pode auditar todas as transações, contas e o histórico do seu token através do explorador oficial utilizando o seu endereço público:

```text
https://explorer.solana.com/address/<MINT_ADDRESS>?cluster=devnet
```
*(Substitua `<MINT_ADDRESS>` pelo endereço do seu token)*

---

## 🦊 4. Importar a Carteira para uma Extensão (Ex: Phantom)

Para gerir os seus tokens graficamente na extensão **Phantom Wallet**, siga estes passos:

1. No terminal, extraia a sua chave privada em formato de matriz numérica (Array) executando:
   ```bash
   cat ~/.config/solana/devnet.json
   ```
2. Abra a sua **Phantom Wallet**:
   * Clique no menu de contas -> **Adicionar/Conectar Carteira**.
   * Escolha **Importar Chave Privada**.
   * Cole toda a linha numérica extraída (a array com parênteses retos `[]`).
   * Atribua um nome à conta e conclua a importação.

> ⚠️ **Aviso:** Lembre-se de ativar o **Modo Devnet / Testnet** nas configurações avançadas da Phantom Wallet para visualizar corretamente o seu saldo de testes e o token criado.

---

## 📂 5. Adicionar Metadados à Criptomoeda

Como o token ainda não tem nome ou imagem pública no explorador, precisamos de preparar os metadados.

1. Crie uma pasta chamada `metadata/` no seu projeto.
2. Guarde lá dentro o logótipo do token (`mytoken-logo.png`).
3. Crie um ficheiro `metadata.json` com a seguinte estrutura:

```json
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
    "creators": [{ "address": "<SUA_CARTEIRA_SOLANA>", "share": 100 }]
  }
}
```

### 5.1 Hospedar os Metadados no IPFS e Vincular ao Token

Para que carteiras como a Phantom e o Solana Explorer reconheçam o nome, símbolo e imagem do seu token, o ficheiro `metadata.json` e a imagem associada precisam de estar alojados publicamente de forma descentralizada.

1. Aceda a um serviço de gateway IPFS (como o Pinata ou o Storacha).
2. Faça o upload da imagem do logótipo e do ficheiro `metadata.json`.
3. Copie o link HTTP público gerado pelo Pinata para o seu ficheiro JSON (uma URL de gateway IPFS).

Com a URL em mãos, execute o comando abaixo para atualizar os metadados diretamente na blockchain da Solana:

```bash
spl-token update-metadata <MINT_ADDRESS> uri <URL_DO_SEU_IPFS_GATEWAY>
```

#### Exemplo prático de execução:
```bash
spl-token update-metadata Y2QcQnCP...ENDERECO_DO_TOKEN...bM3wAnTyU uri https://<SUA_GATEWAY>.mypinata.cloud/ipfs/<SEU_HASH_IPFS>
```

Após a confirmação da transação na rede, os dados do seu token passarão a ser lidos automaticamente e indexados em todas as plataformas compatíveis com o padrão **Token-2022**.
