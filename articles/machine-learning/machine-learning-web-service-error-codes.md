---
title: "aaaAzure felkoder för Machine Learning REST API | Microsoft Docs"
description: "Dessa felkoder kunde returneras av en åtgärd på en Azure Machine Learning-webbtjänst."
keywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 0923074b-3728-439d-a1b8-8a7245e39be4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 11/16/2016
ms.author: garye
ms.openlocfilehash: 9495c8ef16e684d3c8978bcd11747cf95227b091
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-rest-api-error-codes"></a>Machine Learning felkoder för REST API
 
hello kan följande felkoder returneras av en åtgärd på en Azure Machine Learning-webbtjänst.
 
## <a name="badargument-http-status-code-400"></a>BadArgument (HTTP-statuskod 400)
 
Ogiltigt argument har angetts.
 
Fel i den här klassen innebär ett argument som tillhandahålls någonstans var ogiltigt. Detta kan bero på en plats för Azure storage toosomething skickades toohello webbtjänst eller autentiseringsuppgifter. Tittar du på hello ”felkoden” fält i hello ”information” avsnittet toodiagnose vilka specifika argument var ogiltigt.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| BadParameterValue | hello parametervärdet angetts uppfyller inte hello parametern regeln hello som parameter |
| BadSubscriptionId | hello prenumerations-Id som används tooscore är inte hello finns i hello resursen en |
| BadVersionCall | Ogiltig version parameter skickades under hello API-anrop: {0}. Kontrollera hello API hjälpsida för att skicka hello rätt version och försök igen. |
| BatchJobInputsNotSpecified | hello följande obligatoriska input(s) inte har angetts i begäran hello: {0}. Kontrollera att alla indata har angetts och försök igen. |
| BatchJobInputsTooManySpecified | hello begäran specifika fler indata än definierade i hello-tjänsten. Lista över godkända input(s): {0}. Kontrollera att alla indata är rätt angivet och försök igen. |
| BlobNameTooLong | Azure blob storage sökvägen för diagnostiska utdata är för lång: {0}. Korta hello sökväg och försök igen. |
| BlobNotFound | Det går inte tooaccess hello som Azure blob - {0}.  Azure-felmeddelande: {1}. |
| ContainerIsEmpty | Inga Azure storage behållarens namn angavs. Ange ett giltigt behållarnamn och försök igen. |
| ContainerSegmentInvalid | Ogiltig behållarnamn. Ange ett giltigt behållarnamn och försök igen. |
| ContainerValidationFailed | BLOB-behållaren verifieringen misslyckades med felet: {0}. |
| DataTypeNotSupported | Typ av data som inte stöds har angetts. Ange giltig datatyperna och försök igen. |
| DuplicateInputInBatchCall | hello batch-begäran är ogiltig. Det går inte att ange både enskilda och flera indata på hello samma tid. Ta bort ett av objekten från hello begäran och försök igen. |
| ExpiryTimeInThePast | Förfallotiden som finns i tidigare hello: {0}. Ange en framtida förfallotiden i UTC och försök igen. toonever går ut kan du ange förfallodatum tid tooNULL. |
| IncompleteSettings | Diagnostikinställningar är ofullständig. |
| InputBlobRelativeLocationInvalid | Inget Azure storage blob namn har angivits. Ange ett giltigt blob-namn och försök igen. |
| InvalidBlob | Ogiltig blob-specifikationen för blob: {0}. Kontrollera att anslutningssträngen / relativ sökväg eller SAS-token specifikationen är korrekt och försök igen. |
| InvalidBlobConnectionString | Hej anslutningssträngen som angetts för en hello i/o-blobbar ogiltig: {0}. Åtgärda detta och försök igen. |
| InvalidBlobExtension | Hej blobbreferens: {0} har ett filtillägg som är ogiltig eller saknas. Filnamnstillägg som stöds för den här utdatatypen är: ”{1}”. |
| InvalidInputNames | Ogiltig indata namn anges i begäran hello: {0}. Mappa hello indata toohello rätt tjänst indata och försök igen. |
| InvalidOutputOverrideName | Ogiltig åsidosättningsnamn: {0}. hello-tjänsten har inte en output-nod med detta namn. Ange i en korrekt utdata nod namn toooverride (skiftlägeskänslighet gäller). |
| InvalidQueryParameter | Ogiltig parameter: {0}. {1} |
| MissingInputBlobInformation | Azure storage blob information saknas. Ange en giltig anslutningssträng och relativ sökväg eller URI och försök igen. |
| MissingJobId | Inget jobb-Id har angetts. Ett jobb Id returneras när ett jobb har skickats för hello första gången. Kontrollera hello jobb-Id är korrekt och försök igen. |
| MissingKeys | Inga nycklar som eller en primär eller sekundärnyckel har inte angetts. |
| MissingModelPackage | Ingen modell paket-Id eller paket i modellen. Ange en giltig modell paket-Id eller modell paketet och försök igen. |
| MissingOutputOverrideSpecification | hello-förfrågan saknar hello blob-specifikationen för utdata åsidosättning {0}. Ange en giltig blobbplats med hello begäran eller ta bort specifikationen för hello utdata om ingen plats åsidosättning önskas. |
| MissingRequestInput | hello webbtjänst indata, men inga indata har angetts. Se till att giltiga indata har angetts baserat på hello publicerade inkommande portar i hello modellen och försök igen. |
| MissingRequiredGlobalParameters | Inte alla nödvändiga web service parametrar angavs. Kontrollera hello parametrar förväntas för hello modulen eller modulerna som är korrekta och försök igen. |
| MissingRequiredOutputOverrides | När du anropar en krypterad tjänstslutpunkten är det obligatoriska toopass i åsidosätter utdata för alla hello service utdata. Saknas åsidosättningar just nu för dessa utdata: {0} |
| MissingWebServiceGroupId | Ingen web serviceavtalsgrupp-Id har angetts. Ange en giltig web serviceavtalsgrupp Id och försök igen. |
| MissingWebServiceId | Inga webbtjänster Id angetts. Ange en giltig webbtjänst Id och försök igen. |
| MissingWebServicePackage | Inga web Service-paketet som anges. Ange ett giltigt web service-paket och försök igen. |
| MissingWorkspaceId | Ingen arbetsyta Id angetts. Ange en giltig arbetsyta Id och försök igen. |
| ModelConfigurationInvalid | Ogiltig modellkonfiguration i hello modellen paketet. Kontrollera hello modellen konfigurationen innehåller utdata-slutpunkt(er) definition, std fel slutpunkt och std ut slutpunkt och försök igen. |
| ModelPackageIdInvalid | Ogiltig modell paketets Id. Kontrollera att hello modellen paket-Id är korrekt och försök igen. |
| RequestBodyInvalid | Inga begärandetexten tillhandahålls eller fel vid avserialisering hello-begäran. |
| RequestIsEmpty | Ingen förfrågan. Ange en giltig begäran och försök igen. |
| UnexpectedParameter | Oväntat parametrar som ges. Kontrollera alla parameternamn är rättstavade endast förväntade parametrar har skickats och försök igen. |
| UnknownError | Ett okänt fel. |
| UserParameterInvalid | {0} |
| WebServiceConcurrentRequestRequirementInvalid | Det går inte att ändra samtidiga begäranden krav för {0}-webbtjänsten. |
| WebServiceIdInvalid | Ogiltig webbtjänst-id angavs. Webbtjänst-id måste vara ett giltigt guid. |
| WebServiceTooManyConcurrentRequestRequirement | Det går inte att ange antal samtidiga begäranden krav toomore än {0}. |
| WebServiceTypeInvalid | Ogiltig web service-typen som anges. Kontrollera hello giltig web service-typen stämmer och försök igen. Giltig web tjänsttyper: {0}. |
 
## <a name="baduserargument-http-status-code-400"></a>BadUserArgument (HTTP-statuskod 400)
 
Ogiltigt användar-argumentet som tillhandahålls.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| InputMismatchError | Inkommande data överensstämmer inte med schemat för indataport. |
| InputParseError | Tolka inkommande vector misslyckades.  Kontrollera hello inkommande vector har hello rätt antal kolumner och -datatyper.  Ytterligare information: {0}. |
| MissingRequiredGlobalParameters | Parametrar som förväntades av hello webbtjänsten saknas. Kontrollera att alla hello krävs parametrar förväntades av hello-webbtjänsten är korrekta och försök igen. |
| UnexpectedParameter | Verifiera endast hello nödvändiga parametrar som förväntades av hello webbtjänsten skickas och försök igen. |
| UserParameterInvalid | {0} |
 
## <a name="invalidoperation-http-status-code-400"></a>InvalidOperation (HTTP-statuskod 400)
 
hello-förfrågan är ogiltig i hello aktuella kontexten.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| CannotStartJob | hello jobbet kan inte startas eftersom den är i {0}. |
| IncompatibleModel | hello modellen är inkompatibel med hello-version för begäran. hello-version för begäran stöder endast enstaka datatable utdata modeller. |
| MultipleInputsNotAllowed | hello modellen tillåter inte flera inmatningar. |
 
## <a name="libraryexecutionerror-http-status-code-400"></a>LibraryExecutionError (HTTP-statuskod 400)
 
Körningen av-modulen påträffade ett internt bibliotek för fel.
 
 
## <a name="moduleexecutionerror-http-status-code-400"></a>ModuleExecutionError (HTTP-statuskod 400)
 
Ett fel uppstod vid körningen av modulen.
 
 
## <a name="webservicepackageerror-http-status-code-400"></a>WebServicePackageError (HTTP-statuskod 400)
 
Ogiltig web service-paketet. Kontrollera hello web service-paket som angetts är korrekt och försök igen.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| FormatError | hello web service-paketet har fel format. Information: {0} |
| RuntimesError | hello web service paketet diagrammet är ogiltig. Information: {0} |
| ValidationError | hello web service paketet diagrammet är ogiltig. Information: {0} |
 
## <a name="unauthorized-http-status-code-401"></a>Obehörig (HTTP-statuskod 401)
 
Begäran är obehörig tooaccess resurs.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| AdminRequestUnauthorized | Behörighet saknas |
| ManagementRequestUnauthorized | Behörighet saknas |
| ScoreRequestUnauthorized | Ogiltiga autentiseringsuppgifter. |
 
## <a name="notfound-http-status-code-404"></a>NotFound (HTTP-statuskod 404)
 
Det gick inte att hitta resursen.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| ModelPackageNotFound | Modell-paketet hittades inte. Kontrollera hello modellen paket-Id är korrekt och försök igen. |
| WebServiceIdNotFoundInWorkspace | Webbtjänsten under den här arbetsytan gick inte att hitta. Det finns ett matchningsfel mellan hello webServiceId och hello workspaceId. Kontrollera hello-webbtjänst som är del av hello arbetsytan och försök igen. |
| WebServiceNotFound | Web service hittades inte. Kontrollera hello webbtjänst-Id är korrekt och försök igen. |
| WorkspaceNotFound | Det gick inte att hitta arbetsyta. Kontrollera hello arbetsytans Id är rätt och försök igen. |
 
## <a name="requesttimeout-http-status-code-408"></a>RequestTimeout (HTTP-statuskod 408)
 
hello-åtgärden slutfördes inte inom hello tillåtna tiden.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| RequestCanceled | Begäran avbröts av hello-klienten. |
| ScoreRequestTimeout | Körningsbegäran tidsgränsen. |
 
## <a name="conflict-http-status-code-409"></a>Konflikt (HTTP-statuskod 409)
 
Resursen finns redan.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| ModelOutputMetadataMismatch | Ogiltig parameter utdatanamn. Försök att använda hello editor modulen toorename metadatakolumner och försök igen. |
 
## <a name="memoryquotaviolation-http-status-code-413"></a>MemoryQuotaViolation (HTTP-statuskod 413)
 
hello modellen har överskridit hello minneskvoten tilldelade tooit.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| OutOfMemoryLimit | hello modellen förbrukat mer minne än vad som var används för den. Högsta tillåtna minne för hello modellen är {0} MB. Kontrollera din modell för problem. |
 
## <a name="internalerror-http-status-code-500"></a>InternalError (HTTP-statuskod 500)
 
Ett internt fel inträffade vid körning.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| AdminAuthenticationFailed |  |
| BackendArgumentError |  |
| BackendBadRequest |  |
| ClusterConfigBlobMisconfigured |  |
| ContainerProcessTerminatedWithSystemError | hello behållaren processen kraschat med systemfel |
| ContainerProcessTerminatedWithUnknownError | hello behållaren processen kraschat med ett okänt fel |
| ContainerValidationFailed | BLOB-behållaren verifieringen misslyckades med felet: {0}. |
| DeleteWebServiceResourceFailed |  |
| ExceptionDeserializationError |  |
| FailedGettingApiDocument |  |
| FailedStoringWebService |  |
| InvalidMemoryConfiguration | InvalidMemoryConfiguration, ConfigValue: {0} |
| InvalidResourceCacheConfiguration |  |
| InvalidResourceDownloadConfiguration |  |
| InvalidWebServiceResources |  |
| MissingTaskInstance | Inga argument har angetts. Kontrollera att giltiga argument har skickats och försök igen. |
| ModelPackageInvalid |  |
| ModuleExecutionFailed |  |
| ModuleLoadFailed |  |
| ModuleObjectCloneFailed |  |
| OutputConversionFailed |  |
| PortDataTypeNotSupported | Port-id = {0} har en datatyp som inte stöds: {1}. |
| ResourceDownload |  |
| ResourceLoadFailed |  |
| ServiceUrisNotFound |  |
| SwaggerGeneration | Swagger gick inte att generera, information: {0} |
| UnexpectedScoreStatus |  |
| UnknownBackendErrorResponse |  |
| UnknownError |  |
| UnknownJobStatusCode | Okänt jobb statuskoden {0}. |
| UnknownModuleError |  |
| UpdateWebServiceResourceFailed |  |
| WebServiceGroupNotFound |  |
| WebServicePackageInvalid | InvalidWebServicePackage, information: {0} |
| WorkerAuthorizationFailed |  |
| WorkerUnreachable |  |
 
## <a name="internalerrorsystemlowonmemory-http-status-code-500"></a>InternalErrorSystemLowOnMemory (HTTP-statuskod 500)
 
Ett internt fel inträffade vid körning. Systemet har ont om ledigt minne. Försök igen.
 
 
## <a name="modelpackageformaterror-http-status-code-500"></a>ModelPackageFormatError (HTTP-statuskod 500)
 
Ogiltig modell-paketet. Kontrollera hello modellen paket i är korrekt och försök igen.
 
 
## <a name="webservicepackageinternalerror-http-status-code-500"></a>WebServicePackageInternalError (HTTP-statuskod 500)
 
Ogiltig web service-paketet. Kontrollera hello webbpaket i är korrekt och försök igen.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| ModuleError | hello web service paketet diagrammet är ogiltig. Information: {0} |
 
## <a name="initializingcontainers-http-status-code-503"></a>InitializingContainers (HTTP-statuskod 503)
 
hello begäran kan inte köra som hello behållare initieras.
 
 
## <a name="serviceunavailable-http-status-code-503"></a>ServiceUnavailable (HTTP-statuskod 503)
 
Tjänsten är otillgänglig.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| NoMoreResources | Inga resurser som är tillgängliga för begäran. |
| RequestThrottled | Begäran har begränsats för slutpunkten {0}. hello maximala samtidighet för hello slutpunkten är {1}. |
| TooManyConcurrentRequests | För många samtidiga förfrågningar som skickas. |
| TooManyHostsBeingInitialized | För många värdar initieras på hello samma tid. Överväg att begränsa / försöker igen. |
| TooManyHostsBeingInitializedPerModel | För många värdar initieras på hello samma tid. Överväg att begränsa / försöker igen. |
 
## <a name="gatewaytimeout-http-status-code-504"></a>GatewayTimeout (HTTP-statuskod 504)
 
hello-åtgärden slutfördes inte inom hello tillåtna tiden.
 
| Felkod | Användarmeddelande |
| ---------- |--------------|
| BackendInitializationTimeout | initiera för hello web-tjänsten kunde inte slutföras inom hello tillåtna tiden. |
| BackendScoreTimeout | hello web service begäran körs kunde inte slutföras inom hello tillåtna tiden. |
 
