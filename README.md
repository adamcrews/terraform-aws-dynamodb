<!-- 














  ** DO NOT EDIT THIS FILE
  ** 
  ** This file was automatically generated by the `build-harness`. 
  ** 1) Make all changes to `README.yaml` 
  ** 2) Run `make init` (you only need to do this once)
  ** 3) Run`make readme` to rebuild this file. 
  **
  ** (We maintain HUNDREDS of open source projects. This is how we maintain our sanity.)
  **















  -->
[![README Header][readme_header_img]][readme_header_link]

[![Cloud Posse][logo]](https://cpco.io/homepage)

# terraform-aws-dynamodb [![Codefresh Build Status](https://g.codefresh.io/api/badges/pipeline/cloudposse/terraform-modules%2Fterraform-aws-dynamodb?type=cf-1)](https://g.codefresh.io/public/accounts/cloudposse/pipelines/5d1cdacfa7e22eb4aa7ea776) [![Latest Release](https://img.shields.io/github/release/cloudposse/terraform-aws-dynamodb.svg)](https://github.com/cloudposse/terraform-aws-dynamodb/releases/latest) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Terraform module to provision a DynamoDB table with autoscaling.

Autoscaler scales up/down the provisioned OPS for the DynamoDB table based on the load.

## Requirements

This module requires [AWS Provider](https://github.com/terraform-providers/terraform-provider-aws) `>= 1.17.0`


---

This project is part of our comprehensive ["SweetOps"](https://cpco.io/sweetops) approach towards DevOps. 
[<img align="right" title="Share via Email" src="https://docs.cloudposse.com/images/ionicons/ios-email-outline-2.0.1-16x16-999999.svg"/>][share_email]
[<img align="right" title="Share on Google+" src="https://docs.cloudposse.com/images/ionicons/social-googleplus-outline-2.0.1-16x16-999999.svg" />][share_googleplus]
[<img align="right" title="Share on Facebook" src="https://docs.cloudposse.com/images/ionicons/social-facebook-outline-2.0.1-16x16-999999.svg" />][share_facebook]
[<img align="right" title="Share on Reddit" src="https://docs.cloudposse.com/images/ionicons/social-reddit-outline-2.0.1-16x16-999999.svg" />][share_reddit]
[<img align="right" title="Share on LinkedIn" src="https://docs.cloudposse.com/images/ionicons/social-linkedin-outline-2.0.1-16x16-999999.svg" />][share_linkedin]
[<img align="right" title="Share on Twitter" src="https://docs.cloudposse.com/images/ionicons/social-twitter-outline-2.0.1-16x16-999999.svg" />][share_twitter]


[![Terraform Open Source Modules](https://docs.cloudposse.com/images/terraform-open-source-modules.svg)][terraform_modules]



It's 100% Open Source and licensed under the [APACHE2](LICENSE).







We literally have [*hundreds of terraform modules*][terraform_modules] that are Open Source and well-maintained. Check them out! 







## Usage


**IMPORTANT:** The `master` branch is used in `source` just as an example. In your code, do not pin to `master` because there may be breaking changes between releases.
Instead pin to the release tag (e.g. `?ref=tags/x.y.z`) of one of our [latest releases](https://github.com/cloudposse/terraform-aws-dynamodb/releases).


```hcl
module "dynamodb_table" {
  source                       = "git::https://github.com/cloudposse/terraform-aws-dynamodb.git?ref=master"
  namespace                    = "eg"
  stage                        = "dev"
  name                         = "cluster"
  hash_key                     = "HashKey"
  range_key                    = "RangeKey"
  autoscale_write_target       = 50
  autoscale_read_target        = 50
  autoscale_min_read_capacity  = 5
  autoscale_max_read_capacity  = 20
  autoscale_min_write_capacity = 5
  autoscale_max_write_capacity = 20
  enable_autoscaler            = true
}
```

## Advanced Usage

With additional attributes, global secondary indexes and `non_key_attributes` (see [examples/complete](examples/complete)).

```hcl
module "dynamodb_table" {
  source                       = "git::https://github.com/cloudposse/terraform-aws-dynamodb.git?ref=master"
  namespace                    = "eg"
  stage                        = "dev"
  name                         = "cluster"
  hash_key                     = "HashKey"
  range_key                    = "RangeKey"
  autoscale_write_target       = 50
  autoscale_read_target        = 50
  autoscale_min_read_capacity  = 5
  autoscale_max_read_capacity  = 20
  autoscale_min_write_capacity = 5
  autoscale_max_write_capacity = 20
  enable_autoscaler            = true

  dynamodb_attributes = [
    {
      name = "DailyAverage"
      type = "N"
    },
    {
      name = "HighWater"
      type = "N"
    },
    {
      name = "Timestamp"
      type = "S"
    }
  ]

  local_secondary_index_map = [
    {
      name               = "TimestampSortIndex"
      range_key          = "Timestamp"
      projection_type    = "INCLUDE"
      non_key_attributes = ["HashKey", "RangeKey"]
    },
    {
      name               = "HighWaterIndex"
      range_key          = "Timestamp"
      projection_type    = "INCLUDE"
      non_key_attributes = ["HashKey", "RangeKey"]
    }
  ]

  global_secondary_index_map = [
    {
      name               = "DailyAverageIndex"
      hash_key           = "DailyAverage"
      range_key          = "HighWater"
      write_capacity     = 5
      read_capacity      = 5
      projection_type    = "INCLUDE"
      non_key_attributes = ["HashKey", "RangeKey"]
    }
  ]
}
```






## Makefile Targets
```
Available targets:

  help                                Help screen
  help/all                            Display help for all targets
  help/short                          This help short screen
  lint                                Lint terraform code

```
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| attributes | Additional attributes (e.g. `policy` or `role`) | list(string) | `<list>` | no |
| autoscale_max_read_capacity | DynamoDB autoscaling max read capacity | number | `20` | no |
| autoscale_max_write_capacity | DynamoDB autoscaling max write capacity | number | `20` | no |
| autoscale_min_read_capacity | DynamoDB autoscaling min read capacity | number | `5` | no |
| autoscale_min_write_capacity | DynamoDB autoscaling min write capacity | number | `5` | no |
| autoscale_read_target | The target value (in %) for DynamoDB read autoscaling | number | `50` | no |
| autoscale_write_target | The target value (in %) for DynamoDB write autoscaling | number | `50` | no |
| billing_mode | DynamoDB Billing mode. Can be PROVISIONED or PAY_PER_REQUEST | string | `PROVISIONED` | no |
| delimiter | Delimiter to be used between `namespace`, `stage`, `name`, and `attributes` | string | `-` | no |
| dynamodb_attributes | Additional DynamoDB attributes in the form of a list of mapped values | object | `<list>` | no |
| enable_autoscaler | Flag to enable/disable DynamoDB autoscaling | string | `true` | no |
| enable_encryption | Enable DynamoDB server-side encryption | bool | `true` | no |
| enable_point_in_time_recovery | Enable DynamoDB point in time recovery | bool | `true` | no |
| enable_streams | Enable DynamoDB streams | bool | `false` | no |
| enabled | Set to false to prevent the module from creating any resources | bool | `true` | no |
| global_secondary_index_map | Additional global secondary indexes in the form of a list of mapped values | object | `<list>` | no |
| hash_key | DynamoDB table Hash Key | string | - | yes |
| hash_key_type | Hash Key type, which must be a scalar type: `S`, `N`, or `B` for (S)tring, (N)umber or (B)inary data | string | `S` | no |
| local_secondary_index_map | Additional local secondary indexes in the form of a list of mapped values | object | `<list>` | no |
| name | Name  (e.g. `app` or `cluster`) | string | - | yes |
| namespace | Namespace (e.g. `eg` or `cp`) | string | `` | no |
| range_key | DynamoDB table Range Key | string | `` | no |
| range_key_type | Range Key type, which must be a scalar type: `S`, `N`, or `B` for (S)tring, (N)umber or (B)inary data | string | `S` | no |
| regex_replace_chars | Regex to replace chars with empty string in `namespace`, `environment`, `stage` and `name`. By default only hyphens, letters and digits are allowed, all other chars are removed | string | `/[^a-zA-Z0-9-]/` | no |
| stage | Stage (e.g. `prod`, `dev`, `staging`, `infra`) | string | `` | no |
| stream_view_type | When an item in the table is modified, what information is written to the stream | string | `` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`) | map(string) | `<map>` | no |
| ttl_attribute | DynamoDB table TTL attribute | string | `Expires` | no |

## Outputs

| Name | Description |
|------|-------------|
| global_secondary_index_names | DynamoDB secondary index names |
| local_secondary_index_names | DynamoDB local index names |
| table_arn | DynamoDB table ARN |
| table_id | DynamoDB table ID |
| table_name | DynamoDB table name |
| table_stream_arn | DynamoDB table stream ARN |
| table_stream_label | DynamoDB table stream label |




## Share the Love 

Like this project? Please give it a ★ on [our GitHub](https://github.com/cloudposse/terraform-aws-dynamodb)! (it helps us **a lot**) 

Are you using this project or any of our other projects? Consider [leaving a testimonial][testimonial]. =)


## Related Projects

Check out these related projects.

- [terraform-aws-dynamodb-autoscaler](https://github.com/cloudposse/terraform-aws-dynamodb-autoscaler) - Terraform module to provision DynamoDB autoscaler
- [terraform-aws-tfstate-backend](https://github.com/cloudposse/terraform-aws-tfstate-backend) - Provision an S3 bucket to store terraform.tfstate file and a DynamoDB table to lock the state file to prevent concurrent modifications and state corruption
- [terraform-aws-elasticache-redis](https://github.com/cloudposse/terraform-aws-elasticache-redis) - Terraform module to provision an ElastiCache Redis Cluster
- [terraform-aws-elasticache-memcached](https://github.com/cloudposse/terraform-aws-elasticache-memcached) - Terraform Module for ElastiCache Memcached Cluster
- [terraform-aws-elasticache-cloudwatch-sns-alarms](https://github.com/cloudposse/terraform-aws-elasticache-cloudwatch-sns-alarms) - Terraform module that configures CloudWatch SNS alerts for ElastiCache
- [terraform-aws-rds-cluster](https://github.com/cloudposse/terraform-aws-rds-cluster) - Terraform module to provision an RDS Aurora cluster for MySQL or Postgres
- [terraform-aws-rds](https://github.com/cloudposse/terraform-aws-rds) - Terraform module to provision AWS RDS instances



## Help

**Got a question?** We got answers. 

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-dynamodb/issues), send us an [email][email] or join our [Slack Community][slack].

[![README Commercial Support][readme_commercial_support_img]][readme_commercial_support_link]

## DevOps Accelerator for Startups


We are a [**DevOps Accelerator**][commercial_support]. We'll help you build your cloud infrastructure from the ground up so you can own it. Then we'll show you how to operate it and stick around for as long as you need us. 

[![Learn More](https://img.shields.io/badge/learn%20more-success.svg?style=for-the-badge)][commercial_support]

Work directly with our team of DevOps experts via email, slack, and video conferencing.

We deliver 10x the value for a fraction of the cost of a full-time engineer. Our track record is not even funny. If you want things done right and you need it done FAST, then we're your best bet.

- **Reference Architecture.** You'll get everything you need from the ground up built using 100% infrastructure as code.
- **Release Engineering.** You'll have end-to-end CI/CD with unlimited staging environments.
- **Site Reliability Engineering.** You'll have total visibility into your apps and microservices.
- **Security Baseline.** You'll have built-in governance with accountability and audit logs for all changes.
- **GitOps.** You'll be able to operate your infrastructure via Pull Requests.
- **Training.** You'll receive hands-on training so your team can operate what we build.
- **Questions.** You'll have a direct line of communication between our teams via a Shared Slack channel.
- **Troubleshooting.** You'll get help to triage when things aren't working.
- **Code Reviews.** You'll receive constructive feedback on Pull Requests.
- **Bug Fixes.** We'll rapidly work with you to fix any bugs in our projects.

## Slack Community

Join our [Open Source Community][slack] on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

## Newsletter

Sign up for [our newsletter][newsletter] that covers everything on our technology radar.  Receive updates on what we're up to on GitHub as well as awesome new projects we discover. 

## Office Hours

[Join us every Wednesday via Zoom][office_hours] for our weekly "Lunch & Learn" sessions. It's **FREE** for everyone! 

[![zoom](https://img.cloudposse.com/fit-in/200x200/https://cloudposse.com/wp-content/uploads/2019/08/Powered-by-Zoom.png")][office_hours]

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-dynamodb/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://cpco.io/help-out) with our other projects, we would love to hear from you! Shoot us an [email][email].

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2019 [Cloud Posse, LLC](https://cpco.io/copyright)



## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know by [leaving a testimonial][testimonial]!

[![Cloud Posse][logo]][website]

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We ❤️  [Open Source Software][we_love_open_source].

We offer [paid support][commercial_support] on all of our projects.  

Check out [our other projects][github], [follow us on twitter][twitter], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.



### Contributors

|  [![Erik Osterman][osterman_avatar]][osterman_homepage]<br/>[Erik Osterman][osterman_homepage] | [![Andriy Knysh][aknysh_avatar]][aknysh_homepage]<br/>[Andriy Knysh][aknysh_homepage] | [![Jamie Nelson][Jamie-BitFlight_avatar]][Jamie-BitFlight_homepage]<br/>[Jamie Nelson][Jamie-BitFlight_homepage] | [![Daren Desjardins][darend_avatar]][darend_homepage]<br/>[Daren Desjardins][darend_homepage] | [![Maarten van der Hoef][maartenvanderhoef_avatar]][maartenvanderhoef_homepage]<br/>[Maarten van der Hoef][maartenvanderhoef_homepage] |
|---|---|---|---|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: https://img.cloudposse.com/150x150/https://github.com/osterman.png
  [aknysh_homepage]: https://github.com/aknysh
  [aknysh_avatar]: https://img.cloudposse.com/150x150/https://github.com/aknysh.png
  [Jamie-BitFlight_homepage]: https://github.com/Jamie-BitFlight
  [Jamie-BitFlight_avatar]: https://img.cloudposse.com/150x150/https://github.com/Jamie-BitFlight.png
  [darend_homepage]: https://github.com/darend
  [darend_avatar]: https://img.cloudposse.com/150x150/https://github.com/darend.png
  [maartenvanderhoef_homepage]: https://github.com/maartenvanderhoef
  [maartenvanderhoef_avatar]: https://img.cloudposse.com/150x150/https://github.com/maartenvanderhoef.png

[![README Footer][readme_footer_img]][readme_footer_link]
[![Beacon][beacon]][website]

  [logo]: https://cloudposse.com/logo-300x69.svg
  [docs]: https://cpco.io/docs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=docs
  [website]: https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=website
  [github]: https://cpco.io/github?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=github
  [jobs]: https://cpco.io/jobs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=jobs
  [hire]: https://cpco.io/hire?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=hire
  [slack]: https://cpco.io/slack?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=slack
  [linkedin]: https://cpco.io/linkedin?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=linkedin
  [twitter]: https://cpco.io/twitter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=twitter
  [testimonial]: https://cpco.io/leave-testimonial?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=testimonial
  [office_hours]: https://cloudposse.com/office-hours?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=office_hours
  [newsletter]: https://cpco.io/newsletter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=newsletter
  [email]: https://cpco.io/email?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=email
  [commercial_support]: https://cpco.io/commercial-support?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=commercial_support
  [we_love_open_source]: https://cpco.io/we-love-open-source?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=we_love_open_source
  [terraform_modules]: https://cpco.io/terraform-modules?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=terraform_modules
  [readme_header_img]: https://cloudposse.com/readme/header/img
  [readme_header_link]: https://cloudposse.com/readme/header/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=readme_header_link
  [readme_footer_img]: https://cloudposse.com/readme/footer/img
  [readme_footer_link]: https://cloudposse.com/readme/footer/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=readme_footer_link
  [readme_commercial_support_img]: https://cloudposse.com/readme/commercial-support/img
  [readme_commercial_support_link]: https://cloudposse.com/readme/commercial-support/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-dynamodb&utm_content=readme_commercial_support_link
  [share_twitter]: https://twitter.com/intent/tweet/?text=terraform-aws-dynamodb&url=https://github.com/cloudposse/terraform-aws-dynamodb
  [share_linkedin]: https://www.linkedin.com/shareArticle?mini=true&title=terraform-aws-dynamodb&url=https://github.com/cloudposse/terraform-aws-dynamodb
  [share_reddit]: https://reddit.com/submit/?url=https://github.com/cloudposse/terraform-aws-dynamodb
  [share_facebook]: https://facebook.com/sharer/sharer.php?u=https://github.com/cloudposse/terraform-aws-dynamodb
  [share_googleplus]: https://plus.google.com/share?url=https://github.com/cloudposse/terraform-aws-dynamodb
  [share_email]: mailto:?subject=terraform-aws-dynamodb&body=https://github.com/cloudposse/terraform-aws-dynamodb
  [beacon]: https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/terraform-aws-dynamodb?pixel&cs=github&cm=readme&an=terraform-aws-dynamodb
