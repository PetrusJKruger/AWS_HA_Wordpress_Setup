# Create a Highly Available Wordpress Site

/* ****************** PLAN ***************** */


  * Create an S3 bucket for Code with default settings
  * Create an S3 bucket for Media with default settings
  * Create a Cloudfront Distribution to source Media bucket with default settings. 
  * Create a WEBDMZ security group that is open to the world
  * Create a RDS security group that is open to your WEBDMZ security group
    on the the MySQL port.
  * Create RDS instance
    * MySQL
    * Include a Multi AZ instance
    * Use burstable class T2 Micro
    * Do not make it publicly accessible
    * Add to RDS Security group
    * Add DB name in Additional Information section to ensure a DB is created
  * Setup a role to allow access to S3 for EC2
  * Provision EC2 instance with S3 for EC2 role
    * Add to WEBDMZ security group


# For your wordpress site, you'll need to configure your EC2 instances with the following steps:

* SSH into your EC2 instance
* Check that the appache service is running.
  * service httpd status
  * service httpd start
* Navigate to ip address
* Do Wordpress Setup
  * Use your RDS instances endpoint for the database host
  * Create the wp-config,php file using the terminal
  * If this process hangs, your security group is probably blocking port 3306
  * Complete Setup
* Login to WP

To force Cloudfront to store all uploaded photo's to S3:

In the terminal window:
* Type aws s3 ls
 * If you cannot see your S3 buckets, you'll need to add the role.
* Copy files to s3 using:
 * aws s3 cp --recursive /var/www/html/wp-content/uploads s3://...media bucket
* Copy code to s3 using:
 *  aws s3 cp --recursive /var/www/html/ s3://...code bucket
* Edit the .htaccess file to point to the s3 media bucket
 * Get domain name from cloudfront
* Setup sync to s3 using:
 * aws s3 sync /var/html/ s3://...code bucket
* Tell apache that we will allow url rewrite using:
 * cd /etc/httpd
 * cd conf
 * ls
 * cp httpd.conf httpd-copy.conf #for backup of file
 * nano httpd.conf
 * Scroll to "AllowOverride" rule
 * Change rule to "All"
 * service httpd restart #to restart apache just in case it needs a kickstart to pick up changes
 * change bucket policy for media s3 bucket
  * {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": [
            "s3:GetObject"
            ],
          "Resource": [
            "arn:aws:s3:::BUCKET_NAME/*"
            ]
        }
      ]
    }
 * Change the ARN
 * You may need to edit your public access settings
 * It may take an hour or two for Cloudfront to propogate
 * Create Application Load Balancer
   * Setup with usual
   * Put in all availability zones
   * Put in WEBDMZ SG
   * Health check is "healthy.html"
   * Healthy Threshold to 2
   * Unhealthy Threshold to 3
   * Timeout to 5 seconds
   * Interval = 6 seconds
   * Success code = 200
   
  # Point to domain in Route-53
  * Chose domain
  * Create Record Set
  * Leave name as naked domain name
  * Select an alias = application load balancer
  
  # Place EC2 instances in target group
  
  
