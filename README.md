---
title: "A query-based approach to GWAS through integration of Blockchain and Paillier Encryption"
output:
  html_document: default
  pdf_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

###**1. Abstract**
We developed a query-based system which allows users to respond using their genome is developed via incorporating additive homomorphic encryption into a blockchain. This allows for secure and efficient accumulation of data in the form of genetic responses based on queries, which provides the security and robustness a regular spreadsheet or password-protected document is unable to offer. In the deployment of our system on real biological data, we used 73 VCF files obtained from OpenSNP belonging to users which have Hay Fever. The files can be found [here](https://opensnp.org/phenotypes/195)

###**2. Introduction**
All of us are well familiar with Facebook, a social networking platform allowing free open transmission of information, as well as Google Spreadsheet, which allows the upload and manipulation of data among users which have access to the spreadsheet. However, open sharing and manipulation of information may not necessarily be desired, especially in the field of Genomics. We therefore seek to develop a platform similar to Facebook and Google Spreadsheet which is adapted to the field of Genomics.

This is done through the integration of Blockchain and Paillier homomorphic encryption technologies, which we believe allows for a secure method of transmitting and storing private information in a scalable fashion without the risk of personal information being seen or leaked.

#####**Figure 2.1: Operation of our system**
![k](https://user-images.githubusercontent.com/45550811/52999384-91da0500-3460-11e9-82f2-750061e9b27a.png)  

Using our system, anyone who suspects that a particular allele mutation causes a particular genetic disease in humans starts a genesis block containing a starting number (encrypted using the Paillier cryptosystem) in the blockchain for blocks to be subsequently added to it. They will generate the Paillier public and private key pair and will send out the public key along with a query for respondents with the genetic disease in question to respond to by encoding either a "1" if they have the allele mutation in question or a "0" if they do not, using the public key and adding this to a block which is subsequently added to the blockchain. The person who sent out the query can then use the private key to decrypt the encrypted responses and analyze the proportion of respondents who have the mutation, with a high proportion indicating the allele in question is likely correlated to the disease.

###**3. Opportunities**
Our system can be implemented and used by anyone, and the 2 figures below illustrate the different scenarios based on the targeted audience.

#####**Figure 3.1: Implementation of our system within a group of family members**
![m](https://user-images.githubusercontent.com/45550811/52999387-92729b80-3460-11e9-94e8-ecb579fdb6fb.png)

In this scenario, family members can have their genomic data sequenced and obtain the necessary digital files to start the process. The initiator of the blockchain can then generate the paillier cryptosystem key pair, namely the red public key and the yellow private key, followed by a random integer x, as shown in **Figure 3.1**. After encrypting x with the public key, the initiator can then encrypt his/her response with the public key and generate the genesis block, with the “Timestamp” being the exact date and time it was created, the “Data” being the encrypted sum of his/her response and x, the unique alphanumeric “Hash”, the hashed “Identity” of each participant and finally the “Previous Hash”, which would be 0000 since there are no existing blocks before the genesis block. After this, the initiator would upload the blockchain, where other family members can subsequently add in their own blockchains with the respective information. It is important to note that the “Hash” of block 1 **will** match the “Previous Hash” of the next block and the data in each subsequent block is just the addition of its encrypted response and the encrypted value in the previous block. For instance, from **Figure 3.1**, the “Data” section in block 2 would be the addition of its encrypted response, Enc(0), and the encrypted value in the previous block 1, Enc(X) + Enc(1), which would give the result Enc(1+X+0). After each family member has added their respective blockchains with the respective parameters, the initiator can then decrypt the final numeric value with the private key to obtain the response proportion, as shown above in **Figure 3.1**.

#####**Figure 3.2: Implementation of our system within a group of doctors and patients**
![n](https://user-images.githubusercontent.com/45550811/52999374-90a8d800-3460-11e9-8d04-a93604240a91.png)
 
In this scenario, the doctor would like to enquire if a certain phenotype he observe in some patients is due to their genetics. To obtain a larger sample size, the doctor In Charge (IC) would cooperate with his/her other colleagues. The doctor would make use of our query-based approach system and generate the paillier cryptosystem key pair, namely the red public key and yellow private key as shown in **Figure 3.2**. After generating the random number x, the doctor IC would distribute the keys accordingly: The public keys to the participants and the private keys to the other doctors. After making sure each of the participants have their sequenced genomic file with them, the doctor IC would send out a query about a particular allele that he is interested in. Participants would then encrypt their responses to the query with the public key provided and after the Peer-To-Peer(P2P) network is established between the doctors, the doctor IC would create the genesis block and upload the blockchain. Subsequently, patients can add their own blocks to the growing chain with the same synchronised data shared and displayed across all the nodes. Other than the increase in sample size, the collaboration between the doctors would ensure that even if a node is breached, the overall blockchain would still remain intact, as discussed earlier. After this process, the doctor IC can decrypt the “Data” section of the last block to obtain the response proportion of the participants.


###**4. Comparison between GWAS and our system**

###**5. Technologies implemented**

#####**5.1: Paillier homomorphic encryption**
A set of paillier keys is used, the public key and the private key. The public key is used to encrypt the necessary data while the private key is used to decrypt the encrypted data. One special characteristic of this is that computations, such as addition, can still be done on the encrypted data. Meaning, if a + b = c, then Decrypt[Encrypt(a)+Encrypt(b)] = Decrypt[Encrypt(c)] = c. This characteristic is useful in our approach, since the response proportion still needs to be known at the end of the day.

#####**Figure 5.11: How Paillier homomorphic encryption functions in our system**
![j](https://user-images.githubusercontent.com/45550811/52999382-91da0500-3460-11e9-9299-c272f4a01017.png)

#####**5.2: Example of implementation of Paillier homomorphic encryption in our system**
Generation of public and private key, and encryption of starting message
```
keyPair <- keys(modulusBits)  #Generate Paillier public and private keys
message <- sample.int(message_range,1,replace=TRUE)  #Generate starting msg
message  #print starting msg
enc_message <- keyPair$pubkey$encrypt(message)  #encrypt starting msg
enc_new_message <- enc_message
```

#####**5.3: Blockchain**
The blockchain is a type of digitalized, public ledger that is open to anyone: It contains replicated, synchronised and shared data that can potentially be geographically spread across the world. One block can contain 5 sets of information, mainly: The exact time it was created (Timestamp), the data that is encoded into the block (Data), a verification id (Hash), the verification id of the previous block (Previous Hash) and finally the identity of the owner of the block. The Hash function is a function which generates a unique and long string of alphanumeric characters based on the other parameters of the block. For a blockchain to be valid, the “Hash” of the current block must be the exact copy of the “Previous Hash” of the next block. Therefore, if any parameter in a block is changed, the Hash would change as well, and since the “Hash” of that block and the recorded “Previous Hash” of the next block isn’t the same, the blockchain would become invalid. As the blockchain uses a Peer-to-Peer (P2P) technology, as long as the majority of the nodes have a consensus to the correct blockchain, the system would not be compromised.

#####**Figure 5.31: How the Blockchain functions in our system**
![a](https://user-images.githubusercontent.com/45550811/52999375-90a8d800-3460-11e9-82b4-052b54e1f9cb.png)

#####**5.4: Example of implementation of Blockchain in our system**
Generation of genesis block
```
block_genesis <-  list(identity = "Doctor",
                   	timestamp = Sys.time(),
                   	data = enc_new_message,
                   	previous_hash = "0")
blockchain <- list(block_genesis)	#create blockchain
```
#####**5.5: Analogy to Blockchain**
![b](https://user-images.githubusercontent.com/45550811/52999376-90a8d800-3460-11e9-831f-422d5cd8271b.png)


To visualise the Hash and the Previous Hash functions better, we can imagine the blockchain as a chain of unique NTUC trolleys. Each trolley would represent a block: The metal chain would be the block’s “Hash” while the counter on the next trolley would represent the “Previous Hash”. As each trolley is unique, block 1’s metal chain can only fit into Block 2’s counter while block 2’s “metal chain can only fit into Block 3’s counter. Let’s say that for a certain supermarket company, a manager would check the trolleys 10 minutes after they arrive from a new shipment. If the trolleys are all connected properly in a chain, the manager accepts the shipment. If not, the shipment would be rejected a new one would be requested. Now, imagine that there is a saboteur, who wants to sabotage the trolleys by replacing them with spoilt trolleys. Let’s say he wants to replace the 2nd trolley with a spoilt one.

However, it would be a very tedious task, as if an entirely new 2nd trolley is inserted into the chain, its current metal chain would be changed entirely, hence this metal chain cannot be fitted into the counter of the 3rd trolley. Therefore, the 3rd trolley also has to be replaced with a new trolley and with a new counter, so the metal chain from the new 2nd trolley can be fitted and connected to the 3rd trolley. However, with a new 3rd trolley, the metal chain would also be replaced with a new one, meaning that it cannot be fitted into the counter of the 4th trolley. Hence, the entire chain of trolleys starting from the first replaced trolley needs to be replaced to make a proper and valid blockchain. Other than that, this would have to happen under 10 minutes before he loses his chance.

Now, even if the saboteur wants to remove the 2nd trolley entirely, as the blockchain must be a complete chain of blocks, it’s would mean that Block 1’s metal chain would have to match Block 3’s counter, and since each trolley’s metal chain can only fit into the next trolley’s counter, there would exist a broken chain between block 1 and block 3, hence the chain of trolleys would be broken and not be considered a valid chain by the manager.

#####**5.6: Hash**
The use of hash functions to hash each user's identity ensures the protection of identities, and is additionally used to hash the contents of each block in the blockchain, ensuring data integrity. This is due to the properties of a blockchain, as the blocks in the chain are held together by their hashes, which depend on their contents. Hence editing the contents in any block renders the rest of the chain invalid, preventing hacking of the chain.

#####**5.7: Example of implementation of Hashes in our system**
Function to hash blocks before adding them to the Blockchain
```
#function to generate and add a hash of the current block to the block (hash is based on identity, cumulative sum of votes and hash of the previous block)
hash_block <- function(block){
  block$new_hash <- digest(c(block$identity,
                         	block$data,
                         	block$previous_hash), "sha256")
  return(block)
}
```

###**6. Comparison between our system and a digital spreadsheet**
![l](https://user-images.githubusercontent.com/45550811/52999385-92729b80-3460-11e9-880d-8725777789fb.png)
From this figure, we can observe the distinct difference between these 2 ways of storing data. In terms of access, the blockchain allows anyone to view it, but only a select few, such as the doctor in our case, would be able to view the data inside the blockchain with the use of the private key. On the other hand, for a digital spreadsheet such as google spreadsheet, everyone with the password, participant or not, has access to it and everyone is able to observe its contents. The blockchain uses a peer to peer (P2P) distribution network to keep the blockchain updated and data storage is decentralised, while the digital spreadsheet usually uses a client and server distribution, with data being sent to the main server before it would be sent back, hence it utilises a centralised data storage system. For the contents inside each storage system, the pairing of blockchain and additive homomorphic encryption has allowed the data to be encrypted, synchronised and shared between participants in real time, with the addition of data in blockchains being orderly and well controlled. On the other hand, the password protected digital spreadsheet offers little security as the data is unencrypted and anyone with access to the spreadsheet is able to tamper with it. Addition of data is uncontrollable and unorderly while the password protection is ineffective as it would be easy to crack and intercepted. For the blockchain, when data has been uploaded, it would be immutable. The blockchain operates on a public system, with it being accessible to everyone and anonymity ensured for each participant. The digital spreadsheet, on the other hand, uses a private-based system where the identity of each participant may not be anonymous and they are trusted and pre-vetted before being granted access to the spreadsheet. In terms of its defense against hackers, although a centralised data storage system is used and maximum security is provided for the server, the data in question is still vulnerable. The hacker could either intercept transmissions between the participant and the server to obtain the password for the spreadsheet; or he could simply break the password securing the document. With the data in the spreadsheet being unencrypted, the hacker would be exposed to a trove of data that can be manipulated, leading to dire consequences. However, for the blockchain, it utilises the P2P network to deter hackers as they would need to replace the blockchains in >50% of all the nodes to successfully alter the final product. In addition, the altering of data in each block is made troublesome and time consuming for the saboteur through the use of the Hash function. The data is also protected with paillier encryption, hence only those with the private key, namely the doctors in our case, are able to decrypt the encrypted data.


###**7. Explanation and deployment of our system**
####Here's a general overview of how our code works:
**Initialisation will be done by the initiator:** 
![c](https://user-images.githubusercontent.com/45550811/52999377-91416e80-3460-11e9-9c54-025eec6c7b0e.png)
**Each respondent will then carry out the following steps:**
![d](https://user-images.githubusercontent.com/45550811/52999378-91416e80-3460-11e9-8584-23f4d4835671.png)
![e](https://user-images.githubusercontent.com/45550811/52999380-91416e80-3460-11e9-9d5e-8b6d90d36ccb.png)
**Finally the initiator will carry out the analysis:**
![f](https://user-images.githubusercontent.com/45550811/52999381-91da0500-3460-11e9-94cd-3bb0a0950957.png)

We will now proceed on to the analysis of the files we downloaded from OpenSNP to test out our system on real data. There are 3 different phases, namely: The initiation phase, the extension phase and the analysis phase. Firstly, the initiation phase:

####**7.1: Initialisation phase**

Firstly, we need to call the relevant package and functions associated with it.
```
library(VariantAnnotation)
library(tidyverse)
library(digest)
library(homomorpheR)
```
Next, we need to initialize and specify the parameters of the analysis.
```
modulusBits = 1024  #size of public and private key
message_range = 100000  #range of values to pick for first message
number_of_patients = 73  #number of patients
snp_id <- "rs4420638" #specified allele name 
base <- "AG"  #specified genotype to check against
```
Next, we need to initialize the files which we are analyzing.
```
p1 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/3639.23andme.2392"
p2 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/3695.23andme.2438"
p3 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/3799.23andme.3845"
p4 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/4509.23andme.3106"
p5 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/4962.23andme.3503"
p6 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/5853.23andme.4332"
p7 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/5866.23andme.4342"
p8 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/5917.23andme.4386"
p9 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/6143.ancestry.4660"
p10 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/6257.23andme.4754"
p11 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/943.23andme.470"
p12 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1626.23andme.924"
p13 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1233.23andme.652"
p14 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1712.23andme.988"
p15 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1627.23andme.925"
p16 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1133.23andme.587"
p17 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1075.23andme.550"
p18 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/807.23andme.389"
p19 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1190.23andme.626"
p20 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1742.23andme.1010"
p21 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1273.23andme.675"
p22 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1269.23andme.673"
p23 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1633.23andme.929"
p24 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/368.23andme.168"
p25 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1780.23andme.1036"
p26 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1641.23andme.935"
p27 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1384.23andme.734"
p28 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/500.23andme.2637"
p29 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1440.23andme.782"
p30 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1465.23andme.803"
p31 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1499.23andme.818"
p32 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/187.23andme.267"
p33 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1568.23andme.878"
p34 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1664.23andme.952"
p35 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1686.23andme.969"
p36 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1808.23andme.1056"
p37 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1822.23andme.1067"
p38 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1876.23andme.1114"
p39 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1877.23andme.1115"
p40 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1970.23andme.1158"
p41 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2009.23andme.1208"
p42 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2013.23andme.1197"
p43 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/1024.23andme.510"
p44 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2052.23andme.1229"
p45 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/613.23andme.285"
p46 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2090.23andme.1265"
p47 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2151.23andme.1303"
p48 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/4191.23andme.2844"
p49 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2314.23andme.1424"
p50 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2333.23andme.1443"
p51 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2362.23andme.1461"
p52 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2512.23andme.1541"
p53 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2648.23andme.1664"
p54 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/3046.23andme.1955"
p55 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/35.23andme.5822"
p56 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/3503.23andme.2288"
p57 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/3611.23andme.2372"
p58 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/7876.23andme.6234"
p59 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/2288.23andme.1401"
p60 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/3306.23andme.2150"
p61 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/4095.23andme.2771"
p62 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/4147.23andme.2811"
p63 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/3990.23andme.2684"
p64 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/4428.23andme.3034"
p65 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/4784.23andme.3373"
p66 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/5027.ancestry.3561"
p67 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/5629.23andme.4119"
p68 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/6493.ancestry.4913"
p69 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/6858.23andme.5234"
p70 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/7344.ancestry.5705"
p71 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/7565.23andme.5911"
p72 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/7572.ancestry.5917"
p73 <- "C:/Users/arceu/ARP/Hay_Fever_Positive/429.23andme.187"
```
Next, we need to assign identities to each respondent.
```
p1_identity <- "3X45tyggJ9"  #Identity of respondent 1
p2_identity <- "65tKKuioP0"  #Identity of respondent 2
p3_identity <- "uoikA34RTY"  #Identity of respondent 3 and so on
p4_identity <- "7thLK9iU8O"
p5_identity <- "uO09IU95ht"
p6_identity <- "56Tr4eF329"
p7_identity <- "3GxPGGz71Q"
p8_identity <- "6eyhYU782T"
p9_identity <- "L5uj0rMjGf"
p10_identity <- "91dXb5wvdb"
p11_identity <- "3X45tyggJ8"
p12_identity <- "3X45tyggJ7"
p13_identity <- "3X45tyggJ6"
p14_identity <- "3X45tyggJ5"
p15_identity <- "3X45tyggJ4"
p16_identity <- "3X45tyggJ3"
p17_identity <- "3X45tyggJ2"
p18_identity <- "3X45tyggJ1"
p19_identity <- "3X45tyggJ0"
p20_identity <- "3X45tyggK9"
p21_identity <- "3X45tyggK8"
p22_identity <- "3X45tyggK7"
p23_identity <- "3X45tyggK6"
p24_identity <- "3X45tyggK5"
p25_identity <- "3X45tyggK4"
p26_identity <- "3X45tyggK3"
p27_identity <- "3X45tyggK2"
p28_identity <- "3X45tyggK1"
p29_identity <- "3X45tyggK0"
p30_identity <- "3X45tyggL9"
p31_identity <- "3X45tyggL8"
p32_identity <- "3X45tyggL7"
p33_identity <- "3X45tyggL6"
p34_identity <- "3X45tyggL5"
p35_identity <- "3X45tyggL4"
p36_identity <- "3X45tyggL3"
p37_identity <- "3X45tyggL2"
p38_identity <- "3X45tyggL1"
p39_identity <- "3X45tyggL0"
p40_identity <- "3X45tyggM9"
p41_identity <- "3X45tyggM8"
p42_identity <- "3X45tyggM7"
p43_identity <- "3X45tyggM6"
p44_identity <- "3X45tyggM5"
p45_identity <- "3X45tyggM4"
p46_identity <- "3X45tyggM3"
p47_identity <- "3X45tyggM2"
p48_identity <- "3X45tyggM1"
p49_identity <- "3X45tyggM0"
p50_identity <- "3X45tyggN9"
p51_identity <- "3X45tyggN8"
p52_identity <- "3X45tyggN7"
p53_identity <- "3X45tyggN6"
p54_identity <- "3X45tyggN5"
p55_identity <- "3X45tyggN4"
p56_identity <- "3X45tyggN3"
p57_identity <- "3X45tyggN2"
p58_identity <- "3X45tyggN1"
p59_identity <- "3X45tyggN0"
p60_identity <- "3X45tyggO9"
p61_identity <- "3X45tyggO8"
p62_identity <- "3X45tyggO7"
p63_identity <- "3X45tyggO6"
p64_identity <- "3X45tyggO5"
p65_identity <- "3X45tyggO4"
p66_identity <- "3X45tyggO3"
p67_identity <- "3X45tyggO2"
p68_identity <- "3X45tyggO1"
p69_identity <- "3X45tyggO0"
p70_identity <- "3X45tyggP9"
p71_identity <- "3X45tyggP8"
p72_identity <- "3X45tyggP7"
p73_identity <- "3X45tyggP6"
```
The following portion of code is to be run by the initiator of the blockchain:
Firstly, the Paillier public and private keys are generated using the keys() function. 

```
keyPair <- keys(modulusBits)  
```

#####**7.11: keys()**
We define a function which allows us to quickly generate the Paillier public and private key pair based on the desired bit size of the key.
The keys() function simply accepts the input of the modulus bit size of the paillier key pair and calls The function PaillierKeyPair$new(), which uses the input of the keys() function to generate the public key and the private key. This key pair is then returned by the keys() function in a variable named keyPair.

```
keys <- function(modulusBits){  
  keyPair <- PaillierKeyPair$new(modulusBits)
  return(keyPair)
}
```

Next, the random starting number is generated using the sample.int() function.

```
message <- sample.int(message_range,1,replace=TRUE)  
```

#####**7.12: sample.int()**
The sample.int() function is a standard R function used to output randomized nonnegative integer from 1 to a specified nonnegative integer. The first input specifies the range of numbers to randomize, the second input specifies how many randomized numbers to output, and the third input specifies whether or not randomized numbers can be repeated. For our system, we only need 1 random starting number, hence the inputs as message_range, 1, and replace=TRUE.

The random starting number is then printed out and encrypted using the Paillier public key.

```
message  
enc_message <- keyPair$pubkey$encrypt(message) 
enc_new_message <- enc_message
```

#####**7.13: KeyPair\$PubKey\$Encrypt()**
The KeyPair\$PubKey\$Encrypt() function accepts the input of a number and generates an encrypted version of the number using the public key from the paillier key pair. The first \$ sign states that we are selecting the Public key from the KeyPair and the second \$ sign states that we are encrypting the input using the Public key.

Next, the genesis block is generated containing the initiator's identity, the exact time, the encrypted starting number and the previous hash of the block which is set as 0, using the list() function.

```
block_genesis <-  list(identity = "Doctor",
                       timestamp = Sys.time(),
                       data = enc_new_message,
                       previous_hash = "0")
```

#####**7.14: List()**
This function literally creates a list of items, and these items can store data. Think of it as a container manufacturing line, where after you make the line, you can still make more containers and access whatever is inside the containers. In our system, we use the list() function to simulate the parameters in each block and to simulate a blockchain.

The genesis block is then hashed using the hash_block function.

```
block_genesis <- hash_block(block_genesis)  
```

We define a function to hash a newly created block before its added to the blockchain.

#####**7.15: Hash_block()**
The Hash_block() function is, as its name suggests, used to hash a block after its created. The function takes in a newly created block as input. It then creates a unique hash by combining the identity, data and previous_hash parameters of the block into an array and hashing this array using both the sha256 Hash algorithm as well as the R digest() function obtained from the digest package in R. It then stores this created hash in a newly generated parameter of the block named "new_hash". The block, now containing its own hash, is then designated as output of the Hash_block function.

```
hash_block <- function(block){
  block$new_hash <- digest(c(block$identity,
                             block$data,
                             block$previous_hash), "sha256")
  return(block)
}
```

The blockchain is then simulated using an array via the list() function and the genesis block is designated as the first block in the blockchain.

```
blockchain <- list(block_genesis)   
previous_block <- blockchain[[1]]
```

####**7.2: Extension phase**

Next, we initialize an error list to contain the file numbers which belong to repeated individuals.

```
error_list <- c() 
```

#####**7.21: error_list()**
The error_list is defined as an R array, and is used to track files which are added by any same individual twice or more in order to prevent double-counting. Any individual who tries to add more than one file will fail on the second or higher attempt, since the check_identity() which runs before a block can be created will return a 1, indicating the presence of a repeat identity, after which the respondent's number in question will be sent to the error_list. This error list is subsequently printed out. This is especially useful when the person creating the blockchain already has a bulk of VCF files (along with all corresponding identities) to begin with, in which case the files can be added rapidly through a for loop, and the exact files which have repeated identities will be displayed in the error_list, leaving out the need for a tedious manual check.

A variable to measure the length of the blockchain is initialized and set to 0.

```
length_of_blockchain = 0
```
Next, a for loop is constructed to go through every block in the blockchain.

```
for (i in 1:number_of_patients) {   
```
For each, block, first print out a statement indicating which file is being read.

```
  cat("Reading file ",i," out of ",number_of_patients,"\n") 
```
Next, check if there is a repeat identity

```
  if((check_identity(blockchain, i-length(error_list), all_identity[i])==0)){ 
```
We define a function to check identity of each patient before a patient adds a block, to ensure there's no double counting.

#####**7.22: Check_identity()**
This function checks the given identity against all the identities so far within the blockchain. By inputting the current blockchain, number of patients so far along with the identity of the given patient, this function accesses every single block to compare the identities of the given patient with the ones in the blocks. It will return a '0' if there is no error or a '1' if it is a repeat identity.

```
check_identity <- function(blockchain, patients, identity){   
```
Firstly, it initializes a flag with value 0 as a value to check against

```
  flag = 0   
```
Then it constructs a for loop to go through every block in the blockchain
.
```
  for (i in 1:patients){   
```
For every block, it checks if patient's identity matches with that which is stored in the current block. If they are not equal, then it  sets the flag value to 1 to indicate the patient has already previously added a block under the same identity and sets the loop's counter equal to the number of patients to end the loop.

```
  if (identity == blockchain[[i]][1]$identity){    
      flag = 1  
      i = patients   
    }
  }
```
It then returns the final flag value.

```
  return (flag)   
}
```

Determine the respondent's binary response by applying the File_to_binary() function.

```
    patient_vote <- File_to_binary(all_file[i], snp_id, base) 
```

We define the File_to_binary function, which takes in the respondent's file, the queried allele name and the queried genotype.

#####**7.23: File_to_binary()**
This function takes a file and returns a '1' or a '0' based on the patient's vote, thus the name file to binary. By inputting the patient's file, SNP position and the base the doctor wants, it will access the file for that snp position. If the file does not have the position, a '2' is returned for an error to be flagged. If it does, it will read the file in table format. The 3 most commonly used files for storing SNP data are ftdna-illumina, 23andme and ancestry files, all of which have different number of columns, 1, 4 and 5 respectively. We use this to identify what kind of format the file is in, then we extract the base pair data from the given position. If this matches the doctor's pair, it will return a '1'. If not, it will return a '0'.

```
File_to_binary <- function(file,snp_id,doctor_base){
```
The function then reads the respondent's file and stores the data in a table format using the read.table() function in R.

```
  table <- read.table(file)   
```
It then checks the number of columns in each file and applies the appropriate file reading process for each type of file: ftdna-illumina files have 1 column, 23andme files have 4 columns and ancestry file have 5 columns.

If the file is a ftdna-illumina VCF file:

```
  if (ncol(table) == 1){  
```
The name "finder" is set as the column name in the table.

```
    names(table) <- c("finder") 
```
It then looks for the row which contains the queried allele name and retrieves the genotype from the end of the string

```
    base <- gsub(",","",substr(table[grep(paste("^",snp_id,",",sep=""),table$finder),], nchar(snp_id)+4, nchar(snp_id)+6))  
  }
```
If the file is a 23andme VCF file:

```
  if (ncol(table) == 4){  
```
The names "rsid", "chrom", "position" and "genotype" are assigned to the 1st to 4th columns respectively.

```
    names(table) <- c("rsid", "chrom", "position", "genotype")   
```
It then looks for the row which matches the queried allele name and retrieves the genotype from the last column of the table

```
    base <- paste(table[which(table$rsid==snp_id),]$genotype)   
  }
```
If the file is an ancestry file:

```
  if (ncol(table) == 5){   
```
The names "rsid", "chrom", "position", "allele1" and "allele 2" are assigned to the 1st to 5th columns respectively.

```
    names(table) <- c("rsid", "chrom", "position", "allele1", "allele2") 
```

It then looks for the row which matches the queried allele name and retrieves the genotype by combining the bases in the last 2 columns.

```
    base <- paste(paste(table[which(table$rsid==snp_id),]$allele1),paste(table[which(table$rsid==snp_id),]$allele2),sep="")   
  }
```
The retrieved genotype is then printed out.
```
  print(base)
```
It then returns a 1 if the respondent's genotype matches the queried genotype, and returns a 0 if it does not.
```
    if (base == doctor_base)  
      return(1) 
  else
      return(0) 

  }
}
```

Display the respondent's binary response.
```
    cat("File ",i," vote is ",patient_vote,"\n") 
```
The binary response is then encrypted using the enc_vote() function.
```
    enc_patient_vote <- enc_vote(patient_vote,keyPair)  
```

We define a function which allows us to encrypt the respondent's binary response.

#####**7.24: enc_vote()**
The enc_vote() function accepts the input of the participant’s response (1 or 0) as well as the paillier key pair. After calling the KeyPair\$PubKey\$Encrypt() function, the enc_vote() function returns the encrypted participant’s response in the variable enc_patient_vote.
```
enc_vote <- function(patient_vote,keyPair){  
  enc_patient_vote <- keyPair$pubkey$encrypt(patient_vote)
  return(enc_patient_vote)
}
```

The encrypted binary response is then displayed.
```
    print(enc_patient_vote)  
```
The encrypted response is then added to the cumulative sum of responses to update it.
```
    enc_new_message <- add_vote(enc_patient_vote,blockchain[[i-length(error_list)]][3]$data) 
```

We define a function to add the respondent's encrypted binary response to the encrypted cumulative sum of responses using the Paillier public key.

#####**7.25: add_vote()**
The add_vote() function accepts the input of the “data” section from the previous block (enc_patient_vote) as well as the “data” section from the current block (enc_message). It calls the function keyPair\$pubkey\$add(), which uses the input, enc_message and enc_patient_vote, to generate the encrypted sum from the addition of these 2 variables. The add_vote() function will then return the encrypted sum in a variable named enc_new_message.
```
add_vote <- function(enc_patient_vote,enc_message){  
  enc_new_message <- keyPair$pubkey$add(enc_patient_vote,enc_message)
  return(enc_new_message)
}
```

The previous block's hash, the updated cumulative sum of responses and the respondent's identity is then used as inputs in the gen_new_block() function to generate a new block to be added to the blockchain.
```
    block_to_add <- gen_new_block(previous_block, enc_new_message, all_identity[i])  
```

We define a function to generate a new block to add to the blockchain.

#####**7.26: gen_new_block()**
This function creates a new block with the given parameters and returns it. By inputting the previous block, the cumulative sum and identity of the patient, it generates a new block, containing the identity of the patient, timestamp of when it was created, the cumulative sum and the hash of the previous block. It then uses the hash_block() function, returning a new block.
```
gen_new_block <- function(previous_block, votesum, identity){
```
It creates a new Block containing the respondent's identity, exact time, cumulative sum of votes and hash of the previous block.
```
  new_block <- list(identity = identity,
                    timestamp = Sys.time(),
                    data = votesum,
                    previous_hash = previous_block$new_hash)
```
It then hashes the new block by calling the hash_block function defined earlier before returning the hashed block as output.
```
  new_block_hashed <- hash_block(new_block)
  
  return(new_block_hashed)
}
```

The blockchain is then updated, taking into account the repeated identities
```
    blockchain[i+1-length(error_list)] <- list(block_to_add)  
    cat("Block ",i," added to blockchain\n") 
    previous_block <- block_to_add  
    length_of_blockchain = i
  }
```
If there is a repeat identity, the error list is updated with the file number corresponding to the repeat identity.
```
  if(check_identity(blockchain, i-length(error_list), all_identity[i])==1){  
    error_list <- c(error_list, i)  
  }
}  
```
If the length of the error list is greater than 0, indicating an error is present, the following process is taken.
```
if (length(error_list) > 0){
```
The number of files in the error list, as well as the specific file numbers corresponding to the repeated identities, are printed.
```
  cat("Number of error files is ",length(error_list),"\n")
  print(error_list)  
```
The number of respondents is then updated and displayed.
```
  number_of_patients = number_of_patients - length(error_list) 
  cat("Updated number of patients: ",number_of_patients,"\n")  
```
Next, a for loop is constructed to go through the error list.
```
  for (i in 1:length(error_list)){ 
```
It then updates the array of VCF files and array of identities by getting rid of the repeat identities and the files corresponding to them.
```
    all_file = all_file[-error_list[i]]  
    all_identity = all_identity[-error_list[i]] 
  }
}
```

####**7.3: Analysis phase**

The cumulative sum of responses is then retrieved from the last block of the blockchain using the get_data() function.
```
final_message <- get_data(blockchain,number_of_patients,message)
```

We define a function to get cumulative sum of responses from a specific block.

#####**7.31: get_data()**

This function returns the cumulative sum of the votes so far from the blockchain. By inputting the current blockchain, patient number along with the initial message the doctor created, it decrypts the \$data found within a given block, and minuses off the initial message to give the cumulative sum of votes so far.
```
get_data <- function(blockchain, patient_number, message){
```
Firstly, it decrypts the sum of the cumulative sum of responses + initial random number using the Paillier private key.
```
  final_message <- keyPair$getPrivateKey()$decrypt(blockchain[[patient_number+1]][3]$data)  
```
It then obtains the cumulative sum of responses and returns it as the output.
```
  final_message <- final_message - message   
  return (final_message)   
}
```

The cumulative sum of responses is then printed out.
```
final_message
```
The proportion of respondents who responded 1 is then calculated using the proportion_of_1 function and displayed.
```
proportion <- proportion_of_1(blockchain,number_of_patients,message)
proportion  
```

We define a function to find proportion of respondents who responded "1".
```
proportion_of_1 <- function(blockchain,patient_number,message){
  final_message <- get_data(blockchain,patient_number,message)   #obtain the cumulative sum of votes from the specified block
  proportion <- final_message/patient_number   #obtain the proportion of patients who voted "1"
  return(proportion)   #return proportion of patients who voted "1"
}
```

Lastly, to validate the blockchain, the system checks and verifies the hashes in the entire blockchain using the check_hash() function.
```
check_hash(blockchain, number_of_patients)
```

We define a function to check hashes to ensure there is no disparity.

#####**7.32: check_hash()**
The check_hash() function checks and verifies that for every block in the blockchain, the current block's hash matches that of the previous block's.
```
check_hash <- function(blockchain, patients){
```
It starts by initializing a flag with value 1 as a value to check against.
```
  flag = 1   
```
It then constructs a for loop to go through every block in the blockchain.
```
  for (i in (patients)){
```
It then checks if the current block's hash value matches with that which is stored in the block directly after it, and if the 2 values are not equal, it initializes a variable called error to store the position of the blocks containing the hash mismatch, sets the loop's counter equal to the number of patients to end the loop and sets the flag value to 0 to indicate there is an error present.
```
    if (blockchain[[i]][5]$new_hash != blockchain[[i+1]][4]$previous_hash){    
      error = i   
      i = patients   
      flag = 0   
    }
  }
```
If the flag value equals 1, it prints the statement "Hashes checked and verified".
```
  if (flag == 1)  
    print ("Hashes checked and verified")   
```
If the flag value equals 0, it prints a statement indicating the position of the error.
```
  if (flag == 0)
    cat("Error, difference in block", i, "hash and block", i+1, "previous hash")  
}
```

####**7.4: Glossary**
Other functions we defined that are not used for this analysis are listed and explained in detail below:

We defined a function to get the individual response from a block. It does so by applying the get_data() function on the specified block as well as the block directly before it. It then returns the individual response as output.

#####**7.41: indiv_vote()**
This function returns the individual vote of any patient. By inputting the current blockchain, patient number along with the starting message, it uses the get_data() function to get the cumulative sum from that block and the block before. It then takes the difference between these two blocks to get the individual vote of the given patient.
```
indiv_vote <- function(blockchain,patient_number,message){
  ans <- get_data(blockchain,patient_number,message) - get_data(blockchain,(patient_number-1),message) 
  return(ans)  
} 
```
We defined a function to get sum of responses from the specified range of blocks by applying the get_data() function over the specified range of blocks.

#####**7.42: get_range_data()**
This function returns the sum of votes across the specified range of blocks. By inputting the current blockchain, patient_x and patient_y, along with the initial message the doctor created, we can find the sum of votes across patient_x to patient_y. We do this by finding the cumulative data within both blocks using the function get_data(), and taking the difference between these values.
```

get_range_data <- function(blockchain,patient_number_1,patient_number_2,message){
  sum_range <- get_data(blockchain,patient_number_2,message) - get_data(blockchain,patient_number_1-1,message) 
  return(sum_range)   
}
```
We defined a function to get proportion of respondents who responded "1" from a specified range of blocks by applying the get_range_data() function defined earlier over the specified range of blocks and dividing the output by the total number of respondents in the blockchain.

#####**7.43: vote_proportion_range()**
```

vote_proportion_range <- function(blockchain,patient_number_1,patient_number_2,message){
  total_votes <- get_range_data(blockchain,patient_number_1,patient_number_2,message)   #obtain sum of votes from the specified range of blocks
  patient_number <- patient_number_2 - patient_number_1 + 1  
  proportion <- total_votes/patient_number  
  return(proportion)  
}
```

Lastly, we defined a function to find a respondent's number given respondent's identity.

#####**7.44: get_patient_number()**
```
get_patient_number <- function(blockchain,patients,identity){
```
Firstly, it initializes a variable to store the respondent's number.
```
  patient_number = 0  
```
It then constructs a for loop to go through every block in the blockchain.
```
  for (i in 1:patients){  
```
For each block, it checks if the respondent's identity matches with that which is stored in the current block. If they are equal, it sets the respondent's number to the current block number - 1, and sets the loop's counter equal to the number of respondents to end the loop before returning the respondent's number.
```
    if (identity == blockchain[[i]][1]$identity){    
      patient_number = i - 1  
      i = patients   
    }
  }  
  return(patient_number)   
}
```


###**8. Conclusion**
Of the 96 files we analyzed, 73 had the SNP we were looking for. Thus, the blockchain, excluding the genesis block, was 73 blocks in length. The obtained proportion of users who tested positive for "GG" genotype of the specified allele was 49/73, which is around 67.1%. Thus, we can conclude that the "GG" genotype of rs1269486 has indeed, as literature suggested [ref](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5346276/), a high probability of correlation with causing Hay Fever. To ensure reliability of our system, we conducted a negative control test on the 73 files. We chose the allele rs4420638 with genotype AG, which has a high correlation to Alzheimer’s disease [ref](https://doi.org/10.1016/j.ajhg.2008.10.008) [ref](http://dx.doi.org/10.1001/archneurol.2007.3), for this test. Out of the 46 files which had the queried SNP, the obtained proportion of users which tested positive for “AG” genotype of the specified allele was 7/23, which is approximately 30.4%. Although the population frequency of this allele has yet to be studied thoroughly, a study on its frequency in an Algerian population of 755 people reported a frequency of 19.8% [ref](https://doi.org/10.1186/1476-511X-12-155), which is a mere 10.6% difference from what we have determined using 46 files, taking into consideration that 46 files of people from different populations obtained from OpenSNP is not a very accurate determination of population frequency. Since the results of the negative control test are reasonable, it is clear that our system is not only able to fulfill its function, but can even be used to estimate the frequency of a SNP given a pool of respondents.

We also recorded down the average time required to run different parts of the code, and the results are summarized in the table below:


Time taken to                                                                	| seconds
---------------------------------------------------------------------------------|---------
Run through the entire system with 73 files                                  	| 843.83   
Read 1 VCF file, check identity and generate a block                         	| 11.24   
Generate Paillier Key Pair, generate random number and encrypt the random number | 0.98    
Decrypt responses and obtain response proportion                             	| 0.03    
Generate genesis block                                                       	| 0.01    
Check and verify hashes                                                      	| 0.01    


###**9. Future work**
This technology can potentially be used in clinics and hospitals in the near future to confirm or even identify the SNPs that are responsible for a certain phenotype for efficient treatment. Additionally, we envision a society where people with genetic diseases will no longer solely rely on doctors and scientists to research on genetic diseases, but will also take up the initiative to, individually or in small groups, start blockchains of their own and conduct the necessary research. We will also develop an API (Application Programming Interface) so that people who use our platform to conduct analysis of their genomes do not need to obtain the code and change the necessary parameters before running it, but rather, can instead instruct a bot to do it for them, making it even more accessible and easy to understand and use.

###**10. Acknowledgement**
We would like to thank Winston Koh for his invaluable guidance and mentorship. Thanks also goes to Shawn Hoon of A*STAR MEL and our teacher mentor Mr Ng Chee Loong of NUS High School for providing insightful counsel and assistance. 

###**11. References**
Roberts, N. D., Kortschak, R. D., Parker, W. T., Schreiber, A. W., Branford, S., Scott, H. S., … Adelson, D. L. (2013). A comparative analysis of algorithms for somatic SNV detection in cancer. Bioinformatics. https://doi.org/10.1093/bioinformatics/btt375

Kim, M., & Lauter, K. (2015). Private genome analysis through homomorphic encryption. BMC Medical Informatics and Decision Making, 15(5), S3. https://doi.org/10.1186/1472-6947-15-S5-S3

Paillier, P. (1999). Public-key cryptosystems based on composite degree residuosity classes. Lecture Notes in Computer Science (Including Subseries Lecture Notes in Artificial Intelligence and Lecture Notes in Bioinformatics), 1592, 223–238. https://doi.org/10.1007/3-540-48910-X_16

Bertram, L., Lange, C., Mullin, K., Parkinson, M., Hsiao, M., Hogan, M. F., … Tanzi, R. E. (2008). Genome-wide Association Analysis Reveals Putative Alzheimer’s Disease Susceptibility Loci in Addition to APOE. American Journal of Human Genetics, 83(5), 623–632. https://doi.org/10.1016/j.ajhg.2008.10.008

Li, H., Wetten, S., Li, L., & al, et. (2008). Candidate single-nucleotide polymorphisms from a genomewide association study of alzheimer disease. Archives of Neurology, 65(1), 45–53. Retrieved from http://dx.doi.org/10.1001/archneurol.2007.3

Boulenouar, H., Benchekor, S. M., Meroufel, D. N., Hetraf, S. A. L., Djellouli, H. O., Hermant, X., … Goumidi, L. (2013). Impact of APOE gene polymorphisms on the lipid profile in an Algerian population. Lipids in Health and Disease, 12(1), 1–8. https://doi.org/10.1186/1476-511X-12-155

Shirkani, A., Mansouri, A., Abbaszadegan, M. R., Faridhosseini, R., Jabbari Azad, F., & Gholamin, M. (2017). GATA3 Gene Polymorphisms Associated with Allergic Rhinitis in an Iranian Population. Reports of Biochemistry & Molecular Biology, 5(2), 97–102. Retrieved from http://www.ncbi.nlm.nih.gov/pubmed/28367470%0Ahttp://www.pubmedcentral.nih.gov/articlerender.fcgi?artid=PMC5346276

VariantAnnotation @ www.rdocumentation.org. (n.d.). Retrieved from https://www.rdocumentation.org/packages/VariantAnnotation

tidyverse @ www.rdocumentation.org. (n.d.). Retrieved from https://www.rdocumentation.org/packages/tidyverse

hadley @ github.com. (n.d.). Retrieved from https://github.com/hadley

digest @ www.rdocumentation.org. (n.d.). Retrieved from https://www.rdocumentation.org/packages/digest

eddelbuettel @ github.com. (n.d.). Retrieved from https://github.com/eddelbuettel

homomorpheR @ www.rdocumentation.org. (n.d.). Retrieved from https://www.rdocumentation.org/packages/homomorpheR

homomorpheR @ github.com. (n.d.). Retrieved from https://github.com/cran/homomorpheR

Kawahara-Miki R, Tsuda K, Shiwa Y, et al. Whole-genome resequencing shows numerous genes with nonsynonymous SNPs in the Japanese native cattle Kuchinoshima-Ushi. BMC Genomics. 2011;12:103. Published 2011 Feb 10. doi:10.1186/1471-2164-12-103
https://www.ebi.ac.uk/training/online/course/human-genetic-variation-i-introduction/variant-identification-and-analysis/what-variant
