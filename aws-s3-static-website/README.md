# Deploy a Static Website on AWS
- In this project, I created a technical guide for hosting a static website using AWS infrastructure. The main goal was to document a clear and reusable process that uses Amazon S3 for file storage, Amazon CloudFront for secure (HTTPS) content delivery, and Amazon Route 53 for domain name management (DNS), along with an external provider such as GoDaddy.

- Throughout this process, I focused on building a secure, scalable, and efficient hosting solution with a custom domain. This document shares the requirements, configuration steps, and implementation phases that I followed, as well as what I learned while setting up the environment.

# 1. Setting Up an S3 Bucket for Static Hosting:
- In this step, I prepared the base for the website’s content. The S3 bucket is where all the site files are stored safely. I set it up with the right permissions and enabled static website hosting to get it ready for the next step, where the content will be shared globally.

## Procedimiento de Creación y Configuración del Bucket:
### 1.1 Creación del Bucket:
- En el campo Nombre del bucket, ingrese el nombre de dominio personalizado que se utilizará. Esta es una regla crítica: el nombre del bucket debe coincidir exactamente con el nombre de dominio (p. ej., yourdomain.com).

## 1.2 Public Access Settings:
- When creating the bucket, uncheck the Block all public access option.
- Check the confirmation box acknowledging that this setting may make the bucket and its objects public. This step is essential so that users' browsers can request and render the website's files.
- Upload your files

## 1.3 Enabling Static Hosting:
- Once the bucket is created, navigate to the Properties tab.
- Scroll down to the Static Website Hosting section and click Edit.
- Select the option to Enable Static Website Hosting.
- Set index.html as the Index Document and save the changes.

## 1.4 Bucket Policy Definition:
- Navigate to the Permissions tab and, in the Bucket Policy section, click Edit.
- In this part, I edited the bucket policy to set the right access permissions for the website. This step allows the files in the S3 bucket to be publicly available through the web or securely accessed by CloudFront. It’s an important setup to make sure the website can load correctly from any location
- Here you can find the code (https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html)

## Initial Verification:
- To confirm that the configuration is correct, return to the Properties tab and, in the Static Website Hosting section, find the bucket website endpoint (e.g., [http://yourdomain.com.s3-website-us-west-2.amazonaws.com][http://yourdomain.com.s3-website-us-west-2.amazonaws.com/]). When accessing this URL in a browser, the website should load correctly. At this stage, the site is only accessible through the S3 URL and not the custom domain, and the connection is made over HTTP.

- The next step is to configure content distribution and HTTPS security using Amazon CloudFront.

# 2. Implementing CloudFront for Secure Distribution (HTTPS):
- S3 static website hosting does not support HTTPS, so we use CloudFront as the secure entry point. CloudFront handles HTTPS traffic, serves content from its global CDN, and fetches files from the S3 bucket over HTTP, improving both performance and security.

## 2.1 Setting Up the Distribution:
- In the AWS console, navigate to the CloudFront service and select Create a Distribution.
- In the Source Domain field, enter the S3 static website endpoint generated in the previous step.

## 2.2 Viewer Protocol Policy:
- In the Default Behavior configuration section, set the Viewer Protocol Policy to Redirect HTTP to HTTPS. This enforces that all user requests are served over a secure connection.

## 2.3 SSL Certificate Configuration:
- To enable HTTPS on a custom domain, your CloudFront distribution must be associated with a validated SSL/TLS certificate. In the Configuration section, click Request Certificate to be redirected to AWS Certificate Manager (ACM).
- Solicite un nuevo certificado público.
- Agregue el nombre de dominio completo (p. ej., yourdomain.com) y una variante con el prefijo www (p. ej., www.yourdomain.com).
- Choose the DNS Validation method. If your DNS is managed by Route 53, use the Create Records in Route 53 button to automate the validation process. This option demonstrates one of the advantages of maintaining DNS services within the AWS ecosystem.

## 2.4 Certificate Binding and Alternative Domain Names:
- Once the certificate status in ACM changes to "Issued," return to the CloudFront distribution settings and click Edit.
- In the Custom SSL Certificate section, select the certificate you just created.
- CRITICAL: In the Alternative Domain Names (CNAME) field, add both domain names: [yourdomain.com](http://yourdomain.com/) and [www.yourdomain.com(http://www.yourdomain)](http://www.yourdomain.com/).
- Technical Analysis: Skipping this step is the root cause of the ERR_SSL_VERSION_OR_CIPHER_MISMATCH error. When an HTTPS request for [evgenyurubkov.com](http://evgenyurubkov.com/) reaches CloudFront, it inspects the distribution's CNAME field. If it doesn't find a match, it doesn't know which SSL certificate to present, resulting in a TLS negotiation failure that the browser interprets as such an error.
- Save the changes to begin deploying the updated distribution.

With the CloudFront distribution configured and deployed, the final step is to update the DNS records in Route 53 to direct the domain's traffic to this distribution.


