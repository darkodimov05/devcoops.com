---
layout: post
title:  "Restrict Variable Values in Terraform"
categories: [ terraform ]
image: assets/images/terraform-vars-validation.jpg
tags: [ terraform ]
---
If you have worked before with Terraform by provisioning a cloud infrastructure and/or writing Terraform modules, you must have stumbled upon some variable validation issues. This could happen alot when you are trying to assign a value to a variable which is out of the valid variable values scope, or it's the wrong variable type. Let's see how can we fix it.

## Prerequisites
* Terraform

## Example
Let's say we got an `alb_target_group` and as a target group it can one of 3 target types: `instance`, `ip` and `lambda`. The custom variable validation should look something like this:
```bash
variable "target_type" {
  description = "Type of target that you must specify when registering targets with this target group."
  type        = string
  default     = "instance"

  validation {
    condition     = contains(["instance", "ip", "lambda"], var.target_type)
    error_message = "Valid values for target_type are (instance, ip, lambda)."
  }
}
```

Now, let's assign a value that doesn't exist in the valid variable values scope, for example: `s3`. Output:  
```bash
Error: Invalid value for variable
│ 
│   on main.tf line 453, in module "target_group":
│  453:   target_type = "s3"
│ 
│ Valid values for target_type are (instance, ip, lambda).
│ 
│ This was checked by the validation rule at ../modules/ecs/alb_target_group/variables.tf:16,3-13.
```

As we can see, the output will show us which variable values are valid.

## Conclusion
This feature is quite handy, especially if you are writing public modules that could be used by others, so having a custom validation is always a good idea in mind. On the other hand, it allows us as devops engineers to distinguish between an error that occurs because of variable validation itself, or an error used as condition for validation.  
My two cents as a good practice is to always use custom variable validation whenever you deal with a scope of valid variable values. The custom variable validation feature was released in Terraform 0.13 including the official blog [announcement](https://www.hashicorp.com/blog/custom-variable-validation-in-terraform-0-13){:target="_blank"}.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.