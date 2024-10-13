
---

# Merkle Airdrop

This project demonstrates an on-chain airdrop mechanism using a **Merkle tree** for efficient verification. It includes contracts and scripts for deploying a custom ERC-20 token (BagelToken) and distributing it via a Merkle airdrop.

## Features

- **Merkle Tree Airdrop**: Allows claimers to verify their inclusion in the airdrop by submitting a proof generated from the Merkle root.
- **Signature Validation**: Claims must be signed to prevent unauthorized claims.
- **Custom ERC-20 Token**: The airdropped token is called `BagelToken`.
- **Scripts**: Deployment and claim scripts are included for efficient contract interaction and verification.

## Contracts Overview

### 1. **MerkleAirdrop.sol**
   - Handles the airdrop process.
   - Uses **MerkleProof** to verify if the claimer is eligible.
   - Validates EIP-712 compliant signatures to prevent unauthorized claims.
   - Emits an event once a claim is successful.

### 2. **BagelToken.sol**
   - A simple ERC-20 token with a minting function.
   - The contract owner can mint tokens to any address.

### 3. **DeployMerkleAirdrop.s.sol**
   - Deploys the **BagelToken** and **MerkleAirdrop** contracts.
   - Mints a set amount of `BagelToken` and transfers them to the **MerkleAirdrop** contract for distribution.

### 4. **ClaimAirdrop.s.sol**
   - Claims the airdrop on behalf of a designated address using a Merkle proof and signature.
   - Uses `DevOpsTools` for fetching the most recently deployed **MerkleAirdrop** contract.

### 5. **GenerateInput.s.sol**
   - Generates input files for Merkle tree processing.
   - Prepares the list of addresses and their airdrop amounts.

### 6. **MakeMerkle.s.sol**
   - Generates Merkle proofs and constructs the tree using inputs from **GenerateInput**.
   - Writes the output proof data to a file for claimers to use in their transactions.

## How to Use

### Requirements

- **Forge**: Install Forge to compile and test contracts.
- **Foundry**: Scripts and deployment rely on Foundry.

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/merkle-airdrop.git
cd merkle-airdrop
```

### 2. Install Dependencies

Ensure you have Forge installed. Then, run:

```bash
forge install
```

### 3. Compile the Contracts

Compile all the contracts in the project:

```bash
forge build
```

### 4. Deploy the Airdrop Contracts

To deploy the `BagelToken` and `MerkleAirdrop` contracts:

```bash
forge script script/DeployMerkleAirdrop.s.sol --broadcast --rpc-url <RPC_URL>
```

### 5. Generate Merkle Tree Input

Generate the input for the Merkle tree (eligible addresses and airdrop amounts):

```bash
forge script script/GenerateInput.s.sol
```

### 6. Generate Merkle Proofs

After the input is created, generate the Merkle proofs:

```bash
forge script script/MakeMerkle.s.sol
```

### 7. Claim Airdrop

To claim the airdrop, use the claim script:

```bash
forge script script/ClaimAirdrop.s.sol --broadcast --rpc-url <RPC_URL>
```

### Testing

You can run the test suite to ensure everything is working:

```bash
forge test
```

## Contract Interaction

### Claiming Tokens

1. Ensure the claimer has a valid proof (generated using `MakeMerkle`).
2. Submit a claim using the `claim()` function with the necessary parameters: account, amount, Merkle proof, and EIP-712 signature.

### Example Claim Transaction

```solidity
airdrop.claim(account, amount, merkleProof, v, r, s);
```

## Folder Structure

```plaintext
.
├── src
│   ├── MerkleAirdrop.sol
│   ├── BagelToken.sol
├── script
│   ├── DeployMerkleAirdrop.s.sol
│   ├── ClaimAirdrop.s.sol
│   ├── GenerateInput.s.sol
│   ├── MakeMerkle.s.sol
├── target
│   ├── input.json
│   ├── output.json
├── test
│   └── MerkleAirdrop.t.sol
├── README.md
```

## License

This project is licensed under the MIT License.

---

