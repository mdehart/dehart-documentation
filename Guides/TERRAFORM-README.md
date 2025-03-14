<h1 align="center">
  <img src="./assets/bb-1.png" alt="BBLogo" width="250" /></br></br>
  <strong style="font-size:60px;">Terraform</strong>
</h1></br>

# Mac Installation
Download the Teffaform binary zip from Hashicorp, unzipping it and copying the binary to a place in the path.

1. Log in to the system and create a folder for the Terraform executable:
`sudo mkdir -p /opt/terraform`

2. Navigate to the new folder:
`cd /opt/terraform`

3. From the Terraform downloads website, copy the macOS Terraform zip file download link.

4. Download the zip file in the previously created folder:
`sudo curl -O <copied_url>`

5. Unzip the Terraform files in the current folder:
`sudo unzip <zip_file_name>`

The unzipped files will be in the current folder.

6. Add Terraform to the PATH. Navigate to the home folder and execute this command to open and edit the bash profile:
`sudo vi ~/.bash_profile`

Add these two lines on the opened file:
```
PATH="/opt/terraform:${PATH}"
export PATH
```

7. Execute the file for the path changes to get into effect:
`source ~/.bash_profile`

8. Now that Terraform is installed, verify the Terraform version by running
`terraform –version`

This should show the Terraform version, which is installed in the system.

That completes your MacOs installation of Terraform

# Basic Commands
## Show version
`terraform -V`

## Init Terraform
The following command initializes the terraform module. This will install:
• terraform modules
• eventually a backend
• and provider(s) plugins
and allow you to run a plan, apply, import, and destroy
`terraform init`

## Init Terraform and don't ask any input
`terraform init -input=false`

## Change backend configuration during the init
`terraform init -backend-config=cfg/s3.dev.tf - reconfigure`
`-reconfigure` is used in order to tell terraform to not copy the existing state to the new remote state location.

## Get
This command is useful when you have defined some modules. Modules are vendored so when you edit them, you need to get again modules content.
`terraform get -update=true`
When you use modules, the first thing you'll have to do is to do a terraform get. This pulls modules into the terraform directory. Once you do that, unless you do another `terraform get - update=true`, you've essentially vendored those modules.

## Plan
The plan step check configuration to execute and write a plan to apply to target infrastructure provider:
`terraform plan -out plan.out`
It's an important feature of Terraform that allows a user to see which actions Terraform will perform prior to making any changes, increasing confidence that a change will have the desired effect once applied. When you execute terraform plan command, terraform will scan all `*.tf` files in your directory and create the plan.

## Apply
Now you have the desired state so you can execute the plan:
`terraform apply plan.out`
Since terraform v0.11+, in an interactive mode (non Cl/CD/autonomous pipeline), you can just execute terraform apply command which will print out which actions TF will perform. By generating the plan and applying it in the same command, Terraform can guarantee that the execution plan won't change, without needing to write it to disk. This reduces the risk of potentially-sensitive data being left behind, or accidentally checked into version control.
`terraform apply`

## Apply and auto approve
`terraform apply -auto-approve`

## Apply and define new variables value
```
terraform apply -auto-approve
-var tags-repository_url=$(GIT_URL}
```

## Apply only one module
`terraform apply -target=module.s3`
This `-target` option works with `terraform plan` too.

## Destroy
`terraform destroy`
Delete all the resources!

A deletion plan can be created before:
`terraform plan -destroy`

`-target` option allow to destroy only one resource, for example a S3 bucket :
`terraform destroy -target aws_s3_bucket.my_bucket`

## Debug
The Terraform console command is useful for testing interpolations before using them in configurations. Terraform console will read configured state even if it is remote.
`echo "aws_iam_user.notif.arn" | terraform console arn:aws:iam:123456789:user/notif`

## Logs level
Set the log to DEBUG level and save the log in an output external file.
`IF_LOG_PATH=mylogfile.txt IF_LOG=debug terraform apply`

## Graph
`terraform graph | dot -Tpng > graph.png`
Visual dependency graph of terraform resources.

## Validate
Validate command is used to validate/check the syntax of the Terraform files. A syntax check is done on all the terraform files in the directory, and will display an error if any of the files doesn't validate. The syntax check does not cover every syntax common issues.

# Example Terraform Files
There are several files are required for terraform to be used correctly. 
1. main.tf – containing the resource blocks that define the resources to be created in the target cloud platform.
2. variables.tf – containing the variable declarations used in the resource blocks.
3. provider.tf – containing the terraform block, s3 backend definition, provider configurations, and aliases.
4. output.tf – containing the output that needs to be generated on successful completion of “apply” operation.
5. *.tfvars – containing the environment-specific default values of variables.

## main.tf
The main.tf is where the resource blocks are declared for what resources are to be created/destroyed. An example file is below:
```
resource "provider_integration" "example_aws" {
  name                        = "Custom"
  integration_definition_id   = "8013680b-311a-4c2e-b53b-c8735fd97a5c"
  polling_interval            = "THIRTY_MINUTES"
  description                 = "Custom integration"

  config = {
  }
}


resource "provider_integration" "example_custom_file_transfer" {
  name                      = "Custom File Transfer"
  integration_definition_id = "4c66100d-3771-473d-99ce-cbe638b5ab50"
  polling_interval          = "THIRTY_MINUTES"
  description               = "Custom integration"

  config = jsonencode({
    "@tag" = {
      "AccountName" = "Custom",
    },
    "entities" = [
      {
        "id" = "Test",
        "uniqueIdentifier" = "758ba675-ff35-46aa-ae88-fd2d421a3c1f",
        "_class" = "ThreatIntel",
        "_keyField" = "test",
        "_type" = "test"
      }
    ]
  })
}

resource "provider_integration" "example_custom_integration" {
  name                      = "Custom Integration"
  integration_definition_id = "8013680b-311a-4c2e-b53b-c8735fd97a5c"
  polling_interval          = "THIRTY_MINUTES"
  description               = "Custom integration"

  config = jsonencode({
    "@tag" = {
      "AccountName" = "Custom Integration",
    }
  })
}
```
## variables.tf
This is to be used in addition to the main.tf for defining less static variables. This will require you to update the resource block to use the new variable. The instance_name variable block will default to its default value unless you declare a different value.
```
 resource "aws_instance" "app_server" {
   ami           = "ami-08d70e59c07c61a3a"
   instance_type = "t2.micro"

   tags = {
   - Name = "ExampleAppServerInstance"
   + Name = var.instance_name
   }
 }
 ```

## provider.tf
The provider.tf will be used to declare the variables exclusive to the provider. The example file content is below:
```
provider "example-provider" {
  api_key    = var.provider_api_key
  account_id = var.provider_account
  region     = var.provider_region
}
```

## *.tfvars
The .tfvars files are used to assign values to the input variables declared in other Terraform configuration files. By default, Terraform will load variable values from files called terraform.tfvars or any_name.auto.tfvars. If you have both files, `any_name.auto.tfvars` will take precedence over `terraform.tfvars`.

## tf.state
This file will be created when the `terraform apply` command is run

# Other Files
Throughout the project, we may need to add more files to serve various purposes besides the Terraform configurations. You can find some examples of these files in the list below:

## README.md
As a general best practice, every repository should contain a README.md file that includes an overview of the source code, usage instructions, and any other relevant and important information

## Automation scripts
When there is a need to include automation scripts (bash, shell, python, golang, etc.) in CI/CD workflow, when certain scripts are required to be executed on the target resource being created, or to build a source code, etc. Bash/shell scripts are very powerful in general; there are many reasons to use them.

## YAMLs
The most common usage of YAML files in Terraform in this context is when implementing CI/CD automation.

## .gitignore
For several reasons, not all files and directories should be part of the git repository. The following are some of the files included in the .gitignore file in a generic Terraform project.

`.terraform.tfstate` — Terraform state files should never be pushed to the git repositories. Note that when using the remote backend for Terraform, the state files will not be available on the local system. A couple of reasons are:
1. Security — State files may store sensitive details like keys, tokens, passwords, etc. 
2. Collaboration — When working within teams, managing the state file locally by each developer poses a high risk of state files needing to be more consistently overwritten. 
3. Binaries — The provider plugins downloaded locally or on a Terraform host (in .terraform directory) should not be part of the Git repository. The binaries thus downloaded are large in memory. Pushing and pulling the binaries from a remote git repo is inefficient for using network bandwidth.
4. Crash.log — Crash log files are not always required, especially when a crash occurs due to the local environment.
5. *.tfplan — We use the terraform plan command to save and use the output during the apply phase. This information is not required to be stored on a remote git repository.

Refer to the following https://github.com/github/gitignore/blob/main/Terraform.gitignore for more information on how to write a good .gitignore