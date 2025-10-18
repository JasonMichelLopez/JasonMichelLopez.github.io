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
- Technical Analysis: Skipping this step is the root cause of the ERR_SSL_VERSION_OR_CIPHER_MISMATCH error. When an HTTPS request for [yourdomain.com](http://yourdomain.com/) reaches CloudFront, it inspects the distribution's CNAME field. If it doesn't find a match, it doesn't know which SSL certificate to present, resulting in a TLS negotiation failure that the browser interprets as such an error.
- Save the changes to begin deploying the updated distribution.

- With the CloudFront distribution configured and deployed, the final step is to update the DNS records in Route 53 to direct the domain's traffic to this distribution.

# 3. Final DNS Configuration in Route 53
- In this final part, the architecture is completed by reconfiguring the custom domain's DNS records. The goal is to direct all end-user traffic arriving through the domain name to the CloudFront distribution, thus enabling global, fast, and secure access to the website.

## 3.1 Creating the Hosted Zone
- Create a public Hosted Zone in Amazon Route 53 for your custom domain. If the domain was registered with a third-party provider (e.g., Google Domains), you'll need to update the name servers in that registrar's control panel with the four name servers provided by the AWS Hosted Zone. Please note that it may take up to 48 hours for these changes to propagate across the internet.

## 3.2 Updating DNS Records
1. Deleting Old Records: If an 'A' record was previously configured to point directly to the S3 endpoint, it should be deleted to avoid conflicts.

2. Creating the 'A' Record for the Root Domain:
* Within your Hosted Zone, click Create Record.
* Leave the record name blank to configure the root domain (e.g., evgenyurubkov.com).
* Select the A record type.
* Enable the Alias ​​option. Under Route traffic to, choose Alias ​​to the CloudFront distribution.
* In the Route traffic to field, your new CloudFront distribution may not immediately appear in the drop-down list. If it is not available, navigate to the CloudFront console, copy the distribution's Domain Name (e.g., d12345abcdef.cloudfront.net), and paste it directly into the field.
* Click Create Records.

3. Create the 'A' Record for the 'www' Subdomain:
* Repeat the above process to create a second record.
* In the Record Name, type www.
* Select the A record type and enable Aliases.
* Point the record to the same CloudFront distribution using the same copy and paste method if necessary.
* Click Create Records.

With these two records, both domain.com and www.domain.com will direct traffic to the CloudFront distribution.

## 4. Final Verification and Expected Behaviors
- Final validation is a crucial step to ensure all architectural components work in harmony. It's important to understand that changes in distributed systems like DNS and CDNs aren't instantaneous. The expected behaviors and root causes of potential discrepancies are described below.

### DNS Propagation:
* The website may be unreachable or display a DNS error immediately after updating the nameservers or 'A' records.

* Explanation: DNS propagation is an asynchronous process that can take up to 48 hours to complete globally. DNS servers around the world refresh their caches at different intervals. A waiting period should be allowed before assuming a configuration error.

### SSL Negotiation Errors:
* When accessing the site over HTTPS, an error occurs. This error can manifest itself in several ways, including ERR_SSL_VERSION_OR_CIPHER_MISMATCH in desktop browsers or a 403 error (The request could not be satisfied) returned by the CloudFront API.

* Root Cause: The root cause is not adding the custom domain names (evgenyurubkov.com and www.evgenyurubkov.com) to the Alternate Domain Names (CNAME) field in the CloudFront distribution configuration.

* Solution: Edit the CloudFront distribution, add the correct domain names to the CNAME field, and save the changes to start a new deployment.

### CloudFront Deployment:
* Changes made to the CloudFront configuration (e.g., adding a certificate or CNAMEs) are not reflected immediately.

* Explanation: Each modification to a distribution initiates a new deployment. During this process, which may take several minutes, AWS propagates the new configuration to its global network of over 400 edge locations. The distribution status in the CloudFront console will indicate when the process is complete.

## Final Test

Once the DNS changes have been propagated and the CloudFront distribution is fully deployed, the website should be securely accessible via https://domain.com and https://www.domain.com. The browser should display a security padlock, confirming that the HTTPS connection is working properly.

## Conclusion

This project demonstrates a basic and reliable setup for hosting static websites using AWS services. It includes secure access through HTTPS, global delivery with CloudFront, and domain management with Route 53. The goal is to provide a simple and scalable foundation that can be improved over time, and I’m open to any suggestions or feedback to make it better.
