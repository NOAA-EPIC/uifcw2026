# HSD-training


## Connecting to an HPC Environment via SSH

A Secure SHell (SSH) tunnel creates an encrypted connection between two computer systems. This secure connection allows users to access and use a remote system via the command line on their local machine. SSH connections can also be used to transfer data securely between two systems. Many HPC platforms, including NOAA systems and commercial cloud systems (e.g., AWS, Azure), are accessed via SSH from a user’s computer.

SSH connections require a set of SSH keys, *private* and *public* to be created. A *private*
key remains on your local system. A *public* key is uploaded to the remote machine.

Note the pre-training requirements to join the Slack workspace (<https://join.slack.com/t/epicworkshops-pza9734/signup>) with your e-mail registered for the HSD Training. Then you
join the channels **#uifcw26-training-2-adding-idealized-test-cases** and **#public-ssh-keys-hsd-training**. Your *public* key needs to be uploaded to the second channel. The workshop administrators will add it to the authorization file on the bastion host, which will allow you to log in. (A bastion host, or "jump host," is a secure gateway that SSH connections route through to reach internal systems.)

To connect to a remote HPC environment via SSH, you use an application on your laptop that opens a terminal. The instructions below are given separately for Mac and Windows users.

### Instructions for Mac Users

Open a MacOS terminal application[^1] and type the following commands to generate a public/private key pair on your local system:

```
ssh-keygen -t ed25519 -f /Users/<username>/.ssh/id_ed25519 
```

where <username> is replaced with your actual username.

The output from the command should look like the following lines except substitution with your actual username:

```
Generating public/private ed25519 key pair.
Enter passphrase for "/Users/<username>/.ssh/id_ed25519" (empty for no passphrase): 
Enter same passphrase again:
```

When prompted for a passphrase, press return/enter twice and leave blank. 
This should generate a public/private key pair in the user's home `.ssh` directory.

```
Your identification has been saved in /Users/<username>/.ssh/id_ed25519
Your public key has been saved in /Users/<username>/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:GhEIm283dy9n5vAzdkaPfQ3g7z5C6wLOIRFzF8wYYjo <username>@username-MacBook-Pro
The key's randomart image is:
+--[ED25519 256]--+
|      o....o...  |
|      o=. +..o   |
|      += . .E    |
|      .o     . . |
|      ..S o = = +|
|      .+0. +.o = |
|      .+ o ...O .|
|        o .ooo.* |
|         ..+=+o..|
+----[SHA256]-----+
```

Use a text editor of your choice to view the public key file in the user's home `.ssh` directory (e.g., vim).

For example:
```
vim /Users/<username>/.ssh/id_ed25519.pub
```
(when using vim, press `:q` to quit the editor)

Copy and paste the contents of the public key to the workshop administrator via the Slack workspace channel **#public-ssh-keys-hsd-training**.  

NOTE: There will be two (2) keys generated, a public and a private key. DO NOT SEND THE PRIVATE KEY! A public key (the correct one) will end in `.pub` and will start like this:

```
ssh-ed25519 AAAA3N
```

A private key will look like this:

```
-----BEGIN OPENSSH PRIVATE KEY-----
AAAAAAAAABAAAA
11111111==
-----END OPENSSH PRIVATE KEY-----
```

Next, add the newly generated key to your laptop’s identity by issuing the command:

```
ssh-add ~/.ssh/id_ed25519
```

If successful, you should see a message similar to the following:

```
Identity added: /Users/<username>/.ssh/id_ed25519 (username@MacBook-Pro.local)
```

**Note:** The identity added by `ssh-add` lasts only for your current session.
If you open a new terminal window or restart your Mac, you may need to run the
`ssh-add` command again. To avoid this, add the key to your keychain instead:

```
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

and add the following to `~/.ssh/config`:

```
Host *
    UseKeychain yes
    AddKeysToAgent yes
```

[^1]: The default **Terminal** application supports all command-line commands on the remote host needed for the Training. However, it does
not support the inline-image protocol for displaying resulting plots.  Plot viewing is optional. To enable inline-image capabilities (*imgcat* support), **iTerm2** application could be installed (optional).

#### Testing the SSH connection before the Training event:
Test your SSH keys to confirm successful connection to the HPC environment before the event as shown below:

```
ssh -i ~/.ssh/id_ed25519 test@jump2.epic.noaa.gov
```

If the connection is successful, you should see the following message:

```
It worked! Authorized Key Authentication Successful.
Connection to jump2.epic.noaa.gov closed.
```

#### Logging to your HPC environment during the Training Session:
Now you may access the HPC environment through the bastion host proxy by issuing the following command in the terminal. Use your username confirmed in the Slack **#public-ssh-keys-hsd-training** channel, replacing <firstname>.<lastname> below:

```
ssh <firstname>.<lastname>@jump2.epic.noaa.gov
```

You should be automatically redirected through the bastion proxy host to the controller node of your HPC environment.  

The user may see a message asking whether the user wants to continue connecting.
Verify that you are connecting to the correct system and enter `yes` to continue.

This should automatically redirect users through the bastion proxy to the controller node of their own HPC environment. In this environment, all the participants have their usernames as **ubuntu**.


### Instructions for Windows Users

Ensure that the Windows SSH client (OpenSSH) is installed and configured. Information on how to perform this task can be found here:
https://learn.microsoft.com/en-us/windows/terminal/tutorials/ssh

Open the PowerShell, Command Prompt, or PuTTY application[^2] and run the following command
in a command line of the window terminal:

```
ssh-keygen -t ecdsa
```

The output from the command should look like the following lines except that <username> is replaced with your actual username:
```
Generating public/private ecdsa key pair.
Enter file in which to save the key (C:\Users\<username>\.ssh\id_ecdsa):
```
To accept the default file path, select Enter; otherwise, specify a path or file name for
your generated keys.

Next, you will be prompted to use a passphrase to encrypt your private key files. Leave the passphrase empty by pressing `Enter` twice:
```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\<username>\.ssh\id_ecdsa
Your public key has been saved in C:\Users\<username>\.ssh\id_ecdsa.pub
```

This should generate a public/private key pair in the directory you selected (default or other).
```
The key fingerprint is:
SHA256:AbCdEf123456789exampleFingerprint <username>@WINDOWS-PC
The key's randomart image is:
+---[ECDSA 256]---+
|       .o.       |
|      . + .      |
|     . = +       |
|      + B .      |
|     . S *       |
|      + O .      |
|     . B +       |
|      o.= .      |
|      .+Eo       |
+----[SHA256]-----+
```

Now you have a public/private ECDSA key pair in the specified location. The `.pub` file is the public key, and the file without an extension is the private key.
Use a text editor of your choice to view the public key file or view it in the command line:
```
type C:\Users\<username>\.ssh\id_ecdsa.pub
```

Copy-paste the public key contents to the workshop administrator via the Slack workspace channel **#public-ssh-keys-hsd-training**.

NOTE: Two (2) keys are generated: a public and a private key. DO NOT SEND THE PRIVATE KEY! 
A public key will end in `.pub` and will start something like this:
```
ecdsa-sha2-nistp256 AAAAA
```

And a private key will look like this: 
```
-----BEGIN OPENSSH PRIVATE KEY-----
AAAAAAAAABAAAA
11111111==
-----END OPENSSH PRIVATE KEY-----
```

[^2]: Commonly used PowerShell, Command Prompt, or PuTTY applications support all command-line commands on the remote host needed for the Training. However, they do not support the inline-image protocol for displaying resulting plots. Plot viewing is optional. To enable inline-image capabilities (*imgcat* support), **WezTerm** terminal application could be installed (optional).

#### Testing the SSH connection before the Training event:
Test your SSH keys to confirm successful connection to the HPC environment before the event as shown below, entered from the terminal window:

```
ssh -i C:\Users\<User>\.ssh\id_ecdsa test@jump2.epic.noaa.gov
```
where `C:\Users\<User>\.ssh\` is replaced with the path to the `id_ecdsa` file on the user’s system.

If the connection is successful, you should see the following message:

```
It worked! Authorized Key Authentication Successful.
Connection to jump2.epic.noaa.gov closed.
```

#### Logging to your HPC environment during the Training Session:
Now you may access the HPC environment through the bastion host proxy by issuing the following command in the Powershell/Command Prompt/PuTTY terminal. Use your username confirmed in the Slack **#public-ssh-keys-hsd-training** channel, replacing <firstname>.<lastname> below:

```
ssh -i C:\Users\<User>\.ssh\id_ecdsa <firstname>.<lastname>@jump2.epic.noaa.gov
```
where `C:\Users\<User>\.ssh\` is replaced with the path to the `id_ecdsa` file on the user’s system.

NOTE: This will only work during the training when the HPC system is active for the training!

The user may see a message asking whether the user wants to continue connecting. 
Verify that you are connecting to the correct system and enter `yes` to continue.

This should automatically redirect users through the bastion proxy to the controller node of their own HPC environment. In this environment, all the participants have their usernames as **ubuntu**.

## Quick Reference for Terminal Command-line Commands Used in the Training Session

```
ssh <firstname>.<lastname>@jump2.epic.noaa.gov
cd /scratch
mkdir <username>
cd <username>
git clone --recursive -b feature/2026-HSD-training https://github.com/NOAA-EPIC/ufs-weather-model.git 
cd ufs-weather-model
export SRC_DIR=$PWD
# $SRC_DIR for convenience and later reference
cd tests-dev
# Run a control case for the aquaplanet:
./ufs_test.sh -a ubuntu -s -c -k -r -n "aquaplanet intel"

# Open another terminal window (and SSH to your HPC cluster)
cd /scratch/<username>/ufs-weather-model
export SRC_DIR=$PWD
cd tests-dev
# During the compile:
ll run_dir/compile_atm_dyn32_intel/out
# Monitor the progress:
tail -f run_dir/compile_atm_dyn32_intel/out # exit with ^C
# Check last 20 lines of the output file:
tail -n 20 run_dir/compile_atm_dyn32_intel/out
# View the entire file:
view run_dir/compile_atm_dyn32_intel/out # exit with :q

# During the model run:
ll run_dir/aquaplanet_intel/out
# Monitor the progress:
tail -f run_dir/aquaplanet_intel/out # exit with ^C
# Check last 50 lines of the output file:
tail -n 50 run_dir/aquaplanet_intel/out 
# View the entire file:
view run_dir/aquaplanet_intel/out  # exit with :q

# After the run has finished:
cd run_dir/aquaplanet_intel
export RUN_DIR=$PWD # for reference later
ls sfcf* atmf*
# Store the control run results:
mkdir orig-results-000
mv log* atmf* sfcf* out err orig-results-000/

# EXERCISE 1: 
# Make changes in the run directory
# Use vi/vim or another text editor of your choice (nano, gedit, ed)
vim input.nml   
sbatch job_card
squeue -u $USER
tail -f out   # exit with ^C
view out      # exit with :q
ll atmf* sfcf*
mkdir orig-results-001
mv log* atmf* sfcf* out err orig-results-001/

# Plot Exercise 1 results from $RUN_DIR
# cd $RUN_DIR        # if needed
cp $SRC_DIR/tests-dev/test_cases/utils/plot-test* .
ll plot-test-*
./plot-test-1.sh
ll *.png
# If your local terminal supports inline-image protocol:
imgcat prec-control.png # or other graphics file


# EXERCISE 2: 
# Make changes in model source code; recompile, run the new executable
echo $RUN_DIR
echo $SRC_DIR
cd $SRC_DIR
# Use vi/vim or another text editor of your choice (nano, gedit, ed)
vim UFSATM/ccpp/physics/physics/Radiation/radiation_astronomy.f
module use $PWD/modulefiles
module load ufs_awspc.intel
# Compile the model:
mkdir build
cd build
cmake .. -DAPP=ATM -DCCPP_SUITES=FV3_GFS_v17_p8_ugwpv1 -D32BIT=ON -DMPI=ON -DCMAKE_BUILD_TYPE=Release
ll Makefile
make -j 6
cp ufs_model $RUN_DIR/fv3.exe
# Rerun the model with changes in the source code
cd $RUN_DIR
vim input.nml
sbatch job_card
squeue -u $USER
tail -f out   # exit with ^C
grep "SOLAR CONSTANT" out
view out      # exit with :q
ll atmf* sfcf*
mkdir orig-results-002
mv log* atmf* sfcf* out err orig-results-002/
# cp $SRC_DIR/tests-dev/test_cases/utils/plot-test* .  # already done
ll plot-test-*
./plot-test-2.sh
ll *.png
# If your local terminal supports inline-image protocol:
imgcat prec-control.png # or other graphics file
