# Azure-AI-services-using-Terraform

# Initialize Terraform
Run terraform init to initialize the Terraform deployment. This command downloads the Azure provider required to manage your Azure resources.

terraform init -upgrade

Key points:
The -upgrade parameter upgrades the necessary provider plugins to the newest version that complies with the configuration's version constraints.

# Create a Terraform execution plan
Run terraform plan to create an execution plan.

terraform plan -out main.tfplan

Key points:
The terraform plan command creates an execution plan, but doesn't execute it. Instead, it determines what actions are necessary to create the configuration specified in your configuration files. This pattern allows you to verify whether the execution plan matches your expectations before making any changes to actual resources.
The optional -out parameter allows you to specify an output file for the plan. Using the -out parameter ensures that the plan you reviewed is exactly what is applied.

# Apply a Terraform execution plan
Run terraform apply to apply the execution plan to your cloud infrastructure.

terraform apply main.tfplan

Key points:
The example terraform apply command assumes you previously ran terraform plan -out main.tfplan.
If you specified a different filename for the -out parameter, use that same filename in the call to terraform apply.
If you didn't use the -out parameter, call terraform apply without any parameters.

# Verify the results
Azure CLI

Get the Azure resource name in which the Azure AI services multi-service resource was created.

resource_group_name=$(terraform output -raw resource_group_name)

# Get the Azure AI services multi-service resource name.

azurerm_aiservices_account_name=$(terraform output -raw azurerm_aiservices_account_name)

Run az cognitiveservices account show to show the Azure AI services account you created in this article.

az cognitiveservices account show --name $azurerm_aiservices_account_name \
                                  --resource-group $resource_group_name

# Clean up resources
When you no longer need the resources created via Terraform, do the following steps:

Run terraform plan and specify the destroy flag.

terraform plan -destroy -out main.destroy.tfplan

Key points:
The terraform plan command creates an execution plan, but doesn't execute it. Instead, it determines what actions are necessary to create the configuration specified in your configuration files. This pattern allows you to verify whether the execution plan matches your expectations before making any changes to actual resources.
The optional -out parameter allows you to specify an output file for the plan. Using the -out parameter ensures that the plan you reviewed is exactly what is applied.
Run terraform apply to apply the execution plan.

terraform apply main.destroy.tfplan
