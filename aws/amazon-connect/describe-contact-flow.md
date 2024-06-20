---
title: Describe Contact Flow
draft: false
tags:
  - aws
  - amazon-connect
  - terraform
  - iac
---
# Describe Contact Flow

## Synopsis
```bash
  describe-contact-flow
--instance-id <value>
--contact-flow-id <value>
[--cli-input-json | --cli-input-yaml]
[--generate-cli-skeleton <value>]
[--debug]
[--endpoint-url <value>]
[--no-verify-ssl]
[--no-paginate]
[--output <value>]
[--query <value>]
[--profile <value>]
[--region <value>]
[--version <value>]
[--color <value>]
[--no-sign-request]
[--ca-bundle <value>]
[--cli-read-timeout <value>]
[--cli-connect-timeout <value>]
[--cli-binary-format <value>]
[--no-cli-pager]
[--cli-auto-prompt]
[--no-cli-auto-prompt]
```

## Describe and Download Contact Flow

Describe and download Amazon Connect contact flows for use in terraform:
### contact flow module
```bash
aws connect describe-contact-module --instance-id <instance-id> --contact-flow-id <contact-flow-moudle-id> --region eu-central-1 | jq '.ContactFlowModule.Content | fromjson' > tmp/contact-flow.json 
```

### contact flow module
```bash
aws connect describe-contact-flow-module --instance-id <instance-id> --contact-flow-module-id <contact-flow-moudle-id> --region eu-central-1 | jq '.ContactFlowModule.Content | fromjson' > tmp/contact-flow-module.json 
```


## Usage in Terraform

[[terraform]]

```json
resource "aws_connect_contact_flow" "test" {
  instance_id  = "aaaaaaaa-bbbb-cccc-dddd-111111111111"
  name         = "Test"
  description  = "Test Contact Flow Description"
  type         = "CONTACT_FLOW"
  filename     = "contact_flow.json"
  content_hash = filebase64sha256("contact_flow.json")
  tags = {
    "Name"        = "Test Contact Flow",
    "Application" = "Terraform",
    "Method"      = "Create"
  }
}
```

> [!warning]
> Using exported flows/modules will mess up any hardcoded references to lambdas, lex-bots, etc.
> Prevent this by replacing them with dynamic references:
> ```json
> {
  "Parameters": {
    "EventHooks": {
      "CustomerRemaining": "${aws_connect_contact_flow.disconnect_flow.arn}"
    }
  },
  "Identifier": "a0ea1bfa-e1d0-4939-9b08-1b7338edbfb7",
  "Type": "UpdateContactEventHooks",
  "Transitions": {
    "NextAction": "bd0c652d-b2d0-4132-895a-573ca35d8cf2",
    "Errors": [
      {
        "NextAction": "bd0c652d-b2d0-4132-895a-573ca35d8cf2",
        "ErrorType": "NoMatchingError"
      }
    ]
  }
},
> ```

> [!info]
> While you don't really need the `Metadata` part of the `contact-flow.json` as it only holds information for the contact flow editor, you may want to keep it in order to edit it later using the editor.


---
## Resources
- [describe-contact-flow](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/connect/describe-contact-flow.html)
- [Terraform - connect_contact_flow](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/connect_contact_flow)