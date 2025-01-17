---
# Text Analytics SDK Documentation
by **MAQ Software**

[Privacy Policy][PP]
---
## Prerequisites
> Requires Python 3.6 or above
## Installation
```sh
$ pip install MAQTextSDK
```
## Getting Started with SDK
#### 1. Register for the SDK
You need an O365 Account to register.
   * Click [here][PlDb] to register using the *Free Trial* button in the *Developer Zone* pane. 
   * Save the **API Endpoint** and **API Key** you receive while registering on the *Developer Zone* pane.
#### 2. Install the SDK
```sh
$ pip install MAQTextSDK
```
#### 3. Using the SDK for Sentiment Analysis (Refer to Demo [here](Samples/SentimentDemo.ipynb))
   * Load the *Corpus* you plan to use in a *List* format, as shown in the following code snippet:
```sh
$ corpus = ['I love working on ML stuff.'
          ,'I hate working on stuff that is boring and repetitive.'
          ,'I love working from home during COVID-19.']
```
   * Use the **API Key** and **API Endpoint** you received while registering on the *Developer Zone* pane, as shown in the following code snippet:
```sh
$ APIKey = "Your_API_Key"
$ APIEndpoint = "Your_API_Endpoint"
````
   * Import the SDK and pass the *Corpus* along with the **API Endpoint** and **API Key**, as shown in the following code snippet:
```sh
$ import  MAQTextSDK.maq_text_analytics_linux as SentimentSDK
$ sentimentClient = SentimentSDK.MAQTextAnalyticsLinux(base_url = APIEndpoint)

$ response = sentimentClient.post_sentimentclassifier(api_key = APIKey, data_input = corpus)

$ for document in response:
    print("Document:")
    print(document['text'])
    print("Sentiment:")
    print(document['sentiment'])
    print()
````
#### 4. Using the SDK for Key Phrase Extraction (Refer to Demo [here](Samples/KeyPhrasesDemo.ipynb))
   * Load the *Corpus* you plan to use in a *Dict* format, as shown in the following code snippet:
```sh
$ #Load Text
keyphrase_input = dict()

#Set Text
keyphrase_input["text"] = "Does social capital determine innovation ? To what extent? This paper deals with two questions: Does social capital determine innovation in manufacturing firms?"

#Top Number Of Keyphrases
keyphrase_input["keyphrases_count"] = 10

#More Score, more different/diverse keyphrases are generated
#Less Score, more duplicate keyphrases are generated
#Value of Diversity Threshold can be between 0 and 1
keyphrase_input["diversity_threshold"] = 0.52

#Similarity threshold for Alias/Similar Keyphrase with Top Key Phrase [Similar Column in Output]
#More The Value, More accurate keyphrases are found with top key-phrase [Similar Column in Output]
#Value of Alias Threshold can be between 0 and 1
keyphrase_input["alias_threshold"] = 0.65
```
   * Use the **API Key** and **API Endpoint** you received while registering on the *Developer Zone* pane, as shown in the following code snippet:
```sh
$ APIKey = "Your_API_Key"
$ APIEndpoint = "Your_API_Endpoint"
````
   * Import the SDK and pass the *Corpus* along with the **API Endpoint** and **API Key**, as shown in the following code snippet:
```sh
$ import  MAQTextSDK.maq_text_analytics_linux as TextSDK
$ textClient = TextSDK.MAQTextAnalyticsLinux(base_url = APIEndpoint)
$ response = textClient.post_keyphrase_extractor(api_key = APIKey, data_input = keyphrase_input)

$ response_df = pd.DataFrame(response, columns = ['KeyPhrase','Score','Similar'])

#KeyPhrase	Score	Similar
#0 structural social capital	1.000000	[cognitive social capital]
#1 innovation	0.754714	[advancement]
#2 research network assets	0.586890	[information network assets, business network ...
#3 participation assets	0.575738	[]
#4 reciprocal trust	0.564874	[]
#5 explanatory variable	0.390424	[traditional explanatory variables]
#6 empirical investigations	0.355306	[study]
#7 many different forms	0.264404	[other forms]
#8 dominating view	0.207086	[]
#9 paper	0.121013	[]
````
#### 5. Using the SDK for PII Scrubber (Refer to Demo [here](Samples/PIIScrubberDemo.ipynb))
##### Note: PII Scrubber is powered by [presidio]

   * Load the *Corpus* you plan to use in a *Dict* format, as shown in the following code snippet:
```sh
$ #Load Text into dict
pii_input = dict()
pii_input["data"] = "Parker Doe has repaid all of their loans as of 2020-04-25. Their SSN is 859-98-0987. To contact them, use their phone number 555-555-5555. They are originally from Brazil"

#Set the list of entities to be removed
pii_input["entity_list"] = ["PERSON", "US_SSN", "PHONE_NUMBER"]

```
**List of Supported Entities:**

Entity Name | Action 
--- | --- 
PERSON | A full person name, which can include first names, middle names or initials, and last names.
CREDIT_CARD | A credit card number is between 12 to 19 digits. https://en.wikipedia.org/wiki/Payment_card_number
DATE_TIME | Absolute or relative dates or periods or times smaller than a day.
DOMAIN_NAME | A domain name as defined by the DNS standard.
EMAIL_ADDRESS | An email address identifies an email box to which email messages are delivered
IBAN_CODE | The International Bank Account Number (IBAN) is an internationally agreed system of identifying bank accounts across national borders to facilitate the communication and processing of cross border transactions with a reduced risk of transcription errors.
IP_ADDRESS | An Internet Protocol (IP) address (either IPv4 or IPv6).
LOCATION | Name of politically or geographically defined location (cities, provinces, countries, international regions, bodies of water, mountains
PHONE_NUMBER | A telephone number
US_BANK_NUMBER | A US bank account number is between 8 to 17 digits.
US_DRIVER_LICENSE | A US driver license according to https://ntsi.com/drivers-license-format/
US_PASSPORT | A US passport number with 9 digits.
US_SSN | A US Social Security Number (SSN) with 9 digits.

   * Use the **API Key** and **API Endpoint** you received while registering on the *Developer Zone* pane, as shown in the following code snippet:
```sh
$ APIKey = "Your_API_Key"
$ APIEndpoint = "Your_API_Endpoint"
````
   * Import the SDK and pass the *Corpus* along with the **API Endpoint** and **API Key**, as shown in the following code snippet:
```sh
$ import  MAQTextSDK.maq_text_analytics_linux as TextSDK
$ textClient = TextSDK.MAQTextAnalyticsLinux(base_url = APIEndpoint)
$ response = textClient.post_piiscrubber(api_key = APIKey, data_input = pii_input.copy())
$ print("Original Text: ", pii_input['data'])
  print("After PII Scrubbing: ", response['scrubbed_text'])
#Original Text:  Parker Doe has repaid all of their loans as of 2020-04-25. Their SSN is 859-98-0987. To contact them, use their phone number #555-555-5555. They are originally from Brazil
#After PII Scrubbing:  <PERSON> has repaid all of their loans as of 2020-04-25. Their SSN is <US_SSN>. To contact them, use their phone number #<PHONE_NUMBER>. They are originally from Brazil
````
## Using the Requests Package
   * Load the *Corpus* you plan to use in a Json format, as shown in the following code snippet:
```sh
$ corpus = {"data": [
    {"id": "1", "text": "I love working on ML stuff."},
    {"id": "2", "text": "I hate working on stuff that is boring and repetitive."},
    {"id": "3", "text": "I love working from home in COVID-19."}
]}
```
   * Input the **API Key** and **API Endpoint** that you received when you registered on the Text Analytics Platform, as shown in the following code snippet:
```sh
$ APIKey = "Your_API_Key"
$ APIEndpoint = "Your_API_Endpoint"
$ headers = {"APIKey": APIKey}
````
   * Import the Requests library and pass the *Corpus*, **API Endpoint**, and **Headers** into the Json parameter, as shown in the following code snippet:
```sh
$ import requests

$ response = requests.post(APIEndpoint + "/SentimentClassifier", headers = headers, json = corpus)
$ sentiment = response.json()
````

 * Import the Requests library and pass the *Corpus*, **API Endpoint**, and **Headers** into the Json parameter, as shown in the following code snippet for Key Phrase Extraction:
```sh
$ import requests

$ response = requests.post(APIEndpoint + "/KeyPhrase", headers = headers, json = keyphrase_input)
$ response_df = pd.DataFrame(response.json(), columns = ['KeyPhrase','Score','Similar'])
````

## Limit Tracking in API
In the free trial subscription, the API Key has a default quota of 500 batch calls. In a single batch call for Sentiment Analysis, you can send maximum 25 documents. Each batch call, irrespective of the how many documents it processes, counts as a single call. For Key Phrase Extraction and PII Scrubber each batch call can send maximum 1 document. 

## Build the SDK
To build the SDK from scratch. Please use *MAQTextAnalyticsLinux.swagger.json* file and build it using *[autorest][autorestLink]*. To build the SDK package please run the following code snippet:
```sh
$ git clone https://github.com/maqsoftware/MAQTextAnalyticsSDK.git
$ npm install -g autorest
$ autorest --input-file="MAQTextAnalyticsSDK/MAQTextAnalyticsLinux.swagger.json" --python --output-folder="TextAnalyticsSDK"
````

## FAQs

1. **What happens when my API Key expires?**
You can re-register for a free trial subscription by clicking [here][PlDb]. For more information, you can connect with us at support@maqsoftware.com.

    
2. **What happens when my API Limit is exhausted?**
    You will get an API Limit exceeded error when you try to use the SDK. You can visit the *Developer Zone* pane and generate a new API Key. For a premium service, you can connect with us at support@maqsoftware.com

3.	**Why I am getting batch size error?**
You can send maximum 25 documents in a single batch call for Sentiment Analysis. For Key Phrase Extraction and PII Scrubber, maximum one document can be sent in single batch call. Check that you are not trying to send more.

[presidio]: https://github.com/microsoft/presidio/
[PlDb]: https://maqtextanalytics.azurewebsites.net/#/DevelopersZone
[PP]: <https://maqsoftware.com/privacystatement>
[autorestLink]: <https://github.com/Azure/autorest>
