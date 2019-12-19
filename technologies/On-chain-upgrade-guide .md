## On-chain governance upgrade guide

### 1 Introduction

During the operation of the PlatON test network, due to system abnormalities and increased functions, all nodes are required to follow the on-chain governance process to upgrade local nodes, and candidate nodes must vote.

- Each candidate node can only vote once for each proposal.
- Candidate nodes must participate in the voting, otherwise they will be downgraded to candidates for candidate nodes and cannot participate in consensus to obtain block rewards or Staking rewards.
- If the missed voting period is reduced to a candidate node candidate, you can initiate a version declaration transaction after upgrading to the specified version locally to become a candidate node again.

### 2 Get upgrade proposal information

Nodes need to pay attention to the upgrade proposal information in time to ensure that they can be upgraded in a timely manner without affecting the normal participation in consensus. Information on upgrade proposals can be obtained through community announcements and on-chain inquiries. In addition, if the download script fails, set the DNS server to 8.8.8.8.

#### 2.1 Follow community announcements

You can get the information of the upgrade proposal in time by following the announcement of the community. When a node upgrade is required to vote, the community will release the relevant [Announcement] (../README.md). The announcement is as follows:

```bash
Proposal details:
PIPID: 100
Proposal address: https://platscan.platon.network/proposal-detail?proposalHash=0xad330d8a5fddf3526a8622dab22454f8861fee968b6482eebbd360c8d15691c3

ProposalID: 0xad330d8a5fddf3526a8622dab22454f8861fee968b6482eebbd360c8d15691c3

Target version number: 0.9.0
Voting cycle: starting block height 533681, ending block height 619980

Address of the upgrade code on the chain:
Code branch: https://github.com/PlatONnetwork/PlatON-Go/tree/pip_v0.7.3
commit ID: d2b7bf51a68158bfa61d622ffde3841c55634a18
```

#### 2.2 Query directly from the chain

The upgrade proposal can also be queried directly in the blockchain browser [PlatScan](https://platscan.platon.network/proposal).

### 3 Upgrade Operation Process

#### 3.1 Upgrade Node to Specified Version

Deploy and install according to the upgrade version number (assuming 0.9.0) obtained from the community announcement or blockchain browser. Proceed as follows:

Operate on the machine where the node is deployed:

- Download script

 ```bash
wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/update_platon.sh
 ```

  or

 ```bash
wget http://47.91.153.183/opensource/scripts/update_platon.sh
 ```

- Execute command to update version

 ```bash
chmod u + x update_platon.sh && ./update_platon.sh 0.9.0
 ```

Note: 0.9.0 is the version number of the specified upgrade. When **[sudo] password for platon: ** is similar to the prompt, you need to enter the password of the current user; when **Do you want to continue?** When prompted, enter: **y**; the execution result indicates that the version upgrade was successful:

 ```bash
Currently installed version: 0.8.0 ===========
Start installation: 0.9.0 version ===========
Node suspended successfully ===========
Do you want to continue? [Y/n] y
Uninstall the current version: platon0.8.0 success ===========
Installation version: platon0.9.0 success ===========
Restart node successfully =============
 ```

  > Note:
  >
  > No version upgrade:
  >
  >- Specified upgrade version does not exist
  >- Specify that the version to be upgraded is not higher than the installed version
  >
  > Nodes that have not been version upgraded run with the previous version.

#### 3.2 Voting

##### 3.2.1 Get the current block height

```bash
cd ~/platon-node/data && platon attach ipc: platon.ipc -exec platon.blockNumber
```

Check to see if the current block height is within the voting period published by the community. Link address reference: [2.1 Follow community governance related announcements](#21-Follow community governance related announcements). The community ’s voting period is [533681, 619980], if the current block height is 600,000, it is within the voting period; if the current block height is greater than 619980, it indicates that the voting period has passed.

##### 3.2.2 Vote on Upgrade Proposal

- If during the voting period, vote for the upgrade proposal, otherwise skip this section.

 ```bash
  mtool-client.bat vote_versionproposal --proposalid proposalid --keystore %MTOOLDIR%\keystore\staking.json --config %MTOOLDIR%\validator\validator_config.json
 ```

Note: ProposalID is the ProposalID of the upgrade proposal provided by the community announcement, and the link address is referenced: [2.1 Follow community governance related announcements](#21-Follow community governance related announcements). If the voting transaction is sent successfully, **voting transaction hash** will be returned. Such as:

 ```bash
operation finished
transaction hash: 0x344d5c916a070453567fe95c6b79cd86bb70c248f96da98fb0ec3aa6617cc9a3
SUCCESS
 ```

The above indicates that the transaction is successful; if other prompts appear, it means that the voting for the upgrade proposal has failed.

- Verify the success of the vote

  Execute the commands in sequence on the machine where the node is deployed:

 ```bash
wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/verify_transaction.sh
 ```

  or

 ```bash
wget http://47.91.153.183/opensource/scripts/verify_transaction.sh
 ```

  Execute the script:

 ```bash
chmod u + x verify_transaction.sh && ./verify_transaction.sh 0x344d5c916a070453567fe95c6b79cd86bb70c248f96da98fb0ec3aa6617cc9a3
 ```

Note: Among them, 0x344d5c916a070453567fe95c6b79cd86bb70c248f96da98fb0ec3aa6617cc9a3 is the transaction hash returned by the voting operation for the upgrade proposal. Please modify it according to the actual return value. If the success message is displayed, the upgrade proposal vote was successful, as shown below; otherwise, the upgrade proposal vote failed.

 ```bash
Get transaction receipt success ===========
Parsing transaction receipts ===========
Successful transaction! !! !!
 ```

##### 3.2.3 Release Statement

- If the voting period has passed and the voting operation has not been completed, a version declaration operation is required:

 ```bash
mtool-client.bat declare_version --keystore %MTOOLDIR%\keystore\staking.json --config %MTOOLDIR%\validator\validator_config.json
 ```

  Note: If the version statement transaction is successfully sent, the **version statement transaction hash** will be returned. Such as:

 ```bash
operation finished
transaction hash: 0x776d5be7363451540b7113771cf4263de6a18973ed8904796a561acf37e58ff2
SUCCESS
 ```

The above indicates that the transaction was successful; if other prompts appear, the version declaration failed.

- Verify the success of the version declaration

  Execute the commands in sequence on the machine where the node is deployed: (If you have downloaded the verify_transaction.sh script, you do not need to execute the first command)

 ```bash
wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/verify_transaction.sh
 ```

  or

 ```bash
wget http://47.91.153.183/opensource/scripts/verify_transaction.sh
 ```

  Execute the script:

 ```bash
chmod u + x verify_transaction.sh && ./verify_transaction.sh 0x776d5be7363451540b7113771cf4263de6a18973ed8904796a561acf37e58ff2
 ```

 Note: Among them, 0x776d5be7363451540b7113771cf4263de6a18973ed8904796a561acf37e58ff2 is the transaction hash returned by the version declaration operation. Please modify it according to the actual return value. If the success message is displayed, the version declaration is successful, as shown below; otherwise, the version declaration fails.

 ```bash
Get transaction receipt success ===========
Parsing transaction receipts ===========
Successful transaction! !! !!
 ```

### 4 Verify node upgrade results

Operate on the machine where the node is deployed:

- Download verification script

 ```bash
wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/verify_votResult.sh
 ```

  or

 ```bash
wget http://47.91.153.183/opensource/scripts/verify_votResult.sh
 ```

- Download rlp tools

  In the same directory as the verification script, execute this command:

 ```bash
wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/ppos_tool
 ```

  or

 ```bash
wget http://47.91.153.183/opensource/scripts/ppos_tool
 ```

- Execute verification script

  After the 1000th block of the voting cut-off block height (the number of blocks of the four consensus rounds, assuming the cut-off block height is 619980, then the 620980 block), execute the following script to check the upgrade result:

 ```bash
chmod u + x verify_votResult.sh && chmod u + x ppos_tool && ./verify_votResult.sh 0x85083f1d4f3e5e1aeafbd29df1db9764c25216e94fa0b446caa36f89ffa8f26f
 ```

  > Note:
  >
  >- Among them, 0x85083f1d4f3e5e1aeafbd29df1db9764c25216e94fa0b446caa36f89ffa8f26f is **the ProposalID of the upgrade proposal**. Please modify the value according to the actual announcement, and refer to the link address: [2.1 Follow community governance related announcements](#21-Follow community governance related announcements).

The following results indicate that the upgrade was successful:

 ```bash
Get Proposal Success ===========
Proposal effective block height is: 620980
The current block height is: 621010
The block height has reached the effective block height of the proposal, and the verification has begun ===========
Start verifying the voting results of the upgrade proposal ===========
Verification of the voting result of the upgrade proposal is successful ===========
Begin to verify proposal version number == binary version number == effective version number on the chain ===========
Upgrade proposal version verified successfully ===============
Begin to verify the pledge status of the node ==============
Successfully obtained node pledge information ===========
The node did not exit the validator list, the upgrade version was successful, and the current chain effective version is: 2304
 ```

  > Note:
  >
  > The verification script does the following checks:
  >
  >- Whether the proposal is approved
  >
  >- Does the current node version number match the proposal version number?
  >
  >- The pledge status of the current node, if not 0, exit the candidate node candidate