# orcid-auth-widget
A simple JS widget for obtaining authenticated ORCID iDs using OAuth with OpenID Connect and collecting id_tokens.

Note: This uses implicit OAuth, so does not collect any write permissions.  This means it is not suitable for member integrations that wish to update records.  

## Demo
https://orcid.github.io/orcid-auth-widget/widget.html

## Widget setup
### Pre-requisites

- You have a live website, and have access to add/edit HTML code including ```<script>``` tags on the page that you would like to embed the widget into
- It is possible for you to host the orcid-widget.js on your server, or copy the source code into your page.
- You have registered an ORCID public or member API client, per [Register your ORCID API client](https://support.orcid.org/hc/en-us/categories/360000663054-Register-your-ORCID-API-client)

Note: you cannot link to the .js file on github, as github will prevent it being loaded.

### Basic setup

Copy and paste the code below into a page on your website that you'd like the widget to appear on. Edit the code to include your ORCID API client ID and the URL of the page that you've pasted the code into. 

    <script src="http://[YOUR SERVER LOCATION FOR THE SCRIPT]/orcid-widget.js"></script>
    <div id="orcidWidget" data-clientid='[YOUR ORCID API CLIENT ID]' data-redirecturi='[URI OF THE PAGE YOU HAVE PASTED THIS CODE INTO]'></div>

If you cannot host the .js file, then you can copy and paste the contents of orcid-widget.js in between the script tags.

### Configuration options

The following configuration options are available and can be added to the div tag as ```data-``` tag attributes.

| Tag attribute | Allowed values | Default | Description                           |
| ------------- | -------------- | ------- | --------------------------------------|
| data-size     | lg, sm         | sm      | Widget size (400px wide or 300px wide) | 
| data-clientid |                |         | **Required** Your ORCID public or member API client ID. To obtain a client ID, see [Register your ORCID API client](https://support.orcid.org/hc/en-us/categories/360000663054-Register-your-ORCID-API-client) |  
| data-env      | production, sandbox | sandbox  | ORCID environment (https://sandbox.orcid.org or https://orcid.org) | 
| data-scopes   | See [scopes](https://github.com/ORCID/ORCID-Source/tree/master/orcid-model/src/main/resources/record_2.0#scopes)       | openid | Requested permission scopes.  Only the openid scope is supported at this time| 
| data-redirecturi   |          |       | **Required** The full URI of the page on your site that the widget is displayed on (ie: the page pasted the code above into). This URI must be registered as one of your ORCID API client's redirect URIs as described in the help docs in [Register your ORCID API client](https://support.orcid.org/hc/en-us/categories/360000663054-Register-your-ORCID-API-client).    | 
| data-submituri   |          |       |  The API endpoint that data obtained by this widget should be submitted to after a user signs into ORCID using the widget. If data-submituri is not specified, data is inserted into hidden input fields on the page that the widget is located on.  |

Example configuration using all available options:

    <div id="orcidWidget" data-size='lg' data-clientid='APP-XXXXXXXXXXXXXXXX' data-env='production' data-scopes='/read-limited%20/activities/update%20/person/update' data-redirecturi='https://you-site/your-page.html' data-submituri='https://your-api-endpoint/submit'></div>

## Widget usage

1. Direct user to the page that you embedded the widget into, ex: during an existing workflow on your site (like registering for an account) or by sending an email with a link to the page.  You may optionally add a state parameter to this URL in order to identify the user ```https://you-site/your-page.html?state=abc123```.
<img src="/img/widget-button.png" width="400" alt="Widget button">


2. User clicks the 'Connect your ORCID iD' button included in the widget and is prompted to sign into ORCID. If the user does not have an ORCID iD, they can register at this time.
<img src="/img/oauth-signin.png" width="400" alt="ORCID signin">

3. User is presented with a screen showing information about your API client and the permission scopes that you have requested (as specified in data-scopes).
<img src="/img/oauth-authorize.png" width="400" alt="ORCID authorize">

4. User clicks Authorize or Deny and is directed back to the page that you embedded the widget into (as specified in data-redirecturi) and a success or error message is displayed.
<img src="/img/widget-success.png" width="400" alt="Widget success message">

5. The following hidden input fields are added to your page:

        <input type="hidden" id="orcidId" name="orcidId" value="https://orcid.org/0000-0001-5727-2427">
        <input type="hidden" id="orcidGivenName" name="orcidGivenName" value="Sofia Maria">
        <input type="hidden" id="orcidFamilyName" name="orcidFamilyName" value="Hernandez Garcia">
        <input type="hidden" id="orcidIdToken" name="orcidIdToken" value="eyJraWQiOiJxYS1vcmNpZC1vcmctcjlhZmw3cWY2aGNnN2c5bmdzenU1bnQ3Z3pmMGVhNmkiLCJhbGciOiJSUzI1NiJ9.eyJhdF9oYXNoIjoiMW52bXZBbVdwaVd0Z3ZKZW1DQmVYUSIsImF1ZCI6IkFQUC02TEtJSjNJNUIxQzRZSVFQIiwic3ViIjoiMDAwMC0wMDAyLTUwNjItMjIwOSIsImF1dGhfdGltZSI6MTUwNTk4Nzg2MiwiaXNzIjoiaHR0cHM6XC9cL29yY2lkLm9yZyIsIm5hbWUiOiJNciBDcmVkaXQgTmFtZSIsImV4cCI6MTUwNTk4ODQ2MywiZ2l2ZW5fbmFtZSI6IlRvbSIsImlhdCI6MTUwNTk4Nzg2Mywibm9uY2UiOiJ3aGF0ZXZlciIsImZhbWlseV9uYW1lIjoiRGVtIiwianRpIjoiY2U0YzlmNWUtNTBkNC00ZjhiLTliYzItMmViMTI0ZDVkNmNhIn0.hhhts2-4-ibjXPW6wEsFRaNqV_A-vTz2JFloYn7mS1jzQt3xuHiSaSIiXg3rpnt1RojF_yhcvE9Xe4SOtYimxxVycpjcm8yT_-7lUSrc46UCt9qW6gV7L7KQyKDjNl23wVwIifpRD2JSnx6WbuC0GhAxB5-2ynj6EbeEEcYjAy2tNwG-wcVlnfJLyddYDe8AI_RFhq7HrY4OByA91hiYvHzZ8VzoRW1s4CTCFurA7DoyQfCbeSxdfBuDQbjAzXuZB5-jD1k3WnjqVHrof1LHEPTFV4GQV-pDRmkUwspsPYxsJyKpKWSG_ONk57E_Ba--RqEcE1ZNNDUYHXAtiRnM3w">
        <input type="hidden" id="orcidState" name="orcidState" value="abc123">

6. If data-submituri is specified in your widget configuration, the following JSON data is sent to that URI via an HTTP POST request:

        {"id_token": "eyJraWQiOiJxYS1vcmNpZC1vcmctcjlhZmw3cWY2aGNnN2c5bmdzenU1bnQ3Z3pmMGVhNmkiLCJhbGciOiJSUzI1NiJ9.eyJhdF9oYXNoIjoiMW52bXZBbVdwaVd0Z3ZKZW1DQmVYUSIsImF1ZCI6IkFQUC02TEtJSjNJNUIxQzRZSVFQIiwic3ViIjoiMDAwMC0wMDAyLTUwNjItMjIwOSIsImF1dGhfdGltZSI6MTUwNTk4Nzg2MiwiaXNzIjoiaHR0cHM6XC9cL29yY2lkLm9yZyIsIm5hbWUiOiJNciBDcmVkaXQgTmFtZSIsImV4cCI6MTUwNTk4ODQ2MywiZ2l2ZW5fbmFtZSI6IlRvbSIsImlhdCI6MTUwNTk4Nzg2Mywibm9uY2UiOiJ3aGF0ZXZlciIsImZhbWlseV9uYW1lIjoiRGVtIiwianRpIjoiY2U0YzlmNWUtNTBkNC00ZjhiLTliYzItMmViMTI0ZDVkNmNhIn0.hhhts2-4-ibjXPW6wEsFRaNqV_A-vTz2JFloYn7mS1jzQt3xuHiSaSIiXg3rpnt1RojF_yhcvE9Xe4SOtYimxxVycpjcm8yT_-7lUSrc46UCt9qW6gV7L7KQyKDjNl23wVwIifpRD2JSnx6WbuC0GhAxB5-2ynj6EbeEEcYjAy2tNwG-wcVlnfJLyddYDe8AI_RFhq7HrY4OByA91hiYvHzZ8VzoRW1s4CTCFurA7DoyQfCbeSxdfBuDQbjAzXuZB5-jD1k3WnjqVHrof1LHEPTFV4GQV-pDRmkUwspsPYxsJyKpKWSG_ONk57E_Ba--RqEcE1ZNNDUYHXAtiRnM3w"
        "state": "abc123"}








