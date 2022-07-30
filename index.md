## Welcome to Axl website 
10\. Interface & Application Programming

10.1 Objectives
---------------

Wk 1

-   [x] Setup Dev Environment for ESP32 S2

-   [ ] Setup NodeJS Dev Environment on your PC

-   [ ] Explain the HackOmation quadrant in relation to your final project.

-   [ ] Build UI mockups for your FInal Project and HTML Layout

Wk 2

-   [ ] Build HTML5 Chat app\
    * Draw mockup / layout\
    [x] frame and add id's to <div>'s\
    [x] Style the page and\
    [x] wire up the JS code and understand

Wk 3

-   [ ] Build Chat app back-end NodeJS\
    Build NodeJS server side to:\
    host your ChatApp (Express static HTML)\
    * Build / test API endpoints (for: users & messages)

Wk 4

-   [ ] Setup MongoDB datastore & connect via NodeJS\
    Setup MongoDB datastore + mongoose ODM (Object-Document-Manager)\
    Store and recall message data using an API (ex. request top 100 msg)\
    Wire up MongoDB to API endpoints\
    Update app-flow to use back-end for Users and "old" messages

Wk 5

-   [ ] Create data-bound widgets to display sensor data\
     On ESP32 add MQTT client + ArduinoJSON\
     Send Sensor data to MQTT server (as a JSON object)\
     Create a DataCard, a Gauge and a time Chart widget on Dashboard (use chat app)\
     Strategy on DataBinding and Widget updating (Last updated)\
    * User Login/Pw (state persistence)

-   [ ] Add Screenshots and description of the process of creation. 

-   [ ] Describe the design & programming steps

-   [ ] Screenshots or video of your Prototype/app working

-   [ ] Describe any errors or problems with the process and how you fixed them. 

-   [ ] Include all the files you created folder

### 10.2 building html chatapp

I made the mistake of not drawing a mockup but it is something that is very important

### First iteration

![](https://lh6.googleusercontent.com/Ifai8vXPkyKOGDDklW57zlNaNv1uZggSWJsJ4Th3zArl28lFnfRSPkLeznWtjT8gIigDhdGlJtOu556dvpPRsosjtHV7bSECuTp-4RLNZu_PVUnPyYeLl2wqXP-HNmbmtnmpixt4WTNrtqwJV5xqRx8)

This is the first iteration of my chat app it is pretty basic the problem with my first iteration is that the messages had no scroll feature so the message box would over flow

This is the index.html


```

<!DOCTYPE html>

<html  lang="en"  style="background-color: rgb(255, 122, 122);"></html>

  <head>

    <meta  charset="UTF-8"  />

    <title>axl chat app</title>

    <link  rel="stylesheet"  type="text/css"  href="style.css"  />

    <script  src="https://cdnjs.cloudflare.com/ajax/libs/mqtt/4.3.7/mqtt.min.js"  type="text/javascript"></script>

    <script  src="app.js"  type="text/javascript"></script>

  </head>

  <body>

    <div  class="chatbox"  id="chatlog"></div>

    <input  class="input"  type="text"  id="chatInput">

    <button

        onclick="sendMessageButton(getElementById('chatInput').value); getElementById('chatInput').value='';">

        </button>

        <div  id="userlist"></div>

        <h1><i>hello welcome to my website</i></h1>\
     
```

So what is happening here is the were linking the style.css and the script.js in the html code so our code can be nice and neat and were making were making a div to search a class named chatbox with the id chatlog 

And a div for the users

Second interaction 
-------------------

![](https://lh5.googleusercontent.com/ym7T8QoRPFXhFh1AJ3B4CqL3du4er1fsOIGpS1ILgYgSSWbOsolH5nBI8ExpuzZMBCoHuPxZjRh-4vUZkPlmANXD_HHc0wEftLPjKuNPM6o-_uDyCj6KQW_C7c8RxoQLODIhq8Q_E8wLhujJD4sY0Wc)

This one has a lot of improvements like i have a scroll option for the messages 

And I can press enter to send messages .

This is a piece index.html code

     
 ```

 <link  rel="stylesheet"  type="text/css"  href="style.css"  />

    <script  src="https://cdnjs.cloudflare.com/ajax/libs/mqtt/4.3.7/mqtt.min.js"  type="text/javascript"></script>

    <script  src="appv3.js"  type="text/javascript"></script>

  </head>

  <body>

    <div  class="container"  id="chatlog"  ></div>

   <div  id="userlist"></div>

<input  id ="chatInput"  type="text"  placeholder="Type a message"  onkeydown="sendMsg(this)"/>

    <button

        onclick="sendMessageButton(getElementById('chatInput').value); getElementById('chatInput').value='';">

        </button>

        <h1><i>hello<sub></sub> welcome to my website</i></h1>

  </body>

</html>

 ```

So here were linking the style.js and the script.js

And were making a div of the class container and the id chat log

Another div with the id userlist which basically makes a userlist 

And a input were you can press enter to send you're messages

This is a piece of style.css code

 


```
.chatbox{

background-color: rgb(245, 236, 236);

float: right;

width: 500px;

height:500px;

}

}

.container {

    margin: 60px auto;

    background: #e2dddd;

    padding: 0;

    border-radius: 7px;

    height: 240px;

    overflow: auto;

    display: flex;

    flex-direction: column-reverse;

  }

 .userlist {

    margin: 60px auto;

    background: #red;

    padding: 0;

    border-radius: 7px;

    height: 240px;

    overflow: auto;

    display: flex;

    flex-direction: column-reverse;

  }


  ```

So here we are styling the page like the message box and also the scroll feature

This are piece of the script.js code 

```

  function sendPing(usr = '') {

      // ping sends out a message to all () or any specific user to respond if ur there

      var pingObj = { ping: usr }; // JS Object {ping : "usr"} -> JSON {/"ping/":/"usr/"}

      client.publish(mqttTopic, JSON.stringify(pingObj));

    }

    function sendPong() {

      // sends clientID and UserName in a JSON object (and whatever u need more)

      var pongObj = { pong: { userName: userName, clientId: clientId } };

      client.publish(mqttTopic, JSON.stringify(pongObj));

      console.log(JSON.stringify(pongObj));

    }

    function redrawUserList() {

      // Generate the userlist HTML

      var ulist = "      userList.forEach(function (item) {

        //var x = arrayItem.prop1 + 2;

 ```

So what is basically  happening hier is when we log on to the website it send a pong  and if another person logs on they send a ping and it will show in the userlist function and function redrawUserlist means that if i don't receive a ping from someone they will not appear in the uselist

10.2 
-----

So this is the node js server it basically jst my chatapp but it is hosted on a local server

![](https://lh6.googleusercontent.com/h4qJl0V-dUwOXWuhktpGAg9e7xpZKLDMiEy3_iFEEHejRFJDidVqQfIolbOy_u7H5EGIFdNpp3SxdVFT7hwKhYNTrz_Aegl3JT0oko8lAZhhp0TID1Y31A2YFeeghEyKRLVfaJG2TIA3nC5kvgmKYas)

This is the cmd for running the chat server

![](https://lh5.googleusercontent.com/sWP29TsK-VZApAHr7DWl0I8VcS5Nor89FsdJNlFGKjbi8ob-kolgxWIBQeA-XKVUaEgIia50w7CbOTSPrkBeqym4HZ_C6D9sbC2exw33-znyRS_5cv58TMS1SAC8Rh9_Tdl_MOLuXR7nqItawth74ZY)

This is the chat server it is basically the same a  chat app
