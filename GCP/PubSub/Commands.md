

This command creates a topic in the selected project, and this can be verified in the Cloud Console:
```
gcloud beta pubsub topics create exampleTopic
```
```
 gcloud help beta pubsub topics create
```


Run the following command to create a subscription for the exampleTopic created previously:
```
gcloud beta pubsub subscriptions create --topic exampleTopic exampleTopicSubscription
```

Run the following command to publish a message with data to Cloud Pub/Sub. The -- attribute flag is used to pass the data with the message:
```
gcloud beta pubsub topics publish exampleTopic --message "Sample Message" --attribute="EmployeeID=20,FirstName=Sarvesh,LastName=Thirukkumaran"
```


Run the following command to pull the message from Cloud Pub/Sub. 
The --auto-ack flag will remove the message from the Pub/Sub once it is pulled:
```
gcloud beta pubsub subscriptions pull --auto-ack exampleTopicSubscription
``` 

#### PRECO
https://cloud.google.com/pubsub/pricing


Run the following command to publish a message to the intended topic in Google Cloud Pub/Sub. Replace Message Text and attribute in the message as per your project:
```
gcloud beta pubsub topics publish pubsubTopic 
	--message "Message Text" 
	--attribute="EmployeeID=20,FirstName=Vinod,LastName=Kumar,DateOfJoining=2016-02-16,Country=Norway"
```

Use the following command to pull the message without the --auto-ack flag to see the contents of the message in JSON format in the console. Replace pubSubTopicSubscriber with the Pub/Sub subscriber of your project. The message will be displayed in JSON format. The JSON format inserts each attribute in a new line; so it cannot be directly imported to BigQuery:
```
gcloud beta pubsub subscriptions pull pubSubTopicSubscriber --format=json
```


Run the following command to see the output flattened, where each key-value pair is written in one line. Replace pubSubTopicSubscriber with the Pub/Sub subscriber of your project:
```
gcloud beta pubsub subscriptions pull pubSubTopicSubscriber --format=flattened
```

The following command exports the message in CSV format for the message that was published earlier. CSV files require a projection to be specified, which is a defining list of attributes from a message to be exported to the file. The following code will export all the columns in the message data. The columns are specified in the format option within (). Remove [no-heading] if you want a heading to be added to the output:
```
gcloud beta pubsub subscriptions pull pubSubTopicSubscriber --format="csv[no-heading](message.attributes.EmployeeID,message.attributes.FirstName,message.attributes.LastName,message.attributes.DateOfJoining,message.attributes.Country)"
```

To know the list of formats that can be used to export the Cloud Pub/Sub message, run the following command:
```
gcloud topic formats
```


# Importing message data into BigQuery

```
gcloud beta pubsub subscriptions pull pubSubTopicSubscriber --format="csv[no-heading](message.attributes.EmployeeID,message.attributes.FirstName,message.attributes.LastName,message.attributes.DateOfJoining,message.attributes.Country)" --auto-ack >> messages.csv

gsutil cp messages.csv gs://bucketname

bq load ResultDS.temp_table gs://bucketname/messages.csv employee_id:integer,first_name:string,last_name:string,date_of_joining:datetime,country:string


```


