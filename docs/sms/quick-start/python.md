no_breadcrumb:true

# SMS Python Quick Start

Welcome to the RingCentral Platform. RingCentral is the leading unified communications platform. From one system developers can integrate with, or build products around all the ways people communicate today: SMS, voice, fax, chat and meetings.

In this Quick Start, we are going to help you send your first SMS on the platform in just a few minutes. Let's get started.

## Create an App

The first thing we need to do is create an app in the RingCentral Developer Portal. This can be done quickly by clicking the "Create SMS App" button below. Just click the button, enter a name and description if you choose, and click the "Create" button. If you do not yet have a RingCentral account, you will be prompted to create one.

<a target="_new" href="https://developer.ringcentral.com/new-app?name=SMS+Quick+Start+App&desc=A+simple+app+to+demo+sending+an+SMS+on+RingCentral&public=false&type=ServerOther&carriers=7710,7310,3420&permissions=SMS,ReadMessages&redirectUri=" class="btn btn-primary">Create SMS App</a>
<a class="btn-link btn-collapse" data-toggle="collapse" href="#create-app-instructions" role="button" aria-expanded="false" aria-controls="create-app-instructions">Show detailed instructions</a>

<div class="collapse" id="create-app-instructions">
<ol>
<li><a href="https://developer.ringcentral.com/login.html#/">Login or create an account</a> if you have not done so already.</li>
<li>Go to Console/Apps and click 'Create App' button.</li>
<li>Give your app a name and description, then click Next.</li>
<li>On the second page of the create app wizard enter the following:
  <ul>
  <li>Select 'Private' for Application Type.</li>
  <li>Select 'Server-only (No UI)' for Platform Type.</li>
  </ul>
  </li>
<li>On the third page of the create app wizard, select the following permissions:
  <ul>
    <li>SMS</li>
    <li>Webhook Subscriptions</li>
  </ul>
  </li>
<li>Leave "OAuth Redirect URI" blank for now. We will come back and edit that later.</li>
</ol>
</div>

When you are done, you will be taken to the app's dashboard. Make note of the Client ID and Client Secret. We will be using those momentarily.

## Send an SMS

<h3>Install Python Module</h3>

<pre><code>pip install ringcentral
</code></pre>

<h3>Create and Edit sms.py</h3>

<p>Create a file called <tt>sms.py</tt>. Be sure to edit the variables in ALL CAPS with your app and user credentials. Be sure to also set the recipient's phone number.</p>

<pre><code>from ringcentral import SDK

RECIPIENT = '&lt;ENTER PHONE NUMBER>'

RINGCENTRAL_CLIENTID = '&lt;ENTER CLIENT ID>'
RINGCENTRAL_CLIENTSECRET = '&lt;ENTER CLIENT SECRET>'
RINGCENTRAL_SERVER = 'https://platform.devtest.ringcentral.com'

RINGCENTRAL_USERNAME = '&lt;YOUR ACCOUNT PHONE NUMBER>'
RINGCENTRAL_PASSWORD = '&lt;YOUR ACCOUNT PASSWORD>'
RINGCENTRAL_EXTENSION = '&lt;YOUR EXTENSION, PROBABLY "101">'

sdk = SDK( RINGCENTRAL_CLIENTID, RINGCENTRAL_CLIENTSECRET, RINGCENTRAL_SERVER)
platform = sdk.platform()
platform.login(RINGCENTRAL_USERNAME, RINGCENTRAL_EXTENSION, RINGCENTRAL_PASSWORD)

platform.post('/restapi/v1.0/account/~/extension/~/sms',
              {
                  'from' : { 'phoneNumber': RINGCENTRAL_USERNAME },
                  'to'   : [ {'phoneNumber': RECIPIENT} ],
                  'text' : 'Hello World from Python'
              },
              None,
              { 'Content-Type': 'application/json' } )
</code></pre>

<h3>Run Your Code</h3>

<p>You are almost done. Now run your script.</p>

<pre><code class="bash">$ python sms.py
</code></pre>

## Publish Your App

Congratulations on creating your first RingCentral application. The last step is to publish your application. We recommend [going through this process](../basics/publish) for your first application so you can understand the steps to take in the future, but also to come to appreciate the care taken by RingCentral to ensure that only high-quality apps are allowed into our production environment.