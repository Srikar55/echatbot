
Note: the bot is still using the original State API that was deprecated. It works, but the recommendation from Microsoft is to implement a [custom state data](https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-state-azure-cosmosdb).

# EChatbot

An example of a chatbot built with [Microsoft Bot Framework](https://dev.botframework.com/) and featuring e-commerce capabilities via:

### Resources used

* [Moltin](https://moltin.com)
* [Azure Search](https://azure.microsoft.com/en-us/services/search)
* [LUIS](https://www.microsoft.com/cognitive-services/en-us/language-understanding-intelligent-service-luis)
* [Text Analytics](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)

## How To Run

If you would like to run it, you would need:

* A [Moltin](https://moltin.com) subscription with the [Adventure Works](https://msftdbprodsamples.codeplex.com/releases/view/125550) data (I previously [shared scripts](https://github.com/pveller/adventureworks-moltin) to import Adventure Works data into Moltin)
* [Azure Search](https://azure.microsoft.com/en-us/services/search) service with three indexes - `categories`, `products`, and `variants`. You can find the index definitions and the script that can set up everything you need [here](/indexes)
* ~[Recommendations API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) endpoint with the FBT (frequently bought together) model trained on historical orders. Here's the [instruction on how to set it all up](/recommendations)~
* Trained [LUIS](https://www.microsoft.com/cognitive-services/en-us/language-understanding-intelligent-service-luis) model for the intents that require NLU to be recognized. You can import [the app that I trained](/luis) to get a head start

Deploy your bot (I used [Azure App Service](https://azure.microsoft.com/en-us/services/app-service/)) and register it with the [dev.botframework.com](https://dev.botframework.com/).

Set the following environment variables:

* `MICROSOFT_APP_ID` - you will get it from the [dev.botframework.com](https://dev.botframework.com/) during registration
* `MICROSFT_APP_PASSWORD` - you will get it from the [dev.botframework.com](https://dev.botframework.com/) during registration
* `SEARCH_APP_NAME` - the name of your [Azure Search](https://azure.microsoft.com/en-us/services/search) service. The code assumes that you have all three indexes in the same Azure Search resource
* `SEARCH_API_KEY`- your API key to the [Azure Search](https://azure.microsoft.com/en-us/services/search) service
* `LUIS_ENDPOINT` - the URL of your published LUIS model. Please keep the `Add verbose flag` on and remove `&q=` from the URL. THe bot framework will add it.
* `SENTIMENT_API_KEY` - your API key to the [Text Analytics](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api) service.
* `SENTIMENT_ENDPOINT` - the enpoint of yout [Text Analytics](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api) service. Defaults to `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

If you would like to connect the [Bing Spell Check](https://www.microsoft.com/cognitive-services/en-us/bing-spell-check-api) service, you would do so in LUIS when publishing your endpoint. This integration is transparent to the app and all you do is provision your Azure subscription key to the service and connect it to your LUIS app.

## To-Do

* The shopping cart is currently kept in the bot's memory (`session.privateConversationData.cart`) and does not sync back to Moltin
* Checkout process is not integrated with Moltin
* The bot is not multi-lingual
