# Getting started with Merkle tree
In this example, we will understand what Merkle trees and Mekrle proofs are, how it works in Web3.

## What is a Merkle tree?

If you're a software engineer, you've probably used Git, which manages versions using Merkle trees. Git allows you to commit changes without renaming files like file1.js, file2.2.js, etc., by using hashes to track changes in content.

Each commit in Git captures a snapshot of the repository at a specific time, including changes, the author, and the commit message. Git stores this data in a directed acyclic graph (DAG) data structure, where each node represents a commit with links to parent commits, enabling tracking of code changes over time.

## How does Merkle tree work in Web3?

The same concept is applied in Web3 technology. Merkle trees are utilized for hashing algorithms, and merkle proofs are employed for transaction verification. Essentially, transactions are arranged in pairs, the hash of each pair is computed, these hashes are then combined again in pairs, and their hashes are computed once more. This process is repeated until the root of the tree, known as the Merkle root, is reached. If you are presented with a transaction and its hash, you can easily verify whether that hash is part of the tree. This method aids in transaction verification without the need to download the entire blockchain, as the Merkle root serves as a summary of numerous transactions.

## What is a Merkle proof?

A Merkle proof verifies specific transactions represented by a leaf or branch hash within a Merkle hash root. This allows anyone to demonstrate that a transaction was indeed part of the blockchain at a certain point in time by providing a Merkle proof.

To understand the concept, let us look at the example in \[\]() file.

1. **Import Libraries**:
    
    ```javascript
    const { MerkleTree } = require('merkletreejs');
    const keccak256 = require('keccak256');
    ```
    
    This imports the `MerkleTree` class from the `merkletreejs` library and the `keccak256` hashing function. `merkletreejs` is a library for constructing Merkle trees, and `keccak256` is a cryptographic hash function.
    
2. **Define Whitelist Addresses**:
    
    ```javascript
    let whitelistAddresses = [...];
    ```
    
    An array of Ethereum addresses is initialized. These addresses are typically used in applications to represent a list of approved or eligible addresses for a particular operation, such as participating in an ICO or claiming airdrops.
    
3. **Generate Leaf Nodes**:
    
    ```javascript
    const leafNodes = whitelistAddresses.map(addr => keccak256(addr));
    ```
    
    Each address in the `whitelistAddresses` array is hashed using `keccak256`. The result is an array of hashes, which are the leaf nodes of the Merkle tree.
    
4. **Create Merkle Tree**:
    
    ```javascript
    const merkleTree = new MerkleTree(leafNodes, keccak256, { sortPairs: true});
    ```
    
    A new Merkle tree is created using the leaf nodes, the `keccak256` hash function, and an option to sort pairs. Sorting can help ensure that the tree is deterministic and its structure is consistent regardless of the order of input data.
    
5. **Get Root Hash**:
    
    ```javascript
    const rootHash = merkleTree.getRoot();
    ```
    
    This extracts the root hash of the Merkle tree, which is a single hash that represents the entire set of transactions (or data) in the tree.
    
6. **Output Merkle Tree and Root Hash**:
    
    ```javascript
    console.log('Whitelist Merkle Tree\n', merkleTree.toString());
    console.log("Root Hash: ", rootHash);
    ```
    
    The structure of the Merkle tree and the root hash are printed to the console. This is useful for debugging or verification purposes.
    
7. **Define a Claiming Address**:
    
    ```javascript
    const claimingAddress = leafNodes[6];
    ```
    
    This selects a specific address from the leaf nodes that might be used to demonstrate or test the verification process in the Merkle tree.
    
8. **Get Hexadecimal Proof for the Address**:
    
    ```javascript
    const hexProof = merkleTree.getHexProof(claimingAddress);
    console.log(hexProof);
    ```
    
    A Merkle proof (a series of hashes) is generated for the `claimingAddress`. This proof can be used to verify that the address is indeed part of the Merkle tree without revealing the entire tree. The proof is printed to the console.
    
9. **Verify the Proof**:
    
    ```javascript
    console.log(merkleTree.verify(hexProof, claimingAddress, rootHash));
    ```
    
    The `verify` method of the `MerkleTree` class is used to check if the `claimingAddress` can be verified against the `rootHash` using the `hexProof`. It returns `true` if the verification is successful, indicating that the address is part of the Merkle tree, and `false` otherwise. The result is printed to the console.
    

With this simple example, you learn the basic concept of Merkle tress, and how it is used in Web3.