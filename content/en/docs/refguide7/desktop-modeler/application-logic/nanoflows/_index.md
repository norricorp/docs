---
title: "Nanoflows"
url: /refguide7/nanoflows/
parent: "application-logic"
weight: 20
description: "Presents an overview of all the elements that can be used in a nanoflow."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## 1 Introduction

{{% alert color="info" %}}

This page is an overview of all the elements that can be used in a nanoflow. For the properties of the nanoflow itself, see [Nanoflow Properties](/refguide7/nanoflow/).

{{% /alert %}}

Nanoflows are similar to [microflows](/refguide7/microflows/), as they allow you to express the logic of your application. However, they do have some specific benefits (for example, they run directly on the browser/device and can be used in an offline app). Furthermore, most of the actions run directly on the device, so there is also a speed benefit.

## 2 When to Use Nanoflows

### 2.1 Offline Mobile Apps

Nanoflows are designed with offline-first applications in mind, as they allow you to model application logic that works in offline apps. Since all database-related actions will be executed on the local offline database, nanoflows in offline apps will be super fast.

### 2.2 Logic Where No Connection Is Needed

Nanoflows also offer great value in online applications (for example, for UI logic, validations, calculations, and navigation). However, please keep in mind that when you perform database-related actions, each action will create a separate network request to the Mendix Runtime. The following actions interact with the database:

* Create
* Commit
* Retrieve
* Rollback

Therefore, the best practice is to use nanoflows in online applications when they do not contain the above actions.

{{% alert color="info" %}}
Changing objects without committing is not a database-related action, as changes are applied on the device or in the browser.
{{% /alert %}}

### 2.3 Other Cases

The previous section stated that nanoflows perform best in online applications when no database-related actions are used, and these are generally the best cases. However, nanoflows that contain at most one database-related action can also still perform well. Because such nanoflows only require one network call, they perform as well as a microflow. An example of such a use case is performing validation logic on an object and committing the object in the same nanoflow.

## 3 Differences from Microflows

There are three main differences betweeen nanoflows and microflows:

* When a nanoflow steps through its actions, client actions are directly executed. For example, an open page action immediately opens a page instead of at the end of the nanoflow. This is different from client actions in a microflow, which only run when the client receives the result from the microflow.
* When used in nanoflow activities, expressions do not support the following variables: `$latestError`, `$latestSoapFault`, `$latestHttpResponse`, `$currentSession`, `$currentUser`, `$currentDeviceType`.
* Nanoflows are not run inside a transaction, so if an error occurs in a nanoflow, it will not roll back any previous changes.

## 4 Keyboard Support

The nanoflow editor offers keyboard support for navigating and manipulating the nanoflows. The following table shows the keys that can be used.

| Key(s) | Effect |
| --- | --- |
| Arrow keys | Selects the nearby element (activity, event, loop, or parameter) in the direction of the arrow. |
| <kbd>Enter</kbd> | Edits the properties of the selected element. |
| <kbd>F2</kbd> | Renames the variable introduced by the selected element. |
| <kbd>Shift</kbd> + <kbd>F2</kbd> or just starting to type | Edits the caption of the selected element. |
| <kbd>Ctrl</kbd> + arrow keys | Moves the selected element in the direction of the arrow. |
| <kbd>Tab</kbd> | If a loop is selected, the first element inside the loop is selected. |
| <kbd>Shift</kbd> + <kbd>Tab</kbd> | If an element inside a loop is selected, the loop itself is selected. |
| <kbd>Home</kbd> | Selects the start event. |
| <kbd>End</kbd> | Cycles through the end events. |
| Context-menu key or <kbd>Shift</kbd> + <kbd>F10</kbd> | Opens the context menu for the currently selected element. |

## 5 Notation and Categories

The graphical notation of nanoflows is based on the [Business Process Model and Notation](https://en.wikipedia.org/wiki/Business_Process_Model_and_Notation) (BPMN). BPMN is a standardized graphical notation for drawing business processes in a workflow.

A nanoflow is composed of elements. The following categories are used:

* [Events](#events) represent the start and end points of a nanoflow and special operations in a loop
* [Flows](#flows) form the connection between elements
* [Gateways](#gateways) deal with making choices and merging different paths again
* [Activities](#activities) are the actions that are executed in a nanoflow
* [Artifacts](#artifacts) provide the nanoflow with input and allow comments to be made

### 5.1 Events<a name="events"></a>

Events represent the start and end points of a nanoflow and special operations in a loop.

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/917902.png" link="/refguide7/start-event/" >}} | [Start event](/refguide7/start-event/) | The starting point of the nanoflow. A nanoflow can only have one start event. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/918113.png" link="/refguide7/end-event/" >}} | [End event](/refguide7/end-event/) | Defines the location where the nanoflow will stop. Depending on the return type of the nanoflow, in some cases a value must be specified. There can be more than one end event. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/918115.png" link="/refguide7/continue-event/" >}} | [Continue event](/refguide7/continue-event/) | Used to stop the current iteration of a loop and continue with the next iteration. Please note that continue events can only be used inside a [loop](/refguide7/loop/). |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/918026.png" link="/refguide7/break-event/" >}} | [Break Event](/refguide7/break-event/) | Used to stop iterating over the list of objects and to continue with the rest of the flow after the loop. Please note that break events can only be used inside a [loop](/refguide7/loop/). |

### 5.2 Flows<a name="flows"></a>

Flows form the connection between elements.

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/917883.png" link="/refguide7/sequence-flow/" >}} | [Sequence flow](/refguide7/sequence-flow/) | An arrow that links events, activities, splits, and merges with each other. Together they define the order of execution within a nanoflow. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/917688.png" link="/refguide7/annotation-flow/" >}} | [Annotation flow](/refguide7/annotation-flow/) | A connection that can be used to connect an annotation to another element. |

### 5.3 Gateways<a name="gateways"></a>

Gateways deal with making choices and merging different paths.

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/917726.png" link="/refguide7/exclusive-split/" >}} | [Exclusive split](/refguide7/exclusive-split/) | Makes a decision based on a condition and follows one and only one of the outgoing flows. Please note that there is no parallell execution in nanoflows. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/918116.png" link="/refguide7/merge/" >}} | [Merge](/refguide7/merge/) | Can be used to combine multiple sequence flows into one. If a choice is made in a nanoflow and afterwards some common work needs to be done, you can combine the two (or more) paths using a merge. |

### 5.4 Activities<a name="activities"></a>

Activities are the actions that are executed in a nanoflow.

#### 5.4.1 Object Activitities

Object activities can be used to create and manipulate objects. The [domain model](/refguide7/domain-model/) defines the object types ([entities](/refguide7/entities/)) that can be used.

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/917661.png" link="/refguide7/change-object/" >}} | [Change object](/refguide7/change-object/) | Can be used to change the members of an object. This can be done with or without commiting. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/17661961.png" link="/refguide7/committing-objects/" >}} | [Commit object(s)](/refguide7/committing-objects/) | Can be used to commit the changes to one or more objects. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/917756.png" link="/refguide7/create-object/" >}} | [Create object](/refguide7/create-object/) | Can be used to create an object. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/917866.png" link="/refguide7/retrieve/" >}} | [Retrieve](/refguide7/retrieve/) | Can be used to get one (or more) associated objects of another object. The activity can also get one (or more) objects directly from the database. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/918119.png" link="/refguide7/rollback-object/" >}} | [Rollback object](/refguide7/rollback-object/) | Can be used to undo the changes (that have not been committed) made to the object in the part of the nanoflow preceding the activity. This also deletes objects that have been created but never committed. |

#### 5.4.2 List Activitities

List activities can be used to create and manipulate lists of objects.

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/918007.png" link="/refguide7/change-list/" >}} | [Change list](/refguide7/change-list/) | Can be used to change the content of a list variable. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/918009.png" link="/refguide7/create-list/" >}} | [Create list](/refguide7/create-list/) | Can be used to create a (empty) list variable. |

#### 5.4.3 Variable Activities

Variable activities can be used to create or change a variable within a microflow.

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/918011.png" link="/refguide7/change-variable/" >}} | [Change variable](/refguide7/change-variable/) | Can be used to change the value of a variable. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/918110.png" link="/refguide7/create-variable/" >}} | [Create variable](/refguide7/create-variable/) | Can be used to create a new variable. |

#### 5.4.4 Client Activities

Client activities can be used to have the web client of your application perform an action, such as showing a different page or downloading a file.

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/918114.png" link="/refguide7/close-page/" >}} | [Close page](/refguide7/close-page/) | Closes the page that is opened last by the user that calls the microflow in which this activity is used. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/917544.png" link="/refguide7/show-page/" >}} | [Show page](/refguide7/show-page/) | Can be used to show a page to the user that calls the microflow in which this activity is used. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/common-elements/activities/918097.png" link="/refguide7/validation-feedback/" >}} | [Validation feedback](/refguide7/validation-feedback/) | Can be used to display red text below a widget that displays an attribute or association. |

### 5.5 Loop

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/917804.png" link="/refguide7/loop/" >}} | [Loop](/refguide7/loop/) | A looped activity is used to iterate over a list of objects. For every object the flow inside the looped activity is executed. A looped activity can contain all elements used in nanoflows, with the exception of start and stop events. The flow starts at the first element with no incoming flows. |

### 5.6 Artifacts<a name="artifacts"></a>

Artifacts provide the nanoflow with input and allow comments to be made.

| Graphic | Name | Description |
| --- | --- | --- |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/918019.png" link="/refguide7/parameter/" >}} | [Parameter](/refguide7/parameter/) | Data that serves as input for the nanoflow. Parameters are filled at the location from which the nanoflow is triggered. |
| {{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/917689.png" link="/refguide7/annotation/" >}} | [Annotation](/refguide7/annotation/) | An element that can be used to put comments in a nanoflow. |

## 6 Variable Usages

The Modeler visualizes which variables are used by selected object(s). It does this by showing the used variables in white text on a blue background. Conversely, elements that use the variable(s) defined by the selected object(s) are marked with the word **Usage** in white text on a green background.

In the example below, the parameter **AccountPasswordData** is highlighted because it is used in the selected activity. And the activity **Save password** has a usage label because it uses the variable defined by the selected activity.

{{< figure src="/attachments/refguide7/desktop-modeler/application-logic/microflows/16843950.png" >}}

## 7 Errors

When an error occurs in a nanoflow, all the changes that have been made to objects are not rolled back and the nanoflow is aborted. Nanoflow actions do not support error handlers.

## 8 Nanoflow Debugging

Step-by-step debugging is not supported yet. For now, we recommend using log message actions, which are shown in the console log of the Desktop Modeler (for version 7.13.0 and above).

## 9 Security

Nanoflows are executed in the context of the current user. Any operation for which the user is unauthorized will fail. For instance, when objects are retrieved in a nanoflow, only the ones for which the current user has read access will be returned. Committing an object only succeeds when the current user has write access for all changes.
