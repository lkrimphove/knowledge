---
title: Logging of Contact Flows
draft: false
tags:
  - terraform
  - amazon-connect
  - logging
  - debugging
  - iac
  - aws
---
# Logging of Contact Flows

- How to enable logs for your [[amazon-connect | Amazon Connect]] contact flows.
- When logging is enabled, data for each block in your flow is sent to Amazon CloudWatch Logs.

## Terraform Coding
Add this to your `aws_connect_contact_flow` in [[Terraform]]:
```json
{
  "Parameters": {
    "FlowLoggingBehavior": "${var.log_lvl == "DEBUG" ? "Enabled" : "Disabled"}"
  },
  "Identifier": "ca9a637a-c375-4ab8-a1cc-83709d21d352",
  "Type": "UpdateFlowLoggingBehavior",
  "Transitions": {
    "NextAction": "ed8efce8-96b0-42ec-9d1c-7bfce1eaaa77"
  }
}
```

> [!Hint]
> This guide assumes you have a variable `log_lvl` that stores your environments Log Level (e.g. `DEBUG`). Otherwise adjust the code to fit your use case or hardcode this by setting `FlowLoggingBehavior` to "Enabled".

## Flow Designer
When deployed using Terraform (or added using the flow designer), you contact flow should look like this:
![[contact-flow-logging-ui.png]]

Logging is now enabled.

> [!Warning]
> Keep in mind that Amazon will charge you for using CloudWatch. There is a free tier, but you will be charged as soon as you exceed their service quotas. Check [CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/).

---

## Resources
- [Flow block: Set logging behavior](https://docs.aws.amazon.com/connect/latest/adminguide/set-logging-behavior.html)
- [Flow logs stored in an Amazon CloudWatch log group](https://docs.aws.amazon.com/connect/latest/adminguide/contact-flow-logs-stored-in-cloudwatch.html)
- [CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)