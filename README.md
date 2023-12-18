# github_action101
                                                  HOW TO AUTOMATE GITHUB ACTIONS THAT LAUNCHES AN APACHE SERVER USING TERRAFORM SCRIPT
What are github actions? Simply put, a way to automate infra provisioning such that the slighest code integration would tigger a new build, testing and deployment if so configured.
                                                                                   Step 1: Create a Repository
The first step is to create a repository; you can do so by going to your github homepage, click on "Repositories" and click [New]. You'd assign the repository a name, choose either public or private and select create repository. In this case, i have chosen public.
                                                                                   Step 2: Clone the Repo
To clone a repo, simply click on the repo and click the green button tagged "<>Code" to open it. Under "Local", Select ssh and copy the url link. Take the link to your local working directory and use 'git clone' command to clone the repo. e.g git clone git@github.com:adebayoadebusoye1/ApacheServer_git-Action.git 
                                                                                   Step 3: Create a .git directory and a workflows sub-directory
The third step is to create a directory; .git directory and workflows subdirectory -  the command to create a directory is mkdir .git/workflows. 
                                                                                   Step 4: Create a .yaml file that will set the parameters
At this time, you'd create a yaml file with which you will set up the environment. Create a GitHub Actions workflow file that automates the provisioning of an EC2 instance using Terraform. There are certain parameters that must be customized to enable the provisioning infrastructure using your own aws account. For example, 
Name - you must give the file a name
description - provide a description that fits the use case
required - set this value to true
default - a name value that fits the use case
type - (set this to string)
runs on: (set this to ubuntu-latest or whichever operating system you're going to use) 
steps: under this column, you have "uses": type actions/checkout@v3
                                    "uses": type actions/setup-node@v3
node-version: set this to '14'
name - type Configure AWS Credentials
uses - type aws-actions/configure-aws-credentials@v1
aws-access-key-id: (must be in this format.... '${{ secrets.ACCESS_KEY }}'.........you must configure an Aws IAM profile and generate an access_key and secret_access_key. 
aws-secret-access-key: '${{ secrets.SECRET_ACCESS_KEY_ID }}' 
Note: You must set your variables on the repository to allow access to your aws account. To do this, click on your repo and go to the settings. (Repo settings, not github settings). Go to secrets and variables on the left panel, click on Actions and scroll down to repository secrets....there you'd click on 'new repository secret'....for name, youd enter your variables i.e 1. Access_Key_ID, 2. Secret_Access_key and 3. Aws_Region. under 'secret' for each value, you'd enter the respective value you're importing from AWS IAM profile, i.e the access_key and secret_access_key and aws_region value and click add secret.
aws-region: set this to your preferred region (e.g US-east-1)
name: Setup Terraform
uses: hashicorp/setup-terraform@v2
terraform_wrapper: false
name: Terraform Apply
id:   apply
(under environment you have)  TF_VAR_ec2_name: type "${{ github.event.inputs.ec2_name }}"
run:
cd terraform-example/ (Note: this folder name "terraform-example" may differ depending on what you called it)
          terraform init
          terraform validate
          terraform plan 
          terraform apply -auto-approve
                                                                               Step 5: Create the folder where your terraform files will be resident
this is when you'd create the "terraform-example" folder i referenced above. Again, you can call it whatever you like. Within this folder, you'll have your terraform files, such as main.tf, variable.tf or provider.tf that you'll use to provision the resources in the desired cloud platform, under your account.  


                                                        How to Manually launch the automation configuration
From github home click on the github actions repository. Then click on 'Actions'.
On the left pane, under 'Actions', select 'All Workflows'. Select the workflow you want to provision. Click on 'Run Workflow' on the right hand side of the page. A display will pop-up with the 'run workflow' button. Click on run workflow (you may select the branch you want to run workflow from; in this case it's the main). 

 The name you gave your workflows file should pop-up which is an indication that the manual launch is in progress. You may click on it....then click on 'provision-ec2'. There you should see a list of tasks that the system is performing, starting from 'set up job' all-the-way to 'complete job'. You can click them one-by-one to see step by step what was being done. In the event that there is an error, you can click on the stage where the error is to see what it is and how to go about rectifying it.

