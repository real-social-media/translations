# REAL Frontend Resources

## Overview

This [serverless](https://serverless.com) project stores and serves static files used by the frontend app.

- files are stored encrypted-at-rest in S3
- files are globally cached & served via CloudFront

Useful CloudFormation outputs:

- `S3FrontendResourcesBucketName`: name of the S3 bucket to upload files to
- `CloudFrontFrontendResourcesDomainName`: CloudFront domain where files can be downloaded

## Translation Files

Translation files are json files that live in the directory `/translations/`. They are expected to have the two-letter language code as their filename. For example: `/translations/en.json`

### Updating translation files

#### Via the `upload-translation` script

```console
$ bin/upload-translation en.json
Uploading `en.json` to `s3://{bucket-name}/translations/en.json`... uploaded.
Creating invalidation for CloudFront path `/translations/en.json`... created.
File is available for download at `https://{cloudfront-subdomain}.cloudfront.net/translations/en.json`
```

Run `bin/upload-translation -h` for usage details.

#### Via AWS Web Console

- navigate to the [AWS S3 Web Console](https://s3.console.aws.amazon.com/s3/home)
- select the correct bucket
- create the `translations` folder if it doesn't already exist
- inside the `translations` foler, click `upload` and upload your new or updated translations file
- naviate to the [AWS CloudFront Web Console](https://console.aws.amazon.com/cloudfront/home)
- select the correct distribution
- go to the *Invalidations* tab
- create an invalidation for the file you just uploaded

### List the CloudFront translation file urls

#### Via the `list-translation-urls` script

```console
$ bin/list-translation-urls
https://{cloudfront-subdomain}.cloudfront.net/translations/en.json
https://{cloudfront-subdomain}.cloudfront.net/translations/es.json
https://{cloudfront-subdomain}.cloudfront.net/translations/de.json
```

Run `bin/list-translation-urls -h` for usage details.

#### Via AWS Web Console

- navigate to the [AWS CloudFront Web Console](https://console.aws.amazon.com/cloudfront/home)
- select the correct distribution
- note the *Domain Name*
- navigate to the [AWS S3 Web Console](https://s3.console.aws.amazon.com/s3/home)
- select the correct bucket
- look inside the `translations` folder and note what files are there
- for each file, its cloudfront url is `https://{cloudfront-domain}/translations/{file}`
