The following resources were most helpful:
* https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html
* https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html
* https://www.tecmint.com/install-wordpress-on-ubuntu-16-04-with-lamp/
* https://wordpress.org/support/article/changing-the-site-url/#changing-the-htaccess-file
* https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/hosting-wordpress.html

### FTP to AWS - and it worked first time
* https://forums.aws.amazon.com/thread.jspa?messageID=450265
## Important Commands

## Route53 Setup
* https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/migrate-dns-domain-in-use.html

### Some Notes

* I wasn't too careful about my db setups and may have left my setup on localhost db instead of wp-petruskruger
* The AMI I stored worked great for launching a new instance of different tier.

## Choosing an EC2 Instance
https://aws.amazon.com/ec2/instance-types/t3/
* My t3.nano instance seems to be crashing the DB constantly. It may be that I exhausted the burst capacity.
* I should test again a little later.
