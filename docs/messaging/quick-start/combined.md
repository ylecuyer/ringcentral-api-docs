no_breadcrumb:true
style: quick-start

# SMS Quick Start

Welcome to the RingCentral Platform. RingCentral is the leading unified communications platform. From one system developers can integrate with, or build products around all the ways people communicate today: SMS, voice, fax, chat and meetings.

In this Quick Start, we are going to help you send your first SMS on the platform in just a few minutes. Let's get started.

!!! note "Also check out our SMS Best Practices Guide"
    After you finish this quick start, be sure to read our [SMS Best Practices Guide](../../sms/best-practices/).

## Create App and Get Credentials

The first thing we need to do is create an app in the RingCentral Developer Portal. This can be done quickly by clicking the "Create SMS App" button below. Just click the button, enter a name and description if you choose, and click the "Create" button. If you do not yet have a RingCentral account, you will be prompted to create one.

<a target="_new" href="https://developer.ringcentral.com/new-app?name=SMS+Quick+Start+App&desc=A+simple+app+to+demo+sending+an+SMS+on+RingCentral&public=false&type=ServerOther&carriers=7710,7310,3420&permissions=SMS,ReadMessages&redirectUri=&utm_source=devguide&utm_medium=button&utm_campaign=quickstart" class="btn btn-primary">Create SMS App</a>
<a class="btn-link btn-collapse" data-toggle="collapse" href="#create-app-instructions" role="button" aria-expanded="false" aria-controls="create-app-instructions">Show detailed instructions</a>

<div class="collapse" id="create-app-instructions">
<ol>
<li><a href="https://developer.ringcentral.com/login.html#/">Login or create an account</a> if you have not done so already.</li>
<li>Go to Console/Apps and click 'Create App' button.</li>
<li>Select "API App for RingCentral Office" under "What type of app are you creating?"</li>
<li>Select "Other Non-UI" under "Where will you be calling the API from?"
<li>Select "Only members of my organization/company" under "Who will be authorized to access your app?"
<li>On the second page of the create app wizard, enter your app's name and description. Then select the following permissions:
  <ul>
    <li>SMS</li>
  </ul>
  </li>
<li>We are using Password Flow authentication, so leave "OAuth Redirect URI" blank.</li>
</ol>
</div>

When you are done, you will be taken to the app's dashboard. Make note of the Client ID and Client Secret. We will be using those momentarily.

## Send an SMS

Select your preferred language below.

=== "Javascript"

    ### Install RingCentral JavaScript SDK

    ```bash
    $ npm install @ringcentral/sdk --save
    ```

    ### Create and Edit sms.js

    Create a file called `sms.js`. Be sure to edit the variables in ALL CAPS with your app and user credentials. Be sure to also set the recipient's phone number.

    ```javascript
    const SDK = require('@ringcentral/sdk').SDK

    RECIPIENT = '<ENTER PHONE NUMBER>'

    RINGCENTRAL_CLIENTID = '<ENTER CLIENT ID>'
    RINGCENTRAL_CLIENTSECRET = '<ENTER CLIENT SECRET>'
    RINGCENTRAL_SERVER = 'https://platform.devtest.ringcentral.com'

    RINGCENTRAL_USERNAME = '<YOUR ACCOUNT PHONE NUMBER>'
    RINGCENTRAL_PASSWORD = '<YOUR ACCOUNT PASSWORD>'
    RINGCENTRAL_EXTENSION = '<YOUR EXTENSION, PROBABLY "101">'

    var rcsdk = new SDK({
        server: RINGCENTRAL_SERVER,
        clientId: RINGCENTRAL_CLIENTID,
        clientSecret: RINGCENTRAL_CLIENTSECRET
    });
    var platform = rcsdk.platform();
    platform.login({
        username: RINGCENTRAL_USERNAME,
        password: RINGCENTRAL_PASSWORD,
        extension: RINGCENTRAL_EXTENSION
        })
        .then(function(resp) {
            send_sms()
        });

    function send_sms(){
      platform.post('/restapi/v1.0/account/~/extension/~/sms', {
           from: {'phoneNumber': RINGCENTRAL_USERNAME},
           to: [{'phoneNumber': RECIPIENT}],
           text: 'Hello World from JavaScript'
         })
         .then(function (resp) {
            console.log("SMS sent. Message status: " + resp.json().messageStatus)
         });
    }
    ```

    ### Run Your Code

    You are almost done. Now run your script.

    ```bash
    $ node sms.js
    ```

=== "Python"

    ### Install RingCentral Python SDK

    ```bash
    $ pip install ringcentral
    ```

    ### Create and Edit sms.py

    Create a file called `sms.py`. Be sure to edit the variables in ALL CAPS with your app and user credentials. Be sure to also set the recipient's phone number.

    ```python
    from ringcentral import SDK

    RECIPIENT = '<ENTER PHONE NUMBER>'

    RINGCENTRAL_CLIENTID = '<ENTER CLIENT ID>'
    RINGCENTRAL_CLIENTSECRET = '<ENTER CLIENT SECRET>'
    RINGCENTRAL_SERVER = 'https://platform.devtest.ringcentral.com'

    RINGCENTRAL_USERNAME = '<YOUR ACCOUNT PHONE NUMBER>'
    RINGCENTRAL_PASSWORD = '<YOUR ACCOUNT PASSWORD>'
    RINGCENTRAL_EXTENSION = '<YOUR EXTENSION, PROBABLY "101">'

    rcsdk = SDK( RINGCENTRAL_CLIENTID, RINGCENTRAL_CLIENTSECRET, RINGCENTRAL_SERVER)
    platform = rcsdk.platform()
    platform.login(RINGCENTRAL_USERNAME, RINGCENTRAL_EXTENSION, RINGCENTRAL_PASSWORD)

    platform.post('/restapi/v1.0/account/~/extension/~/sms',
                  {
                      'from' : { 'phoneNumber': RINGCENTRAL_USERNAME },
                      'to'   : [ {'phoneNumber': RECIPIENT} ],
                      'text' : 'Hello World from Python'
                  })
    ```

    ### Run Your Code

    You are almost done. Now run your script.

    ```bash
    $ python sms.py
    ```

=== "PHP"

    ### Install RingCentral PHP SDK

    ```php
    $ curl -sS https://getcomposer.org/installer | php
    $ php composer.phar require ringcentral/ringcentral-php
    ```

    ### Create and Edit sms.php

    Create a file called `sms.php`. Be sure to edit the variables in ALL CAPS with your app and user credentials. Be sure to also set the recipient's phone number.

    ```php
    <?php
    require('vendor/autoload.php');

    $RECIPIENT = '<ENTER PHONE NUMBER>';

    $RINGCENTRAL_CLIENTID = '<ENTER CLIENT ID>';
    $RINGCENTRAL_CLIENTSECRET = '<ENTER CLIENT SECRET>';
    $RINGCENTRAL_SERVER = 'https://platform.devtest.ringcentral.com';

    $RINGCENTRAL_USERNAME = '<YOUR ACCOUNT PHONE NUMBER>';
    $RINGCENTRAL_PASSWORD = '<YOUR ACCOUNT PASSWORD>';
    $RINGCENTRAL_EXTENSION = '<YOUR EXTENSION, PROBABLY "101">';

    $rcsdk = new RingCentral\SDK\SDK($RINGCENTRAL_CLIENTID, $RINGCENTRAL_CLIENTSECRET, $RINGCENTRAL_SERVER);

    $platform = $rcsdk->platform();
    $platform->login($RINGCENTRAL_USERNAME, $RINGCENTRAL_EXTENSION, $RINGCENTRAL_PASSWORD);

    $resp = $platform->post('/account/~/extension/~/sms',
        array(
           'from' => array ('phoneNumber' => $RINGCENTRAL_USERNAME),
           'to' => array(array('phoneNumber' => $RECIPIENT)),
           'text' => 'Hello World from PHP'
         ));
    print_r ("SMS sent. Message status: " . $resp->json()->messageStatus);
    ?>
    ```

    ### Run Your Code

    You are almost done. Now run your script.

    ```bask
    $ php sms.php
    ```

=== "C#"

    ### Create a Visual Studio project

    * Choose Console Application .Net Core -> App
    * Select Target Framework .NET Core 2.1
    * Enter project name "Send_SMS"
    * Add NuGet package RingCentral.Net (1.2.1) SDK

    ### Edit the file Program.cs

    Be sure to edit the variables in ALL CAPS with your app and user credentials. Be sure to also set the recipient's phone number.

    ``` c#
    using System;
    using System.Threading.Tasks;
    using RingCentral;

    namespace Send_SMS
    {
        class Program
        {
            const string RECIPIENT = "<ENTER PHONE NUMBER>";
            const string RINGCENTRAL_CLIENTID = "<ENTER CLIENT ID>";
            const string RINGCENTRAL_CLIENTSECRET = "<ENTER CLIENT SECRET>";

            const string RINGCENTRAL_USERNAME = "<YOUR ACCOUNT PHONE NUMBER>";
            const string RINGCENTRAL_PASSWORD = "<YOUR ACCOUNT PASSWORD>";
            const string RINGCENTRAL_EXTENSION = "<YOUR EXTENSION, PROBABLY '101'>";

            static RestClient restClient;

            static void Main(string[] args)
            {
                restClient = new RestClient(RINGCENTRAL_CLIENTID, RINGCENTRAL_CLIENTSECRET, RINGCENTRAL_PRODUCTION);
                restClient.Authorize(RINGCENTRAL_USERNAME, RINGCENTRAL_EXTENSION, RINGCENTRAL_PASSWORD).Wait();
                send_sms().Wait();
            }
            static private async Task send_sms()
            {
                var parameters = new CreateSMSMessage();
                parameters.from = new MessageStoreCallerInfoRequest { phoneNumber = RINGCENTRAL_USERNAME };
                parameters.to = new MessageStoreCallerInfoRequest[] { new MessageStoreCallerInfoRequest { phoneNumber = RECIPIENT } };
                parameters.text = "Hello World from C#";

                var resp = await restClient.Restapi().Account().Extension().Sms().Post(parameters);
                Console.WriteLine("SMS sent. Message status: " + resp.messageStatus);
            }
        }
    }
    ```

    ### Run Your App

    You are almost done. Now run your app from Visual Studio.


=== "Java"

    ### Create a Java project (using Eclipse IDE)

    * Create a new Java project
    * Select the Gradle Project wizard
    * Enter project name "Send_SMS"
    * Open the <tt>build.gradle</tt> file and add the RingCentral Java SDK to the project as shown below:

        ```json
        dependencies {
            // ...
            compile 'com.ringcentral:ringcentral:1.0.0-beta10'
        }
        ```

    * Right-click the project in the Package Explorer and choose "Refresh Gradle Project" under the "Gradle" sub-menu

    ### Create a new Java Class

    Select "File -> New -> Class" to create a new Java class named "Send_SMS"

    ```java
    package Send_SMS;

    public class Send_SMS {

      public static void main(String[] args) {
        // TODO Auto-generated method stub

      }
    }
    ```

    ### Edit the file "Send_SMS.java".

    Be sure to edit the variables in ALL CAPS with your app and user credentials. Be sure to also set the recipient's phone number.

    ```java
    package Send_SMS;

    import java.io.IOException;

    import com.ringcentral.*;
    import com.ringcentral.definitions.*;


    public class Send_SMS {
        static String RECIPIENT_NUMBER = "<ENTER PHONE NUMBER>";

        static String RINGCENTRAL_CLIENTID = "<ENTER CLIENT ID>";
        static String RINGCENTRAL_CLIENTSECRET = "<ENTER CLIENT SECRET>";
        static String RINGCENTRAL_SERVER = "https://platform.devtest.ringcentral.com";

        static String RINGCENTRAL_USERNAME = "<YOUR ACCOUNT PHONE NUMBER>";
        static String RINGCENTRAL_PASSWORD = "<YOUR ACCOUNT PASSWORD>";
        static String RINGCENTRAL_EXTENSION = "<YOUR EXTENSION, PROBABLY '101'>";

        static RestClient restClient;
        public static void main(String[] args) {
            var obj = new Send_SMS();
            try {
              restClient = new RestClient(RINGCENTRAL_CLIENTID, RINGCENTRAL_CLIENTSECRET, RINGCENTRAL_SERVER);
              restClient.authorize(RINGCENTRAL_USERNAME, RINGCENTRAL_EXTENSION, RINGCENTRAL_PASSWORD);
              obj.send_sms()();
            } catch (RestException | IOException e) {
              e.printStackTrace();
            }
        }
        public static void send_sms() throws RestException, IOException {
            CreateSMSMessage postParameters = new CreateSMSMessage();
            postParameters.from = new MessageStoreCallerInfoRequest().phoneNumber(RINGCENTRAL_USERNAME);
            postParameters.to = new MessageStoreCallerInfoRequest[]{new MessageStoreCallerInfoRequest().phoneNumber(RECIPIENT_NUMBER)};
            postParameters.text = "Hello World from Java";

            var response = restClient.restapi().account().extension().sms().post(postParameters);
            System.out.println("SMS sent. Message status: " + response.messageStatus);
        }
    }
    ```

    ### Run Your App

    You are almost done. Now run your app from Eclipse.


=== "Ruby"

    ### Install RingCentral Ruby SDK

    ```bash
    $ gem install ringcentral-sdk
    ```

    ### Create and Edit sms.rb

    Create a file called `sms.rb`. Be sure to edit the variables in ALL CAPS with your app and user credentials. Be sure to also set the recipient's phone number.

    ```ruby
    require 'ringcentral'

    RECIPIENT = '<ENTER PHONE NUMBER>'

    RINGCENTRAL_CLIENTID = '<ENTER CLIENT ID>'
    RINGCENTRAL_CLIENTSECRET = '<ENTER CLIENT SECRET>'
    RINGCENTRAL_SERVER = 'https://platform.devtest.ringcentral.com'

    RINGCENTRAL_USERNAME = '<YOUR ACCOUNT PHONE NUMBER>'
    RINGCENTRAL_PASSWORD = '<YOUR ACCOUNT PASSWORD>'
    RINGCENTRAL_EXTENSION = '<YOUR EXTENSION, PROBABLY "101">'

    rc = RingCentral.new(RINGCENTRAL_CLIENTID, RINGCENTRAL_CLIENTSECRET, RINGCENTRAL_SERVER)
    rc.authorize(username: RINGCENTRAL_USERNAME, extension: RINGCENTRAL_EXTENSION, password: RINGCENTRAL_PASSWORD)

    resp = rc.post('/restapi/v1.0/account/~/extension/~/sms', payload: {
        to: [{phoneNumber: RECIPIENT}],
        from: {phoneNumber: RINGCENTRAL_USERNAME},
        text: 'Hello World from Ruby'
    })

    puts "SMS sent. Message status: " + resp.body['messageStatus']
    ```

    ### Run Your Code

    You are almost done. Now run your script.

    ```bash
    $ ruby sms.rb
    ```


## Need Help?

Having difficulty? Feeling frustrated? Receiving an error you don't understand? Our community is here to help and may already have found an answer. Search our community forums, and if you don't find an answer please ask!

<a target="_new" href="https://forums.developers.ringcentral.com/search.html?c=11&includeChildren=false&f=&type=question+OR+kbentry+OR+answer+OR+topic&redirect=search%2Fsearch&sort=relevance&q=sms">Search the forums &raquo;</a>

## What's Next?

When you have successfully made your first API call, it is time to take your next step towards building a more robust RingCentral application. 

<a class="btn btn-success btn-lg" href="../../../basics/your-first-steps/">Take your next step &raquo;</a>
