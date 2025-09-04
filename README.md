# CloudLaunch Project

This project sets up a simple AWS cloud environment with a **static website** hosted on S3 and a secure **VPC design** for future expansion.

---

##  Task 1: S3 Website Hosting & IAM Access
- Created **two S3 buckets**:
  - `cloudlaunch-public`: hosts the static website (`index.html`).
  - `cloudlaunch-private`: stores private files (only accessible via IAM).
- Configured **S3 Static Website Hosting** on the public bucket.
- Enabled public access only for `cloudlaunch-public` using a bucket policy.
- Kept `cloudlaunch-private` restricted (no public access).
- Created IAM policies:
  - **Admin Policy**: full access to both buckets.
  - **Custom Read Policy**: a limited policy allowing only bucket listing and metadata lookup.

###  Static Website Link
[S3 Website Endpoint](http://cloudlaunch-public.s3-website-us-east-1.amazonaws.com)


[CloudFront URL](http://d3vqt2smj80gim.cloudfront.net)

---

##  Task 2: VPC Design
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
  - Created IAM user `cloudlaunch-user` with a **custom read-only policy** allowing only bucket listing and location metadata lookup.

---

## Custom IAM Policy for `cloudlaunch-user`

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowListBuckets",
      "Effect": "Allow",
      "Action": [
        "s3:ListAllMyBuckets",
        "s3:GetBucketLocation"
      ],
      "Resource": "*"
    }
  ]
}
```

---

##  Summary
- Public static site available via S3.
- Private file storage is IAM-restricted.
- Secure VPC network created for future expansion.
- IAM controls implemented with a **custom limited-access policy**.




