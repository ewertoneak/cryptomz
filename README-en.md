# 🪙 Guide to Creating a Token in Solana using the existing "Crypto MZN" token as an example, developed by the Mozambican "Mr. Robot" Ewertoneak.

This step-by-step tutorial demonstrates how to set up your development environment, create a wallet on the test network (Devnet), mint your own token using the **Token-2022** standard, and associate metadata with it.

---

## 🛠 1. Solana CLI Installation (WSL / Linux)

Run the following command in your server or Linux/WSL terminal to install the complete Solana package and its core dependencies:

```bash
curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
```

### Installed Packages
The installation will automatically configure the following tools:
1. **Rust** (`cargo`, `clippy`, `rust-docs`, `rust-std`)
2. **Solana CLI**
3. **Anchor CLI**
4. **Yarn**
5. **Node.js**

Upon completion, the expected output should look similar to this:

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

> 💡 **Note:** If **Yarn** or **Node.js** encounter installation errors or are not installed by default, you can install them individually using:
> ```bash
> sudo apt install yarn
> sudo apt install nodejs
> ```

To verify that all tools were successfully installed, run:
```bash
rustc --version && solana --version && anchor --version && node --version && yarn --version
```

⚠️ **Important:** Restart your terminal for all environment variables and installations to take effect.

---

## 💳 2. Wallet Configuration and Creation

### 2.1 Switch to the DEVNET Network
**Devnet** is Solana's free public test network. Change the CLI environment by executing:

```bash
solana config set --url devnet
```

*Expected output:*
```text
Config File: /home/user/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /home/user/.config/solana/id.json
Commitment: confirmed
```

### 2.2 Create a New Wallet
Generate the keypair that will serve as your test wallet:

```bash
solana-keygen new --outfile ~/.config/solana/devnet.json
```

*Expected output:*
```text
Generating a new keypair

For added security, enter a BIP39 passphrase

NOTE! This passphrase improves security of the recovery seed phrase NOT the
keypair file itself, which is stored as insecure plain text

BIP39 Passphrase (empty for none): 
Enter same passphrase again: 

Wrote new keypair to /home/user/.config/solana/devnet.json
==================================================================================
pubkey: 4m3hTY7C...SAMPLE_PUBLIC_KEY...4mt5epYXA
==================================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
pool hungry donkey accident trap message plastic under cost permit lens rally
==================================================================================
```
> ⚠️ **Warning:** Save your seed phrase and public key in a secure location.

### 2.3 Activate the Created Wallet in the CLI
Set this new wallet as the default account for all subsequent CLI operations:

```bash
solana config set --keypair ~/.config/solana/devnet.json
```

### 2.4 Verify Current Settings
Ensure the CLI is pointing to the correct file and network:

```bash
solana config get
```

*Expected output:*
```text
Config File: /home/user/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /home/user/.config/solana/devnet.json
Commitment: confirmed
```

### 2.5 Request Test Balance (Airdrop)
To interact with the network, we need test funds (SOL). Try requesting funds directly via the terminal:

```bash
solana airdrop 2
```

If you receive an error message due to exceeded rate limits:
```text
Error: airdrop request failed. This can happen when the rate limit is reached.
```

#### Alternative Method (Web Faucets):
1. Go to the official Faucet website: https://faucet.solana.com/
2. Switch the network to **DEVNET** in the top left corner.
3. Retrieve your wallet address in the terminal using the command: `solana address`
4. Paste the address into the field, change the **Amount** to `2.5`, and click **Confirm Airdrop**.
5. Back in the terminal, validate that the funds have arrived using the command: `solana balance`

---

## 🪙 3. Token Creation (Cryptocurrency)

### 3.1 Issue the Token Using the Token-2022 Extension
We will use the **Token-2022** standard, which enables native on-chain metadata and future scalability improvements.

```bash
spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb --enable-metadata --decimals 9
```

*Expected output:*
```text
Creating token Y2QcQnCP...TOKEN_ADDRESS...bM3wAnTyU under program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb
To initialize metadata inside the mint, please run `spl-token initialize-metadata Y2QcQnCP...TOKEN_ADDRESS...bM3wAnTyU <YOUR_TOKEN_NAME> <YOUR_TOKEN_SYMBOL> <YOUR_TOKEN_URI>`, and sign with the mint authority.

Address:  Y2QcQnCP...TOKEN_ADDRESS...bM3wAnTyU
Decimals:  9

Signature: 2Z6iX3x8...TRANSACTION_SIGNATURE...9V5Kr792hR
```
> 📌 **Important:** The generated address above is your **Mint Account** (the contract managing your token's rules). Copy it.

### 3.2 Create an Associated Token Account
For your main wallet to hold or receive this new token, you must create a subordinate account linked to the generated token address.

```bash
spl-token create-account <MINT_ADDRESS>
```
*(Replace `<MINT_ADDRESS>` with the token address generated in the previous step)*

*Expected output:*
```text
Creating account tbJofoA2...ASSOCIATED_ACCOUNT...uWiDxo

Signature: 52WUT3Jk...SIGNATURE...hN3DkH95
```

### 3.3 Mint Units of the Token
Now, let's generate an initial balance of tokens and send them to the associated account you just created:

```bash
spl-token mint <MINT_ADDRESS> 1000000
```

*Expected output:*
```text
Minting 1000000 tokens
  Token: Y2QcQnCP...TOKEN_ADDRESS...bM3wAnTyU
  Recipient: tbJofoA2...ASSOCIATED_ACCOUNT...uWiDxo

Signature: 3DpbtpLc...SIGNATURE...GhHdiS6z
```

### 3.4 Verify the Token Balance
You can confirm the total amount of tokens generated with the command:

```bash
spl-token balance <MINT_ADDRESS>
```

### 3.5 Explore the Transaction on the Web
You can audit all transactions, accounts, and your token's history through the official explorer using your public address:

```text
https://explorer.solana.com/address/<MINT_ADDRESS>?cluster=devnet
```
*(Replace `<MINT_ADDRESS>` with your token's address)*

---

## 🦊 4. Import the Wallet into an Extension (e.g., Phantom)

To manage your tokens graphically in the **Phantom Wallet** extension, follow these steps:

1. In the terminal, extract your private key as a numeric array format by running:
   ```bash
   cat ~/.config/solana/devnet.json
   ```
2. The output will be a numeric sequence looking like this:
   ```text
   [92,140,50,185,...]
   ```
3. Open your **Phantom Wallet**:
   * Click on the account menu -> **Add/Connect Wallet**.
   * Choose **Import Private Key**.
   * Paste the entire extracted numeric array, including the square brackets `[]`.
   * Give your account a name and complete the import.

> ⚠️ **Warning:** Remember to enable **Devnet / Testnet Mode** in Phantom Wallet's advanced settings to properly view your test balance and the created token.

---

## 📂 5. Add Metadata to the Cryptocurrency

Since the token does not yet display a name or public image in the explorer, we need to prepare the metadata.

1. Create a folder named `metadata/` in your project.
2. Save your token logo inside it (`mytoken-logo.png`).
3. Create a `metadata.json` file with the following structure:

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
    "creators": [{ "address": "<YOUR_SOLANA_WALLET>", "share": 100 }]
  }
}
```

### 5.1 Host Metadata on IPFS and Link to Token

For wallets like Phantom and the Solana Explorer to read your token's name, symbol, and image, the `metadata.json` file and the associated image need to be publicly hosted in a decentralized manner.

1. Access an IPFS gateway service (such as [Pinata](https://www.pinata.cloud/) or **Storacha**).
2. Upload the logo image and the `metadata.json` file.
3. Copy the public HTTP link generated by Pinata for your JSON file (an IPFS gateway URL).

With the URL ready, run the command below to update the metadata directly on the Solana blockchain:

```bash
spl-token update-metadata <MINT_ADDRESS> uri <YOUR_IPFS_GATEWAY_URL>
```

#### Practical execution example:
```bash
spl-token update-metadata Y2QcQnCP...TOKEN_ADDRESS...bM3wAnTyU uri https://<YOUR_GATEWAY>.mypinata.cloud/ipfs/<YOUR_IPFS_HASH>
```

After the transaction is confirmed on the network, your token's data will automatically be read and indexed across all platforms compatible with the **Token-2022** standard.
