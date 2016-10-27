# How to use CNL-Brain1
**IP address: 155.198.108.197**

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [How to use CNL-Brain1](#how-to-use-cnl-brain1)
	- [How to connect](#how-to-connect)
		- [Via SSH](#via-ssh)
		- [With a remote desktop](#with-a-remote-desktop)
		- [Connect with OpenVPN](#connect-with-openvpn)
			- [MacOS X](#macos-x)
			- [Windows](#windows)
			- [Fedora](#fedora)
			- [Generic Linux](#generic-linux)
			- [iOS and Android](#ios-and-android)
		- [Use matlab](#use-matlab)
		- [Use iPython/Jupyter notebook](#use-ipythonjupyter-notebook)
	- [Parallel Computing](#parallel-computing)
		- [Run a script in parallel](#run-a-script-in-parallel)
- [Cluster specifications](#cluster-specs)
<!-- /TOC -->


## How to connect

### Via SSH
1. If your are not on imperial network, connect to the VPN (if you have trouble here, see below: connect with OpenVPN.

2. ssh <yourusername>@155.198.108.197

3. Use the ‘screen’ function to restore or create a session
  * to create a session:
     `screen -S <session_name>`

  * to restore a session: `screen -r <session_name>`

  * to list available sessions: `screen -ls`

  * to detach from a session (which lets you restore it later): <kbd>CTRL</kbd>+<kbd>A</kbd>+<kbd>D</kbd>

  * to delete/exit a session: exit


* Optional: Configure ssh without password
http://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/
  - `ssh-keygen`  (without passphrase)
  - `ssh-copy-id -i ~/.ssh/id_rsa.pub <yourusername>@155.198.108.197` (https://github.com/beautifulcode/ssh-copy-id-for-OSX)
  - Then try: `ssh <yourusername>@155.198.108.197`

### With a remote desktop

* Install x2goclient:
http://wiki.x2go.org/doku.php/start

* Configure a new session like the one below (with your username)

![alt text](/CNL-Brain1/images/image03.png)

### Connect with OpenVPN

Imperial College is switching to OpenVPN, which may be more reliable that the PPTP VPN that was previously recommended. Check this section if you have troubles connecting with the PPTP VPN.

#### MacOS X

1. Download Tunnelblick from here and install it: https://tunnelblick.net/

2. Download the OpenVPN config from here: https://openvpn.ic.ac.uk/ic.ovpn

3. If Tunnelblick is correctly installed, you should be able to double-click on the config file and connect after entering your username/password. Disconnect using the app window

#### Windows

1. Download the OpenVPN client from here: https://openvpn.net/index.php/open-source/downloads.html

2. Download the OpenVPN config from here: https://openvpn.ic.ac.uk/ic.ovpn

3. Either:

* place this file in c:\program files\openvpn\config, run "OpenVPN GUI" from the start menu, then right-click the icon on the status bar to connect. Diesconnect using the app window or right-click on the status bar.

* place this file on the desktop and double-click it, enter your password in the terminal window when prompted. Close the terminal window to disconnect

#### Fedora
Fedora Linux with Gnome 3/NetworkManager - doesn't allow use of OpenVPN profiles for some reason

1. Ensure the openvpn client is installed with your system package manager

2. Go to Activities at the top-right and type "Network", selecting "Network - control how you connect to the internet"

3. Hit the + icon to add a connection, and select VPN, OpenVPN

4. Use the following settings:

* gateway: openvpn.ic.ac.uk

* authentication: password

* CA certificate: download and select this file: https://openvpn.ic.ac.uk/icov.pem

* Ensure "Routes - automatic" is set under IPv4

5. Connect to and disconnect from the VPN using the NetworkManager GUI

#### Generic Linux

1. Ensure the openvpn client is installed with your system package manager

2. Download the OpenVPN config from here: https://openvpn.ic.ac.uk/ic.ovpn

3. Open a terminal window

4. Run "openvpn ic.ovpn" where the 2nd argument is the path/file you downloaded in step 2

5. Disconnect by killing the command at the terminal, by hitting ctrl+c

#### iOS and Android

1. Downlaod the OpenVPN connect app from:

* iOS - itunes: https://itunes.apple.com/gb/app/openvpn-connect/id590379981?mt=8

* Android - Google Play store: https://play.google.com/store/apps/details?id=net.openvpn.openvpn&hl=en_GB

2. Download the OpenVPN config from here: https://openvpn.ic.ac.uk/ic.ovpn

3. Open the OpenVPN connect app and import the config file

4. Connect using the on-screen prompts


### Use matlab


1. Go on the matworks website to create a new license file: https://uk.mathworks.com/licensecenter?s_tid=mwa_com_vwlc_cta2
2. Click activate
![alt text](/CNL-Brain1/images/image02.png)
3. Enter this host ID: `DC4A3E655430`

4. Enter the same username as the one you use to login to the computer
![alt text](/CNL-Brain1/images/image01.png)
5. Download the license file
![alt text](/CNL-Brain1/images/image01.png)
6. ssh into the computer
7. Create a folder name R2016a_licenses in the .matlab folder:

  `mkdir ~/.matlab/R2016a_licenses`
8. copy the file into the folder ~/.matlab/R2016a_licenses/
For example:

  `scp ~/Downloads/license.lic <yourusername>@155.198.108.197:~/.matlab/R2016a_licenses/`

7. That should work!

### Use iPython/Jupyter notebook

You can tunnel the ipython notebook to your local machine:

* set up the ssh tunnel:

  `ipython notebook --no-browser --port=1234`

* locally, run:

  `ssh -N -f -L localhost:1111:localhost:1234 remote_user@remote_host`

  (replace with your usnername and the server address)

Now, you can open your browser at http://localhost:1111 and run your code as if it were on the local machine.


## Parallel Computing

### Run a script in parallel

Run a script in parallel
Create a script file called for example script.sh.
Make the file executable :  chmod +x script.sh



An example of a script file:

```bash
#!/bin/bash

printf '=%.0s' {1..100}
print ‘Begin the simulation’
printf '=%.0s' {1..100}

thr=20
trap "exit" INT
parallel --bar -j $thr --header : ./cortex -N 100 -ext _0.txt -d1 10 -d2 30000 -d3 10 -before 10 -after 30000 -S 100 -G 10 \
        -s 60 -WII 1100 -LTP 0.0282 -LTD 0.00188 -model gp-izh-subnetworks \
        -r 0 -global 0 -sG {SG} -sWII {WII} -tauv {TAUV}\
         ::: TAUV `seq 15 2 95` ::: SG `seq 0 2 30` ::: WII 10 50 0


printf '=%.0s' {1..100}
echo ‘Done!’
printf '=%.0s' {1..100}

exit 0
```


It uses the GNU parallel function on 20 cores. Replace thr=20 by thr=+0 if you want to use all the cores. Use ‘htop’ to view the CPU usage.

When running the script, you can use the ‘nice’ function to set the priority (goes from -20 (highest priority) to 19 (lowest priority) )
For ex:
```bash
nice -10 ./script.sh
```

(it will set the priority to 10, to set to -10, use --10 instead)


Run a matlab function from a unix shell:
```matlab
matlab -nodisplay -nodesktop -nojvm -nosplash -r  "try double2(2); catch; end; quit"
```

# Cluster specs

* CPU 1: Intel Xeon E5-2680v4 2.4 2400 14C 
* CPU 2: Intel Xeon E5-2680v4 2.4 2400 14C 
* System Memory: 256GB DDR4-2400 (8x32GB) 
* GPU 1: NVIDIA Quadro K420 2GB DL-DVI(I)
* GPU 2: NVIDIA Titan X 12GB