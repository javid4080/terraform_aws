# terraform_aws_kubernetes

# Prerequisite's

	1. terraform version 12 : Click here to download.
	2. AWS Account : Click here to create a free tier AWS account.
	3. PuTTy, PuTTygen, Pageant : Click here to download .
	4. Git Bash : Click here to download.
	
	
# HowToStart

	1. Open Git Bash or any equivalent terminal  and  export `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`
	2. Make sure `terraform`  cli is in path.
	3. `git clone https://github.com/rbhokare145/terraform_aws.git`
	4. `cd /terraform_aws/dev/dev.tfvars`  : Update the user_vars as per requirements.
	5. Run terraform init
	6. Run terraform plan -var-file=dev.tfvars 
	7. Run terraform apply -var-file=dev.tfvars
	8. Enter user ip-address range to allow an ec2 instances access externally. On Google type  my-ip
	and then pass the ip range as for ex. 123.201.100.40/32
	


# Issue1
while making use of terraform resource `provision --> connection`, getting error "Failed to read ssh private key: no key found"
although the key does present on the given path.

# Reason : terraform v0.12 does not support private/public keys generated by PuTTygen those are in `.ppk` format.
https://aws.amazon.com/premiumsupport/knowledge-center/convert-pem-file-into-ppk/

# Solution :
1) To generate open ssh keys --> https://compbio.cornell.edu/about/resources/linux-ssh-keys-and-ssh-key-generation/
   ssh-keygen -t rsa -b 2048  (do not specify any passphrase while key creation, as terraform again not support encrypted private keys)
2) Step#1 does solve the problem for terraform to login on ec2 and execute given provision instructions
   but, its not possible to login to an instance by using the open-ssh keys which are in .pem format. thus
3) Make use of PuTTy-Gen to convert the given private key (which is in .pem format) to .ppk
4) Open PuTTygen --> click on Conversions -> import key --> and then same private key (.ppk) and public key
5) Make use of the available .ppk private key to login inside ec2 by using putty


# Issue2

I have seen this issue when creating shell scripts in Windows env using pycharm editor and then porting over to run on a Unix environment.

The script execution getting failed with "bin/bash^M: bad interpreter: No such file or directory"

# Reason : Unix uses different line endings so can't read the file you created on Windows. Hence it is seeing ^M as an illegal character.

If you want to write a file on Windows and then port over, make sure your editor is set to create files in UNIX format.


# Solution :

1) In notepad++ in the bottom right of the screen, it tells you the document format. By default, it will say Dos\Windows.   To change it go to
2) Pycharm LineSeperator to change to Unix 
Try running dos2unix on the script http://dos2unix.sourceforge.net/
