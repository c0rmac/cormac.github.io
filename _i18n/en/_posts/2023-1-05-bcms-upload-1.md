---
title: 'Public Documentation on BCMS Upload Services'
date: 2023-1-5
permalink: /posts/2023/1/bcms-upload-1/
tags:
  - bcms upload
  - technical details
---
I provide public documentation of my work regarding [BCMS Upload Services](/portfolio/bcms-upload/) in this article where you can see how I structured the application and what decisions led me to those structures be implemented in the application. I show my own way of creating an application and apply my principles which are shown in my work in `trin-app` and `trin-react`.

Note that this article is continuously being developed.

The BCMS Upload Services application is based on Kotlin Multiplatform with a Common Module, a Server Module and a Web App Module. The `bcms-alt-service` submodule has also been created which holds logic that interacts with the official BCMS site. There is an external Wordpress module that constitutes the front page site.

## The Common Module
The fundamental model of the application is developed here, i.e. where most of the basic features of the application are kept. Data can be safely transferred between the Web App and the server. Generally, the module follows the trin-app guidelines and it imports `trin-app` as a submodule. We setup our submodules as follows:

```
commons
|-----upload
    |-----entity
        |-----SubmissionStatus
        |-----UploadStatus
                |-----Standby : UploadStatus
                |-----InternalUploading : UploadStatus
                |-----PendingPayment : UploadStatus
                |-----PendingBCMSUpload : UploadStatus
                |-----BCMSUploading : UploadStatus
                |-----Success : UploadStatus
                |-----Error : UploadStatus
    |-----exception
        |-----UserNotInternallyFoundException
        |-----BCMSException
        |-----BCMSDeletionInProgressException : BCMSException
        |-----BCMSUploadInProgressException : BCMSException
        |-----BCMSBusyException : BCMSException
        |-----PaymentCannotBeTakenException
        |-----InsufficientFundsException
        |-----BCMSSubmissionNotFoundException
        |-----BCMSAuthNotFoundException
        |-----UploadOrderRequestNotFoundException
        |-----UploadOrdersNotFoundException
    |-----port
        |-----MyAppSuppDocUploadAPI
        |-----SubmissionsUploadAPI
        |-----MyAppStatDocUploadAPI
        |-----MyNoticesSuppDocUploadAPI
        |-----MyNoticesStatDocUploadAPI
    |-----transport
        |-----generic
            |-----IUploadOrder
            |-----UploadOrder : IUploadOrder
            |-----UploadOrderRequest
        |-----myappsuppdoc
            |-----IUploadOrder
            |-----UploadOrder : IUploadOrder
            |-----UploadOrderRequest
        |-----Submission
        |-----MyApplicationsSubmission
        |-----MyNoticesSubmission
        |-----UploadOrderRequest
        |-----UploadOrder

```

It is important that this module is kept as compact as possible and keeping any model here requires careful reasoning. In the file tree above, I draw your attention to the `upload` subpackage to give an example of my own reasoning. `UploadStatus` and `SubmissionStatus` appear in <em>entity</em>. It makes sense to keep them in the Common Module since information about uploading documents is transmitted frequently in the application and requires a standard model throughout the application.

Although care must be taken about most of what is kept in this module, there are a couple of rules of thumb for what can normally be included in the module. Consider `exception`. Many errors are thrown in an application between 'a form not filled in correctly' and an internal problem in the application occuring. Many of the errors a user would see are kept here. An error can be thrown on the Server Module side to notify the user of an error. Thanks to `RESTAPIManager` from `trin-react`, no preconfiguration in the web application is required to handle an `exception` error. The rules of thumb are discussed further in `trin-app' guidelines and more information on trin-app matters can be found in an article to be published soon.

### Transmission of Models
`kotlinx.serialization` is used for serializing the models throughout the application. Each `port` subpackage maintains a model of commands that are made between the server and the web application. It is up to the Web App Module and the Server Module how the models should be implemented.

## The Web App Module
A ReactJS application written in Kotlin-JS and based on the `trin-react` framework is developed in this module. This is a simple interface application with minimal number of pages; they are:

```
/public/login - LogincVC
/public/register - RegisterVC


/consumer/dashboard - DashboardVC

/consumer/myapplications - MyApplicationVC
/consumer/services/myapplications/suppdoc/:pUUID - MyApplicationsSuppDocVC
/consumer/services/myapplications/statdoc/:pUUID - MyApplicationsStatDocVC

/consumer/mynotices - MyNoticesVC
/consumer/services/mynotices/suppdoc/:pUUID - MyNoticesSuppDocVC
/consumer/services/mynotices/statdoc/:pUUID - MyNoticesStatDocVC
```

### Application Forms
I give you an overview of the different forms available on the official BCMS site: My Notices/Supporting Documents, My Notices/Statutory Documents, My Applications/Statutory Documents, My Applications/Supporting Documents and it can be seen that their counterpart has been developed in the above. The first parts of the forms are very similar and I am motivated to create an abstract class on those three pages. The last form is quite different from the other three and cannot easily be based on the same abstract class.

Accordingly, I set up the <em>VCs</em> (ie <em>ViewControllers</em>) as seen in the diagram below:
<div style="text-align: center; padding-bottom: 16px;">
<img src="/images/portfolio/bcms-upload/client-app-upload-service-structure.gv.svg?raw=true" alt="Léaráid Aicmí de struchtúr na VCs." />
</div>

Here is an exact overview of the abstract features of `BaseUploadServiceVC`
```
// Document Submission Logic
abstract suspend fun uploadDocumentsLoadOnClickHandler()

abstract fun addDocumentOnClickHandler(event: Event)

abstract suspend fun getUploadRequests(): List<T>

abstract suspend fun getSubmission(): Submission

abstract suspend fun executeUploadRequest(uploadOrderRequest: T): T

abstract suspend fun deleteAllUploads()

abstract fun getSubmissionStatus(submission: Submission): SubmissionStatus

abstract fun updateSubmissionStatus(submissionStatus: SubmissionStatus)

// Render Logic
abstract fun RBuilder.uploadOrdersRender()

abstract fun RBuilder.uploadsRender()

abstract fun RBuilder.addDocumentRender()
```

and here is an overview of the abstract features of `GenericUploadServiceVC` and the inherited functions it implements from `BaseUploadServiceVC`:
```
abstract val documentTypes: Array<DocumentType.Generic>

// Implemented Functions from BaseUploadServiceVC
override fun addDocumentOnClickHandler(event: Event)

override fun RBuilder.uploadOrdersRender()

override fun RBuilder.uploadsRender()

override fun RBuilder.addDocumentRender()
```

That leaves `MyNoticesSuppDocVC`, `MyNoticesStatDocVC` and `MyNoticesStatDocVC` only `uploadDocumentsLoadOnClickHandler`, `getUploadRequests`, `getSubmission`, `executeUploadRequest`, `deleteAllUploads`, `getSubmissionStatus`, `updateSubmissionStatus` to implement.

## The Server Module
The server module is divided into sub-modules based on a hexagonal architecture. The http program is kept in a separate `web-app` module and out of the business logic. The basic logic of the application is kept in the `core` module as recommended by the hexagonal architecture guidelines but the difference is that `core` imports the Common Module together with the `port` module, a module that controls the external logic (ie database logic, `web-app` module etc.) and which decide what logic `core` needs to interact with. As an experiment, I've allowed the Common Module to be imported into every module, even `core' to find out how a structure like this will turn out.

For `web-app`, the classes in `port` of each subpackage are implemented in `common`. Spring Boot is used for all http matters.

The application is hosted by AWS - Amazon Web Services.

Not much else can be commented on this module for the sake of application security.