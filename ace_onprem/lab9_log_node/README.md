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

