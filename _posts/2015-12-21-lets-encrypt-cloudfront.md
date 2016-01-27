---
layout: post
title: "How to setup Letsencrypt with Amazon S3 and Cloudfront to get free SSL"
tags:
- Let's Encrypt
- S3
- Cloudfront
- AWS
thumbnail_path: blog/lets-encrypt/logo.png
---
{% include figure.html path="blog/lets-encrypt/logo.png" link_to_full_size_image=false %}

CloudFront offers SSL certificates and they are only [$600](https://aws.amazon.com/cloudfront/custom-ssl-domains/)!

# PART 1
There is a CLI certificate installation and verification, [here](https://github.com/dlapiduz/letsencrypt-s3front). Simply follow the instructions and the setup should take less than 5 minutes.

**However** in the case that it doesn't work simply follow the instructions below.

<pre>
Bucket Name = domain.com
</pre>
Now, you want your site to accessible from the www subdomain as well and you also want the Lets Encrypt certificate to show up on both. Therefore your subdomain will be.
<pre>
www.domain.com
</pre>
Then, run the letsencrypt script. We will be creating a 2048 bit certificate. This is the [max](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/SecureConnections.html) for cloudfront.
<pre>
./letsencrypt-auto certonly -a manual -d domain.com -d www.domain.. --rsa-key-size 2048 --server https://acme-v01.api.letsencrypt.org/directory --agree-tos
</pre>
**Note:** Make sure both domain.com and www.domain.com are listed like above.
If all went well, you will have to prove ownership of the domain by placing a file into your S3 bucket. Simply create the appropriate folder and place the text into an index.html file.

Once successfully verified you will receive the following message.

<pre>
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/domain.com/fullchain.pem. Your cert
   will expire on 2016-03-04. To obtain a new version of the
   certificate in the future, simply run Let's Encrypt again.
 - If like Let's Encrypt, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
   </pre>

   
# PART 2
Create a user in IAM and give it cloudfront permissions.
Now that we have an SSL certificate it is time to import it into AWS. Download and install the [AWS CLI](https://aws.amazon.com/cli/). It works on Windows as well.
Run aws configure. Then paste the following code into the terminal (I used Windows).
<pre>
iam upload-server-certificate 
  --server-certificate-name mycert 
  --certificate-body file:///C:/cert.pem 
  --private-key file:///C:\privkey.pem 
  --certificate-chain file:///C:\chain.pem 
  --path /cloudfront/ 
  </pre>
**Note:** file:/// must be present for it to work.

# PART 3
Once the certificate is uploaded login to CloudFront go to the General tab click edit, then import. Your custom SSL cert should be there.

{% include figure.html path="blog/lets-encrypt/import.png" caption="Import Certificate and save" link_to_full_size_image=true %}

**Optional:** Now go to the Behaviors tab, and edit the Default behavior. You may wish to redirect HTTP to HTTPS.

{% include figure.html path="blog/lets-encrypt/redirect.png" caption="Redirect to HTTPs only" link_to_full_size_image=true %}

Conclusion:
{% include figure.html path="blog/lets-encrypt/final.png" caption="Let's Encrypt Certificate installed on S3/Cloudfront" link_to_full_size_image=true %}
