# Using Log Node


---
# Table of Contents
- [1. Introduction](#introduction)
- [2. App Connect Toolkit](#toolkit)
- [3. Import Tutorial](#tutorial-import)
- [4. Log Nodes](#log-nodes)
  * [4a. Configure Log Node 1](#log-node1-configure)
  * [4b. Configure Log Node 2](#log-node2-configure)
- [5. Create Integration Server TEST_SERVER](#create-is)
- [6. Flow Exerciser Testing ](#testing)
  * [6a. Test valid message ](#test-valid-message)
  * [6b. Test invalid message ](#test-invalid-message)
- [7. Summary ](#summary)
---

<br>

## 1. Introduction <a name="introduction"></a>

In this lab, you will explore App Connect Enterprise (ACE) Toolkit Log Node. We have introduced Log Node in ACE v12.0.11.x, this node allows you to write log entries to the Activity log. <br>



## 2. App Connect Toolkit <a name="toolkit"></a>

Open IBM App Connect Toolkit, and workspace /home/<user>/workspace/ace-workshop. <br>
![alt text](./images/image.png)

You will be greeted with the IBM App Connect Enterprise Toolkit Welcome page. <br>
![alt text](./images/image-1.png)



## 3. Import Tutorial <a name="tutorial-import"></a>

Click on the **Tutorials** tile. <br>

Search for "Validate". <br>
![alt text](./images/image-2.png)

Select "Using a Validate node to check a message against a JSON schema" tutorial. <br>
![alt text](./images/image-3.png)

Click Start button. <br>
![alt text](./images/image-4.png)


Click Import button. <br>
![alt text](./images/image-5.png)

That should import ValidateJSON.msgflow. Minimize the side Tutorial pane. <br>
![alt text](./images/image-6.png)


## 4. Log Nodes <a name="log-nodes"></a>

Now add Log nodes into the Message Flow canvas. Drag Log Node(s) into the Flow Canvas as below. <br>
![alt text](./images/image-7.png)


Rewire the nodes as below. <br>
![alt text](./images/image-8.png)


### 4.1. Configure Log Node 1 <a name="log-node1-configure"></a>

![alt text](./images/image-9.png)

![alt text](./images/image-10.png)

![alt text](./images/image-11.png)

Click on the Basic Tab, and Configure button. <br>
For the Message detail, enter "Received an order in flow: ", and add Timestamp, Flow Name, Application fields as below. <br>
![alt text](./images/image-12.png)


Add timestamp. <br>
![alt text](./images/image-13.png)

Add "Flow Name", and "Application Name". <br>
![alt text](./images/image-14.png)

Add a custom field "Order", click "Add property". <br>
![alt text](./images/image-15.png)

Enter Order, and type String...then, click "Edit Mappings". <br>

![alt text](./images/image-16.png)

![alt text](./images/image-17.png)

Select Order object under Http Input > Payload > Order segment as below. <br>
![alt text](./images/image-18.png)

Save, and close the window. <br>
![alt text](./images/image-19.png)

Now, you can see that the Log node is configured with JSONata expression. <br>
![alt text](./images/image-20.png)

<br>


### 4.2. Configure Log Node 2 <a name="log-node2-configure"></a>

We will add ExceptionList tree object. <br>

Click on Log Node 2, and Map Inputs tab. <br>
![alt text](./images/image-21.png)

Add **ExceptionList**. <br>
![alt text](./images/image-22.png)

Click **OK**.<br>
![alt text](./images/image-23.png)

Now, click on the Basic tab, then Configure button. <br>
![alt text](./images/image-24.png)

For the "Message detail", enter "Error occurred while validating the mnessage in flow: <br>
Add "Flow Name", "Application Name" from the Flow Details segment. <br>
Also, add Property "ErrorMessage" and copy/paste the below JSONata expression into the value textbox. <br>
```
{{$map($mappingInput_ExceptionList.Insert, function($v) { $v.Text })}}
```
![alt text](./images/image-25.png)

Hit the Save the configuration. <br><br>

