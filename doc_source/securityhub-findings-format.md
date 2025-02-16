# AWS Security Finding Format<a name="securityhub-findings-format"></a>

AWS Security Hub consumes, aggregates, organizes, and prioritizes findings from AWS security services and from the third\-party product integrations\. Security Hub processes these findings using a standard findings format called the AWS Security Finding Format, thus eliminating the need for time\-consuming data conversion efforts\. Then it correlates ingested findings across products to prioritize the most important ones\.

**Topics**
+ [Syntax of the AWS Security Finding Format](#securityhub-findings-format-syntax)
+ [Attributes of the AWS Security Finding Format](#securityhub-findings-format-attributes)
+ [Types Taxonomy of the AWS Security Finding](#securityhub-findings-format-type-taxonomy)

## Syntax of the AWS Security Finding Format<a name="securityhub-findings-format-syntax"></a>

The following is the syntax of the complete finding JSON in the AWS Security Finding Format\.

```
"Findings": [ 
     {
        "AwsAccountId": "string",
		"Compliance": { 
		   "Status": "string"
		},
		"Confidence": number,
		"CreatedAt": "string",
		"Criticality": number,
		"Description": "string",
		"FirstObservedAt": "string",
		"GeneratorId": "string",
		"Id": "string",
		"LastObservedAt": "string",
		"Malware": [ 
		   { 
		      "Name": "string",
		      "Path": "string",
		      "State": "string",
		      "Type": "string"
		   }
		],
		"Network": { 
		   "DestinationDomain": "string",
		   "DestinationIpV4": "string",
		   "DestinationIpV6": "string",
		   "DestinationPort": number,
		   "Direction": "string",
		   "Protocol": "string",
		   "SourceDomain": "string",
		   "SourceIpV4": "string",
		   "SourceIpV6": "string",
		   "SourceMac": "string",
		   "SourcePort": number
		    },
		"Note": { 
		   "Text": "string",
		   "UpdatedAt": "string",
		   "UpdatedBy": "string"
		},
		"Process": { 
		   "LaunchedAt": "string",
		   "Name": "string",
		   "ParentPid": number,
		   "Path": "string",
		   "Pid": number,
		   "TerminatedAt": "string"
		},
		"ProductArn": "string",
		"ProductFields": { 
		   "string" : "string" 
		},
		"RecordState": "string",
		"RelatedFindings": [ 
		   { 
		      "Id": "string",
		      "ProductArn": "string"
		   }
		],
		"Remediation": { 
		   "Recommendation": { 
		      "Text": "string",
		      "Url": "string"
		   }
		},
		"Resources": [ 
		   { 
		      "Details": { 
		         "AwsEc2Instance": { 
		            "IamInstanceProfileArn": "string",
		            "ImageId": "string",
		            "IpV4Addresses": [ "string" ],
		            "IpV6Addresses": [ "string" ],
		            "KeyName": "string",
		            "LaunchedAt": "string",
		            "SubnetId": "string",
		            "Type": "string",
		            "VpcId": "string"
		         },
		         "AwsIamAccessKey": { 
		            "CreatedAt": "string",
		            "Status": "string",
		            "UserName": "string"
		         },
		         "AwsS3Bucket": { 
		            "OwnerId": "string",
		            "OwnerName": "string"
		         },
		         "Container": { 
		            "ImageId": "string",
		            "ImageName": "string",
		            "LaunchedAt": "string",
		            "Name": "string"
		         },
		         "Other": { 
		            "string" : "string" 
		         }
		      },
		      "Id": "string",
		      "Partition": "string",
		      "Region": "string",
		      "Tags": { 
		         "string" : "string" 
		      },
		      "Type": "string"
		   }
		],
		"SchemaVersion": "string",
		"Severity": { 
		   "Normalized": number,
		   "Product": number
		},
		"SourceUrl": "string",
		"ThreatIntelIndicators": [ 
		   { 
		      "Category": "string",
		      "LastObservedAt": "string",
		      "Source": "string",
		      "SourceUrl": "string",
		      "Type": "string",
		      "Value": "string"
		   }
		],
		"Title": "string",
		"Types": [ "string" ],
		"UpdatedAt": "string",
		"UserDefinedFields": { 
		   "string" : "string" 
		},
		"VerificationState": "string",
		"WorkflowState": "string"
      }
    ]
```

## Attributes of the AWS Security Finding Format<a name="securityhub-findings-format-attributes"></a>

The following table provides descriptions and examples for the AWS Security Finding Format attributes\.


| **Attribute** | **Required** | **Description** | 
| --- | --- | --- | 
| Partition | Description | 
| --- | --- | 
| Severity Label | Severity Score Range | 
| --- | --- | 
| AwsAccountId | Yes | The AWS account ID where a finding is generated\. Type: string \(12 digits max\) Example: <pre>"AwsAccountId": "111111111111"</pre>  | 
| Compliance | No | Exclusive to findings that are generated as the result of a check run against a specific rule in a supported standard \(for example, CIS AWS Foundations\)\. Contains compliance\-related finding details\. Type: object Example: <pre>"Compliance": {<br />    "Status": "PASSED"<br />	}</pre>  | 
| Compliance\.Status | No | The result of a compliance check\. Type: enum [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) Example: <pre>"Status": "PASSED"</pre>  | 
| Confidence | No |  A finding's confidence\. Confidence is defined as the likelihood that a finding accurately identifies the behavior or issue that it was intended to identify\. Confidence is scored on a 0–100 basis using a ratio scale, where 0 means zero\-percent confidence and 100 means 100\-percent confidence\. However, a data exfiltration detection based on a statistical deviation of network traffic has a much lower confidence because an actual exfiltration hasn't been verified\. Type: integer \(range 0–100\) Example: <pre>"Confidence": 42</pre>  | 
| CreatedAt | Yes |  An ISO8601\-formatted timestamp \(as defined in [RFC\-3339 Date and Time on the Internet: Timestamps](https://tools.ietf.org/html/rfc3339)\) that indicates when the potential security issue captured by a finding was created\. Because the `CreatedAt` timestamp reflects the time when the finding record was created, it can differ from the `FirstObservedAt` timestamp, which reflects the time when the event or vulnerability was first observed\. This timestamp *must* be provided on the first generation of the finding and *can't* be changed upon subsequent updates to the finding\. Type: timestamp Example: <pre>"CreatedAt": "2017-03-22T13:22:13.933Z"</pre>  Findings are deleted 90 days after the most recent update or 90 days after the creation date if no update occurs\. To store findings for longer than 90 days, you can configure a rule in CloudWatch Events that routes findings to your Amazon S3 bucket\.  | 
| Criticality | No | The level of importance that is assigned to the resources associated with the finding\. A score of 0 means that the underlying resources have no criticality, and a score of 100 is reserved for the most critical resources\.  Type: integer \(range 0–100\) Criticality is scored on a 0–100 basis, using a ratio scale that supports only full integers\. This means that you should assess not only which findings impact resources that are more critical than others but also how much more critical those resources are compared to other resources\. A score of 0 means that the underlying resources have no criticality, and a score of 100 is reserved for the most critical resources\. When assessing criticality of a finding, consider the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) You can use the following guidelines: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) Example: <pre>"Criticality": 99</pre>  | 
| Description | Yes | A finding's description\. This field can be nonspecific boilerplate text or details that are specific to the instance of the finding\. Type: string \(1,024 characters max\) Example: <pre>"Description": "The version of openssl found on instance i-abcd1234 is known to contain a vulnerability."</pre>  | 
| FirstObservedAt | No | An ISO8601\-formatted timestamp \(as defined in [RFC\-3339 Date and Time on the Internet: Timestamps](https://tools.ietf.org/html/rfc3339)\) that indicates when the potential security issue captured by a finding was first observed\. Type: timestamp Because this timestamp reflects the time of when the event or vulnerability was first observed, it can differ from the `CreatedAt` timestamp, which reflects the time this finding record was created\.  This timestamp should be immutable between updates of the finding record, but can be updated if a more accurate timestamp has been determined\. Example: <pre>"FirstObservedAt": "2017-03-22T13:22:13.933Z"</pre>  | 
| GeneratorId | Yes | The identifier for the solution\-specific component \(a discrete unit of logic\) that generated a finding\. In various solutions from security findings products, this generator can be called a rule, a check, a detector, a plug\-in, and so on\. Type: string \(512 characters max\) or Amazon Resource Name \(ARN\) Example: <pre>"GeneratorId": "acme-vuln-9ab348"</pre>  | 
| Id | Yes | The product\-specific identifier for a finding\. Type: string \(512 characters max\) or ARN The finding ID must comply with the following constraints: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) These constraints are expected to hold within a findings product, but aren't required to hold across findings products\. Example: <pre>"Id": "us-west-2/111111111111/98aebb2207407c87f51e89943f12b1ef"</pre>  | 
| LastObservedAt | No | An ISO8601\-formatted timestamp \(as defined in [RFC\-3339 Date and Time on the Internet: Timestamps](https://tools.ietf.org/html/rfc3339)\) that indicates when the potential security issue captured by a finding was most recently observed by the security findings product\. Type: timestamp Because this timestamp reflects the time of when the event or vulnerability was last or most recently observed, it can differ from the `UpdatedAt` timestamp, which reflects the time this finding record was last or most recently updated\.  You can provide this timestamp, but it isn't required upon the first observation\. If you provide the field in this case, the timestamp should be the same as the `FirstObservedAt` timestamp\. You should update this field to reflect the last or most recently observed timestamp each time a finding is observed\. Example: <pre>"LastObservedAt": "2017-03-23T13:22:13.933Z"</pre>  | 
| Malware | No | A list of malware related to a finding\. Type: array of up to five malware objects Example: <pre>"Malware": [<br />    {<br />        "Name": "Stringler",<br />  	  "Type": "COIN_MINER",<br />  	  "Path": "/usr/sbin/stringler",<br />  	  "State": "OBSERVED"<br />    }</pre>  | 
| Malware\.Name | Yes | The name of the malware that was observed\. Type: string \(64 characters max\) Example: <pre>"Name": "Stringler"</pre>  | 
| Malware\.Path | No | The filesystem path of the malware that was observed\.  Type: string \(512 characters max\) Example: <pre>"Path": "/usr/sbin/stringler"</pre>  | 
| Malware\.State | No | The state of the malware that was observed\. Valid values are OBSERVED \| REMOVAL\_FAILED \| REMOVED\. Type: enum Example: <pre>"State": "OBSERVED"</pre>  | 
| Malware\.Type | No | The type of the malware that was observed\. Valid values are ADWARE \| BLENDED\_THREAT \| BOTNET\_AGENT \| COIN\_MINER \| EXPLOIT\_KIT \| KEYLOGGER \| MACRO \| POTENTIALLY\_UNWANTED \| SPYWARE \| RANSOMWARE \| REMOTE\_ACCESS \| ROOTKIT \| TROJAN \| VIRUS \| WORM\. Type: enum Example: <pre>"Type": "COIN_MINER"</pre>  | 
| Network | No | The details of network\-related information about a finding\. Type: object Example: <pre>"Network": {<br />    "Direction": "IN",<br />	"Protocol": "TCP",<br />	"SourceIpV4": "1.2.3.4",<br />	"SourceIpV6": "FE80:CD00:0000:0CDE:1257:0000:211E:729C",<br />	"SourcePort": "42",<br />	"SourceDomain": "here.com",<br />	"SourceMac": "00:0d:83:b1:c0:8e",<br />	"DestinationIpV4": "2.3.4.5",<br />	"DestinationIpV6": "FE80:CD00:0000:0CDE:1257:0000:211E:729C",<br />	"DestinationPort": "80",<br />	"DestinationDomain": "there.com"<br />    }</pre>  | 
| Network\.DestinationDomain | No | The destination domain of network\-related information about a finding\.Type: string \(128 characters max\) Example: <pre>"DestinationDomain": "there.com"</pre>  | 
| Network\.DestinationIpV4 | No | The destination IPv4 address of network\-related information about a finding\.Type: IPv4 Example: <pre>"DestinationIpV4": "2.3.4.5"</pre>  | 
| Network\.DestinationIpV6 | No | The destination IPv6 address of network\-related information about a finding\. Type: IPv6 Example: <pre>"DestinationIpV6": "FE80:CD00:0000:0CDE:1257:0000:211E:729C"</pre>  | 
| Network\.DestinationPort | No | The destination port of network\-related information about a finding\.Type: number \(range of 0–65535\) Example: <pre>"DestinationPort": "80"</pre>  | 
| Network\.Direction | No | The direction of network traffic associated with a finding\. Valid values are IN \| OUT\.  Type: enum Example: <pre>"Direction": "IN"</pre>  | 
| Network\.Protocol | No | The protocol of network\-related information about a finding\. Type: string \(16 characters max\) The name should be the [IANA registered name](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) for the associated port except in the case where the finding product can determine a more accurate protocol\. Example: <pre>"Protocol": "TCP"</pre>  | 
| Network\.SourceDomain | No | The source domain of network\-related information about a finding\.  Type: string \(128 characters max\) Example: <pre>"SourceDomain": "here.com"</pre>  | 
| Network\.SourceIpV4 | No | The source IPv4 address of network\-related information about a finding\. Type: IPv4 Example: <pre>"SourceIpV4": "1.2.3.4"</pre>  | 
| Network\.SourceIpV6 | No | The source IPv6 address of network\-related information about a finding\.Type: IPv6 Example: <pre>"SourceIpV6": "FE80:CD00:0000:0CDE:1257:0000:211E:729C"</pre>  | 
| Network\.SourceMac | No | The source media access control \(MAC\) address of network\-related information about a finding\.Type: string \(must match `MM:MM:MM:SS:SS:SS`\) Example: <pre>"SourceMac": "00:0d:83:b1:c0:8e"</pre>  | 
| Network\.SourcePort | No | The source port of network\-related information about a finding\. Type: number \(range of 0–65535\) Example: <pre>"SourcePort": "80"</pre>  | 
| Note | No | A user\-defined note that is added to a finding\.Type: object Example: <pre>"Note": {<br />        "Text": "Don't forget to check under the mat.",<br />        "UpdatedBy": "jsmith",<br />        "UpdatedAt": "2018-08-31T00:15:09Z"<br />    }</pre>  | 
| Note\.Text | Yes | The text of a finding note\. Type: string \(512 characters max\)  Example: <pre>"Text": "Example text."</pre>  | 
| Note\.UpdatedAt | Yes | The timestamp of when the note was updated\. Type: timestamp Example: <pre>"UpdatedAt": "2018-08-31T00:15:09Z"</pre>  | 
| Note\.UpdatedBy | Yes | The principal that created a note\. Type: string \(512 characters max\) or ARN Example: <pre>"UpdatedBy": "jsmith"</pre>  | 
| Process | No | The details of process\-related information about a finding\.Type: object Example: <pre>"Process": {<br />  "Name": "syslogd",<br />  "Path": "/usr/sbin/syslogd",<br />  "Pid": 12345,<br />  "ParentPid": 56789,<br />  "LaunchedAt": "2018-09-27T22:37:31Z",<br />  "TerminatedAt": "2018-09-27T23:37:31Z"<br />}</pre>  | 
| Process\.LaunchedAt | No | The timestamp for the date and time when the process was launched\. Type: timestamp Example: <pre>"LaunchedAt": "2018-09-27T22:37:31Z"</pre>  | 
| Process\.Name | No | The name of the process\.Type: string \(64 characters max\) Example: <pre>"Name": "syslogd"</pre>  | 
| Process\.ParentPid | No | The parent process ID\.Type: number Example: <pre>"ParentPid": 56789</pre>  | 
| Process\.Path | No | The path to the process executable\. Type: string \(512 characters max\) Example: <pre>"Path": "/usr/sbin/syslogd"</pre>  | 
| Process\.Pid | No | The process ID\. Type: number Example: <pre>"Pid": 12345</pre>  | 
| Process\.TerminatedAt | No | The timestamp for the date and time when the process was terminated\. Type: timestamp Example: <pre>"TerminatedAt": "2018-09-27T23:37:31Z"</pre>  | 
| ProductArn | Yes | The ARN generated by Security Hub that uniquely identifies a third\-party findings product after the product is registered with Security Hub\. Type: ARN The format of this field is `arn:partition:securityhub:region:account-id:product/company-id/product-id`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) Example: <pre>// Private ARN<br />    "ProductArn": "arn:aws:securityhub:us-east-1:111111111111:product/111111111111/default"<br />									<br />// Public ARN<br />    "ProductArn": "arn:aws:securityhub:us-west-2::product/aws/guardduty"<br />    "ProductArn": "arn:aws:securityhub:us-west-2:222222222222:product/generico/secure-pro"</pre>  | 
| ProductFields | No | A data type where security findings products can include additional solution\-specific details that aren't part of the defined AWS Security Finding Format\. Type: map of up to 50 key/value pairs This field shouldn't contain redundant data and must not contain data that conflicts with AWS Security Finding Format fields\. The "aws/" prefix represents a reserved namespace for AWS products and services only and must not be submitted with findings from partner products\. Although not required, products should format field names as `company-id/product-id/field-name`, where the `company-id` and `product-id` match those supplied in the `ProductArn` of the finding\. Fields names can include alphanumeric characters, white space, and the following symbols: \_ \. / = \+ \\ \- @ Example: <pre>"ProductFields": {<br />    "generico/secure-pro/Count": "6",<br />	"generico/secure-pro/Action.Type", "AWS_API_CALL",<br />	"API", "DeleteTrail",<br />	"Service_Name": "cloudtrail.amazonaws.com",<br />	"aws/inspector/AssessmentTemplateName": "My daily CVE assessment",<br />	"aws/inspector/AssessmentTargetName": "My prod env",<br />	"aws/inspector/RulesPackageName": "Common Vulnerabilities and Exposures"<br />}</pre>  | 
| RecordState | No | The record state of a finding\. Valid values are `ACTIVE` and `ARCHIVED`\.  Type: enum  By default, findings when initially generated by a service are considered `ACTIVE`\. The `ARCHIVED` state indicates that a finding should be hidden from view\. Archived findings aren't deleted and remain in the service historically\. You can search, review, and report against them at any time\. Example: <pre>"RecordState": "ACTIVE"</pre>  | 
| RelatedFindings | No | A list of related findings\.  Type: array of up to 10 `RelatedFinding` objects Example: <pre>"RelatedFindings": [<br />    { "ProductArn": "arn:aws:securityhub:us-west-2::product/aws/guardduty", <br />      "Id": "123e4567-e89b-12d3-a456-426655440000" },<br />	{ "ProductArn": "arn:aws:securityhub:us-west-2::product/aws/guardduty", <br />  	"Id": "AcmeNerfHerder-111111111111-x189dx7824" }<br />]</pre>  | 
| RelatedFindings\.Id | Yes | The product\-generated identifier for a related finding\. Type: string \(512 characters max\) or ARN Example: <pre>"Id": "123e4567-e89b-12d3-a456-426655440000"</pre>  | 
| RelatedFindings\.ProductArn | Yes | The ARN of the product that generated a related finding\. Type: ARN Example: <pre>"ProductArn": "arn:aws:securityhub:us-west-2::product/aws/guardduty"</pre>  | 
| Remediation | No | The remediation options for a finding\. Type: object Example: <pre>"Remediation": {<br />    "Recommendation": {<br />	"Text": "Run sudo yum update and cross your fingers and toes.",<br />	"Url": "http://myfp.com/recommendations/dangerous_things_and_how_to_fix_them.html"<br />	}<br />}</pre>  | 
| Remediation\.Recommendation | No | A recommendation on how to remediate the issue identified within a finding\. If the recommendation object is present, either the `Text` or `Url` field must be present and populated, though both can be present and populated\. The `Recommendation` field is meant to facilitate manual instructions or details to resolve a finding\. Type: object Example: <pre>"Recommendation": {<br />    "Text": "Example text.",<br />	"Url": "http://myfp.com/recommendations/dangerous_things_and_how_to_fix_them.html"<br />}</pre>  | 
| Recommendation\.Text | No | A free\-form string that is the recommendation of what to do about the finding when presented to a user\. This field can contain nonspecific boilerplate text or details that are specific to this instance of the finding\.Type: string \(512 characters max\) Example: <pre>"Text": "Example text."</pre>  | 
| Recommendation\.Url | No | A URL to link to general remediation information for the finding type of a finding\. This URL must not require credentials to access\. It must be accessible from the public internet and must not expect any context or session\. Type: URL Example: <pre>"Url": "http://myfp.com/recommendations/example_domain.html"</pre>  | 
| Resources | Yes | A set of resource data types that describe the resources that the finding refers to\. Type: array of up to 10 resource objects Example: <pre>"Resources": [<br />  {<br />    "Type": "AwsEc2Instance",<br />    "Id": "i-cafebabe",<br />    "Partition": "aws",<br />    "Region": "us-west-2",<br />    "Tags": {<br />      "billingCode": "Lotus-1-2-3",<br />      "needsPatching": "true"<br />    },<br />    "Details": {<br />      "AwsEc2Instance": {<br />        "Type": "i3.xlarge",<br />        "ImageId": "ami-abcd1234",<br />        "IpV4Addresses": [ "54.194.252.215", "192.168.1.88" ],<br />        "IpV6Addresses": [ "2001:db8:1234:1a2b::123" ],<br />        "KeyName": "my_keypair",<br />        "IamInstanceProfileArn": "arn:aws:iam::111111111111:instance-profile/AdminRole",<br />        "VpcId": "vpc-11112222",<br />        "SubnetId": "subnet-56f5f633",<br />        "LaunchedAt": "2018-05-08T16:46:19.000Z"<br />      }  <br />    }<br />  }<br />]</pre>  | 
| Resource\.Details | No | This field provides additional details about the resource through one \(and only one\) of its subfields\. The `Resource.Type` field indicates which of the subfields should contain data\. Because this field is optional, there might be a `Resource.Type` value set but no details in any subfields\. If a subfield is defined, the `Resource.Type` must match the subfield name\. For example, if the `AwsS3Bucket` subfield is populated, the `Resource.Type` must be set to `AwsS3Bucket`\.  You can define only one subfield in a finding\. All other subfields that contain finding details \(except the subfield defined in `Resource.Type`\) are ignored by all processors\.  If multiple resources are present, such as an Amazon EC2 instance and an Amazon S3 bucket, you must create two separate `Resource` objects in the `Resources` field\.  If a resource doesn't fit into one of the existing subfields, you can use the `Other` field to provide the resource details\. You must set the `Resource.Type` to `Other` when providing details in the `Other` subfield\. You must *not* use the `Other` field when an existing subfield is more appropriate for the resource that you're defining\. You must *not* use the `Other` field with any other subfield\. For example, when you use the `AwsEc2Instance` field, you must *not* provide additional details about an EC2 instance in the `Other` field\. Additionally, you can't use the `Other` field instead of the `AwsEc2Instance` field when providing details about an EC2 instance\. Type: object Example: <pre>"Details": {<br />  "AwsEc2Instance": {<br />    "Type": "i3.xlarge",<br />    "ImageId": "ami-abcd1234",<br />    "IpV4Addresses": [ "54.194.252.215", "192.168.1.88" ],<br />    "IpV6Addresses": [ "2001:db8:1234:1a2b::123" ],<br />    "KeyName": "my_keypair",<br />    "IamInstanceProfileArn": "arn:aws:iam::111111111111:instance-profile/AdminRole",<br />    "VpcId": "vpc-11112222",<br />    "SubnetId": "subnet-56f5f633",<br />    "LaunchedAt": "2018-05-08T16:46:19.000Z"<br />  },<br />  "AwsS3Bucket": {<br />    "OwnerId": "da4d66eac431652a4d44d490a00500bded52c97d235b7b4752f9f688566fe6de",<br />    "OwnerName": "acmes3bucketowner"<br />  },<br />  "Other": [<br />    { "Key": "LightPen", "Value": "blinky" },<br />    { "Key": "SerialNo", "Value": "1234abcd" }<br />  ]<br />}</pre>  | 
| Resource\.Details\.AwsEc2Instance | No | The details of an Amazon EC2 instance\.Type: object | 
| Resource\.Details\.AwsEc2Instance\.IamInstanceProfileArn | No | The IAM profile ARN of the instance\. | 
| Resource\.Details\.AwsEc2Instance\.ImageId | No | The Amazon Machine Image \(AMI\) ID of the instance\.Type: string \(64 characters max\) | 
| Resource\.Details\.AwsEc2Instance\.IpV4Addresses | No | The IPv4 addresses that are associated with the instance\.Type: array of up to 10 IPv4 addresses | 
| Resource\.Details\.AwsEc2Instance\.IpV6Addresses | No | The IPv6 addresses that are associated with the instance\.Type: array of up to 10 IPv6 addresses | 
| Resource\.Details\.AwsEc2Instance\.KeyName | No | The key name that is associated with the instance\.Type: string \(128 characters max\) | 
| Resource\.Details\.AwsEc2Instance\.LaunchedAt | No | The date and time when the instance was launched\. Type: timestamp | 
| Resource\.Details\.AwsEc2Instance\.SubnetId | No | The identifier of the subnet where the instance was launched\. Type: string \(32 characters max\) | 
| Resource\.Details\.AwsEc2Instance\.Type | No | The instance type of the instance\. This must be a valid [EC2 instance type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html)\. Type: string \(16 characters max\) | 
| Resource\.Details\.AwsEc2Instance\.VpcId | No | The identifier of the VPC where the instance was launched\. Type: string \(32 characters max\) | 
| Resource\.Details\.AwsIamAccessKey | No | IAM access key details that are related to a finding\.Type: object | 
| Resource\.Details\.AwsIamAccessKey\.CreatedAt | No | The creation date and time of the IAM access key that is related to a finding\.Type: timestamp | 
| Resource\.Details\.AwsIamAccessKey\.Status | No | The status of the IAM access key that is related to a finding\. Valid values are `ACTIVE` and `INACTIVE`\.Type: enum | 
| Resource\.Details\.AwsIamAccessKey\.UserName | No | The user associated with the IAM access key that is related to a finding\.Type: string \(128 char max\) | 
| Resource\.Details\.AwsS3Bucket | No | The details of an Amazon S3 bucket\.Type: object | 
| Resource\.Details\.AwsS3Bucket\.OwnerId | No | The canonical user ID of the owner of the Amazon S3 bucket\.Type: string \(64 char max\) | 
| Resource\.Details\.AwsS3Bucket\.OwnerName | No | The display name of the owner of the Amazon S3 bucket\.Type: string \(128 char max\) | 
| Resource\.Details\.Container | No | Container details that are related to a finding\. Type: object Example: <pre>"Container": {<br />    "Name": "Secret Service Container",<br />    "ImageId": "image12",<br />    "ImageName": "SecSvc v1.2 Image",<br />    "LaunchedAt": "2018-09-29T01:25:54Z"<br />}</pre>  | 
| Resource\.Details\.Container\.ImageId | No | The identifier of the image that is related to a finding\.Type: string \(128 characters max\) | 
| Resource\.Details\.Container\.ImageName | No | The name of the image that is related to a finding\.Type: string \(128 characters max\) | 
| Resource\.Details\.Container\.LaunchedAt | No | The date and time that the container was started\.Type: timestamp | 
| Resource\.Details\.Container\.Name | No | The name of the container that is related to a finding\.Type: string \(128 characters max\) | 
| Resource\.Details\.Other | No | The details of a resource that doesn't have a specific subfield for the resource type that is defined under `Resource.Details`\. To populate this field, you must set `Resource.Type.Other`\.Type: map of up to 50 key/value pairs For each key/value pair, they key must be less than 128 characters, and the value must be less than 1,024 characters\. | 
| Resource\.Id | Yes | The canonical identifier for the given resource type\. For AWS resources that are identified by ARNs, this must be the ARN\. For all other AWS resource types that lack ARNs, this must be the identifier as defined by the AWS service that created the resource\. For non\-AWS resources, this should be a unique identifier associated with the resource\.Type: string \(512 characters max\) or ARN Example: <pre>"Id": "arn:aws:s3:::example-bucket"</pre>  | 
| Resource\.Partition | No | The canonical AWS partition name that the Region is assigned to\.  Type: enum Valid values include the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) Example: <pre>"Partition": "aws"</pre>  | 
| Resource\.Region | No | The canonical AWS external Region name where this resource is located\. Type: string \(16 characters max\) Example: <pre>"Region": "us-west-2"</pre>  | 
| Resource\.Tags | No | A list of AWS tags that are associated with a resource at the time the finding was processed\. Include the `Resource.Tags` attribute only for resources that have an associated tag\. If a resource has no associated tag, don't include a `Resource.Tags` attribute in the finding\. Type: map of up to 50 tags \(values are limited to 256 characters max\) The following basic restrictions apply to tags: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) Example: <pre>"Tags": {<br />  "billingCode": "Lotus-1-2-3",<br />  "needsPatching": "true"<br />}</pre>  | 
| Resource\.Type | Yes | The type of the resource that you're providing details for\.  Type: string \(32 characters max\) Valid values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) Example: <pre>"Type": "AwsS3Bucket"</pre>  | 
| SchemaVersion | Yes | The schema version that a finding is formatted for\. The value of this field must be one of the officially published versions identified by AWS\. In the current release, the AWS Security Finding Format schema version is 2018\-10\-08\.  Type: string \(10 characters max, conforms to `YYYY-MM-DD`\) Example: <pre>"SchemaVersion": "2018-10-08"</pre>  | 
| Severity | Yes | A finding's severity\.Type: object Example: <pre>"Severity": {<br />  "Product": 8.3,<br />  "Normalized": 25<br />}</pre>  | 
| Severity\.Normalized | Yes | The normalized severity of a finding\. For findings that supported AWS services generate, Security Hub automatically translates the native severity into the normalized severity based on the following guidance\. For findings supported third\-party partner products generate, partners can use this guidance to determine the normalized severity required by the AWS Security Finding Format before sending these findings to Security Hub\. In the AWS Security Finding Format, a finding severity doesn't include consideration of the criticality of the assets that are involved in the activity that resulted in this finding\. Findings that are associated with actual data loss or denial of service are considered most severe\. Findings that are associated with an active compromise but that don't indicate that data loss or other negative effects have occurred are considered second\-most severe\. Findings associated with issues that indicate potential for a future compromise are considered third\-most severe\.  Severity is scored on a 0–100 basis, using a ratio scale that supports only full integers\. This means that when determining the normalized severity, you should assess not only which findings are more severe than others but also how more severe one finding is than another\. Zero means that no severity applies \(e\.g\., the severity is "Informational"\), and 100 means that the finding has the maximum possible severity\. We recommend that you use the following guidance when translating findings' native severity scores to normalized severity for the AWS Security Finding Format: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) In Security Hub, the normalized severity scores are available both in their numeric form and in a translated severity label using the following translation table\. You can use the severity labels in **Filters** and **Group By** statements when managing insights using `Severity.Label`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html)  | 
| Severity\.Product | No | The native severity as defined by the finding product that generated the finding\. Type: number \(single\-precision 32\-bit IEEE 754 floating point number, restricted to finite values\) | 
| SourceUrl | No | A URL that links to a page about the current finding in the finding product\. Type: URL | 
| ThreatIntelIndicators | No | Threat intel details that are related to a finding\. Type: array of up to five threat intel indicator objects Example: <pre>"ThreatIntelIndicators": [<br />  {<br />    "Type": "IPV4_ADDRESS",<br />    "Value": "8.8.8.8",<br />    "Category": "BACKDOOR",<br />    "LastObservedAt": "2018-09-27T23:37:31Z",<br />    "Source": "Threat Intel Weekly",<br />    "SourceUrl": "http://threatintelweekly.org/backdoors/8888"<br />  }<br />]</pre>  | 
| ThreatIntelIndicators\.Category | No | The category of a threat intel indicator\. Valid values are BACKDOOR \| CARD\_STEALER \| COMMAND\_AND\_CONTROL \| DROP\_SITE \| EXPLOIT\_SITE \| KEYLOGGER\.Type: enum | 
| ThreatIntelIndicators\.LastObservedAt | No | The date and time of the last observation of a threat intel indicator\.Type: timestamp | 
| ThreatIntelIndicators\.Source | No | The source of the threat intel\.Type: string \(64 characters max\) | 
| ThreatIntelIndicators\.SourceUrl | No | The URL for more details from the source of the threat intel\.Type: URL | 
| ThreatIntelIndicators\.Type | No | The type of a threat intel indicator\. Valid values are DOMAIN \| EMAIL\_ADDRESS \| HASH\_MD5 \| HASH\_SHA1 \| HASH\_SHA256 \| HASH\_SHA512 \| IPV4\_ADDRESS \| IPV6\_ADDRESS \| MUTEX \| PROCESS \| URL\.Type: enum | 
| ThreatIntelIndicators\.Value | No | The value of a threat intel indicator\.Type: string \(512 characters max\) | 
| Title | Yes | A finding's title\. This field can contain nonspecific boilerplate text or details that are specific to this instance of the finding\.Type: string \(256 characters max\) | 
| Types | Yes | One or more finding types in the format of `namespace/category/classifier` that classify a finding\. Type: array of 50 strings max Valid namespace values are Software and Configuration Checks \| TTPs \| Effects \| Unusual Behaviors \| Sensitive Data Identifications\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) Namespaces are required for all finding types, but categories and classifiers are optional\. If you specify a classifier is specified, you must also specify a category\. The '`/`' character is reserved and must *not* be used in a category or classifier\. Escaping the '`/`' character isn't supported\. Example: <pre>"Types": [<br />  "Software and Configuration Checks/Vulnerabilities/CVE"<br />]</pre>  | 
| UpdatedAt | Yes | An ISO8601\-formatted timestamp \(as defined in [RFC\-3339 Date and Time on the Internet: Timestamps](https://tools.ietf.org/html/rfc3339)\) that indicates when the findings product last updated the finding record\. Because this timestamp reflects the time when the finding record was last or most recently updated, it can differ from the `LastObservedAt` timestamp, which reflects when the event or vulnerability was last or most recently observed\. When you update the finding record, you must update this timestamp to the current timestamp\. Upon creation of a finding record, the `CreatedAt` and `UpdatedAt` timestamps must be the same timestamp\. After an update to the finding record, the value of this field must be greater than all of the previous values that it contained\.Type: timestamp Findings are deleted 90 days after the most recent update or 90 days after the creation date if no update occurs\. To store findings for longer than 90 days, you can configure a rule in CloudWatch Events that routes findings to your Amazon S3 bucket\.  | 
| UserDefinedFields | No | A list of name/value string pairs that are associated with the finding\. These are custom, user\-defined fields that are added to a finding\. These fields can be generated automatically via your specific configuration\. Findings products must *not* use this field for data that the product generates\. Instead, findings products can use the `ProductFields` field for data that doesn't map to any standard AWS Security Finding Format field\.Type: map of up to 50 key/value pairs Example: <pre>"UserDefinedFields": {<br />    "reviewedByCio": "true",<br />    "comeBackToLater": "Check this again on Monday"<br />}</pre>  | 
| VerificationState | No | The veracity of a finding\. Findings products can provide the value of `UNKNOWN` for this field\. A findings product should provide this value if there is a meaningful analog in the findings product's system\. This field is typically populated by a user determination or action after they have investigated a finding\.Type: enum Valid values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html)  | 
| WorkflowState | No | The workflow state of a finding\. Findings products can provide the value of `NEW` for this field\. A findings product can provide a value for this field if there is a meaningful analog in the findings product's system\. Type: enum Valid values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html) Example: <pre>"WorkflowState": "NEW"</pre>  | 
| aws | Commercial | 
| aws\-cn | China | 
| aws\-us\-gov | AWS GovCloud \(US\) | 
| Informational | 0 | 
| Low | 1–39 | 
| Medium | 40–69 | 
| High | 70–89 | 
| Critical | 90–100 | 

## Types Taxonomy of the AWS Security Finding<a name="securityhub-findings-format-type-taxonomy"></a>

The following information describes the first three levels of the `Types` path\. The top\-level bullets are namespaces, the second\-level bullets are categories, and the third\-level bullets \(shown only for Software and Configuration Checks\) are `Classifiers`\.
+ Namespaces
  + Categories
    + Classifiers

Findings products can define classifiers\. A findings product might define a partial path\. For example, the following finding types are all valid: TTPs, TTPs/Defense Evasion, and TTPs/Defense Evasion/CloudTrailStopped\.

TTPs stands for Tactics, Techniques, and Procedures\. The TTP categories in the following list align to the [MITRE ATT&CK MatrixTM](https://attack.mitre.org/matrices/enterprise/)\. Unusual Behaviors are different from TTPs because they reflect general unusual behavior \(e\.g\., general statistical anomalies\) and aren't aligned with a specific TTP\. However, you could classify a finding with both Unusual Behaviors and TTPs finding types\.
+ Software and Configuration Checks
  + Vulnerabilities
    + CVE
  + AWS Security Best Practices
    + Network Reachability
    + Runtime Behavior Analysis
  + Industry and Regulatory Standards
    + CIS Host Hardening Benchmarks
    + CIS AWS Foundations Benchmark
    + PCI\-DSS Controls
    + Cloud Security Alliance Controls
    + ISO 90001 Controls
    + ISO 27001 Controls
    + ISO 27017 Controls
    + ISO 27018 Controls
    + SOC 1
    + SOC 2
    + HIPAA Controls \(USA\)
    + NIST 800\-53 Controls \(USA\)
    + NIST CSF Controls \(USA\)
    + IRAP Controls \(Australia\)
    + K\-ISMS Controls \(Korea\)
    + MTCS Controls \(Singapore\)
    + FISC Controls \(Japan\)
    + My Number Act Controls \(Japan\)
    + ENS Controls \(Spain\)
    + Cyber Essentials Plus Controls \(UK\)
    + G\-Cloud Controls \(UK\)
    + C5 Controls \(Germany\)
    + IT\-Grundschutz Controls \(Germany\)
    + GDPR Controls \(Europe\)
    + TISAX Controls \(Europe\)
+ TTPs
  + Initial Access
  + Execution
  + Persistence
  + Privilege Escalation
  + Defense Evasion
  + Credential Access
  + Discovery
  + Lateral Movement
  + Collection
  + Command and Control
+ Effects
  + Data Exposure
  + Data Exfiltration 
  + Data Destruction 
  + Denial of Service 
  + Resource Consumption
+ Unusual Behaviors
  + Application
  + Network Flow
  + IP address
  + User
  + VM
  + Container
  + Serverless
  + Process
  + Database
  + Data 
+ Sensitive Data Identifications
  + PII
  + Passwords
  + Legal
  + Financial
  + Security
  + Business