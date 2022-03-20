`az login`
`az cloud set --name AzureUSGovernment`
`az account list-locations -o table`
`az group create --name rg-packer-drok-dev-001 --location "USGov Virginia"`
`az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${ARM_SUBSCRIPTION_ID}"`
`az ad sp create-for-rbac --role="Reader" --scopes="/subscriptions/[SUBSCRIPTION_ID]"`