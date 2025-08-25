CloudLaunch Project

This project sets up a simple AWS cloud environment with a static website hosted on S3 and a secure VPC design for future expansion.

---

 Task 1: S3 Website Hosting & IAM Access
- Created **two S3 buckets**:
  - `cloudlaunch-privatetest1`: hosts the static website (`index.html`).
  - `cloudlaunch-public-cloudassignment1`: stores private files (only accessible via IAM).
- Configured **S3 Static Website Hosting** on the public bucket.
- Enabled public access only for cloudlaunch-public-cloudassignment1` using a bucket policy.
- Kept `cloudlaunch-privatetest1` restricted (no public access).
- Created IAM policies:
  - **Admin Policy**: full access to both buckets.
  - **Read-Only Policy**: can only `GetObject` from the private bucket.

Static Website Link
[S3 Website Endpoint](http://cloudlaunch-public-cloudassignment1.s3-website-eu-west-1.amazonaws.com/)




[CloudFront URL](http://d3vqt2smj80gim.cloudfront.net)

---

Task 2: VPC Design
Designed a secure, logically separated **VPC** named `cloudlaunch-vpc` (`10.0.0.0/16`).

- **Subnets**:
  - Public Subnet: `10.0.1.0/24` → for load balancers / public services.
  - Application Subnet: `10.0.2.0/24` → for application servers (private).
  - Database Subnet: `10.0.3.0/28` → for databases (private).

- **Internet Gateway**:  
  - Created `cloudlaunch-igw` and attached it to `cloudlaunch-vpc`.

- **Route Tables**:
  - `cloudlaunch-public-rt`: Public subnet routes `0.0.0.0/0` to `cloudlaunch-igw`.
  - `cloudlaunch-app-rt`: Private route table, local routing only.
  - `cloudlaunch-db-rt`: Private route table, local routing only.

- **Security Groups**:
  - `cloudlaunch-app-sg`: Allows HTTP (80) from within VPC.
  - `cloudlaunch-db-sg`: Allows MySQL (3306) only from App subnet.

- **IAM Permissions**:
  - Created IAM user `cloudlaunch-user` with **read-only permissions** to VPC and its resources.

---

 IAM Policy for `cloudlaunch-user`

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets",
        "ec2:DescribeRouteTables",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeInternetGateways"
      ],
      "Resource": "*"
    }
  ]
}
```

---






 Summary
- Public static site available via S3.
- Cloudfront configured
- Private file storage is IAM-restricted.
- Secure VPC network created for future expansion.
- IAM controls implemented for least-privilege access.



Console sign-in URL

https://073186739637.signin.aws.amazon.com/console
Username:cloudlaunch-user
Password:Lipse255@

