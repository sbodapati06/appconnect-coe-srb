# lab4 - Using Avro based schemas with Kafka Nodes

---

# Table of Contents
- [1. Introduction](#introduction)
- [2. Postgres - create database and sample data ](#postgres-db)
- [3. App Connect Toolkit](#toolkit)
  * [3a. Open Workspace](#open-workspace)
  * [3b. Create Vault, Credential](#create-vault)
  * [3c. Start Integration Server demo-postgres](#start-is)
  * [3d. Create Message Flow](#message-flow)
- [4. Deployment](#deployment)
  * [4a. Connect to Integration Server ](#integration-server)
  * [4b. Deploy Policy, Application](#deploy)
- [5. Testing](#testing)
  * [5a. Insert new record ](#testing-new-record)
  * [5b. Update record ](#testing-update-record)  
- [6. Summary ](#summary)
- [7. Miscellaneous ](#misc)

---

<br>

## 1. Introduction <a name="introduction"></a>
In this lab, you will explore the KafkaProducer Node of the App Connect Enterprise (ACE) Toolkit to utilize and verify input data against an Avro schema. <br>

From ACE 13.0.2.0, you can use Avro based schemas with Kafka nodes to have data serialization and deserialization. <br>

Avro is an open source data serialization format known for its efficiency in storage and speed, particularly in big data environments. It offers advantages in terms of schema evolution, compact binary format, and high performance for write intensive operations, making it suitable for use cases like data streaming with Apache Kafka. <br>

In App Connect v13.0.2, the KafkaProducer node has been enhanced so that the JSON messages parsed into the node can be serialized into Avro format using a referenced Avro schema before it's published to the Kafka topic. <br>

The KafkaConsumer, and KafkaRead nodes have similar properties, but for consuming messages from a topic and deserializing back into JSON. <br>

## 2. Workshop Environments  <a name="workshop-env"></a>

![alt text](image.png)

You will be doing this lab from the Windows VM. <br>

Also, a Cloud Pak for Integration Platform having IBM Event Streams capability. <br>
![alt text](image-1.png)


## 3. Capture Kafka connection details <a name="toolkit"></a>

Login to IBM Event Streams console as student(n). <br>

On the welcome page, click on "Connect to this cluster" tile. <br>

Now, click on "Generate SCRAM credentials". <br>
Name it student(n)-scram-cred. Replace (n) with your student id, example student1. <br>
![alt text](image-16.png)
<Next> <next> <Next> Hit 3 next buttons, and finally hit "Generate credentials". <br>

Copy and Save the credentials into the Notepad as below. <br>
Bootstrap Servers: aaaa <br>
SCRAM username: bbbb  <br>
SCRAM password: cccc <br>

![alt text](image-17.png)

Now, click on the "Basic Authentication token" tab, and copy / paste the "SCRAM Basic Authentication token" into the Notepad as below. <br>
**Basic Authentication Token: dddd** <br>
![alt text](image-18.png)

Now, lets **download the PKCS12 certificate.** <br>

Scroll down to the Certificates section, and click "Download Certificate". <br>
![alt text](image-19.png)

A password should be genrated, copy/paste the password into the Notepad as below. <br>
**Certificate Password: eeee** <br>

You should have captured like this. <br>
![alt text](image-20.png)


## 3. App Connect Toolkit <a name="toolkit"></a>

From the Windows VM, open IBM App Connect Enterprise Toolkit from the desktop. <br>

### 3a. Open Workspace <a name="open-workspace"></a>

Workspace: C:\Users\techzone\IBM\ACET13\workspace\avro-demo <br>
![alt text](image-2.png)

Close the welcome page. <br>

Navigate menu, Window > Show view > Other > search for Terminal > select Terminal and click Open button. <br>


### 3d. Create KakfaProducer Message Flow  <a name="message-flow"></a>

Create a New Application, name it KafkaAvro. <br>

Create a New Message Flow, name it FlightLanding_Events_KafkaProducer. <br>


Drop and drop HttpInput, Compute, KafkaProducer, HttpReply nodes in to the Message Flow canvas, and wire as below. <br>

![alt text](image-3.png)


### 3d. Configure Message Flow Nodes<a name="configure-nodes"></a>

Click on the **HTTP Input** Node. <br>
Set "Path suffix for URL": /flight-landings <br>
![alt text](image-4.png)

Change Input Message Parsing as "JSON". <br>
![alt text](image-6.png)


Click on the **Compute** Node. <br>
![alt text](image-5.png)

Uncomment ESQL lines. <br>
![alt text](image-33.png)


Click on the **Kafka Producer** Node. <br>
![alt text](image-7.png)

Policy Tab. <br>
![alt text](image-8.png)


Click, Create a policy link. <br>
![alt text](image-22.png)

Create a new Policy project. <br>
![alt text](image-23.png)

Name it "MyPolicies" and click Finish. <br>
![alt text](image-24.png)


Enter Policy name as "KafkaPolicy". <br>
![alt text](image-25.png)

Select Template as Kafka. <br>

Confirm to switch policy or template ... click OK. <br>
Populate the Kafka connection properties. <br>
Bootstrap server: <br>
Security Protocol: SASL_SSL <br>
SASL Mechanism: SCRAM_SHA_512 <br>
Secrity Identity: kafkaCred <br>
SASL config: org.apache.kafka.common.security.scram.ScramLoginModule required; <br>
SSL truststore location: C:\Users\techzone\Downloads\es-cert.p12 <br>
SSL truststore type: PKCS12 <br>
SSL truststore security identity: kafkaTrust <br>
![alt text](image-27.png)

Hit Control+s to save the policy, and close the policy. <br>
![alt text](image-9.png)

**Hit Control+s to save the Message flow.** <br>


### 3e. Create KakfaConsumer Message Flow  <a name="message-flow"></a>

Create New Message flow, call it "FlightLanding_Events_Consumer". <br>
![alt text](image-10.png)

Drag KafkaConsumer, FileOutput nodes into the Messaeg flow canvas and wire them as below. <br>
![alt text](image-11.png)

Configure KafkaConsumer, and FileOutput Nodes as below. <br>

**KafkaConsumer** node configuration. <br>
![alt text](image-12.png)

Right click on "Kafka Consumer" node, and "Select Policy > Policy". Select KafkaPolicy from the list. <br> 
![alt text](image-13.png)

** File Output** node configuration. <br>
![alt text](image-14.png)

**Hit Control+s to save the Message flow.** <br>





### 3b. Create Vault, Credential <a name="create-vault"></a>

![alt text](image-21.png)

![alt text](image-28.png)

![alt text](image-29.png)

![alt text](image-30.png)

![alt text](image-31.png)

![alt text](image-32.png)


## 4. Deployment  <a name="deployment"></a>



### 4a. Deploy Policy Project <a name="integration-server"></a>

![alt text](image-34.png)

### 4b. Deploy Application <a name="deploy"></a>

![alt text](image-35.png)


![alt text](image-36.png)

<br><br>


## 5. Testing <a name="testing"></a>

On the Windows VM, open Command Line. Then copy/paste the below command. <br>

```
curl http://localhost:7800/flight-landings -d "{\"timestamp\":\"2025-11-13T01:29:44.192618\",\"location\":\"Orlando, FL\",\"airport\":\"MCO\",\"airline\":\"Saudhi Airlines\",\"flight\":\"SA623\",\"terminal\":\"E\",\"gate\":32,\"passengers\":28}"
```

![alt text](image-37.png)

Output... <br>
![alt text](image-38.png)
<br>

Now, lets check the Topic in Event Streams and make sure the Event is created. <br><br>

![alt text](image-39.png)

Flight_Landings_KafkaConsumer Message flow should have received the event and written to C:\temp\flight-landings.out file. Use Notepad++ and check the data. <br>

NOTE that we do not have any validation to the JSON event. We will now use the AVRO schema to validate our JSON. <br>



### 5a. Upload Avro schema <a name="testing-new-record"></a>

Upload the schema if not there already. Open IBM Event Streams Console and upload the flight-landings.json schema. <br>


Download flight-landings.json schema here. <br>
ADD FLIGHT LANDINGS SCHEMA HERE. <br>

Our flight)landings schema looks like below. <br>
![alt text](image-42.png)


Click "Add Schema" button. <br>
![alt text](image-40.png)

![alt text](image-41.png)

![alt text](image-43.png)

<br>


### 5c. Create Avro Policy for KafkaProducer <a name="testing-cdc--trace"></a>

Back to App Connect Toolkit, and Flight_Landings_KafkaProducer flow, click on KafkaProducer node then Avro tab. <br>

![alt text](image-44.png)

Click Browse, then "Create Policy" to create Avro policy. <br>

![alt text](image-45.png)

Select Template "Schema Registry". <br>
![alt text](image-46.png)
Save, and close the Policy. <br>

<br>
Now, we need to populate Schema ID. Lets retrieve the Schema ID using the curl command below. <br>
```
curl -k -X GET -H 'Content-Type: application/json" -H "Authorization: The Basic Auth Token you saved into the Notepad" https://schema-registry-url-you-saved-into-the-notepad/artifacts/flight_landings/versions/1/meta
```
![alt text](image-47.png)
In this case the the Global_id is 1. So, populate KafkaProducer Avro Schema Id as below. <br>
![alt text](image-48.png)
<br>

### 5c. Create Avro Policy for KafkaConsumer <a name="testing-cdc--trace"></a>

Back to App Connect Toolkit, and Flight_Landings_KafkaConsumer flow, click on KafkaConsumner node then Avro tab. <br>

Browse and select the Avro policy that you have created above. <br>
![alt text](image-49.png)

Save the flow. <br>

Create schemaCred as below. <br>
![alt text](image-51.png)

<br>


### 5c. Redeploy the Policy and Application <a name="testing-cdc--trace"></a>

Drag and drop the MyPolicies and KafkaAvro application into the Integration server demo-avro. <br>

<br>

## 6. Testing Avro <a name="summary"></a>

Run the below curl command to send another flight landings event. <br>

```
curl http://localhost:7800/flight-landings -d "{\"timestamp\":\"2025-11-13T02:30:45.192618\",\"location\":\"New Delhi, IN\",\"airport\":\"RGV\",\"airline\":\"Air India\",\"flight\":\"AI601\",\"terminal\":\"A\",\"gate\":11,\"passengers\":401}"
```

![alt text](image-50.png)


## 6. Testing Summary <a name="summary"></a>

Congratulations! You have successfully generated records in PostgreSQL database and received the records as events using APP Connect Change Data Capture connector.

<br><br><br>




