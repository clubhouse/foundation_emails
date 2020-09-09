# Clubhouse HTML Email Templates :e-mail:

This repository generates responsive, inlined HTML emails for use in [Intercom](https://app.intercom.com). It is built using [Foundation for Emails](http://foundation.zurb.com/emails), see the default README [preserved below](https://github.com/clubhouse/foundation_emails#for-reference---foundation-for-emails-readme) :point_down: for more information about using this framework.

## Using this Repository

This assumes installation of [Node.js](https://nodejs.org/en/) 0.12 or greater and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) on your development machine. 

* Clone repository and run `npm install` from the repo's root directory
* Run `npm start` to kick off the dev process. A new browser tab will open with a server pointing to your project files

## Project structure

Foundation for emails utilizes Handlebars HTML templates with [Panini](http://github.com/zurb/panini) & a simplified HTML email syntax called [Inky](http://github.com/zurb/inky).

Individual emails are generated from the files located in `/src/pages`. Clubhouse page files declare which layout they extend, set the email subject line and `utm-content` variable for email link tracking. Body content and link urls are also declared in each page file.

### Example page file

```
---
layout: clubhouse-layout
subject: Welcome! Get started in 3 easy steps.
utmContent: drip1o
---

{{> drip-body-welcome-v1
  listItemOne="https://clubhouse.io/webinars/training-building-blocks/?utm_source=email&utm_medium=ch&utm_campaign=onboarding&utm_term=learn&utm_content=drip1o"
  listItemTwo="https://app.clubhouse.io/?utm_source=email&utm_medium=ch&utm_campaign=onboarding&utm_term=create&utm_content=drip1o"
  listItemThree="https://app.clubhouse.io/?utm_source=email&utm_medium=ch&utm_campaign=onboarding&utm_term=invite&utm_content=drip1o"
}}
```

### Other files in the `/src` directory

* `/assets`
  * `/img` - place image files here for development, or streamline your workflow by uploading them to Prismic early on... see below :point_down:
  * `/scss` & `/scss/partials` - styles declared here will be inlined and minified during the build process
* `/layouts` - a page's layout is declared in its frontmatter, see above example :point_up:
* `/partials` - keep templates lean by breaking them down into resuable partials

## Output Process for Intercom

### 1. Host images in Prismic

Images are hosted in our [prismic.io](https://clubhouse.prismic.io/) media library. After an image is uploaded, view its details, right click on the image and "Copy link address" to obtain the file's URI. 

*Note:* Always include `?auto=compress,format` at the end of the image URI to return an optimized version of the image file.

:warning: Update all image src paths to their prismic URIs before proceeding :warning:

### 2. Run build script

Run `npm run build` to inline CSS styles into the HTML and minify the output during the build process. Output code ready to paste into Intercom will be appear in the `/dist` folder.

### 3. Intercom tags

It is required to add `{{ content }}` and `{{ unsubcribe_url }}` Intercom template tags to our templates when saving them in Intercom. To facilitate this, the Clubhouse email templates contain the following placemarkers:
* replace `[--INTERCOM--CONTENT--]` with `{{ content }}`
* replace `[--INTERCOM-UNSUBSCRIBE-URL--]` with `{{ unsubscribe_url }}`

:tada: **Voila! Your emails are ready to paste into Intercom** :tada:

## For Reference - Foundation for Emails README

[![devDependency Status](https://david-dm.org/zurb/foundation-emails-template/dev-status.svg)](https://david-dm.org/zurb/foundation-emails-template#info=devDependencies)

**Please open all issues with this template on the main [Foundation for Emails](http://github.com/zurb/foundation-emails/issues) repo.**

This is the official starter project for [Foundation for Emails](http://foundation.zurb.com/emails), a framework for creating responsive HTML devices that work in any email client. It has a Gulp-powered build system with these features:

- Handlebars HTML templates with [Panini](http://github.com/zurb/panini)
- Simplified HTML email syntax with [Inky](http://github.com/zurb/inky)
- Sass compilation
- Image compression
- Built-in BrowserSync server
- Full email inlining process

### Installation

To use this template, your computer needs [Node.js](https://nodejs.org/en/) 0.12 or greater. The template can be installed with the Foundation CLI, or downloaded and set up manually.

#### Using the CLI

Install the Foundation CLI with this command:

```bash
npm install foundation-cli --global
```

Use this command to set up a blank Foundation for Emails project:

```bash
foundation new --framework emails
```

The CLI will prompt you to give your project a name. The template will be downloaded into a folder with this name.

#### Manual Setup

To manually set up the template, first download it with Git:

```bash
git clone https://github.com/zurb/foundation-emails-template projectname
```

Then open the folder in your command line, and install the needed dependencies:

```bash
cd projectname
npm install
```

### Build Commands

Run `npm start` to kick off the build process. A new browser tab will open with a server pointing to your project files.

Run `npm run build` to inline your CSS into your HTML along with the rest of the build process.

Run `npm run litmus` to build as above, then submit to litmus for testing. *AWS S3 Account details required (config.json)*

Run `npm run mail` to build as above, then send to specified email address for testing. *SMTP server details required (config.json)*

Run `npm run zip` to build as above, then zip HTML and images for easy deployment to email marketing services. 

#### Speeding Up Your Build

If you create a lot of emails, your build can start to slow down, as each build rebuilds all of the emails in the
repository. A simple way to keep it fast is to archive emails you no longer need by moving the pages into `src/pages/archive`.
You can also move images that are no longer needed into `src/assets/img/archive`. The build will ignore pages and images that
are inside the archive folder.

### Litmus Tests (config.json)

Testing in Litmus requires the images to be hosted publicly. The provided gulp task handles this by automating hosting to an AWS S3 account. Provide your Litmus and AWS S3 account details in the `example.config.json` and then rename to `config.json`. Litmus config, and `aws.url` are required, however if you follow the [aws-sdk suggestions](http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-configuring.html) you don't need to supply the AWS credentials into this JSON.

```json
{
  "aws": {
    "region": "us-east-1",
    "accessKeyId": "YOUR_ACCOUNT_KEY",
    "secretAccessKey": "YOUR_ACCOUNT_SECRET",
    "params": {
        "Bucket": "elasticbeanstalk-us-east-1-THIS_IS_JUST_AN_EXAMPLE"
    },
    "url": "https://s3.amazonaws.com/elasticbeanstalk-us-east-1-THIS_IS_JUST_AN_EXAMPLE"
  },
  "litmus": {
    "username": "YOUR_LITMUS@EMAIL.com",
    "password": "YOUR_ACCOUNT_PASSWORD",
    "url": "https://YOUR_ACCOUNT.litmus.com",
    "applications": ["ol2003","ol2007","ol2010","ol2011","ol2013","chromegmailnew","chromeyahoo","appmail9","iphone5s","ipad","android4","androidgmailapp"]
  }
}
```

### Manual email tests (config.json)

Similar to the Litmus tests, you can have the emails sent to a specified email address. Just like with the Litmus tests, you will need to provide AWS S3 account details in `config.json`. You will also need to specify to details of an SMTP server. The email address to send to emails to can either by configured in the `package.json` file or added as a parameter like so: `npm run mail -- --to="example.com"`

```json
{
  "aws": {
    "region": "us-east-1",
    "accessKeyId": "YOUR_ACCOUNT_KEY",
    "secretAccessKey": "YOUR_ACCOUNT_SECRET",
    "params": {
        "Bucket": "elasticbeanstalk-us-east-1-THIS_IS_JUST_AN_EXAMPLE"
    },
    "url": "https://s3.amazonaws.com/elasticbeanstalk-us-east-1-THIS_IS_JUST_AN_EXAMPLE"
  },
  "mail": {
    "to": [
      "example@domain.com"
    ],
    "from": "Company name <info@company.com",
    "smtp": {
      "auth": {
        "user": "example@domain.com",
        "pass": "12345678"
      },
      "host": "smtp.domain.com",
      "secureConnection": true,
      "port": 465
    }
  }
}
```

For a full list of Litmus' supported test clients(applications) see their [client list](https://litmus.com/emails/clients.xml).

**Caution:** AWS Service Fees will result, however, are usually very low do to minimal traffic. Use at your own discretion.

