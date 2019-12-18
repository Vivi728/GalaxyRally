# PlatON node installation and deployment manual

## 1. Node installation and deployment

### 1.1 Environmental requirements

- Hardware

| Requirements | Suggested Configuration |
| ------------ | ----------------------- |
| Cores        | \> = 4                  |
| CPU          | \> = 2.4 GHz            |
| RAM          | \> = 8GB                |
| DISK         | \> = 500G               |

> Note:
>
> - The DISK capacity in the table is the basic configuration. With the long-term operation of the network, the actual demand for DISK will gradually increase. It is recommended to use an SSD disk that can be dynamically expanded online.

- Software

The PlatON node has been fully tested with the Ubuntu 18.04 operating system. It is recommended to use Ubuntu 18.04. All node-related operations in this document are based on Ubuntu 18.04.

- The internet

| Port  | Service Name |
| ----- | ------------ |
| 443   | https        |
| 16789 | p2p          |

PlatON's network bandwidth requirement is greater than or equal to 20Mbps. At the same time, the cloud host needs to open external access permissions for the above service ports.

- MTool environment

MTool is a node management tool. To facilitate operation, it is recommended to use the Windows 10 operating system. All MTool-related operations in this document are based on Windows.

### 1.2 Install PlatON

PlatON data and logs are generated under the root directory of the non-root user running the script. Make sure that the user's root directory has enough space to store these files.

**step1.** Download the platon_setup.sh script as a non-root account

```bash
wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/platon_setup.sh
```

or

```bash
wget http://47.91.153.183/opensource/scripts/platon_setup.sh

```

**step2.** Execute script

```bash
chmod + x platon_setup.sh && ./platon_setup.sh 0.7.4
```

> Attention
>
> - During the installation process, the old version of platon is uninstalled, the running platon process is stopped, and the old version data is deleted.
> - 0.7.4 is the version number of the specified installation, which is modified according to the actual version number.
> - When prompted for [sudo] password for`, please enter the current account password.
> - When you press `Press [ENTER] to continue or Ctrl-c to cancel adding it`, please press Enter.
> - When the prompt `install platon and attach platon and get block number succeed` is displayed, it means that PlatON is successfully installed. If it is not successfully installed, please feedback specific problems through our official customer contact information.

### 1.3 Configure nginx

For security reasons, it is not recommended to open the rpc port of the node directly to the outside. You can consider using Nginx as a reverse proxy, and strengthen the security of Nginx ports through user authentication and HTTPS. Nginx configuration steps are as follows:

**step1.** Download nginx_conf.sh as a non-root account

```bash
wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/nginx_conf.sh
```

or

```bash
http://47.91.153.183/opensource/scripts/nginx_conf.sh
```

**step2.** Execute script

```bash
chmod + x nginx_conf.sh && ./nginx_conf.sh
```

> Attention
>
> - When prompted for [sudo] password for`, enter the current account password.
> - When you are prompted for `Enter your name:`, enter your user name, and when you are prompted for `Enter your password:`, enter your password. Be sure to keep the user name and password in mind, and you will need to fill in the subsequent MTool configuration to verify the node information.
> - When the prompt `ngnix conf succeed` is displayed, it means that nginx is successfully configured. If it is not configured successfully, please contact us for specific questions.

## 2. Access the test network

### 2.1 Overview of Validation Nodes

PlatON is a blockchain project that implements democratic governance. Verification nodes are jointly selected by all Energon holders to maintain and develop the PlatON network. The 101 nodes with the most votes will become candidate nodes, from which 25 verification nodes will be randomly selected using VRF to participate in the management of the entire PlatON network.

- Pledged 1 million Energon as candidate node candidate, and can accept commission
- At the beginning of each staking cycle, the top 101 nodes with total votes (including self-pledged nodes and other people's entrustment) become candidate nodes for the current staking cycle
- The 230th block of each consensus cycle (round of 250 blocks), from the 101 candidate nodes in the current staking cycle, randomly select 25 verification nodes based on the weight of the total votes to produce blocks

### 2.2 Preparation before installation

Anti-virus software such as 360 security guards and Tencent Computer Manager misjudge tool files as virus files and delete them. It is recommended to close these anti-virus software before operation.

Windows key + x, click Windows PowerShell (Administrator) (A), select Yes in the pop-up window, bring up the Administrator: powershell window, and execute the `mtool-client.bat --version` command.

The execution result shows that `mtool-client.bat 'cannot be recognized as the name of a cmdlet, function, script file, or executable program. Check the spelling of the name, and if the path is included, make sure the path is correct and try again. `, Which means the following operation is not required if the old version is not installed.

The execution result shows the version number, time stamp and other information indicating that the old version is installed. At this time, you need to back up important information, and then manually uninstall the old version. Operation steps:

**step1.** Back up all files under the directory `C: \ tools \ mtool \ current \ keystore` to the D drive or other directories other than` C: \ tools`. After installing the new version, you need to copy the backup file back to the `C: \ tools \ mtool \ current \ keystore` directory.

**step2.** Back up all files under the directory `C: \ tools \ mtool \ current \ validator` to the D drive or other directories other than` C: \ tools`. After installing the new version, you need to copy the backup file back to the `C: \ tools \ mtool \ current \ validator` directory.

**step3.** Execute the following command in the previously opened powershell window to uninstall the old tool software related to mtool.

Execute the `choco list --local-only` command to obtain related tools that include the mtool keyword, such as:` platon_mtool_other 0.7.3`, `platon_mtool_all 0.7.3`,` mtool 0.7.3`, etc.

Run `choco uninstall platon_mtool_other 0.7.3`,` choco uninstall platon_mtool_all 0.7.3` and other commands to uninstall the old related tools one by one.

**step4.** Delete the `C: \ tools \ mtool`,` C: \ tools \ platon_mtool_other` and other directories under `C: \ tools` that contain the mtool keyword.

### 2.3 Install MTool

MTool is a command-line version of the node management tool that can easily initiate transactions such as pledges. MTool provides two signature methods for transactions such as pledges: online signature and offline signature.

- Online signature is relatively convenient. This document mainly introduces the online signature operation process
- Users with high security requirements can refer to [Offline MTool User Manual.md] (./ Offline MTool User Manual.md) to use offline signature.

In addition, this document mainly introduces MTool operation in the Windows environment. If the download script fails, please set the DNS server to 8.8.8.8.

The installation steps of Mtool in Windows environment are as follows:

**step1.** Administrators called up in the [Preparation before installation] (# 22-Preparation before installation) step: In the powershell window, copy the following two commands for execution.

```bash
$ env: chocolateyUseWindowsCompression = 'true'
Set-ExecutionPolicy -ExecutionPolicy Bypass
```

prompt:

```plain
Enforcing policy changes
Execution policies help you prevent execution of untrusted scripts. Changing execution policies can create security risks, as described in the about_Execution_Policies help topic at https: /go.microsoft.com/fwlink/? LinkID = 135170. Do you want to change the execution strategy?
[Y] Yes (Y) [A] All (A) [N] No (N) [L] No (L) [S] Pause (S) [?] Help (default is "N"):
```
Please enter: y and press Enter to end.

**step2.** Browser copy link <https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/mtool_install.bat> or <http://47.91.153.183/opensource/scripts /mtool_install.bat> Download script

**step3.** Right-click mtool_install.bat and select Run as administrator

> Attention
>
> - The password for mysql installation is empty for the first time. When the prompt "Enter password:" is displayed, please press the Enter key directly. If you have already installed mysql, when you are prompted for `Enter password:`, please enter the password of the mysql root account that you have set (if you previously installed mysql using the mtool_install.bat script, the password is 123456), and then press Enter.
> - When the message `install and start mtool success` is displayed, MTool is successfully installed. If it is not successfully installed, please contact our official customer contact to feedback specific problems.
> - When prompting `Please press any key to continue ... ', please press Enter to close the current cmd window.

### 2.4 Create Wallet

In PlatON, two wallets are created to participate in the verification node for block generation.

- Pledge wallet

  The pledge wallet is used to pledge tokens. Only after successful pledge can be a candidate node candidate.
  Windows + R, enter cmd into the terminal, run the following command to create a pledge wallet:

  ```shell
  mtool-client.bat create_wallet --name staking
  ```
  Enter the password once and confirm the password again to create the wallet file. After the creation is successful, the pledge wallet file `staking.json` will be generated in the directory` C: \ tools \ mtool \ current \ keystore`.

- Earning wallet

    It is used to collect block rewards and Staking rewards. Staking rewards are uniformly distributed to verification nodes, which are distributed by the verification nodes themselves.
    Windows + R, enter cmd into the terminal, run the following command to create a revenue wallet:

    ```shell
    mtool-client.bat create_wallet --name reward
    ```
    Enter the password once and enter the confirmation password again to create.

### 2.5 Configure Verification Node Information

**step1.** Browser copy link <https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/validator_conf.bat> or <http://47.91.153.183/opensource/scripts/validator_conf.bat>  Download script.

**step2.** Right-click validator_conf.bat and select Run as administrator

> Attention
>
> - When prompting `Please enter the platon node IP address:`, please enter the PlatON node server IP address.
>   -When prompted `Please enter the platon chain id:`, please enter the chain ID.
>   -When prompted `Enter your name:`, enter the username you entered when configuring nginx.
> - When prompted `Enter your password:`, please enter the password you entered when configuring nginx.
> - When prompting `Enter your platon node name:`, enter the name of the PlatON node.
> - When prompted `Enter your platon node description:`, enter the PlatON node description.
> - When the prompt "validator conf success" is displayed, it means that the script has been executed successfully. If the script is not executed successfully, please contact our official customer contact to feedback specific problems.
> - When prompting `Please press any key to continue ... ', please press Enter to close the current cmd window.
>   -Please go to `C: \ tools \ mtool \ current \ validator` directory and check the validator_config.json content.

### 2.6 Apply for pledged funds

Pledged funds LAT To apply to PlatON official, please send an email to PlatON official email <support@platon.network>, the format is as follows:

```plain
[Rally-LAT Application]

Applicant / Applicant Organization / Institution: {XXX}

|Node ID | Node Name | Node IP and Port | Pledge Account Address | Revenue Account Address |
| -------- | ---------- | ------------ | ------------ | --- --------- |
| {nodeId} | {nodeName} | {ip}: {port} | {address} | {address} |
```

- nodeId: obtained in `~ / platon-node / data / nodeid`
- nodeName: the name given to the node by the PlatON node owner
- ip: IP address of the node
- port: Node port number, the default value is 16789
- address: Pledge wallet address and income wallet address. After MTool creates the wallet, it can be obtained in the wallet file. The address in the wallet file plus 0x is a valid address.

After the application is submitted, please wait patiently. You can enter the pledge account address in the search box on the main page of PlatON's official blockchain browser [PlatScan] (https://platscan.platon.network) to confirm whether the officially issued token has arrived.

### 2.7 Verify node pledge

Use node tool MTool to perform node pledge and delegation. For capable developers, you can develop your own node tools based on the Java SDK and Javascript SDK. Please refer to the documentation for SDK.

-[Java-SDK] (./ Java-SDK.md)
-[JavaScript-API] (./ JavaScript-API.md)

After the application of the pledged funds is received, ensure that the balance of the pledged account is sufficient, and replace {Pledged Amount} according to the user's situation. The minimum threshold for pledge is 1 million LAT.

```shell
mtool-client.bat staking --amount {Pledged Amount} --keystore% MTOOLDIR% \ keystore \ staking.json --config% MTOOLDIR% \ validator \ validator_config.json
```

E.g:

```shell
mtool-client.bat staking --amount 1000000 --keystore% MTOOLDIR% \ keystore \ staking.json --config% MTOOLDIR% \ validator \ validator_config.json
```

> Attention
>
> - When prompting `please input keystore password:`, enter the password of the pledge wallet.
> - The prompt `operation finished` and the prompt` SUCCESS` indicate that the pledge was successful.

### 2.8 Verification node confirmation

After completing the pledge operation, you can click the verification node list at the top of the homepage on the PlatON official blockchain browser [PlatScan](https://platscan.platon.network) to view all verification nodes, or enter the node name in the input box to query the nodes For specific information, check whether it becomes a verification node.

## 3. Data backup

When the data of the verifier node is inconsistent and the chain cannot run normally, the verification node needs to roll back the data to the block that has recently reached a consensus state, and the verification node can use the backup data provided by the community.

**For real-time and security reasons, it is recommended that conditional nodes build backup nodes by themselves. Data backup configuration steps are as follows:**

**step1.** Install another backup node according to [node installation deployment] (# 1-node installation deployment)

**step2.** Download the timing_backup.sh script as a non-root account

```bash
wget https://7w6qnuo9se.s3.eu-central-1.amazonaws.com/opensource/scripts/timing_backup.sh
```

or

```bash
wget http://47.91.153.183/opensource/scripts/timing_backup.sh
```

**step3.** Execute the following script

```bash
chmod + x timing_backup.sh && ./timing_backup.sh
```

> Attention
>
> - When prompted for [sudo] password for`, enter the current account password.
> - When the prompt `add backup_task.sh to crontab succeed` is displayed, it means that the configuration database backup task is successful. If it is not configured successfully, please contact our official customer contact to feedback specific problems.
> - `crontab -l` shows that there are` 0 0 * * * $ {HOME} / platon-node / backup_task.sh` fields, where `$ {HOME}` is replaced with the user's root directory, indicating success.
> - The database backup scheduled task is executed daily at 0 o'clock, and the backup file is generated in the `$ {HOME} / platon-node / data` directory.

## 4. Emergency Management

Please pay attention to the latest news from the community in real time, and the major events of the underlying chain will be announced in the community as soon as possible.

When the underlying chain fails to continue to run normally, please execute [Offline Data Rollback Upgrade Guide] (./Offchain Data Rollback Upgrade Guide.md) to restore the underlying chain to normal operation.

When you find any problems with the underlying chain, please notify the community as soon as possible.

​    