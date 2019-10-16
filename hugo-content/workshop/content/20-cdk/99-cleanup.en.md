<!--
+++
title = "Cleanup"
date = 2019-10-15T11:53:46-04:00
weight = 999
+++
-->

Some manual clean up steps are required to delete resources created in the course of this module. You may need to follow these steps if you *used your personal or corporate AWS Account*. 

You will also need to use these steps if you are taking this lab at an AWS event and you simply want to reset your cloud resources and *restart the module from the [beginning](./10-overview.html)*.

> When using your personal or corporate AWS Account, leaving resources created by this lab will result in recurring monthly charges for used resources.
> 
> Commands below may remove most of the resources (no guarantees though) created in the course of this module.

### Delete Infrastructure "Stacks" Created by CDK Projects

```bash
aws cloudformation delete-stack --stack-name UnicornSuperstoreStack
aws cloudformation delete-stack --stack-name Unicorn-Store-CI-CD-PipelineStack
```
Although both commands return instantaneously, the process of deleting resources up to 10 minutes, give or take. To see the progress, please browse to [CloudFormation -> Stacks](https://console.aws.amazon.com/cloudformation/home) in AWS Console. There you should see **DELETE_IN_PROGRESS** status next to "UnicornSuperstoreStack" and "Unicorn-Store-CI-CD-PipelineStack" items. Refresh the page periodically using the round arrow icon.

### Delete KMS Key

This section may become unnecessary after project is upgraded to CDK 1.13.

Browse to AWS [Key Management Service](https://console.aws.amazon.com/kms/home) at AWS Console, and enter "codepipeline-cicdinfraascodestackbuildpipelineb5eb558e" in the search box. Click the key entry and copy the Key ID guid value to the clipboard.

Now remove the key alias and schedule the key for deletion
```bash
aws kms delete-alias --alias-name alias/codepipeline-cicdinfraascodestackbuildpipelineb5eb558e

aws kms schedule-key-deletion --key-id <PASTE Key ID VALUE HERE> --pending-window-in-days 7
```

### Delete ECR Docker Image Repository

```bash
aws ecr delete-repository --repository-name unicorn-store-app --force
```
