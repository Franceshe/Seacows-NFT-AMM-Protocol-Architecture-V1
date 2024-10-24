# Seacows NFT AMAM Protocol Overview

Created: September 29, 2024 6:08 PM
Last Edited Time: September 29, 2024 6:14 PM
Created By: Frances He

**Seacows NFT AMAM Protocol Overview**

In this session, it gives out a general idea of how Seacows Protocol works.

1. [Architecture](https://github.com/yolominds/seacows-protocol-specification#architecture)
2. [PositionManager](https://github.com/yolominds/seacows-protocol-specification#PositionManager)
3. [Main Contracts](https://github.com/yolominds/seacows-protocol-specification#Main-contracts)
4. [User Flow](https://github.com/yolominds/seacows-protocol-specification#user-flow)
    1. [Deposit](https://github.com/yolominds/seacows-protocol-specification#deposit)
    2. [Swap](https://github.com/yolominds/seacows-protocol-specification#swap)
    3. [Withdraw](https://github.com/yolominds/seacows-protocol-specification#withdraw)
    4. [Swap after withdrawal](https://github.com/yolominds/seacows-protocol-specification#swap-after-withdrawal)
5. [Complement Algorithm](https://github.com/yolominds/seacows-protocol-specification#Complement-Algorithm)
6. [SpeedBump](https://github.com/yolominds/seacows-protocol-specification#SpeedBump)
7. [System Diagrams](https://github.com/yolominds/seacows-protocol-specification#System-Diagrams)

**Architecture**

![image.png](Seacows%20NFT%20AMAM%20Protocol%20Overview%20110a0707041b80d1ba3fe7370ea6e275/image.png)

Seacows Protocol is a binary smart contract system comprised of many libraries, which together make the Core and Periphery.

Core contracts provide the basic safety guarantees for all parties interacting with Seacows. They define the logi of pair contrcat deployment, the pair themselves, position management, and the interactions involving the respective assets therein.

Periphery contracts interact with one or more Core contracts but are not part of the core. They are designed to provide methods of interacting with the core that increase clarity and user safety.

**PositionManager**

![image.png](Seacows%20NFT%20AMAM%20Protocol%20Overview%20110a0707041b80d1ba3fe7370ea6e275/image%201.png)

PositionManager is based on [EIP-3525](https://eips.ethereum.org/EIPS/eip-3525) and we modified so that is a mapping in solana token standard in which we called SPL3525.
Please check out and understand erc3525 principle before get started. 
Core concept of erc3525(SPL3525) is id-slot-value, and correspond to position-pair-liquidity in PositionManager.

`erc3525(SPL3525) = erc20(Token-22 program) + (metaplex)erc721`

| erc3525 | PositionManager |
| --- | --- |
| id | position id |
| slot | pair id |
| value | liquidity amount |
1. when pool created, will create a new `slot`, it stand for pool/pair id. And will mint an initial position `id` for pool.
2. when userA add liquidity to this pool, will mint a new position id for userA. And will use `value` to store user liquidity.

For example:

USER-A create a POOL-A, and add liquidity 100 to the POOL-A.

```

POOL-A:
ID=1  SLOT=1  VALUE=0

USER-A:
ID=2  SLOT=1  VALUE=100

```

USER-B add liquidity 50 to the POOL-A, USER-A add liquidity 20 to the POOL-A again.

```
// Position manager state:

POOL-A:
ID=1  SLOT=1  VALUE=0

USER-A:
ID=2  SLOT=1  VALUE=120

USER-B:
ID=3  SLOT=1  VALUE=50

```

USER-C create a POOL-B, and add liquidity 666 to the POOL-B.

```

// Position manager state:

POOL-A:
ID=1  SLOT=1  VALUE=0

USER-A:
ID=2  SLOT=1  VALUE=120

USER-B:
ID=3  SLOT=1  VALUE=50

POOL-B:
ID=4  SLOT=2  VALUE=0

USER-C:
ID=5  SLOT=2  VALUE=666

```

**Main contracts**

![image.png](Seacows%20NFT%20AMAM%20Protocol%20Overview%20110a0707041b80d1ba3fe7370ea6e275/image%202.png)

**User flow**

**Deposit:**

![image.png](Seacows%20NFT%20AMAM%20Protocol%20Overview%20110a0707041b80d1ba3fe7370ea6e275/image%203.png)

**Swap**

![image.png](Seacows%20NFT%20AMAM%20Protocol%20Overview%20110a0707041b80d1ba3fe7370ea6e275/image%204.png)

**Withdraw**

![image.png](Seacows%20NFT%20AMAM%20Protocol%20Overview%20110a0707041b80d1ba3fe7370ea6e275/image%205.png)