---
title: Daria QA Green Fifteen v2
description: Handle the message execution in case of errors.
auto_validation: true
time: 15
tags: [ tutorial>intermediate, software-product>api-management]
primary_tag: products>sap-hana--express-edition
parser: v2
---


### You will learn
  - How to induce an artificial error or exception in your message pipeline
  - How to handle exceptions through _Exception Sub-process_
  - How to use different end events to manage errors
  - What is a send step

In this exercise, we will induce an artificial exception situation via a script.
An _Exception Sub-process_ will be added to handle the artificial exception.
We will also play with the different End Events to see how they differ.

---

### Step 1: Induce artificial exception

Let us induce an artificial error in the integration flow. We shall use the script step for the same.

1. Add a script step:

    * Click on the design palette.
    * Choose __Message Transformers__.
    * Choose __Script__.
    * Choose __Groovy Script__.
    * Drag it on to the execution pipeline before the __Message Mapping__ step.
    * Click the __Script__ step and choose __Create (+)__ from the list of speed buttons.

        ![Add script](Add Script.png)

    * Add the following code to the script's `processData` method:

        ```Groovy
        def Message processData(Message message)
        {
            throw new Exception();
        }
        ```     

    * Click __Ok__.

2. Save, deploy and execute the integration flow.

     * Go to the **Monitoring** view and look for an entry for the message just processed. The status of the message should be __Failed__ -- this indicates that an exception was thrown.     


     The __Error Details__ section gives more information about the exception.

     * To get additional information about the exception, scroll down to the __logs__ section (or click on the __Logs__ tab) and click __Info__.

     A graphical viewer opens up and provides a visual representation of the exception location on the message execution pipeline.

     As the graphical viewer shows, the exception is caused by the __Script__ step.


### Step 2: Add Exception Sub-process to handle exception

Let us now add an Exception Sub-process to catch the exception and handle it.

1. Add a Exception Sub-process to the Integration Flow:

    * Click on the design palette.

    * Choose __Process__.

    * Choose __Exception Subprocess__.

    * Drag it within the __Integration Flow__.


2. Save, deploy and execute the integration flow.

       * Go to the __Monitoring view__ and look for an entry for the message just processed.  The status of the message should be __Completed__ -- this indicates that the exception thrown by the script step is caught by the __Exception Subprocess__.     

       ![Error View With Exception SubProcess](Error View With Exception SubProcess.png)    


### Step 3: Send mail to intimate about exception

You can configure additional actions in the Exception Sub-process that you would like to perform in case of an exception. In the exercise below, we intimate a systems manager about the exception via an email and ask him to resend the request if he likes us to reprocess.

1. Add a mail receiver

    * Add a receiver to the canvas and connect it to the _End Message_ in the _Exception Sub-process_.

    * Choose a mail adapter:

    * Copy the mail channel configuration:

        * Click any of the configured mail channel and choose the speed button for _Copy Configuration_.
        * Now select the newly created mail channel and choose the speed button for _Paste Configuration_. Click _yes_ on the confirmation pop-up.

    * Click on the new mail channel, go to the _Properties Sheet_, go to the _Connection_ tab and set the _Subject_ field to _An error occurred - Please resend_:

 

2. Save, deploy and execute the integration flow.

3. Check your configured inbox. You should get the following email:

    ![Error email](Error email.png)   
