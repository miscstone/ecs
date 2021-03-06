# RunInstances

You can call this operation to create one or more pay-as-you-go or subscription ECS instances.

## Description

-   **Preparations**:
    -   Cost estimation: You have learned about the billing methods of ECS. For more information, see [Billing overview](~~25398~~).
    -   Instance type selection: You have called the [DescribeInstanceTypes](~~25620~~) operation to query the performance data of the instance types to which the instances to be created belong. For more information, see [Select instance types](~~58291~~).
    -   Available resource query: You have called the [DescribeAvailableResource](~~66186~~) operation to query instance availability in the specified region or zone.
    -   Network planning: You have created security groups. For more information, see [CreateSecurityGroup](~~25553~~). Before you create a VPC-type instance, create a VPC in the region where you want to create the instance. For more information, see [Create a VPC](~~65430~~).
-   **Precautions**:
    -   You can create a maximum of 100 instances at a time.
    -   You can use the `AutoReleaseTime` parameter to set the time when the instances are automatically released.
    -   After instances are created, the system returns the ID list of the created instances. You can call the [DescribeInstances](~~25506~~) operation to check the status of the instances.
    -   Instances automatically start after they are created. The instances are ready for use when they are in the `Running` state.
    -   Different from the [CreateInstance](~~25499~~) operation, the `RunInstances` operation enables the system to assign public IP addresses to the new instances if you set the `InternetMaxBandwidthOut` parameter to be greater than 0.
    -   After you call this operation, an error is returned if a parameter is invalid or available resources are insufficient. For more information, see the "Error codes" section in this topic.
-   **Best practices**:
    -   The `RunInstances` operation allows you to create multiple instances at a time. To better manage and search for these instances, we recommend that you specify the same tag pair by using the `Tag.N.Key` and `Tag.N.Value` parameters for the instances. You can also append sequential suffixes specified by the `UniqueSuffix` parameter to the hostnames specified by the `HostName` parameter and instance names specified by the `InstanceName` parameter.
    -   An instance launch template automatically specifies parameters that are required for creating an instance. You can call the [CreateLaunchTemplate](~~74686~~) operation to create an instance launch template. Then, in your request to call the `RunInstances` operation, specify the `LaunchTemplateId` and `LaunchTemplateVersion` parameters to use the launch template.
    -   In the [ECS console](https://ecs.console.aliyun.com/), you can view the best practices for calling the `RunInstances` operation when you create an ECS instance. On the Preview page, click View Open API in the Configurations Selected section. In the dialog box that appears, the left-side **API Workflow** section shows the operations and request parameters that are associated with the `RunInstances` operation. The right-side section shows the SDK examples in the **Java** or **Python** format.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates a sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Ecs&api=RunInstances&type=RPC&version=2014-05-26)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|RunInstances|The operation that you want to perform. Set the value to RunInstances. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. You can call the [DescribeRegions](~~25609~~) operation to query the most recent region list. |
|ImageId|String|No|aliyun\_2\_1903\_x64\_20G\_alibase\_20200324.vhd|The ID of the image used to create the instances. You can call the [DescribeImages](~~25534~~) operation to query the available images. If you do not use the `LaunchTemplateId` or `LaunchTemplateName` parameter to specify the instance launch template, and not set the `ImageFamily` parameter to select the latest available custom images, you must specify the `ImageId` parameter. |
|ImageFamily|String|No|hangzhou-daily-update|The name of the image family. You can set this parameter to obtain the latest available custom images within the specified image family to create ECS instances.

-   If you have set the `ImageId` parameter, you cannot set the ImageFamily parameter.
-   If you have not set the `ImageId` parameter, but you set the `ImageId` parameter in the launch template specified by the `LaunchTemplateId` or `LaunchTemplateName` parameter, you cannot set the ImageFamily parameter.
-   If you have not set the `ImageId` parameter, and not set the `ImageId` parameter in the launch template specified by the `LaunchTemplateId` or `LaunchTemplateName` parameter, you can set the ImageFamily parameter.
-   If you have not set the `ImageId` parameter, and not set the `LaunchTemplateId` and `LaunchTemplateName` parameters, you can set the ImageFamily parameter. |
|InstanceType|String|No|ecs.g6.large|The instance type. If you do not set `LaunchTemplateId` or `LaunchTemplateName` to specify a launch template, you must specify the `InstanceType` parameter.

-   Select instance types: See [Instance families](~~25378~~) or call the [DescribeInstanceTypes](~~25620~~) operation to query the performance data of the instance types to which the instances to be created belong, or see [Select instance types](~~58291~~) to learn how to select instance types.
    -   Query available ECS resources: You can call the [DescribeAvailableResource](~~66186~~) operation to query available resources in the specified region or zone. |
|SecurityGroupId|String|No|sg-bp15ed6xe1yxeycg7\*\*\*\*|The ID of the security group to which the instance belongs. Instances in the same security group can communicate with each other. The number of instances that a security group can contain depends on the type of the security group. For more information, see the "Security group limits" section in [Limits](~~25412~~).

**Note:** The SecurityGroupId parameter determines the network type of instances in the security group. For example, a security group of the VPC type can only contain VPC-type instances, and you must also specify the VSwitchId parameter for the instances.

If you do not specify the `LaunchTemplateId` or `LaunchTemplateName` parameter to set the instance launch template, you must specify the `SecurityGroupId` parameter. |
|SecurityGroupIds.N|RepeatList|No|sg-bp15ed6xe1yxeycg7\*\*\*\*|The IDs of security group N to which the instance belongs. The valid values of N are based on the maximum number of security groups to which an instance can belong. For more information, see the "Security group limits" section in [Limits](~~101348~~).

**Note:** You cannot specify both SecurityGroupId and SecurityGroupIds.N at the same time. |
|VSwitchId|String|No|vsw-bp1s5fnvk4gn2tws0\*\*\*\*|The ID of the VSwitch to which the instance is connected. You must specify this parameter when you create a VPC-type instance. The VSwitch and security group must belong to the same VPC. |
|InstanceName|String|No|k8s-node-\[1,4\]-alibabacloud|The name of the instance. The name must be 2 to 128 characters in length and can contain letters, digits, colons \(:\), underscores \(\_\), periods \(.\), and hyphens \(-\). It must start with a letter and cannot start with http:// or https://. Default value: the `InstanceId` value.

**Note:** When you create multiple ECS instances, you can use `UniqueSuffix` to specify different instance names for the instances. You can also use the `name_prefix[begin_number,bits]name_suffix` format to name sequential instance names. For example, if you set `InstanceName` to `k8s-node-[1,4]-alibabacloud`, the instance name of the first ECS instance is `k8s-node-0001-alibabacloud`. For more information, see [API FAQ](~~122617#howToAddSequentialSuffix~~).

If you do not append suffixes to the instance names or hostnames by using the `name_suffix` format, but only use the `name_prefix[begin_number,bits]` format, the `UniqueSuffix` parameter is ignored. For example, when you set `InstanceName` to `instance-[99,3]` and `UniqueSuffix` to `true`, the instance name is `instance099` instead of `instance099001`. |
|Description|String|No|Instance\_Description|The description of the instance. The description must be 2 to 256 characters in length and cannot start with http:// or https://. |
|InternetMaxBandwidthIn|Integer|No|10|The maximum inbound public bandwidth. Unit: Mbit/s. Valid values:

-   If the purchased outbound public bandwidth is less than or equal to 10 Mbit/s, valid values of this parameter are 1 to 10. Default value: 10.
-   If the purchased outbound public bandwidth is greater than 10 Mbit/s, valid values of this parameter are 1 to the value of the `InternetMaxBandwidthOut` parameter. Default value: the value of the `InternetMaxBandwidthOut` parameter. |
|InternetMaxBandwidthOut|Integer|No|10|The maximum outbound public bandwidth. Unit: Mbit/s. Valid values: 0 to 100.

Default value: 0. |
|HostName|String|No|k8s-node-\[1,4\]-ecshost|The hostname of the instance.

-   The hostname cannot start or end with a period \(.\) or hyphen \(-\). It cannot contain consecutive periods \(.\) or hyphens \(-\).
-   For Windows instances, the hostname must be 2 to 15 characters in length and can contain letters, digits, and hyphens \(-\). The hostname cannot contain periods \(.\) or only contain digits.
-   For instances that run other operating systems such as Linux, the hostname must be 2 to 64 characters in length. You can use periods \(.\) to separate the hostname into multiple segments. Each segment can contain letters, digits, and hyphens \(-\).

**Note:** When you create multiple ECS instances, you can use the `UniqueSuffix` parameter to set different hostnames for these instances. You can also use the `name_prefix[begin_number,bits]name_suffix` format to name sequential hostnames. For example, if you set `HostName` to `k8s-node-[1,4]-ecshost`, the hostname of the first ECS instance is `k8s-node-0001-ecshost`. For more information, see [API FAQ](~~122617#howToAddSequentialSuffix~~).

If you do not append suffixes to the instance names or hostnames by using the `name_suffix` format, but only use the `name_prefix[begin_number,bits]` format, the `UniqueSuffix` parameter is ignored. For example, when you set `InstanceName` to `instance-[99,3]` and `UniqueSuffix` to `true`, the instance name is `instance099` instead of `instance099001`. |
|UniqueSuffix|Boolean|No|true|Specifies whether to add sequential suffixes to the hostnames specified by the `HostName` parameter and instance names specified by the `InstanceName` parameter. The sequential suffixes range from 001 to 999. For example, the hostnames can be `LocalHost001` and `LocalHost002`. The instance names can be `MyInstance001` and `MyInstance002`. Valid values:

-   true
-   false

Default value: false. |
|Password|String|No|EcsV587!|The password of the instance. The password must be 8 to 30 characters in length. It must include at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters. Special characters include

```

( ) ` ~ ! @ # $ % ^ & * - _ + = | { } [ ] : ; ' < > , . ? /
                                
```

Passwords of Windows instances cannot start with a forward slash \(/\).

**Note:** If the `Password` parameter is specified, we recommend that you send requests over HTTPS to secure your password. |
|PasswordInherit|Boolean|No|false|Specifies whether to use the password predefined in the image. Valid values:

-   true
-   false

**Note:** To use the PasswordInherit parameter, the Password parameter must be empty and you must make sure that the selected image has a password configured. |
|ZoneId|String|No|cn-hangzhou-g|The zone ID of the instance. You can call the [DescribeZones](~~25610~~) operation to query the zone list.

If you do not specify a zone ID, the system randomly selects a zone. |
|InternetChargeType|String|No|PayByTraffic|The billing method for network usage. Default value: PayByTraffic. Valid values:

-   PayByBandwidth: pay-by-bandwidth.
-   PayByTraffic: pay-by-traffic.

**Note:** When the pay-by-traffic billing method is used, the peak inbound and outbound bandwidths indicate the upper limits for bandwidths and are only for reference. In the event of resource contention, the peak bandwidths cannot be guaranteed. If you want guaranteed bandwidths for your instance, use the pay-by-bandwidth billing method. |
|SystemDisk.Size|String|No|40|The size of the system disk. Unit: GiB. Valid values: 20 to 500.

The value of this parameter must be at least 20 and greater than or equal to the size of the image.

Default value: 40 or the size of the image, depending on whichever is greater. |
|SystemDisk.Category|String|No|cloud\_ssd|The category of the system disk. Valid values:

-   cloud\_efficiency: ultra disk
-   cloud\_ssd: standard SSD
-   cloud\_essd: enhanced SSD \(ESSD\)
-   cloud: basic disk

For non-I/O optimized instances of retired instance types, the default value is cloud. For other instances, the default value is cloud\_efficiency. |
|SystemDisk.DiskName|String|No|cloud\_ssdSystem|The name of the system disk. The name must be 2 to 128 characters in length and can contain letters, digits, periods \(.\), colons \(:\), underscores \(\_\), and hyphens \(-\). It must start with a letter and cannot start with http:// or https://.

This parameter is empty by default. |
|SystemDisk.Description|String|No|SystemDisk\_Description|The description of the system disk. The description must be 2 to 256 characters in length and cannot start with http:// or https://. |
|SystemDisk.PerformanceLevel|String|No|PL1|The performance level of the enhanced SSD used as the system disk. Default value: PL1. Valid values:

-   PL1: A single ESSD can deliver up to 50,000 random read/write IOPS.
-   PL2: A single ESSD can deliver up to 100,000 random read/write IOPS.
-   PL3: A single ESSD can deliver up to 1,000,000 random read/write IOPS.

For more information about ESSD performance levels, see [Enhanced SSD \(ESSD\)](~~122389~~). |
|SystemDisk.AutoSnapshotPolicyId|String|No|sp-bp67acfmxazb4p\*\*\*\*|The ID of the automatic snapshot policy to be applied to the system disk. |
|DataDisk.N.Size|Integer|No|2000|The size of data disk N. Valid values of N: 1 to 16. Unit: GiB. Valid values:

-   If the data disk is an ultra disk, the valid values are 20 to 32768.
-   If the data disk is a standard SSD, the valid values are 20 to 32768.
-   If the data disk is an ESSD, the valid values are 20 to 32768.
-   If the data disk is a local SSD, the valid values are 5 to 800.
-   If the data disk is a basic disk, the valid values are 5 to 2000.

The value of this parameter must be greater than or equal to the size of the snapshot specified by the `SnapshotId` parameter. |
|DataDisk.N.SnapshotId|String|No|s-bp17441ohwka0yuh\*\*\*\*|The ID of the snapshot used to create data disk N. Valid values of N: 1 to 16.

When the `DataDisk.N.SnapshotId` parameter is specified, the `DataDisk.N.Size` parameter is ignored. The data disk will be created based on the size of the specified snapshot. Use snapshots created after July 15, 2013. Otherwise, an error message will be returned and your request will be rejected. |
|DataDisk.N.Category|String|No|cloud\_ssd|The category of data disk N. Valid values:

-   cloud\_efficiency: ultra disk
-   cloud\_ssd: standard SSD
-   ephemeral\_ssd: local SSD
-   cloud\_essd: ESSD
-   cloud: basic disk

For I/O optimized instances, the default value is cloud\_efficiency. For non-I/O optimized instances, the default value is cloud. |
|DataDisk.N.Encrypted|String|No|false|Specifies whether data disk N is encrypted. Valid values:

-   true: The disk is encrypted.
-   false: The disk is not encrypted.

Default value: false. |
|DataDisk.N.KMSKeyId|String|No|0e478b7a-4262-4802-b8cb-00d3fb40\*\*\*\*|The KMS key ID for data disk N. |
|DataDisk.N.DiskName|String|No|cloud\_ssdData|The name of data disk N. The name must be 2 to 128 characters in length and can contain letters, digits, periods \(.\), colons \(:\), underscores \(\_\), and hyphens \(-\). It must start with a letter and cannot start with http:// or https://.

This parameter is empty by default. |
|DataDisk.N.Description|String|No|DataDisk\_Description|The description of data disk N. The description must be 2 to 256 characters in length and cannot start with http:// or https://. |
|DataDisk.N.Device|String|No|null|The mount point of data disk N.

**Note:** This parameter will be removed in the future. We recommend that you use other parameters to ensure future compatibility. |
|DataDisk.N.DeleteWithInstance|Boolean|No|true|Specifies whether to release data disk N together with the instance. Valid values:

-   true: Data disk N is released together with the instance.
-   false: Data disk N is not released together with the instance.

Default value: true. |
|DataDisk.N.PerformanceLevel|String|No|PL2|The performance level of the ESSD used as data disk N. The N value must be the same as that in DataDisk.N.Category when `DataDisk.N.Category` is set to cloud\_essd. Default value: PL1. Valid values:

-   PL1: A single ESSD can deliver up to 50,000 random read/write IOPS.
-   PL2: A single ESSD can deliver up to 100,000 random read/write IOPS.
-   PL3: A single ESSD can deliver up to 1,000,000 random read/write IOPS.

For more information about ESSD performance levels, see [Enhanced SSD \(ESSD\)](~~122389~~). |
|DataDisk.N.AutoSnapshotPolicyId|String|No|sp-bp67acfmxazb4p\*\*\*\*|The ID of the automatic snapshot policy to be applied to data disk N. |
|IoOptimized|String|No|optimized|Specifies whether the instance is I/O optimized. For instances of retired instance types, the default value is none. For other instances, the default value is optimized. For more information, see [Retired instance types](~~55263~~). Valid values:

-   none: The instance is not I/O optimized.
-   optimized: The instance is I/O optimized. |
|NetworkInterface.N.PrimaryIpAddress|String|No|172.16.236.\*\*\*|The primary IP address of the secondary Elastic Network Interface \(ENI\) N. Set the value of N to 1.

**Note:** You can attach only one secondary ENI when you are creating an ECS instance. After the instance is created, you can call the [CreateNetworkInterface](~~58504~~) and [AttachNetworkInterface](~~58515~~) operations to attach more secondary ENIs.

Default value: an IP address that is randomly selected from within the CIDR block of the VSwitch to which the ENI belongs. |
|NetworkInterface.N.VSwitchId|String|No|vsw-bp67acfmxazb4p\*\*\*\*|The ID of the VSwitch to which secondary ENI N belongs. Set the value of N to 1.

Default value: the ID of the VSwitch to which the ECS instance belongs. |
|NetworkInterface.N.SecurityGroupId|String|No|sg-bp67acfmxazb4p\*\*\*\*|The ID of the security group to which the secondary ENI N belongs. Set the value of N to 1.

Default value: the ID of the security group to which the instance will belong. |
|NetworkInterface.N.SecurityGroupIds.N|RepeatList|No|sg-bp15ed6xe1yxeycg7\*\*\*\*|ID N of the security group to which secondary ENI N belongs. The valid values of N in `SecurityGroupIds.N` are based on the maximum number of security groups to which the instance equipped with the ENI can belong. For more information, see [Limits](~~25412~~).

**Note:** You cannot specify `NetworkInterface.N.SecurityGroupId` and `NetworkInterface.N.SecurityGroupIds.N` at the same time. |
|NetworkInterface.N.NetworkInterfaceName|String|No|Network\_Name|The name of secondary ENI N. Set the value of N to 1. |
|NetworkInterface.N.Description|String|No|Network\_Description|The description of secondary ENI N. Set the value of N to 1. The description must be 2 to 256 characters in length and cannot start with http:// or https://. |
|NetworkInterface.N.QueueNumber|Integer|No|8|The queue number of secondary ENI N. Set the value of N to 1.

-   The value of this parameter cannot exceed the maximum queue number for one ENI allowed for the instance type.
-   The total number of queues for all ENIs on the instance cannot exceed the service queue quota allowed for the instance type. To learn the maximum queue number for one ENI and queue quota allowed for an instance type, you can call the [DescribeInstanceTypes](~~25620~~) operation to query the `MaximumQueueNumberPerEni` and `TotalEniQueueQuantity` fields. |
|UserData|String|No|ZWNobyBoZWxsbyBlY3Mh|The user data of the instance. User data must be encoded in Base64. The maximum size of raw data is 16 KB. |
|KeyPairName|String|No|KeyPair\_Name|The name of the key pair to be bound to the instance.

-   For Windows instances, this parameter is ignored and is empty by default. The `Password` parameter takes effect even if the KeyPairName parameter is specified.
-   For Linux instances, the username and password logon method is disabled by default. |
|RamRoleName|String|No|RAM\_Name|The name of the RAM role to be attached. You can call the [ListRoles](~~28713~~) operation provided by RAM to query the RAM roles that you have created. |
|Amount|Integer|No|3|The number of instances you want to create. Valid values: 1 to 100.

Default: 1. |
|MinAmount|Integer|No|2|The minimum number of the instances that can be created. Valid values: 1 to 100.

-   If the number of available ECS instances is less than the MinAmount value, the instance creation operation fails.
-   If the number of available ECS instances is greater than or equal to the MinAmount value, the instance creation operation succeeds and the number of instances that are created equals the total available instances. |
|AutoReleaseTime|String|No|2018-01-01T12:05:00Z|The automatic release time of the pay-as-you-go instance. Specify the time in the [ISO 8601](~~25696~~) standard in the yyyy-MM-ddTHH:mm:ssZ format. The time must be in UTC.

-   If the value of seconds \(`ss`\) is not `00`, it is automatically set to the start time of the current minute \(`mm`\).
-   The release time must be at least 30 minutes later than the current time.
-   The release time must be at most three years from the current time. |
|SpotStrategy|String|No|NoSpot|The preemption policy for a preemptible or pay-as-you-go instance. This parameter takes effect only when the `InstanceChargeType` parameter is set to `PostPaid`. Default value: NoSpot. Valid values:

-   NoSpot: applies to regular pay-as-you-go instances.
-   SpotWithPriceLimit: applies to preemptible instances that have maximum hourly prices.
-   SpotAsPriceGo: applies to pay-as-you-go instances priced at the market price at the time of purchase. |
|SpotDuration|Integer|No|1|The retention period of a preemptible instance. Unit: hours. Valid values: 0, 1, 2, 3, 4, 5, and 6.

-   The values 2, 3, 4, 5, and 6 of this parameter are in invitational preview. Submit a ticket to enable these values.
-   If this parameter is set to 0, the created preemptible instance has no protection period.

Default: 1. |
|SpotPriceLimit|Float|No|0.97|The maximum hourly price of the instance. This parameter takes effect only when `SpotStrategy` is set to `SpotWithPriceLimit`. A maximum of three decimal places are allowed. |
|SpotInterruptionBehavior|String|No|Terminate|The interruption mode of the preemptible instance. Set the value to Terminate, which specifies that the instance is released. |
|SecurityEnhancementStrategy|String|No|Active|Specifies whether to enable security enhancement. Valid values:

-   Active: enables security enhancement. This value is applicable only to public images.
-   Deactive: disables security enhancement. This value is applicable to all image types. |
|ClientToken|String|No|123e4567-e89b-12d3-a456-426655440000|The client token that is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The **ClientToken** value can only contain ASCII characters and cannot exceed 64 characters in length. For more information, see [How to ensure idempotence](~~25693~~). |
|Tag.N.Key|String|No|TestKey|The key of tag N to be bound to the instance, disks, and primary ENI. Valid values of N: 1 to 20. The tag key cannot be an empty string. It can be up to 128 characters in length and cannot start with acs: or aliyun. It cannot contain http:// or https://. |
|Tag.N.Value|String|No|TestValue|The value of tag N to be bound to the instance, disks, and the primary ENI. Valid values of N: 1 to 20. The tag value can be an empty string. It can be up to 128 characters in length and cannot start with acs:. It cannot contain http:// or https://. |
|HpcClusterId|String|No|hpc-bp67acfmxazb4p\*\*\*\*|The ID of the E-HPC cluster to which the instance belongs. |
|DryRun|Boolean|No|false|Specifies whether to check the validity of the request without actually making the request. Default value: false. Valid values:

-   true: The validity of the request is checked but the request is not made. Check items include the required parameters, request format, service limits, and available ECS resources. If the check fails, the corresponding error code is returned. If the check succeeds, the `DryRunOperation` error code is returned.
-   false: The validity of the request is checked. If the check succeeds, the request is made. |
|DedicatedHostId|String|No|dh-bp67acfmxazb4p\*\*\*\*|The ID of the dedicated host for the ECS instance. If the `DedicatedHostId` parameter is specified, the `SpotStrategy` and `SpotPriceLimit` parameters are ignored. This is because preemptible instances cannot be created on dedicated hosts.

You can call the [DescribeDedicatedHosts](~~134242~~) operation to query the dedicated host list. |
|LaunchTemplateId|String|No|lt-bp1apo0bbbkuy0rj\*\*\*\*|The ID of the launch template. For more information, see [DescribeLaunchTemplates](~~73759~~).

To use a launch template to create an instance, you must specify the launch template by using the `LaunchTemplateId` or `LaunchTemplateName` parameter. |
|LaunchTemplateName|String|No|LaunchTemplate\_Name|The name of the launch template.

To use a launch template to create an instance, you must specify the launch template by using the `LaunchTemplateId` or `LaunchTemplateName` parameter. |
|LaunchTemplateVersion|Long|No|3|The version of the launch template. If you specify `LaunchTemplateId` or `LaunchTemplateName` but do not specify the version number of the launch template, the default template version is used. |
|ResourceGroupId|String|No|rg-bp67acfmxazb4p\*\*\*\*|The ID of the resource group to which the instance belongs. |
|Period|Integer|No|1|The subscription period of the instance. The unit is specified by the `PeriodUnit` parameter. This parameter is valid and is required only when the `InstanceChargeType` parameter is set to `PrePaid`. If the `DedicatedHostId` parameter is specified, the specified period cannot exceed the subscription period of the dedicated host of the instance. Valid values:

-   If the PeriodUnit parameter is set to Month, valid values are 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, 36, 48, and 60. |
|PeriodUnit|String|No|Month|The unit of the subscription period. Default value: Month. Valid values:

-   Month |
|AutoRenew|Boolean|No|true|Specifies whether to enable auto-renewal for the instance. This parameter is valid only when `InstanceChargeType` is set to `PrePaid`. Default value: false. Valid values:

-   true: Auto-renewal is enabled for the instance.
-   false: Auto-renewal is disabled for the instance. |
|AutoRenewPeriod|Integer|No|1|The auto-renewal period of the instance. Valid values:

-   If the PeriodUnit parameter is set to Month, valid values are 1, 2, 3, 6, 12, 24, 36, 48, and 60.

Default: 1. |
|InstanceChargeType|String|No|PrePaid|The billing method of the instance. Default value: PostPaid. Default value: PostPaid. Valid values:

-   PrePaid: subscription
-   PostPaid: pay-as-you-go

If you set this parameter to PrePaid, ensure that your account has sufficient credit. Otherwise, an `InvalidPayMethod` error is returned. |
|DeploymentSetId|String|No|ds-bp1brhwhoqinyjd6\*\*\*\*|The ID of the deployment set. |
|PrivateIpAddress|String|No|10.1.2.1|The private IP address of the instance.

To allocate a private IP address to a VPC-type ECS instance, ensure that the IP address is an idle IP address within the CIDR block of the VSwitch specified by the `VSwitchId` parameter.

**Note:** When you specify `PrivateIpAddress`, the value of the `Amount` parameter must be set to 1. |
|CreditSpecification|String|No|Standard|The performance mode of the burstable instance. Valid values:

-   Standard: the standard mode. For more information, see the "Standard mode" section in [Burstable instances](~~59977~~).
-   Unlimited: the unlimited mode. For more information, see the "Unlimited mode" section in [Burstable instances](~~59977~~).

This parameter is empty by default. |
|Ipv6Address.N|RepeatList|No|Ipv6Address.1=2001:db8:1234:1a00::\*\*\*|IPv6 address N to be assigned to the primary ENI. Set the value of N to 1. Example: `Ipv6Address.1=2001:db8:1234:1a00::***`.

**Note:** When the `Ipv6Address.N` parameter is specified, the value of the `Amount` parameter must be set to 1 and the `Ipv6AddressCount` parameter must not be specified. |
|Ipv6AddressCount|Integer|No|1|The number of IPv6 addresses to be randomly generated for the primary ENI.

**Note:** You cannot specify the `Ipv6Addresses.N` and `Ipv6AddressCount` parameters at the same time. |
|NetworkInterfaceQueueNumber|Integer|No|8|The queue number of the primary ENI.

-   The value of this parameter cannot exceed the maximum queue number for one ENI allowed for the instance type.
-   The total number of queues for all ENIs on the instance cannot exceed the service queue quota allowed for the instance type. To learn the maximum queue number for one ENI and queue quota allowed for an instance type, you can call the [DescribeInstanceTypes](~~25620~~) operation to query the `MaximumQueueNumberPerEni` and `TotalEniQueueQuantity` fields. |
|DeletionProtection|Boolean|No|false|The release protection property of the instance. It specifies whether you can use the ECS console or call the [DeleteInstance](~~25507~~) operation to manually release the instance. Default value: false. Valid values:

-   true: Release protection is enabled for the instance.
-   false: Release protection is disabled for the instance.

**Note:** This parameter is applicable only to pay-as-you-go instances. It can protect instances only against manual releases, not against system releases. |
|Affinity|String|No|default|Specifies whether the instance on a dedicated host is associated with the dedicated host. Valid values:

-   default: The instance is not associated with the dedicated host. With the No Fees for Stopped Instances \(VPC-Connected\) feature enabled, when the instance is restarted after being stopped, it is automatically deployed to another dedicated host in the automatic deployment resource pool if the resources of original dedicated host are insufficient.
-   host: The instance is associated with the dedicated host. With the No Fees for Stopped Instances \(VPC-Connected\) feature enabled, when the instance is restarted after being stopped, it still resides on the original dedicated host. If the resources of the original dedicated host are insufficient, the instance will fail to be restarted.

Default value: default. |
|Tenancy|String|No|default|Specifies whether to create the instance on a dedicated host. Valid values:

-   default: creates the instance on a non-dedicated host.
-   host: creates the instance on a dedicated host. If you do not specify the `DedicatedHostId` parameter, Alibaba Cloud automatically selects a dedicated host for the instance.

Default value: default. |
|StorageSetId|String|No|ss-bp67acfmxazb4p\*\*\*\*|The ID of the storage set. |
|StorageSetPartitionNumber|Integer|No|2|The maximum number of partitions in a storage set. Valid values: greater than or equal to 2. |
|CpuOptions.Core|Integer|No|2|The number of CPU cores. This parameter cannot be specified but only uses its default value. |
|CpuOptions.ThreadsPerCore|Integer|No|2|The number of threads per core. The following formula is used to calculate the number of vCPUs of the ECS instance: `CpuOptions.Core` value × `CpuOptions.ThreadPerCore` value.

-   If the value of `CpuOptions.ThreadPerCore` is set to 1, CPU hyper-threads are disabled.
-   This parameter is applicable only to some instance types. |
|CpuOptions.Numa|String|No|1|The number of CPU Numa nodes. |
|HttpEndpoint|String|No|enabled|Specifies whether to enable the metadata endpoint on the instance. Valid values:

-   enabled
-   disabled

Default value: enabled.

**Note:** For more information about instance metadata, see [Overview of instance metadata](~~49122~~). |
|HttpTokens|String|No|optional|Specifies whether to forcibly use the security-enhanced mode \(IMDSv2\) to access instance metadata. Valid values:

-   optional: The security-enhanced mode \(IMDSv2\) is not forcibly used.
-   required: The security-enhanced mode \(IMDSv2\) is forcibly used. After you set this parameter to required, you cannot access the instance metadata in normal mode.

Default value: optional.

**Note:** For more information about modes of accessing instance metadata, see [Modes of accessing instance metadata](~~150575~~). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|InstanceIdSets|List|\["i-bp67acfmxazb4pd2\*\*\*\*", "i-bp1i43l28m7u48p1\*\*\*\*", "i-bp12yqg7jdyxl11f\*\*\*\*"\]|The list of instance IDs \(`InstanceIdSet`\). |
|RequestId|String|473469C7-AA6F-4DC5-B3DB-A3DC0DE3C83E|The ID of the request. |
|TradePrice|Float|0.165|The transaction price. |

## Examples

Sample requests

```
https://ecs.aliyuncs.com/?Action=RunInstances
&RegionId=cn-hangzhou
&ImageId=aliyun_2_1903_x64_20G_alibase_20200324.vhd
&InstanceType=ecs.g6.large
&SecurityGroupId=sg-bp67acfmxazb4p****
&VSwitchId=vsw-bp1s5fnvk4gn2tws0****
&SystemDisk.AutoSnapshotPolicyId=sp-bp67acfmxazb4p****
&Amount=3
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RunInstancesResponse>
      <InstanceIdSets>
            <InstanceIdSet>i-bp67acfmxazb4pd2****</InstanceIdSet>
            <InstanceIdSet>i-bp1i43l28m7u48p1****</InstanceIdSet>
            <InstanceIdSet>i-bp12yqg7jdyxl11f****</InstanceIdSet>
      </InstanceIdSets>
      <RequestId>04F0F334-1335-436C-A1D7-6C044FE73368</RequestId>
</RunInstancesResponse>
```

`JSON` format

```
{
    "InstanceIdSets": {
        "InstanceIdSet": [
            "i-bp67acfmxazb4pd2****",
            "i-bp1i43l28m7u48p1****",
            "i-bp12yqg7jdyxl11f****"
        ]
    },
    "RequestId": "04F0F334-1335-436C-A1D7-6C044FE73368"
}
```

## Error codes

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|400|InvalidInstanceType.ValueUnauthorized|The specified InstanceType is not authorized.|The error message returned because you are not authorized to use the specified instance type.|
|404|InvalidSecurityGroupId|The specified SecurityGroupId is invalid or does not exist.|The error message returned because the specified security group ID is invalid or does not exist.|
|400|InvalidInstanceType.ValueNotSupported|The specified InstanceType beyond the permitted range.|The error message returned because the specified InstanceType parameter is invalid.|
|404|InvalidSecurityGroupId.NotFound|The SecurityGroupId provided does not exist in our records.|The error message returned because the specified security group does not exist in this account. Check whether the security group ID is correct.|
|400|InvalidDescription.Malformed|The specified parameter "Description" is not valid.|The error message returned because the specified Description parameter is invalid. The description must be 2 to 256 characters in length and cannot start with http:// or https://.|
|400|InvalidInternetChargeType.ValueNotSupported|The specified InternetChargeType is not valid.|The error message returned because the specified InternetChargeType parameter is invalid.|
|400|InvalidParameter|The specified parameter "InternetMaxBandwidthOut" is not valid.|The error message returned because the specified InternetMaxBandwidthOut parameter is invalid.|
|400|InvalidHostName.Malformed|The specified parameter "HostName" is not valid.|The error message returned because the specified HostName parameter is invalid.|
|403|InvalidParams.InstanceNameExceed|The uniqueSuffix takes three naming places, please shorten your InstanceName.|The error message returned because the UniqueSuffix parameter occupies three namespaces. Shorten the instance name.|
|403|InvalidParams.HostnameExceed|The uniqueSuffix takes three naming places, please shorten your Hostname.|The error message returned because the UniqueSuffix parameter occupies three namespaces. Shorten the hostname.|
|400|InvalidPassword.Malformed|The specified parameter "Password" is not valid.|The error message returned because the specified Password parameter is invalid.|
|400|InvalidPasswordParam.Mismatch|The input password should be null when passwdInherit is true.|The error message returned because the Password parameter is specified when PasswdInherit is set to true.|
|400|InvalidSystemDiskCategory.ValueNotSupported|The specified parameter "SystemDisk.Category" is not valid.|The error message returned because the specified SystemDisk.Category parameter is invalid.|
|400|InvalidDiskName.Malformed|The specified parameter "SyatemDisk.DiskName or DataDisk.n.DiskName" is not valid.|The error message returned because the specified SystemDisk.DiskName or DataDisk.N.DiskName parameter is invalid. The name must be 2 to 128 characters in length and can contain letters, digits, periods \(.\), underscores \(\_\), and hyphens \(-\). It must start with a letter but cannot start with http:// or https://.|
|400|InvalidDiskDescription.Malformed|The specified parameter "SyatemDisk.DiskDescription" or "DataDisk.n.Description" is not valid.|The error message returned because the specified SystemDisk.Description or DataDisk.N.Description parameter is invalid.|
|400|InvalidDataDiskSize.ValueNotSupported|The specified DataDisk.n.Size beyond the permitted range, or the capacity of snapshot exceeds the size limit of the specified disk category.|The error message returned because the specified DataDisk.N.Size parameter is invalid or the snapshot size exceeds the maximum capacity that is allowed for the specified disk category.|
|400|InvalidDataDiskCategory.ValueNotSupported|The specified parameter "DataDisk.n.Category" is not valid.|The error message returned because the specified value of the DataDisk.N.Category parameter is invalid.|
|404|InvalidDataDiskSnapshotId.NotFound|The specified parameter "DataDisk.n.SnapshotId" is not valid.|The error message returned because the specified DataDisk.N.SnapshotId parameter is invalid.|
|400|InvalidDataDevice.Malformed|The specified parameter "DataDisk.n.Device" is not valid.|The error message returned because the specified DataDisk.N.Device parameter is invalid.|
|400|InvalidNodeControllerId.Malformed|The specified parameter "NodeControllerId" is not valid.|The error message returned because the specified value of the NodeControllerId parameter is invalid.|
|400|InvalidInnerIpAddress.Malformed|The specified parameter "InnerIpAddress" is not valid.|The error message returned because the specified InnerIpAddress parameter is invalid.|
|400|InvalidInnerIpAddress.Unusable|The specified InnerIpAddress is already used or not found in usable ip range.|The error message returned because the specified internal IP address is unavailable.|
|400|InvalidParameter.Conflict|The specified image does not support the specified instance type.|The error message returned because the specified image does not support the specified instance type.|
|400|ImageNotSupportCloudInit|The specified image does not support cloud-init.|The error message returned because cloud-init is not supported by the specified image.|
|400|InvalidSnapshotId.BasedSnapshotTooOld|The specified snapshot is created before 2013-07-15.|The error message returned because the specified snapshot was created before July 15, 2013.|
|400|QuotaExceed.AfterpayInstance|Living afterpay instances quota exceeded.|The error message returned because the maximum number of active pay-as-you-go instances has been reached.|
|403|ImageNotSubscribed|The specified image has not be subscribed.|The error message returned because you have not subscribed to the specified Alibaba Cloud Marketplace image.|
|403|InvalidSystemDiskCategory.ValueUnauthorized|The disk category is not authorized.|The error message returned because you are not authorized to use the specified disk category.|
|403|InvalidSnapshotId.NotReady|The specified snapshot has not completed yet.|The error message returned because the specified snapshot has not been created.|
|403|InstanceDiskCategoryLimitExceed|The total size of specified disk category in an instance exceeds.|The error message returned because the total size of the specified disks exceeds the maximum capacity that is allowed for the instance type.|
|403|InvalidDevice.InUse|The specified device has been occupied.|The error message returned because the specified device already has an attached disk.|
|403|ImageRemovedInMarket|The specified market image is not available, Or the specified user defined image includes product code because it is based on an image subscribed from marketplace, and that image in marketplace includeing exact the same product code has been removed.|The error message returned because the specified Alibaba Cloud Marketplace image is unavailable, or the specified custom image contains the product code of the Alibaba Cloud Marketplace image from which the custom image is derived and the Alibaba Cloud Marketplace image has been removed from Alibaba Cloud Marketplace.|
|404|InvalidClusterId.NotFound|The ClusterId provided does not exist in our records.|The error message returned because the specified ClusterId parameter does not exist.|
|403|CategoryNotSupported|The specified zone does not offer the specified disk category.|The error message returned because the specified disk category is unavailable in the specified zone.|
|400|InvalidInstanceName.Malformed|The specified parameter "InstanceName" is not valid.|The error message returned because the specified InstanceName parameter is invalid. The name must be 2 to 128 characters in length and can contain letters, digits, periods \(.\), underscores \(\_\), and hyphens \(-\). It must start with a letter but cannot start with http:// or https://.|
|400|InvalidDiskDescription.Malformed|The specified parameter "SyatemDisk.DiskDescription or DataDisk.n.Description" is not valid.|The error message returned because the specified SystemDisk.Description or DataDisk.N.Description parameter is invalid.|
|403|QuotaExceed.PortableCloudDisk|The quota of portable cloud disk exceeds.|The error message returned because the maximum number of removable disks has been reached.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because an internal error has occurred. Try again later. If the problem persists, submit a ticket.|
|400|InvalidParameter.Conflict|The speicified region and cluster do not match.|The error message returned because the specified region and the specified cluster do not correspond to each other.|
|403|SecurityGroupInstanceLimitExceed|Exceeding the allowed amount of instances of a security group.|The error message returned because the maximum number of instances in the security group has been reached.|
|403|NodeControllerUnavailable|The Node Controller is temporarily unavailable.|The error message returned because the node controller is unavailable.|
|403|RegionUnauthorized|There is no authority to create instance in the specified region.|The error message returned because you are not authorized to create instances in the specified region.|
|403|CategoryNotSupported|The specified Zone or cluster does not offer the specified disk category.|The error message returned because the specified disk category is unavailable in the specified zone or cluster.|
|403|InvalidSnapshotId.NotDataDiskSnapshot|The specified snapshot is system disk snapshot.|The error message returned because the specified snapshot is a system disk snapshot.|
|403|CategoryNotSupported|The specified cluster does not offer the specified disk category.|The error message returned because the specified disk category is unavailable in the specified cluster.|
|404|InvalidVSwitchId.NotFound|Specified virtual switch does not exist.|The error message returned because the specified VSwitchId parameter does not exist.|
|404|InvalidVSwitchId.NotFound|%s|The error message returned because the specified VSwitchId parameter does not exist.|
|400|InvalidParameter.Mismatch|Specified security group and virtual switch are not in the same VPC.|The error message returned because the specified security group and VSwitch do not belong to the same VPC.|
|400|InvalidNetworkType.Mismatch|Specified parameter "InternetMaxBandwidthIn" or "InternetMaxBandwidthOut" conflict with instance network type.|The error message returned because the specified InternetMaxBandwidthIn or InternetMaxBandwidthOut parameter conflicts with the instance network type.|
|400|InvalidNetworkType.Mismatch|Specified parameter "InternetChargeType" conflict with instance network type.|The error message returned because the network type of the instance does not support the specified billing method for network usage.|
|400|InvalidPrivateIpAddress|Specified private IP address is not in the CIDR block of virtual switch.|The error message returned because the specified private IP address is not within the CIDR block of the VSwitch.|
|400|InvalidPrivateIpAddress.Malformed|Specified private IP address is malformed.|The error message returned because the specified private IP address is invalid.|
|400|InvalidPrivateIpAddress.Duplicated|Specified private IP address is duplicated.|The error message returned because the specified private IP address has been used. Change the IP address and try again.|
|400|QuotaExceeded.PrivateIpAddress|Don't have enough private IPs in this switch|The error message returned because all the private IP addresses in the CIDR block of the specified VSwitch are already in use. Try another VSwitch.|
|400|QuotaExceeded|Living instances quota exceeded in this VPC.|The error message returned because the maximum number of active instances has been reached.|
|400|IncorrectVSwitchStatus|The current status of virtual switch does not support this operation.|The error message returned because the specified VSwitch is in the Pending state and cannot be deleted.|
|400|InvalidParameter.Mismatch|Specified virtual switch is not in the specified zone.|The error message returned because the specified VSwitch does not exist in the specified zone.|
|400|ResourceNotAvailable|Resource you requested is not available in this region or zone.|The error message returned because VPCs are not supported by the specified region or zone.|
|400|MissingParameter|The input parameter "VSwitchId" that is mandatory for processing this request is not supplied.|The error message returned because the required VSwitchId parameter is not specified.|
|400|InvalidDiskCategory.Mismatch|The specified disk categories' combination is not supported.|The error message returned because the specified combination of disk categories is not supported.|
|403|Forbbiden|User not authorized to operate on the specified resource.|The error message returned because you are not authorized to perform operations on the specified resource.|
|403|DeleteWithInstance.Conflict|The specified disk is not a portable disk and cannot be set to DeleteWithInstance attribute.|The error message returned because the specified disk is not removable and cannot be released along with the instance.|
|404|InvalidImageId.NotFound|The specified ImageId does not exist.|The error message returned because the specified image does not exist in this account. Check whether the image ID is correct.|
|403|InstanceDiskNumLimitExceed|The number of specified disk in an instance exceeds.|The error message returned because the maximum number of disks that can be attached to the instance has been reached.|
|403|IoOptimized.NotSupported|The specified image is not support IoOptimized Instance.|The error message returned because the specified image does not support I/O optimized instances.|
|404|IoOptimized.NotSupported|The specified instancetype is not support IoOptimized instance|The error message returned because the specified instance type does not support I/O optimization.|
|403|InvalidDiskSize.TooSmall|Specified disk size is less than the size of snapshot|The error message returned because the size of the specified disk is smaller than that of the snapshot.|
|404|InvalidInstanceChargeType.NotFound|The InstanceChargeType does not exist in our records|The error message returned because the specified InstanceChargeType parameter does not exist.|
|400|MissingParamter|The specified parameter "Period" is not null.|The error message returned because the Period parameter is not specified.|
|400|InvalidPeriod|The specified period is not valid.|The error message returned because the specified Period parameter is invalid.|
|403|InvalidDiskCategory.Mismatch|The specified disk categories combination is not supported.|The error message returned because the specified combination of disk categories is not supported.|
|403|IoOptimized.NotSupported|Vpc is not support IoOptimized instance.|The error message returned because I/O optimized instances are not supported in the VPC.|
|400|InvalidDataDiskCategory.ValueNotSupported|The specified parameter " DataDisk.n.Category " is not valid.|The error message returned because the specified DataDisk.N.Category parameter is invalid.|
|400|InvalidSystemDiskCategory.ValueNotSupported|The specified parameter " SystemDisk.Category " is not valid.|The error message returned because the specified SystemDisk.Category parameter is invalid.|
|403|InvalidDiskCategory.NotSupported|The specified disk category is not support the specified instance type.|The error message returned because the specified disk category does not support the instance type.|
|400|InstanceDiskCategoryLimitExceed|The specified DataDisk.n.Size beyond the permitted range, or the capacity of snapshot exceeds the size limit of the specified disk category.|The error message returned because the specified DataDisk.N.Size parameter is invalid or the snapshot size exceeds the maximum capacity that is allowed for the specified disk category.|
|403|QuotaExceed.BuyImage|The specified image is from the image market? You have not bought it or your quota has been exceeded.|The error message returned because the specified image has not been purchased from Alibaba Cloud Marketplace or the maximum number of images has been reached.|
|404|DependencyViolation.IoOptimized|The specified instancetype must be IoOptimized instance.|The error message returned because the specified instance type is not I/O optimized.|
|404|PaymentMethodNotFound|No payment method has been registered on the account.|The error message returned because you have not configured a payment method for your account.|
|400|InvalidInstanceType.ValueNotSupported|InternetMaxBandwidthOut should be set.|The error message returned because the InternetMaxBandwidthOut parameter is not specified.|
|400|InvalidInstanceType.ValueNotSupported|instancetype is not valid.|The error message returned because the specified InstanceType parameter is invalid.|
|400|InvalidClientToken.ValueNotSupported|clienttoken is not valid.|The error message returned because the specified ClientToken parameter is invalid.|
|400|InvalidIoOptimize.ValueNotSupported|The specified IoOptimize is not valid.|The error message returned because the specified IoOptimized parameter is invalid.|
|400|InvalidHostName.Malformed|The specified parameter HostName is not valid.|The error message returned because the specified HostName parameter is invalid.|
|404|HOSTNAME\_ILLEGAL|hostname is not valid.|The error message returned because the specified HostName parameter is invalid.|
|400|InvalidInternetMaxBandwidthOut.Malformed|The specified parameter internetMaxBandwidthOut is not valid.|The error message returned because the specified InternetMaxBandwidthOut parameter is invalid.|
|400|InvalidInternetMaxBandwidthIn.Malformed|The specified parameter internetMaxBandwidthIn is not valid.|The error message returned because the specified InternetMaxBandwidthIn parameter is invalid.|
|400|InvalidSnapshotId.NotFound|The specified parameter SnapshotId is not exist.|The error message returned because the specified SnapshotId parameter does not exist. Check whether the snapshot ID is correct.|
|400|InvalidTagKey.Malformed|The specified Tag.n.Key is not valid.|The error message returned because the specified Tag.N.Key parameter is invalid.|
|400|InvalidTagValue.Malformed|The specified Tag.n.Value is not valid.|The error message returned because the specified Tag.N.Value parameter is invalid.|
|403|InvalidResourceId.NotSupported|The specified ResourceId does not support tagging.|The error message returned because the specified resource does not support tagging.|
|400|InvalidTag.Mismatch|The specified Tag.n.Key and Tag.n.Value are not match.|The error message returned because the specified Tag.N.Key and Tag.N.Value parameters do not match.|
|400|InvalidTagCount|The specified tags are beyond the permitted range.|The error message returned because the maximum number of tags has been reached.|
|400|InvalidMinAmount.Malformed|The specified parameter MinAmount is not valid.|The error message returned because the specified MinAmount parameter is invalid.|
|400|InvalidMaxAmount.Malformed|The specified parameter MaxAmount is not valid.|The error message returned because the specified MaxAmount parameter is invalid.|
|400|InvalidAutoReleaseTime.Malformed|The specified parameter AutoReleaseTime is not valid.|The error message returned because the specified AutoReleaseTime parameter is invalid. Specify the time in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format. The time must be in UTC.|
|400|InvalidPrivateIpAddress.Malformed|The specified parameter PrivateIpAddress is not valid.|The error message returned because the specified value of the PrivateIpAddress parameter is invalid.|
|400|InvalidInnerIpAddress.Malformed|The specified parameter InnerIpAddress is not valid.|The error message returned because the specified InnerIpAddress parameter is invalid.|
|404|InvalidSystemDiskSize.LessThanImageSize|The specified parameter SystemDisk.Size is less than the image size.|The error message returned because the specified system disk size is less than the size of the image.|
|404|InvalidSystemDiskSize.LessThanMinSize|The specified parameter SystemDisk.Size is less than the min size.|The error message returned because the specified system disk size is smaller than the allowed minimum size.|
|403|OperationDenied|The specified RegionId does not support the creation of the network type ECS instance.|The error message returned because instances of the specified network type cannot be created in the specified region. Check whether instance resources of this network type are available in the region.|
|400|OperationDenied.NoVlan|The specified parameter "VlanId" is not valid or vlan has not enough IP address.|The error message returned because the specified VLAN ID is invalid or no more IP addresses are available in this VLAN.|
|403|OperationDenied.ImageNotValid|The specified Image is disabled or is deleted.|The error message returned because the specified image does not exist.|
|403|OperationDenied.SnapshotNotValid|The specified snapshot is not allowed to create disk.|The error message returned because the snapshot is invalid and you cannot create disks by using the specified snapshot.|
|403|OperationDenied.SnapshotNotAllowed|The specified snapshot is not allowed to create disk.|The error message returned because the snapshot is invalid and you cannot create disks by using the specified snapshot.|
|403|OperationDenied.ZoneNotAllowed|The creation of Instance to the specified Zone is not allowed.|The error message returned because instances cannot be created in the specified zone.|
|403|OperationDenied.ZoneSystemCategoryNotMatch|The specified Zone or cluster does not offer the specified disk category or the speicified zone and cluster do not match.|The error message returned because the specified disk category is unavailable in the specified zone or cluster, or because the specified zone and cluster do not correspond to each other.|
|403|OperationDenied.ResourceControl|The specified region is in resource control, please try later.|The error message returned because the specified region is under resource control. Try again later.|
|403|OperationDenied.NoStock|The resource is out of usage.|The error message returned because the instance is not in the Running state. Start the instance or check whether the specified operation is valid.|
|403|OperationDenied.SnapshotParamsNotValid|The capacity of snapshot exceeds the size limit of the specified disk category or the specified category is not authorizied.|The error message returned because the maximum snapshot size has been reached or you are not authorized to use the specified disk category.|
|404|OperationDenied.CreatingConflict|Another Instance has been creating|The error message returned because no new instances can be created while another instance is being created.|
|403|OperationDenied.DiskTypeNotSupport|The type of the disk does not support the operation|The error message returned because the specified operation is invalid.|
|400|OperationDenied.QuotaExceed|The quota of tags on resource is beyond permitted range.|The error message returned because the maximum number of tags that can be bound to the resource has been reached.|
|400|Account.Arrearage|Your account has been in arrears.|The error message returned because your account balance is insufficient. Add funds to your account and try again.|
|403|InvalidUserData.Forbidden|User not authorized to input the parameter "UserData", please apply for permission "UserData"|The error message returned because you are not authorized to manage user data. Apply for the permission first.|
|400|InvalidUserData.SizeExceeded|The specified parameter "UserData" exceeds the size.|The error message returned because the size of user data specified by the UserData parameter exceeds the limit.|
|400|InvalidUserData.NotSupported|The specified parameter "UserData" only support the vpc and IoOptimized Instance.|The error message returned because the specified UserData parameter is not supported. UserData is supported only on VPC-type instances and I/O optimized instances.|
|404|InvalidZoneId.NotFound|The specified zoneId does not exist.|The error message returned because the specified ZoneId parameter does not exist.|
|403|Zone.NotOpen|The specified zone is not granted to you to buy resources yet.|The error message returned because you are not authorized to purchase resources in the specified zone.|
|403|Zone.NotOnSale|The resource in the specified zone is no longer available for sale. Please try other regions and zones.|The error message returned because the specified resource is unavailable in the specified zone. Try other resource types, or select other regions or zones.|
|403|InvalidClusterId.NotFound|The specified clusterId does not exist.|The error message returned because the specified ClusterId parameter does not exist.|
|403|OperationDenied.NoStock|The resource is out of stock in the specified zone. Please try other types, or choose other regions and zones.|The error message returned because the specified resource is unavailable in the specified zone. Try other resource types, or select other regions or zones.|
|403|InvalidInstanceType.ZoneNotSupported|The specified zone does not support this instancetype.|The error message returned because the specified instance type is not supported in the specified zone.|
|400|InstanceDiskNumber.LimitExceed|The total number of specified disk in an instance exceeds.|The error message returned because the maximum number of disks that can be attached to the instance has been reached.|
|400|InvalidSpotStrategy|The specified SpotStrategy is not valid.|The error message returned because the specified SpotStrategy parameter is invalid.|
|400|InvalidSpotPriceLimit|The specified SpotPriceLimitis not valid.|The error message returned because the specified SpotPriceLimit parameter is invalid.|
|400|InvalidSpotDuration|The specified SpotDuration is not valid.|The error message returned because the specified SpotDuration parameter is invalid.|
|400|InvalidSpotAuthorized|The specified Spot param is unauthorized.|The error message returned because you are not authorized to create preemptible instances based on the specified preemption policy.|
|400|InvalidSpotPrepaid|The specified Spot type is not support PrePay Instance.|The error message returned because the specified preemption policy does not support the subscription billing method.|
|400|InvalidSpotAliUid|The specified UID is not authorized to use SPOT instance.|The error message returned because the user with the specified user identifier \(UID\) does not have the permissions to use preemptible instances.|
|400|InvalidParameter.Bandwidth|%s|The error message returned because the specified bandwidth is invalid.|
|400|InvalidDataDiskCategory.ValueNotSupported|%s|The error message returned because the specified DiskCategory parameter is invalid.|
|400|InvalidSystemDiskCategory.ValueNotSupported|%s|The error message returned because the operation is not applicable to the specified system disk category.|
|400|InvalidParameter.Conflict|%s|The error message returned because a specified parameter is invalid. Check whether parameter conflicts exist.|
|400|InvalidInternetChargeType.ValueNotSupported|%s|The error message returned because the specified InternetChargeType parameter is not supported. Check whether the parameter is correct.|
|400|InvalidInstanceType.ValueNotSupported|%s|The error message returned because the operation is not supported by the specified instance type.|
|403|InstanceType.Offline|%s|The error message returned because the instance type is retired or available instances of this instance type are insufficient.|
|400|RegionUnauthorized|%s|The error message returned because you are not authorized to perform the operation in the specified region.|
|400|Zone.NotOnSale|%s|The error message returned because the sales of the specified resources in the specified zone are suspended.|
|400|InvalidSystemDiskSize.ValueNotSupported|%s|The error message returned because the specified system disk size is not supported.|
|400|InvalidDataDiskSize.ValueNotSupported|%s|The error message returned because the specified data disk capacity is invalid.|
|403|DependencyViolation.WindowsInstance|The instance creating is window, cannot use ssh key pair to login.|The error message returned because the specified instance is a Windows instance and does not support logons with SSH key pairs.|
|400|InvalidInstanceType.ValueNotSupported|The specified parameter "KeyPairName" only support IoOptimized Instance.|The error message returned because non-I/O optimized instances do not support SSH key pairs.|
|404|InvalidResourceGroup.NotFound|The ResourceGroup provided does not exist in our records.|The error message returned because the specified resource group does not exist.|
|403|InvalidUser.PassRoleForbidden|The RAM user does not have the privilege to pass a RAM role.|The error message returned because the RAM user is not authorized to pass a RAM role.|
|404|InvalidRamRole.NotFound|The specified parameter "RAMRoleName" does not exist.|The error message returned because the specified RAMRoleName parameter does not exist.|
|404|InvalidLaunchTemplate.NotFound|%s|The error message returned because the specified launch template does not exist. Check whether the specified LaunchTemplateId or LaunchTemplateName parameter is correct.|
|404|InvalidLaunchTemplateVersion.NotFound|%s|The error message returned because the specified LaunchTemplateVersion parameter does not exist. Check whether the launch template version is correct.|
|400|InvalidInstanceType.ElasticNetworkInterfaceNotSupported|The specified instance type does not support Elastic Network Interface, you can not attach Elastic Network Interface to generation I instances.|The error message returned because ENIs are not supported by the specified instance type.|
|400|InvalidParameter.EncryptedIllegal|%s|The error message returned because the specified parameter is invalid. Check whether the encryption is valid.|
|400|InvalidParameter.EncryptedNotSupported|%s|The error message returned because the specified parameter is invalid and your encryption operation is not supported.|
|500|InternalError|The request processing has failed due to some unknown error, exception or failure.|The error message returned because an internal error has occurred. Try again later. If the problem persists, submit a ticket.|
|403|Forbidden.RiskControl|This operation is forbidden by Aliyun RiskControl system.|The error message returned because the operation is forbidden by the risk control system.|
|403|InvalidInstance.UnPaidOrder|The specified Instance has unpaid order.|The error message returned because you have outstanding bills for the specified instance. Complete the payment and try again.|
|403|RealNameAuthenticationError|Your account has not passed the real-name authentication yet.|The error message returned because you have not completed the real-name verification. Complete the real-name verification and try again.|
|403|InvalidInstanceType.NotSupported|The specified InstanceType is not Supported.|The error message returned because the specified InstanceType parameter is invalid.|
|403|InvalidPayMethod|The specified pay method is not valid.|The error message returned because the specified payment method is invalid.|
|403|InvalidAccountStatus.NotEnoughBalance|Your account does not have enough balance.|The error message returned because your account balance is insufficient. Add funds to your account before you proceed.|
|400|InvalidSpotPriceLimit.LowerThanPublicPrice|The specified parameter "spotPriceLimit" can't be lower than current public price.|The error message returned because the specified SpotPriceLimit parameter is less than the current market price.|
|400|InvalidRelationResource.NotFound|The relation resource has been deleted.|The error message returned because the associated resource has been deleted and is invalid.|
|400|IncorrectRecycleBinStatus|The operation is not permitted due to resource status.|The error message returned because this operation is not supported while the resource is in the current state.|
|400|InvalidHpcClusterId.Unnecessary|The specified HpcClusterId is unnecessary|The error message returned because the HpcClusterId parameter is specified.|
|400|InvalidVSwitchId.Necessary|The VSwitchId is necessary|The error message returned because the VSwitchId parameter is not specified.|
|400|InvalidHpcClusterId.Necessary|The HpcClusterId is necessary|The error message returned because the HpcClusterId parameter is not specified.|
|400|InvalidHpcClusterId.NotFound|The specified HpcClusterId is not found|The error message returned because the specified HpcClusterId parameter does not exist.|
|404|InvalidVSwitchId.NotExist|%s|The error message returned because the specified VSwitchId parameter does not exist.|
|403|ImageNotSupportInstanceType|The specified image does not support the specified InstanceType.|The error message returned because the specified image cannot be used for instances of the specified instance type.|
|400|InvalidSecurityGroup.NotInDefaultVpc|%s|The error message returned because the specified security group is not in the default VPC. Check whether the specified SecurityGroupId parameter is correct.|
|400|VpcNotFound|Vpc is not found according to the specified VSwitch or the vpc does not belong to you.|The error message returned because the specified VSwitch does not belong to any VPC, or the corresponding VPC does not belong to your account.|
|400|InvalidSystemDiskSize.ImageNotSupportResize|The specified image does not support resize.|The error message returned because the specified image does not support resizing.|
|403|OperationDenied.InvalidNetworkType|%s|The error message returned because the operation is not supported by the specified network type.|
|400|InvalidSpotInterruptionBehavior|%s|The error message returned because the specified SpotInterruptionBehavior parameter is invalid.|
|403|InvalidSpotInterruptionBehavior.ClassicNetworkNotSupport|The specified SpotInterruptionBehavior does not support Classic network Instance.|The error message returned because the operation is not applicable to instances in the classic network.|
|403|InvalidSpotInterruptionBehavior.LocalDiskNotSupport|The specified SpotInterruptionBehavior does not support local disk instance.|The error message returned because the operation is not supported by instances that have local disks attached.|
|400|InvalidDeploymentOnHost|%s|The error message returned because the instance cannot be deployed in the specified deployment set.|
|400|InvalidInstanceChargeType.NotSupport|The Dedicated host not support the specified Instance charge type.|The error message returned because the dedicated host does not support instances that use the specified billing method.|
|400|InvalidNetworkType.NotSupported|The classic networkType not support create ECS on dedicatedHost|The error message returned because the dedicated host does not support instances in the classic network.|
|400|InvalidDedicatedHostId.NotFound|The specified DedicatedHostId does not exist.|The error message returned because the specified DedicatedHostId parameter does not exist.|
|400|InvalidDedicatedHostStatus.NotSupport|Operation denied due to dedicated host status.|The error message returned because the operation is not supported while the dedicated host is in the current state.|
|400|IncorrectDedicatedHostStatus|The current status of the resource does not support this operation.|The error message returned because this operation is not supported while the dedicated host is in the current state.|
|400|ChargeTypeViolation.PostPaidDedicatedHost|Prepaid instance onto postpaid dedicated host is not allowed|The error message returned because subscription instances cannot be deployed to pay-as-you-go dedicated hosts.|
|400|InvalidPeriod.ExceededDedicatedHost|Instance expired date can't exceed dedicated host expired date.|The error message returned because the expiration date of the instance is later than that of the dedicated host.|
|400|InvalidInstanceType.ValueUnauthorized|The specified InstanceType is not authorize.|The error message returned because you are not authorized to use the specified instance type.|
|400|DedicatedHostType.Unmatched|The specified DedicatedHostType doesn? t match the instance type.|The error message returned because the specified dedicated host type does not correspond to the specified instance type.|
|400|MissingParameter|The input parameter ImageId that is mandatory for processing this request is not supplied.|The error message returned because the ImageId parameter is not specified.|
|400|MissingParameter|The input parameter InstanceType that is mandatory for processing this request is not supplied.|The error message returned because the InstanceType parameter is not specified.|
|400|ChargeTypeViolation.PostPaidDedicatedHost|Prepaid instance onto postpaid dedicated host is not allowed.|The error message returned because subscription instances cannot be deployed to pay-as-you-go dedicated hosts.|
|400|InvalidParam.NetworkInterface|%s|The error message returned because a specified parameter is invalid. Check whether the parameter corresponds to the interface.|
|403|OperationDenied.ImageNotValid|%s|The error message returned because the image does not support the operation.|
|403|QuotaExceed.PostPaidDisk|Living postPaid disks quota exceeded.|The error message returned because the maximum number of pay-as-you-go disks has been reached.|
|401|InvalidRamRole.NotEcsRole|The specified ram role is not authorized for ecs, please check your role policy.|The error message returned because the specified RAM role is not authorized to use ECS. Check your role policy.|
|403|InvalidParameter.NotMatch|%s|The error message returned because a specified parameter is invalid. Check whether parameter conflicts exist.|
|400|InvalidParameter.CreditSpecification|The specified CreditSpecification is not supported in this region.|The error message returned because the specified credit policy is not supported in the region.|
|404|InvalidMarketImage.NotFound|The specified marketplace image does not exist, please change the imageId and try again.|The error message returned because the specified Alibaba Cloud Marketplace image does not exist. Modify the ImageId parameter and try again later.|
|400|IncorrectVpcStatus|Current VPC status does not support this operation.|The error message returned because the operation is not supported while the VPC is in the current state.|
|400|InvalidInstanceType.NotSupported|The specified instanceType is not supported by the deployment set.|The error message returned because the specified instance type is not supported by the current deployment set. Try another instance type.|
|400|InvalidVpcZone.NotSupported|The specified operation is not allowed in the zone to which your VPC belongs, please try in other zones.|The error message returned because the specified region does not support the specified operation. Try another region.|
|400|IncorrectDefaultVpcStatus|The status of the default VPC is invalid.|The error message returned because the status of the default VPC is invalid.|
|404|DeploymentSet.NotFound|The specified deployment set does not exist.|The error message returned because the specified deployment set does not exist.|
|400|InvalidAutoRenewPeriod.ValueNotSupported|The specified autoRenewPeriod is invalid.|The error message returned because the specified AutoRenewPeriod parameter is invalid.|
|403|OperationDenied.LocalDiskUnsupported|The configuration change is not allowed when the specified instance has local disks mounted.|The error message returned because the instance types of instances that have local disks attached cannot be changed.|
|403|OperationDenied.InconsistentNetwork|The specified security group and vswitch are not in the same vpc.|The error message returned because the specified security group and VSwitch do not belong to the same VPC.|
|403|OperationDenied|If the network segment of the vswitch is the same as that of its VPC. Therefore, the VPC cannot create other vswitchs across the region.|The error message returned because the VPC and VSwitch have the same CIDR block and no more VSwitches can be created for other zones of this VPC.|
|403|DefaultVswitch.Existed|The default vswitch for VPC already exists.|The error message returned because a default VSwitch already exists in the current VPC.|
|400|InvalidMarketImageChargeType.NotSupport|The specified chargeType of marketImage is unsupported.|The error message returned because the billing method of the Alibaba Cloud Marketplace image is not supported.|
|403|OperationDenied.NoStock|The requested resource is sold out in the specified zone; try other types of resources or other regions and zones.|The error message returned because the requested resources are insufficient in the specified zone. Try other types of resources or other regions and zones.|
|403|IncorrectInstanceStatus|The current status of the resource does not support this operation.|The error message returned because the operation is not supported while the instance is in the current state.|
|403|CategoryViolation|The specified instance does not support this operation because of its disk category.|The error message returned because the configurations of instances that have local disks attached cannot be upgraded or downgraded.|
|400|InvalidPeriodType.ValueNotSupported|The specified parameter PeriodType is invalid.|The error message returned because the specified PeriodType parameter is invalid.|
|403|ResourcesNotInSameZone|The specified instance and dedicated host are not in the same zone.|The error message returned because the specified instance and the dedicated host are not in the same region.|
|403|InvalidDisk.SystemDiskSize|The specified SystemDiskSize beyond the permitted range.|The error message returned because the specified system disk size exceeds the upper limit.|
|400|InvalidClientToken.ValueNotSupported|The ClientToken provided is invalid.|The error message returned because the specified ClientToken parameter is invalid.|
|400|OperationDenied|The current user does not support this operation.|The error message returned because your account does not support this operation.|
|403|InsufficientBalance|Your account does not have enough balance.|The error message returned because your account balance is insufficient. Add funds to your account and try again.|
|400|InvalidInstanceType.ValueNotSupported|The specified InstanceType does not exist or beyond the permitted range.|The error message returned because the specified instance type does not exist or you are not authorized to manage instances of this instance type.|
|400|InvalidParam.Tenancy|The specified Tenancy is invalid.|The error message returned because the specified Tenancy parameter is invalid.|
|403|MaxEniIpv6IpsCountExceeded|%s|The error message returned because the maximum number of IPv6 addresses that can be assigned to the ENI has been reached.|
|403|InvalidIp.IpRepeated|%s|The error message returned because the specified IP address already exists.|
|403|InvalidIp.IpAssigned|%s|The error message returned because the specified IP address is already assigned.|
|403|InvalidIp.Address|%s|The error message returned because the specified IPv6 address is invalid.|
|403|InvalidOperation.Ipv4CountExceeded|%s|The error message returned because the maximum number of IPv4 addresses has been reached.|
|403|InvalidOperation.Ipv6CountExceeded|%s|The error message returned because the maximum number of IPv6 addresses has been reached.|
|403|InvalidOperation.Ipv6NotSupport|%s|The error message returned because IPv6 does not support the current operation.|
|403|InvalidVSwitch.Ipv6NotTurnOn|%s|The error message returned because IPv6 is not enabled on your VSwitch. Enable this feature and try again.|
|403|InvalidParam.Amount|%s|The error message returned because the specified Amount parameter is invalid.|
|403|InvalidVSwitchId.IpInvalid|%s|The error message returned because the specified private IP address is invalid.|
|403|Forbidden.RegionId|%s|The error message returned because the service is unavailable in the current region.|
|400|IncorrectVpcStatus|The current status of vpc does not support this operation.|The error message returned because the operation is not supported while the VPC is in the current state.|
|403|QuotaExceed.DeploymentSetInstanceQuotaFull|instance quota in one deployment set exceeded.|The error message returned because the maximum number of instances in the current deployment set has been reached.|
|403|InvalidRegion.NotSupport|The specified region does not support byok.|The error message returned because the bring your own key \(BYOK\) feature is not supported in this region.|
|403|UserNotInTheWhiteList|The user is not in byok white list.|The error message returned because you are not in the BYOK whitelist. Try again when you are in the whitelist.|
|400|InvalidParameter.EncryptedIllegal|The specified parameter Encrypted must be true when kmsKeyId is not empty.|The error message returned because the encryption feature is not enabled after the KMSKeyId parameter is specified.|
|400|IoOptimized.NotSupported|The specified instance must be IoOptimized instance when kmsKeyId is not empty.|The error message returned because I/O optimized instances must be used after the KMSKeyId parameter is specified.|
|404|InvalidParameter.KMSKeyId.NotFound|The specified KMSKeyId does not exist.|The error message returned because the specified KMSKeyId parameter does not exist.|
|403|InvalidParameter.KMSKeyId.KMSUnauthorized|ECS service have no right to access your KMS.|The error message returned because ECS is not authorized to access your KMS resources.|
|403|SecurityRisk.3DVerification|We have detected a security risk with your default credit or debit card. Please proceed with verification via the link in your email.|The error message returned because risks are detected in your default credit card or debit card. Use the link in the email for verification.|
|400|Duplicate.TagKey|The Tag.N.Key contain duplicate key.|The error message returned because the specified tag key already exists. Tag keys must be unique.|
|404|InvalidSecurityGroupId.NotFound|%s|The error message returned because the specified SecurityGroupId parameter does not exist.|
|404|InvalidDiskIds.NotPortable|The specified DiskId is not portable.|The error message returned because the specified disk is not removable.|
|403|QuotaExceed.Tags|%s|The error message returned because the maximum number of tags has been reached.|
|400|InvalidHttpEndpoint.NotSupported|The specified HttpEndpoint not supported, you can use enabled\(default\) or disabled.|The error message returned because the specified HttpEndpoint parameter is invalid. Set the value to enabled or disabled. The default value is enabled.|
|400|InvalidHttpTokens.NotSupported|The specified HttpTokens not supported, you can use optional\(default\) or required.|The error message returned because the specified HttpTokens parameter is invalid. Set the value to optional or required. The default value is optional.|
|400|InvalidHttpPutResponseHopLimit.NotSupported|The specified HttpPutResponseHopLimit not supported, more than 1 and less than 64 is reasonable.|The error message returned because the specified HttpPutResponseHopLimit parameter is invalid. The value must be in the range of 1 to 64.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Ecs).

