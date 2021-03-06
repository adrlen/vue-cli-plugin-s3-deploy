s3-deploy for vue-cli
===

[![npm version](https://badge.fury.io/js/vue-cli-plugin-s3-deploy.svg)](https://badge.fury.io/js/vue-cli-plugin-s3-deploy)

This [vue-cli](https://github.com/vuejs/vue-cli) plugin aims to make it easier to deploy a built Vue.js app to an S3 bucket.

Supports:

* Custom AWS regions
* Concurrent uploads for improved deploy times
* CloudFront distribution invalidation
* Correct `Cache-Control` metadata for use with PWAs and Service Workers

Prerequisites
---

You must have a set of [valid AWS credentials set up on your system](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html).


Installation
---
```
yarn add vue-cli-plugin-s3-deploy
```

Usage
---

After installation, invoke the plugin with `vue invoke s3-deploy`.

Answer the configuration prompts. This will inject a `deploy` script command into your `package.json` file.

Deploy your app with `yarn deploy`.

Options
---

Options are set in `vue.config.js` and overridden on a per-environment basis by `.env`, `.env.staging`, `.env.production`, etc.

```js
{
    bucket: "The S3 bucket name (required)",
    region: "AWS region for the specified bucket (default: us-east-1)",
    assetPath: "The path to the built assets (default: dist)",
    uploadConcurrency: "The number of concurrent uploads to S3 (default: 3)",
    pwa: "Sets max-age=0 for the PWA-related files specified",
    enableCloudfront: "Enables support for Cloudfront distribution invalidation",
    cloudfrontId: "The ID of the distribution to invalidate",
    cloudfrontMatchers: "A comma-separated list of paths to invalidate (default: /*)"
}
```

The `pwa` option is meant to help make deploying progressive web apps a little easier. Due to the way service workers interact with caching, this option alone will tell the browser to not cache the `service-worker.js` file by default. This ensures that changes made to the service worker are reflected as quickly as possible.

You can specify which files aren't cached by setting a value for the `pwaFiles` option:

```js
{
    pwaFiles: "index.html,dont-cache.css,not-this.js"
}
```

Per-Environment Overrides
---

Deployment options can be overridden with .env files to support development, staging, and production deployment environments.

The .env file options are, with examples:

```sh
VUE_APP_S3D_BUCKET=staging-bucket
VUE_APP_S3D_ASSET_PATH=dist/staging
VUE_APP_S3D_REGION=staging-aws-east-1
VUE_APP_S3D_PWA=service-worker-stage.js,index.html
VUE_APP_S3D_UPLOAD_CONCURRENCY=5
VUE_APP_S3D_ENABLE_CLOUDFRONT=true
VUE_APP_S3D_CLOUDFRONT_ID=AIXXXXXXXX
VUE_APP_S3D_CLOUDFRONT_MATCHERS=/index.html,/styles/*.css,/*.png
```

**These options OVERRIDE the config options set in vue.config.js** and should be used to customize a default set of options. A common use case is only overriding `VUE_APP_S3D_BUCKET` for production deployment.


Changelog
---

**v2.0.2**

- Fixed bug where deployment crashes if you declined Cloudfront on initial invocation.

**v2.0.0**
- Added support for invalidating Cloudfront distributions on deploy. 
- Refactored how the configuration is stored and brought it more inline with vue cli standards. All config is in vue.config.js now.
- Updated the dependency on vue-cli to 3.0.0-rc3
- Squashed a few bugs along the way

**v1.3**
- Added support for .env files and per-environment options

**v1.2**
- Added parallel uploading

**v1.1**
- Initial Release

Contributing
---

Contributions welcome. 
Just open a pull request.
