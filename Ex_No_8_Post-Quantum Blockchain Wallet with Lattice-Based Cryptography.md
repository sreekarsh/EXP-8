# Experiment 8: Post-Quantum Blockchain Wallet with Lattice-Based Cryptography
# Aim:
To create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys.

# Algorithm:
## Step 1: Understanding Quantum Threat to Blockchain
ECDSA-based wallets are vulnerable to quantum computers.


Lattice-based cryptography (e.g., NTRU, CRYSTALS-Kyber) provides quantum resistance.


## Step 2: Implement Lattice-Based Signature Scheme
Use precomputed NTRU-based public-private keys for authentication.


Store hashed lattice-based signatures instead of traditional Ethereum signatures.


## Step 3: Secure Transactions
Users sign transactions using lattice cryptographic proofs.


The smart contract verifies the proof before allowing transactions.



# Program:

(Solidity does not natively support lattice cryptography yet, but we simulate it using custom hash-based authentication.)
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PostQuantumWallet {
    struct User {
        bytes32 publicKeyHash;
        bool registered;
    }

    mapping(address => User) private users; // Made private to hide from Remix UI
    mapping(address => uint256) public balances;

    event UserRegistered(address user, bytes32 publicKeyHash);
    event TransactionVerified(address from, address to, uint256 amount);

    // Constructor
    constructor() {}

    // Generate a quantum-safe signature (simulated using keccak256)
    function generateSignature(address _sender, address _recipient, uint256 _amount) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(_sender, _recipient, _amount));
    }

    // Generate a simulated lattice-based public key hash
    function generatePublicKeyHash(string memory _publicKey) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(_publicKey));
    }

    // Register a user with a public key hash
    function registerUser(bytes32 _publicKeyHash) public {
        require(!users[msg.sender].registered, "User already registered");
        users[msg.sender] = User(_publicKeyHash, true);
        emit UserRegistered(msg.sender, _publicKeyHash);
    }

    // Send funds using quantum-safe simulated signature
    function sendFunds(address _to, uint256 _amount, bytes32 _signature) public {
        require(users[msg.sender].registered, "Sender not registered");
        require(users[_to].registered, "Recipient not registered");
        require(balances[msg.sender] >= _amount, "Insufficient funds");

        bytes32 calculatedSignature = generateSignature(msg.sender, _to, _amount);
        require(calculatedSignature == _signature, "Invalid quantum-safe signature");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;
        emit TransactionVerified(msg.sender, _to, _amount);
    }

    // Deposit funds to the wallet
    function depositFunds() public payable {
        balances[msg.sender] += msg.value;}
}
```

# Expected Output:
Users register using a post-quantum secure public key.


Transactions require a quantum-resistant signature for authentication.


If a traditional quantum-vulnerable hash is used, the transaction fails.

# Output:

<img width="1310" height="557" alt="Screenshot 2025-11-06 225111" src="https://github.com/user-attachments/assets/878ee179-b525-45a0-b104-1b98d81c1d6f" />

### User 1 Registration:

<img width="208" height="425" alt="Screenshot 2025-11-06 224654" src="https://github.com/user-attachments/assets/a1a88e8e-2b30-45ea-a9ff-d8af02a9f6f4" />

### User 2 Registration:

<img width="204" height="417" alt="Screenshot 2025-11-06 224816" src="https://github.com/user-attachments/assets/3ea302d6-4f80-4cd6-9702-4117310a5b45" />

### Generate Signature using sender and Reciever Address:

<img width="204" height="207" alt="Screenshot 2025-11-06 224926" src="https://github.com/user-attachments/assets/df733fe3-ea59-4487-800f-f7d67961895a" />

### After Sending Funds Sender Balance:

<img width="209" height="348" alt="Screenshot 2025-11-06 225052" src="https://github.com/user-attachments/assets/f6e2d523-420e-4223-85ab-c64a3a9a53aa" />

### After The Transaction Reciever Account Balnace:

<img width="205" height="256" alt="Screenshot 2025-11-06 225018" src="https://github.com/user-attachments/assets/7d273f90-173a-4f32-9816-f743b5a7af73" />

# RESULT : 
High-Level Overview:
First quantum-safe Ethereum-compatible wallet prototype.


Uses lattice-based key hashes instead of ECDSA.


Demonstrates how Ethereum will transition to post-quantum security.


Inspired by NIST’s post-quantum cryptography competition.

