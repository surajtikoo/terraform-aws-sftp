<!-- This file was automatically generated by the `geine`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS SFTP
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    This terraform module is used to create sftp on AWS for S3.
     </p>

<p align="center">

<a href="https://github.com/clouddrove/terraform-aws-sftp/releases/latest">
  <img src="https://img.shields.io/github/release/clouddrove/terraform-aws-sftp.svg" alt="Latest Release">
</a>
<a href="https://github.com/clouddrove/terraform-aws-sftp/actions/workflows/tfsec.yml">
  <img src="https://github.com/clouddrove/terraform-aws-sftp/actions/workflows/tfsec.yml/badge.svg" alt="tfsec">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-APACHE-blue.svg" alt="Licence">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/clouddrove/terraform-aws-sftp'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+SFTP&url=https://github.com/clouddrove/terraform-aws-sftp'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+SFTP&url=https://github.com/clouddrove/terraform-aws-sftp'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>


We eat, drink, sleep and most importantly love **DevOps**. We are working towards strategies for standardizing architecture while ensuring security for the infrastructure. We are strong believer of the philosophy <b>Bigger problems are always solved by breaking them into smaller manageable problems</b>. Resonating with microservices architecture, it is considered best-practice to run database, cluster, storage in smaller <b>connected yet manageable pieces</b> within the infrastructure. 

This module is basically combination of [Terraform open source](https://www.terraform.io/) and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of maintaining the whole infrastructure code yourself.

We have [*fifty plus terraform modules*][terraform_modules]. A few of them are comepleted and are available for open source usage while a few others are in progress.




## Prerequisites

This module has a few dependencies: 






## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/clouddrove/terraform-aws-sftp/releases).


### Simple Example
### Public
Here is an example of how you can use this module in your inventory structure:
```hcl
    module "sftp" {
      source         = "clouddrove/sftp/aws"
      version        = "1.3.1"      
      name           = "sftp"
      environment    = "test"
      label_order    = ["environment", "name"]
      enable_sftp    = true
      s3_bucket_name = module.s3_bucket.id
      endpoint_type  = "PUBLIC"
      workflow_details = {
        on_upload = {
          execution_role = "arn:aws:iam::1234567890:role/test-sftp-transfer-role"
          workflow_id    = "w-12345XXXX6da"
        }
      }
    }
```

### VPC
Here is an example of how you can use this module in your inventory structure:
```hcl
    module "sftp" {
      source                 = "clouddrove/sftp/aws"
      version                = "1.3.1"      
      name                   = "sftp"
      environment            = "test"
      label_order            = ["environment", "name"]
      eip_enabled            = false
      s3_bucket_name         = module.s3_bucket.id
      sftp_users             = var.sftp_users
      subnet_ids             = module.subnets.private_subnet_id
      vpc_id                 = module.vpc.vpc_id
      restricted_home        = true
      vpc_security_group_ids = [module.security_group_sftp.security_group_id]
      workflow_details = {
        on_upload = {
          execution_role = "arn:aws:iam::1234567890:role/test-sftp-transfer-role"
          workflow_id    = "w-12345XXXX6da"
        }
      }
    }
```





## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| address\_allocation\_ids | A list of address allocation IDs that are required to attach an Elastic IP address to your SFTP server's endpoint. This property can only be used when endpoint\_type is set to VPC. | `list(string)` | `[]` | no |
| attributes | Additional attributes (e.g. `1`). | `list(any)` | <pre>[<br>  "transfer"<br>]</pre> | no |
| domain | Where your files are stored. S3 or EFS | `string` | `"S3"` | no |
| domain\_name | Domain to use when connecting to the SFTP endpoint | `string` | `""` | no |
| eip\_enabled | Whether to provision and attach an Elastic IP to be used as the SFTP endpoint. An EIP will be provisioned per subnet. | `bool` | `false` | no |
| enable\_sftp | Set to false to prevent the module from creating any resources. | `bool` | `true` | no |
| enable\_workflow | n/a | `bool` | `false` | no |
| enabled | Set to false to prevent the module from creating any resources. | `bool` | `true` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| force\_destroy | Forces the AWS Transfer Server to be destroyed | `bool` | `false` | no |
| identity\_provider\_type | The mode of authentication enabled for this service. The default value is SERVICE\_MANAGED, which allows you to store and access SFTP user credentials within the service. API\_GATEWAY. | `string` | `"SERVICE_MANAGED"` | no |
| label\_order | Label order, e.g. `name`,`application`. | `list(any)` | `[]` | no |
| managedby | ManagedBy, eg 'CloudDrove'. | `string` | `"hello@clouddrove.com"` | no |
| name | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| repository | Terraform current module repo | `string` | `"https://github.com/clouddrove/terraform-aws-sftp"` | no |
| restricted\_home | Restricts SFTP users so they only have access to their home directories. | `bool` | `true` | no |
| s3\_bucket\_name | This is the bucket that the SFTP users will use when managing files | `string` | n/a | yes |
| security\_policy\_name | Specifies the name of the security policy that is attached to the server. Possible values are TransferSecurityPolicy-2018-11, TransferSecurityPolicy-2020-06, and TransferSecurityPolicy-FIPS-2020-06. Default value is: TransferSecurityPolicy-2018-11. | `string` | `"TransferSecurityPolicy-2018-11"` | no |
| sftp\_users | List of SFTP usernames and public keys. The keys `user_name`, `public_key` are required. The keys `s3_bucket_name` are optional. | `any` | `{}` | no |
| subnet\_ids | A list of subnet IDs that are required to host your SFTP server endpoint in your VPC. This property can only be used when endpoint\_type is set to VPC. | `list(string)` | `[]` | no |
| vpc\_id | VPC ID | `string` | `null` | no |
| vpc\_security\_group\_ids | A list of security groups IDs that are available to attach to your server's endpoint. If no security groups are specified, the VPC's default security groups are automatically assigned to your endpoint. This property can only be used when endpoint\_type is set to VPC. | `list(string)` | `[]` | no |
| workflow\_details | Workflow details for triggering the execution on file upload. | <pre>object({<br>    on_upload = object({<br>      execution_role = string<br>      workflow_id    = string<br>    })<br>  })</pre> | n/a | yes |
| zone\_id | Route53 Zone ID to add the CNAME | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| id | The Server ID of the Transfer Server (e.g. s-12345678). |
| tags | A mapping of tags to assign to the resource. |
| transfer\_server\_endpoint | The endpoint of the Transfer Server (e.g. s-12345678.server.transfer.REGION.amazonaws.com). |




## Testing
In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system. 

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```



## Feedback 
If you come accross a bug or have any feedback, please log it in our [issue tracker](https://github.com/clouddrove/terraform-aws-sftp/issues), or feel free to drop us an email at [hello@clouddrove.com](mailto:hello@clouddrove.com).

If you have found it worth your time, go ahead and give us a ★ on [our GitHub](https://github.com/clouddrove/terraform-aws-sftp)!

## About us

At [CloudDrove][website], we offer expert guidance, implementation support and services to help organisations accelerate their journey to the cloud. Our services include docker and container orchestration, cloud migration and adoption, infrastructure automation, application modernisation and remediation, and performance engineering.

<p align="center">We are <b> The Cloud Experts!</b></p>
<hr />
<p align="center">We ❤️  <a href="https://github.com/clouddrove">Open Source</a> and you can check out <a href="https://github.com/clouddrove">our other modules</a> to get help with your new Cloud ideas.</p>

  [website]: https://clouddrove.com
  [github]: https://github.com/clouddrove
  [linkedin]: https://cpco.io/linkedin
  [twitter]: https://twitter.com/clouddrove/
  [email]: https://clouddrove.com/contact-us.html
  [terraform_modules]: https://github.com/clouddrove?utf8=%E2%9C%93&q=terraform-&type=&language=
