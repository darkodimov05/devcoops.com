---
layout: post
title:  "How to remove ECS task definitions"
categories: [ aws ]
image: assets/images/remove-ecs-task-definitions.png
tags: [ aws, ecs ]
---
For those who have experience with AWS ECS, we all know how frustrating is sometimes that we just can't delete task definitions. The only option is to update all task definition revisions to `INACTIVE` state, which however the task definition itself will be still discovered indefinitely. This could be done through the AWS Console, also known as ClickOps, but let's see how can we script this thing out.

## Prerequisites
* AWS ECS task definitions
* jq

## Remove a single task definition
Copy the script below on your local machine, replace `<region>` and `<task-definition>` with your region and task definition name respectively, make it executable `chmod +x ./remove-task-definition.sh`, and run the script `./remove-task-definition.sh`.

```bash
#!/usr/bin/env bash

get_task_definition_arns() {
    aws ecs list-task-definitions --region <region> --family-prefix <task-definition> \
        | jq -M -r '.taskDefinitionArns | .[]'
}

delete_task_definition() {
    local arn=$1

    aws ecs deregister-task-definition \
        --region <region> \
        --task-definition "${arn}" > /dev/null
}

for arn in $(get_task_definition_arns)
do
    echo "Deregistering ${arn}..."
    delete_task_definition "${arn}"
done
```

## Remove all task definiitions
Copy the script below on your local machine, replace `<region>` with your region, make it executable `chmod +x ./remove-all-task-definitions.sh`, and run the script `./remove-all-task-definitions.sh`.

```bash
#!/usr/bin/env bash

get_task_definition_arns() {
    aws ecs list-task-definitions --region <region> \
        | jq -M -r '.taskDefinitionArns | .[]'
}

delete_task_definition() {
    local arn=$1

    aws ecs deregister-task-definition \
        --region <region> \
        --task-definition "${arn}" > /dev/null
}

for arn in $(get_task_definition_arns)
do
    echo "Deregistering ${arn}..."
    delete_task_definition "${arn}"
done
```

Instead of hard-coding it, you could also improve it, by replacing `<region>` and `<task-definition>` as arguments or variables, add some validations, a help function, etc..

If you want to mess around, you can find the original script [here](https://gist.github.com/jen20/e1c25426cc0a4a9b53cbb3560a3f02d1){:target="_blank"}.

## Conclusion
There is a container roadmap by AWS, and leaving a thumbs up on [Delete task definitions](https://github.com/aws/containers-roadmap/issues/685){:target="_blank"} issue, could help the maintainers to prioritize the request.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.