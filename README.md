# Python Flask Quickstart Sample Code for Integrating with Okta using the Redirect Model

This repository contains a sample of integrating with [Okta](https://www.okta.com/) for authentication using [the redirect model in a Python Flask app](https://developer.okta.com/docs/guides/sign-into-web-app/python/main/).

Read more about getting started with Okta and authentication best practices on the [Okta Developer Portal](https://developer.okta.com).

This code sample demonstrates

* Configuring Okta
* Sign-in and sign-out
* Protecting routes
* Displaying user profile information from the ID Token

## Prerequisites

Before running this sample, you will need the following:

* [The Okta CLI Tool](https://github.com/okta/okta-cli#installation)
* An Okta Developer Account (create one using `okta register`, or configure an existing one with `okta login`)
* Poetry

## The Code

This repo was created by using `okta start flask`
and I just followed the instructions printed to the console.
After that, I changed the port becuase MacOS has 5000 already in use:

* changed the port from `5000` in the app.py to `8080`
* changed the port on the okta app in the management console from `5000` to `8080`

## Run the Example

* run this application, install its dependencies:

    ```shell
    poetry shell
    python3 app.py
    ```

Navigate to <http://localhost:8080> in your browser.

If you see a home page that prompts you to login, then things are working!  Clicking the **Log in** button will redirect you to the Okta hosted sign-in page.

You can sign in with the same account that you created when signing up for your Developer Org, or you can use a known username and password from your Okta Directory.

> **Note:** If you are currently using your Developer Console, you already have a Single Sign-On (SSO) session for your Org.  You will be automatically logged into your application as the same user that is using the Developer Console.  You may want to use an incognito tab to test the flow from a blank slate.

## Helpful resources

* [Learn about Authentication, OAuth 2.0, and OpenID Connect](https://developer.okta.com/docs/concepts/)
* [Get started with Flask](https://flask.palletsprojects.com/en/2.0.x/quickstart/)

## Help

Please visit our [Okta Developer Forums](https://devforum.okta.com/).

### Okta authorization servers

Okta authorization servers handle token issuance and validation etc. There is always an "org authorization server" at the url
<https://{domain}.okta.org/oauth>. You can also create addtional "custom authorization servers" if you need customized behaviors.
A new Okta organization automatically has the org authorization server active at <https://{domain}.okta.org/oauth> and a
custom authorization server called "default" availble at <https://{domain}.okta.org/oauth/default>.

In my (limited) experience, the out of the box custom authorization server called "default" is enabled, but has no policy or rules
attached and as such, does not work for OIDC/OAuth authentication. Strangely however, the okta cli and most of the Okta
examples you find assume that you will use the custom authorization server called "default".

Solutions:

1. Change the urls to use the app.py to use the org auth server. For
   example change the following to remove the `/default` auth server
   (you'll have to do this for all URLs):

   ```python
   # base_url=os.environ['ORG_URL'] + "oauth2/default/v1/authorize",
   base_url=os.environ['ORG_URL'] + "oauth2/v1/authorize",
   ```

2. Modify the custom server called "default" to have a Policy and a rule. 

   1. Go to Admin console <https://dev-1234567-admin.okta.com/admin/dashboard>
   2. Go to Security > API > Authorization Servers > select "default" auth server
   3. Go to Access Polices > click "Add New Access Policy" (I called my "default")
   4. Click "Add Rule". I called mine "Catch ALL" and took the default settings.
