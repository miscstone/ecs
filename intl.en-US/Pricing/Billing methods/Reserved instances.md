---
keyword: [reserved instance, pay-as-you-go, all upfront, partial upfront, no upfront, no upfront, billing method]
---

# Reserved instances

Reserved instances are independently billed coupons that can be used to offset the bills of pay-as-you-go instances. This topic describes the payment options and billing rules of reserved instances.

## Payment options

Reserved instances cannot be used independently and must be used together with pay-as-you-go instances. Reserved instances can offset the bills of computing resources based on the computing power of the reserved instances. You cannot use reserved instances to offset the bills of network and storage resources of pay-as-you-go instances. For more information, see [Reserved instance overview](/intl.en-US/Instance/Instance purchasing options/Reserved Instances/Reserved instance overview.md).

The following payment options are supported for reserved instances:

-   All Upfront: Full payment is required upfront at purchase. No fees are paid during the term.
-   Partial Upfront: Partial payment \(about 50%\) is required upfront at purchase. The remaining fees are paid on an hourly basis during the term.
-   No Upfront: No payment is required upfront at purchase. All fees are paid on an hourly basis during the term.

    **Note:** Whether you can use the No Upfront option depends on your ECS usage.


For the Partial Upfront and No Upfront options, reserved instances are billed by second. Bills are generated by hour and paid by month. For more information, see the Pricing tab on the [Elastic Compute Service](https://www.alibabacloud.com/product/ecs) page. If the payable amount reaches USD 1,000, the amount is automatically deducted from your account. Any amount less than USD 1,000 is added to the monthly bill.

## Billing rules

After a reserved instance is purchased, it automatically matches and offsets the bills of pay-as-you-go instances on an hourly basis. You are charged based on the payment option that you selected regardless of whether the reserved instance matches pay-as-you-go instances. The All Upfront option is most cost-effective.

A reserved instance takes effect and is billed starting on the hour of your purchase. The reserved instance expires at 24:00:00 of the expiration day. For example, if you purchase a reserved instance with a one-year term at 22:45:00 on May 1, 2020, the reserved instance takes effect and is billed starting from 22:00:00 on May 1, 2020. The reserved instance expires at 24:00:00 on May 2, 2021. If you already have eligible pay-as-you-go instances when you purchase a reserved instance, the reserved instance is applied to offset the bills generated by the pay-as-you-go instances starting from the hour of 22:00 to 23:00 on May 1, 2020 until the reserved instance expires.
