---
title: Creating Contact Flows
draft: false
tags:
  - amazon-connect
  - aws
  - terraform
  - iac
---
# Creating Contact Flows

- Contact flows can be created using [[amazon-connect | Amazon Connects]] built in Flow Designer or using [[Terraform]].

## Flow Designer
- [Create a Flow](https://docs.aws.amazon.com/connect/latest/adminguide/create-contact-flow.html)

## Terraform
- Each flow can also be represented and edited as code. However, this isn't the nicest experience.
- It is easier to create the flow in the flow designer and then [[export-contact-flow | export the contact flow]] for usage in Terraform.
```json
resource "aws_connect_contact_flow" "test" {
  instance_id = "aaaaaaaa-bbbb-cccc-dddd-111111111111"
  name        = "Test"
  description = "Test Contact Flow Description"
  type        = "CONTACT_FLOW"
  content = jsonencode({
    Version     = "2019-10-30"
    StartAction = "12345678-1234-1234-1234-123456789012"
    Actions = [
      {
        Identifier = "12345678-1234-1234-1234-123456789012"
        Type       = "MessageParticipant"

        Transitions = {
          NextAction = "abcdef-abcd-abcd-abcd-abcdefghijkl"
          Errors     = []
          Conditions = []
        }

        Parameters = {
          Text = "Thanks for calling the sample flow!"
        }
      },
      {
        Identifier  = "abcdef-abcd-abcd-abcd-abcdefghijkl"
        Type        = "DisconnectParticipant"
        Transitions = {}
        Parameters  = {}
      }
    ]
  })

  tags = {
    "Name"        = "Test Contact Flow"
    "Application" = "Terraform"
    "Method"      = "Create"
  }
}
```

> [!warning]
> Using exported flows/modules will mess up any hardcoded references to lambdas, lex-bots, etc.
> Prevent this by replacing them with dynamic references:
> ```json
> {
>  "Parameters": {
>    "EventHooks": {
>      "CustomerRemaining": "${aws_connect_contact_flow.disconnect_flow.arn}"
>    }
>  },
>  "Identifier": "a0ea1bfa-e1d0-4939-9b08-1b7338edbfb7",
>  "Type": "UpdateContactEventHooks",
>  "Transitions": {
>    "NextAction": "bd0c652d-b2d0-4132-895a-573ca35d8cf2",
>    "Errors": [
>      {
>        "NextAction": "bd0c652d-b2d0-4132-895a-573ca35d8cf2",
>        "ErrorType": "NoMatchingError"
>      }
>    ]
>  }
>},
> ```

## Logging & Debugging
- It is useful to [[log-contact-flow | enable contact flow logging]] for debugging purposes.

---
## Resources
- [Create a Flow](https://docs.aws.amazon.com/connect/latest/adminguide/connect-contact-flows.html)
- [Terraform Resource: aws_connect_contact_flow](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/connect_contact_flow)