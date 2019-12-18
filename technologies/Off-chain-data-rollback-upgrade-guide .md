## Off-chain data rollback upgrade guide

### 1 Introduction

When the data of the verifier nodes are inconsistent, consensus cannot be reached, and the chain cannot run.

All verification nodes need to roll back the node data to the highest block that reached a consensus state. If the verification node has a backup node, the data rollback operation must be performed in the same way as the verification node.

### 2 Verify node data rollback

If the download script fails, set the DNS server to 8.8.8.8.

#### 2.1 Back up the current block data

- Download data backup script

  ```bash
  wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/node_back.sh
  ```

  or

  ```bash
  wget http://47.91.153.183/opensource/scripts/node_back.sh
  ```

- Execute data backup script

  ```bash
  chmod u + x node_back.sh && ./node_back.sh
  ```

#### 2.2 Deploying a new version of platon

Deploy and install according to the upgrade version number (assuming 0.10.0) obtained from the community announcement. Proceed as follows:

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
  chmod u + x update_platon.sh && ./update_platon.sh 0.10.0 nostart
  ```

  Note: 0.10.0 is the version number of the designated upgrade.**nostart**means that the node will not start (default startup). When [**][sudo] password for platon: "**similar prompt, you need to enter the current user's Password; when the**"Do you want to continue?"**prompt appears, enter:**y**; the execution result is as follows: the version upgrade was successful:

  ```
  Currently installed version: 0.9.0 ===========
  Start installation: 0.10.0 version ===========
  Do you want to continue? [Y / n] y
  Uninstall the current version: platon0.9.0 success ===========
  Installation version: platon0.10.0 success ===========
  ```

> Note:
>
> No version upgrade:
>
> - Specified upgrade version does not exist
> - Specify that the version to be upgraded is not higher than the installed version
>
> Nodes that have not been version upgraded run with the previous version.

#### 2.3 Rollback backup data

When the data needs to be rolled back, the community will release the relevant off-chain data rollback announcement in the**Off-chain data rollback upgrade announcement**link in the [Announcement Area] (../ README.md). The announcement will notify the user to roll back to the specified block height and the backup data address. Taking the rollback block height**10000**as an example, the announcement template is as follows:

```bash
The block height of the data rollback is: [10000]
The block hash of the data rollback is: 0xf0be4fe085ad98f355a9797b7d2a3927cc53f2e8354567f6142ab8b954572b3c

1. For data rollback solutions, please refer to: [Offline Data Rollback Upgrade Guide.md]
2. Data download address to be rolled back: [...]
3. The code branches for this off-chain upgrade are:
   [Code branch: https://github.com/PlatONnetwork/PlatON-Go/tree/pip_v0.7.2]
   [Commit ID: 5696a3b458099a58f4308949a977f5f1ec9922d5]
4. Target version number: 0.10.0
```

If you see this announcement, you can choose the following two methods for data rollback operation.

##### 2.3.1**Rollback the data backed up by the backup node**

If the node backed up the data itself, the format of the backup file is: data_backup_YYYY_MM_DD_num.tar.gz, where num is the block height at the time of backup. From the files backed up by you, select the file with a block height not higher than 10000 and the closest to 10000 for rollback. For the operation of the backup node, refer to the**3. Data backup**section of the [PlatON node installation and deployment manual. Md] (./PlatON node installation and deployment manual. Md).

- **Filter backup files**

Execute the command on the backup node:

```bash
cd ~ / platon-node / data && ls -t | awk -F '[_.]' -vt = 10000 'BEGIN {min = 65535} {d = t- $ 6; if (d> = 0 && min> d ) {min = d; minfile = $ 0;}} END {print minfile} '
```
Note: 10000 in**t = 10000**is the height of the rolled back block, which is subject to the actual block height in the announcement, reference: [2.3 Rollback backup data] (# 23-Rollback backup data). This command returns the qualified backup file name, such as: data_backup_2019_11_05_9900.tar.gz.

- **Copy backup file**

Copy the filtered backup file from the**backup node**to the**~ platon-node / data**directory of the**verification node**.

- **Rollback data**

Execute the command on the verification node:
```bash
cd ~ / platon-node / data && tar -xzvf data_backup_2019_11_05_9900.tar.gz
```

Note: data_backup_2019_11_05_9900.tar.gz needs to be modified to the backup file name returned by the**Filter backup files**command.

##### 2.3.2 Rolling back data downloaded by the community

Get the download path of the backup file from the community announcement, refer to: [2.3 Rollback backup data] (# 23-rollback backup data) Download URL in the announcement, such as: https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/backup/data_backup_2019_11_05_10000.tar.gz

- **Get backup file**

```bash
cd ~ / platon-node / data && wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/backup/data_backup_2019_11_05_10000.tar.gz
```

> Note: The download address needs to be changed to the actual backup file download address in the announcement.

- **Rollback data**

```bash
tar -xzvf data_backup_2019_11_05_10000.tar.gz
```

> Note: data_backup_2019_11_05_10000.tar.gz needs to be modified to the name of the backup file actually downloaded.

#### 2.4 Restart Node

```shell
cd ~ / platon-node && nohup platon --identity "platon" --datadir ./data --port 16789 --rpc --rpcaddr 127.0.0.1 --maxpeers 25 --rpcport 6789 --rpcapi "platon, debug, personal, admin, net, web3 "--nodekey" ./data/nodekey "--cbft.blskey ./data/blskey &
```

#### 2.5 Verify rollback

Excuting an order:

```bash
cd ~ / platon-node / data && platon attach ipc: platon.ipc -exec platon.blockNumber
```

Execute the above command multiple times and observe the block height value returned by the execution command. It will go through the following three stages:

> - The block height grows to the specified rollback block height, and then stops producing blocks.
> - Any verification node collects 2/3 of the number of validators, that is, 76 or more version declaration transactions, and then continue to produce blocks.
> - Synchronize blocks, block heights start to grow.

Note: If the rollback situation is as described above, the data rollback is successful; otherwise, the data rollback fails. During the second phase of collecting verifier version statement transactions, any transaction sent by the node will not be chained, and will be temporarily placed in the transaction pool. After the version statement transactions collected by the verification node reach 76, the transaction pool will be in the transaction pool. The transactions will be packaged one after another. For a version statement transaction, please refer to the document: [Online MTool User Manual.md](./Online-MTool-User-Manual.md)
**4.13 Version Declaration Operation Chapter**; if you use the offline signature transaction method, please refer to the document: [Offline MTool User Manual.md](./Offline-MTool-User-Manual.md).