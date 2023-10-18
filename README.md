
# EduChain: Peer-to-Peer Learning Platform on Custom Blockchain

A decentralized educational platform leveraging blockchain technology to foster a community-driven learning ecosystem. Users can upload educational content, access resources, and earn tokens through contributions and engagement, all managed transparently through smart contracts on a custom-built blockchain.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Directory Structure](#directory-structure)
3. [Getting Started](#getting-started)
4. [Blockchain Architecture](#blockchain-architecture)
5. [Smart Contracts](#smart-contracts)
6. [Token Economy](#token-economy)
7. [Content Management](#content-management)
8. [P2P Network](#p2p-network)
9. [User Interface](#user-interface)
10. [Testing](#testing)
11. [Deployment](#deployment)
12. [Security](#security)
13. [Community and Contributions](#community-and-contributions)
14. [License](#license)
15. [Contact](#contact)

## Project Overview

This project aims to create a Peer-to-Peer Learning Platform built on a custom blockchain, fostering a decentralized community of learners and educators. Key features include:

- Custom blockchain for transaction and smart contract management.
- Uploading and accessing educational content.
- Token economy for incentivizing contributions and managing resources.
- Smart contracts for automating platform operations.
- Decentralized storage solutions for efficient content management.
- Simple and intuitive user interface for interacting with the blockchain.

## Directory Structure

```plaintext
project-root/
|-- blockchain/                # Custom blockchain implementation
|   |-- src/
|   |   |-- consensus/
|   |   |-- core/
|   |   |-- network/
|   |-- tests/
|   |-- README.md
|-- smart-contracts/           # Smart contracts for platform operations
|   |-- src/
|   |-- tests/
|   |-- README.md
|-- dapp/                      # Decentralized application (frontend & backend)
|   |-- src/
|   |   |-- components/
|   |   |-- assets/
|   |-- public/
|   |-- README.md
|-- storage/                   # Off-chain storage solutions
|   |-- README.md
|-- documentation/             # Project documentation
|   |-- architecture.md
|   |-- api.md
|-- scripts/                   # Utility scripts
|-- README.md                  # Project overview and setup instructions
```

## Getting Started

### Prerequisites

- Node.js and npm
- Truffle Suite for smart contract compilation, testing, and deployment.
- Ganache for a personal blockchain for Ethereum development.
- Metamask for interacting with the blockchain through the browser.

### Installation

... (installation steps)

---

## Blockchain Architecture

### **Task 1: Create Basic Block Structure**

#### **Objective:**
Create the fundamental structure of a block which will form the building blocks of our custom blockchain.

#### **Sub-Tasks:**

1. **Define Block Class:** 
   - Create a file named `block.js`.
   - Define a class named `Block` with properties:
     - `index`: The position of the block in the blockchain.
     - `timestamp`: The time when the block was created.
     - `transactions`: An array of transactions included in the block.
     - `previousHash`: The hash of the previous block.
     - `hash`: The hash of the current block.
     - `nonce`: A random value to ensure the block hash meets the required difficulty.

```javascript
class Block {
  constructor(index, timestamp, transactions, previousHash = '') {
    this.index = index;
    this.timestamp = timestamp;
    this.transactions = transactions;
    this.previousHash = previousHash;
    this.hash = this.calculateHash();
    this.nonce = 0;
  }

  calculateHash() {
    // Implement hash calculation using SHA-256
  }
}
```

2. **Implement Hash Calculation:**
   - Use a cryptographic hash function such as SHA-256 to calculate the hash of the block.
   - The hash should include the block's index, timestamp, transactions, previous hash, and nonce.

```javascript
  calculateHash() {
    return sha256(this.index + this.previousHash + this.timestamp + JSON.stringify(this.transactions) + this.nonce).toString();
  }
```

3. **Implement Proof of Work:**
   - Add a method to the `Block` class to perform a simple proof of work.
   - This method should increment the nonce until the hash of the block begins with a certain number of leading zeros, determined by the network difficulty.

```javascript
  mineBlock(difficulty) {
    while (this.hash.substring(0, difficulty) !== Array(difficulty + 1).join("0")) {
      this.nonce++;
      this.hash = this.calculateHash();
    }
  }
```

### **Task 2: Implement Simplified Consensus Mechanism**

#### **Objective:**
Implement a simplified consensus mechanism to validate and agree on the state of the blockchain.

#### **Sub-Tasks:**

1. **Define Blockchain Class:**
   - Create a file named `blockchain.js`.
   - Define a class named `Blockchain` with properties:
     - `chain`: An array to hold all the blocks.
     - `difficulty`: The required leading zeros in the hash for a block to be valid.

```javascript
class Blockchain {
  constructor() {
    this.chain = [this.createGenesisBlock()];
    this.difficulty = 2;
  }

  createGenesisBlock() {
    return new Block(0, "01/01/2023", "Genesis Block", "0");
  }
}
```

2. **Implement Block Validation:**
   - Add a method to validate a block by checking its hash and the hash of the previous block.
   - Ensure that the block's hash meets the network difficulty requirement.

```javascript
  isBlockValid(block, previousBlock) {
    if (block.previousHash !== previousBlock.hash) {
      return false;
    }

    if (block.hash !== block.calculateHash()) {
      return false;
    }

    if (block.hash.substring(0, this.difficulty) !== Array(this.difficulty + 1).join("0")) {
      return false;
    }

    return true;
  }
```

3. **Implement Chain Validation:**
   - Add a method to validate the entire blockchain by iterating through the chain and validating each block.

```javascript
  isChainValid() {
    for (let i = 1; i < this.chain.length; i++) {
      const currentBlock = this.chain[i];
      const previousBlock = this.chain[i - 1];

      if (!this.isBlockValid(currentBlock, previousBlock)) {
        return false;
      }
    }

    return true;
  }
```

### **Task 3: Develop P2P Network Protocol**

#### **Objective:**
Develop a protocol for peer-to-peer communication to propagate and validate new blocks and transactions.

#### **Sub-Tasks:**

1. **Setup a Basic P2P Server:**
   - Choose a P2P library or framework suitable for your technology stack.
   - Setup a basic server to handle P2P connections, messages, and events.

2. **Implement Block Propagation:**
   - When a new block is mined, propagate this block to all connected peers.
   - When a peer receives a new block, validate the block and add it to the blockchain if valid.

3. **Implement Transaction Propagation:**
   - When a new transaction is created, propagate this transaction to all connected peers.
   - When a peer receives a new transaction, validate the transaction and add it to the transaction pool if valid.

4. **Handle New Peers:**
   - When a new peer connects, send them the current state of the blockchain and the transaction pool.

---

## Smart Contracts

### **Task 1: Develop Smart Contract for Content Upload**

#### **Objective:**
Create a smart contract that facilitates the uploading of educational content by users, while also handling the metadata and fees associated with the upload.

#### **Sub-Tasks:**

1. **Setup Your Development Environment:**
   - Install Truffle Suite for smart contract compilation, testing, and deployment.
   - Setup Ganache as your personal blockchain for Ethereum development.
   - Configure Metamask to interact with your blockchain.

2. **Create Content Upload Contract:**
   - Create a file named `ContentUpload.sol`.
   - Define a smart contract named `ContentUpload` with functions to:
     - Upload content metadata.
     - Handle the payment for content upload.
     - Emit an event upon successful content upload.

```solidity
pragma solidity ^0.8.0;

contract ContentUpload {

    struct Content {
        address uploader;
        string title;
        string description;
        uint uploadFee;
        string contentHash;
    }

    uint public uploadFee;
    mapping(uint => Content) public contents;
    uint public contentCount = 0;

    event ContentUploaded(address indexed uploader, uint indexed contentId, string contentHash);

    constructor(uint _uploadFee) {
        uploadFee = _uploadFee;
    }

    function uploadContent(string memory _title, string memory _description, string memory _contentHash) public payable {
        require(msg.value == uploadFee, "Incorrect upload fee");

        contentCount ++;
        contents[contentCount] = Content(msg.sender, _title, _description, msg.value, _contentHash);

        emit ContentUploaded(msg.sender, contentCount, _contentHash);
    }
}
```

### **Task 2: Implement Smart Contract for Content Access**

#### **Objective:**
Develop a smart contract to manage the access and payment for viewing educational content.

#### **Sub-Tasks:**

1. **Create Content Access Contract:**
   - Create a file named `ContentAccess.sol`.
   - Define a smart contract named `ContentAccess` with functions to:
     - Access content metadata.
     - Handle payment for content access.
     - Emit an event upon successful content access.

```solidity
pragma solidity ^0.8.0;

import "./ContentUpload.sol";

contract ContentAccess {

    ContentUpload public contentUploadContract;

    event ContentAccessed(address indexed viewer, uint indexed contentId);

    constructor(address _contentUploadContract) {
        contentUploadContract = ContentUpload(_contentUploadContract);
    }

    function accessContent(uint _contentId) public payable {
        ContentUpload.Content memory content = contentUploadContract.contents(_contentId);
        require(msg.value >= content.uploadFee, "Insufficient funds");

        payable(content.uploader).transfer(content.uploadFee);

        emit ContentAccessed(msg.sender, _contentId);
    }
}
```


The provided architecture and tasks don't currently cover review sharing, but this is a great feature to include! It would encourage users to provide feedback, improving the overall quality and trustworthiness of the content on your platform. Here's a breakdown on how you could implement review sharing:

---

## Smart Contracts - Review Sharing

### **Task 1: Develop Smart Contract for Review Management**

#### **Objective:**
Create a smart contract to handle the submission, storage, and retrieval of reviews for different educational content.

#### **Sub-Tasks:**

1. **Create Review Management Contract:**
   - Create a file named `ReviewManager.sol`.
   - Define a smart contract named `ReviewManager` with functions to:
     - Submit reviews.
     - Retrieve reviews for a particular piece of content.

```solidity
pragma solidity ^0.8.0;

contract ReviewManager {

    struct Review {
        address reviewer;
        uint contentId;
        uint rating;
        string comments;
    }

    mapping(uint => Review[]) public reviews;

    event ReviewSubmitted(uint indexed contentId, address indexed reviewer);

    function submitReview(uint _contentId, uint _rating, string memory _comments) public {
        require(_rating >= 1 && _rating <= 5, "Rating should be between 1 and 5");

        Review memory newReview = Review(msg.sender, _contentId, _rating, _comments);
        reviews[_contentId].push(newReview);

        emit ReviewSubmitted(_contentId, msg.sender);
    }

    function getReviews(uint _contentId) public view returns (Review[] memory) {
        return reviews[_contentId];
    }
}
```

In this contract:

- Users can submit reviews for content, including a rating and comments.
- Anyone can retrieve all reviews for a particular piece of content.

### **Task 2: Integrate Review Management with Existing Smart Contracts**

#### **Objective:**
Ensure that the review management system interacts seamlessly with your existing contracts and overall platform functionality.

#### **Sub-Tasks:**

1. **Update Content Interaction Contract:**
   - Modify the `ContentInteraction.sol` file.
   - Import the `ReviewManager.sol` contract.
   - Update the `ContentInteraction` contract to include a reference to the `ReviewManager` contract.
   - Add a function to allow users to submit reviews through the `ContentInteraction` contract, which in turn interacts with the `ReviewManager` contract.

```solidity
// Importing ReviewManager contract
import "./ReviewManager.sol";

// Updating ContentInteraction contract
contract ContentInteraction {

    // ... existing code ...

    ReviewManager public reviewManager;

    constructor(
        address _eduToken,
        address _contentUpload,
        address _contentAccess,
        address _reviewManager
    ) {
        // ... existing code ...
        reviewManager = ReviewManager(_reviewManager);
    }

    // ... existing code ...

    function submitReview(uint _contentId, uint _rating, string memory _comments) public {
        reviewManager.submitReview(_contentId, _rating, _comments);
    }
}
```

Revenue sharing is a crucial aspect to incentivize content creators and maintain a sustainable platform ecosystem. Implementing a revenue-sharing mechanism on your platform can be achieved through smart contracts. Here’s how you might approach this:

---

## Smart-Contracts: Revenue Sharing

### **Task 1: Develop Smart Contract for Revenue Sharing**

#### **Objective:**
Create a smart contract to manage the distribution of revenue generated from content access fees among content creators and platform maintainers.

#### **Sub-Tasks:**

1. **Create Revenue Sharing Contract:**
   - Create a file named `RevenueSharing.sol`.
   - Define a smart contract named `RevenueSharing` with functions to:
     - Distribute revenue.
     - Withdraw revenue.

```solidity
pragma solidity ^0.8.0;

import "./EduToken.sol";

contract RevenueSharing {

    EduToken public eduToken;
    mapping(address => uint) public pendingWithdrawals;

    constructor(address _eduToken) {
        eduToken = EduToken(_eduToken);
    }

    function distributeRevenue(uint _contentId, address _creator) public {
        require(msg.sender == address(this), "Only callable internally");

        uint contentFee = eduToken.balanceOf(address(this)) / 2;  // Assuming a 50-50 split
        pendingWithdrawals[_creator] += contentFee;
        // The rest goes to platform maintenance, which could also be withdrawn by a specific address
    }

    function withdrawRevenue() public {
        uint amount = pendingWithdrawals[msg.sender];
        require(amount > 0, "No revenue to withdraw");
        pendingWithdrawals[msg.sender] = 0;

        eduToken.transfer(msg.sender, amount);
    }
}
```

### **Task 2: Integrate Revenue Sharing with Existing Smart Contracts**

#### **Objective:**
Ensure that the revenue-sharing mechanism interacts seamlessly with your existing contracts and overall platform functionality.

#### **Sub-Tasks:**

1. **Update Content Interaction Contract:**
   - Modify the `ContentInteraction.sol` file.
   - Import the `RevenueSharing.sol` contract.
   - Update the `ContentInteraction` contract to include a reference to the `RevenueSharing` contract.
   - Modify the `completeContent` function to invoke the `distributeRevenue` function.

```solidity
// Importing RevenueSharing contract
import "./RevenueSharing.sol";

// Updating ContentInteraction contract
contract ContentInteraction {

    // ... existing code ...

    RevenueSharing public revenueSharing;

    constructor(
        address _eduToken,
        address _contentUpload,
        address _contentAccess,
        address _reviewManager,
        address _revenueSharing
    ) {
        // ... existing code ...
        revenueSharing = RevenueSharing(_revenueSharing);
    }

    // ... existing code ...

    function completeContent(uint _contentId) public {
        // ... existing code ...

        // Distribute revenue upon content completion
        revenueSharing.distributeRevenue(_contentId, contentUpload.uploaderAddress(_contentId));

        // ... existing code ...
    }
}
```


---

## **Token Economy**

This stage focuses on creating a sustainable and incentivized environment for both content creators and learners on the platform. The token economy is based on a custom token that will be utilized for transactions within the platform. This stage is broken down into several tasks each with a specific goal toward building a robust token economy.

### **Task 1: Token Creation**

#### **Objective:**
Develop a custom ERC-20 token for the platform.

#### **Sub-Tasks:**
1. Define and deploy an ERC-20 token contract using the OpenZeppelin library.
2. Ensure the token has a fixed supply, a name, and a symbol.
3. Implement a function to distribute initial tokens to a specific address, if necessary.

### **Task 2: Mining Rewards**

#### **Objective:**
Implement a mechanism to reward miners with platform tokens for their contribution to the network.

#### **Sub-Tasks:**
1. Modify the blockchain code to include a reward transaction for miners.
2. Determine the reward amount and implement a function to adjust it over time, if necessary.

### **Task 3: Content Access Fees**

#### **Objective:**
Implement a system where users pay tokens to access content.

#### **Sub-Tasks:**
1. Define and deploy a smart contract to handle content access fees.
2. Implement functions to lock tokens upon accessing content and release them upon completion or after a certain period.

### **Task 4: Revenue Sharing**

#### **Objective:**
Develop a mechanism to distribute revenue generated from content access fees among content creators and platform maintainers.

#### **Sub-Tasks:**
1. Define and deploy a smart contract for revenue distribution.
2. Implement functions to distribute revenue and allow beneficiaries to withdraw their share.

### **Task 5: Review Incentives**

#### **Objective:**
Encourage users to submit reviews by offering token incentives.

#### **Sub-Tasks:**
1. Modify the review management contract to award tokens for submitting reviews.
2. Implement a function to adjust the review reward amount, if necessary.

### **Task 6: Token Refunds upon Course Completion**

#### **Objective:**
Refund a portion of the tokens spent on content access upon course completion to incentivize course completion.

#### **Sub-Tasks:**
1. Modify the content interaction contract to refund tokens upon course completion.
2. Determine the refund amount and implement a function to adjust it, if necessary.

### **Task 7: Token Economy Monitoring**

#### **Objective:**
Develop tools to monitor and analyze the token economy.

#### **Sub-Tasks:**
1. Implement functions to retrieve data on token distribution, transactions, and other relevant metrics.
2. Create a dashboard or utilize existing tools to visualize the token economy data.

Each task in this stage takes you closer to establishing a fully functional token economy, which is essential for incentivizing user interaction, content creation, and maintaining a healthy ecosystem on your educational platform.

---

## Content Management

### Task 1: Implement On-Chain Metadata Storage

... (detailed instructions)

### Task 2: Develop Off-Chain Content Storage Solution

... (detailed instructions)

... (and so on for each content management task)

... (similarly for remaining sections)

## P2P Network
## User Interface
## Testing
## Deployment
## Security
## Community and Contributions
## License
## Contact

---
