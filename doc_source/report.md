--------

This guide is for the Snowball Edge \(100 TB of storage space\)\. If you are looking for documentation for the Snowball, see the [AWS Snowball User Guide](http://docs.aws.amazon.com/snowball/latest/ug/whatissnowball.html)\.

--------

# Get Your Job Completion Report and Logs in the Console<a name="report"></a>

Whenever data is imported into or exported out of Amazon S3, you get a downloadable PDF job report\. For import jobs, this report becomes available at the very end of the import process\. For export jobs, your job report typically becomes available for you while the AWS Snowball Edge appliance for your job part is being delivered to you\.

The job report provides you insight into the state of your Amazon S3 data transfer\. The report includes details about your job or job part for your records\. The job report also includes a table that provides a high\-level overview of the total number of objects and bytes transferred between the appliance and Amazon S3\.

For deeper visibility into the status of your transferred objects, you can look at the two associated logs: a success log and a failure log\. The logs are saved in comma\-separated value \(CSV\) format, and the name of each log includes the ID of the job or job part that the log describes\.

You can download the report and the logs from the AWS Snowball Management Console\.

**To get your job report and logs**

1. Sign in to the AWS Management Console and open the [AWS Snowball Management Console](https://console.aws.amazon.com/importexport/home?region=us-west-2)\.

1. Select your job or job part from the table and expand the status pane\.

   Three options appear for getting your job report and logs: **Get job report**, **Download success log**, and **Download failure log**\.

1. Choose the log you want to download\.

The following list describes the possible values for the report:

+ **Completed** – The transfer was completed successfully\. You can find more detailed information in the success log\.

+ **Completed with errors** – Some or all of your data was not transferred\. You can find more detailed information in the failure log\.

**Next:** [Where Do I Go from Here?](where-to.md) 