--------

This guide is for the Snowball Edge \(100 TB of storage space\)\. If you are looking for documentation for the Snowball, see the [AWS Snowball User Guide](http://docs.aws.amazon.com/snowball/latest/ug/whatissnowball.html)\.

--------

# Using the Amazon S3 Adapter<a name="using-adapter"></a>

Following, you can find an overview of the Amazon S3 Adapter for Snowball, which allows you to programmatically transfer data to and from the AWS Snowball Edge appliance using Amazon S3 REST API actions\. This Amazon S3 REST API support is limited to a subset of actions\. You can use this subset of actions with one of the AWS SDKs to transfer data programmatically\. You can also use the subset of supported AWS Command Line Interface \(AWS CLI\) commands for Amazon S3 to transfer data programmatically\.

If your solution uses the AWS SDK for Java version 1\.11\.0 or newer, you must use the following `S3ClientOptions`:

+ `disableChunkedEncoding()` – Indicates that chunked encoding is not supported with the adapter\.

+ `setPathStyleAccess(true)` – Configures the adapter to use path\-style access for all requests\.

For more information, see [Class S3ClientOptions\.Builder](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/S3ClientOptions.Builder.html) in the Amazon AppStream SDK for Java\.

**Important**  
We recommend that you only use one method of reading and writing data to a local bucket on an AWS Snowball Edge appliance at a time\. Using both the file interface and the Amazon S3 Adapter for Snowball on the same bucket at the same time can result in read/write conflicts\.

## Getting and Using Local Amazon S3 Credentials<a name="adapter-credentials"></a>

Every interaction with a Snowball Edge is signed with the AWS Signature Version 4 algorithm\. For more information on the algorithm, see [Signature Version 4 Signing Process](http://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\.

You can get the local Amazon S3 credentials to sign your requests to the Snowball Edge device by running the `snowballEdge list-access-keys` and `snowballEdge get-secret-access-key` Snowball client commands\. For more information, see [Getting Credentials](using-client-commands.md#client-credentials)\. These local Amazon S3 credentials include a pair of keys: an access key ID and a secret key\. These credentials are only valid for the appliances associated with your job\. They can't be used in the AWS Cloud because they have no AWS Identity and Access Management \(IAM\) counterpart\.

You can add these credentials to the AWS credentials file on your server\. The default credential profiles file is typically located at `~/.aws/credentials`, but the location can vary per platform\. This file is shared by many of the AWS SDKs and by the AWS CLI\. You can save local credentials with a profile name as in the following example\.

```
[snowballEdge]
aws_access_key_id = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

### Specifying the Adapter as the AWS CLI Endpoint<a name="using-adapter-cli-endpoint"></a>

When you use the AWS CLI to issue a command to the AWS Snowball Edge appliance, you specify that the endpoint is the Amazon S3 Adapter for Snowball\. You have the choice of using the HTTPS endpoint, or an unsecured HTTP endpoint, as shown following\.

**HTTPS secured endpoint**

```
aws s3 ls --profile snowballEdge --endpoint https://192.0.2.0:8443 --ca-bundle path/to/certificate
```

**HTTP unsecured endpoint**

```
aws s3 ls --profile snowballEdge --endpoint http://192.0.2.0:8080
```

If you use the HTTPS endpoint of `8443`, your data is securely transferred from your server to the Snowball Edge\. This encryption is ensured with a certificate that's generated by the Snowball Edge whenever it gets a new IP address\. After you have your certificate, you can save it to a local `ca-bundle.pem` file\. Then you can configure your AWS CLI profile to include the path to your certificate, as described following\.

**To associate your certificate with the adapter endpoint**

1. Connect the Snowball Edge to power and network, and turn it on\.

1. After the device has finished booting up, make a note of its IP address on your local network\.

1. From a terminal on your network, make sure you can ping the Snowball Edge\.

1. Run the `snowballEdge get-certificate` command in your terminal\. For more information on this command, see [Getting Your Certificate for Transferring Data](using-client-commands.md#snowball-edge-certificates-cli)\.

1. Save the output of the `snowballEdge get-certificate` command to a file, for example `ca-bundle.pem`\.

1. Run the following command from your terminal\.

   ```
   aws configure set snowballEdge.ca_bundle /path/to/ca-bundle.pem
   ```

After you complete the procedure, you can run CLI commands with these local credentials, your certificate, and your specified endpoint, as in the following example\.

```
aws s3 ls --profile snowballEdge --endpoint https://192.0.2.0:8443
```