# Sending Custom Payload In DialogFlow
Sending a Custom Payload written in JSON format in a JS file, i faced this when i was using  I’m using the Firebase inline editor/Firebase CTL. After this you will be able to send RichMessages and more right from the Index.js file.

Just follow this small tutorial to get it to work:

#1- In your ```Index.js``` search for ``` ‘default’: () => {``` after this down bellow you will find ```“responseToUser ```, just a little bellow uncomment this line ```“data: richResponsesV1,”```... now you are able to start sending richmessages for both ```Slack``` and ```FB Messenger```

#2- Now back to your action handler and add this example for ```websearch``` i made, i will explain things in the code:
```Node.js
 'web.search': () => {
      // Use the Actions on Google lib to respond to Google requests; for other requests use JSON
      var a = parameters['searchfor']; //This is a method to replace all the spaces to '-', because links cant have spaces
      var r = a.replace(/ /g,"-");
      // This is a rich message format sent to google assistant this format is exclusive to google assistant, not our problem here
     if (requestSource === googleAssistantRequest) {
        if (app.hasSurfaceCapability(app.SurfaceCapabilities.SCREEN_OUTPUT)) {
   app.ask(app.buildRichResponse()
     // Create a basic card and add it to the rich response
             .addSimpleResponse('Here is what i found about '+parameters['searchfor']+'')
             .addBasicCard(app.buildBasicCard('Here is what i found about '+parameters['searchfor']+'')
             .setTitle(''+parameters['searchfor']+'')
             .addButton('Press Here...', 'https://www.google.com.eg/search?source=hp&ei=FGOYWo-NFIvxUvH7sfgE&q='+parameters['searchfor']+'')
             )
         );
 } // Send Google response to user
     } else {
     // NOW HERE IS WHERE I WANT YOU
     // as you can see i got the rich message format in JSON and made a const for it
     const websearch = {
          'facebook': {
            'attachment': {
              'type': 'template',
              'payload': {
                'template_type': 'button',
                'text':'Here is what i found about '+parameters['searchfor']+' ',
                    'buttons': [
                      {
                        'type': 'web_url',
                        'url': 'https://www.google.com.eg/search?source=hp&ei=FGOYWo-NFIvxUvH7sfgE&q='+r+'',
                        'title': 'Open Link...'
                      }
                    ]
                  }
              }
            }
          }
          //and here is the trick you make a let to a new variable to handle the previous JSON Message as a data so he won't mix it up with text or speech
    let responseToUser = {
          data: websearch, 
        };
        sendResponse(responseToUser); //Finally just send it as a response normally, and you have your rich message right there..
     }
     
    }
```

#3[Optional]- If you want to craft more rich messages formats for FB Messenger you can use this link to see some Example for some JSON formats: https://developers.facebook.com/docs/messenger-platform/send-messages/templates 
