How Puppet Works
======


### Easily get started with infrastructure automation
With Puppet you can start automating easily with an agentless task-based approach. You can move a step beyond from shareable scripts and leverage existing content on the Puppet Forge from your own workstation or laptop.


### Know what you have
With Puppet, you know what software you have, how, where, and why it changed, and who did it, all from one view. Always know what you have in your hybrid infrastructure so you can adapt to new technological challenges with confidence.


### Orchestrate change and applications intelligently 
Whether you schedule a change or push it out directly from HipChat, Git or Jenkins, or commit your next application update, Puppet gives you control, visibility, and automated intelligence to orchestrate change across your software and infrastructure.


### Ensure security & compliance, inherently
Puppet makes security and compliance inherent and automatic. With Puppet, you get the automation needed to continually enforce policies and the traceability required to prove compliance.
Today we will focus on routing internet traffic to the Amazon S3 bucket. Please ignore the CloudFront in the image below. We will perfom that someday.
![AWS_StaticWebsiteHosting_Architecture](https://github.com/ripples3/Static-Website-using-Amazon-S3/blob/master/AWS_StaticWebsiteHosting_Architecture.png)


### Adopt modern technologies with consistency
Puppet makes it possible to automatically install, configure and manage public, hybrid and private cloud, microservice and container resources -- and even manage cluster orchestration tools -- giving you instant portability and streamlined application delivery for multi-cloud environments.


### Full control and faster deployment of your applications
Puppet offers you a simplified standard way to build and deploy traditional and containerized applications with full control and visibility. Plus, we integrate with the most popular source control systems, cloud platforms and ChatOps tools so you don’t have to do custom integration work.


Does your bucket and website has the same domain name? If yes, Let proceed the onto the next steep.
Follow steps below to turn on static website hosting for you S3
- Navigate to S3 in the AWS Console
- Click into your bucket
- Click the “Properties” section
- Click the “Static website hosting” option
- Select “Use this bucket to host a website”
- Enter “index.html” as the Index document

Your bucket is configured for static website hosting, and you now have an S3 website url like this `http://example.com.s3-website-us-east-1.amazonaws.com/`.

Your bucket serves your static website, so it must be accessible to anyone in the world. This is referred to as anonymous access to the bucket.

By default, any new buckets created in an AWS account deny you the ability to add a public access bucket policy. This is in response to the recent leaky buckets where private information has been exposed to bad actors. However, for our use case, we need a public access bucket policy. To allow this you must complete the following steps before adding your bucket policy.

- Click into your bucket.
- Select the “Permissions” tab at the top.
- Under “Public Access Settings” we want to click “Edit”.
- Change “Block new public bucket policies”, “Block public and cross-account access if bucket has public policies”, and “Block new public ACLs and uploading public objects” to be false and Save.

You must complete this step before adding the bucket policy to your static website bucket.

Now you must update the Bucket Policy of your bucket to have public read access to anyone in the world. The steps to update the policy of your bucket in the AWS Console are as follows:

- Navigate to S3 in the AWS Console.
- Click into your bucket.
- Click the “Permissions” section.
- Select “Bucket Policy”.
- Add the following Bucket Policy and then Save

```
{
    "Version": "2008-10-17",
    "Id": "PolicyForPublicWebsiteContent",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::example.com/*"
        }
    ]
}
```

### 3. Adding A CNAME Record For Your Bucket Url
We are almost done!!! Now that your S3 bucket is accessible anywhere around the world.

In order for a user to load your S3 website you’ll need to provide mapping from your domain name example.com, to your S3 website url example.com.s3-website-us-east-1.amazonaws.com. This mapping is often referred to as a CNAME record inside of your Domain Name Servers (DNS) records.

#### To route traffic to an S3 bucket

Sign in to the AWS Management Console and open the Route 53 console at https://console.aws.amazon.com/route53/.

In the navigation pane, choose ##### Hosted Zones.

Choose the name of the hosted zone that has the domain name that you want to use to route traffic to your S3 bucket.

Choose **Create Record Set**

Specify the following values:

#### Name
Enter the domain name that you want to use to route traffic to your S3 bucket. The default value is the name of the hosted zone.

For example, if the name of the hosted zone is example.com and you want to use acme.example.com to route traffic to your bucket, enter acme.

#### Type
⋅⋅⋅Choose **A – IPv4 address**.

#### Alias
⋅⋅⋅Choose **Yes**.

Alias Target
In the S3 website endpoints section of the list, choose the bucket that has the same name that you specified for **Name**.

#### Routing Policy
Choose the applicable routing policy. For more information, see Choosing a Routing Policy.

#### Evaluate Target Health
Accept the default value of **No**.

Choose **Create**.

Changes generally propagate to all Route 53 servers within 60 seconds. When propagation is done, you'll be able to route traffic to your S3 bucket by using the name of the alias record that you created in this procedure.

### 4. Uploading Static Website
You can create the first page for your website and upload it to (save it in) your bucket.

1. Copy the following text and paste it into a text editor:
```
<html>
<head>
<title>Amazon Route 53 Getting Started</title>	
</head>

<body>

<h1>Routing Internet Traffic to an Amazon S3 Bucket for Your Website</h1>

<p>For more information, see 
<a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/getting-started.html">Getting Started with Amazon Route 53</a> 
in the <emphasis>Amazon Route 53 Developer Guide</emphasis>.</p>

</body>

</html>
```

2. Save the file with the file name **index.html**.

3. In the Amazon S3 console, choose the name of the bucket that you just created.

4. Choose **Upload**.

5. Choose **Add files**.

Follow the on-screen prompts to select **index.html**, and then choose **Start Upload**.

### 5. Validate That It Worked
Your static website has been uploaded to your S3 website bucket. You can go to [example.com] and your static website loads from your S3 bucket.

