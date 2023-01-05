---
title: 'Cáipéisíocht Phoiblí ar BCMS Upload Services'
date: 2023-1-5
permalink: /posts/2023/1/bcms-upload-1/
tags:
  - bcms upload
  - technical details
---
Cuirim ar fáil cáipéisíocht phoiblí ar mo chuid oibre i dtaobh [BCMS Upload Services](/ga/portfolio/bcms-upload/) san alt seo ina bhfeictear cén struchtúr a chuir mé ar an bhfeidhmchlár agus cé na cinntí a thug orm na struchtúir úd a chur i bhfeidhm sa bhfeidhmchlár. Léirím mo mhodh féin ar a gcruthóinn feidhmchlár agus cuirim i bhfeidhm mo chuid prionsabal atá i léirithe i mo chuid oibre i `trin-app` agus `trin-react`.

Tabhair faoi deara go bhfuil an t-alt seo dhá fhorbairt i gcónaí.

Tá an feidhmchlár BCMS Upload Services bunaithe ar Kotlin Multiplatform le Modúl Coitianta, Modúl Freastálaí agus Modúl Feidhmchlár Idirlín. Cruthaíodh seachmhodúil `bcms-alt-service` freisin ina gcoinnítear loighic a idirghníomhann le suíomh oifigiúil BCMS. Tá seachfheidhmchlár Wordpress ann a leagans amach an príomhsuíomh.

## An Modúl Coitianta
Anseo a choinnítear samhail bhunúsach an fheidhmchláir. .i. tá an chuid is mó de bhunghnéithe an fheidhmchláir coinnithe sa modúl coitianta. Is féidir sonraí a thraschur go sábháilteach idir an feidhmchlár idirlín agus an freastálaí. Go hiondúil, cloíonn an modúl le treoirlíntí trin-app, iompórtálann sé `trin-app` mar sheachmhodúl agus is mar seo a leanas a leagtar amach cuid den mhodúl:

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

Tá sé tábhachtach go gcoinnítear an modúl seo chomh dlúth dingthe agus is féidir agus teastaíonn réasúnú cúramach le samhail ar bith a choinneáil ann. Sa gcrann comhaid thuas, tarrnaím bhur n-aird ar an bhfóphacáiste `upload` le sampla a thiubhairt ar mo réasúnú féin. Feictear `UploadStatus` agus `SubmissionStatus` in <em>entity</em>. Tá ciall agus réasún le hiad a choinneáil sa Modúl Coitianta ós go dtraschuirtear eolas faoi uaslódáil cáipéisí go minic sa bhfeidhmchlár agus teastaíonn samhail chaighdeánach ar fud an fheidhmchláir.

Cé go gcaithfear a bheith cúramach faoin gcuid is mó dá gcoinnítear sa modúl seo, tá cupla riail lámha faoinar féidir a chur sa modúl de ghnáth. Féach `exception`. Is iomaí earráid a chaitear i bhfeidhmchlár idir 'foirm nach bhfuil comhlíonta i gceart' agus fadhb inmhéanach sa bhfeidhmchlár. Coinnítear roinnt mhaith de na hearráidí a d'fheicfeadh úsáideoir anseo. Is féidir earráid a chaitheamh ar thaobh an Mhodúil Freastálaí le fógra earráide a thiubhairt don úsáideoir. A bhuí le `RESTAPIManager` de chuid `trin-react`, ní gá le haon réamhshocrú sa bhfeidhmchlár idirlín le earráid `exception` a láimhseáil. Díontar tilleadh trácht ar na rialacha lámha i dtreoirlíntí `trin-app` agus is féidir tiocht ar tuilleadh eolais ar chúrsaí trin-app in alt a fhoilseofar go luath.

### Traschur na Samhailtí
Úsáidtear `kotlinx.serialization` le haghaidh na samhailtí a srathú ar fud an fheidhmchláir. Coinníonn gach fóphacáiste `port` samhail d'orduithe a dhíontar idir an freastálaí agus an feidhmchlár idirlín. Tá sé de dhualgas ag an Modúl Feidhmchláir Idirlín agus ag an Modúl Freastálaí cén chaoi ar chóir na samhailtí a chur i bhfeidhm.

## An Modúl Feidhmchláir Idirlín
Anseo a choinnítear feidhmchlár ReactJS atá scríobtha i Kotlin-JS agus bunaithe ar an gcreat `trin-react`. Is feidhmchlár coimhéadáin simplí é seo agus tá a laghad leathanach ann; is iad sin:

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

### Foirmeacha Iarratais
Tugaim léargas dhaoibh ar na foirmeacha éagsúla atá ar fáil ar an suíomh oifigiúil BCMS: My Notices/Supporting Documents, My Notices/Statutory Documents, My Applications/Statutory Documents, My Applications/Supporting Documents agus feictear go bhfuil a macasamhail forbartha sa méid thuas. Tá na céad chuid de na foirmeacha an-chosúil le chéile agus spreagtar dhom aicme theibí a bhunú ar na trí leathanach úd. Tá an fhoirm dheireanach sách-éagsúil ó na trí cinn eile agus ní féidir é a bhunú go héasca ar an aicme theibí chéanna.

Dá réir sin, bunaím na <em>VCs</em> (.i. <em>ViewControllers</em> i mBéarla) mar a fheictear sa léaráid thíos:
<div style="text-align: center; padding-bottom: 16px;">
<img src="/images/portfolio/bcms-upload/client-app-upload-service-structure.gv.svg?raw=true" alt="Léaráid Aicmí de struchtúr na VCs." />
</div>

Ag seo léargas cruinn de na gnéithe teibí a bhaineanns le `BaseUploadServiceVC`
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

agus ag seo léargas de na gnéithe teibí a bhaineanns le `GenericUploadServiceVC` agus na feidhmeanna oidhreachta  a chuireanns sé i bhfeidhm ó `BaseUploadServiceVC`:
```
abstract val documentTypes: Array<DocumentType.Generic>

// Implemented Functions from BaseUploadServiceVC
override fun addDocumentOnClickHandler(event: Event)

override fun RBuilder.uploadOrdersRender()

override fun RBuilder.uploadsRender()

override fun RBuilder.addDocumentRender()
```

Ní fhágann sé sin ar `MyNoticesSuppDocVC`, `MyNoticesStatDocVC` agus ar `MyNoticesStatDocVC` ach `uploadDocumentsLoadOnClickHandler`, `getUploadRequests`, `getSubmission`, `executeUploadRequest`, `deleteAllUploads`, `getSubmissionStatus`, `updateSubmissionStatus` le cur i bhfeidhm.

## An Modúl Freastálaí
Roinntear an modúl freastálaí ina fhómhodúil bunaithe ar ailtireacht heicseagánach. Tá an clár http coinnithe i modúl `web-app` ar leith agus amuigh ón loighic ghnó. Coinnítear bunloighic an fheidhmchláir sa modúl `core` mar a mholanns treoirlínte ailtireacht heicseagánach ach is é an éagsúlacht go n-iompórtálann `core` an Modúl Coitianta mar aon leis an modúl `port`, modúl a rialanns na loighic sheachtracha (.i. loighic bhunachair sonraí, modúl `web-app` ⁊rl) agus a shocraíonns cén loighic ar gá le `core` idirghníonú léi. Mar thurgnamh, tugaim cead mo chinn an Modúl Coitianta a iompórtáíl i ngach aon mhodúl, fiú `core` nó go bhfuighfí amach cé mar a éireofas le struchtúr mar seo.

Maidir le `web-app`, cuirtear i bhfeidhm na haicmí atá i `port` de chuid chuile fhóphacáiste i `common`. Úsáidtear Spring Boot le haghaidh cúrsaí http ar fad.

Tá an feidhmchlár á óstáíl ag AWS - Amazon Web Services.

Ní féidir mórán eile a thrácht ar an modúl seo ar mhaithe le slándáil an fheidhmchláir.