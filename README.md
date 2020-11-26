# Serverless Static Website Deployment S3 & Cloudfront

This service will deploy a ***static website*** to AWS via the Serverless Framework: https://www.serverless.com/

It will deploy with the preferred features listed below.

## S3 Features
 - Creation of bucket if it doesn't exist
 - Encrypted Bucket (AWS default AES256)
 - Private S3 bucket ACL
 - Base configured CacheControl file headers (for Cloudfront to utilise)
 - Deletion of files in S3 not in local folder being deployed.
 - "index.html" as the default root index document
 - "404.html" as the 404 & 403 error document (S3 returns 403 Access Denied for pages that don't exist).

## Cloudfront Features

 - Entering Cloudfront Access Identity permission only to S3 Bucket (NOTE: Identity needs to already exist)
 - Lambda@Edge function connection to "origin-request" for redirection of index.html pages to SEO friendly urls (e.g. /url displays the /urls/index.html page. NOTE: Lambda@Edge function needs to already exist).
	 - See here: https://github.com/digital-sailors/standard-redirects-for-cloudfront
 - Redirects http to https
 - Preferred opinionated default cache behaviour settings. NOTE: Assumes that Cache Invalidation will be done on each deploy through "serverless-cloudfront-invalidate".
 - Custom error pages for 404 & 403 using 404.html

## Configure serverless.yml required variables

    AWS_PROFILE:        # (required) Enter aws credentials profile name here
    WEBSITE_NAME:       # (required) e.g. website-name
    WEBSITE_BUCKET_NAME:        # (required) e.g. website-bucket-name
    CLOUDFRONT_IDENTITY:        # (required) cloudfront identity
    LOCAL_DEPLOY_DIRECTORY:     # (required) e.g. public
    CLOUDFRONT_COMMENT:     # (required) Comment here will show in Cloudfront console.
    SLS_DEPLOY_BUCKET_NAME:     # (required) Only saves Cloudformation templates. I like to keep them altogether across projects.
    LAMBDA_REDIRECT_FUNCTION_ARN:   # (required) arn:aws:lambda:us-east-1:XXXXXX

## Deployment / Removal

You can deploy and remove using serverless, `stage` and `region` are permitted params:

    # Deploy
    sls deploy --stage=production --region=ap-southeast-2
	# Remove
    sls remove --stage=production --region=ap-southeast-2

Alternatively you can use the package.json scripts available - note: update region as desired:

For **Yarn**:

    yarn deploy:production
    yarn remove:production

For **npm**:

	npm run deploy:production
    npm run remove:production
