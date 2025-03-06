## State Commands

### terrafrom state List
This command lists all resources that are being tracked in the Terraform state.
```
terraform state list
```

### terraform state show
Displays detailed information about a specific resource in the Terraform state, including its attributes and metadata.
```
terraform state show aws_instance.terraform-remoteState
```

### terraform state mv
Moves an item in the Terraform state from one address to another. This is useful when you want to rename a resource or change its location in the state file.
```
terraform state mv aws_instance.terraform-remoteState aws_instance.new_terraform-remoteState
```

### terraform state rm
Removes one or more items from the Terraform state. This is typically used when you want to delete a resource from your infrastructure and stop managing it with Terraform.
```
terraform state rm aws_instance.new_terraform-remoteState
```

### terraform state pull
Manually pulls the state and outputs it as JSON. This is useful for debugging or inspecting the current state of your infrastructure.
```
terraform state pull > terraform_state.json
```

### terraform state push
Manually pushes the local state to the configured backend. This command is typically used in scenarios where you want to sync the local state with the remote state backend.
```
terraform state push
```

