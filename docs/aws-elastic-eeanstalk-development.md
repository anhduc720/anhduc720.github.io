---
title: AWS Elastic Eeanstalk Development
date: 2018-09-15 07:42:34
slug: aws-elastic-eeanstalk-development
tags: ["aws-elastic", "development"]
status: active
---

## EB-CLI Installer

### 1.1. Prerequisites

If you don't have Git, install it from [Git Downloads](https://git-scm.com/downloads).

Python, which the EBCLI Installer depends on, requires the following prerequisites for each operating system.

- **Linux**
    - **Ubuntu and Debian**

        ```shell
        build-essential zlib1g-dev libssl-dev libncurses-dev libffi-dev libsqlite3-dev libreadline-dev libbz2-dev
        ```

    - **Amazon Linux and Fedora**

       ```shell
       "Development Tools" zlib-devel openssl-devel ncurses-devel libffi-devel sqlite-devel.x86_64 readline-devel.x86_64 bzip2-devel.x86_64
       ```

- **macOS**

     ```shell
     Xcode openssl zlib readline
     ```

------


### 2. Use

------

#### 2.1. Clone this repository

Use the following:

```shell
git clone https://github.com/aws/aws-elastic-beanstalk-cli-setup.git
```

#### 2.2. Install/Upgrade the EB CLI

On **Bash** and **Zsh** on macOS and Linux:

```shell
./aws-elastic-beanstalk-cli-setup/scripts/bundled_installer
```

In **PowerShell** or in a **Command Prompt** window:

```ps1
.\aws-elastic-beanstalk-cli-setup\scripts\bundled_installer
```

#### 2.3. Troubleshooting

- **Linux**

    Most installation problems have been due to missing libraries such as `OpenSSL`.

  - On **Ubuntu and Debian**, run the following command to install dependencies.

    ```shell
    apt-get install \
        build-essential zlib1g-dev libssl-dev libncurses-dev \
        libffi-dev libsqlite3-dev libreadline-dev libbz2-dev
    ```

  - On **Amazon Linux and Fedora**, run the following command to install dependencies.

    ```shell
    yum group install "Development Tools"
    yum install \
        zlib-devel openssl-devel ncurses-devel libffi-devel \
        sqlite-devel.x86_64 readline-devel.x86_64 bzip2-devel.x86_64
    ```

- **macOS**

  Most installation problems on macOS are related to loading and linking OpenSSL and zlib. The following command installs the necessary packages and tells the Python installer where to find them:

    ```
    brew install zlib openssl readline
    CFLAGS="-I$(brew --prefix openssl)/include -I$(brew --prefix readline)/include -I$(xcrun --show-sdk-path)/usr/include" LDFLAGS="-L$(brew --prefix openssl)/lib -L$(brew --prefix readline)/lib -L$(brew --prefix zlib)/lib"
    ```
    Run `brew info` to get the latest environment variable export suggestions, such as `brew info zlib`

- **Windows**

    - In PowerShell, if you encounter an error with the message "execution of scripts is disabled on this system", set the [execution policy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6) to `"RemoteSigned"` and then rerun `bundled_installer`.

      ```ps1
      Set-ExecutionPolicy RemoteSigned
      ```
    - If you encounter an error with the message "No module named 'virtualenv'", use the following commands to install `virtualenv` and the EB CLI:
      ```ps1
      pip uninstall -y virtualenv
      pip install virtualenv
      python .\aws-elastic-beanstalk-cli-setup\scripts\ebcli_installer.py
      ```

## EB Development

### Clone Source code
```
$ git clone
```
### Init EB
```bash
$ eb init

Select a default region
1) us-east-1 : US East (N. Virginia)
2) us-west-1 : US West (N. California)
3) us-west-2 : US West (Oregon)
4) eu-west-1 : EU (Ireland)
5) eu-central-1 : EU (Frankfurt)
6) ap-south-1 : Asia Pacific (Mumbai)
7) ap-southeast-1 : Asia Pacific (Singapore)
8) ap-southeast-2 : Asia Pacific (Sydney)
9) ap-northeast-1 : Asia Pacific (Tokyo)
10) ap-northeast-2 : Asia Pacific (Seoul)
11) sa-east-1 : South America (Sao Paulo)
12) cn-north-1 : China (Beijing)
13) cn-northwest-1 : China (Ningxia)
14) us-east-2 : US East (Ohio)
15) ca-central-1 : Canada (Central)
16) eu-west-2 : EU (London)
17) eu-west-3 : EU (Paris)
18) eu-north-1 : EU (Stockholm)
19) ap-east-1 : Asia Pacific (Hong Kong)
20) me-south-1 : Middle East (Bahrain)
(default is 3): 9

Select an application to use
1) Bukkens-Sync
2) [ Create new Application ]
(default is 2): 1
Do you wish to continue with CodeCommit? (y/N) (default is n): n

```
### Deploy code
#### Commit all code first
#### Runing deploy command

```bash
$ eb deploy
Creating application version archive "app-ee24-200318_102026".
Uploading Bukkens-Sync/app-ee24-200318_102026.zip to S3. This may take a while.
Upload Complete.
2020-03-18 03:20:27    INFO    Environment update is starting.
2020-03-18 03:21:06    INFO    Deploying new version to instance(s).
2020-03-18 03:21:27    INFO    Successfully loaded 1 scheduled tasks from cron.yaml.
2020-03-18 03:22:04    INFO    New application version was deployed to running EC2 instances.
2020-03-18 03:22:05    INFO    Environment update completed successfully.

```