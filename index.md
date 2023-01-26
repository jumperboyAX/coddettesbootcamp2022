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






### 10.3 installing cura 

So in this sesion we installed cura so that we can learn to 3d print on our own 

![](https://lh4.googleusercontent.com/wVJnvqL30hnauS0Asn4EfdeQFXYeo0wlpDGcOypVgBcCZ2fOY6CAIsYujo_Kb281G_zDSWzYAuPb3DVCRJqGXEpNU_efghNqdQZ3qaXT0OEGnJ8LHu_VPPD9EydCgrdV4awimEX31jJpyrhJbp5eBA)

 So this is cura it is used for 3D printing

This is the download link

<https://www.filecroco.com/download-cura/>

How to get G code to 3D print 

![](https://lh6.googleusercontent.com/FAXTKJRVSo5TmFE5yjOl_9HffX0Sikn9PFlvp-HzEPjG_kRBP-ldRG6jO7JZuL53h8Ttyen0OVvUFO2dNIMZg5--Yy_NVNX_9l1IPCm73b09jmYRSPvAbGODPDs6afGf-vLexdBzWvpOZuSU1NXRtUg)

 So on thinker cad get the thing you want to 3D print 

And then you press export

![](https://lh4.googleusercontent.com/Hg72hcLxaMCg0p0r1lxYCLZ6Z7PwvbnyhuTHTtJEVTzO4PDLFDOQ3bnzuERa1zGMVQnYVGBzZuD53OFZiyXPqfxS0yK-VoVWvcfTw1Z_QcnIH7ehNXrkp_PR00WGGibksqQelbR7ZBvnSq6IUtEPOoQ)

And then you press stl that will make you print into a stl file

Then go on cura 

![](https://lh6.googleusercontent.com/8jA7GI3ns5APsdATb6exGfvZw8NlQhsy1xD3neT4glqWZvDsnifcHWDXD414fEyEXAlTWzaCLCnN0nyzH1x2ZRW76RtbbSpU6SanYXCLt1I-boH2hMU5MMnTJH5BlYMV198UWuzXDWGnMy1Bj3d57KY)

And press the file icon in the left corner

![](https://lh5.googleusercontent.com/CmTKvUv5Gt7Rq34-asH8AvPdkPBVig5K33EFDtyWHaGskAsj6gAXk6tDz0JSZ4rqrmJCnDpZQ_3I6Z9Jq3-tIcoDVIgrctigerkuMhFtmT-RlMCyxRRLPpeSHkNUjqNn7pTpc3M_j_mV2mQwKfjVN4o)

And then find you file you want to 3D print and open it

![](https://lh6.googleusercontent.com/5-n6NYHdtAc5mdMlJlUyUXcDGGDmD0y0RK6olFJdoernqUq9VC3Eq4C710ct0DjFgfQ-QsojGfyx9y-QAtOEMyZogkCv2BErwlLk1gV_kDMf8qBiD8ibkQV_Ww1MM2j9jB0wr4iDV3drOJUr3N1uuuk)

Something like this shoul show op and the press save to disk and that will make your thing in to G code wich you can insert into a 3D printer


### 10.4 learning about business model canvasses 

So what is a business model canvas

 The Business Model Canvas is a [strategic management](https://en.wikipedia.org/wiki/Strategic_management) template used for developing new [business models](https://en.wikipedia.org/wiki/Business_model) and documenting existing ones.[[2]](https://en.wikipedia.org/wiki/Business_Model_Canvas#cite_note-2)[[3]](https://en.wikipedia.org/wiki/Business_Model_Canvas#cite_note-3) It offers a visual chart with elements describing a firm's or product's [value proposition](https://en.wikipedia.org/wiki/Value_proposition), infrastructure, customers, and finances,[[1]](https://en.wikipedia.org/wiki/Business_Model_Canvas#cite_note-Osterwalder2010-1) assisting businesses to align their activities by illustrating potential trade-offs.

Canvas was initially proposed in 2005 by [Alexander Osterwalder](https://en.wikipedia.org/wiki/Alexander_Osterwalder)

Customer relationship

The first thing we learned is how to have a good customer relationship.

 So to have a good customer relationship you cannot rely on price.

Because if they find someone cheaper they will buy their product instead.

So what you need to do is something your competitors are not doing

Or if it has something to do with tech you have to make an echo system 

Let's take appel for an example, so we all know that apple is one of the biggest maybe the biggest but it's not just their good products it's their echo system. Every product works so well together, and they made it so that it works really bad with any other phone like a Samsung or something else. So that is why most people will not switch to a Samsung because it will make their life harder or they will need to replace almost every apple product they have.

Value proposition

And we also learned about value proposition 

So what is value proposition is basically what do you do that is different then other people 

Like what is special about you that people are gonna buy from you and continue to buy from you

Customer segments 

 We also learned about customer segments 

And what that means is what is the group of customers you are targeting or more like for who is your product made and how big that market is

Channels

Another thing we learned about channels

And that is how are you going to advertise your product or service

Key partners

So this is just your list of partners but we learned that it's better to have more than 1 business partner because if one of them does not want to work with you anymore then you will still have other partners to do business with.




Key resources

So this is for all the resources you need for your product and

Cost structure

So this is for all of the costs that you need to pay like business partners 

Advertisements, resources stuff like that

Revenue streams

So this is your sales and your profit so after all the cost have been subtracted

This is an example of a business model canvas 

![](https://lh6.googleusercontent.com/O0-ZaDIYH5ZVHVD7WAnKuwvWmHehVFIofr5yne3c3YAAOI7sjdERLti8JByTcehrjGrARk8YhmzZ3cMcvAhMAtxIk_LkW8sTmFBSghMoMZ-rUyKnJEB0N1zluQR_JPv3LWaFx8uDMp4rVz5WcS046bY)

### 10.5 installing freecad

 So after downloading the file ,

You should see this 

![](https://lh3.googleusercontent.com/PPIO2mCoYmXOHG9u7dLXdZZjh2Fqb0BMUl7gJ5_mX6aKy8jBnCrfViLLjdDPnRTqWg536UGwFihJhF2Q1kndSQ3KXA-XPZXA6HQTO5JLCZqbHSq6v45iysPoWSrnkuvLgpJrJ-uEIc2KuNXf2pj2rsN1dwALaX3ENUsWYd7lrej55H4SR_bqaM7ISOWbQw)

The you press on next

After you press on next you should see this ![](https://lh6.googleusercontent.com/mwrecyQ6Bm7lMTgXB_7MKinrrJ_WRUCdI92Y6eO5n4_VdHkQiRgDAnGaCpCni-Fko6SoUAzC6E-LZDPtRG47XBQ0z6Ar5_yziFUsbkD6c6w8hw8uIWmBNspK-d8gJu0KGfdpVtjQVUp_WZaOdDxDK9k65mR44d-E8aMyhHpxAtcdgtGrlbl1Vu9BbLk-4A)

Then press on next

And you should see this 

![](https://lh3.googleusercontent.com/W1HkRJpuCceWUr3E2Fk33xBuWlC2ASUrZktcMwg6sv4uHnDVrMzuTbHPQAeP5o8W79za2_XGKMdXnWQmRZTstdkJImheElxTfFv4w1XCm5lxgqhsAraZkU9iBvKLnW2W38tXVoVotmrIpQOXRzUpKG9klPDcDXpm4Ca2dTO6Aigpq2wEd2mvcz40tHheZQ)

You can choose wich one is for you

And press on next and your good 

 And this is the download link

[freeCad downoad link](https://drive.google.com/file/d/1UYIprnmoOGLFf4dSsa_ORXcL45E78jNX/view)

### 10.5.2 using freeCad

![](https://lh5.googleusercontent.com/7Z_fRy9k3t-V1bKzT0O3kLgzuwzYp2EyodgVegbWMzW2ig05MsDxy6XFwY4ErJUY37pXP9WjY36sR8GaxdiRUmIXAyI6SC4gITrkm76c466IotMZ_nElSdiIDa4VUYUO3M1alBCuVTP9SZ9R4xoZS87oueTFTAAdfbvl8AqbZfVycJM0TTlrYFz46MJXKg)

Press on creat new

You should see this 

![](https://lh4.googleusercontent.com/3wbf_1fi9F64czcOpiY7jQsnkUFZKZtf0dd86J_h_x30F4UVKzeETuV3vLV_oMkqhwQezxRZAwZqNRTRuUsC_81vXDnWwvjDRHbvQ1G-cUNn8htenYebXVgsypCmm-WunQ6tA_ZNp7pMYJ2_8ZKVlvTb0flwyXoZwR2gFt8wPUOkMnOvbQJA5OB625EFhg)

And the press on start

![](https://lh5.googleusercontent.com/mpMY0mO60spcbDbKRJYLGliYHtQd7qwEtA2fG-JJMV9cJB99sG0QYdR9XPeNDZ5UjU4Zpk-c6kxQ1CuADqK2vXenYVKWmP5Zp2RkhOcKU2d7AXvM-Miv7YC1LsANmhdCB2qr1R7XWz7E1Z4xwn0QOb_Ag631dKpxPYImQbxRpfZq6BtHPQDuJ8RUxibEYA)

Then press in part design

![](https://lh4.googleusercontent.com/J77myEHpdLbrOj1pRjiUzksH3MmVJEU-OPdnPxLsw5VIamWw0IXfop4GP-uazKPEdrGmMsre0sN-ekKiy-0-VfnY0BaY3K4GezLjxQHOZbrA4cVUvaUxjAfx5Fig7CQJ7PF2ed-zqPikAH7bWhzqq0LRQ7vDno40_CZVqWJLsnuOe6yXNbAmfTFKMPa0vA) and then press on create new sketch

![](https://lh3.googleusercontent.com/5hFfyqTS7Cgmxst7753GMlmMfSzjJm0xv-9Os-avRH_8UpRtWN5g2FyqWJ9ewcSMKc5d27jpKCeelccUds4MBOU1R86fc8kA0BBNMrpkoeZsULG5n-H1_ani4Jl40yFgu1TNWS4r9x0hbv5TLq1BOD9ILydmkEB34uY-yIV3qV2stmO0oJqYIG2XScFYFg)

Then press on XY plan

You should see this ![](https://lh4.googleusercontent.com/ikHjU2vBiEFaRpJJh3Z7-Cr2WdTVfyIebF1ypnREhPT9m_Pb4E-1eXIO8Ko_NIxr1a7s2pkiqqW7vxJFcGd__jRasBziEslGNJWO7-clbsZQUnk2ejF_Y07b9TzSWgGYT6Q_1xOuvUIVUKu0z0M_XlhnbiUzB7fykb0l80sN0H4PYrcTPdiuSl7-_FS86Q)

The press on the square

![](https://lh6.googleusercontent.com/f00YyQmqvPhmq7C3iSzHI81DW_03KZOUyH7yOFqMu_KUPCUUp_f21FmYZwEzsELfD-yflI9MJoLva3CHYroJhhBUhk4n3fwjczMpqPx9MTmFAhblCpde_fJW3Tj7smoXLjxB1oNroVudMk0GjStFZOJdYFDDovHEMDFIvOCffnCodv1PD5VV1ZkEhWTL6A)

Press the dot in the middel of the the screen

And make a square 

![](https://lh4.googleusercontent.com/geROBI9De757L7RlkJxC2qzcb9vP3c3wSJvCufrMWjpyzh9Oft07W_z6-8UpxtmqQNjYdgCnvEG3yV8JMhUujX2CH7uESjsvmD4XIb5MxVxLZYshGko5_miSvTkzQ06BYbaCiKgdiakaDRi_SRdOWQq6bIz6wJ9hkUrHTCA6h5viNYLP6Hsg8J3NoN5fRw)

And then you have to press on constraint horizontal distance

![](https://lh6.googleusercontent.com/q-uye5wPRo8uhXWEB8U0un7zQFT1MDUcw17JsyVmJLOBZg-1ARIjizaGOQhF0CKzlG3MJ7UOxI6-g2Y0u7Br37lynB2Qb6vF0gmVe64teaaCTa6w7AdzxGSfPxW7XNmr65CBy90WypAyJXf-0uDU-3hgAIdjLyqUUsEcRYzQp1jmIPmMDJ6yFKjPCFF4Ng)

And the you have to press on the top of the squere 

![](https://lh4.googleusercontent.com/pAb7S_4l8QtehD8JmeaK4SNmN94_l3P-7l0bnMNaZtXIv3Phg1SqJf0l4o-kdz4ghcPAi6Q7DgmiK8XfshtCGBDdwWH3rEEo6RwS-QUMfv0iWi1P0pI1bRRid46EqiCFGCXVuVcd273Sdlyb1lf1bCjb0NBJueEUIuTqktz9ck5Ow3zbmyNflA9BsWmY5Q)

Set the distance to 50mm

Then press on constrain vertical distance ![](https://lh6.googleusercontent.com/vnBi35_coNgcmQoyqOhkAQnzMxx1tPlU0yvldUd5gfP4aeMEykHwfKQt4Srk_jPSdeGsAubcJZm-qqExOaC6njX8MMpju3j7hDe-PEO_8_pVdrC-bZXmiVri4sBSZRdK9JxaEHhvvg9iCOWkai7Wr7KZuLQLHFUX9F1742ZskNzr0OwhrRqyJQcfctpXbQ)

Then set the distace to 50mm 

![](https://lh4.googleusercontent.com/2rhHdmxSltGueEsB4yjoTiBoNyOf2HZ4fED9txuvT3srwxqVvUOp7TRgovksgK5nVk53knMosDQKsfuJl67MGUfhGY_24jBsqDHwr52P5bxVMTcguDFDSvYtIL2Koc_fOvvEE83qjrOzoqGy8Mzu-c5PIeTR4V5yGTDryHpoinaIPKrK6_7ZqQi729q3BA)

Then press on close 

![](https://lh4.googleusercontent.com/Klu-f0mGaOjoe1Z1KG0JDlHooGsBTxY6Vg40h4vULZBzsc0XCwy0ebtanyVYD0z536rI7dVPcME8ydQgCkvB6GG0bynDiANKGjdqMU4SNpmwJ5shA70sn_PIWXEnFdPrANe0GIz4s2RtyNqH7uypUIrQ_QLglS7Q7n1v6SRp2cqc0sOgUi_vGu8RpLYb4w)

Then press on close 

![](https://lh4.googleusercontent.com/uz1anel46Ta6hiISMHcaW1y3h2ITQ04q5OqTSK5WRa_wZFVnoyyhRnCtu-_bfoX71BqCnTUmX3PlGtfvDfcsMcGP96g0rg4H5nhnOgprgap0d8X61ZyueWXzBeNRdK76SIs5dg_7nunwAmSsmgCyFTpCrnBPt0Pg3uafT3HiAN3SfPi5uKnxJT88emiQ5Q)

The press on pad 

![](https://lh5.googleusercontent.com/q9bHy1v6lppcpfxfAOXRnfeKBtAmrPADut79zsuEb9dizAE9F3tFlo7w8jn1f-QZbnl-PTCA1Q4m2ctViTDOwfZWqeeuJumaYtMaWlpbJPnZplBmj9pCeRLyhZYrY-N92-OzCu5uq4k_IQIhntkkyfetoyM5Zxdnm3wydhIwDalziFQcDsF8tTF1Fhs-0w)

Then press on creat new sketch 

![](https://lh4.googleusercontent.com/MkpPRUB_W9h82yzJDvahVTSdBp0zuqQw4nJyO5isJhn-Nm9rttgeO80VPiUtteJYWA0UuU_pnHivLRop8EKPwZg_4nXDa08gGOgGYoYJEP6ugWuLYK-LHPKvjPBSk3Nr6YHrQn_hcYNUIytnjLbHtjOL6O-HcsWSTUq7FwaJ9RHJ15RIF-6cKoKqGi4zxQ)

Then press on XY plane 

![](https://lh6.googleusercontent.com/BflCvNUwHWVJ-QGWU-_Xzx6MTp_t50mkeMxLNWpThIwkpIn0ZB4Zn7Z6Pri8lQ0XlkE0NrLKoomkdkRA0pjx7Mu8jDdEardwTbyiNrLNDMcZFJreKMfmDfbuHnCMmRI-kwXF30GxD6nuFNadYiQQtTqQzRxsGS6sM42XF5oHA31CSF5rg7qF_zuV1QGZtA)

Then press on creat circle 

And make a circle 

It will look invisible but its there

![](https://lh6.googleusercontent.com/bSL6qOGNcZFlKmnmKwq8iKfKVGGVlOFX3a6Yt6BTGs2ZG80vFm13Ee5uF-gCW44ti8u-A1DZE3BojJKa0o43pqSWzU5ExUJuz29Wm4ja540sZIq02qjW25UGzJUpiHZlcMXSozizKffLGqyr2vaREefrlNUQ8bCum1iJVT5c61zNOjA86zLt5CmXPlcv0Q)

The press close 

![](https://lh3.googleusercontent.com/Ea6RFMWABfdPE-OXkR17DYR2rJqVHIf3QvAymFa5Ps4RSVvx0G9NVlkmJkG20YNmcpT8TUzvbTuQjY8cPA3dejWNxCuSngTQPxNH3bBZaGcDkwOnWNaQRyRTQSUBQO_HtR2kZxRdoONTYJIItGrqpXJCcw-HpwPOoAHj43WbalKXjxd6_Ykogu2f96QBEg)

Press on the arrows in the right corner

Until it shows the circle 

![](https://lh4.googleusercontent.com/6bMnmU76iqer9_NIHoZLUxwLT57JhxoIgLnMECZwvwYHimYqi2_vuYuCZ59WfAYX4ZuYrS7cnxHGZBZx3DLFXw67QC0KvLJ0HTP7-wd0Gs9_iKK-LxOANhFK0Toe3Jl1QbUiUeDrF0LlhUvUu8Y2GEIv_E_I3uPMJnGgiU5FNvwkEem9xIIm21Y-iAcsXw)

Press on the pocket tool

![](https://lh6.googleusercontent.com/1JhBsDqP7DLnWAsK2VCh4n4njrjfJXZYQE6KYUuvBF9NtGFcPBfhJI_48ls4I0aXxo76y33UgDVQ_M391Ewla-ffPk2lxc0Lpn0hvTjFMHPsOajoSo6UXsWGk9OTuB3YUkOInpxPZb9yuEY6aNADVZS6O5fA-AmaIlG1YBnriCnmG910hygIcmYKwqoswA)

And press on the circle and then press ok

![](https://lh4.googleusercontent.com/vl-Pm2EDdm3EdxpkYWDSU1Pt99rP9Nx5GwUxsM0FmhJzJwK-17-xloc3QqzX-do7Ai5pFinsL3YL6D3IxC7hk4tL8vkm8SeQZqbU-woBMakVViCQTRD2I2l0AXzje86oo6hg89D_v8fUXGyWr4EPviTEkTXHclGJwezfOGPtEKYmEdaxHgKMPeMm7hGZWg)

And you should have a pocket

###  10.6 cnc

So we leard about cnc and cnc is and what is cnc

CNC stands for Computer Numerical Control; this is simply a method for automating the control of machine tools. Computer numerical control machines rely on computer instructions and a CAM (computer-aided manufacturing) program to control, automate and monitor machine tools' movement to create the desired part

### Esp32 cam set up/settings

### How to connect esp32 cam to ftdi 

![](https://lh5.googleusercontent.com/Pp9lFjPtc4fp9ndTKenUi_-SLWllGHvUGxiPtX3He9ZSTlRxhYkW9Gyz2o3swFr5h0KmVHlkRKOFdHFdR_u7xWx81rL0PPSwHlX-TxWmjZmlOV6JAZOODbPKP9yYsZ2V3N4kCLytKUBPgal2S5Kh9I8aT99-v2MyHp7oGlNwO32eA4F4PeOcLCXRVmyP5g)

Setting for Arduino ide

Arduino board software = ai thinker

Additional notes = ground and power must be on the same side on the esp 32 cam

