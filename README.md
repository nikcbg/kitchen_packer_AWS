# kitchen_packer_AWS

### Purpose of the repository
- The repository creates Ubuntu box in AWS with `nginx` installed and test that with `kitchen` framework.

#### List of files in the repository

File name                            | File description 
------------------------------------ | --------------------------------------------------------------
`test/integration/default/check_pkg.rb` | script needed by kitchen to check if `nginx` is installed.
`.gitignore` | which files and directories to ignore in the repository.
`.kitchen.yml` | configuration file for `kitchen` framework.
`Gemfile` | used for `ruby` dependencies.
`template.json` | template with code for `packer` tool to create AWS image and installs `nginx`.

### How to use this repository. 
- Make sure you have AWS account(Amazon offers free tier accounts for 12 month or have an account from your organization).
- Install `packer` by following this [instructions](https://www.packer.io/intro/getting-started/install.html).
- Clone the repository to your local computer: `git clone git@github.com:nikcbg/pkitchen_packer_AWS.git`.
- Go to the cloned repo on your computer: `cd kitchen_packer_AWS`.

### Creating and configuring the AWS box.
### Note: NEVER make your AWS access keys public so you can prevent your account been compromised.
- To be able to connect to your AWS account without compromising your credentials you need to use environment variables.
- You need to get your secret access and access keys form your AWS account, you can follow this [instructions](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html)
- Execute `export AWS_ACCESS_KEY_ID=your_access_key_here` 
- Execute `export AWS_SECRET_ACCESS_KEY=your_secret_access_key_here` 

Command execution	                   |    Command outcome
------------------------------------ | --------------------------------------------------------------
`packer validate template.json` | to validate the template.
`packer build template.json` | to start building the AWS image, successful build output will display the following:

```
==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs: AMIs were created:
us-east-1: ami-079c3b1b38309f925

```
- After the AMI is created you need to use the newly created AMI (ami-079c3b1b38309f925) in your `.kitchen.yml` file so your test can pass successfully. 

- Next you need to set up your testing environment.

### Setting up `ruby` environment on Ubuntu 18.04 instructions:
- Before testing with `kitchen` you need to install and prepare `ruby` environment.
- Execute `sudo apt update` to update the packages on your Ubuntu computer. 
- Execute `sudo apt install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev` to install `ruby` dependencies.
- Execute `wget -q https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-installer -O- | bash` to download and install `rbenv` environment
- Execute `echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile` to change your `~/.bashrc` file to use `ruby` command line utility 
- Execute `echo 'eval "$(rbenv init -)"' >> ~/.bashrc` so `rbenv` loads automatically.
- Execute `source ~/.bash_profile` to reload `bash` profile.
- Execute `type rbenv` command to verify that `rbenv` is set up properly, the output will display the following:
```
rbenv is a function
rbenv ()
{
    local command;
    command="${1:-}";
    if [ "$#" -gt 0 ]; then
        shift;
    fi;
    case "$command" in
        rehash | shell)
            eval "$(rbenv "sh-$command" "$@")"
        ;;
        *)
            command rbenv "$command" "$@"
        ;;
    esac
}
```

Command execution	                   |    Command outcome
------------------------------------ | --------------------------------------------------------------
`rbenv install 2.5.3` | to install `ruby 2.5.3` version.
`rbenv local 2.5.3` | to set the default version of `ruby` to your local directory.
`rbenv -v` | to make sure `ruby` is installed and you have the correct version.
`gem install bundler` | to install `gems` which is package manager for `ruby`, the output will display the following:

```
Successfully installed bundler-1.17.1
1 gem installed
```

### Commands needed to test with `kitchen`.

Command execution                    | Command outcome
------------------------------------ | --------------------------------------------------------------
`bundle exec kitchen list` | to list `kitchen` instances.
`bundle exec kitchen converge` | to create `kitchen` environment.
`bundle exec kitchen verify` | command to execute `kitchen` test.
`bundle exec kitchen destroy` | to destroy `kitchen` environment.
`bundle exec kitchen test` | to automatically build, test and destroy `kitchen` environment.

- If the command/s complete successfully the output will display the following:

```
  System Package nginx
     ✔  should be installed

Test Summary: 1 successful, 0 failures, 0 skipped

```

# TO DO:
- Check if everything works as expected 
