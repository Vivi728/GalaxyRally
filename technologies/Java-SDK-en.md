# getting Started

Depending on the build tool, add related dependencies to your project using:

-Use requires jdk1.8 or above.

## maven

> Project configuration:

```
<repository>
<id> platon-public</ id>
<url> https://sdk.platon.network/nexus/content/groups/public/</ url>
</ repository>
```

maven reference way:

```
<dependency>
      <groupId> com.platon.client</ groupId>
      <artifactId> core</ artifactId>
      <version> 0.7.3.4</ version>
</ dependency>
```

## gradle

> Project configuration:

```
repositories {
     maven {url "https://sdk.platon.network/nexus/content/groups/public/"}
}
```

gradle way of reference:

```
compile "com.platon.client: core: 0.7.3.4"
```

# Use API

Encapsulates some APIs for developers to use, including the following two parts:

-System contracts: including contract interfaces related to economic models and governance
-Basic API: including network, transaction, query, node information, economic model parameter configuration and other related interfaces

## System contract

### Pledge related interface

> Interfaces related to pledge contracts in the PlatON economic model

#### Loading pledge contract

```java
// Java 8
Web3j web3j = Web3j.build(new HttpService("http: // localhost: 6789"));
String chainId = "100"
Credentials credentials = WalletUtils.loadCredentials("password", "/ path / to / walletfile");
StakingContract contract = StakingContract.load(web3j, credentials, chainId)
```

#### Interface Description

#####**StakingReturnTransaction**

> Node candidate applies for pledge

- **Introduction**
  -String: nodeId node id, hexadecimal format, starting with 0x
  -BigInteger: amount of von pledged, the pledged amount must be greater than or equal to 1,000,000 LAT
  -StakingAmountType: stakingAmountType, enumeration, FREE_AMOUNT_TYPE means use the free amount of the account, RESTRICTING_AMOUNT_TYPE means use the amount of the lock to make a pledge
  -String: benefitAddress revenue account
  -String: nodeName The name of the node being pledged
  -String: externalId External Id(the length of the Id described by the third-party pull node), currently the keybase account public key
  -String: The third-party homepage of the webSite node(the length is limited, indicating the homepage of the node)
  -String: description of the details node(there is a length limitation, indicating the description of the node)
  -ProgramVersion: the real version of the processVersion program, governing rpc acquisition
  -String: blsPubKey bls public key
  -String: Proof of blsProof bls
- **return value**

```
BaseRespons
```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
// Indicates the free amount of the account
StakingAmountType stakingAmountType = StakingAmountType.FREE_AMOUNT_TYPE;
// 20bytes income account for receiving block rewards and pledged rewards
    String benifitAddress = "0xXXXXXXXXX";
    // The external Id(the length is limited to the ID described by the third-party pull node) is currently the user keybase account public key
    String externalId = "";
    // Custom node name
        String nodeName = "integration-node1";
        // The third-party homepage of the node(there is a length limitation, indicating the homepage of the node)
        String webSite = "https://www.platon.network/#/";
        // Description of the node(there is a length limit, which means the description of the node)
        String details = "integration-node1-details";
        // Pledged von
        BigDecimal stakingAmount = Convert.toVon("1000000", Unit.LAT) .add(BigDecimal.valueOf(1L));
    Ranch
        PlatonSendTransaction platonSendTransaction = stakingContract.stakingReturnTransaction(new StakingParam.Builder()
                    .setNodeId(nodeId)
                    .setAmount(new BigInteger(stakingAmount))
                    .setStakingAmountType(stakingAmountType)
                    .setBenifitAddress(benifitAddress)
                    .setExternalId(externalId)
                    .setNodeName(nodeName)
                    .setWebSite(webSite)
                    .setDetails(details)
                    .setBlsPubKey(blsPubKey)
                    .setProcessVersion(stakingContract.getProgramVersion())
                    .setBlsProof(stakingContract.getAdminSchnorrNIZKProve())
                    .build()). send();
        BaseResponse baseResponse = stakingContract.getStakingResult(platonSendTransaction) .send();
        return baseResponse;
```

#####**UnStakingReturnTransaction**

> Node revocation pledge(initiate all revocations at one time, multiple accounts)

- **Introduction**
  -String: nodeId node id, hexadecimal format, starting with 0x
- **return value**

```
BaseRespons
```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
// node id
String nodeId = "0x6bad331aa2ec6096b2b6034570e1761d687575b38c3afc3a3b5f892dac4c86d0fc59ead0f0933ae041c0b6b43a7261f1529bad5189be4fba343875548dc9efd3;

// Call interface
PlatonSendTransaction platonSendTransaction = stakingContract.unStakingReturnTransaction(nodeId) .send();

BaseResponse baseResponse = stakingContract.getUnStakingResult(platonSendTransaction) .send();
```

#####**UpdateStakingInfoReturnTransaction**

> Modify pledge information

- **Introduction**
  -String: nodeId node id, hexadecimal format, starting with 0x
  -String: externalId External Id(with a length limit, the ID described by the third-party pull node), currently the keybase account public key
  -String: benefitAddress revenue account
  -String: nodeName The name of the node being pledged
  -String: the third-party homepage of the webSite node
  -String: description of the details node(there is a length limitation, indicating the description of the node)
- **return value**

```
BaseRespons
```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
// Modified revenue address
String benifitAddress = benefitCredentials.getAddress();
// The external Id(the length is limited to the ID described by the third-party pull node) is currently the user keybase account public key
    String externalId = "";
    // The name of the pledged node
        String nodeName = "integration-node1-u";
        // Node's third-party homepage
        String webSite = "https://www.platon.network/#/";
        // Description of the node
        String details = "integration-node1-details-u";
    
PlatonSendTransaction platonSendTransaction = stakingContract.updateStakingInfoReturnTransaction(new UpdateStakingParam.Builder()
        .setBenifitAddress(benifitAddress)
        .setExternalId(externalId)
        .setNodeId(nodeId)
        .setNodeName(nodeName)
        .setWebSite(webSite)
        .setDetails(details)
        .build()). send();

BaseResponse baseResponse = stakingContract.getUpdateStakingInfoResult(platonSendTransaction) .send();
```

#####**AddStakingReturnTransaction**

> Increase pledge and increase pledged deposits of pledged nodes

- **Introduction**
  -String: nodeId node id, hexadecimal format, starting with 0x
  -StakingAmountType: stakingAmountType, enumeration, FREE_AMOUNT_TYPE means use the free amount of the account, RESTRICTING_AMOUNT_TYPE means use the amount of the lock to make a pledge
  -BigInteger: addStakingAmount
- **return value**

```
BaseRespons
```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
// node id
String nodeId = "0x6bad331aa2ec6096b2b6034570e1761d687575b38c3afc3a3b5f892dac4c86d0fc59ead0f0933ae041c0b6b43a7261f1529bad5189be4fba343875548dc9efd3;
// Indicate whether to use the free amount of the account or the locked amount of the account for pledge
StakingAmountType stakingAmountType = StakingAmountType.FREE_AMOUNT_TYPE;
        BigDecimal addStakingAmount = Convert.toVon("4000000", Unit.LAT)
// Interface call
        PlatonSendTransaction platonSendTransaction = stakingContract.addStakingReturnTransaction(nodeId, StakingAmountType.FREE_AMOUNT_TYPE, addStakingAmount.toBigInteger()). Send();
BaseResponse baseResponse = stakingContract.getAddStakingResult(platonSendTransaction) .send();
```

#####**GetStakingInfo**

> Query the pledge information of the current node

- **Introduction**
  -String: nodeId node id, hexadecimal format, starting with 0x
- **return value**

```
BaseResponse<Node> baseRespons
```

-BaseResponse<Node> description
  -int: Code result flag, 1 is success, 0 is failure
  -Node: Data Node object data
  -String: ErrMsg error message, exists on failure

- **Node**: object that holds the current node pledge information

  -String: BenefitAddress is used to accept the block reward and pledged reward income account

  -String: The description of the Details node(the length is limited, indicating the description of the node)

  -String: NodeId The node Id of the pledge(also called the candidate's node Id)

  -String: NodeName The name of the node being pledged(the length is limited, indicating the name of the node)

  -BigInteger: ProgramVersion The real version number of the PlatON process of the pledged node(the interface for obtaining the version number is provided by the governance)

  -BigInteger: Released von who initiated a free amount locked period pledged account

  -BigInteger: ReleasedHes initiated the free amount of the hesitation period of the pledged account

  -BigInteger: RestrictingPlan initiates the lock-up period of the locked account amount of the pledged account.

  -BigInteger: RestrictingPlanHes initiated the hedging period of the locked amount of the pledged account

  -BigInteger: Shares the current candidate's total pledge plus the number of entrusted vons

  -String: StakingAddress The account used when initiating the pledge(when the pledge is cancelled, von will be returned to the account or the account's lock information)

  -BigInteger: block height when StakingBlockNum initiated pledge

  -BigInteger: StakingEpoch's current settlement cycle when the pledge amount is changed

  -BigInteger: StakingTxIndex transaction index when pledge is initiated

  -BigInteger: Status of the status candidate, 0: node is available, 1: node is unavailable, 2: node block rate is low but the removal condition is not met,

    4: The node's von is insufficient to the minimum pledge threshold(only the penultimate bit is 1), 8: the node is reported to be double signed, 16: the node block rate is low and the removal condition is reached(the penultimate bit is 1); : Node initiates cancellation

  -BigInteger: ValidatorTerm

  -String: The third-party homepage of the Website node(the length of the node is the homepage of the node)

- **Java SDK contract use**

```java
// node id
String nodeId = "0x6bad331aa2ec6096b2b6034570e1761d687575b38c3afc3a3b5f892dac4c86d0fc59ead0f0933ae041c0b6b43a7261f1529bad5189be4fba343875548dc9efd3;
BaseResponse<Node> baseResponse = stakingContract.getStakingInfo(nodeId) .send();
Node node = baseResponse.data;
```

### Delegation related interface

> Principal related contract interface in PlatON economic model

#### Load delegate contract

```java
// Java 8
Web3j web3j = Web3j.build(new HttpService("http: // localhost: 6789"));
String chainId = "100"
Credentials credentials = WalletUtils.loadCredentials("password", "/ path / to / walletfile");
delegateContract contract = DelegateContract.load(web3j, credentials, chainId);
```

#### Interface Description

#####**DelegateReturnTransaction**

> Initiate a commission, commission a node that has been pledged, and commission a node to increase the weight of the node to obtain revenue

- **Introduction**
  -String: nodeId node id, hexadecimal format, starting with 0x
  -StakingAmountType: stakingAmountType, enumeration, FREE_AMOUNT_TYPE means use the free amount of the account, RESTRICTING_AMOUNT_TYPE means use the amount of the lock to make a pledge
  -BigInteger: amount of amount commissioned(based on the smallest unit, 1LAT = 10**18 von)
- **return value**

```
BaseRespons
```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
// node id
String nodeId = "0x6bad331aa2ec6096b2b6034570e1761d687575b38c3afc3a3b5f892dac4c86d0fc59ead0f0933ae041c0b6b43a7261f1529bad5189be4fba343875548dc9efd3;
 // Indicate whether to use the free amount of the account or the locked amount of the account for pledge,
StakingAmountType stakingAmountType = StakingAmountType.FREE_AMOUNT_TYPE;
// Entrusted amount(calculated according to the smallest unit, 1LAT = 10**18 von)
        BigDecimal amount = Convert.toVon("500000", Unit.LAT);
    Ranch
        PlatonSendTransaction platonSendTransaction = delegateContract.delegateReturnTransaction(nodeId, stakingAmountType, amount.toBigInteger()). Send();
        BaseResponse baseResponse = delegateContract.getDelegateResult(platonSendTransaction) .send();
```

#####**GetRelatedListByDelAddr**

> Query the NodeID and Pledged Id of the node entrusted by the current account address

- **Introduction**
  -String: address Account address of the principal
- **return value**

```
BaseResponse<List<DelegationIdInfo >> baseRespons
```

-BaseResponse<List<DelegationIdInfo >>
  -int: Code result identification, 1 is success, 0 is failure
  -List<DelegationIdInfo>: List of Data DelegationIdInfo objects
  -String: ErrMsg error message, exists on failure

- **DelegationIdInfo**: An object that stores the NodeID and the height of the pledged block of the node commissioned by the current account address
  -String: address Account address of the principal
  -String: NodeId Node Id of the validator
  -BigInteger: block height when StakingBlockNum initiated pledge
- **Java SDK contract use**

```java
BaseResponse<List<DelegationIdInfo >> baseResponse = delegateContract.getRelatedListByDelAddr(delegateCredentials.getAddress()). Send();
List<DelegationIdInfo> DelegationIdInfoList = baseResponse.data;

```

#####**GetDelegateInfo**

> Query current single commission information

- **Introduction**
  -String: address Account address of the principal
  -String: nodeId node id, in hexadecimal format, starting with 0x
  -BigInteger: block height when stakingBlockNum initiated pledge
- **return value**

```
BaseResponse<Delegation>

```

-BaseResponse<Delegation>
  -int: Code result identification, 1 is success, 0 is failure
  -Delegation: Data delegation object data
  -String: ErrMsg error message, exists on failure

- **Delegation**: the object to save the delegation information of the current delegation account
  -String: Address The account address of the principal
  -String: NodeId Node Id of the validator
  -BigInteger: block height when StakingBlockNum initiated pledge
  -BigInteger: DelegateEpoch's settlement cycle at the time of the most recent delegation to this candidate
  -BigInteger: Released to initiate a free amount lock-in period of the commissioned account
  -BigInteger: ReleasedHes initiated the free amount of the hesitation period commissioned by the commissioned account von
  -BigInteger: RestrictingPlan initiates a lock-in period of the entrusted account
  -BigInteger: RestrictingPlanHes initiated the hedging period of the locked account of the entrusted account
  -BigInteger: Reduction von in revocation plan
- **Java SDK contract use**

```java
// node id
String nodeId = "4fcc251cf6bf3ea53a748971a223f5676225ee4380b65c7889a2b491e1551d45fe9fcc19c6af54dcf0d5323b5aa8ee1d919791695082bae1f86dd282dba4150f";
// Delegation address
String address = "0xc1f330b214668beac2e6418dd651b09c759a4bf5";
// Pledged block height
BigInteger stakingNumber = new BigInteger("10888");
BaseResponse<Delegation> baseResponse = delegateContract.getDelegateInfo(nodeId, address, stakingBlockNum) .send();
Delegation delegation = baseResponse.data;
```

#####**UnDelegateReturnTransaction**

> Reduction / revocation of commission(all reductions are revocation)

- **Introduction**
  -String: nodeId node id, hexadecimal format, starting with 0x
  -BigInteger: The stakingBlockNum entrusted node has a high pledge block, which represents a unique sign of a pledge of a node
  -BigInteger: the commission amount of stakingAmount reduction(based on the smallest unit, 1LAT = 10**18 von)
- **return value**

```
BaseRespons
```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
// node id
String nodeId = "0x6bad331aa2ec6096b2b6034570e1761d687575b38c3afc3a3b5f892dac4c86d0fc59ead0f0933ae041c0b6b43a7261f1529bad5189be4fba343875548dc9efd3;
// The amount of reduction
        BigDecimal stakingAmount = Convert.toVon("500000", Unit.LAT);
        // The pledge block of the entrusted node is high, which represents the unique sign of a pledge of a certain node
        BigInteger stakingBlockNum = new BigInteger("12134");
        PlatonSendTransaction platonSendTransaction = delegateContract.unDelegateReturnTransaction(nodeId, stakingBlockNum, stakingAmount.toBigInteger()). Send();
BaseResponse baseResponse = delegateContract.getUnDelegateResult(platonSendTransaction) .send();
```

### Node-related contracts

> Principal related contract interface in PlatON economic model

#### Load node contract

```java
// Java 8
Web3j web3j = Web3j.build(new HttpService("http: // localhost: 6789"));
String chainId = "100"
Credentials credentials = WalletUtils.loadCredentials("password", "/ path / to / walletfile");
nodeContract contract = NodeContract.load(web3j, credentials, chainId);
```

#### Interface Description

#####**GetVerifierList**

> Query the queue of validators in the current settlement cycle

- **Introduction**

  no

- **return value**

```
BaseResponse<List<Node >> baseResponse
```

-BaseResponse<List<Node >>
  -int: Code result identification, 1 is success, 0 is failure
  -List<Node>: Data nodeList object data
  -String: ErrMsg error message, exists on failure

- **Node**: object for saving node information for a single current settlement cycle

  -String: BenefitAddress is used to accept the block reward and pledged reward income account

  -String: The description of the Details node(the length is limited, indicating the description of the node)

  -String: NodeId The node Id of the pledge(also called the candidate's node Id)

  -String: NodeName The name of the node being pledged(the length is limited, indicating the name of the node)

  -BigInteger: ProgramVersion The real version number of the PlatON process of the pledged node(the interface for obtaining the version number is provided by the governance)

  -BigInteger: Released von who initiated a free amount locked period pledged account

  -BigInteger: ReleasedHes initiated the free amount of the hesitation period of the pledged account

  -BigInteger: RestrictingPlan initiates the lock-up period of the locked account amount of the pledged account.

  -BigInteger: RestrictingPlanHes initiated the hedging period of the locked amount of the pledged account

  -BigInteger: Shares the current candidate's total pledge plus the number of entrusted vons

  -String: StakingAddress The account used when initiating the pledge(when the pledge is cancelled, von will be returned to the account or the account's lock information)

  -BigInteger: block height when StakingBlockNum initiated pledge

  -BigInteger: StakingEpoch's current settlement cycle when the pledge amount is changed

  -BigInteger: StakingTxIndex transaction index when pledge is initiated

  -BigInteger: Status of the status candidate, 0: node is available, 1: node is unavailable, 2: node block rate is low but the removal condition is not met,

    4: The node's von is insufficient to the minimum pledge threshold(only the penultimate bit is 1), 8: the node is reported to be double signed, 16: the node block rate is low and the removal condition is reached(the penultimate bit is 1); : Node initiates cancellation

  -BigInteger: ValidatorTerm

  -String: The third-party homepage of the Website node(the length of the node is the homepage of the node)

- **Java SDK contract use**

```java
BaseResponse<List<Node >> baseResponse = nodeContract.getVerifierList(). Send();
List<Node> nodeList = baseResponse.data;

```

#####**GetValidatorList**

> Query the list of validators in the current consensus cycle

- **Introduction**

  no

- **return value**

```
BaseResponse<List<Node >> baseResponse

```

-BaseResponse<List<Node >>
  -int: Code result identification, 1 is success, 0 is failure
  -List<Node>: Data nodeList object data
  -String: ErrMsg error message, exists on failure

- **Node**: object that saves the information of a single current consensus cycle verification node

  -String: BenefitAddress is used to accept the block reward and pledged reward income account

  -String: The description of the Details node(the length is limited, indicating the description of the node)

  -String: NodeId The node Id of the pledge(also called the candidate's node Id)

  -String: NodeName The name of the node being pledged(the length is limited, indicating the name of the node)

  -BigInteger: ProgramVersion The real version number of the PlatON process of the pledged node(the interface for obtaining the version number is provided by the governance)

  -BigInteger: Released von who initiated a free amount locked period pledged account

  -BigInteger: ReleasedHes initiated the free amount of the hesitation period of the pledged account

  -BigInteger: RestrictingPlan initiates the lock-up period of the locked account amount of the pledged account.

  -BigInteger: RestrictingPlanHes initiated the hedging period of the locked amount of the pledged account

  -BigInteger: Shares the current candidate's total pledge plus the number of entrusted vons

  -String: StakingAddress The account used when initiating the pledge(when the pledge is cancelled, von will be returned to the account or the account's lock information)

  -BigInteger: block height when StakingBlockNum initiated pledge

  -BigInteger: StakingEpoch's current settlement cycle when the pledge amount is changed

  -BigInteger: StakingTxIndex transaction index when pledge is initiated

  -BigInteger: Status of the status candidate, 0: node is available, 1: node is unavailable, 2: node block rate is low but the removal condition is not met, 4: The node's von is less than the minimum pledge threshold(only the penultimate bit is 1),8: The node is reported with double sign, 16: The node's block generation rate is low and the removal condition is reached(the penultimate bit is 1); 32: The node actively initiates the cancellation

  -BigInteger: ValidatorTerm

  -String: The third-party homepage of the Website node(the length of the node is the homepage of the node)

- **Java SDK contract use**

```java
BaseResponse<List<Node >> baseResponse = nodeContract.getValidatorList(). Send();
List<Node> nodeList = baseResponse.data;
```

#####**GetCandidateList**

> Query all real-time candidate lists

- **Introduction**

  no

- **return value**

```
BaseResponse<List<Node >> baseResponse
```

-BaseResponse<List<Node >>
  -int: Code result identification, 1 is success, 0 is failure
  -List<Node>: Data nodeList object data
  -String: ErrMsg error message, exists on failure

- **Node**: holds a single candidate node information object

  -String: BenefitAddress is used to accept the block reward and pledged reward income account

  -String: The description of the Details node(the length is limited, indicating the description of the node)

  -String: NodeId The node Id of the pledge(also called the candidate's node Id)

  -String: NodeName The name of the node being pledged(the length is limited, indicating the name of the node)

  -BigInteger: ProgramVersion The real version number of the PlatON process of the pledged node(the interface for obtaining the version number is provided by the governance)

  -BigInteger: Released von who initiated a free amount locked period pledged account

  -BigInteger: ReleasedHes initiated the free amount of the hesitation period of the pledged account

  -BigInteger: RestrictingPlan initiates the lock-up period of the locked account amount of the pledged account.

  -BigInteger: RestrictingPlanHes initiated the hedging period of the locked amount of the pledged account

  -BigInteger: Shares the current candidate's total pledge plus the number of entrusted vons

  -String: StakingAddress The account used when initiating the pledge(when the pledge is cancelled, von will be returned to the account or the account's lock information)

  -BigInteger: block height when StakingBlockNum initiated pledge

  -BigInteger: StakingEpoch's current settlement cycle when the pledge amount is changed

  -BigInteger: StakingTxIndex transaction index when pledge is initiated

  -BigInteger: Status of the status candidate, 0: node is available, 1: node is unavailable, 2: node block rate is low but the removal condition is not met,

    4: The node's von is insufficient to the minimum pledge threshold(only the penultimate bit is 1), 8: the node is reported to be double signed, 16: the node block rate is low and the removal condition is reached(the penultimate bit is 1); : Node initiates cancellation

  -BigInteger: ValidatorTerm

  -String: The third-party homepage of the Website node(the length of the node is the homepage of the node)

- **Java SDK contract use**

```java
BaseResponse<List<Node >> baseResponse = nodeContract.getCandidateList(). Send();
List<Node> nodeList = baseResponse.data;
```

### Governance related contracts

> Contract interface related to PlatON governance

#### Load governance contract

```java
// Java 8
Web3j web3j = Web3j.build(new HttpService("http: // localhost: 6789"));
String chainId = "100"
Credentials credentials = WalletUtils.loadCredentials("password", "/ path / to / walletfile");
ProposalContract contract = ProposalContract.load(web3j, credentials, chainId);

```

#### Interface Description

#####**SubmitProposalReturnTransaction**

> Submit a Proposal

- **Introduction**
  -Proposal: Submit proposals
- **Proposal Type Description**
  -TextProposal: 0x01, text proposal
  -VersionProposal: 0x02, upgrade proposal
  -CancelProposal: 0x04, cancel proposal
- **Proposal status definition**
  -For text proposals, there are three states: 0x01,0x02, 0x03;
  -For the upgrade proposal, there are four states: 0x01,0x03, 0x04, 0x05, 0x06.
  -For cancellation proposals, there are three states: 0x01,0x02, 0x03;
- **Statement**
  -Voting: 0x01, voting
  -Pass: 0x02, voted through
  -Failed: 0x03, voting failed
  -PreActive: 0x04,(upgrade proposal) pre-effective
  -Active: 0x05,(upgrade proposal) takes effect
  -Canceled: 0x06,(upgrade proposal) canceled
- **return value**

```
BaseRespons
```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
// Create proposal
Proposal proposal = Proposal.createSubmitTextProposalParam(proposalNodeId, String.valueOf(pid));
// Send text proposal transaction
PlatonSendTransaction platonSendTransaction = proposalContract.submitProposalReturnTransaction(proposal) .send();
// Query text proposal transaction results
BaseResponse baseResponse = proposalContract.getSubmitProposalResult(platonSendTransaction, FunctionType.SUBMIT_TEXT_FUNC_TYPE) .send();
```

#####**VoteReturnTransaction**

> Vote on proposals

- **Introduction**
  -ProgramVersion: the real version of the ProgramVersion program, managed by the rpc interface admin_getProgramVersion
  -VoteOption: voteOption voting type, YEAS in favor, NAYS against, ABSTENTIONS abstaining
  -String: proposalID proposal ID
  -String: verifier declared node, can only be validator / candidate
- **return value**

```
BaseRespons
```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**:

```java
// Node ID
String proposalID = "";
// declared node
String verifier = "";
PlatonSendTransaction platonSendTransaction = voteInfo.getVoteContract(). VoteReturnTransaction(proposalID, verifier, VoteOption.YEAS) .send();
BaseResponse baseResponse = voteInfo.getVoteContract(). GetVoteResult(platonSendTransaction) .send();

```

#####**GetProposal**

> Query Proposal

- **Introduction**
  -String: proposalID proposal id
- **return value**

```
BaseResponse<Proposal>

```

-BaseResponse<Proposal>
  -int: Code result identification, 1 is success, 0 is failure
  -Proposal: Data Proposal object data
  -String: ErrMsg error message, exists on failure

- **Proposal**: Objects that hold information about a single proposal
  -String: proposalId
  -String: proposer ID of the proposal node
  -int: proposalType proposal type, 0x01: text proposal; 0x02: upgrade proposal; 0x03 parameter proposal
  -String: piPid proposal PIPID
  -BigInteger: submitBlock
  -BigInteger: endVotingBlock block height
  -BigInteger: newVersion
  -BigInteger: ID of the promotion proposal to be canceled by the toBeCanceled proposal
  -BigInteger: activeBlock(if the vote passes) the effective block height(endVotingBlock + 20 + 4 * 250<effective block height<= endVotingBlock + 20 + 10 * 250)
  -String: verifier
- **Contract use**

```java
// Proposal id
String proposalID = "";
BaseResponse<Proposal> baseResponse = proposalContract.getProposal(proposalID) .send();
Proposal proposal = baseResponse.data;

```

#####**GetTallyResult**

> Query Proposal Results

- **Introduction**
  -String: proposalID proposal ID
- **return value**

```
BaseResponse<TallyResult>
```

-BaseResponse<TallyResult>
  -int: Code result identification, 1 is success, 0 is failure
  -TallyResult: Data TallyResult object data
  -String: ErrMsg error message, exists on failure

- **TallyResult**: Object that holds the results of a single proposal
  -String: proposalID
  -BigInteger: yeas votes
  -BigInteger: nays
  -BigInteger: abstentions
  -BigInteger: accuVerifiers Total number of validators who have qualified to vote throughout the voting period
  -int: status proposal status
- **Contract use**

```java
// Proposal id
String proposalID = "";
BaseResponse<TallyResult> baseResponse = proposalContract.getTallyResult(proposalID) .send();
TallyResult tallyResult = baseResponse.data;
```

#####**GetProposalList**

> Query proposal list

- **Introduction**

  no

- **return value**

```
BaseResponse<List<Proposal >>
```

-BaseResponse<List<Proposal >>
  -int: Code result identification, 1 is success, 0 is failure
  -List<Proposal>: Data ProposalList object data
  -String: ErrMsg error message, exists on failure

- **Proposal**: object for saving a single proposal
  -String: proposalId
  -String: proposer ID of the proposal node
  -int: proposalType proposal type, 0x01: text proposal; 0x02: upgrade proposal; 0x03 parameter proposal
  -String: piPid proposal PIPID
  -BigInteger: submitBlock
  -BigInteger: endVotingBlock block height
  -BigInteger: newVersion
  -String: ID of the promotion proposal to be canceled by the toBeCanceled proposal
  -BigInteger: activeBlock(if the vote passes) the effective block height(endVotingBlock + 20 + 4 * 250<effective block height<= endVotingBlock + 20 + 10 * 250)
  -String: verifier
- **Contract use**

```java
BaseResponse<List<Proposal >> baseResponse = proposalContract.getProposalList(). Send();
List<Proposal> proposalList = baseResponse.data;
```

#####**DeclareVersionReturnTransaction**

> Release statement

- **Introduction**
  -ProgramVersion: the real version of the ProgramVersion program, managed by the rpc interface admin_getProgramVersion
  -String: verifier declared node, can only be validator / candidate
- **return value**

```
BaseRespons

```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
String proposalNodeId = "";
PlatonSendTransaction platonSendTransaction = proposalContract.declareVersionReturnTransaction(proposalNodeId) .send();
BaseResponse baseResponse = proposalContract.getDeclareVersionResult(platonSendTransaction) .send();

```

#####**GetActiveVersion**

> Query node chain effective version

- **Introduction**

  no

- **return value**

```
BaseRespons

```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
BaseResponse baseResponse = proposalContract.getActiveVersion(). Send();
baseResponse.data;

```

### Double sign report related interface

> PlatON report contract related punishment interface

#### Load report contract

```
// Java 8
Web3j web3j = Web3j.build(new HttpService("http: // localhost: 6789"));
String chainId = "103"
Credentials credentials = WalletUtils.loadCredentials("password", "/ path / to / walletfile");
SlashContract contract = SlashContract.load(web3j, credentials, chainId);

```

#### Interface Description

#####**ReportDoubleSignReturnTransaction**

> Submit a Proposal

- **Introduction**
  -DuplicateSignType: DuplicateSignType enumeration, representing double sign types: prepareBlock, EprepareVote, viewChange
  -String: data json value of a single evidence, format refer to [RPC interface Evidences](# evidences_interface)
- **return value**

```
BaseRespons

```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
String data = ""; // Report evidence
PlatonSendTransaction platonSendTransaction = slashContract.reportDoubleSignReturnTransaction(DuplicateSignType.PREPARE_BLOCK, data) .send();
BaseResponse baseResponse = slashContract.getReportDoubleSignResult(platonSendTransaction) .send();

```

#####**CheckDoubleSign**

> Query whether a node has been reported as oversigned

- **Introduction**
  -DuplicateSignType: DuplicateSignType enumeration, representing double sign types: prepareBlock, EprepareVote, viewChange
  -String: address of the node reported by address
  -BigInteger: blockNumber multi-sign block height
- **return value**

```
BaseRespons

```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
BaseResponse baseResponse = slashContract.checkDoubleSign(DuplicateSignType.PREPARE_BLOCK, "0x4F8eb0B21eb8F16C80A9B7D728EA473b8676Cbb3", BigInteger.valueOf(500L)). Send();
String hash = baseResponse.data; // Reported transaction hash, may be zero transaction hash, ie: 0x000 ... 000

```

### Lock related interface

> PlatON report contract related punishment interface

#### Loading the hedging contract

```java
// Java 8
Web3j web3j = Web3j.build(new HttpService("http: // localhost: 6789"));
String chainId = "100"
Credentials credentials = WalletUtils.loadCredentials("password", "/ path / to / walletfile");
RestrictingPlanContract contract = RestrictingPlanContract.load(web3j, credentials, chainId);

```

#### Interface Description

#####**CreateRestrictingPlan**

> Create Lockup Plan

- **Introduction**
  -String: address lock position is released to the account
  -List<RestrictingPlan>: plan Locked plan list(array)
    -epoch: indicates a multiple of the settlement cycle. The product of the number of blocks produced per settlement cycle indicates the release of locked funds at the height of the target block. If account is the incentive pool address, the period value is a multiple of 120(that is, 30 * 4). In addition, period, the number of blocks per cycle must be at least greater than the highest irreversible block height.
    -amount: indicates the amount to be released on the target block.
- **return value**

```
BaseResponse

```

-BaseResponse: General Response Packet
  -int: Code result identification, 1 is success, 0 is failure
  -String: Data response data
  -String: ErrMsg error message, exists on failure

- **Contract use**

```java
List<RestrictingPlan> restrictingPlans = new ArrayList<>();
restrictingPlans.add(new RestrictingPlan(BigInteger.valueOf(100), new BigInteger("100000000000000000000")));
restrictingPlans.add(new RestrictingPlan(BigInteger.valueOf(200), new BigInteger("200000000000000000000")));
 PlatonSendTransaction platonSendTransaction = restrictingPlanContract.createRestrictingPlanReturnTransaction(restrictingRecvCredentials.getAddress(), restrictingPlans).send();
BaseResponse baseResponse = restrictingPlanContract.getCreateRestrictingPlanResult(platonSendTransaction) .send();

```

#####**GetRestrictingInfo**

> Get Locked Up Plan

- **Introduction**
  -String: address lock position is released to the account
- **return value**

```
BaseResponse<RestrictingItem> baseResponse
```

-BaseResponse<RestrictingItem> description
  -int: Code result identification, 1 is success, 0 is failure
  -RestrictingItem: Data RestrictingItem object data
  -String: ErrMsg error message, exists on failure

- **RestrictingItem**: save lock information object
  -BigInteger: balance
  -BigInteger: pledge pledge / mortgage amount
  -BigInteger: debt release amount due
  -List<RestrictingInfo>: info lock entry information
- **RestrictingInfo**: Object that saves information of a single lock entry
  -BigInteger: blockNumber releases block height
  -BigInteger: amount released
- **Contract use**

```java
BaseResponse<RestrictingItem> baseResponse = restrictingPlanContract.getRestrictingInfo(restrictingRecvCredentials.getAddress()). Send();
RestrictingItem restrictingItem = baseResponse.data;
```

## Basic API

### web3ClientVersion

> Returns the current client version

- **parameters**

  no

- **return value**

```java
Request<?, Web3ClientVersion>
```

The string in the Web3ClientVersion property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, Web3ClientVersion> request = currentValidWeb3j.web3ClientVersion();
String version = request.send(). GetWeb3ClientVersion();
```

### web3Sha3

> Keccak-256 returning the given data

- **parameters**

  String: The data before encryption

- **return value**

```java
Request<?, Web3Sha3>

```

The string in the Web3Sha3 attribute is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String date = "";
Request<?, Web3Sha3> request = currentValidWeb3j.web3Sha3(date);
String resDate = request.send(). GetWeb3ClientVersion();

```

### netVersion

> Returns the current network ID

- **parameters**

  no

- **return value**

```java
Request<?, NetVersion>

```

The string in the NetVersion attribute is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, NetVersion> request = currentValidWeb3j.netVersion();
String version = request.send(). GetNetVersion();

```

### netListening

Return true if the client is actively listening for a network connection

- **parameters**

  no

- **return value**

```java
Request<?, NetListening>

```

The boolean in the NetListening property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, NetListening> request = currentValidWeb3j.netListening();
boolean req = request.send(). isListening();

```

### netPeerCount

> Returns the number of peers currently connected to the client

- **parameters**

  None

- **return value**

```java
Request<?, NetPeerCount>

```

The BigInteger in the NetPeerCount property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, NetPeerCount> request = currentValidWeb3j.netPeerCount();
BigInteger req = request.send(). GetQuantity();

```

### platonProtocolVersion

> Returns the current platon protocol version

- **parameters**

  no

- **return value**

```java
Request<?, PlatonProtocolVersion>

```

The String in the PlatonProtocolVersion property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonProtocolVersion> request = currentValidWeb3j.platonProtocolVersion();
String req = request.send(). GetProtocolVersion();

```

### PlatonSyncing

Return an object containing data about the synchronization status or false

- **parameters**

  no

- **return value**

```java
Request<?, PlatonSyncing>

```

The String in the PlatonSyncing property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonSyncing> request = currentValidWeb3j.platonSyncing();
boolean req = request.send(). isSyncing();

```

### platonGasPrice

> Return current gas price

- **parameters**

  no

- **return value**

```java
Request<?, PlatonGasPrice>

```

The BigInteger in the PlatonGasPrice property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonGasPrice> request = currentValidWeb3j.platonGasPrice();
BigInteger req = request.send(). GetGasPrice();

```

### platonAccounts

> Return the list of addresses owned by the client

- **parameters**

  no

- **return value**

```java
Request<?, PlatonAccounts>

```

The String array in the PlatonAccounts property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonAccounts> request = currentValidWeb3j.platonAccounts();
List<String> req = request.send(). GetAccounts();

```

### platonBlockNumber

> Returns the current highest block height

- **parameters**

  no

- **return value**

```java
Request<?, PlatonBlockNumber>

```

The BigInteger in the PlatonBlockNumber property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonBlockNumber> request = currentValidWeb3j.platonBlockNumber();
BigInteger req = request.send(). GetBlockNumber();

```

### platonGetBalance

> Back to query address balance

- **parameters**
  -String: address The address to query
  -DefaultBlockParameter:
    -DefaultBlockParameterName.LATEST latest block height(default)
    -DefaultBlockParameterName.EARLIEST minimum block height
    -DefaultBlockParameterName.PENDING unpackaged transaction
    -DefaultBlockParameter.valueOf(BigInteger blockNumber)
- **return value**

```java
Request<?, PlatonGetBalance>

```

The BigInteger in the PlatonGetBalance property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String address = "";
Request<?, PlatonGetBalance> request = currentValidWeb3j.platonGetBalance(address, DefaultBlockParameterName.LATEST);
BigInteger req = request.send(). GetBlockNumber();

```

### platonGetStorageAt

Return value from storage location of given address

- **parameters**
  -String: address
  -BigInteger: integer for position in position memory
  -DefaultBlockParameter:
    -DefaultBlockParameterName.LATEST latest block height(default)
    -DefaultBlockParameterName.EARLIEST minimum block height
    -DefaultBlockParameterName.PENDING unpackaged transaction
    -DefaultBlockParameter.valueOf(BigInteger blockNumber)
- **return value**

```java
Request<?, PlatonGetStorageAt>

```

The String in the PlatonGetStorageAt property is the corresponding storage data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String address = "";
Request<?, PlatonGetStorageAt> request = currentValidWeb3j.platonGetStorageAt(address, BigInteger.ZERO, DefaultBlockParameterName.LATEST);
String req = request.send(). GetData();

```

### platonGetBlockTransactionCountByHash

> Query the number of transactions in a block according to the block hash

- **parameters**
  -String: blockHash block hash
- **return value**

```java
Request<?, PlatonGetBlockTransactionCountByHash>

```

The BigInteger in the PlatonGetBlockTransactionCountByHash property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String blockhash = "";
Request<?, PlatonGetBlockTransactionCountByHash> request = currentValidWeb3j.platonGetBlockTransactionCountByHash(blockhash);
BigInteger req = request.send(). GetTransactionCount();
```

### platonGetTransactionCount

> Query the number of transactions sent by the address according to the address

- **parameters**
  -String: address
  -DefaultBlockParameter:
    -DefaultBlockParameterName.LATEST latest block height(default)
    -DefaultBlockParameterName.EARLIEST minimum block height
    -DefaultBlockParameterName.PENDING unpackaged transaction
    -DefaultBlockParameter.valueOf(BigInteger blockNumber)
- **return value**

```java
Request<?, PlatonGetTransactionCount>
```

The BigInteger in the PlatonGetTransactionCount property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String address = "";
Request<?, PlatonGetTransactionCount> request = currentValidWeb3j.platonGetTransactionCount(address, DefaultBlockParameterName.LATEST);
BigInteger req = request.send(). GetTransactionCount();
```

### platonGetBlockTransactionCountByNumber

> Returns the total number of transactions in block high school based on block height

- **parameters**
  -DefaultBlockParameter:
    -DefaultBlockParameterName.LATEST latest block height(default)
    -DefaultBlockParameterName.EARLIEST minimum block height
    -DefaultBlockParameterName.PENDING unpackaged transaction
    -DefaultBlockParameter.valueOf(BigInteger blockNumber)
- **return value**

```java
Request<?, PlatonGetBlockTransactionCountByNumber>
```

The BigInteger in the PlatonGetBlockTransactionCountByNumber property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonGetBlockTransactionCountByNumber> request = currentValidWeb3j.platonGetBlockTransactionCountByNumber(DefaultBlockParameterName.LATEST);
BigInteger req = request.send(). GetTransactionCount();
```

### platonGetCode

> Return code for given address

- **parameters**
  -String: address
  -DefaultBlockParameter:
    -DefaultBlockParameterName.LATEST latest block height(default)
    -DefaultBlockParameterName.EARLIEST minimum block height
    -DefaultBlockParameterName.PENDING unpackaged transaction
    -DefaultBlockParameter.valueOf(BigInteger blockNumber)
- **return value**

```java
Request<?, PlatonGetCode>

```

The String in the PlatonGetCode property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String address = "";
Request<?, PlatonGetCode> request = currentValidWeb3j.platonGetCode(address, DefaultBlockParameterName.LATEST);
String req = request.send(). GetCode();

```

### platonSign

> Data Signature

- **parameters**
  -String: address
  -String: sha3HashOfDataToSign data to be signed
- **return value**

```java
Request<?, PlatonSign>

```

The String in the PlatonSign property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String address = "";
String sha3HashOfDataToSign = "";
Request<?, PlatonSign> request = currentValidWeb3j.platonSign(address, DefaultBlockParameterName.LATEST);
String req = request.send(). GetSignature();

```

Note: The address must be unlocked in advance

### platonSendTransaction

> Send service signing transaction

- **parameters**
  -Transaction: Transaction: transaction structure
    -String: from: transaction sending address
    -String: to: address of transaction receiver
    -BigInteger: gas: maximum gas usage for this transaction
    -BigInteger: gasPrice: gas price
    -BigInteger: value: transfer amount
    -String: data: data on the chain
    -BigInteger: nonce: transaction unique identifier
      -Call platonGetTransactionCount, get the from address as a parameter, and get the total number of sent transactions to that address
      -Each use of the address nonce +1
- **return value**

```java
Request<?, PlatonSendTransaction>

```

The String in the PlatonSendTransaction property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Transaction transaction = new Transaction("from", "to", BigInteger.ZERO, BigInteger.ZERO, BigInteger.ZERO, "data", BigInteger.ONE);
Request<?, PlatonSendTransaction> request = currentValidWeb3j.platonSendTransaction(transaction);
String req = request.send(). GetTransactionHash();

```

### platonSendRawTransaction

> Send transaction

- **parameters**
  -String: data: wallet signed data
- **return value**

```java
Request<?, PlatonSendTransaction>

```

The String in the PlatonSendTransaction property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String data = "";
Request<?, PlatonSendTransaction> request = currentValidWeb3j.platonSendRawTransaction(data);
String req = request.send(). GetTransactionHash();

```

### platonCall

> Execute a message call transaction, the message call transaction is executed directly in the node 旳 VM without the need to execute through blockchain mining

- **parameters**
  -Transaction: Transaction: transaction structure
    -String: from: transaction sending address
    -String: to: address of transaction receiver
    -BigInteger: gas: maximum gas usage for this transaction
    -BigInteger: gasPrice: gas price
    -BigInteger: value: transfer amount
    -String: data: data on the chain
    -BigInteger: nonce: transaction unique identifier
      -Call platonGetTransactionCount, get the from address as a parameter, and get the total number of sent transactions to that address
      -Each use of the address nonce +1
- **return value**

```java
Request<?, PlatonCall>

```

The String in the PlatonCall property is the corresponding stored data

- **Example**

```javas
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Transaction transaction = new Transaction("from", "to", BigInteger.ZERO, BigInteger.ZERO, BigInteger.ZERO, "data", BigInteger.ONE);
Request<?, PlatonSendTransaction> request = currentValidWeb3j.platonCall(transaction);
String req = request.send(). GetValue();

```

### platonEstimateGas

> Estimating contract method gas usage

- **parameters**
  -Transaction: Transaction: transaction structure
    -String: from: transaction sending address
    -String: to: address of transaction receiver
    -BigInteger: gas: maximum gas usage for this transaction
    -BigInteger: gasPrice: gas price
    -BigInteger: value: transfer amount
    -String: data: data on the chain
    -BigInteger: nonce: transaction unique identifier
      -Call platonGetTransactionCount, get the from address as a parameter, and get the total number of sent transactions to that address
      -Each use of the address nonce +1
- **return value**

```java
Request<?, PlatonEstimateGas>

```

The BigInteger in the PlatonEstimateGas property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Transaction transaction = new Transaction("from", "to", BigInteger.ZERO, BigInteger.ZERO, BigInteger.ZERO, "data", BigInteger.ONE);
Request<?, PlatonEstimateGas> request = currentValidWeb3j.platonEstimateGas(transaction);
BigInteger req = request.send(). GetAmountUsed();

```

### platonGetBlockByHash

> Query block information based on block hash

- **parameters**
  -String: blockHash block hash
  -boolean:
    -true: complete transaction list in block
    -false: only transaction hash list in block
- **return value**

```java
Request<?, PlatonBlock>

```

The block in the PlatonBlock property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String blockHash = "";

Request<?, PlatonBlock> request = currentValidWeb3j.platonGetBlockByHash(blockHash, true);
Block req = request.send(). GetBlock();

```

### platonGetBlockByNumber

> Query block information based on block height

- **parameters**
  -DefaultBlockParameter:
    -DefaultBlockParameterName.LATEST latest block height(default)
    -DefaultBlockParameterName.EARLIEST minimum block height
    -DefaultBlockParameterName.PENDING unpackaged transaction
    -DefaultBlockParameter.valueOf(BigInteger blockNumber)
  -boolean:
    -true: complete transaction list in block
    -false: only transaction hash list in block
- **return value**

```java
Request<?, PlatonBlock>

```

The block in the PlatonBlock property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonBlock> request = currentValidWeb3j.platonGetBlockByNumber(DefaultBlockParameter.valueOf(BigInteger.ZERO), true);
Block req = request.send(). GetBlock();

```

### platonGetTransactionByBlockHashAndIndex

> Query the transaction with the specified serial number in the block according to the block hash

- **parameters**
  -String: blockHash block hash
  -BigInteger: transactionIndex number of the transaction in the block
- **return value**

```java
Request<?, PlatonTransaction>

```

The transaction in the PlatonTransaction property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String blockHash = "";
Request<?, PlatonTransaction> request = currentValidWeb3j.platonGetTransactionByHash(blockHash, BigInteger.ZERO);
Optional<Transaction> req = request.send(). GetTransaction();
```

### platonGetTransactionByBlockNumberAndIndex

> Query the transaction with the specified serial number in the block according to the block height

- **parameters**
  -DefaultBlockParameter:
    -DefaultBlockParameterName.LATEST latest block height(default)
    -DefaultBlockParameterName.EARLIEST minimum block height
    -DefaultBlockParameterName.PENDING unpackaged transaction
    -DefaultBlockParameter.valueOf(BigInteger blockNumber)
  -BigInteger: transactionIndex number of the transaction in the block
- **return value**

```java
Request<?, PlatonTransaction>
```

The transaction in the PlatonTransaction property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String blockHash = "";
Request<?, PlatonTransaction> request = currentValidWeb3j.platonGetTransactionByHash(DefaultBlockParameter.valueOf(BigInteger.ZERO), BigInteger.ZERO);
Optional<Transaction> req = request.send(). GetTransaction();
```

### platonGetTransactionReceipt

> Query transaction receipt based on transaction hash

- **parameters**
  -String: transactionHash
- **return value**

```java
Request<?, PlatonGetTransactionReceipt>
```

The transaction in the PlatonGetTransactionReceipt property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String blockHash = "";
Request<?, PlatonGetTransactionReceipt> request = currentValidWeb3j.platonGetTransactionReceipt(DefaultBlockParameter.valueOf(BigInteger.ZERO), BigInteger.ZERO);
Optional<TransactionReceipt> req = request.send(). GetTransactionReceipt();
```

### platonNewFilter

Create a filter to notify when the client receives a matching whisper message

- **parameters**
  -PlatonFilter: PlatonFilter:
    -SingleTopic:
- **return value**

```java
Request<?, PlatonFilter>

```

The BigInteger in the PlatonFilter property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
org.web3j.protocol.core.methods.request.PlatonFilter filter = new org.web3j.protocol.core.methods.request.PlatonFilter();
filter.addSingleTopic("");
Request<?, PlatonFilter> request = currentValidWeb3j.platonNewFilter(filter);
BigInteger req = request.send(). GetFilterId();

```

### platonNewBlockFilter

> Create a filter in the node to be notified when new blocks are generated. To check if the status changes

- **parameters**

  no

- **return value**

```java
Request<?, PlatonFilter>

```

The BigInteger in the PlatonFilter property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonFilter> request = currentValidWeb3j.platonNewBlockFilter();
BigInteger req = request.send(). GetFilterId();

```

### platonNewPendingTransactionFilter

> Create a filter in the node to be notified when a pending transaction occurs. To check if the status has changed

- **parameters**

  no

- **return value**

```java
Request<?, PlatonFilter>

```

The BigInteger in the PlatonFilter property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonFilter> request = currentValidWeb3j.platonNewPendingTransactionFilter();
BigInteger req = request.send(). GetFilterId();

```

### platonNewPendingTransactionFilter

> Write in filter with specified number. This call is always needed when listening is no longer needed

- **parameters**

  no

- **return value**

```java
Request<?, PlatonFilter>

```

The BigInteger in the PlatonFilter property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonFilter> request = currentValidWeb3j.platonNewPendingTransactionFilter();
BigInteger req = request.send(). GetFilterId();

```

### platonUninstallFilter

> Write in filter with specified number. This call is always needed when listening is no longer needed

- **parameters**
  -BigInteger: filterId: filter ID
- **return value**

```java
Request<?, PlatonUninstallFilter>

```

The boolean in the PlatonUninstallFilter attribute is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonUninstallFilter> request = currentValidWeb3j.platonNewPendingTransactionFilter(BigInteger.ZERO);
boolean req = request.send(). isUninstalled();

```

### platonGetFilterChanges

> Polling the specified filter and returning a newly generated log array since the last poll

- **parameters**
  -BigInteger: filterId: filter ID
- **return value**

```java
Request<?, PlatonLog>

```

The LogResult array in the PlatonLog property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonLog> request = currentValidWeb3j.platonGetFilterChanges(BigInteger.ZERO);
List<PlatonLog.LogResult> req = request.send(). GetLogs();

```

### platonGetFilterLogs

> Polling the specified filter and returning a newly generated log array since the last poll.

- **parameters**
  -BigInteger: filterId: filter ID
- **return value**

```java
Request<?, PlatonLog>

```

The LogResult array in the PlatonLog property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonLog> request = currentValidWeb3j.platonGetFilterLogs(BigInteger.ZERO);
List<PlatonLog.LogResult> req = request.send(). GetLogs();

```

### platonGetLogs

> Return all logs in the specified filter

- **parameters**
  -PlatonFilter: PlatonFilter:
    -SingleTopic:
- **return value**

```java
Request<?, PlatonLog>

```

The BigInteger in the PlatonLog property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
org.web3j.protocol.core.methods.request.PlatonFilter filter = new org.web3j.protocol.core.methods.request.PlatonFilter();
filter.addSingleTopic("");
Request<?, PlatonLog> request = currentValidWeb3j.platonGetLogs(filter);
List<LogResult> = request.send(). GetLogs();

```

### platonPendingTransactions

> Query pending transactions

- **parameters**

  no

- **return value**

```java
Request<?, PlatonPendingTransactions>

```

The transactions in the PlatonPendingTransactions property are the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonPendingTransactions> req = currentValidWeb3j.platonPendingTx();
EthPendingTransactions res = req.send();
List<Transaction> transactions = res.getTransactions();

```

### dbPutString

> Store strings in local database.

- **parameters**
  -String: databaseName: database name
  -String: keyName: key name
  -String: stringToStore: the string to be stored
- **return value**

```java
Request<?, DbPutString>

```

The boolean in the DbPutString property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String databaseName;
String keyName;
String stringToStore;
Request<?, DbPutString> request = currentValidWeb3j.dbPutString(databaseName, keyName, stringToStore);
List<DbPutString> = request.send(). ValueStored();

```

### dbGetString

Read string from local database

- **parameters**
  -String: databaseName: database name
  -String: keyName: key name
- **return value**

```java
Request<?, DbGetString>

```

The String in the DbGetString property is the corresponding stored data

**Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String databaseName;
String keyName;
Request<?, DbGetString> request = currentValidWeb3j.dbGetString(databaseName, keyName);
String req = request.send(). GetStoredValue();
```

### dbPutHex

> Write binary data to local database

- **parameters**
  -String: databaseName: database name
  -String: keyName: key name
  -String: dataToStore: binary data to be stored
- **return value**

```java
Request<?, DbPutHex>
```

The boolean in the DbPutHex attribute is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String databaseName;
String keyName;
String dataToStore;
Request<?, DbPutHex> request = currentValidWeb3j.dbPutHex(databaseName, keyName, dataToStore);
boolean req = request.send(). valueStored();
```

### dbGetHex

> Read binary data from local database

- **parameters**
  -String: databaseName: database name
  -String: keyName: key name
- **return value**

```java
Request<?, DbGetHex>
```

The String in the DbGetHex property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
String databaseName;
String keyName;
Request<?, DbGetHex> request = currentValidWeb3j.dbGetHex(databaseName, keyName);
String req = request.send(). GetStoredValue();
```

### platonEvidences

> Return double sign report data

- **parameters**

  no

- **Export parameters**

Parameters | Type | Description |
| ------- | ------ | ---------- |
| jsonrpc | string | rpc version number |
| id | int | id serial number |
| result | string | Evidence String |

result is the evidence string, which contains 3 types of evidence: duplicatePrepare, duplicateVote, duplicateViewchange
Each type contains multiple evidences, so it is an array structure, and you need to pay attention when parsing.

- **duplicatePrepare**

```json
{
    "prepare_a": {
        "epoch": 0, // epoch value of consensus round
        "view_number": 0, // View value of consensus round
        "block_hash": "0xf41006b64e9109098723a37f9246a76c236cd97c67a334cfb4d54bc36a3f1306",
        // block hash
        "block_number": 500, // block number
        "block_index": 0, // The index value of the block in a round view
        "validate_node": {
            "index": 0, // The index value of the validator in a round of epoch
            "address": "0x0550184a50db8162c0cfe9296f06b2b1db019331", // Verifier address
            "NodeID": "77fffc999d9f9403b65009f1eb27bae65774e2d8ea36f7b20a89f82642a5067557430e6edfe5320bb81c3666a19cf4a5172d6533117d7ebcd0f2c82055499050", // Verifier nodeID
            "blsPubKey": "5ccd6b8c32f2713faa6c9a46e5fb61ad7b7400e53fabcbc56bdc0c16fbfffe09ad6256982c7059e7383a9187ad93a002a7cda717771fffccfcfcfccfcfcfcfc6cfc6c6c6c6c3c6c6c3c3c3c3c3c3c3c3c3c3c3c2c3c3c3c3c3c3c1c2c1c2c1c2c2c1c2c2c2c2c2c2c2c2c2c2c2c2c2c2c5c2c2c2c2c2f5cd6b8c32f271
        },
        "signature": "0xa7205d571b16696b3a9b68e4b9ccef001c751d860d0427760f650853fe563f5191f2292dd67ccd6c89ed050182f19b9200000000000000000000000000000000" // Signature of the message
    }
 }
```

- **duplicateVote**

```json
{
    "voteA": {
        "epoch": 0, // epoch value of consensus round
        "view_number": 0, // View value of consensus round
        "block_hash": "0xf41006b64e9109098723a37f9246a76c236cd97c67a334cfb4d54bc36a3f1306",
        // block hash
        "block_number": 500, // block number
        "block_index": 0, // The index value of the block in a round view
        "validate_node": {
            "index": 0, // The index value of the validator in a round of epoch
            "address": "0x0550184a50db8162c0cfe9296f06b2b1db019331", // Verifier address
            "NodeID": "77fffc999d9f9403b65009f1eb27bae65774e2d8ea36f7b20a89f82642a5067557430e6edfe5320bb81c3666a19cf4a5172d6533117d7ebcd0f2c82055499050", // Verifier nodeID
            "blsPubKey": "5ccd6b8c32f2713faa6c9a46e5fb61ad7b7400e53fabcbc56bdc0c16fbfffe09ad6256982c7059e7383a9187ad93a002a7cda717771fffccfcfcfccfcfcfcfc6cfc6c6c6c6c3c6c6c3c3c3c3c3c3c3c3c3c3c3c2c3c3c3c3c3c3c1c2c1c2c1c2c2c1c2c2c2c2c2c2c2c2c2c2c2c2c2c2c5c2c2c2c2c2f5cd6b8c32f271
        },
        "signature": "0xa7205d571b16696b3a9b68e4b9ccef001c751d860d0427760f650853fe563f5191f2292dd67ccd6c89ed050182f19b9200000000000000000000000000000000" // Signature of the message
    }
 }
```

- **duplicateViewchange**

```json
{
    "viewA": {
        "epoch": 0, // epoch value of consensus round
        "view_number": 0, // View value of consensus round
        "block_hash": "0xf41006b64e9109098723a37f9246a76c236cd97c67a334cfb4d54bc36a3f1306",
        // block hash
        "block_number": 500, // block number
        "block_index": 0, // The index value of the block in a round view
        "validate_node": {
            "index": 0, // The index value of the validator in a round of epoch
            "address": "0x0550184a50db8162c0cfe9296f06b2b1db019331", // Verifier address
            "NodeID": "77fffc999d9f9403b65009f1eb27bae65774e2d8ea36f7b20a89f82642a5067557430e6edfe5320bb81c3666a19cf4a5172d6533117d7ebcd0f2c82055499050", // Verifier nodeID
            "blsPubKey": "5ccd6b8c32f2713faa6c9a46e5fb61ad7b7400e53fabcbc56bdc0c16fbfffe09ad6256982c7059e7383a9187ad93a002a7cda717771fffccfcfcfccfcfcfcfc6cfc6c6c6c6c3c6c6c3c3c3c3c3c3c3c3c3c3c3c2c3c3c3c3c3c3c1c2c1c2c1c2c2c1c2c2c2c2c2c2c2c2c2c2c2c2c2c2c5c2c2c2c2c2f5cd6b8c32f271
        },
        "signature": "0xa7205d571b16696b3a9b68e4b9ccef001c751d860d0427760f650853fe563f5191f2292dd67ccd6c89ed050182f19b9200000000000000000000000000000000" // Signature of the message
    }
 }
```

- **return value**

```java
Request<?, PlatonEvidences>
```

The Evidences object in the PlatonEvidences property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, PlatonEvidences> req = currentValidWeb3j.platonEvidences();
Evidences evidences = req.send(). GetEvidences();
```

### getProgramVersion

> Get code version

- **parameters**

  no

- **return value**

```java
Request<?, AdminProgramVersion>
```

The ProgramVersion object in the AdminProgramVersion property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, AdminProgramVersion> req = currentValidWeb3j.getProgramVersion();
ProgramVersion programVersion = req.send(). GetAdminProgramVersion();
```

- **ProgramVersion Object Parsing**
  -BigInteger: version: code version
  -String: sign: code version signature

### getSchnorrNIZKProve

> Get proof of bls

- **parameters**

  no

- **return value**

```java
Request<?, AdminSchnorrNIZKProve>
```

The String in the AdminSchnorrNIZKProve property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, AdminProgramVersion> req = currentValidWeb3j.getSchnorrNIZKProve();
String res = req.send(). GetAdminSchnorrNIZKProve();
```

### getEconomicConfig

> Get PlatON parameter configuration

- **parameters**

  no

- **return value**

```java
Request<?, DebugEconomicConfig>
```

The String in the DebugEconomicConfig property is the corresponding stored data

- **Example**

```java
Web3j platonWeb3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
Request<?, DebugEconomicConfig> req = currentValidWeb3j.getEconomicConfig();
String debugEconomicConfig = req.send(). GetEconomicConfigStr();
```