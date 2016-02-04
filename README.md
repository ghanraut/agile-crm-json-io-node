Webhooks integration guide
===================================

Webhooks facilitates communication with third-party applications by sending instant web notifications every time an event occurs in Agile CRM. By letting you register a URL that we will notify anytime an event happens in your Agile CRM. When the event occurs—for example, when a successful contact is created , Agile CRM creates an Event object. This object contains all the relevant information about what just happened, including the type of event and the data associated with that event. Agile CRM then sends the Event object to any URLs in your account's webhooks settings via an HTTP POST request. 

 - **Agile CRM web hooks is located at** - Admin Settings > Webhooks
 
![alt text](https://raw.githubusercontent.com/agilecrm/agile-crm-json-io-node/master/Screenshots/jsonio_dialog.png "JSONIO node")

 - **List of Fields in Webhook** 
 
|Field Name|Description|Data Type|
|:-----|:------|:--------------|
|Notify URL|Specify the REST API URL of the third-party application.|URL|
|Module|Contact and Deal|Check Box|
|Trigger Event|Contact Created,Contact Updated, Deal Created, Deal Updated|Json Data|

 - **Params**     
‘Key-Value’ pairs which are appended as Query Params to URL later. More params can be added using ’Add’ button.    
At least one Key-Value param should be present. Each param can be edited or deleted. Any Agile Contact Merge Field like {{email}} can be added as Value


Output:
--------

The output from the URL (JSON format) are stored as user parameters and are available in the rest of the workflow. 

For an example response such as {"firstName": "John", "lastName": "Smith"} You can use {{firstName}} in your workflow as merge fields anywhere.


Example:
--------

Here is a quick example (weather) to illustrate JSON IO usage:

The following REST URL fetches weather data of London city http://api.openweathermap.org/data/2.5/weather?q=London&lat=42.98&lon=-81.23

We follow the simple below steps in JSON IO node to get weather data:

- **URL** -  http://api.openweathermap.org/data/2.5/weather (without query params).

- **Method type** - GET

-  **Params:**

    <table>
      <tr>
        <th>Key</th>
        <th>Value</th>
      </tr>
      <tr>
        <td>q</td>
        <td>London</td>
      </tr>
      <tr>
        <td>lat</td>
        <td>42.98</td>
      </tr>
      <tr>
        <td>lon</td>
        <td>-81.23</td>
      </tr>
    </table>

**Note:**    

URL with query params is also supported. But at least one param under ‘Params’ is mandatory. For URL which do not take any parameters, you can send a dummy param.

Node Exit paths
----------------

The node has two exit paths - Success & Failure
                                  
  ![alt text](https://raw.githubusercontent.com/agilecrm/agile-crm-json-io-node/master/Screenshots/jsonio_branches.png  "Exit paths")                           

If URL request was Successful (a valid JSON response for GET or a successful POST) , then it goes through ‘success’ path, and takes the ‘failure’ path otherwise.

Using Response Data
-------------------

The response JSON data can be used in subsequent nodes in the campaign using merge field.    
For example, if the response data from the above request is as below, this data can be accessed using merge fields like {{name}}, {{main.humidity}}
<pre>
{
    "dt": 1399279349,
    "id": 6058560,
    "name": "London",
    "cod": 200,
    "coord": {
        "lon": -81.23,
        "lat": 42.98
    },
    "weather": [			        {
            "id": 802,
            "main": "Clouds",
            "description": "scattered clouds",
            "icon": "03n"
        }
    ],
    "base": "cmc stations",
    "main": {
        "temp": 274.05,
        "humidity": 83,
        "pressure": 1012,
        "temp_min": 273.15,
        "temp_max": 275.15
    },
    "wind": {
        "speed": 1.76,
        "deg": 326.505
    },
    "clouds": {
        "all": 36
    }
}
</pre>

**Using response data in ‘Condition’ node**

 
![alt text](https://raw.githubusercontent.com/agilecrm/agile-crm-json-io-node/master/Screenshots/jsonio_condition.png "Condition node")


**Using response data in ‘Send Email’ node**


 ![alt text](https://raw.githubusercontent.com/agilecrm/agile-crm-json-io-node/master/Screenshots/jsonio_sendemail.png "Send Email")


Mustache
---------

Agile supports advanced Mustache templates. You can use Mustache syntax in your email content or other places.     
Refer - http://mustache.github.io/mustache.5.html for conditions or loops.

You can test your mustache template code using this tool - http://trymustache.com/
