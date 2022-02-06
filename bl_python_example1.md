https://www.javatpoint.com/building-a-blockchain-using-python


We have divided the process of building a blockchain into several steps for better understanding. These steps are as follows:

Step 1: Creating a Blockchain class

Step 2: Writing a Function to build New Blocks


Step 3: Writing Functions to create New Transactions and get the Last Block

Step 4: Writing a Function to "Hash" the Blocks

Step 5: Creating a New Blockchain and Sending some money

Now, we will discuss these steps in the following sections.

Creating a Blockchain class
We will start by importing the required libraries. In this case, we will be needing the hashlib library for the encryption, the JSON library for our blocks formatting, and the time library for the timestamp of each block. We will then be creating a class and initializing the following variables:


chain: This will be an empty list to which we will add blocks. Quite literally, the 'blockchain'.
pendingTransactions: When users send the coins to each other, their transactions will locate in this array until we approve and insert them into a new block.
newBlock: This is a method that we will define soon, and we will utilize it in order to include each block in the chain.
Let us consider the following snippet of code demonstrating the same.

Example:

# importing the required libraries  
import hashlib  
import json  
from time import time  
  
# creating the Block_chain class  
class Block_chain(object):  
    def __init__(self):  
        self.chain = []  
        self.pendingTransactions = []  
  
        self.newBlock(previousHash = "The Times 03/Jan/2009 Chancellor on brink of second bailout for banks.", the_proof = 100)  
Explanation:


In the above snippet of code, we have imported the required libraries and created the Block_chain class where we initialized the different variables described earlier.

Writing a Function to construct New Blocks
Now that we have initialized an empty chain, let us begin inserting blocks into it. We will then define the JSON object with the following properties:

index: Taking the length of the blockchain and adding 1 to it. We will use this to reference an individual block, so for instance, the genesis block has index = 1.
timestamp: With the help of the time() module, we will stamp the block when it's created. Users can now check when their transaction was confirmed on-chain.
transactions: Any transactions that have been in the 'pending' list will be displayed in the new block.
proof: This property comes from the miner who thinks they found a valid 'proof' or 'nonce'.
previous_hash: A hashed version of the most recent approved block.
Once we add the above properties to the new block, we will include them in the chain. Initially, we empty the pending list of transactions and then add the new block to the self.chain and return it.

Let us understand the above using the snippet of code shown below.

Example:

# Creating a new block listing key/value pair of  
# block information in a JSON object.  
# Reset the list of pending transactions &  
# append the newest block to the chain.  
    def newBlock(self, the_proof, previousHash = None):  
        the_block = {  
            'index': len(self.chain) + 1,  
            'timestamp': time(),  
            'transactions': self.pendingTransactions,  
            'proof': the_proof,  
            'previous_hash': previousHash or self.hash(self.chain[-1]),  
        }  
        self.pendingTransactions = []  
        self.chain.append(the_block)  
  
        return the_block  
Explanation:

In the above snippet of code, we have defined the newBlock function and included the properties described earlier. We emptied the pending list of transactions and added the new block to the chain. At last, we have returned the new block.

Writing Functions to Create New Transactions and Get the Last Block
Now, let us include the list of transactions in the program because this whole program is quite pointless without one. So, let us first define a method that returns the block that was added most recently (we will use this in a second for the new index).

After that, we will create another method to represent a new transaction. This method will consist of the three most significant variables - the_sender, the_recipient, and the_amount. Without these variables included in every transaction, the users cannot spend, earn, or buy things with the newly produced cryptocurrency. Remember that these transactions are over-simplified and do not reflect the things one may find in a true cryptocurrency.


We will include the the_transaction JSON object to the pool of pendingTransactions. These will stay in an indetermination state until a new block is mined and added to the blockchain. And for future reference, we will return the index of the block to which the new transaction is about to be added.

Let us consider the following snippet of code demonstrating the same.

Example:

#Searching the blockchain for the most recent block.  
    @property  
    def lastBlock(self):  
   
        return self.chain[-1]  
  
# Adding a transaction with relevant info to the 'blockpool' - list of pending transactions.   
    def newTransaction(self, the_sender, the_recipient, the_amount):  
        the_transaction = {  
            'sender': the_sender,  
            'recipient': the_recipient,  
            'amount': the_amount  
        }  
        self.pendingTransactions.append(the_transaction)  
        return self.lastBlock['index'] + 1  
Explanation:

In the above snippet of code, we defined the method as lastBlock(), which returns the most recent block added. We have then defined the function as newTransaction(), within which we have defined the JSON object as the_transaction and included the addresses to the sender, recipient, and amount. We added this JSON object to the pendingTransaction and returned the last block.

Writing a Function to "Hash" the Blocks
Now, let us add Cryptography to the program. As we know, Bitcoin and many other blockchains utilize SHA-256, an encryption hash function, which accepts some text string (stored as a Unicode value) and spits out a 64-character long encrypted string. In a blockchain, the text that we encrypt is considered a block. For instance, the encrypted string, or "hash", of the Bitcoin genesis block appears like this:

fbc13b85c4ade52e2def26eae950f3b55b174df887ad0f0fb5ebfd56881f7fcb

Blockchains are considered tamper-proof as every block consists of a copy of the previous hash of the block. And as the new hash is derived from the last block, we cannot change any aspect of a block without altering every single hash in front of it.

Suppose that someone downloaded the Bitcoin blockchain to their computer and wrote "Satoshi sends Alex 7,236,000 Bitcoin!" into the genesis block and broadcasted this to the Bitcoin network and claimed that he is a secret billionaire. However, as soon as any self-respecting miner compares their current copy of the blockchain, especially the hash values stored in each block, with his copy of the chain, they will notice that he is a liar, refusing to validate it and run him off the network.

We will define the method that accepts the new block and alter its key/value pairs into strings. We will then convert that string into Unicode, which we will pass into the SHA256 method from the hashlib library and create a hexadecimal string from its return value. We will then return the new hash.

Let us understand the same using the following snippet of code.

Example:

# receiving one block. Turning it into a string, turning that into  
# Unicode (for hashing). Hashing with SHA256 encryption,  
# then translating the Unicode into a hexadecimal string.  
    def hash(self, the_block):  
        stringObject = json.dumps(the_block, sort_keys = True)  
        blockString = stringObject.encode()  
  
        rawHash = hashlib.sha256(blockString)  
        hexHash = rawHash.hexdigest()  
  
        return hexHash  
Explanation:

In the above snippet of code, we have defined the hash() function and accepts one block and turned them into Strings and then into Unicode for hashing. We have then used the SHA256() function for encryption and then translated the Unicode into a Hexadecimal string.

Creating a New Blockchain and Sending some Money
Since we have created a class for the blockchain and included various methods in order to build a new block and a new transaction, along with a custom method utilized to hash any block with SHA256 encryption, let us begin building the chain.

We will initialize an instance of the Block_chain class and perform some dummy transactions. Make sure to list them in some blocks that we include in the chain.

Let us consider the following snippet of code demonstrating the same.

Example:

block_chain = Block_chain()  
transaction1 = block_chain.newTransaction("Satoshi", "Alex", '10 BTC')  
transaction2 = block_chain.newTransaction("Alex", "Satoshi", '2 BTC')  
transaction3 = block_chain.newTransaction("Satoshi", "James", '10 BTC')  
block_chain.newBlock(10123)  
  
transaction4 = block_chain.newTransaction("Alex", "Lucy", '2 BTC')  
transaction5 = block_chain.newTransaction("Lucy", "Justin", '1 BTC')  
transaction6 = block_chain.newTransaction("Justin", "Alex", '1 BTC')  
block_chain.newBlock(10384)  
  
print("Genesis block: ", block_chain.chain)  
Explanation:

We have instantiated the Block_chain() class in the above snippet of code. We have then performed some transactions and printed them for the users.

Now, let us have a look at a complete code for the project of building blockchain using Python.

Complete Project Code
File: buildingBlockchain.py

# importing the required libraries  
import hashlib  
import json  
from time import time  
  
# creating the Block_chain class  
class Block_chain(object):  
    def __init__(self):  
        self.chain = []  
        self.pendingTransactions = []  
  
        self.newBlock(previousHash = "The Times 03/Jan/2009 Chancellor on brink of second bailout for banks.", the_proof = 100)  
  
# Creating a new block listing key/value pairs of  
# block information in a JSON object.  
# Reset the list of pending transactions &  
# append the newest block to the chain.  
    def newBlock(self, the_proof, previousHash = None):  
        the_block = {  
            'index': len(self.chain) + 1,  
            'timestamp': time(),  
            'transactions': self.pendingTransactions,  
            'proof': the_proof,  
            'previous_hash': previousHash or self.hash(self.chain[-1]),  
        }  
        self.pendingTransactions = []  
        self.chain.append(the_block)  
  
        return the_block  
  
#Searching the blockchain for the most recent block.  
    @property  
    def lastBlock(self):  
   
        return self.chain[-1]  
  
# Adding a transaction with relevant info to the 'blockpool' - list of pending tx's.   
    def newTransaction(self, the_sender, the_recipient, the_amount):  
        the_transaction = {  
            'sender': the_sender,  
            'recipient': the_recipient,  
            'amount': the_amount  
        }  
        self.pendingTransactions.append(the_transaction)  
        return self.lastBlock['index'] + 1  
  
# receiving one block. Turning it into a string, turning that into  
# Unicode (for hashing). Hashing with SHA256 encryption,  
# then translating the Unicode into a hexidecimal string.  
    def hash(self, the_block):  
        stringObject = json.dumps(the_block, sort_keys = True)  
        blockString = stringObject.encode()  
  
        rawHash = hashlib.sha256(blockString)  
        hexHash = rawHash.hexdigest()  
  
        return hexHash  
  
block_chain = Block_chain()  
transaction1 = block_chain.newTransaction("Satoshi", "Alex", '10 BTC')  
transaction2 = block_chain.newTransaction("Alex", "Satoshi", '2 BTC')  
transaction3 = block_chain.newTransaction("Satoshi", "James", '10 BTC')  
block_chain.newBlock(10123)  
  
transaction4 = block_chain.newTransaction("Alex", "Lucy", '2 BTC')  
transaction5 = block_chain.newTransaction("Lucy", "Justin", '1 BTC')  
transaction6 = block_chain.newTransaction("Justin", "Alex", '1 BTC')  
block_chain.newBlock(10384)  
  
print("Genesis block: ", block_chain.chain)  
Output:

Genesis block:  [
    {'index': 1,
    'timestamp': 1640067926.584454,
    'transactions': [],
    'proof': 100,
    'previous_hash': 'The Times 03/Jan/2009 Chancellor on brink of second bailout for banks.'},
    
    {'index': 2,
    'timestamp': 1640067926.584454,
    'transactions': [
        {'sender': 'Satoshi', 'recipient': 'Alex', 'amount': '10 BTC'},
        {'sender': 'Alex', 'recipient': 'Satoshi', 'amount': '2 BTC'},
        {'sender': 'Satoshi', 'recipient': 'James', 'amount': '10 BTC'}
        ],
    'proof': 10123,
    'previous_hash': 'a1b0cf063d43989421eb4b28d9be8f82c2e2e9e40bc9814321e3cbb70b00530a'},
    
    {'index': 3,
    'timestamp': 1640067926.584454,
    'transactions': [
        {'sender': 'Alex', 'recipient': 'Lucy', 'amount': '2 BTC'},
        {'sender': 'Lucy', 'recipient': 'Justin', 'amount': '1 BTC'},
        {'sender': 'Justin', 'recipient': 'Alex', 'amount': '1 BTC'}
        ],
    'proof': 10384,
    'previous_hash': '23699917fdcc013a85bbb5872251768e976bfcc2cd8403565c04970bca24a871'}
    ]
Explanation:

In the above output, we can observe that our blockchain contains three blocks right now: The genesis block (with an index of 1 and no transactions), in addition to the 2 that we added ourselves. We can also notice that the encrypted hashes (derived from every preceding block) and the timestamps do not match each other. Granted, the computer constructed each block almost simultaneously as we executed the program and generated blocks at almost the same time; however, Bitcoin blocks are created approximately every ten minutes.

Does anyone of us notice any account balances? Blockchains are not banks, and here is a good example to distinguish between the two. A cryptocurrency wallet will estimate the balance by scanning the complete chain and summing up how many coins we received and spent. We do not have to rely on a bank to tell us the amount present in the account. We are only trusting the network instead of one mega-corporation. Isn't it fascinating?


Summing Up
In the following tutorial, we have successfully built a blockchain that we can fill with blocks full of transactions of cryptocurrency; however, this is not a secure network. First, we created a block any time somebody calls newBlock(), and there are no conditions. The newBlock() method requires a parameter called proof; however, that can be anything in our case. It can either be by a number or string saying, "hello world", or literally anything.

In the network of Bitcoin, there is a consensus mechanism in a place called Proof of Work, which illustrates the rules by which security is achieved. A proof is a random number that is very difficult to find unless we have some dedicated high-performance machines working around the clock.

There are many other details we are missing, such as fees for the miners to collect, a count of the transaction, public/private key, a Merkle tree structure, and more. However, the above walkthrough was helpful for us as a fundamental example of the moving segments in a blockchain.