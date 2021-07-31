---
layout: post
title:  "How to fix Terraform's acquiring the state lock error"
categories: [ terraform ]
image: assets/images/terraform-state-lock-error.jpg
tags: [ terraform ]
---
A couple of days ago, I was deploying a Terraform ECS cluster, and during the `terraform plan` command, i forgot to add a module. Didn't want to wait it out until the command finish, so i did the usual `ctrl + c`, add the module and run `terraform plan` again. And guess what happened next.

```bash
Acquiring state lock. This may take a few moments...
╷
│ Error: Error acquiring the state lock
│ 
│ Error message: ConditionalCheckFailedException: The conditional request failed
│ Lock Info:
│   ID:        c2024f2b-b615-05bf-e516-e49ed2852087
│   Path:      <...>
│   Operation: OperationTypePlan
│   Who:       <...>
│   Version:   1.0.2
│   Created:   2021-07-28 14:54:32.498842 +0000 UTC
│   Info:      
│ 
│ 
│ Terraform acquires a state lock to protect the state from being written
│ by multiple users at the same time. Please resolve the issue above and try
│ again. For most commands, you can disable locking with the "-lock=false"
│ flag, but this is not recommended.
```

There are few ways to fix this issue, found below.

## Prerequisites
* Terraform

## The cause of error
The error usually is result of `terraform plan`, `terraform apply` commands in a hanging state. This could happen offen, especially if you trying to forcefully interrupt the command, loss of network connectivity, there is an automated deployment currently running, or even a colleague of yours is running one of these commands in the same time, to name a few. 

## The resolution
First, try to unlock the state:
```bash
terraform force-unlock <ID>
```

Then, type yes and hit `Enter`. Example output:  
```bash
Do you really want to force-unlock?
  Terraform will remove the lock on the remote state.
  This will allow local Terraform commands to modify this state, even though it
  may be still be in use. Only 'yes' will be accepted to confirm.

  Enter a value: yes

Terraform state has been successfully unlocked!

The state has been unlocked, and Terraform commands should now be able to
obtain a new lock on the remote state.
```

**Note**: You can find the `ID` from the error's lock info section above. In my case, the ID value is:  
```bash
c2024f2b-b615-05bf-e516-e49ed2852087
```

And, if the command above doesn't work, try the following:
```bash
terraform force-unlock -force <ID>
```

If both commands don't unlock the state, you can just wait for a couple of minutes or half an hour, and give it a try again. At last, try to kill the hanging `terraform` process. If you are running Terraform on Windows machine, find the process in the Task Manager and kill it. If you are running Terraform on Linux or Mac, run the following commands:  
```bash
ps aux | grep terraform
kill -9 <process_id>
```

## Conclusion
There is one more solution you could try, run the same command with `-lock=false`, but i wouldn't recommend it either.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.