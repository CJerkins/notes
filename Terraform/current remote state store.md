resource_group_location = "usgovvirginia"
resource_group_name = "rg-tfstates-drok-dev-001"
storage_account_facts = <sensitive>
storage_account_id = "stremotetfstatedev001"
storage_container_name = "remote-tfstates"

terraform {
  backend "azurerm" {
    resource_group_name  = "rg-tfstates-drok-dev-001"
    storage_account_name = "stremotetfstatedev001"
    container_name       = "remote-tfstates"
    key                  = "dev.terraform.tfstate"
  }
}