# 🪙 Anleitung zur Erstellung eines Tokens in Solana am Beispiel des bestehenden Tokens "Crypto MZN", entwickelt vom mosambikanischen "Mr. Robot" Ewertoneak.

Dieses Schritt-für-Schritt-Tutorial zeigt Ihnen, wie Sie Ihre Entwicklungsumgebung einrichten, eine Wallet im Testnetzwerk (Devnet) erstellen, Ihren eigenen Token mithilfe des **Token-2022**-Standards prägen und Metadaten damit verknüpfen.

---

## 🛠 1. Installation der Solana CLI (WSL / Linux)

Führen Sie den folgenden Befehl in Ihrem Server- oder Linux/WSL-Terminal aus, um das vollständige Solana-Paket und seine Kernabhängigkeiten zu installieren:

```bash
curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
```

### Installierte Pakete
Die Installation konfiguriert automatisch die folgenden Tools:
1. **Rust** (`cargo`, `clippy`, `rust-docs`, `rust-std`)
2. **Solana CLI**
3. **Anchor CLI**
4. **Yarn**
5. **Node.js**

Nach Abschluss sollte die erwartete Ausgabe in etwa so aussehen:

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

> 💡 **Hinweis:** Wenn bei **Yarn** oder **Node.js** Installationsfehler auftreten oder sie nicht standardmäßig installiert sind, können Sie diese einzeln installieren mit:
> ```bash
> sudo apt install yarn
> sudo apt install nodejs
> ```

Um zu überprüfen, ob alle Tools erfolgreich installiert wurden, führen Sie aus:
```bash
rustc --version && solana --version && anchor --version && node --version && yarn --version
```

⚠️ **Wichtig:** Starten Sie Ihr Terminal neu, damit alle Umgebungsvariablen und Installationen wirksam werden.

---

## 💳 2. Wallet-Konfiguration und Erstellung

### 2.1 Zum DEVNET-Netzwerk wechseln
**Devnet** ist das kostenlose öffentliche Testnetzwerk von Solana. Ändern Sie die CLI-Umgebung, indem Sie Folgendes ausführen:

```bash
solana config set --url devnet
```

*Erwartete Ausgabe:*
```text
Config File: /home/user/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /home/user/.config/solana/id.json
Commitment: confirmed
```

### 2.2 Eine neue Wallet erstellen
Generieren Sie das Schlüsselpaar, das als Ihre Test-Wallet dienen soll:

```bash
solana-keygen new --outfile ~/.config/solana/devnet.json
```

*Erwartete Ausgabe:*
```text
Generating a new keypair

For added security, enter a BIP39 passphrase

NOTE! This passphrase improves security of the recovery seed phrase NOT the
keypair file itself, which is stored as insecure plain text

BIP39 Passphrase (empty for none): 
Enter same passphrase again: 

Wrote new keypair to /home/user/.config/solana/devnet.json
==================================================================================
pubkey: 4m3hTY7C...BEISPIEL_FUER_OEFFENTLICHEN_SCHLUESSEL...4mt5epYXA
==================================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
pool hungry donkey accident trap message plastic under cost permit lens rally
==================================================================================
```
> ⚠️ **Warnung:** Speichern Sie Ihre Seed-Phrase und Ihren öffentlichen Schlüssel an einem sicheren Ort.

### 2.3 Die erstellte Wallet in der CLI aktivieren
Legen Sie diese neue Wallet als Standardkonto für alle folgenden CLI-Operationen fest:

```bash
solana config set --keypair ~/.config/solana/devnet.json
```

### 2.4 Aktuelle Einstellungen überprüfen
Stellen Sie sicher, dass die CLI auf die richtige Datei und das richtige Netzwerk verweist:

```bash
solana config get
```

*Erwartete Ausgabe:*
```text
Config File: /home/user/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /home/user/.config/solana/devnet.json
Commitment: confirmed
```

### 2.5 Testguthaben anfordern (Airdrop)
Um mit dem Netzwerk zu interagieren, benötigen wir Testgelder (SOL). Versuchen Sie, Gelder direkt über das Terminal anzufordern:

```bash
solana airdrop 2
```

Wenn Sie eine Fehlermeldung aufgrund überschrittener Ratenbegrenzungen erhalten:
```text
Error: airdrop request failed. This can happen when the rate limit is reached.
```

#### Alternative Methode (Web Faucets):
1. Rufen Sie die offizielle Faucet-Website auf: https://faucet.solana.com/
2. Schalten Sie das Netzwerk oben links auf **DEVNET** um.
3. Rufen Sie Ihre Wallet-Adresse im Terminal mit dem folgenden Befehl ab: `solana address`
4. Fügen Sie die Adresse in das Feld ein, ändern Sie den Betrag (**Amount**) auf `2.5` und klicken Sie auf **Confirm Airdrop**.
5. Überprüfen Sie zurück im Terminal mit dem Befehl `solana balance`, ob die Gelder eingegangen sind.

---

## 🪙 3. Token-Erstellung (Kryptowährung)

### 3.1 Token über die Token-2022-Erweiterung ausgeben
Wir verwenden den **Token-2022**-Standard, der native On-Chain-Metadaten und zukünftige Skalierbarkeitsverbesserungen ermöglicht.

```bash
spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb --enable-metadata --decimals 9
```

*Erwartete Ausgabe:*
```text
Creating token Y2QcQnCP...TOKEN_ADRESSE...bM3wAnTyU under program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb
To initialize metadata inside the mint, please run `spl-token initialize-metadata Y2QcQnCP...TOKEN_ADRESSE...bM3wAnTyU <YOUR_TOKEN_NAME> <YOUR_TOKEN_SYMBOL> <YOUR_TOKEN_URI>`, and sign with the mint authority.

Address:  Y2QcQnCP...TOKEN_ADRESSE...bM3wAnTyU
Decimals:  9

Signature: 2Z6iX3x8...TRANSAKTIONS_SIGNATUR...9V5Kr792hR
```
> 📌 **Wichtig:** Die oben generierte Adresse ist Ihr **Mint-Konto** (der Vertrag, der die Regeln Ihres Tokens verwaltet). Kopieren Sie diese Adresse.

### 3.2 Ein verknüpftes Token-Konto erstellen (Associated Token Account)
Damit Ihre Haupt-Wallet diesen neuen Token halten oder empfangen kann, müssen Sie ein untergeordnetes Konto erstellen, das mit der generierten Token-Adresse verknüpft ist.

```bash
spl-token create-account <MINT_ADDRESS>
```
*(Ersetzen Sie `<MINT_ADDRESS>` durch die im vorherigen Schritt generierte Token-Adresse)*

*Erwartete Ausgabe:*
```text
Creating account tbJofoA2...VERKNUEPFTES_KONTO...uWiDxo

Signature: 52WUT3Jk...SIGNATUR...hN3DkH95
```

### 3.3 Token-Einheiten prägen (Minting)
Generieren wir nun ein Anfangsguthaben an Token und senden es an das soeben erstellte verknüpfte Konto:

```bash
spl-token mint <MINT_ADDRESS> 1000000
```

*Erwartete Ausgabe:*
```text
Minting 1000000 tokens
  Token: Y2QcQnCP...TOKEN_ADRESSE...bM3wAnTyU
  Recipient: tbJofoA2...VERKNUEPFTES_KONTO...uWiDxo

Signature: 3DpbtpLc...SIGNATUR...GhHdiS6z
```

### 3.4 Token-Guthaben überprüfen
Sie können die Gesamtzahl der generierten Token mit folgendem Befehl bestätigen:

```bash
spl-token balance <MINT_ADDRESS>
```

### 3.5 Die Transaktion im Web untersuchen
Sie können alle Transaktionen, Konten und den Verlauf Ihres Tokens über den offiziellen Explorer mithilfe Ihrer öffentlichen Adresse einsehen:

```text
https://explorer.solana.com/address/<MINT_ADDRESS>?cluster=devnet
```
*(Ersetzen Sie `<MINT_ADDRESS>` durch Ihre Token-Adresse)*

---

## 🦊 4. Die Wallet in eine Erweiterung importieren (z. B. Phantom)

Um Ihre Token grafisch in der Erweiterung **Phantom Wallet** zu verwalten, befolgen Sie diese Schritte:

1. Rufen Sie im Terminal Ihren privaten Schlüssel als numerisches Array-Format ab, indem Sie Folgendes ausführen:
   ```bash
   cat ~/.config/solana/devnet.json
   ```
2. Die Ausgabe ist eine Zahlenfolge, die wie folgt aussieht:
   ```text
   [92,140,50,185,...]
   ```
3. Öffnen Sie Ihre **Phantom Wallet**:
   * Klicken Sie auf das Kontomenü -> **Wallet hinzufügen/verknüpfen**.
   * Wählen Sie **Privaten Schlüssel importieren**.
   * Fügen Sie das gesamte extrahierte numerische Array einschließlich der eckigen Klammern `[]` ein.
   * Geben Sie Ihrem Konto einen Namen und schließen Sie den Import ab.

> ⚠️ **Warnung:** Vergessen Sie nicht, den **Devnet- / Testnet-Modus** in den erweiterten Einstellungen der Phantom Wallet zu aktivieren, um Ihr Testguthaben und den erstellten Token korrekt anzuzeigen.

---

## 📂 5. Metadaten zur Kryptowährung hinzufügen

Da der Token im Explorer noch keinen Namen oder kein öffentliches Bild anzeigt, müssen wir die Metadaten vorbereiten.

1. Erstellen Sie in Ihrem Projekt einen Ordner namens `metadata/`.
2. Speichern Sie Ihr Token-Logo darin (`mytoken-logo.png`).
3. Erstellen Sie eine Datei `metadata.json` mit der folgenden Struktur:

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
    "creators": [{ "address": "<IHRE_SOLANA_WALLET>", "share": 100 }]
  }
}
```

### 5.1 Metadaten auf IPFS hosten und mit dem Token verknüpfen

Damit Wallets wie Phantom und der Solana Explorer den Namen, das Symbol und das Bild Ihres Tokens lesen können, müssen die Datei `metadata.json` und das zugehörige Bild öffentlich und dezentral gehostet werden.

1. Nutzen Sie einen IPFS-Gateway-Dienst (wie [Pinata](https://www.pinata.cloud/) oder **Storacha**).
2. Laden Sie das Logobild und die Datei `metadata.json` hoch.
3. Kopieren Sie den von Pinata für Ihre JSON-Datei generierten öffentlichen HTTP-Link (eine IPFS-Gateway-URL).

Wenn die URL bereit ist, führen Sie den folgenden Befehl aus, um die Metadaten direkt auf der Solana-Blockchain zu aktualisieren:

```bash
spl-token update-metadata <MINT_ADDRESS> uri <IHRE_IPFS_GATEWAY_URL>
```

#### Praktisches Ausführungsbeispiel:
```bash
spl-token update-metadata Y2QcQnCP...TOKEN_ADRESSE...bM3wAnTyU uri https://<IHR_GATEWAY>.mypinata.cloud/ipfs/<IHR_IPFS_HASH>
```

Nachdem die Transaktion im Netzwerk bestätigt wurde, werden die Daten Ihres Tokens automatisch auf allen Plattformen gelesen und indexiert, die mit dem **Token-2022**-Standard kompatibel sind.
