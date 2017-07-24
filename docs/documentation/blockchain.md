# Blockchain related requests

NEM builds a block chain which contains every bit of information needed. Subsequent blocks in the block chain have increasing heights that differ by one. Each block can contain transactions. Transactions build the basis of all account activity. It is therefore important to understand the concept and the structures of blocks and transactions.

Blocks are generated by accounts. If an account generates a block and the block gets included in the block chain, the generating account, called the harvester, gets all the transaction fees for transactions that are included in the block. A harvester will therefore usually include as many transactions as possible.

Transactions reflect all account activities. In order for a client to have an up to date balance for every account it is crucial to know about every transaction that occurred and therefore the client must have knowledge about every single block in the chain (one says: the client must be synchronized with the block chain).

[Official Source](https://bob.nem.ninja/docs/#block-chain-related-requests)

# BlockHttp definition

```typescript
export declare type BlockHeight = number;

export declare type BlockChainScore = number;

export declare class BlockHttp extends HttpEndpoint {
    constructor(serverConfig?: ServerConfig);

    /**
     * Gets a block from the chain that has a given hash.
     * @param BlockHeight - A BlockHeight JSON object
     * @returns Observable<Block>
     */
    getBlockByHeight(blockHeight: BlockHeight): Observable<Block>;
}

```

# BlockHttp usage

```typescript
import {BlockHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const blockHttp = new BlockHttp({domain: "104.128.226.60"});
blockHttp.getBlockByHeight(1033023).subscribe(block => {
    console.log(block);
});
```

Output

```
Block {
  height: 1033023,
  type: 1,
  timeStamp: 72650707,
  prevBlockHash: { data: 'ec766003b1f7ed462ddb5e3bd71d7def7f52cf34aa2ed3f0887bfbeaf59bb77c' },
  signature: '1e58ab2147db1edf746e899569e2c371c3b532fdf29ed77f3ddf54723b1ccc9ce745fc01ccb97b445e90e509035b1909950c4ba3428c20f31056bab4feff2e00',
  signer: '45880194fad01fcb55887b73eeffdc263914ed5749bf2f3acb928c843c57bd9a',
  transactions: [],
  version: -1744830463 }
```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/blockchain/BlockHttpExample.ts)


# ChainHttp definition

```typescript
export declare class ChainHttp extends HttpEndpoint {
    constructor(serverConfig?: ServerConfig);
    /**
     * Gets the current height of the block chain.
     * @returns Observable<BlockHeight>
     */
    getBlockchainHeight(): Observable<BlockHeight>;
    /**
     * Gets the current score of the block chain. The higher the score, the better the chain.
     * During synchronization, nodes try to get the best block chain in the network.
     * @returns Observable<BlockChainScore>
     */
    getBlockchainScore(): Observable<BlockChainScore>;
    /**
     * Gets the current last block of the chain.
     * @returns Observable<Block>
     */
    getBlockchainLastBlock(): Observable<Block>;
}


```

# ChainHttp usage

```typescript
import {ChainHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const chainHttp = new ChainHttp({domain: "104.128.226.60"});
chainHttp.getBlockchainLastBlock().subscribe(block => {
    console.log(block);
});
```
Output

```
Block {
  height: 1035218,
  type: 1,
  timeStamp: 72783289,
  prevBlockHash: { data: '551c478f401323be74da2618bd686d30c8c805c2d7bb3c19c49a8c0be6333d7f' },
  signature: '0ccb28a63fe3fb1f65c0815c76ba7bad5896b71a9e696d28dd29f3b5294e977b51ecbf4add3f528c965ef67eba1c62acbb74c0779f2fc1a1c7bda6cb467fdb05',
  signer: 'b3180ba3814a293f6b0ade952a65abc2aad6a430e4c33a20c216c8c6ceba584f',
  transactions: [],
  version: -1744830463 }

```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/blockchain/ChainHttpExample.ts)


# Models

## Block

```typescript

export declare enum BlockVersion {
  MAIN_NET = 0x68,
  TEST_NET = 0x98
}

export declare enum BlockType {
  NEMESIS = -1,
  REGULAR = 1
}

/**
 * A blockchain is the structure that contains the transaction information. A blockchain can contain up to 120 transactions. Blocks are generated and signed by accounts and are the instrument by which information is spread in the network.
 */
export declare class Block  {

  /**
   * The height of the blockchain. Each blockchain has a unique height. Subsequent blocks differ in height by 1.
   */
  readonly height: BlockHeight;

  /**
   * The blockchain type
   */
  readonly type: BlockType;

  /**
   * The number of seconds elapsed since the creation of the nemesis blockchain.
   */
  readonly timeStamp: number;

  /**
   * The sha3-256 hash of the last blockchain as hex-string.
   */
  readonly prevBlockHash: HashData;

  /**
   * The signature of the blockchain. The signature was generated by the signer and can be used to validate that the blockchain data was not modified by a node.
   */
  readonly signature: string;

  /**
   * The public key of the harvester of the blockchain as hexadecimal number.
   */
  readonly signer: string;

  /**
   * The array of transaction
   */
  readonly transactions: Transaction[];

  /**
   * The blockchain version
   */
  readonly version: BlockVersion;
 }
```

