# README

 In this project I have two  ``` chatbots ``` one was made using ```facebook chatbot``` and the other one was made using ``` dialogflow ``` from api.ai

## Facebook chatbot
---
To use the facebook chatbot your app will have to be in production so using ```ngrok``` as a tool for testing your app will be very helpful.
[Click here to download ngrok](https://ngrok.com/)

### Configuring the webhook
Asuming that you ```created an app``` and add the ```messenger product``` to it then you will have to configure your ```webhook```.
1. First add (if you don´t have it) the webhook product to your app.
2. Then Click on the ```webhooks``` product and where it says ```Edit Subscriptions``` click on it.
3. While your ```ngrok``` program is running in a terminal (by typing ```ngrok http 3000```), copy and paste the ```https``` link that shows you on the terminal in the form that appears you when you click ```Edit Subscriptions```.
4. Then where it says ```verify token``` write anything you want.
5. By verifying and saving facebook will send a post request to your app

This piece of code will render your params if the verify_token is the same as your token

```
def index
  if params["hub.verify_token"] == "your_token"
    render text: params["hub.challenge"]
  end
end
```

### Add your access token
In the callback controller in the url method add the ```access``` token from your facebook developers account in the Messenger product, choosing your facebook page will generate the access token

```
class CallbackController < ApplicationController
  def url
    "https://graph.facebook.com/v2.6/me/messages?access_token=your_token"
  end
end

```

## Dialogflow chatbot
---
Add ```gem 'api-ai-ruby'``` in your Gemfile then ```bundle install```

### Add your client access token
In the send message method from the messages controller add your client access token from your Dialogflow account by clicking the settings icon from your ```agent```

```
def send_message
  @client = ApiAiRuby::Client.new(
      :client_access_token => 'your_token' #Here put yout client access token from your API.ai account
  )
  @text = params[:message]
  @response = @client.text_request("#{@text}")
  puts @message = @response[:result][:fulfillment][:speech]
end
```

And there you go, you have your chatbot with Dialogflow!
