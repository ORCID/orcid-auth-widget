# orcid-microplatform-auth
A simple JS widget for obtaining authenticated ORCID iDs using OAuth with OpenID Connect

## Demo
https://orcid.github.io/orcid-microplatform-auth/widget.html

## Widget setup
### Pre-requisites

- You have a live website, and have access to add/edit HTML code including ```<script>``` tags on the page that you would like to embed the widget into
- You have registered an ORCID public or member API client, per [Register your ORCID API client](https://support.orcid.org/hc/en-us/categories/360000663054-Register-your-ORCID-API-client)

## Basic setup

Copy and paste the code below into a page on your website that you'd like the widget to appear on. Edit the code to include your ORCID API client ID and the URL of the page that you've pasted the code into. 

    <script src="https://orcid.github.io/orcid-microplatform-auth/orcid-widget.js"></script>
    <div id="orcidWidget" data-clientid='[YOUR ORCID API CLIENT ID]' data-redirecturi='[URI OF THE PAGE YOU HAVE PASTED THIS CODE INTO]'></div>

## Configuration options

The following configuration options are available and can be added to the div tag as ```data-``` tag attributes.

| Tag attribute | Allowed values | Default | Description                           |
| ------------- | -------------- | ------- | --------------------------------------|
| data-size     | lg, sm         | sm      | Widget size (400px wide or 300px wide) | 
| data-clientid |                |         | **Required** Your ORCID public or member API client ID. To obtain a client ID, see [Register your ORCID API client](https://support.orcid.org/hc/en-us/categories/360000663054-Register-your-ORCID-API-client) |  
| data-env      | production, sandbox | sandbox  | ORCID environment (https://sandbox.orcid.org or https://orcid.org) | 
| data-scopes   | See [scopes](https://github.com/ORCID/ORCID-Source/tree/master/orcid-model/src/main/resources/record_2.0#scopes)       | openid | Requested permission scopes other than openid. When using this widget only to get a user's authenticated ORCID iD, no additional scopes are needed. If other scopes are requested, id tokens returned by this widget must be exchanged for access tokens as described in [Token Delegation](https://github.com/ORCID/ORCID-Source/blob/master/orcid-api-web/tutorial/token_delegation.md) in order to make use of those scopes. To request multiple scopes, separate each scope with a URL-encoded space, ex ```/read-limited%20/activities/update%20/person/update``` .| 
| data-redirecturi   |          |       | **Required** The full URI of the page on your site that the widget is displayed on (ie: the page pasted the code above into). This URI must be registered as one of your ORCID API client's redirect URIs as described in the help docs in [Register your ORCID API client](https://support.orcid.org/hc/en-us/categories/360000663054-Register-your-ORCID-API-client).    | 
| data-submituri   |          |       |  The API endpoint that data obtained by this widget should be submitted to after a user signs into ORCID using the widget. If data-submituri is not specified, data is inserted into hidden input fields on the page that the widget is located on.  |

Example configuration using all available options:

    <script src="https://orcid.github.io/orcid-microplatform-auth/orcid-widget.js"></script>
    <div id="orcidWidget" data-size='lg' data-clientid='APP-XXXXXXXXXXXXXXXX' data-env='production' data-scopes='/read-limited%20/activities/update%20/person/update' data-redirecturi='https://you-site/your-page.html' data-submituri='https://your-api-endpoint/submit'></div>








