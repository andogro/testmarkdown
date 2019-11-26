## Prerequisites
1. You need to generate a private key for ethereum account of Master of Ceremony (MoC), and one more private key for all validators (it will be thrown away after the ceremony, but we need it to launch the nodes, so we can use the same one for all validators). To create new accounts, you can use MetaMask or go to https://www.myetherwallet.com/ It is important that you have private keys in the form of (json keystore file + password).
Additionally, you need to choose a name for your future network, it will be used in network configuration files.
And secret password for netstat server.
You might need to create few more things along the way.

2. Follow this section from another guide https://github.com/poanetwork/wiki/wiki/Validator-Node-on-AWS#validators-node-setup-prerequisites down to "Download and configure playbook"

## Download and configure playbook
0. Clone repositories with playbooks https://github.com/poanetwork/deployment-playbooks
```
git clone https://github.com/poanetwork/deployment-playbooks.git
```

### Launching AWS instances
1. First we need to create instances on AWS
```
cd deployment-playbooks/aws
```

2. Create configuration file for netstat server
```
cp group_vars/all.yml.example group_vars/all.yml
```

3. Edit `group_vars/all.yml`, replace values of the following parameters 
```
access_key: your-AWS-access-key
secret_key: your-AWS-secret-key

awskeypair_name: "keypairname"
region: "us-east-1"
vpc_id: "vpc-ID-number"
vpc_subnet_id: "subnet-ID-number"
```
To save costs for the test, it might be good idea to change all 'instance_type'occurances of "t2.large" to "t2.small"
get from previous step...
go to https://console.aws.amazon.com/vpc/ to get vpc-ID-number & subnet-ID-number

4. Launch the playbook
```
ansible-playbook netstat.yml
```
It should complete without errors and new instance should be created in AWS EC2.
6. You should see new node's IP address in the installation logs or you can get it from AWS, save it for later. <get installation logs?>

how to get in Amazon: 

5. Repeat the same procedure for nodes of MoC, explorer and bootnode
```
ansible-playbook moc.yml
ansible-playbook explorer.yml
ansible-playbook bootnode.yml
```

6. When creating validator nodes, change value of `validator_instance_name` for each new validator, e.g. "validator-1", "validator-2", "validator-3"
```
ansible-playbook validator.yml
# change validator_instance_name and repeat
```

7. By this time you should have 6 nodes running on AWS: 1x for future MoC, 1x netstat, 1x explorer (not blockscout though), 1x bootnode, 3x validators

### Configuring instances
1. Return to root folder of the repository
```
cd ..
```

2. Copy your ssh public key (replace `id_rsa.pub` with correct name)
```
cat ~/.ssh/id_rsa.pub > files/admins.pub
cp files/admins.pub files/ssh_validator.pub
cp files/admins.pub files/ssh_bootnode.pub
cp files/admins.pub files/ssh_moc.pub
```

3. Edit `group_vars/all.yml.network` file
  - replace value of `GENESIS_NETWORK_NAME` with name of the network
  - replace value of `MOC_ADDRESS` with address of MoC that you generated
  - (optionally) replace value of `BLK_GAS_LIMIT` to increase/decrease default block gas limit

4. Choose available subnet in AWS
```
aws ec2 describe-subnets
```
select any subnet with "State": "available" and non-zero "AvailableIpAddressCount". You need to copy/save "SubnetId" of this subnet for later use.

#### Netstat server
1. Create ansible configuration file for netstat node
```
cat group_vars/all.yml.network group_vars/netstat.yml.example > group_vars/all.yml
```

2. Edit `group_vars/all.yml`
  - replace value of `NETSTATS_SECRET` with the secret password for netstat server

3. Create `hosts` file with the following content
```
[netstat]
1.2.3.4

[ubuntu]
1.2.3.4
```
replace 1.2.3.4 with IP address of the netstat server generated previously

4. Launch the playbook
```
ansible-playbook -i hosts site.yml
```
It should complete without errors

5. Open http://1.2.3.4:3000 - you should see an empty dashboard similar to https://core-netstat.poa.network/

#### MoC node
1. Create ansible configuration file for moc node
```
cat group_vars/all.yml.network group_vars/moc.yml.example > group_vars/all.yml
```

2. Edit `group_vars/all.yml` and change values of the following parameters
  - `NODE_FULLNAME` - this name will be displayed in netstat dashboard, e.g. "Master of Ceremony"
  - `NODE_ADMIN_EMAIL` - this email will also be displayed in netstat dashboard, e.g. "moc@example.com"
  - `NETSTATS_SERVER` - insert url http://NETSTAT_IP:3000 of the netstat server
  - `NETSTATS_SECRET` - network secret, same as for netstat server
  - `MOC_KEYPASS` - password for json keystore file of MoC node
  - `MOC_KEYFILE` - here you should include full json string of the keystore file, enclosed in single quotes `'{"version":3,...}'`

3. Create `hosts` file with the following content
```
[moc]
1.2.3.4

[ubuntu]
1.2.3.4
```
replace 1.2.3.4 with IP address of the moc server generated previously

4. Launch the playbook
```
ansible-playbook -i hosts site.yml
```
It should complete without errors

#### Bootnode
1. Create ansible configuration file for bootnode
```
cat group_vars/all.yml.network group_vars/bootnode.yml.example > group_vars/all.yml
```

2. Edit `group_vars/all.yml` and change values of `NODE_FULLNAME`, `NODE_ADMIN_EMAIL`, `NETSTATS_SERVER`, `NETSTATS_SECRET` their meaning is the same as for moc node

3. Create `hosts` file with the following content
```
[bootnode]
1.2.3.4

[ubuntu]
1.2.3.4
```
replace 1.2.3.4 with IP address of the bootnode server generated previously

4. Launch the playbook
```
ansible-playbook -i hosts site.yml
```
It should complete without errors

#### Validator nodes
1. Create ansible configuration file
```
cat group_vars/all.yml.network group_vars/validator.yml.example > group_vars/all.yml
```

2. Edit `group_vars/all.yml` and change values of `NODE_FULLNAME`, `NODE_ADMIN_EMAIL`, `NETSTATS_SERVER`, `NETSTATS_SECRET` their meaning is the same as for moc node
Additionally, change values of
  - `MINING_KEYFILE` - content of validator's json keystore file, enclosed in single quotes
  - `MINING_ADDRESS` - validator address `0x...`
  - `MINING_KEYPASS` - password for the keystore file

3. Create `hosts` file with the following content
```
[validator]
1.2.3.4
5.6.7.8
9.10.11.12

[ubuntu]
1.2.3.4
5.6.7.8
9.10.11.12
```
substituting correct ip addresses of validator servers.

#### Explorer nodes
1. Create ansible configuration file
```
cat group_vars/all.yml.network group_vars/explorer.yml.example > group_vars/all.yml
```

2. There are no configuration changes required

3. Create `hosts` file with the following content
```
[explorer]
1.2.3.4

[ubuntu]
1.2.3.4
```
replacing 1.2.3.4 with correct ip address of the explorer server.

4. Launch the playbook
```
ansible-playbook -i hosts site.yml
```
It should complete without errors for all three nodes.

5. Open http://1.2.3.4:3000 you should see an empty page of the block explorer (but it should work)

#### Erasing the blockchain state
Login to each of the following nodes
  - MoC
  - Bootnode
  - Explorer
  - Validators (3x)
and do the following:

1. stop parity service:
```
sudo systemctl stop poa-parity
```
on explorer node additionally stop
```
sudo systemctl stop explorer # only on explorer node
```

2. erase parity data folder
```
sudo rm -rf /home/ROLE/parity_date/{network,cache}
```
replacing ROLE with `moc`/`bootnode`/`explorer`/`validator` respectively

----

At this moment you should have a bootstrapped configuration files, without any blockchain state, and should be able to proceed with [the main guide](https://github.com/poanetwork/wiki/wiki/How-to-launch-POA-Network-bridged-to-stable-ERC20-coin) starting from **step 1**.
