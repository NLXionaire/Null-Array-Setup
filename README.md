![NulleX-Logo](https://pbs.twimg.com/media/DhAvsGDX0AAZJ92.jpg)
# Nullex Null Array Verifier (NAV) Guide (Ubuntu 16.04)
This guide will assist you in setting up a NulleX NAV on a Linux Server running Ubuntu 16.04. (Use at your own risk)

If you require further assistance contact the support team on [Discord](https://discord.gg/6q7VWpN) or [Telegram](https://t.me/joinchat/ICJpaEhYCYtqic9X1KM7Ew)

***
## Requirements
1) **50,000 NulleX coins per NAV**
2) **VPS running Linux Ubuntu 16.04**
(Vultr is what's used in my guide, but you are free to use whatever you want)
3) **Windows local wallet**
4) **An SSH client such as [Putty](https://www.putty.org/)**
***
## Contents
* **Section A**: Creating the VPS within [Vultr](https://www.vultr.com/?ref=7473052).
* **Section B**: Downloading and installing [Putty](https://www.putty.org/).
* **Section C**: Connecting to the VPS and Manually Installing the NAV via Putty.
* **Section D**: Preparing the local wallet.
* **Section E**: Connecting & starting the NAV.
***

## Section A: Creating the VPS within [Vultr](https://www.vultr.com/?ref=7473052) 
***Step 1***
* Register at [Vultr](https://www.vultr.com/?ref=7473052)
***

***Step 2***
* After you have added funds to your account go [here](https://my.vultr.com/deploy/) to create your server
***

***Step 3*** 
* Choose a server location (preferably somewhere close to you)
![Example-Location](https://i.imgur.com/ozi7Bkr.png)
***

***Step 4***
* Choose a server type: Ubuntu 16.04
![Example-OS](https://i.imgur.com/aSMqHUK.png)
***

***Step 5***
* Choose a server size: $5/mo will be fine 
![Example-OS](https://i.imgur.com/UoGoHcM.png)
***

***Step 6***
* Enable IPv6 and Private Networking  ***(there is a different cmd listed below for those using IPv4)***
![Example-OS](https://imgur.com/EYhmnUx.png)
***

***Step 7*** 
* Set a Server Hostname & Label (name it whatever you want)
![Example-hostname](https://i.imgur.com/uu0rvOr.png)
***

***Step 8***
* Click "Deploy Now"

![Example-Deploy](https://i.imgur.com/4qpYuH0.png)
***


## Section B: Downloading and Installing Putty

***Step 1***
* Download Putty [here](https://www.putty.org/)
***

***Step 2***
* Select the correct installer depending upon your operating system. Then follow the install instructions. 

![Example-Putty Installer](https://imgur.com/wqfWyvg.png)
***


## Section C: Connecting to the VPS & Manually Installing the NAV via Putty

***Step 1***
* Copy your VPS IP (you can find this by going to the server tab within Vultr and clicking on your server. 
![Example-Vultr](https://i.imgur.com/z41MiwY.png)
***

***Step 2***
* Open the Putty application and fill in the "Host Name" box with the IP address of your VPS.
![Example-PuttyInstaller](https://imgur.com/WffwFYW.png)
***

***Step 3***
* Once you have hit enter it will open a security alert (click yes).
![Example-RootPassEnter](https://imgur.com/x7ILFUy.png)
***

***Step 4***
* Type "root" as the login/username.
![Example-Root](https://imgur.com/S0fcGzm.png)
***

***Step 5***
* Copy the root password from the VULTR server page.

![Example-RootPassEnter](https://i.imgur.com/JnXQXav.png)
***

***Step 6*** 
* Paste the password into the Putty terminal by right clicking (it will not show the password so just press enter).
![Example-RootPassEnter](https://imgur.com/65jWobg.png)
***

***Step 7***
* Adduser to secure your VPS, by adding a new username so you aren't running everything as root.  
* Run the command below and use a username that you want associated with this server ***('nullexnav1' is an example, you need to use your own username).***  

`adduser nullexnav1`

***

***Step 8***

* You will then need to make a new password associated with your new username.  Hit 'Enter' for the next 5 lines after your new password has been set, then 'Y' to save your information.

![Example-Bash](https://imgur.com/mrhihrj.png)

***

***Step 9***

* The final step is granting sudo permissions for your adduser.  Run the command below to activate this setting ***('nullexnav1' is an example, you need to use your username).***

`usermod -aG sudo nullexnav1`

* Now you will close your Putty terminal and log back with your new username and password that we made above.  

![Example-Bash](https://imgur.com/FUFkEFu.png)

* ***KEEP YOUR NEW USERNAME AND PASSWORD IN A SAFE PLACE.  THIS WILL BE HOW YOU LOG IN FROM NOW ON.***
***

***Step 10***

Visit [https://github.com/white92d15b7/NLX/releases](https://github.com/white92d15b7/NLX/releases) to find a link to the newest linux wallet and replace the url in the command below with the proper linux wallet download url:

`wget https://github.com/white92d15b7/NLX/releases/download/1.3.6.1/nullex-1.3.6.1-x86_64-linux-gnu.tar.gz`

***Step 11***

Extract the contents of the downloaded wallet file with the following command (be sure to change the filename to match the version you are installing):

`tar -zxvf nullex-1.3.6.1-x86_64-linux-gnu.tar.gz`

**Step 12***

Copy the cli and daemon files to the `/usr/local/bin` directory to make accessing the wallet easier.

`sudo find $PWD -type f \( -name "nullexd" -o -name "nullex-cli" \) -exec cp {} /usr/local/bin/ \;`

***Step 13***

Temporarily open the wallet to generate the config file.

`nullexd -daemon`

***Step 14***

Generate a new genkey value (required for the config file). Save the output of this file in a text file for later.

`nullex-cli masternode genkey`

![Example-console](https://i.imgur.com/qD7yzwG.jpg)

***Step 15***

Close the wallet. You can do this immidiately after opening.

`nullex-cli stop`

***Step 16***

Edit the config file.

`nano ~/.nullexqt/NulleX.conf`

Add the following lines:

```
rpcuser={RPC_USERNAME}
rpcpassword={RPC_PASSWORD}
rpcallowip=127.0.0.1
rpcport=46001
listen=1
server=1
daemon=1
externalip={VPS_IP_ADDRESS}:43879
masternode=1
masternodeaddr={VPS_IP_ADDRESS}:43879
masternodeprivkey={GENKEY_VALUE}
```

Replace the following in the above config:

*{RPC_USERNAME}:* This can be any username you want. You do not have to remember it.
*{RPC_PASSWORD}:* This can be any password you want. You do not have to remember it.
*{VPS_IP_ADDRESS}:* This is the public ip address of the vps. Usually the same address you used to connect via putty from Step 1.
{GENKEY_VALUE}:* This is the genkey value you saved from step 14.

Once the config file is setup correctly you can save and exit the editor with the following key sequences: *CTRL+X*, *Y*, *ENTER*

***Step 17***

Start the wallet again with the new configuration changes.

`nullexd -daemon`

## Section D: Preparing the Local Wallet

***Step 1 (if neccessary)***
* Download and install the NulleX wallet [here](https://github.com/white92d15b7/NLX/releases/download/1.3.6/nullex-qt.exe)
***

***Step 2***
* Create a text document to temporarily store information that you will need. 
***

***Step 3***
* Go to the debug console within the wallet, type the command below and press enter.
* NAV1 is used as an example, name your NAV whatever you want.

`getaccountaddress <NAV1>`

![Example-console](https://i.imgur.com/6NM7G9a.png)
***

***Step 4***
* Send EXACLY 50,000 NulleX to the receive address within your wallet that was generated in Step 3.  Do not send any extra for fees, click ***Choose*** (right next to ***Transaction Fee***) and check the box for ***Send as zero-fee transaction***.

![Example-console](https://imgur.com/FBVOnyA.png)
***

***Step 5***
* Go to the debug console within the wallet 

![Example-console](https://i.imgur.com/6NM7G9a.png)
***

***Step 6***
* Type the command below and press enter 

`masternode outputs` 

![Example-outputs](https://i.imgur.com/GD7Ro1m.png)
***

***Step 7***
* Copy the long key (this is your transaction ID) and the 0 or 1 at the end (this is your output index)
* Paste these into the text document you created earlier as you will need them in the next step.

![Example-outputs](https://imgur.com/Z9lsQyd.png)
***

# Section E: Connecting & Starting the NAV

***Step 1***
* Go to the tools tab within the wallet and click open "masternode configuration file" 

![Example-create](https://i.imgur.com/COsfvfA.png)
***

***Step 2***
* Start a new line for each NAV at the bottom, and make it look like the example on the last line of existing info, but with your info.

 `'Alias' 'VPS IP Address:Port' 'Genkey' 'TxHash' 'Output Index'` ***(Do not use a # at the beginning)***

* For `Alias` use the one you made for ***Section D: Step 3***
* The `VPS IP Address:Port` is the IP from ***Section C: Step 1*** and the dedicated Port is ***43879***.
* The `GenKey` is your masternode genkey from ***Section C: Step 14***.
* The `TxHash` is the transaction ID/long key that you copied to the text file from ***Section D: Step 6 and 7***.
* The `Output Index` is the 0 or 1 that you copied to your text file from ***Section D: Step 6 and 7***.

![Example-create](https://imgur.com/tMkdY7h.png)
* Click "File Save"
***

***Step 3***
* Check the status of your VPS wallet to make sure it's on the same block as your controller wallet.

`nullex-cli getblockcount`

![Example-console](https://imgur.com/B4Wk0in.png)
***

***Step 4***
* Close out of the controller wallet and reopen it
* ***UNLOCK YOUR WALLET TO START YOUR NAV!!!!!!!!!!!!!!***
* Go to the debug console within the wallet, type the command below and press enter

`startmasternode alias false YOUR-NAV-ALIAS`

![Example-console](https://i.imgur.com/6NM7G9a.png)

![Example-console](https://imgur.com/86XpkOc.png)
***

***Step 5***
* Check the status of your NAV within the VPS by using the command below in Putty terminal:

`nullex-cli masternode status`

* You should see ***status 4 or 9***

![Example-console](https://imgur.com/W1ZAMTk.png)

If you do, congratulations! You have now setup a NAV. If you do not, please contact support on [Discord](https://discord.gg/6q7VWpN) or [Telegram](https://t.me/joinchat/ICJpaEhYCYtqic9X1KM7Ew) and they will be happy to assist you.
***
