# ERC Core 50
This is a collection of the most actual and popular ERCs on today.
The collection is splitted in two parts:
1. **Deep Dives [`/Deep`](Deep)**: In-depth analysis, implementation details, and deep mechanics.
2. **Overview [`/Overview`](Overview)**: Quick overview on the other ERCs.
In the future information about every single ERCs will be extended and thus they will be moved from `/Overview` to `/Deep`.

## ERCs List
### [ERC165](Deep/ERC165.md) (Interface Detection)  
Standardizes the concept of interfaces and standardizes the identification (naming) of interfaces. Creates a standard method to publish and detect what interfaces a smart contract implements.

### [ERC20](Deep/ERC20.md) (Token Standard)
A standard interface created to standaritze tokens on Ethereum. It requires implementations to provide basic functionality to transfer tokens and allow tokens to be approved.

### [ERC721](Deep/ERC721.md) (Non-Fungible Token Standard)
A standard interface for non-fungible tokens (NFTs), also known as deeds. It provides basic functionality to track and transfer NFTs.

### [ERC1155](Overview/ERC1155.md) (Multi Token Standard)
A standard for contracts managing multiple token types (Fungible, NFT, Semi-Fungible) in a single contract.

### [ERC1271](Overview/ERC1271.md) (Standard Signature Validation)
A standard allowing Smart Contracts to verify signatures. Acts as a "translation layer" for the Off-Chain Web.

### [ERC1967](Deep/ERC1967.md) (Proxy Storage Slots)
Standardizes where proxy information is stored and prevents storage clashes between the Proxy and the Logic contract.

### [ERC6909](Overview/ERC6909.md) (Minimal Multi-Token Interface)
A "Minimal Multi-Token Interface" designed specifically for high-performance protocols. Strips away "luxury" features of ERC-1155 for raw efficiency.