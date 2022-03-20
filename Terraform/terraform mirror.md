REF: https://www.youtube.com/watch?v=WY9W6tHbS8Y
Setup a provider mirror to cache modules and providers

create a folder to store files `mkdir terra-cache`
create a terraform config `touch .terraformrc`
point the config file in the termial session `export TF_CLI_CONFIG_FILE="/Users/drok/Documents/1. Active_Projects/3. drok-stack-IaC/.terraformrc"`
then insert example to setup local directory
```
cat <<EOF > $TF_CLI_CONFIG_FILE
provider_installation {
  filesystem_mirror {
    path    = "/Users/drok/Documents/1. Active_Projects/3. drok-stack-IaC/terraform-file-mirror"
    include = ["terra-farm/xenorchestra"]
	  include = ["hashicorp/random"]
  }
  direct {
    exclude = ["terra-farm/xenorchestra"]
	  exclude = ["hashicorp/random"]
  }
}
EOF
```

run `terraform providers mirror ~/terraform-file-mirror`


