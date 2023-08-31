STAGE 1 : Configure Security & Create a CodeCommit Repo
STAGE 2 : Configure CodeBuild to clone the repo, create a container image and store on ECR
STAGE 3 : Configure a CodePipeline with commit and build steps to automate build on commit.
STAGE 4 : Create an ECS Cluster, TG's , ALB and configure the code pipeline for deployment to ECS Fargate

# STAGE ONE

1	To create and configure an IAM user for accessing CodeCommit

Create an IAM user, or use an existing one, in your
 Amazon Web Services account. Make sure you have an access key ID and
 a secret access key associated with that IAM user

In the IAM console, in the navigation pane, choose Users, and then
 choose the IAM user you want to configure for CodeCommit access.

On the Permissions tab, choose Add Permissions.

In Grant permissions, choose Attach existing policies directly.

From the list of policies, select AWSCodeCommitPowerUser or another managed policy for 
CodeCommit access. For more information, see AWS managed policies for CodeCommit.

After you have selected the policy you want to attach, choose Next: Review to review 
the list of policies to attach to the IAM user. If the list is correct, choose Add permissions.

Step 2:	 Install Git

3	Set up the public and private keys for Git and CodeCommit

From the terminal on your local machine, run the ssh-keygen command, and follow the directions to save the file to the .ssh directory for your profile.
$ ssh-keygen OR ssh-keygen -t rsa -b 4096

Generating public/private rsa key pair.
Enter file in which to save the key (/home/user-name/.ssh/id_rsa): Type /home/your-user-name/.ssh/ and a file name here, for example /home/your-user-name/.ssh/codecommit_rsa

Enter passphrase (empty for no passphrase): <Type a passphrase, and then press Enter>
Enter same passphrase again: <Type the passphrase again, and then press Enter>

Your identification has been saved in /home/user-name/.ssh/codecommit_rsa.
Your public key has been saved in /home/user-name/.ssh/codecommit_rsa.pub.
The key fingerprint is:
45:63:d5:99:0e:99:73:50:5e:d4:b3:2d:86:4a:2c:14 user-name@client-name
The key's randomart image is:
+--[ RSA 2048]----+
|        E.+.o*.++|
|        .o .=.=o.|
|       . ..  *. +|
|        ..o . +..|
|        So . . . |
|          .      |
|                 |
|                 |
|                 |
+-----------------+

This generates:

The codecommit_rsa file, which is the private key file.

The codecommit_rsa.pub file, which is the public key file.

Run the following command to display the value of the public key: cat ~/.ssh/codecommit_rsa.pub

In the IAM console, in the navigation pane, choose Users, and from the list of users, choose your IAM user.

On the user details page, choose the Security Credentials tab, and then choose Upload SSH public key.

Paste the contents of your SSH public key into the field, and then choose Upload SSH public key.

Copy or save the information in SSH Key ID (for example, APKAEIBAERJR2EXAMPLE).

On your local machine, use a text editor to create a config file in the ~/.ssh directory, and then add the following lines to the file, where the value for User is the SSH key ID you copied earlier:


Host git-codecommit.*.amazonaws.com
  User APKAEIBAERJR2EXAMPLE
  IdentityFile ~/.ssh/codecommit_rsa

From the terminal, run the following command to change the permissions for the config file:


chmod 600 config

Run the following command to test your SSH configuration:


ssh git-codecommit.us-east-2.amazonaws.com


Step 4: Connect to the CodeCommit console and clone the repository
Open the CodeCommit console at
Copy the SSH URL if you are using an SSH public/private key pair with your IAM user.
run this command in your local repo  git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo

