# Regions and Availability Zones<a name="using-regions-availability-zones"></a>

Amazon EC2 is hosted in multiple locations world\-wide\. These locations are composed of Regions and Availability Zones\. Each *Region* is a separate geographic area\. Each Region has multiple, isolated locations known as *Availability Zones*\. Amazon EC2 provides you the ability to place resources, such as instances, and data in multiple locations\. Resources aren't replicated across Regions unless you do so specifically\.

Amazon operates state\-of\-the\-art, highly\-available data centers\. Although rare, failures can occur that affect the availability of instances that are in the same location\. If you host all your instances in a single location that is affected by such a failure, none of your instances would be available\.

**Topics**
+ [Region and Availability Zone Concepts](#concepts-regions-availability-zones)
+ [Available Regions](#concepts-available-regions)
+ [Regions and Endpoints](#using-regions-endpoints)
+ [Describing Your Regions and Availability Zones](#using-regions-availability-zones-describe)
+ [Specifying the Region for a Resource](#using-regions-availability-zones-setup)
+ [Launching Instances in an Availability Zone](#using-regions-availability-zones-launching)
+ [Migrating an Instance to Another Availability Zone](#migrating-instance-availability-zone)

## Region and Availability Zone Concepts<a name="concepts-regions-availability-zones"></a>

Each Region is completely independent\. Each Availability Zone is isolated, but the Availability Zones in a Region are connected through low\-latency links\. The following diagram illustrates the relationship between Regions and Availability Zones\.

![\[Regions and Availability Zones\]](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/aws_regions.png)

Amazon EC2 resources are either global, tied to a Region, or tied to an Availability Zone\. For more information, see [Resource Locations](resources.md)\.

### Regions<a name="concepts-regions"></a>

Each Amazon EC2 Region is designed to be completely isolated from the other Amazon EC2 Regions\. This achieves the greatest possible fault tolerance and stability\.

When you view your resources, you'll only see the resources tied to the Region you've specified\. This is because Regions are isolated from each other, and we don't replicate resources across Regions automatically\.

When you launch an instance, you must select an AMI that's in the same Region\. If the AMI is in another Region, you can copy the AMI to the Region you're using\. For more information, see [Copying an AMI](CopyingAMIs.md)\.

Note that there is a charge for data transfer between Regions\. For more information, see [Amazon EC2 Pricing \- Data Transfer](https://aws.amazon.com/ec2/pricing/on-demand/#Data_Transfer)\.

### Availability Zones<a name="concepts-availability-zones"></a>

When you launch an instance, you can select an Availability Zone or let us choose one for you\. If you distribute your instances across multiple Availability Zones and one instance fails, you can design your application so that an instance in another Availability Zone can handle requests\.

You can also use Elastic IP addresses to mask the failure of an instance in one Availability Zone by rapidly remapping the address to an instance in another Availability Zone\. For more information, see [Elastic IP Addresses](elastic-ip-addresses-eip.md)\. 

An Availability Zone is represented by a Region code followed by a letter identifier; for example, `us-east-1a`\. To ensure that resources are distributed across the Availability Zones for a Region, we independently map Availability Zones to names for each AWS account\. For example, the Availability Zone `us-east-1a` for your AWS account might not be the same location as `us-east-1a` for another AWS account\.

To coordinate Availability Zones across accounts, you must use the *AZ ID*, which is a unique and consistent identifier for an Availability Zone\. For example, `use1-az1` is an AZ ID for the `us-east-1` Region and it has the same location in every AWS account\.

Viewing AZ IDs enables you to determine the location of resources in one account relative to the resources in another account\. For example, if you share a subnet in the Availability Zone with the AZ ID `use-az2` with another account, this subnet is available to that account in the Availability Zone whose AZ ID is also `use-az2`\. The AZ ID for each VPC and subnet is displayed in the Amazon VPC console\. For more information, see [Working with VPC Sharing](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-sharing.html) in the *Amazon VPC User Guide*\.

As Availability Zones grow over time, our ability to expand them can become constrained\. If this happens, we might restrict you from launching an instance in a constrained Availability Zone unless you already have an instance in that Availability Zone\. Eventually, we might also remove the constrained Availability Zone from the list of Availability Zones for new accounts\. Therefore, your account might have a different number of available Availability Zones in a Region than another account\. 

You can list the Availability Zones that are available to your account\. For more information, see [Describing Your Regions and Availability Zones](#using-regions-availability-zones-describe)\.

## Available Regions<a name="concepts-available-regions"></a>

Your account determines the Regions that are available to you\. For example:
+ An AWS account provides multiple Regions so that you can launch Amazon EC2 instances in locations that meet your requirements\. For example, you might want to launch instances in Europe to be closer to your European customers or to meet legal requirements\.
+ An AWS GovCloud \(US\-West\) account provides access to the AWS GovCloud \(US\-West\) Region only\. For more information, see [AWS GovCloud \(US\-West\) Region](https://aws.amazon.com/govcloud-us/)\.
+ An Amazon AWS \(China\) account provides access to the Beijing and Ningxia Regions only\. For more information, see [AWS in China](https://www.amazonaws.cn/about-aws/china/)\.

The following table lists the Regions provided by an AWS account\. You can't describe or access additional Regions from an AWS account, such as AWS GovCloud \(US\-West\) or the China Regions\. To use a Region introduced after March 20, 2019, you must enable the Region\. For more information, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\.


| Code | Name | Opt\-in Status | 
| --- | --- | --- | 
| `us-east-1` | US East \(N\. Virginia\) | Not required | 
| `us-east-2` | US East \(Ohio\) | Not required | 
| `us-west-1` | US West \(N\. California\) | Not required | 
| `us-west-2` | US West \(Oregon\) | Not required | 
| `ca-central-1` | Canada \(Central\) | Not required | 
| `eu-central-1` | EU \(Frankfurt\) | Not required | 
| `eu-west-1` | EU \(Ireland\) | Not required | 
| `eu-west-2` | EU \(London\) | Not required | 
| `eu-west-3` | EU \(Paris\) | Not required | 
| `eu-north-1` | EU \(Stockholm\) | Not required | 
| `ap-east-1` | Asia Pacific \(Hong Kong\) | Required | 
| `ap-northeast-1` | Asia Pacific \(Tokyo\) | Not required | 
| `ap-northeast-2` | Asia Pacific \(Seoul\) | Not required | 
| `ap-northeast-3` | Asia Pacific \(Osaka\-Local\) | Not required | 
| `ap-southeast-1` | Asia Pacific \(Singapore\) | Not required | 
| `ap-southeast-2` | Asia Pacific \(Sydney\) | Not required | 
| `ap-south-1` | Asia Pacific \(Mumbai\) | Not required | 
| `me-south-1` | Middle East \(Bahrain\) | Required | 
| `sa-east-1` | South America \(São Paulo\) | Not required | 

For more information, see [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

The number and mapping of Availability Zones per Region may vary between AWS accounts\. To get a list of the Availability Zones that are available to your account, you can use the Amazon EC2 console or the command line interface\. For more information, see [Describing Your Regions and Availability Zones](#using-regions-availability-zones-describe)\.

## Regions and Endpoints<a name="using-regions-endpoints"></a>

When you work with an instance using the command line interface or API actions, you must specify its regional endpoint\. For more information about the Regions and endpoints for Amazon EC2, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region) in the *Amazon Web Services General Reference*\.

For more information about endpoints and protocols in AWS GovCloud \(US\-West\), see [AWS GovCloud \(US\-West\) Endpoints](https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/using-govcloud-endpoints.html) in the *AWS GovCloud \(US\) User Guide*\.

## Describing Your Regions and Availability Zones<a name="using-regions-availability-zones-describe"></a>

You can use the Amazon EC2 console or the command line interface to determine which Regions and Availability Zones are available for your account\. For more information about these command line interfaces, see [Accessing Amazon EC2](concepts.md#access-ec2)\.

**To find your Regions and Availability Zones using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, view the options in the Region selector\.  
![\[View your Regions\]](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/EC2_select_region.png)

1. On the navigation pane, choose **EC2 Dashboard**\.

1. The Availability Zones are listed under **Service Health**, **Availability Zone Status**\.

**To find your Regions and Availability Zones using the AWS CLI**

1. Use the [describe\-regions](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-regions.html) command as follows to describe the Regions that are enabled for your account\.

   ```
   aws ec2 describe-regions
   ```

   To describe all Regions, including any Regions that are disabled for your account, add the `--all-regions` option as follows\.

   ```
   aws ec2 describe-regions --all-regions
   ```

1. Use the [describe\-availability\-zones](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-availability-zones.html) command as follows to describe the Availability Zones within the specified Region\.

   ```
   aws ec2 describe-availability-zones --region region-name
   ```

**To find your Regions and Availability Zones using the AWS Tools for Windows PowerShell**

1. Use the [Get\-EC2Region](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2Region.html) command as follows to describe the Regions for your account\.

   ```
   PS C:\> Get-EC2Region
   ```

1. Use the [Get\-EC2AvailabilityZone](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2AvailabilityZone.html) command as follows to describe the Availability Zones within the specified Region\.

   ```
   PS C:\> Get-EC2AvailabilityZone -Region region-name
   ```

## Specifying the Region for a Resource<a name="using-regions-availability-zones-setup"></a>

Every time you create an Amazon EC2 resource, you can specify the Region for the resource\. You can specify the Region for a resource using the AWS Management Console or the command line\.

**Note**  
Some AWS resources might not be available in all Regions and Availability Zones\. Ensure that you can create the resources you need in the desired Regions or Availability Zone before launching an instance in a specific Availability Zone\.

**To specify the Region for a resource using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Use the Region selector in the navigation bar\.  
![\[Use the console Region selector\]](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/EC2_select_region.png)

**To specify the default Region using the command line**

You can set the value of an environment variable to the desired regional endpoint \(for example, `https://ec2.us-east-2.amazonaws.com`\):
+ `AWS_DEFAULT_REGION` \(AWS CLI\)
+ `Set-AWSDefaultRegion` \(AWS Tools for Windows PowerShell\)

Alternatively, you can use the `--region` \(AWS CLI\) or `-Region` \(AWS Tools for Windows PowerShell\) command line option with each individual command\. For example, `--region us-east-2`\.

For more information about the endpoints for Amazon EC2, see [Amazon Elastic Compute Cloud Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region)\.

## Launching Instances in an Availability Zone<a name="using-regions-availability-zones-launching"></a>

When you launch an instance, select a Region that puts your instances closer to specific customers, or meets the legal or other requirements you have\. By launching your instances in separate Availability Zones, you can protect your applications from the failure of a single location\. 

When you launch an instance, you can optionally specify an Availability Zone in the Region that you are using\. If you do not specify an Availability Zone, we select one for you\. When you launch your initial instances, we recommend that you accept the default Availability Zone, because this enables us to select the best Availability Zone for you based on system health and available capacity\. If you launch additional instances, only specify an Availability Zone if your new instances must be close to, or separated from, your running instances\.

## Migrating an Instance to Another Availability Zone<a name="migrating-instance-availability-zone"></a>

If you need to, you can migrate an instance from one Availability Zone to another\. For example, if you are trying to modify the instance type of your instance and we can't launch an instance of the new instance type in the current Availability Zone, you could migrate the instance to an Availability Zone where we can launch an instance of that instance type\.

The migration process involves creating an AMI from the original instance, launching an instance in the new Availability Zone, and updating the configuration of the new instance, as shown in the following procedure\.

**To migrate an instance to another Availability Zone**

1. Create an AMI from the instance\. The procedure depends on the operating system and the type of root device volume for the instance\. For more information, see the documentation that corresponds to your operating system and root device volume:
   + [Creating an Amazon EBS\-Backed Linux AMI](creating-an-ami-ebs.md)
   + [Creating an Instance Store\-Backed Linux AMI](creating-an-ami-instance-store.md)
   + [Creating an Amazon EBS\-Backed Windows AMI](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Creating_EBSbacked_WinAMI.html)

1. If you need to preserve the private IPv4 address of the instance, you must delete the subnet in the current Availability Zone and then create a subnet in the new Availability Zone with the same IPv4 address range as the original subnet\. Note that you must terminate all instances in a subnet before you can delete it\. Therefore, you should create AMIs from all the instances in your subnet so that you can move all instances in the current subnet to the new subnet\.

1. Launch an instance from the AMI that you just created, specifying the new Availability Zone or subnet\. You can use the same instance type as the original instance, or select a new instance type\. For more information, see [Launching Instances in an Availability Zone](#using-regions-availability-zones-launching)\.

1. If the original instance has an associated Elastic IP address, associate it with the new instance\. For more information, see [Disassociating an Elastic IP Address and Reassociating with a Different Instance](elastic-ip-addresses-eip.md#using-instance-addressing-eips-associating-different)\.

1. If the original instance is a Reserved Instance, change the Availability Zone for your reservation\. \(If you also changed the instance type, you can also change the instance type for your reservation\.\) For more information, see [Submitting Modification Requests](ri-modifying.md#ri-modification-process)\.

1. \(Optional\) Terminate the original instance\. For more information, see [Terminating an Instance](terminating-instances.md#terminating-instances-console)\.