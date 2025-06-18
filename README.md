# 🚀 Neon EVM System Program Library Implementation

## 📝 Overview

This project shows how to use the composability characteristics of Neon EVM to easily communicate with Solana's Raydium protocol straight from Solidity contracts and perform operations by creating a Raydium pool.

## Prerequisites

- Node.js (v16 or higher)
- A Solana wallet with SOL for deployment
- A Metamask wallet with NEON and wSOL for Deployment.
- Basic understanding of Solana and Ethereum development.

## Project Structure

```
├── contracts/                                     # Smart contract file
│   ├── LibSystemProgram.sol
│   ├── LibSystemData.sol
│   └── CallRaydiumProgram.sol
└── test/                                          # Deployment scripts
|    └── composability/
|       └── raydium.test.js
└── README.md                                      # Project documentation
```

### Features

- **Composability contracts:** Serves as the core contract for the Raydium Protocol integration.
- **Precompiles:** Serves as helper contracts for the composability contracts.

### Installation

```bash
# Initialize project
git clone https://github.com/Harshkumar62367/raydiumProgramLibrary.git
pnpm init
```

### Configuration

1. Create `.env` file in the root folder:

```env
PRIVATE_KEY_OWNER=XYZ
PRIVATE_KEY_USER_1=XYZ
PRIVATE_KEY_USER_2=XYZ
PRIVATE_KEY_USER_3=XYZ
PRIVATE_KEY_SOLANA=XYZ
PRIVATE_KEY_SOLANA_2=XYZ
PRIVATE_KEY_SOLANA_3=XYZ
PRIVATE_KEY_SOLANA_4=XYZ
```

2. Run the below script to deploy the Meme Launchpad

```
npx hardhat test test/composability/raydium.test.js --network neondevnet
```

## Deployed Contracts and Key Transactions

- **Deployer Address:** [`0xd12A585952dea511B17299D10B5059Cbd75fE64A`](https://neon-devnet.blockscout.com/address/0xd12A585952dea511B17299D10B5059Cbd75fE64A)
- **Call Raydium Program Contract:** [`0xF7A04bC5848389BC86866d3C6CfDB355Ad9DEc5c`](https://neon-devnet.blockscout.com/address/0xF7A04bC5848389BC86866d3C6CfDB355Ad9DEc5c)
- **tokenB_Erc20ForSpl token deployed at:** [`0xB52028019c5D874884E54AcBc3BAa152fbC05710`](https://neon-devnet.blockscout.com/address/0xB52028019c5D874884E54AcBc3BAa152fbC05710)
- **Pool ID:** `0x3cc1631801bf63348d179d1eb379d78f7d0e491db107e63a98a029154fc21668`
- **Pool id creation Tx:** [`0x544f0317119da78d14f40f73311f7e41c983c71bc0f8de0adc9e29cf82871009`](https://neon-devnet.blockscout.com/tx/0x544f0317119da78d14f40f73311f7e41c983c71bc0f8de0adc9e29cf82871009)
- **Added Liquidity Tx:** [`0x5f073b5f5f9b23b8d6a4ba60f6727aab53a7c82a7943826e019a3929449f9423`](https://neon-devnet.blockscout.com/tx/0x5f073b5f5f9b23b8d6a4ba60f6727aab53a7c82a7943826e019a3929449f9423)
- **Withdraw Liquidity Tx:** [`0x4cec529c1f4d823ddbb66091c5bfc4ed72270fcebd984a35cdba944840a212cc`](https://neon-devnet.blockscout.com/tx/0x4cec529c1f4d823ddbb66091c5bfc4ed72270fcebd984a35cdba944840a212cc)
- **Lock Liquidity with metadata Tx:** [`0x608c52cadd05d5e48f66b60e4933089fa8d6ed878738fd088cea16a834516af4`](https://neon-devnet.blockscout.com/tx/0x608c52cadd05d5e48f66b60e4933089fa8d6ed878738fd088cea16a834516af4)
- **Lock Liquidity without metadata Tx:** [`0xf03b3b66fc9e044b61ce566c4a75e0f4b504b03383cf2b2ee6df6e8fa22bf290`](https://neon-devnet.blockscout.com/tx/0xf03b3b66fc9e044b61ce566c4a75e0f4b504b03383cf2b2ee6df6e8fa22bf290)
- **Swap Input Tx:** [`0xfa0e05489ae4e5feeeb67591bf2e378419fa6bae0bfa303355565f8fa496453e`](https://neon-devnet.blockscout.com/tx/0xfa0e05489ae4e5feeeb67591bf2e378419fa6bae0bfa303355565f8fa496453e)
- **Swap Output Tx:** [`0x219dcc260b965f4e648e0517a3e6be874dd21ef1b5159cbd75b4f07593421d5b`](https://neon-devnet.blockscout.com/tx/0x219dcc260b965f4e648e0517a3e6be874dd21ef1b5159cbd75b4f07593421d5b)
- **Collect Fees Tx:** [`0x493493f0dcb2954fe186fd8e51b23aa106f3d352bb3502320b736d30b866a3ac`](https://neon-devnet.blockscout.com/tx/0x493493f0dcb2954fe186fd8e51b23aa106f3d352bb3502320b736d30b866a3ac)

## 💡 Example Use Case: Time-Locked Yield Farming Vault

**Idea:**  
Building a dApp that allows users to create a "time-locked yield farming vault" using Raydium pools. Users can deposit two tokens to create a new Raydium pool or add liquidity to an existing one, but their LP tokens are automatically locked for a fixed period (e.g., 30 days). During this time, users cannot withdraw their liquidity, but they earn trading fees and possibly bonus rewards. After the lock period, users can withdraw their liquidity and any accrued rewards.

**How it works with Raydium composability:**

- The dApp uses the composability library to:
  1. Will create a new Raydium pool or add liquidity to an existing pool.
  2. Immediately lock the user's LP tokens using the `lockLiquidity` instruction, with a time-based unlock condition.
  3. Optionally, allow users to claim trading fees periodically using the `collectFeesInstruction`.
  4. After the lock period, users can unlock and withdraw their liquidity.

**Why is this useful?**

- Encourages long-term liquidity provision, which helps stabilize pools and improve trading experience.
- It can be used for launching new tokens ("fair launch" pools) or incentivizing liquidity for existing pairs.

**How it leverages Raydium instructions:**

- Uses `createPoolInstruction` or `addLiquidityInstruction` to provide liquidity.
- Uses `lockLiquidityInstruction` to lock LP tokens.
- Uses `collectFeesInstruction` to let users claim fees.
- Uses `withdrawLiquidityInstruction` after the lock expires.
