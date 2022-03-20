To run unattended 
```
ansible-playbook main.yml -e "provider=lightsail
                                server_name=algo-vanguard
                                ondemand_cellular=false
                                ondemand_wifi=false
                                dns_adblocking=true
                                ssh_tunneling=true
                                store_pki=true
                                aws_access_key=AKIAYGZT4EEYAAQ5WRWJ
								aws_secret_key=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
								region=us-east-2"
```
								
Update users

Playbook:

users.yml

Required variables:

    server - IP or hostname to access the server via SSH
    ca_password - Password to access the CA key

Tags required:

    update-users
