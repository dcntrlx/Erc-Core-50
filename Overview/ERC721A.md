# ERC721A
Created: 2022-01
Description: An optimized implementation of ERC-721 that drastically reduces gas costs for batch minting by using lazy initialization of token ownership.

## Core Concepts

### The Problem: Expensive Batch Minting
In standard ERC-721 (e.g., OpenZeppelin), minting `N` tokens requires `N` separate storage updates (SSTOREs) to set the owner for each ID.
-   Minting 1 token: ~50k gas.
-   Minting 5 tokens: ~150k gas (Linear scaling).

This makes large drops expensive for users.

### The Solution: Lazy Initialization
ERC-721A optimizes storage by assuming that if User A mints tokens #100, #101, and #102, they own the entire range.
1.  **Batch Mint**: Only the *first* token ID (#100) has its owner explicitly set in storage.
2.  **Implicit Ownership**: Tokens #101 and #102 have `address(0)` in the owner slot.
3.  **Lookup**: When `ownerOf(102)` is called, the contract checks slot #102. If empty, it checks #101, then #100. It finds User A at #100 and returns User A.

Result: Minting 5 tokens costs nearly the same as minting 1 token.

## Methods

### _mint
```solidity
function _mint(address to, uint256 quantity) internal;
```
-   Updates storage only once for the starting `tokenId`.
-   Increments the `_currentIndex` by `quantity`.

### ownerOf
```solidity
function ownerOf(uint256 tokenId) public view override returns (address);
```
-   Modified to loop backwards from `tokenId` until a non-zero owner is found.

## Implementation Details

### Storage Layout
Instead of a simple mapping, ERC-721A uses a struct that packs ownership data:
```solidity
struct TokenOwnership {
    address addr;
    uint64 startTimestamp;
    bool burned;
}
```
This allows storing extra metadata (like mint time) without extra gas, as it fits in the same slot as the address.

### Transfer Logic: The "Chain Repair"
Transferring the "Head" of a batch (e.g., #100) forces a "Chain Repair" to prevent the new owner from inheriting the rest of the batch. The contract explicitly writes the previous owner's address to the next slot (#101) to create a new anchor. This extra `SSTORE` makes this specific transfer more expensive, effectively "paying back" the gas saved during minting.

## Nuance & Risks
1.  **Higher Transfer Cost**: Transfers are slightly more expensive than standard ERC-721 because of the look-back loop and the need to initialize adjacent tokens.
2.  **Read Cost**: `ownerOf` can be more expensive (gas-wise) if called on-chain for a token far from the start of a batch, due to the loop.
3.  **No Enumerable**: Standard `ERC721Enumerable` is incompatible or too expensive. ERC-721A provides alternative ways to fetch tokens owned by a user.

## Summary Comparison
-   **ERC-721**: High Mint Cost, Low Transfer Cost.
-   **ERC-721A**: Low Mint Cost (Constant), Slightly Higher Transfer Cost. Ideal for large NFT collections sold in batches.
