# Merkle Tree

# Intro to Merkle Tree
A Merkle tree is a hash-based data structure that is a generalization of the hash list. It is a tree structure in which each leaf node is a hash of a block of data,
and each non-leaf node is a hash of its children. Typically, Merkle trees have a branching factor of 2, meaning that each node has up to 2 children.
Merkle trees are used in distributed systems for efficient data verification. They are efficient because they use hashes instead of full files. 
Hashes are ways of encoding files that are much smaller than the actual file itself. 
Currently, their main uses are in peer-to-peer networks such as Tor, Bitcoin, and Git.

## What Is a Merkle Tree?

A Merkle tree is a data structure that is used in computer science applications. In bitcoin and other cryptocurrencies,
Merkle trees serve to encode blockchain data more efficiently and securely.They are also referred to as "binary hash trees."


## Overview of Merkle Tree
Merkle trees are typically implemented as binary trees, as shown in the following image. However, a Merkle tree can be created as an nn-nary tree,
with nn children per node.In this image, we see an input of data broken up into blocks labeled L1 though L4. Each of these blocks are hashed using some hash function.
Then each pair of nodes are recursively hashed until we reach the root node, which is a hash of all nodes below it.
![Breaking Data int bits of information](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Hash_Tree.svg/1920px-Hash_Tree.svg.png)

# Architecture of Merkle Tree
A Merkle tree totals all transactions in a block and generates a digital fingerprint of the entire set of operations, 
allowing the user to verify whether it includes a transaction in the block.Merkle trees are made by hashing pairs of nodes repeatedly until only one hash remains; this hash is known as the Merkle Root or the Root Hash.
They're built from the bottom, using Transaction IDs, which are hashes of individual transactions. 
Each non-leaf node is a hash of its previous hash, and every leaf node is a hash of transactional data.
Now, look at a little example of a Merkle Tree in Blockchain to help you understand the concept. 

Consider the following scenario: A, B, C, and D are four transactions, all executed on the same block. Each transaction is then hashed,
leaving you with: 

![Architecture of merkle tree](https://www.simplilearn.com/ice9/free_resources_article_thumb/Merkle_Tree_In_Blockchain_3.png)

Hash A 
Hash B
Hash C
Hash D
The hashes are paired together, resulting in:

Hash AB
and

Hash CD

And therefore, your Merkle Root is formed by combining these two hashes: Hash ABCD.
In reality, a Merkle Tree is much more complicated (especially when each transaction ID is 64 characters long). 
Still, this example helps you have a good overview of how the algorithms work and why they are so effective.

## Algorithm for Merkle tree
1.	Computer A sends a hash of the file to computer B..
2.	Computer B checks that hash against the root of the Merkle tree.
3.	If there is no difference, we're done! Otherwise, go to step 4.
4.	If there is a difference in a single hash, computer B will request the roots of the two subtrees of that hash.
5.	Computer A creates the necessary hashes and sends them back to computer B.
6.	Repeat steps 4 and 5 until you've found the data blocks(s) that are inconsistent.It's possible to find more than one data block that is wrong because there might be more than one error in the data.

## Advantages of Merkle tree
* ### Efficient verification- 
Merkle trees offer efficient verification of integrity and validity of data and significantly reduce the amount of memory required for verification. The proof of verification does not require a huge amount of data to be transmitted across the blockchain network. Enable trustless transfer of cryptocurrency in the peer-to-peer, distributed system by the quick verification of transactions.
* ### No delay- 
There is no delay in the transfer of data across the network. Merkle trees are extensively used in computations that maintain the functioning of cryptocurrencies.
* ### Less disk space-
Merkle trees occupy less disk space when compared to other data structures.
* ### Unaltered transfer of data- 
Merkle root helps in making sure that the blocks sent across the network are whole and unaltered.
* ### Tampering Detection- 
Merkle tree gives an amazing advantage to miners to check whether any transactions have been tampered with.
1. Since the transactions are stored in a Merkle tree which stores the hash of each node in the upper parent node, any changes in the details of the transaction such as the amount to be debited or the address to whom the payment must be made, then the change will propagate to the hashes in upper levels and finally to the Merkle root.
2. The miner can compare the Merkle root in the header with the Merkle root stored in the data part of a block and can easily detect this tampering.
* ### Time  Complexity- 
Merkle tree is the best solution if a comparison is done between the time complexity of searching a transaction in a block which as Merkle tree and another block that has transactions arranged in a linked list, then-

Merkle Tree search- O(logn)
Linked List search- O(n)
where n is the number of transactions in a block.


## Implementation of Merkle tree using JAVA
import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.ArrayList;

public class MerkleTree {

    public String createMerkleTree(ArrayList<String> txnLists) {
        ArrayList<String> merkleRoot = merkleTree(txnLists);
        return merkleRoot.get(0);
    }

    private ArrayList<String> merkleTree(ArrayList<String> hashList){
        //Return the Merkle Root
        if(hashList.size() == 1){
            return hashList;
        }
        ArrayList<String> parentHashList=new ArrayList<>();
        //Hash the leaf transaction pair to get parent transaction
        for(int i=0; i<hashList.size(); i+=2){
            String hashedString = getSHA(hashList.get(i).concat(hashList.get(i+1)));
            parentHashList.add(hashedString);
        }
        // If odd number of transactions , add the last transaction again
        if(hashList.size() % 2 == 1){
            String lastHash=hashList.get(hashList.size()-1);
            String hashedString = getSHA(lastHash.concat(lastHash));
            parentHashList.add(hashedString);
        }
        return merkleTree(parentHashList);
    }

    public static String getSHA(String input)
    {
        try {

            // Static getInstance method is called with hashing SHA
            MessageDigest md = MessageDigest.getInstance("SHA-256");

            // digest() method called
            // to calculate message digest of an input
            // and return array of byte
            byte[] messageDigest = md.digest(input.getBytes());

            // Convert byte array into signum representation
            BigInteger no = new BigInteger(1, messageDigest);

            // Convert message digest into hex value
            String hashtext = no.toString(16);

            while (hashtext.length() < 32) {
                hashtext = "0" + hashtext;
            }

            return hashtext;
        }

        // For specifying wrong message digest algorithms
        catch (NoSuchAlgorithmException e) {
            System.out.println("Exception thrown"
                    + " for incorrect algorithm: " + e);

            return null;
        }
    }

}

# Complexity 
Merkle trees have very little overhead when compared with hash lists. Binary Merkle trees, like the one pictured above, 
operate similarly to binary search trees in that their depth is bounded by their branching factor, 2.
Included below is worst-case analysis for a Merkle tree with a branching factor of kk.

![image](https://user-images.githubusercontent.com/87685013/165544705-03999ecc-174a-46b1-9b79-36649792e3a7.png)

However, a very important aspect of Merkle trees (and their biggest use case) is in the synchronization of data. In the typical case,
this synchronization will be a \log_2(n)log(n) operation because it is based on traversal and search. 
However, there is a worst-case situation where there are no nodes in common. In this scenario, synchronization is akin to making a complete copy of the file,which will be a O(n)O(n) operation.

![image](https://user-images.githubusercontent.com/87685013/165545005-f780908a-7f01-4ead-ada8-eef66be419dc.png)

## References
* https://www.simplilearn.com/tutorials/blockchain-tutorial/merkle-tree-in-blockchain#:~:text=Working%20of%20Merkle%20Trees,-A%20Merkle%20tree&text=Merkle%20trees%20are%20made%20by,are%20hashes%20of%20individual%20transactions.
* https://medium.com/@vinayprabhu19/merkel-tree-in-java-b45093c8c6bd
* https://brilliant.org/wiki/merkle-tree/
* https://www.investopedia.com/terms/m/merkle-tree.asp
