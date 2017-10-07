---
title: aaaAzure Resource Manager-providern Operations | Microsoft Docs
description: "Information om hello åtgärder som är tillgängliga på hello Microsoft Azure Resource Manager-resursprovidrar"
services: active-directory
documentationcenter: 
author: jboeshart
manager: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: jaboes
ms.openlocfilehash: 2d2f912ecbade335667d68fdc42ce03a2930a0eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Åtgärder i Azure Resource Manager-Resursprovidern

Det här dokumentet innehåller hello åtgärder som är tillgängliga för varje resursprovider för Microsoft Azure Resource Manager. Dessa kan användas i anpassade roller tooprovide detaljerade rollbaserad åtkomstkontroll (RBAC) behörigheter tooresources i Azure. Lägg märke till detta är inte en omfattande lista och åtgärder kan läggas till eller tas bort under varje provider är uppdaterad. Åtgärden strängar följer hello format för `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Använd för en omfattande och aktuella lista `Get-AzureRmProviderOperation` (i PowerShell) eller `azure provider operations show` (i Azure CLI) toolist drift av Azure-resursprovidrar.

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Åtgärd | Beskrivning |
|---|---|
|/ configuration/åtgärd|Klientkonfiguration för uppdateringar.|
|/ services/åtgärd|Uppdaterar en tjänstinstans i hello-klient.|
|/ configuration/skrivning|Skapar en klientkonfiguration.|
|/Configuration/Read|Läser hello klientkonfiguration.|
|/ services/skrivning|Skapar en instans av tjänsten i hello-klient.|
|/Services/Read|Läser hello tjänstinstanser i hello-klient.|
|/Services/delete|Tar bort en instans av tjänsten i hello-klient.|
|/Services/servicemembers/Action|Skapar en instans av tjänsten medlem i hello-tjänsten.|
|/Services/servicemembers/Read|Läser hello medlem tjänstinstansen i hello-tjänsten.|
|/Services/servicemembers/delete|Tar bort en instans av tjänsten medlem i hello-tjänsten.|
|/Services/servicemembers/Alerts/Read|Läser hello aviseringar för en medlem i tjänsten.|
|/Services/Alerts/Read|Läser hello aviseringar för en tjänst.|
|/Services/Alerts/Read|Läser hello aviseringar för en tjänst.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Åtgärd | Beskrivning |
|---|---|
|/ generateRecommendations/åtgärd|Skapar rekommendationer|
|/ suppressions/åtgärd|Skapat/uppdaterat suppressions|
|registrera/åtgärd|Registrerar hello prenumeration för hello Microsoft Advisor|
|/generateRecommendations/Read|Hämtar generera rekommendationer status|
|/recommendations/Read|Läser rekommendationer|
|/suppressions/Read|Hämtar suppressions|
|/suppressions/delete|Tar bort Undertryckning|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Åtgärd | Beskrivning |
|---|---|
|/Servers/Read|Hämtar hello information för hello angetts Analysis Server.|
|/ servrar/skrivning|Skapar eller uppdaterar hello angivna Analysis Server.|
|/Servers/delete|Tar bort hello Analysis Server.|
|/Servers/suspend/Action|Pausar hello Analysis Server.|
|/Servers/Resume/Action|Återupptar hello Analysis Server.|
|/ servrar/checkNameAvailability<br>/ Action|Kontrollerar att namnet angivna Analysis Server är giltigt och inte i användning.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Åtgärd | Beskrivning |
|---|---|
|/ checkNameAvailability/åtgärd|Kontrollerar om tjänstens namn är tillgängligt|
|registrera/åtgärd|Registrera prenumerationen för Microsoft.ApiManagement resursprovidern|
|avregistrera/åtgärd|Avregistrera prenumerationen för Microsoft.ApiManagement resursprovidern|
|/ service/skrivning|Skapa en ny instans av API Management-tjänsten|
|/Service/Read|Läsa in metadata för en instans för API Management-tjänsten|
|/Service/delete|Ta bort en instans för API Management-tjänsten|
|/Service/updatehostname/Action|Konfigurera, uppdatera eller ta bort anpassade domännamn för en API Management-tjänsten|
|/Service/uploadcertificate/Action|Ladda upp SSL-certifikat för en API Management-tjänsten|
|/Service/Backup/Action|Säkerhetskopiering API Management-tjänsten toohello angivna behållare i en användare som storage-konto|
|/Service/Restore/Action|Återställa API Management-tjänsten från hello angivna behållare i en användare har angett storage-konto|
|/Service/managedeployments/Action|Ändra SKU/enheter, Lägg till/ta bort regionala distributioner av API Management-tjänsten|
|/Service/getssotoken/Action|Hämtar SSO-token som kan använda toologin i API Management-tjänsten äldre portal som administratör|
|/Service/applynetworkconfigurationupdates/Action|Uppdateringar hello Microsoft.ApiManagement resurser som körs i virtuella nätverk toopick uppdateras nätverksinställningar.|
|/Service/operationresults/Read|Hämtar aktuell status för långvarig åtgärd|
|/Service/networkStatus/Read|Hämtar hello åtkomst nätverksstatus av resurser.|
|/Service/loggers/Read|Hämta listan över loggar tangenttryckningar eller hämta information om loggning|
|/Service/loggers/Write|Lägg till ny loggare eller uppdatera information om befintliga transaktionsloggfiler|
|/Service/loggers/delete|Ta bort befintliga transaktionsloggfiler|
|/Service/Users/Read|Hämta en lista över registrerade användare eller hämta kontoinformation för en användare|
|/Service/Users/Write|Registrera en ny användare eller uppdatera kontoinformation för en befintlig användare|
|/Service/Users/delete|Ta bort användarkonto|
|/Service/Users/generateSsoUrl/Action|Generera en URL för enkel inloggning. hello URL: en kan vara används tooaccess administrationsportal|
|/Service/Users/subscriptions/Read|Hämta listan över användare prenumerationer|
|/Service/Users/keys/Read|Hämta lista över användarnycklar|
|/Service/Users/groups/Read|Hämta lista över användargrupper|
|/Service/tenant/operationResults/Read|Hämta listan över Åtgärdsresultat eller hämta resultatet av en specifik åtgärd|
|/Service/tenant/policy/Read|Hämta konfiguration för hello-klient|
|/Service/tenant/policy/Write|Konfigurera principen för hello-klient|
|/Service/tenant/policy/delete|Ta bort konfiguration för hello-klient|
|/Service/tenant/Configuration/Save/Action|Skapar commit med konfiguration ögonblicksbild toohello angivna grenen i hello-databas|
|/Service/tenant/Configuration/Deploy/Action|Kör en uppgift för distribution tooapply ändringar från hello angivna git gren toohello konfigurationen i databasen.|
|/Service/tenant/Configuration/Validate/Action|Verifierar ändringar från hello angivna git gren|
|/Service/tenant/Configuration/operationResults/Read|Hämta listan över Åtgärdsresultat eller hämta resultatet av en specifik åtgärd|
|/Service/tenant/Configuration/syncState/Read|Hämta status för senaste git-synkronisering|
|/Service/tenant/Access/Read|Hämta klient informationsdetaljer|
|/Service/tenant/Access/Write|Uppdatera information om klienten åtkomst information|
|/Service/tenant/Access/regeneratePrimaryKey/Action|Återskapa primärnyckeln|
|/Service/tenant/Access/regenerateSecondaryKey/Action|Återskapa den sekundära åtkomstnyckeln|
|/Service/identityProviders/Read|Hämta lista över identitetsleverantörer eller hämta information av identitetsleverantören.|
|/Service/identityProviders/Write|Skapa en ny identitetsleverantör eller uppdatera information om en befintlig identitetsleverantör|
|/Service/identityProviders/delete|Ta bort den befintliga identitetsleverantören.|
|/Service/subscriptions/Read|Hämta en lista över produktprenumeration eller hämta information om produkten prenumeration|
|/Service/subscriptions/Write|Prenumerera på en befintlig användare tooan befintliga produkt eller uppdatera befintliga prenumerationsinformation. Den här åtgärden kan vara används toorenew prenumeration|
|/Service/subscriptions/delete|Ta bort prenumerationen. Den här åtgärden kan vara används toodelete prenumeration|
|/Service/subscriptions/regeneratePrimaryKey/Action|Återskapa primärnyckeln för prenumeration|
|/Service/subscriptions/regenerateSecondaryKey/Action|Återskapa en sekundär nyckel för prenumeration|
|/Service/backends/Read|Hämta listan över serverdelar eller hämta information om backend|
|/Service/backends/Write|Lägg till en ny serverdel eller uppdatera befintliga backend-information|
|/Service/backends/delete|Ta bort befintlig serverdel|
|/Service/Apis/Read|Hämta lista över alla registrerade API: er eller hämta information om API|
|/Service/Apis/Write|Skapa nya API eller uppdatera befintliga API-information|
|/Service/Apis/delete|Ta bort befintliga API|
|/Service/Apis/policy/Read|Hämta information om principen för API|
|/Service/Apis/policy/Write|Ange information om principen för API|
|/Service/Apis/policy/delete|Ta bort principkonfigurationen från API: et|
|/Service/Apis/Operations/Read|Hämta listan över befintliga API: et eller hämta information om API-åtgärd|
|/Service/Apis/Operations/Write|Skapa nya API-åtgärden eller uppdatera befintliga API-åtgärd|
|/Service/Apis/Operations/delete|Ta bort befintliga API-åtgärd|
|/Service/Apis/Operations/policy/Read|Hämta information om principen för åtgärden|
|/Service/Apis/Operations/policy/Write|Ange information om konfiguration av principen för åtgärden|
|/Service/Apis/Operations/policy/delete|Ta bort principkonfigurationen från åtgärden|
|/Service/products/Read|Hämta lista över produkter eller hämta information om produkt|
|/Service/products/Write|Skapa en ny produkt eller uppdatera befintliga produktinformation|
|/Service/products/delete|Ta bort befintliga produkten|
|/Service/products/subscriptions/Read|Hämta listan över prenumerationer för produkten|
|/Service/products/Apis/Read|Hämta listan över API: er läggs tooexisting produkten|
|/Service/products/Apis/Write|Lägga till befintliga API tooexisting produkt|
|/Service/products/Apis/delete|Ta bort befintliga API från befintliga produkten|
|/Service/products/policy/Read|Hämta konfiguration av befintlig produkt|
|/Service/products/policy/Write|Konfigurera principen för befintliga produkten|
|/Service/products/policy/delete|Ta bort principkonfigurationen från befintliga produkten|
|/Service/products/groups/Read|Hämta lista över developer grupper som är associerade med produkt|
|/Service/products/groups/Write|Associera befintlig developer grupp med befintliga produkt|
|/Service/products/groups/delete|Ta bort associationen av befintlig developer grupp med befintliga produkten|
|/Service/openidConnectProviders/Read|Hämta lista över OpenID Connect providers eller hämta information för OpenID Connect Provider|
|/Service/openidConnectProviders/Write|Skapa en ny information för OpenID Connect Provider eller uppdatering av en befintlig OpenID Connect-Provider|
|/Service/openidConnectProviders/delete|Ta bort befintliga OpenID Connect-providern|
|/Service/Certificates/Read|Hämta listan över certifikat eller hämta information om certifikat|
|/Service/Certificates/Write|Lägg till nytt certifikat|
|/Service/Certificates/delete|Ta bort befintligt certifikat|
|/Service/Properties/Read|Hämtar information om den angivna egenskapen eller hämtar en lista över alla egenskaper|
|/Service/Properties/Write|Skapar en ny egenskap eller uppdaterar värdet för den angivna egenskapen|
|/Service/Properties/delete|Tar bort befintlig egenskap|
|/Service/groups/Read|Hämta listan över grupper eller hämtar information om en grupp|
|/Service/groups/Write|Skapa ny grupp eller uppdatera befintliga gruppinformation|
|/Service/groups/delete|Ta bort en befintlig grupp|
|/Service/groups/Users/Read|Hämta lista över gruppanvändare|
|/Service/groups/Users/Write|Lägg till befintliga tooexisting användargrupp|
|/Service/groups/Users/delete|Ta bort befintliga användare från en befintlig grupp|
|/Service/authorizationServers/Read|Hämta listan över servrar för auktorisering eller hämta information om auktorisering server|
|/Service/authorizationServers/Write|Skapa en ny auktoriserings-server eller uppdatera information om en befintlig server auktorisering|
|/Service/authorizationServers/delete|Ta bort den befintliga auktorisering server|
|/Service/Reports/bySubscription/Read|Hämta rapport aggregeras av prenumerationen.|
|/Service/Reports/byRequest/Read|Hämta begäranden rapportdata|
|/Service/Reports/byOperation/Read|Hämta rapport som visar det sammanlagda av åtgärder|
|/Service/Reports/byGeo/Read|Hämta rapport sammanställs efter geografisk region|
|/Service/Reports/byUser/Read|Hämta rapport som visar det sammanlagda av utvecklare.|
|/Service/Reports/byTime/Read|Hämta rapport sammanställs efter tidsperioder|
|/Service/Reports/byApi/Read|Hämta rapport som har aggregerats med API: er|
|/Service/Reports/byProduct/Read|Hämta rapport sammanställs efter produkter.|

## <a name="microsoftappservice"></a>Microsoft.AppService

| Åtgärd | Beskrivning |
|---|---|
|/ appidentities/Läs|Returnerar hello resursen (webbplats) som är registrerad med hello Gateway.|
|/ appidentities/skrivning|Skapar en ny App-identitet.|
|/ appidentities/ta bort|Tar bort en befintlig App-identitet.|
|/deploymenttemplates/listMetadata/Action|Visar Användargränssnittet Metadata som associeras med hello API-App-paket.|
|/deploymenttemplates/Generate/Action|Returnerar en Distributionsmall tooprovision API-App-instanser.|
|/ gateways/Läs|Returnerar hello Gateway-instans.|
|gateway-skrivåtgärder|Skapar en ny Gateway eller uppdaterar befintlig.|
|/ gateways/ta bort|Tar bort en befintlig Gateway-instans.|
|/gateways/listLoginUris/Action|Fyller tokenarkiv och returnerar OAuth-inloggningen URI: er.|
|/gateways/listKeys/Action|Returnerar Gateway hemligheter.|
|/gateways/tokens/Write|Skapar en ny Zumo Token med hello förnamn.|
|/gateways/Registrations/Read|Returnerar hello resursen (webbplats) som är registrerad med hello Gateway.|
|/gateways/Registrations/Write|Registrerar en resurs (webbplats) med hello Gateway.|
|/gateways/Registrations/delete|Avregistrerar en resurs (webbplats) från hello Gateway.|
|/ apiapps/Läs|Returnerar hello API-App-instansen.|
|/ apiapps/skrivning|Skapar en ny API-App eller uppdaterar befintlig.|
|/ apiapps/ta bort|Tar bort en befintlig instans av API-App.|
|/apiapps/listStatus/Action|Returnerar status för API-App.|
|/apiapps/listKeys/Action|Returnerar API-App hemligheter.|
|/apiapps/apidefinitions/Read|Returnerar API-App API-definition.|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Åtgärd | Beskrivning |
|---|---|
|/ elevateAccess/åtgärd|Ger hello anroparen användare tillgång administratörsåtkomst hello klient definitionsområdet|
|/classicAdministrators/Read|Läser hello administratörer för hello prenumeration.|
|/ classicAdministrators/skrivning|Lägg till eller ändra administratör tooa prenumeration.|
|/classicAdministrators/delete|Tar bort Hej administratör från hello prenumeration.|
|/locks/Read|Hämtar lås i hello angetts omfång.|
|/ Lås/skrivning|Lägg till lås i hello angivna omfattningen.|
|/locks/delete|Ta bort lås i hello angetts omfång.|
|/policyAssignments/Read|Hämta information om en tilldelning av principer.|
|/ policyAssignments/skrivning|Skapa en princip för tilldelning vid hello angivna omfattningen.|
|/policyAssignments/delete|Ta bort en principtilldelning i hello angetts omfång.|
|/permissions/Read|Listar alla hello behörigheter hello anroparen har i ett givet omfång.|
|/roleDefinitions/Read|Hämta information om en rolldefinition.|
|/ roleDefinitions/skrivning|Skapa eller uppdatera en anpassad rolldefinition med angivna behörigheter och tilldelningsbara scope.|
|/roleDefinitions/delete|Ta bort hello angivna anpassade rolldefinitionen.|
|/providerOperations/Read|Hämta åtgärder för alla providrar som kan användas i rolldefinitioner.|
|/policyDefinitions/Read|Hämta information om en principdefinition.|
|/ policyDefinitions/skrivning|Skapa en anpassad princip-definition.|
|/policyDefinitions/delete|Ta bort en principdefinition.|
|/roleAssignments/Read|Hämta information om en rolltilldelning.|
|/ roleAssignments/skrivning|Skapa en roll tilldelning vid hello angivna omfattningen.|
|/roleAssignments/delete|Ta bort en rolltilldelning i hello angetts omfång.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Åtgärd | Beskrivning |
|---|---|
|/automationAccounts/Read|Hämtar ett Azure Automation-konto|
|/ automationAccounts/skrivning|Skapar eller uppdaterar en Azure Automation-konto|
|/automationAccounts/delete|Tar bort en Azure Automation-konto|
|/automationAccounts/Configurations/readContent/Action|Hämtar en Azure Automation DSC-innehåll|
|/automationAccounts/hybridRunbookWorkerGroups/Read|Läser Hybrid Runbook Worker-resurser|
|/automationAccounts/hybridRunbookWorkerGroups/delete|Tar bort Hybrid Runbook Worker-resurser|
|/automationAccounts/jobSchedules/Read|Hämtar ett Azure Automation-Jobbschema|
|/automationAccounts/jobSchedules/Write|Skapar ett Azure Automation-Jobbschema|
|/automationAccounts/jobSchedules/delete|Tar bort ett Azure Automation-Jobbschema|
|/automationAccounts/connectionTypes/Read|Hämtar en Azure Automation-anslutningstyptillgång|
|/automationAccounts/connectionTypes/Write|Skapar en Azure Automation-anslutningstyptillgång|
|/automationAccounts/connectionTypes/delete|Tar bort en Azure Automation-anslutningstyptillgång|
|/automationAccounts/Modules/Read|Hämtar en Azure Automation-modul|
|/automationAccounts/Modules/Write|Skapar eller uppdaterar en Azure Automation-modul|
|/automationAccounts/Modules/delete|Tar bort en Azure Automation-modul|
|/automationAccounts/Credentials/Read|Hämtar en Azure Automation-autentiseringsuppgiftstillgång|
|/automationAccounts/Credentials/Write|Skapar eller uppdaterar en Azure Automation-autentiseringsuppgiftstillgång|
|/automationAccounts/Credentials/delete|Tar bort en Azure Automation-autentiseringsuppgiftstillgång|
|/automationAccounts/Certificates/Read|Hämtar en Azure Automation-certifikattillgång|
|/automationAccounts/Certificates/Write|Skapar eller uppdaterar en Azure Automation-certifikattillgång|
|/automationAccounts/Certificates/delete|Tar bort en Azure Automation-certifikattillgång|
|/automationAccounts/Schedules/Read|Hämtar en Azure Automation-schematillgång|
|/automationAccounts/Schedules/Write|Skapar eller uppdaterar en Azure Automation-schematillgång|
|/automationAccounts/Schedules/delete|Tar bort en Azure Automation-schematillgång|
|/automationAccounts/jobs/Read|Hämtar en Azure Automation-jobb|
|/automationAccounts/jobs/Write|Skapar ett Azure Automation-jobb|
|/automationAccounts/jobs/stop/Action|Stoppar ett Azure Automation-jobb|
|/automationAccounts/jobs/suspend/Action|Pausar ett Azure Automation-jobb|
|/automationAccounts/jobs/Resume/Action|Återupptar ett Azure Automation-jobb|
|/automationAccounts/jobs/runbookContent/Action|Hämtar hello innehållet i hello Azure Automation-runbook när hello hello jobbkörningen|
|/automationAccounts/jobs/Output/Action|Hämtar hello utdata för ett jobb|
|/automationAccounts/jobs/Read|Hämtar en Azure Automation-jobb|
|/automationAccounts/jobs/Write|Skapar ett Azure Automation-jobb|
|/automationAccounts/jobs/stop/Action|Stoppar ett Azure Automation-jobb|
|/automationAccounts/jobs/suspend/Action|Pausar ett Azure Automation-jobb|
|/automationAccounts/jobs/Resume/Action|Återupptar ett Azure Automation-jobb|
|/automationAccounts/jobs/Streams/Read|Hämtar en Azure Automation-jobbström|
|/automationAccounts/Connections/Read|Hämtar en Azure Automation-anslutningstillgång|
|/automationAccounts/Connections/Write|Skapar eller uppdaterar en Azure Automation-anslutningstillgång|
|/automationAccounts/Connections/delete|Tar bort en Azure Automation-anslutningstillgång|
|/automationAccounts/variables/Read|Läser en Azure Automation-variabeltillgång|
|/automationAccounts/variables/Write|Skapar eller uppdaterar en Azure Automation-variabeltillgång|
|/automationAccounts/variables/delete|Tar bort en Azure Automation-variabeltillgång|
|/automationAccounts/runbooks/readContent/Action|Hämtar hello innehållet i en Azure Automation-runbook|
|/automationAccounts/runbooks/Read|Hämtar en Azure Automation-runbook|
|/automationAccounts/runbooks/Write|Skapar eller uppdaterar en Azure Automation-runbook|
|/automationAccounts/runbooks/delete|Tar bort en Azure Automation-runbook|
|/automationAccounts/runbooks/Draft/readContent/Action|Hämtar hello innehållet i ett Azure Automation-runbookutkast|
|/automationAccounts/runbooks/Draft/writeContent/Action|Skapar hello innehållet i ett Azure Automation-runbookutkast|
|/automationAccounts/runbooks/Draft/Read|Hämtar ett Azure Automation-runbookutkast|
|/automationAccounts/runbooks/Draft/publish/Action|Publicerar ett Azure Automation-runbookutkast|
|/automationAccounts/runbooks/Draft/undoEdit/Action|Ångra redigeringar tooan Azure Automation-runbookutkast|
|/automationAccounts/runbooks/Draft/testJob/Read|Hämtar en Azure Automation runbookutkast|
|/automationAccounts/runbooks/Draft/testJob/Write|Skapar en Azure Automation runbookutkast|
|/automationAccounts/runbooks/Draft/testJob/stop/Action|Stoppar ett Azure Automation runbookutkast|
|/automationAccounts/runbooks/Draft/testJob/suspend/Action|Pausar ett Azure Automation runbookutkast|
|/automationAccounts/runbooks/Draft/testJob/Resume/Action|Återupptar ett Azure Automation runbookutkast|
|/automationAccounts/webhooks/Read|Läser en Azure Automation-webhook|
|/automationAccounts/webhooks/Write|Skapar eller uppdaterar en Azure Automation-webhook|
|/automationAccounts/webhooks/delete|Tar bort en Azure Automation-webhook |
|/automationAccounts/webhooks/generateUri/Action|Genererar en URI för en Azure Automation-webhook|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

Den här providern är inte en fullständig ARM-provider och ger inte några ARM-åtgärder.

## <a name="microsoftbatch"></a>Microsoft.Batch

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar hello prenumeration för hello Batch Resursprovidern och möjliggör hello skapandet av Batch-konton|
|/ batchAccounts/skrivning|Skapar ett nytt batchkonto eller uppdaterar ett befintligt batchkonto|
|/batchAccounts/Read|Visar en lista över Batch-konton eller hämtar hello egenskaper för batchkonto|
|/batchAccounts/delete|Tar bort ett Batch-konto|
|/batchAccounts/listkeys/Action|Visar åtkomstnycklar för Batch-kontot|
|/batchAccounts/regeneratekeys/Action|Återskapar åtkomstnycklar för Batch-kontot|
|/batchAccounts/syncAutoStorageKeys/Action|Synkroniserar snabbtangenter för hello automatiskt lagringskonto har konfigurerats för Batch-kontot|
|/batchAccounts/Applications/Read|Visar program eller hämtar hello egenskaper för ett program|
|/batchAccounts/Applications/Write|Skapar ett nytt program eller uppdaterar ett befintligt program|
|/batchAccounts/Applications/delete|Tar bort ett program|
|/batchAccounts/Applications/versions/Read|Hämtar hello egenskaper för ett programpaket|
|/batchAccounts/Applications/versions/Write|Skapar ett nytt programpaket eller uppdaterar ett befintligt programpaket|
|/batchAccounts/Applications/versions/Activate/Action|Aktiverar ett programpaket|
|/batchAccounts/Applications/versions/delete|Tar bort ett programpaket|
|/Locations/Quotas/Read|Hämtar Batch-kvoter för hello angiven prenumeration på hello Azure regionen|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Åtgärd | Beskrivning |
|---|---|
|/invoices/Read|Visar tillgängliga fakturor|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Åtgärd | Beskrivning |
|---|---|
|/ mapApis/Läs|Läsåtgärd|
|/ mapApis/skrivning|Skrivåtgärd|
|/ mapApis/ta bort|Åtgärden ta bort|
|/mapApis/regenerateKey/Action|Återskapar hello nyckel|
|/mapApis/listSecrets/Action|Lista hello hemligheter|
|/mapApis/listSingleSignOnToken/Action|Läs enkel inloggning på auktorisering Token för hello resurs|
|/ Operations/läsning|Beskrivning av hello igen.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Åtgärd | Beskrivning |
|---|---|
|/ checknameavailability/åtgärd|Kontrollerar om ett namn är tillgängliga för användning med ett nytt Redis-Cache|
|registrera/åtgärd|Registrerar hello 'Microsoft.Cache' resursprovidern med en prenumeration|
|avregistrera/åtgärd|Avregistrerar hello 'Microsoft.Cache' resursprovidern med en prenumeration|
|/ redis/skrivning|Ändra inställningar och konfiguration i hanteringsportalen för hello hello Redis-Cache|
|/redis/Read|Visa inställningarna och konfigurationen av hello Redis-Cache i hanteringsportalen hello|
|/redis/delete|Ta bort hello hela Redis-Cache|
|/redis/listKeys/Action|Visa hello värdet för åtkomstnycklarna för Redis-Cache i hanteringsportalen för hello|
|/redis/regenerateKey/Action|Ändra hello värdet för åtkomstnycklarna för Redis-Cache i hanteringsportalen för hello|
|/redis/import/Action|Importera data av ett angivet format från flera blobbar till Redis|
|/redis/export/Action|Exportera Redis data tooprefixed storage-blobbar i angivet format|
|/redis/forceReboot/Action|Tvinga omstart av en cache-instans, vilket kan medföra dataförlust.|
|/redis/stop/Action|Stoppa en cache-instans.|
|/redis/Start/Action|Starta en cache-instans.|
|/redis/metricDefinitions/Read|Hämtar hello tillgängliga mått för ett Redis-Cache|
|/redis/firewallRules/Read|Hämta hello IP-brandväggsregler för ett Redis-Cache|
|/redis/firewallRules/Write|Redigera hello IP-brandväggsregler för ett Redis-Cache|
|/redis/firewallRules/delete|Ta bort IP-brandväggsregler för ett Redis-Cache|
|/redis/listUpgradeNotifications/Read|Lista hello senaste Uppgraderingsmeddelandena för cacheklienten hello.|
|/redis/linkedservers/Read|Hämta länkade servrar som är associerade med ett redis-cache.|
|/redis/linkedservers/Write|Lägg till länkad Server tooa Redis-Cache|
|/redis/linkedservers/delete|Ta bort länkad Server från ett Redis-Cache|
|/redis/patchSchedules/Read|Hämtar hello korrigering schemat för ett Redis-Cache|
|/redis/patchSchedules/Write|Ändra hello korrigering schemat för ett Redis-Cache|
|/redis/patchSchedules/delete|Ta bort hello korrigering schemat för ett Redis-Cache|

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Åtgärd | Beskrivning |
|---|---|
|/ provisionGlobalAppServicePrincipalInUserTenant/åtgärd|Etablera tjänstens huvudnamn för tjänstens huvudnamn för app|
|/ validateCertificateRegistrationInformation/åtgärd|Verifiera certifikatet köp objekt utan att skicka den|
|registrera/åtgärd|Registerresursleverantören hello Microsoft Certificates för hello prenumeration|
|/ certificateOrders/skrivning|Lägg till en ny certificateOrder eller uppdatera en befintlig|
|/ certificateOrders/ta bort|Ta bort en befintlig AppServiceCertificate|
|/ certificateOrders/Läs|Hämta hello lista över CertificateOrders|
|/certificateOrders/reissue/Action|Ange en befintlig certificateorder|
|/certificateOrders/renew/Action|Förnya ett befintligt certificateorder|
|/certificateOrders/retrieveCertificateActions/Action|Hämta hello lista över certifikat-åtgärder|
|/certificateOrders/retrieveEmailHistory/Action|Hämta certifikat e-historik|
|/certificateOrders/resendEmail/Action|Skicka e-post med certifikat|
|/certificateOrders/verifyDomainOwnership/Action|Verifiera ditt ägarskap för domänen|
|/certificateOrders/resendRequestEmails/Action|Skicka om begäran e-postmeddelanden tooanother e-postadress|
|/certificateOrders/resendRequestEmails/Action|Hämta platsen förseglingen för ett certifikat i App Service|
|/certificateOrders/Certificates/Write|Lägg till ett nytt certifikat eller uppdatera en befintlig|
|/certificateOrders/Certificates/delete|Ta bort ett befintligt certifikat|
|/certificateOrders/Certificates/Read|Hämta hello lista över certifikat|

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrera tooClassic beräkning|
|/ checkDomainNameAvailability/åtgärd|Kontrollerar hello tillgängligheten för ett visst domännamn.|
|/ moveSubscriptionResources/åtgärd|Flytta alla klassiska resurser tooa annan prenumeration.|
|/ validateSubscriptionMoveAvailability/åtgärd|Verifiera hello prenumeration tillgänglighet för klassiska move-åtgärd.|
|/operatingSystemFamilies/Read|Visar hello gäst operativsystem finns i Microsoft Azure och visar också hello versioner av operativsystemet för varje f
|/Capabilities/Read|Visar hello-funktioner|
|/operatingSystems/Read|Visar hello versioner av hello gästoperativsystem som är tillgängliga i Microsoft Azure.|
|/resourceTypes/skus/Read|Hämtar hello Sku-listan för resurstyper som stöds.|
|/domainNames/Read|Returnera hello domännamn.|
|/ domainNames/skrivning|Lägg till eller ändra hello domännamn.|
|/domainNames/delete|Ta bort hello domännamn.|
|/domainNames/Swap/Action|Växlingar hello mellanlagring fack toohello produktionsplatsen.|
|/domainNames/serviceCertificates/Read|Returnerar hello tjänstcertifikat som används.|
|/domainNames/serviceCertificates/Write|Lägg till eller ändra hello tjänstcertifikat som används.|
|/domainNames/serviceCertificates/delete|Ta bort hello tjänstcertifikat som används.|
|/domainNames/serviceCertificates/operationStatuses/Read|Läser Åtgärdsstatus för tjänstcertifikat hello domän hello.|
|/domainNames/Capabilities/Read|Visar hello domän domännamnsfunktionerna|
|/domainNames/Extensions/Read|Returnerar hello domännamnstilläggen.|
|/domainNames/Extensions/Write|Lägg till hello domännamnstilläggen.|
|/domainNames/Extensions/delete|Ta bort hello domännamnstilläggen.|
|/domainNames/Extensions/operationStatuses/Read|Läser hello hello domän domännamnstilläggens Åtgärdsstatus.|
|/domainNames/Active/Write|Anger hello aktiva domännamnet.|
|/domainNames/slots/Read|Visar hello distributionsplatser.|
|/domainNames/slots/Write|Skapar eller uppdaterar hello-distribution.|
|/domainNames/slots/delete|Tar bort en given distributionsplats.|
|/domainNames/slots/Start/Action|Startar en distributionsplats.|
|/domainNames/slots/stop/Action|Pausar hello distributionsplatsen.|
|/domainNames/slots/operationStatuses/Read|Läser hello hello domän domännamnsplatsernas Åtgärdsstatus.|
|/domainNames/slots/roles/Read|Hämta hello roll för hello distributionsplatsen.|
|/domainNames/slots/roles/extensionReferences/Read|Returnerar hello tilläggsreferensen för distributionsplatsrollen hello.|
|/domainNames/slots/roles/extensionReferences/Write|Lägg till eller ändra hello tilläggsreferensen för distributionsplatsrollen hello.|
|/domainNames/slots/roles/extensionReferences/delete|Ta bort hello tilläggsreferensen för distributionsplatsrollen hello.|
|/domainNames/slots/roles/extensionReferences/operationStatuses/Read|Läser hello Åtgärdsstatus för hello domän namn fack roller referenser.|
|/domainNames/slots/roles/roleInstances/Read|Hämta hello rollinstans.|
|/domainNames/slots/roles/roleInstances/restart/Action|Startar om rollinstanser.|
|/domainNames/slots/roles/roleInstances/reimage/Action|Reimages hello rollinstans.|
|/domainNames/slots/roles/roleInstances/operationStatuses/Read|Läser hello Åtgärdsstatus för rollinstanser för hello domän namn fack-roller.|
|/domainNames/slots/State/Start/Write|Ändringar hello distribution fack tillstånd toostopped.|
|/domainNames/slots/State/stop/Write|Ändringar hello distribution fack tillstånd toostarted.|
|/domainNames/slots/upgradeDomain/Write|Beskriver hur uppgraderingen hello domän.|
|/domainNames/internalLoadBalancers/Read|Hämtar hello interna belastningsutjämnare.|
|/domainNames/internalLoadBalancers/Write|Skapa en ny intern belastningsutjämning.|
|/domainNames/internalLoadBalancers/delete|Ta bort en ny intern belastningsutjämning.|
|/domainNames/internalLoadBalancers/operationStatuses/Read|Läser Åtgärdsstatus för hello för hello domännamnens interna belastningsutjämnare.|
|/domainNames/loadBalancedEndpointSets/Read|Visar hello belastningsutjämnade slutpunktsuppsättningarna|
|/domainNames/loadBalancedEndpointSets/operationStatuses/Read|Läser Åtgärdsstatus för hello för hello belastningsutjämnade slutpunktsuppsättningar för domännamn.|
|/domainNames/availabilitySets/Read|Visa hello tillgänglighetsuppsättning för hello resurs.|
|/Quotas/Read|Hämta hello kvoten för hello prenumeration.|
|/virtualMachines/Read|Hämtar en lista över virtuella datorer.|
|/ virtualMachines/skrivning|Lägg till eller ändra virtuella datorer.|
|/virtualMachines/delete|Tar bort virtuella datorer.|
|/virtualMachines/Start/Action|Starta hello virtuell dator.|
|/virtualMachines/redeploy/Action|Återdistribuerar hello virtuell dator.|
|/virtualMachines/restart/Action|Startar om virtuella datorer.|
|/virtualMachines/stop/Action|Stoppar hello virtuell dator.|
|/virtualMachines/Shutdown/Action|Avstängning hello virtuella datorn.|
|/virtualMachines/attachDisk/Action|Bifogar en data disk tooa virtuell dator.|
|/virtualMachines/detachDisk/Action|Tar bort en datadisk från den virtuella datorn.|
|/virtualMachines/downloadRemoteDesktopConnectionFile/Action|Hämtar hello RDP-filen för den virtuella datorn.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/läsning|Hämtar hello nätverkssäkerhetsgruppen som är associerade med hello nätverksgränssnitt.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/skrivning|Lägger till en nätverkssäkerhetsgrupp som är associerade med hello nätverksgränssnitt.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/ta bort|Tar bort hello nätverkssäkerhetsgruppen som är associerade med hello nätverksgränssnitt.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/operationStatuses/läsning|Läser Åtgärdsstatus för virtuella datorer hello hello kopplad nätverkssäkerhetsgrupper.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/Read|Hämtar hello mått definitioner.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Read|Hämta hello diagnostikinställningarna.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Write|Lägg till eller ändra diagnostikinställningarna.|
|/virtualMachines/Metrics/Read|Hämtar hello mått.|
|/virtualMachines/operationStatuses/Read|Läser Åtgärdsstatus för virtuella datorer hello hello.|
|/virtualMachines/Extensions/Read|Hämtar hello tillägg för virtuell dator.|
|/virtualMachines/Extensions/Write|Placerar hello tillägg för virtuell dator.|
|/virtualMachines/Extensions/operationStatuses/Read|Läser Åtgärdsstatus för tillägg för virtuella datorer av hello hello.|
|/virtualMachines/asyncOperations/Read|Hämtar hello möjliga asynkrona åtgärder|
|/virtualMachines/Disks/Read|Hämtar en lista över datadiskar|
|/virtualMachines/associatedNetworkSecurityGroups/Read|Hämtar hello nätverkssäkerhetsgruppen som är associerade med hello virtuell dator.|
|/virtualMachines/associatedNetworkSecurityGroups/Write|Lägger till en nätverkssäkerhetsgrupp som är associerade med hello virtuell dator.|
|/virtualMachines/associatedNetworkSecurityGroups/delete|Tar bort hello nätverkssäkerhetsgruppen som är associerade med hello virtuell dator.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/Read|Läser Åtgärdsstatus för virtuella datorer hello hello kopplad nätverkssäkerhetsgrupper.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrera tooClassic nätverk|
|/gatewaySupportedDevices/Read|Hämtar hello lista över enheter som stöds.|
|/reservedIps/Read|Hämtar hello reserverade IP-adresser|
|/ reserverade IP-adresser och skrivning|Lägga till en ny reserverad IP|
|/reservedIps/delete|Ta bort en reserverad IP.|
|/reservedIps/Link/Action|Länka en reserverad IP|
|/reservedIps/JOIN/Action|Anslut till en reserverad IP|
|/reservedIps/operationStatuses/Read|Läser Åtgärdsstatus för hello för hello reserverade IP-adresser.|
|/virtualNetworks/Read|Hämta hello virtuellt nätverk.|
|/ virtualNetworks/skrivning|Lägg till ett nytt virtuellt nätverk.|
|/virtualNetworks/delete|Tar bort hello virtuellt nätverk.|
|/virtualNetworks/peer/Action|Peerar ett virtuellt nätverk med ett annat virtuellt nätverk.|
|/virtualNetworks/JOIN/Action|Kopplar ihop hello virtuellt nätverk.|
|/virtualNetworks/checkIPAddressAvailability/Action|Kontrollerar hello tillgängligheten för en given IP-adress i ett virtuellt nätverk.|
|/virtualNetworks/Capabilities/Read|Visar hello-funktioner|
|/virtualNetworks/undernät /<br>associatedNetworkSecurityGroups/läsning|Hämtar hello nätverkssäkerhetsgruppen som är associerade med hello undernät.|
|/virtualNetworks/undernät /<br>associatedNetworkSecurityGroups/skrivning|Lägger till en nätverkssäkerhetsgrupp som är associerade med hello undernät.|
|/virtualNetworks/undernät /<br>associatedNetworkSecurityGroups/ta bort|Tar bort hello nätverkssäkerhetsgruppen som är associerade med hello undernät.|
|/virtualNetworks/undernät /<br>associatedNetworkSecurityGroups/operationStatuses/läsning|Läser hello Åtgärdsstatus för hello virtuellt undernät associeted nätverkssäkerhetsgruppen.|
|/virtualNetworks/operationStatuses/Read|Läser Åtgärdsstatus för virtuella nätverk för hello hello.|
|/virtualNetworks/gateways/Read|Hämtar hello virtuella nätverksgatewayerna.|
|/virtualNetworks/gateways/Write|Lägger till en virtuell nätverks-gateway.|
|/virtualNetworks/gateways/delete|Tar bort hello virtuell nätverksgateway.|
|/virtualNetworks/gateways/startDiagnostics/Action|Startar diagnostiken för hello virtuell nätverksgateway.|
|/virtualNetworks/gateways/stopDiagnostics/Action|Stoppar hello diagnostiken för hello virtuell nätverksgateway.|
|/virtualNetworks/gateways/downloadDiagnostics/Action|Hämtar hello gatewaydiagnostik.|
|/virtualNetworks/gateways/listCircuitServiceKey/Action|Hämtar kretstjänstnyckeln hello.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/Action|Hämtar hello konfigurationsskript för enheten.|
|/virtualNetworks/gateways/listPackage/Action|Visar hello gateway-paketet för virtuella nätverk.|
|/virtualNetworks/gateways/operationStatuses/Read|Läser Åtgärdsstatus för virtuella nätverksgatewayar för hello hello.|
|/virtualNetworks/gateways/Packages/Read|Hämtar hello gateway-paketet för virtuella nätverk.|
|/virtualNetworks/gateways/Connections/Read|Hämtar hello listan över anslutningar.|
|/virtualNetworks/gateways/Connections/Connect/Action|Ansluter en plats toosite gateway-anslutning.|
|/virtualNetworks/gateways/Connections/disconnect/Action|Kopplar från en plats toosite gateway-anslutning.|
|/virtualNetworks/gateways/Connections/test/Action|Testar en plats toosite gateway-anslutning.|
|/virtualNetworks/gateways/clientRevokedCertificates/Read|Läs hello återkallade certifikat.|
|/virtualNetworks/gateways/clientRevokedCertificates/Write|Återkallar ett klientcertifikat.|
|/virtualNetworks/gateways/clientRevokedCertificates/delete|Upphäver återkallningen av ett klientcertifikat.|
|/virtualNetworks/gateways/clientRootCertificates/Read|Hitta hello rotcertifikat för klienten.|
|/virtualNetworks/gateways/clientRootCertificates/Write|Laddar upp ett nytt klientrotscertifikat.|
|/virtualNetworks/gateways/clientRootCertificates/delete|Tar bort hello virtuellt gateway-klientcertifikat.|
|/virtualNetworks/gateways/clientRootCertificates/Download/Action|Hämtar certifikat efter tumavtryck.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/Action|Listar gatewaycertifikatspaketet för hello virtuellt nätverk.|
|/networkSecurityGroups/Read|Hämtar hello nätverkssäkerhetsgruppen.|
|/ networkSecurityGroups/skrivning|Lägger till en ny nätverkssäkerhetsgrupp.|
|/networkSecurityGroups/delete|Tar bort hello nätverkssäkerhetsgruppen.|
|/networkSecurityGroups/operationStatuses/Read|Läser hello Åtgärdsstatus för nätverkssäkerhetsgrupp hello.|
|/networkSecurityGroups/securityRules/Read|Hämtar hello säkerhetsregeln.|
|/networkSecurityGroups/securityRules/Write|Lägger till eller uppdaterar en säkerhetsregel.|
|/networkSecurityGroups/securityRules/delete|Tar bort hello säkerhetsregeln.|
|/networkSecurityGroups/securityRules/operationStatuses/Read|Läser Åtgärdsstatus för hello säkerhetsregler för nätverkssäkerhetsgrupper hello.|
|/Quotas/Read|Hämta hello kvoten för hello prenumeration.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrera tooClassic lagring|
|/ checkStorageAccountAvailability/åtgärd|Söker efter hello tillgängligheten för ett lagringskonto.|
|/Capabilities/Read|Visar hello-funktioner|
|/publicImages/Read|Hämtar hello offentliga virtuella datoravbildningen.|
|/Images/Read|Returnerar hello bilden.|
|/storageAccounts/Read|Returnera hello storage-konto med hello som anges av kontot.|
|/ storageAccounts/skrivning|Lägger till ett nytt lagringskonto.|
|/storageAccounts/delete|Ta bort hello storage-konto.|
|/storageAccounts/listKeys/Action|Visar hello snabbtangenter för hello storage-konton.|
|/storageAccounts/regenerateKey/Action|Återskapar hello befintliga åtkomstnycklar för hello storage-konto.|
|/storageAccounts/operationStatuses/Read|Läser hello Åtgärdsstatus för hello resurs.|
|/storageAccounts/images/Read|Returnerar hello lagringskontoavbildningen.|
|/storageAccounts/images/delete|Tar bort en given lagringskontoavbildning.|
|/storageAccounts/Disks/Read|Returnerar hello Lagringsdisken för kontot.|
|/storageAccounts/Disks/Write|Lägger till en lagringskontodisk.|
|/storageAccounts/Disks/delete|Tar bort angiven lagringskontodisk.|
|/storageAccounts/Disks/operationStatuses/Read|Läser hello Åtgärdsstatus för hello resurs.|
|/storageAccounts/osImages/Read|Returnerar hello operativsystemet lagringskontoavbildningen.|
|/storageAccounts/osImages/delete|Tar bort en given operativsystemsavbildning för lagringskonto.|
|/storageAccounts/Services/Read|Hämta hello tillgängliga tjänster.|
|/storageAccounts/Services/metricDefinitions/Read|Hämtar hello mått definitioner.|
|/storageAccounts/Services/Metrics/Read|Hämtar hello mått.|
|/storageAccounts/Services/diagnosticSettings/Read|Hämta hello diagnostikinställningarna.|
|/storageAccounts/Services/diagnosticSettings/Write|Lägg till eller ändra diagnostikinställningarna.|
|/Disks/Read|Returnerar hello Lagringsdisken för kontot.|
|/osImages/Read|Returnerar hello operativsystemavbildning.|
|/Quotas/Read|Hämta hello kvoten för hello prenumeration.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Åtgärd | Beskrivning |
|---|---|
|/Accounts/Read|Läser API-konton.|
|/ konton/skrivning|Skriver API-konton.|
|/Accounts/delete|Tar bort API-konton|
|/Accounts/listKeys/Action|Lista nycklar|
|/Accounts/regenerateKey/Action|Återskapa nyckeln|
|/Accounts/skus/Read|Läser tillgängliga SKU: er för en befintlig resurs.|
|/Accounts/usages/Read|Hämta hello kvotanvändningen för en befintlig resurs.|
|/ Operations/läsning|Beskrivning av hello igen.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Åtgärd | Beskrivning |
|---|---|
|/ RateCard/läsning|Returnerar erbjuder data, resurs-mätare metadata och priser för hello angivna prenumerationen.|
|/ UsageAggregates/läsning|Hämtar Microsoft Azure-förbrukningen av en prenumeration. hello innehåller mängder användningsdata, prenumeration och resurs relaterad information, på ett angivet tidsintervall.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar prenumeration med Microsoft.Compute-resursprovidern|
|/restorePointCollections/Read|Hämta hello egenskaperna för en återställning punkt samling|
|/ restorePointCollections/skrivning|Skapar en ny återställning punkt samling eller uppdaterar en befintlig|
|/restorePointCollections/delete|Tar bort hello återställning punkt insamling och befintliga återställningspunkter|
|/restorePointCollections/restorePoints/Read|Hämta hello egenskaperna för en återställningspunkt|
|/restorePointCollections/restorePoints/Write|Skapar en ny återställningspunkt|
|/restorePointCollections/restorePoints/delete|Tar bort hello återställningspunkt|
|/restorePointCollections/restorePoints/retrieveSasUris/Action|Hämta hello egenskaperna för en återställningspunkt tillsammans med blob SAS URI: er|
|/virtualMachineScaleSets/Read|Hämta hello egenskaper i en skaluppsättning för virtuell dator|
|/ virtualMachineScaleSets/skrivning|Skapar en ny skaluppsättning för virtuell dator eller uppdaterar en befintlig|
|/virtualMachineScaleSets/delete|Tar bort hello virtuella datorns skaluppsättning|
|/virtualMachineScaleSets/Start/Action|Startar hello instanser av hello virtuella datorns skaluppsättning|
|/virtualMachineScaleSets/powerOff/Action|Stänger av hello instanser av hello virtuella datorns skaluppsättning|
|/virtualMachineScaleSets/restart/Action|Startar om hello instanser av hello virtuella datorns skaluppsättning|
|/virtualMachineScaleSets/deallocate/Action|Stänger av och versioner hello beräkningsresurser för hello instanser av hello virtuella datorns skaluppsättning |
|/virtualMachineScaleSets/manualUpgrade/Action|Uppdaterar manuellt instanser toolatest modell för hello virtuella datorns skaluppsättning|
|/virtualMachineScaleSets/Scale/Action|Skala / skala ut instansantal av en befintlig virtuell dator scale set|
|/virtualMachineScaleSets/instanceView/Read|Hämtar instansvyn för hello för hello virtuella datorns skaluppsättning|
|/virtualMachineScaleSets/skus/Read|Visar hello giltiga SKU: er för en befintlig skaluppsättning för virtuell dator|
|/virtualMachineScaleSets/virtualMachines/Read|Hämtar hello egenskaperna för en virtuell dator i en Skaluppsättning|
|/virtualMachineScaleSets/virtualMachines/delete|Ta bort en viss virtuell dator i en skaluppsättning.|
|/virtualMachineScaleSets/virtualMachines/Start/Action|Startar en virtuell datorinstans i en skaluppsättning för virtuell dator.|
|/virtualMachineScaleSets/virtualMachines/powerOff/Action|Stänger av en virtuell datorinstans i en skaluppsättning för virtuell dator.|
|/virtualMachineScaleSets/virtualMachines/restart/Action|Startar om en virtuell datorinstans i en skaluppsättning för virtuell dator.|
|/virtualMachineScaleSets/virtualMachines/deallocate/Action|Stänger av och versioner hello beräkningsresurserna för en virtuell dator i en Skaluppsättning.|
|/virtualMachineScaleSets/virtualMachines/instanceView/Read|Hämtar hello instansvyn för en virtuell dator i en Skaluppsättning.|
|/Images/Read|Hämta hello egenskaper för hello bild|
|/ bilder/skrivning|Skapar en ny avbildning eller uppdaterar en befintlig|
|/Images/delete|Tar bort hello-bild|
|/Operations/Read|Visar en lista över åtgärder som är tillgängliga på Microsoft.Compute-resursprovidern|
|/Disks/Read|Hämta hello egenskaperna för en Disk|
|/ diskar/skrivning|Skapar en ny disk eller uppdaterar en befintlig|
|/Disks/delete|Tar bort hello Disk|
|/Disks/beginGetAccess/Action|Hämta hello hello Disk SAS-URI för blobåtkomst|
|/Disks/endGetAccess/Action|Återkalla hello hello Disk SAS-URI|
|/Snapshots/Read|Hämta hello egenskaperna för en ögonblicksbild|
|/ ögonblicksbilder/skrivning|Skapa en ny ögonblicksbild eller uppdatera en befintlig|
|/Snapshots/delete|Ta bort en ögonblicksbild|
|/availabilitySets/Read|Hämta hello egenskaperna för en tillgänglighetsuppsättning|
|/ availabilitySets/skrivning|Skapar en ny tillgänglighetsuppsättning eller uppdaterar en befintlig|
|/availabilitySets/delete|Tar bort hello tillgänglighetsuppsättning|
|/availabilitySets/vmSizes/Read|Lista över tillgängliga storlekar för att skapa eller uppdatera en virtuell dator i hello tillgänglighetsuppsättning|
|/virtualMachines/Read|Hämta hello egenskaper för en virtuell dator|
|/ virtualMachines/skrivning|Skapar en ny virtuell dator eller uppdaterar en befintlig|
|/virtualMachines/delete|Tar bort hello virtuell dator|
|/virtualMachines/Start/Action|Startar hello virtuell dator|
|/virtualMachines/powerOff/Action|Stänger av hello virtuell dator. Observera att hello den virtuella datorn kommer att fortsätta debiteras toobe.|
|/virtualMachines/redeploy/Action|Återdistribuerar virtuell dator|
|/virtualMachines/restart/Action|Startar om hello virtuell dator|
|/virtualMachines/deallocate/Action|Stänger av hello virtuella datorn och frigör hello beräkningsresurser|
|/virtualMachines/generalize/Action|Anger hello virtuella tillstånd tooGeneralized och förbereda hello virtuell dator för avbildning|
|/virtualMachines/Capture/Action|Samlar in hello virtuella datorn genom att kopiera virtuella hårddiskar och genererar en mall som kan använda toocreate liknande virtuella datorer|
|/virtualMachines/convertToManagedDisks/Action|Konverterar hello blob baserat diskar hello virtuella toomanaged diskar|
|/virtualMachines/vmSizes/Read|Visar en lista över tillgängliga storlekar hello virtuell dator kan uppdateras till|
|/virtualMachines/instanceView/Read|Hämtar hello detaljerad Körningsstatus för hello virtuell dator och dess resurser|
|/virtualMachines/Extensions/Read|Hämta hello egenskaper för ett tillägg för virtuell dator|
|/virtualMachines/Extensions/Write|Skapar ett nytt tillägg för virtuell dator eller uppdaterar ett befintligt|
|/virtualMachines/Extensions/delete|Tar bort hello tillägg för virtuell dator|
|/Locations/vmSizes/Read|Visar en lista över tillgängliga storlekar för virtuella datorer på en plats|
|/Locations/usages/Read|Hämtar tjänstbegränsningarna och aktuell användning kvantiteter för hello prenumerationens beräkningsresurser på en plats|
|/Locations/Operations/Read|Hämtar hello status för en asynkron åtgärd|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar hello prenumerationen för hello behållaren registret resursprovidern och gör det möjligt hello skapa behållaren register.|
|/checknameavailability/Read|Kontrollerar att registret namn är giltigt och används inte.|
|/registries/Read|Returnerar hello listan över behållare register eller hämtar hello egenskaper för hello angivna behållaren registernyckeln.|
|/ register/skrivning|Skapar en behållare registret med hello angivna parametrar eller uppdatera hello egenskaperna eller taggarna för hello angivna behållaren registernyckeln.|
|/registries/delete|Tar bort en befintlig behållare registret.|
|/registries/listCredentials/Action|Visar hello inloggningsuppgifterna för hello angivna behållaren registret.|
|/registries/regenerateCredential/Action|Återskapar hello inloggningsuppgifterna för hello angivna behållaren registret.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Åtgärd | Beskrivning |
|---|---|
|/containerServices/subscriptions/Read|Get hello angivna Behållartjänsterna utifrån prenumeration|
|/containerServices/resourceGroups/Read|Get hello angivna Behållartjänsterna utifrån resursgrupp|
|/containerServices/resourceGroups/ContainerServiceName/Read|Hämtar hello angivna Behållartjänsten|
|/containerServices/resourceGroups/ContainerServiceName/Write|Ange placeringar eller uppdateringar hello Container Service|
|/containerServices/resourceGroups/ContainerServiceName/delete|Tar bort hello angivna Behållartjänsten|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Åtgärd | Beskrivning |
|---|---|
|/ updateCommunicationPreference/åtgärd|Uppdatera inställningar för kommunikation|
|/ listCommunicationPreference/åtgärd|Lista över inställningar för kommunikation|
|/Applications/Read|Läsåtgärd|
|/ program/skrivning|Skrivåtgärd|
|/ program/skrivning|Skrivåtgärd|
|/Applications/delete|Åtgärden ta bort|
|/Applications/listSecrets/Action|Lista hemligheter|
|/Applications/listSingleSignOnToken/Action|Läsa token för enkel inloggning|
|/Operations/Read|Läs-och skrivåtgärder|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Åtgärd | Beskrivning |
|---|---|
|/hubs/Read|Läsa alla Azure kunden Insights hubb|
|/ NAV/skrivning|Skapa eller uppdatera alla Azure kunden Insights hubb|
|/hubs/delete|Ta bort alla Azure kunden Insights hubb|
|/hubs/providers/Microsoft.Insights/metricDefinitions/Read|Hämtar hello tillgängliga mått för resurs|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Read|Hämtar hello diagnostikinställningen för resursen hello|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Write|Skapar eller uppdaterar hello diagnostikinställningen för resursen hello|
|/hubs/providers/Microsoft.Insights/logDefinitions/Read|Hämtar hello tillgängliga loggar för resurs|
|/hubs/authorizationPolicies/Read|Läsa några Azure kunden insikter delad åtkomstprincip för signatur|
|/hubs/authorizationPolicies/Write|Skapa eller uppdatera princip för delad åtkomst-signatur alla Azure-kund insikter|
|/hubs/authorizationPolicies/delete|Ta bort princip för delad åtkomst-signatur alla Azure-kund insikter|
|/hubs/authorizationPolicies/regeneratePrimaryKey/Action|Återskapa en primär nyckel för Azure kunden insikter delad signatur åtkomstprincip|
|/hubs/authorizationPolicies/regenerateSecondaryKey/Action|Återskapa en sekundär nyckel för Azure kunden insikter delad signatur åtkomstprincip|
|/hubs/Profiles/Read|Läsa alla Azure-kund insikter profil|
|/hubs/Profiles/Write|Skriv valfri Azure kunden insikter profil|
|/hubs/KPI/Read|Läsa alla Azure-kund insikter Key Performance Indicator|
|/hubs/KPI/Write|Skapa eller uppdatera en Azure-kund insikter Key Performance Indicator|
|/hubs/KPI/delete|Ta bort alla Azure-kund insikter Key Performance Indicator|
|/hubs/Views/Read|Läs valfri Azure kunden insikter App vy|
|/hubs/Views/Write|Skapa eller uppdatera Azure kunden insikter App visa|
|/hubs/Views/delete|Ta bort en Azure-kund insikter App vy|
|/hubs/Interactions/Read|Läsa alla Azure kundinteraktion insikter|
|/hubs/Interactions/Write|Skapa eller uppdatera alla Azure kundinteraktion insikter|
|/hubs/roleAssignments/Read|Läs Azure kunden insikter Rbac tilldelningar|
|/hubs/roleAssignments/Write|Skapa eller uppdatera Azure kunden insikter Rbac tilldelningar|
|/hubs/roleAssignments/delete|Ta bort alla Azure-kund insikter Rbac tilldelning|
|/hubs/Connectors/Read|Läsa alla Azure kunden Insights Connector|
|/hubs/Connectors/Write|Skapa eller uppdatera alla Azure kunden Insights Connector|
|/hubs/Connectors/delete|Ta bort alla Azure kunden Insights Connector|
|/hubs/Connectors/mappings/Read|Läsa alla Azure kunden Insights Connector mappning|
|/hubs/Connectors/mappings/Write|Skapa eller uppdatera alla Azure kunden Insights Connector mappning|
|/hubs/Connectors/mappings/delete|Ta bort alla Azure kunden Insights Connector mappning|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Åtgärd | Beskrivning |
|---|---|
|/ checkNameAvailability/åtgärd|Kontrollerar tillgängligheten för katalog-namnet för klienten.|
|/Catalogs/Read|Visa egenskaper för katalogen eller katalogerna under prenumeration eller resursgrupp.|
|/ kataloger/skrivning|Skapar katalogen eller uppdateringar hello-taggar och egenskaper för hello-katalogen.|
|/Catalogs/delete|Tar bort hello katalog.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Åtgärd | Beskrivning |
|---|---|
|/datafactories/Read|Läser Data Factory.|
|/ datafactories/skrivning|Skapa eller uppdatera Data Factory|
|/datafactories/delete|Tar bort Data Factory.|
|/datafactories/datapipelines/Read|Läser Pipeline.|
|/datafactories/datapipelines/delete|Tar bort Pipeline.|
|/datafactories/datapipelines/pause/Action|Pausar Pipeline.|
|/datafactories/datapipelines/Resume/Action|Återupptar Pipeline.|
|/datafactories/datapipelines/Update/action|Pipeline för uppdateringar.|
|/datafactories/datapipelines/Write|Skapa eller uppdatera pipelinen|
|/datafactories/linkedServices/Read|Läser länkade tjänsten.|
|/datafactories/linkedServices/delete|Tar bort länkade tjänsten.|
|/datafactories/linkedServices/Write|Skapa eller uppdatera länkade tjänst|
|/datafactories/Tables/Read|Läser tabell.|
|/datafactories/Tables/delete|Tar bort tabellen.|
|/datafactories/Tables/Write|Skapa eller uppdatera tabell|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Åtgärd | Beskrivning |
|---|---|
|/Accounts/Read|Hämta information om hello DataLakeAnalytics konto.|
|/ konton/skrivning|Skapa eller uppdatera hello DataLakeAnalytics konto.|
|/Accounts/delete|Ta bort hello DataLakeAnalytics konto.|
|/Accounts/firewallRules/Read|Hämta information om en brandväggsregel.|
|/Accounts/firewallRules/Write|Skapa eller uppdatera en brandväggsregel.|
|/Accounts/firewallRules/delete|Ta bort en brandväggsregel.|
|/Accounts/storageAccounts/Read|Hämta länkade Storage-konto för hello DataLakeAnalytics konto.|
|/Accounts/storageAccounts/Write|Länka en Storage-konto toohello DataLakeAnalytics konto.|
|/Accounts/storageAccounts/delete|Avlänka ett lagringskonto från hello DataLakeAnalytics konto.|
|/Accounts/storageAccounts/containers/Read|Hämta behållare under hello Storage-konto|
|/Accounts/storageAccounts/containers/listSasTokens/Action|Lista över SAS-token för hello lagringsbehållare|
|/Accounts/dataLakeStoreAccounts/Read|Hämta länkade DataLakeStore-konto för hello DataLakeAnalytics konto.|
|/Accounts/dataLakeStoreAccounts/Write|Länka en DataLakeStore konto toohello DataLakeAnalytics konto.|
|/Accounts/dataLakeStoreAccounts/delete|Avlänka ett DataLakeStore konto från hello DataLakeAnalytics konto.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Åtgärd | Beskrivning |
|---|---|
|/Accounts/Read|Hämta information om ett sparats DataLakeStore-konto.|
|/ konton/skrivning|Skapa ett nytt konto DataLakeStore eller uppdatera ett sparats DataLakeStore-konto.|
|/Accounts/delete|Ta bort ett sparats DataLakeStore-konto.|
|/Accounts/firewallRules/Read|Hämta information om en brandväggsregel.|
|/Accounts/firewallRules/Write|Skapa eller uppdatera en brandväggsregel.|
|/Accounts/firewallRules/delete|Ta bort en brandväggsregel.|
|/Accounts/trustedIdProviders/Read|Hämta information om en betrodd identitetsleverantör.|
|/Accounts/trustedIdProviders/Write|Skapa eller uppdatera en betrodd identitetsleverantör.|
|/Accounts/trustedIdProviders/delete|Ta bort en betrodd identitetsleverantör.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrera hello prenumerationen för hello IotHub resource provider och aktiverar hello skapa IotHub resurser|
|/ checkNameAvailability/åtgärd|Kontrollera om IotHub namn är tillgängligt|
|/ användningar/Läs|Hämta prenumeration användningsinformation för den här providern.|
|/ operations/Läs|Hämta alla ResourceProvider-åtgärder|
|/ iotHubs/Läs|Hämtar hello IotHub resurser|
|/ iotHubs/skrivning|Skapa eller uppdatera IotHub resurs|
|/ iotHubs/ta bort|Ta bort resursen med IotHub|
|/iotHubs/listkeys/Action|Hämta alla IotHub-nycklar|
|/iotHubs/exportDevices/Action|Exportera enheter|
|/iotHubs/importDevices/Action|Importera enheter|
|/ IotHubs/metricDefinitions/läsning|Hämtar hello tillgängliga mått för hello IotHub-tjänsten|
|/iotHubs/iotHubKeys/listkeys/Action|Hämta IotHub Key för hello tilltalsnamn|
|/iotHubs/iotHubStats/Read|Få statistik IotHub|
|/iotHubs/quotaMetrics/Read|Hämta mätvärden kvot|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|Skapa EventHub konsumentgrupp|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|Hämta EventHub konsumenten grupper|
|/iotHubs/eventHubEndpoints/consumerGroups/delete|Ta bort EventHub konsumentgrupp|
|/iotHubs/Routing/routes/$ testall/åtgärd|Testmeddelande mot alla befintliga vägar|
|/iotHubs/Routing/routes/$ testnew/åtgärd|Testmeddelande mot ett angivna test väg|
|/ IotHubs/diagnosticSettings/läsning|Hämtar hello diagnostikinställningen för resursen hello|
|/ IotHubs/diagnosticSettings/skrivning|Skapar eller uppdaterar hello diagnostikinställningen för resursen hello|
|/iotHubs/skus/Read|Hämta giltiga IotHub Skus|
|/iotHubs/jobs/Read|Hämta jobb information som skickats på angivna IotHub|
|/iotHubs/routingEndpointsHealth/Read|Hämtar hello hälsotillståndet för alla routning slutpunkter för en IotHub|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Åtgärd | Beskrivning |
|---|---|
|/ Prenumeration/register/åtgärd|Registrerar hello prenumeration|
|/Labs/delete|Ta bort övningar.|
|/Labs/Read|Läsa övningar.|
|/ labs/skrivning|Lägg till eller ändra övningar.|
|/Labs/ListVhds/Action|Lista över tillgängliga för att skapa en anpassad avbildning diskavbildningar.|
|/Labs/GenerateUploadUri/Action|Generera en URI för uppladdning av anpassade disk bilder tooa labbet.|
|/Labs/CreateEnvironment/Action|Skapa virtuella datorer i ett labb.|
|/Labs/ClaimAnyVm/Action|Begär en slumpmässig claimable virtuell dator i labbet hello.|
|/Labs/ExportResourceUsage/Action|Export hello lab Resursanvändning i ett lagringskonto|
|/Labs/Users/delete|Ta bort användarprofiler.|
|/Labs/Users/Read|Läsa användarprofiler.|
|/Labs/Users/Write|Lägg till eller ändra användarprofiler.|
|/Labs/Users/secrets/delete|Ta bort hemligheter.|
|/Labs/Users/secrets/Read|Läsa hemligheter.|
|/Labs/Users/secrets/Write|Lägg till eller ändra hemligheter.|
|/Labs/Users/Environments/delete|Ta bort miljöer.|
|/Labs/Users/Environments/Read|Läsa miljöer.|
|/Labs/Users/Environments/Write|Lägg till eller ändra miljöer.|
|/Labs/Users/Disks/delete|Ta bort diskar.|
|/Labs/Users/Disks/Read|Läsa diskar.|
|/Labs/Users/Disks/Write|Lägg till eller ändra diskar.|
|/Labs/Users/Disks/Attach/Action|Ansluta och skapa hello lånet för hello disk toohello virtuella datorn.|
|/Labs/Users/Disks/Detach/Action|Koppla från och break hello lånet för hello disk ansluten toohello virtuella datorn.|
|/Labs/customImages/delete|Ta bort anpassade avbildningar.|
|/Labs/customImages/Read|Läsa anpassade avbildningar.|
|/Labs/customImages/Write|Lägg till eller ändra anpassade avbildningar.|
|/Labs/serviceRunners/delete|Ta bort tjänsten utlöpare.|
|/Labs/serviceRunners/Read|Läsa service utlöpare.|
|/Labs/serviceRunners/Write|Lägg till eller ändra tjänsten utlöpare.|
|/Labs/artifactSources/delete|Ta bort artefakt källor.|
|/Labs/artifactSources/Read|Läsa artefakt källor.|
|/Labs/artifactSources/Write|Lägg till eller ändra artefakt källor.|
|/Labs/artifactSources/artifacts/Read|Läsa artefakter.|
|/Labs/artifactSources/artifacts/GenerateArmTemplate/Action|Genererar en ARM-mall för hello angivna artefakt, överför hello krävs filer tooa storage-konto och validerar hello genereras artefakt.|
|/Labs/artifactSources/armTemplates/Read|Läs azure resource manager-mallar.|
|/Labs/Costs/Read|Läsa kostnader.|
|/Labs/Costs/Write|Lägg till eller ändra kostnaderna.|
|/Labs/virtualNetworks/delete|Ta bort virtuella nätverk.|
|/Labs/virtualNetworks/Read|Läsa virtuella nätverk.|
|/Labs/virtualNetworks/Write|Lägg till eller ändra virtuella nätverk.|
|/Labs/Formulas/delete|Ta bort formler.|
|/Labs/Formulas/Read|Läsa formler.|
|/Labs/Formulas/Write|Lägg till eller ändra formler.|
|/Labs/Schedules/delete|Ta bort scheman.|
|/Labs/Schedules/Read|Läsa scheman.|
|/Labs/Schedules/Write|Lägg till eller ändra scheman.|
|/Labs/Schedules/Execute/Action|Köra ett schema.|
|/Labs/Schedules/ListApplicable/Action|Visar en lista över alla tillämpliga scheman|
|/Labs/galleryImages/Read|Läsa galleriavbildningar.|
|/Labs/policySets/EvaluatePolicies/Action|Utvärderar lab princip.|
|/Labs/policySets/policies/delete|Ta bort principer.|
|/Labs/policySets/policies/Read|Läsa principer.|
|/Labs/policySets/policies/Write|Lägg till eller ändra principer.|
|/Labs/virtualMachines/delete|Ta bort virtuella datorer.|
|/Labs/virtualMachines/Read|Läsa virtuella datorer.|
|/Labs/virtualMachines/Write|Lägg till eller ändra virtuella datorer.|
|/Labs/virtualMachines/Start/Action|Starta en virtuell dator.|
|/Labs/virtualMachines/stop/Action|Stoppa en virtuell dator|
|/Labs/virtualMachines/ApplyArtifacts/Action|Tillämpa artefakter toovirtual datorn.|
|/Labs/virtualMachines/AddDataDisk/Action|Koppla en ny eller befintlig data disk toovirtual dator.|
|/Labs/virtualMachines/DetachDataDisk/Action|Koppla bort hello angiven disk från hello virtuell dator.|
|/Labs/virtualMachines/Claim/Action|Bli ägare till en befintlig virtuell dator|
|/Labs/virtualMachines/ListApplicableSchedules/Action|Visar en lista över alla tillämpliga scheman|
|/Labs/virtualMachines/Schedules/delete|Ta bort scheman.|
|/Labs/virtualMachines/Schedules/Read|Läsa scheman.|
|/Labs/virtualMachines/Schedules/Write|Lägg till eller ändra scheman.|
|/Labs/virtualMachines/Schedules/Execute/Action|Köra ett schema.|
|/Labs/notificationChannels/delete|Ta bort notificationchannels.|
|/Labs/notificationChannels/Read|Läsa notificationchannels.|
|/Labs/notificationChannels/Write|Lägg till eller ändra notificationchannels.|
|/Labs/notificationChannels/Notify/Action|Skicka tooprovided meddelandekanal.|
|/Schedules/delete|Ta bort scheman.|
|/Schedules/Read|Läsa scheman.|
|/ scheman/skrivning|Lägg till eller ändra scheman.|
|/Schedules/Execute/Action|Köra ett schema.|
|/Schedules/Retarget/Action|Uppdaterar ett schema målresurs Id.|
|/Locations/Operations/Read|Läs-och skrivåtgärder.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Åtgärd | Beskrivning |
|---|---|
|/databaseAccountNames/Read|Söker efter tillgänglighet.|
|/databaseAccounts/Read|Läser ett databaskonto.|
|/ databaseAccounts/skrivning|Uppdatera en databas-konton.|
|/databaseAccounts/listKeys/Action|Lista nycklar för ett databaskonto|
|/databaseAccounts/regenerateKey/Action|Rotera ett databaskonto nycklar|
|/databaseAccounts/listConnectionStrings/Action|Hämta hello anslutningssträngar för ett databaskonto|
|/databaseAccounts/changeResourceGroup/Action|Ändra resursgruppen för ett databaskonto|
|/databaseAccounts/failoverPriorityChange/Action|Ändra prioriteringarna för växling vid fel för områden i ett databaskonto. Detta är att använda tooperform manuell redundans|
|/databaseAccounts/delete|Tar bort hello databasen konton.|
|/databaseAccounts/metricDefinitions/Read|Läser hello databaskonto mått definitioner.|
|/databaseAccounts/Metrics/Read|Läser hello databasen konto mått.|
|/databaseAccounts/usages/Read|Läser hello databasen konto användningsområden.|
|/databaseAccounts/Databases/Collections/metricDefinitions/Read|Läser hello samlingen måttdefinitioner.|
|/databaseAccounts/Databases/Collections/Metrics/Read|Läser hello samling mått.|
|/databaseAccounts/Databases/Collections/usages/Read|Läser hello samling användningsområden.|
|/databaseAccounts/Databases/metricDefinitions/Read|Läser hello måttdefinitionerna för databasen|
|/databaseAccounts/Databases/Metrics/Read|Läser hello databasen mått.|
|/databaseAccounts/Databases/usages/Read|Läser hello databasen användningsområden.|
|/databaseAccounts/readonlykeys/Read|Läser hello databaskonto readonly nycklar.|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Åtgärd | Beskrivning |
|---|---|
|/ generateSsoRequest/åtgärd|Skapa en begäran för att logga in kontrollcenter för domänen.|
|/ validateDomainRegistrationInformation/åtgärd|Verifiera domän köp objekt utan att skicka den|
|/ checkDomainAvailability/åtgärd|Kontrollera om en domän kan köpas|
|/ listDomainRecommendations/åtgärd|Hämta hello listan domän rekommendationer baserat på nyckelord|
|registrera/åtgärd|Registerresursleverantören hello Microsoft Domains för hello prenumeration|
|/ domäner/Läs|Hämta hello lista över domäner|
|/ domäner/skrivning|Lägg till en ny domän eller uppdatera en befintlig|
|/ domäner/ta bort|Ta bort en befintlig domän.|
|/Domains/operationresults/Read|En åtgärd som domän|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Åtgärd | Beskrivning |
|---|---|
|/lcsprojects/Read|Visa Microsoft Dynamics livscykel Services-projekt som tillhör tooa användare|
|/ lcsprojects/skrivning|Skapa och uppdatera Microsoft Dynamics livscykel Services-projekt som tillhör toohello användare. Endast hello namn och beskrivning egenskaper kan uppdateras. hello-prenumeration och plats som är associerade med hello-projekt kan inte uppdateras när den har skapats|
|/lcsprojects/delete|Ta bort Microsoft Dynamics livscykel Services-projekt som tillhör toohello användare|
|/lcsprojects/clouddeployments/Read|Visa Microsoft Dynamics AX 2012 R3 utvärdering distributioner i Microsoft Dynamics livscykel Services-projekt som tillhör tooa användare|
|/lcsprojects/clouddeployments/Write|Skapa Microsoft Dynamics AX 2012 R3 utvärdering distribution i ett Microsoft Dynamics livscykel Services-projekt som tillhör tooa användare. Distributioner kan hanteras från Azure-hanteringsportalen|
|/lcsprojects/Connectors/Read|Läsa kopplingar som tillhör tooa Microsoft Dynamics livscykel Services-projekt|
|/lcsprojects/Connectors/Write|Skapa och uppdatera kopplingar som tillhör tooa Microsoft Dynamics livscykel Services-projekt|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Åtgärd | Beskrivning |
|---|---|
|/ checkNameAvailability/åtgärd|Kontrollerar tillgänglighet för namnrymder i en viss prenumeration.|
|registrera/åtgärd|Registrerar hello prenumerationen för hello EventHub-resursprovidern och gör det möjligt hello skapa EventHub-resurser|
|/ namnområden/skrivning|Skapa en resurs för Namespace och uppdatera dess egenskaper. Taggar och status för hello Namespace är hello egenskaper som kan uppdateras.|
|/Namespaces/Read|Hämta hello lista över Namespace resurs beskrivning|
|namnområden/ta bort|Ta bort Namespace-resurs|
|/Namespaces/metricDefinitions/Read|Hämta lista över Namespace mått resurs beskrivningar|
|/Namespaces/authorizationRules/Read|Hämta hello lista över auktoriseringsregler för namnområden beskrivning.|
|/Namespaces/authorizationRules/Write|Skapa en nivå auktoriseringsregler Namespace och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/authorizationRules/delete|Ta bort Namespace auktoriseringsregeln. hello standard Namespace auktoriseringsregeln kan inte tas bort. |
|/Namespaces/authorizationRules/listkeys/Action|Hämta hello anslutningssträngen toohello Namespace|
|/Namespaces/authorizationRules/regenerateKeys/Action|Återskapa hello primär eller sekundär nyckel toohello resurs|
|/Namespaces/eventhubs/Write|Skapa eller uppdatera EventHub-egenskaper.|
|/Namespaces/eventhubs/Read|Hämta lista över EventHub resurs beskrivningar|
|/Namespaces/eventhubs/delete|Åtgärden toodelete EventHub-resurs|
|/Namespaces/eventHubs/consumergroups/Write|Skapa eller uppdatera ConsumerGroup egenskaper.|
|/Namespaces/eventHubs/consumergroups/Read|Hämta lista över ConsumerGroup resurs beskrivningar|
|/Namespaces/eventHubs/consumergroups/delete|Åtgärden toodelete ConsumerGroup resurs|
|/Namespaces/eventhubs/authorizationRules/Read| Hämta hello lista över EventHub-auktoriseringsregler|
|/Namespaces/eventhubs/authorizationRules/Write|Skapa auktoriseringsregler för EventHub och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/eventhubs/authorizationRules/delete|Åtgärden toodelete EventHub-auktoriseringsregler|
|/Namespaces/eventhubs/authorizationRules/listkeys/Action|Hämta hello anslutningssträngen tooEventHub|
|/Namespaces/eventhubs/authorizationRules/regenerateKeys/Action|Återskapa hello primär eller sekundär nyckel toohello resurs|
|/Namespaces/diagnosticSettings/Read|Hämta lista över Namespace diagnostikinställningar resurs beskrivningar|
|/Namespaces/diagnosticSettings/Write|Hämta lista över Namespace diagnostikinställningar resurs beskrivningar|
|/Namespaces/logDefinitions/Read|Hämta en lista över Namespace loggar resurs beskrivningar|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Åtgärd | Beskrivning |
|---|---|
|/providers/Features/Read|Hämtar hello-funktion i en prenumeration i en given resursleverantör.|
|/providers/Features/register/Action|Registrerar hello-funktionen för en prenumeration i en given resursleverantör.|
|/Features/Read|Hämtar en prenumerations hello funktioner.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Åtgärd | Beskrivning |
|---|---|
|/ kluster och skrivning|Skapa eller uppdatera HDInsight-kluster|
|/Clusters/Read|Få information om HDInsight-kluster|
|/Clusters/delete|Ta bort ett HDInsight-kluster|
|/Clusters/changerdpsetting/Action|Ändra RDP-inställningen för HDInsight-kluster|
|/Clusters/Configurations/Action|Uppdatera konfigurationen för HDInsight-kluster|
|/Clusters/Configurations/Read|Hämta konfigurationer för HDInsight-kluster|
|/Clusters/roles/Resize/Action|Ändra storlek på ett HDInsight-kluster|
|/Locations/Capabilities/Read|Hämta prenumerationens funktioner|
|/Locations/checkNameAvailability/Read|Kontrollera namnet tillgänglighet|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar hello prenumeration för hello import/export-resursprovidern och möjliggör hello skapandet av import/export av jobb.|
|/ jobb/skrivning|Skapar ett jobb med hello angivna parametrar eller uppdatera hello egenskaperna eller taggarna för hello angivna jobbet.|
|/Jobs/Read|Hämtar hello egenskaper för hello angivna jobbet eller returnerar hello lista över jobb.|
|/Jobs/listBitLockerKeys/Action|Hämtar hello BitLocker-nycklar för hello angivna jobbet.|
|/Jobs/delete|Tar bort ett befintligt jobb.|
|/Locations/Read|Hämtar hello egenskaper för hello angiven plats eller returnerar hello lista över platser.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Åtgärd | Beskrivning |
|---|---|
|/ Register/åtgärd|Registrera hello microsoft insikter providern|
|/ AlertRules/skrivning|Skrivning tooan varningsregeln konfiguration|
|/ AlertRules/ta bort|Tar bort en varningsregelskonfiguration|
|/ AlertRules/läsning|Läser en varningsregelskonfiguration|
|/ AlertRules/aktiverat/åtgärd|Varningsregeln har aktiverats|
|/ AlertRules/löst/åtgärd|Regel för varning löst|
|/ AlertRules/begränsas/åtgärd|Varningsregeln begränsas|
|/ AlertRules/incidenter/läsning|Läser en konfiguration för varningsregelincidenter|
|/ MetricDefinitions/läsning|Läs måttdefinitionerna|
|/eventtypes/Values/Read|Läs värden för hanteringshändelsetyp|
|/eventtypes/digestevents/Read|Läs sammandrag av hanteringshändelsetyp|
|/ Mått/läsning|Läsa måtten|
|/ LogProfiles/skrivning|Skriva tooa loggen profilkonfigurationen|
|/ LogProfiles/ta bort|Ta bort konfigurationen av loggen profiler|
|/ LogProfiles/läsning|Läs loggen profiler|
|/ AutoscaleSettings/skrivning|Skriva tooan Autoskala Inställningskonfiguration|
|/ AutoscaleSettings/ta bort|Tar bort en autoskalningskonfiguration|
|/ AutoscaleSettings/läsning|Läser en autoskalningskonfiguration|
|/ AutoscaleSettings/Scaleup/åtgärd|Åtgärd för autoskalning uppåt|
|/ AutoscaleSettings/Scaledown/åtgärd|Autoskala skala ned igen|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Läs måttdefinitionerna|
|/ ActivityLogAlerts/aktiverat/åtgärd|Utlösta hello aktivitet loggen avisering|
|/ DiagnosticSettings/skrivning|Skrivning toodiagnostic Inställningskonfiguration|
|/ DiagnosticSettings/ta bort|Tar bort konfigurationen för diagnostikinställningar|
|/ DiagnosticSettings/läsning|Läser en konfiguration av diagnostikinställningar|
|/ LogDefinitions/läsning|Läs Loggdefinitioner|
|/ ExtendedDiagnosticSettings/skrivning|Skrivning tooextended diagnostikinställningar för konfiguration|
|/ ExtendedDiagnosticSettings/ta bort|Tar bort konfigurationen för utökade diagnostikinställningar|
|/ ExtendedDiagnosticSettings/läsning|Läser en utökad diagnostikinställningar-konfiguration|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar en prenumeration|
|/checkNameAvailability/Read|Kontrollera att nyckelvalvsnamnet är giltigt och inte används|
|/vaults/Read|Visa hello egenskaper för ett nyckelvalv|
|/ valv/skrivning|Skapa en ny nyckel valvet eller update hello egenskaperna för en befintlig nyckelvalv|
|/vaults/delete|Ta bort ett nyckelvalv|
|/vaults/Deploy/Action|Aktiverar åtkomst till toosecrets i en nyckelvalvet när du distribuerar Azure-resurser|
|/vaults/secrets/Read|Visa hello egenskaper för en hemlighet, men inte dess värde|
|/vaults/secrets/Write|Skapa ett nytt hemlighet eller update hello värde för en befintlig hemligt|
|/vaults/accessPolicies/Write|Uppdatera en befintlig åtkomstprincip för av sammanslagning eller ersätta eller lägga till ett nytt åtkomst princip tooa valv.|
|/deletedVaults/Read|Visa hello egenskaper för ej permanent borttagna nyckelvalv|
|/Locations/operationResults/Read|Kontrollera hello resultatet av en åtgärd på lång sikt|
|/Locations/deletedVaults/Read|Visa hello egenskaper för ett mjukt borttagna nyckelvalv|
|/Locations/deletedVaults/PURGE/Action|Rensa en ej permanent borttagna nyckelvalv|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Åtgärd | Beskrivning |
|---|---|
|/Workflows/Read|Läser hello arbetsflöde.|
|/ arbetsflöden och skrivning|Skapar eller uppdaterar hello arbetsflöde.|
|/Workflows/delete|Tar bort hello arbetsflöde.|
|/Workflows/Run/Action|Startar hello arbetsflödet körs.|
|/Workflows/disable/Action|Inaktiverar hello arbetsflöde.|
|/Workflows/enable/Action|Gör hello arbetsflöde.|
|/Workflows/Validate/Action|Verifierar hello arbetsflöde.|
|/Workflows/Move/Action|Flyttar arbetsflödet från den befintliga resursgruppen prenumerations-id, resursgruppen och/eller namnet tooa olika prenumerations-id och/eller namn.|
|/Workflows/listSwagger/Action|Hämtar hello arbetsflöde swagger definitioner.|
|/Workflows/regenerateAccessKey/Action|Återskapar hello access key hemligheter.|
|/Workflows/listCallbackUrl/Action|Hämtar hello återanrop URL för arbetsflödet.|
|/Workflows/versions/Read|Läser hello arbetsflödesversion.|
|/Workflows/versions/Triggers/listCallbackUrl/Action|Hämtar hello återanrop URL för utlösare.|
|-arbetsflöden körs/läsa|Läser hello arbetsflödet körs.|
|/Workflows/Runs/Cancel/Action|Avbryter hello körningen av ett arbetsflöde.|
|/Workflows/Runs/Actions/Read|Läser hello arbetsflöde Kör åtgärden.|
|/Workflows/Runs/Operations/Read|Läser hello arbetsflödet körs Åtgärdsstatus.|
|/Workflows/Triggers/Read|Läser hello utlösare.|
|/Workflows/Triggers/Run/Action|Kör hello utlösare.|
|/Workflows/Triggers/listCallbackUrl/Action|Hämtar hello återanrop URL för utlösare.|
|/Workflows/Triggers/histories/Read|Läser hello utlösaren historik.|
|/Workflows/Triggers/histories/resubmit/Action|Skickar hello arbetsflödet utlösare.|
|/Workflows/accessKeys/Read|Läser hello snabbtangent.|
|/Workflows/accessKeys/Write|Skapar eller uppdaterar hello snabbtangent.|
|/Workflows/accessKeys/delete|Tar bort hello snabbtangent.|
|/Workflows/accessKeys/List/Action|Visar hello access key hemligheter.|
|/Workflows/accessKeys/regenerate/Action|Återskapar hello access key hemligheter.|
|/Locations/Workflows/Validate/Action|Verifierar hello arbetsflöde.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar hello prenumerationen för hello maskininlärning web tjänstresursprovider och gör det möjligt hello skapa webbtjänster.|
|/ webServices/åtgärd|Skapa regionala egenskaper för webbtjänst för regioner som stöds|
|/commitmentPlans/Read|Läs valfri dator utbildningsplan åtagande|
|/ commitmentPlans/skrivning|Skapa eller uppdatera alla åtagande utbildningsplan datorn|
|/commitmentPlans/delete|Ta bort en dator utbildningsplan åtagande|
|/commitmentPlans/JOIN/Action|Ansluta till en dator som utbildningsplan åtagande|
|/commitmentPlans/commitmentAssociations/Read|Läsa alla Machine Learning åtagande Plan Association|
|/commitmentPlans/commitmentAssociations/Move/Action|Flytta alla Machine Learning åtagande Plan Association|
|/ Arbetsytor/läsning|Läsa alla Machine Learning arbetsytan|
|Och arbetsytor/skrivning|Skapa eller uppdatera alla Maskininlärning arbetsytan|
|/ Arbetsytor/ta bort|Ta bort alla Maskininlärning arbetsytan|
|/ Arbetsytor/listworkspacekeys/åtgärd|Lista nycklar för en Machine Learning-arbetsytan|
|/ Arbetsytor/resyncstoragekeys/åtgärd|Synkronisera nycklar för lagringskonto har konfigurerats för en Machine Learning-arbetsytan|
|/WebServices/Read|Läsa alla Machine Learning-webbtjänst|
|/ webServices/skrivning|Skapa eller uppdatera en Machine Learning-webbtjänst|
|/WebServices/delete|Ta bort alla Machine Learning-webbtjänst|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Åtgärd | Beskrivning |
|---|---|
|/agreements/OFFERS/plans/Read|Returnera ett avtal för ett givet marketplace-objekt|
|/agreements/OFFERS/plans/Sign/Action|Registrera ett avtal för ett givet marketplace-objekt|
|/agreements/OFFERS/plans/Cancel/Action|Avbryt ett avtal för ett givet marketplace-objekt|

## <a name="microsoftmedia"></a>Microsoft.Media

| Åtgärd | Beskrivning |
|---|---|
|/mediaservices/Read||
|/ mediaservices/skrivning||
|/mediaservices/delete||
|/mediaservices/regenerateKey/Action||
|/mediaservices/listKeys/Action||
|/mediaservices/syncStorageKeys/Action||

## <a name="microsoftnetwork"></a>Microsoft.Network

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar hello prenumeration|
|avregistrera/åtgärd|Avregistrerar hello prenumeration|
|/ checkTrafficManagerNameAvailability/åtgärd|Kontrollerar hello tillgängligheten för ett Traffic Manager relativt DNS-namn.|
|/dnszones/Read|Hämta hello DNS-zonen i JSON-format. hello zonegenskaperna omfattar taggar, etag, numberOfRecordSets och maxNumberOfRecordSets. Observera att det här kommandot inte hämtar hello postuppsättningar i zonen hello.|
|/ dnszones/skrivning|Skapa eller uppdatera en DNS-zon i en resursgrupp.  Använda tooupdate hello taggar på en DNS-resursposter i zonen. Observera att det här kommandot kan inte använda toocreate eller uppdatera postuppsättningar i zonen hello.|
|/dnszones/delete|Ta bort hello DNS-zonen i JSON-format. hello zonegenskaperna omfattar taggar, etag, numberOfRecordSets och maxNumberOfRecordSets.|
|/dnszones/MX/Read|Hämta hello uppsättning poster av typen MX, i JSON-format. hello postuppsättning innehåller en lista över poster samt hello TTL-värde, taggar och etag.|
|/dnszones/MX/Write|Skapa eller uppdatera en uppsättning poster av typen 'MX' inom en DNS-zon. hello-poster som anges ersätter hello aktuella posterna i uppsättningen av poster hello.|
|/dnszones/MX/delete|Ta bort hello postuppsättning för ett angivet namn och typen MX om du från en DNS-zon.|
|/dnszones/NS/Read|Hämtar uppsättning av DNS-poster av typen NS|
|/dnszones/NS/Write|Skapar eller uppdaterar DNS-postuppsättning av typen NS|
|/dnszones/NS/delete|Tar bort hello uppsättning med DNS-poster av typen NS|
|/dnszones/AAAA/Read|Hämta hello uppsättning poster av typen AAAA, i JSON-format. hello postuppsättning innehåller en lista över poster samt hello TTL-värde, taggar och etag.|
|/dnszones/AAAA/Write|Skapa eller uppdatera en uppsättning poster av typen 'AAAA' inom en DNS-zon. hello-poster som anges ersätter hello aktuella posterna i uppsättningen av poster hello.|
|/dnszones/AAAA/delete|Ta bort hello postuppsättning för ett angivet namn och typen AAAA om du från en DNS-zon.|
|/dnszones/CNAME/Read|Hämta hello uppsättning poster av typen CNAME, i JSON-format. hello postuppsättning innehåller hello TTL-värde, taggar och etag.|
|/dnszones/CNAME/Write|Skapa eller uppdatera en uppsättning poster av typen 'CNAME' inom en DNS-zon. hello-poster som anges ersätter hello aktuella posterna i uppsättningen av poster hello.|
|/dnszones/CNAME/delete|Ta bort hello postuppsättning för ett angivet namn och typen CNAME om du från en DNS-zon.|
|/dnszones/SOA/Read|Hämtar uppsättning av DNS-poster av typen SOA|
|/dnszones/SOA/Write|Skapar eller uppdaterar DNS-postuppsättning av typen SOA|
|/dnszones/SRV/Read|Hämta hello uppsättning poster av typen SRV, i JSON-format. hello postuppsättning innehåller en lista över poster samt hello TTL-värde, taggar och etag.|
|/dnszones/SRV/Write|Skapa eller uppdatera en postuppsättning av typen SRV|
|/dnszones/SRV/delete|Ta bort hello postuppsättning för ett angivet namn och typen SRV om du från en DNS-zon.|
|/dnszones/PTR/Read|Hämta hello uppsättning poster av typen PTR, i JSON-format. hello postuppsättning innehåller en lista över poster samt hello TTL-värde, taggar och etag.|
|/dnszones/PTR/Write|Skapa eller uppdatera en uppsättning poster av typen 'PTR' inom en DNS-zon. hello-poster som anges ersätter hello aktuella posterna i uppsättningen av poster hello.|
|/dnszones/PTR/delete|Ta bort hello postuppsättning för ett angivet namn och typen PTR om du från en DNS-zon.|
|/dnszones/A/Read|Hämta hello uppsättning poster av typen ”A” i JSON-format. hello postuppsättning innehåller en lista över poster samt hello TTL-värde, taggar och etag.|
|/dnszones/A/Write|Skapa eller uppdatera en uppsättning poster av typen ”A” i en DNS-zon. hello-poster som anges ersätter hello aktuella posterna i uppsättningen av poster hello.|
|/dnszones/A/delete|Ta bort hello postuppsättning för ett angivet namn och typen ”A” från en DNS-zon.|
|/dnszones/txt/Read|Hämta hello uppsättning poster av typen TXT, i JSON-format. hello postuppsättning innehåller en lista över poster samt hello TTL-värde, taggar och etag.|
|/dnszones/txt/Write|Skapa eller uppdatera en uppsättning poster av typen 'TXT' inom en DNS-zon. hello-poster som anges ersätter hello aktuella posterna i uppsättningen av poster hello.|
|/dnszones/txt/delete|Ta bort hello postuppsättning för ett angivet namn och typen TXT om du från en DNS-zon.|
|/dnszones/recordsets/Read|Hämtar DNS-postuppsättningar över typer|
|/networkInterfaces/Read|Hämtar en definition av nätverksgränssnitt. |
|/ networkInterfaces/skrivning|Skapar ett nätverksgränssnitt eller uppdaterar en befintlig nätverksgränssnitt. |
|/networkInterfaces/JOIN/Action|Ansluter till ett nätverksgränssnitt för virtuella tooa|
|/networkInterfaces/delete|Tar bort ett nätverksgränssnitt|
|/networkInterfaces/effectiveRouteTable/Action|Hämta routningstabell som konfigurerats på nätverksgränssnittet för hello Vm|
|/networkInterfaces/effectiveNetworkSecurityGroups/Action|Hämta Nätverkssäkerhetsgrupp konfigurerade på nätverket gränssnittet av hello Vm|
|/networkInterfaces/loadBalancers/Read|Hämtar alla hello belastningsutjämnare som hello nätverksgränssnittet tillhör|
|/networkInterfaces/ipconfigurations/Read|Hämtar en IP-konfigurationsdefinition av nätverksgränssnitt. |
|/publicIPAddresses/Read|Hämtar en definition av offentlig ip-adress.|
|/ publicIPAddresses/skrivning|Skapar en offentlig Ip-adress eller uppdaterar en befintlig offentlig Ip-adress. |
|/publicIPAddresses/delete|Tar bort en offentlig Ip-adress.|
|/publicIPAddresses/JOIN/Action|Ansluter till en offentlig ip-adress|
|/routeFilters/Read|Hämtar en definition av flödet filter|
|/routeFilters/JOIN/Action|Ansluter till en vägfilter|
|/routeFilters/delete|Tar bort en väg filterdefinition|
|/ routeFilters/skrivning|Skapar ett vägfilter eller uppdaterar ett befintligt rotue filter|
|/routeFilters/rules/Read|Hämtar en regel för vägen filterdefinition|
|/routeFilters/rules/Write|Skapar en regel för vägen filter eller uppdaterar en befintlig väg filter regel|
|/routeFilters/rules/delete|Tar bort en regel för vägen filterdefinition|
|/networkWatchers/Read|Hämta hello watcher nätverksdefinitionen|
|/ networkWatchers/skrivning|Skapar en nätverksbevakaren eller uppdaterar en befintlig nätverksbevakaren|
|/networkWatchers/delete|Tar bort en nätverksbevakaren|
|/networkWatchers/configureFlowLog/Action|Konfigurerar flöde loggning för en målresurs.|
|/networkWatchers/ipFlowVerify/Action|Returnerar om hello paket tillåts eller nekas tooor från en viss destination.|
|/networkWatchers/nextHop/Action|Returnera hello nexthop-typ för en angiven mål- och mål-IP-adress och nästa hoppas IP-adress.|
|/networkWatchers/queryFlowLogStatus/Action|Hämtar hello status för loggning på en resurs.|
|/networkWatchers/queryTroubleshootResult/Action|Hämtar hello felsökning resultatet från hello tidigare eller pågående felsökning igen.|
|/networkWatchers/securityGroupView/Action|Visa hello konfigurerad och effektiva regler för nätverkssäkerhetsgrupper tillämpas på en virtuell dator.|
|/networkWatchers/topology/Action|Hämtar en nätverket vy på protokollnivå för resurser och deras relationer i en resursgrupp.|
|/networkWatchers/Troubleshoot/Action|Startar felsökning på en resurs för nätverk i Azure.|
|/networkWatchers/packetCaptures/queryStatus/Action|Hämtar information om egenskaper och status för en resurs för avbildningen av paketet.|
|/networkWatchers/packetCaptures/stop/Action|Stoppa hello kör paketet avbildningssessionen.|
|/networkWatchers/packetCaptures/Read|Hämta hello paket avbilda definition|
|/networkWatchers/packetCaptures/Write|Skapar en paketinsamling|
|/networkWatchers/packetCaptures/delete|Tar bort en paketinsamling|
|/loadBalancers/Read|Hämtar en belastningsutjämnardefinition|
|/ loadBalancers/skrivning|Skapar en belastningsutjämnare eller uppdaterar en befintlig belastningsutjämnare|
|/loadBalancers/delete|Tar bort en belastningsutjämnare|
|/loadBalancers/networkInterfaces/Read|Hämtar hänvisningar tooall hello nätverksgränssnitt under en belastningsutjämnare|
|/loadBalancers/loadBalancingRules/Read|Hämtar en belastningsutjämnare belastningsutjämnardefinition av belastningsutjämningsregel|
|/loadBalancers/backendAddressPools/Read|Hämtar en load balancer definition av serverdelsadresspool|
|/loadBalancers/backendAddressPools/JOIN/Action|Ansluter till en belastningsutjämnare adress serverdelspool|
|/loadBalancers/inboundNatPools/Read|Hämtar en belastningsutjämnare definition för inkommande nat-pool|
|/loadBalancers/inboundNatPools/JOIN/Action|Ansluter till en belastningsutjämnare inkommande nat-pool|
|/loadBalancers/inboundNatRules/Read|Hämtar en belastningsutjämnare definition för inkommande nat-regel|
|/loadBalancers/inboundNatRules/Write|Skapar en inkommande nat-regel för belastningsutjämnare eller uppdaterar en befintlig inkommande nat-regel för belastningsutjämnare|
|/loadBalancers/inboundNatRules/delete|Tar bort en inkommande nat-regel för belastningsutjämnare|
|/loadBalancers/inboundNatRules/JOIN/Action|Ansluter en inkommande nat-regel för belastningsutjämnare|
|/loadBalancers/outboundNatRules/Read|Hämtar en utgående nat-regel belastningsutjämnardefinition|
|/loadBalancers/probes/Read|Hämtar en belastningsutjämningsavsökning|
|/loadBalancers/virtualMachines/Read|Hämtar hänvisningar tooall hello virtuella datorer under en belastningsutjämnare|
|/loadBalancers/frontendIPConfigurations/Read|Hämtar en klientdel IP-konfiguration belastningsutjämnardefinition|
|/trafficManagerGeographicHierarchies/Read|Hämtar hello Traffic Manager geografiska hierarki som innehåller områden som kan användas med hello geografiska trafikroutningsmetod|
|/bgpServiceCommunities/Read|Hämta Bgp Service Communities|
|/applicationGatewayAvailableWafRuleSets/Read|Hämtar tillgängliga brandvägg regeluppsättningar Programgateway|
|/virtualNetworks/Read|Hämta hello definitionen av virtuellt nätverk|
|/ virtualNetworks/skrivning|Skapar ett virtuellt nätverk eller uppdaterar ett befintligt virtuellt nätverk|
|/virtualNetworks/delete|Tar bort ett virtuellt nätverk|
|/virtualNetworks/peer/Action|Peers ett virtuellt nätverk med ett annat virtuellt nätverk|
|/virtualNetworks/virtualNetworkPeerings/Read|Hämtar en definition av peering virtuellt nätverk|
|/virtualNetworks/virtualNetworkPeerings/Write|Skapar en peering virtuellt nätverk eller uppdaterar ett befintligt virtuellt nätverk peering|
|/virtualNetworks/virtualNetworkPeerings/delete|Tar bort ett virtuellt nätverk som peering|
|/virtualNetworks/Subnets/Read|Hämtar en definition av undernät för virtuellt nätverk|
|/virtualNetworks/Subnets/Write|Skapar ett undernät för virtuellt nätverk eller uppdaterar ett befintligt undernät för virtuellt nätverk|
|/virtualNetworks/Subnets/delete|Tar bort ett undernät för virtuellt nätverk|
|/virtualNetworks/Subnets/JOIN/Action|Ansluter till ett virtuellt nätverk|
|/virtualNetworks/Subnets/joinViaServiceTunnel/Action|Ansluter till resursen som lagringskonto eller SQL-databas tooa Service tunnel undernät.|
|/virtualNetworks/Subnets/virtualMachines/Read|Hämtar hänvisningar tooall hello virtuella datorer i ett undernät för virtuellt nätverk|
|/virtualNetworks/checkIpAddressAvailability/Read|Kontrollera om Ip-adress är tillgänglig på hello angivna virtuella nätverket|
|/virtualNetworks/virtualMachines/Read|Hämtar hänvisningar tooall hello virtuella datorer i ett virtuellt nätverk|
|/expressRouteServiceProviders/Read|Hämtar Express Route-leverantörer|
|/dnsoperationresults/Read|Hämtar resultat av en DNS-åtgärd|
|/localnetworkgateways/Read|Hämtar LocalNetworkGateway|
|/ localnetworkgateways/skrivning|Skapar eller uppdaterar en befintlig LocalNetworkGateway|
|/localnetworkgateways/delete|Tar bort LocalNetworkGateway|
|/trafficManagerProfiles/Read|Få hello Traffic Manager-profilkonfiguration. Detta inkluderar DNS-inställningar, trafikdirigeringsinställningar, slutpunktsövervakningsinställningar och hello listan över slutpunkter som dirigeras av den här trafikhanterarprofilen.|
|/ trafficManagerProfiles/skrivning|Skapa en trafikhanterarprofil eller ändra hello konfigurationen av en befintlig Traffic Manager-profil. Detta inkluderar att aktivera eller inaktivera en profil och ändra DNS-inställningar, trafikdirigeringsinställningar och slutpunktsövervakningsinställningar. Slutpunkter som dirigeras av hello trafikhanterarprofilen kan läggas till, tas bort, aktiveras eller inaktiveras.|
|/trafficManagerProfiles/delete|Ta bort hello Traffic Manager-profilen. Alla inställningar som är associerade med hello Traffic Manager-profilen kommer att gå förlorade och hello profil kan inte längre används tooroute trafik.|
|/dnsoperationstatuses/Read|Hämtar status för en DNS-åtgärd |
|/Operations/Read|Hämta tillgängliga åtgärder|
|/expressRouteCircuits/Read|Hämta en ExpressRouteCircuit|
|/ expressRouteCircuits/skrivning|Skapar eller uppdaterar en befintlig ExpressRouteCircuit|
|/expressRouteCircuits/delete|Tar bort en ExpressRouteCircuit|
|/expressRouteCircuits/stats/Read|Hämtar en ExpressRouteCircuit Stat|
|/expressRouteCircuits/peerings/Read|Hämtar en ExpressRouteCircuit Peering|
|/expressRouteCircuits/peerings/Write|Skapar eller uppdaterar en befintlig ExpressRouteCircuit Peering|
|/expressRouteCircuits/peerings/delete|Tar bort en ExpressRouteCircuit Peering|
|/expressRouteCircuits/peerings/arpTables/Action|Hämtar en ExpressRouteCircuit Peering ArpTable|
|/expressRouteCircuits/peerings/routeTables/Action|Hämtar en ExpressRouteCircuit Peering Migreringstillståndet|
|/expressRouteCircuits/peerkopplingar /<br>routeTablesSummary/åtgärd|Hämtar en ExpressRouteCircuit Peering Migreringstillståndet sammanfattning|
|/expressRouteCircuits/peerings/stats/Read|Hämtar en ExpressRouteCircuit Peering Stat|
|/expressRouteCircuits/Authorizations/Read|Hämtar ExpressRouteCircuit tillstånd|
|/expressRouteCircuits/Authorizations/Write|Skapar eller uppdaterar en befintlig ExpressRouteCircuit-auktorisering|
|/expressRouteCircuits/Authorizations/delete|Tar bort ExpressRouteCircuit tillstånd|
|/Connections/Read|Hämtar VirtualNetworkGatewayConnection|
|/ anslutningar/skrivning|Skapar eller uppdaterar en befintlig VirtualNetworkGatewayConnection|
|/Connections/delete|Tar bort VirtualNetworkGatewayConnection|
|/Connections/sharedKey/Read|Hämtar VirtualNetworkGatewayConnection SharedKey|
|/Connections/sharedKey/Write|Skapar eller uppdaterar en befintlig VirtualNetworkGatewayConnection SharedKey|
|/networkSecurityGroups/Read|Hämtar en definition av nätverkssäkerhetsgrupp|
|/ networkSecurityGroups/skrivning|Skapar en nätverkssäkerhetsgrupp eller uppdaterar en befintlig nätverkssäkerhetsgrupp|
|/networkSecurityGroups/delete|Tar bort en nätverkssäkerhetsgrupp|
|/networkSecurityGroups/JOIN/Action|Ansluter en nätverkssäkerhetsgrupp|
|/networkSecurityGroups/defaultSecurityRules/Read|Hämtar en definition av säkerhetsregel standard|
|/networkSecurityGroups/securityRules/Read|Hämtar en definition av säkerhetsregel|
|/networkSecurityGroups/securityRules/Write|Skapar en säkerhetsregel eller uppdaterar en befintlig säkerhetsregel|
|/networkSecurityGroups/securityRules/delete|Tar bort en säkerhetsregel|
|/applicationGateways/Read|Hämtar en Programgateway|
|/ applicationGateways/skrivning|Skapar eller uppdaterar en Programgateway|
|/applicationGateways/delete|Tar bort en Programgateway|
|/applicationGateways/backendhealth/Action|Hämtar en programhälsan gateway backend|
|/applicationGateways/Start/Action|Startar en Programgateway|
|/applicationGateways/stop/Action|Stoppar en Programgateway|
|/applicationGateways/backendAddressPools/JOIN/Action|Ansluter en serverdelsadresspool för programmet gateway|
|/routeTables/Read|Hämtar en definition av routningstabell|
|/ routeTables/skrivning|Skapar en routningstabell eller uppdaterar en befintlig tabell rotue|
|/routeTables/delete|Tar bort en definition av routningstabell|
|/routeTables/JOIN/Action|Ansluter till en routningstabell|
|/routeTables/routes/Read|Hämtar en definition av väg|
|/routeTables/routes/Write|Skapar en väg eller uppdaterar en befintlig|
|/routeTables/routes/delete|Tar bort en väg definition|
|/Locations/operationResults/Read|Hämtar resultat av en asynkron post- eller DELETE-åtgärden|
|/Locations/checkDnsNameAvailability/Read|Kontrollerar om dns-etikett finns på hello angiven plats|
|/Locations/usages/Read|Hämtar hello resurser användningsstatistik|
|/Locations/Operations/Read|Hämtar åtgärdsresurs motsvarande status för en asynkron åtgärd|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar hello prenumeration för hello NotifciationHubs resursprovidern och möjliggör hello skapandet av namnområden och NotificationHubs|
|/ CheckNamespaceAvailability/åtgärd|Kontrollerar om ett visst Namespace-resursnamn är tillgängliga i hello NotificationHub service.|
|Och namnområden/skrivning|Skapa en resurs för Namespace och uppdatera dess egenskaper. Taggar och status för hello Namespace är hello egenskaper som kan uppdateras.|
|/ Namnområden/läsning|Hämta hello lista över Namespace resurs beskrivning|
|/ Namnområden/ta bort|Ta bort Namespace-resurs|
|/ Namnområden/authorizationRules/åtgärd|Hämta hello lista över auktoriseringsregler för namnområden beskrivning.|
|/ Namnområden/CheckNotificationHubAvailability/åtgärd|Kontrollerar huruvida ett visst NotificationHub-namn finns i en namnrymd.|
|Och namnområden/authorizationRules/skrivning|Skapa en nivå auktoriseringsregler Namespace och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/ Namnområden/authorizationRules/läsning|Hämta hello lista över auktoriseringsregler för namnområden beskrivning.|
|/ Namnområden/authorizationRules/ta bort|Ta bort Namespace auktoriseringsregeln. hello standard Namespace auktoriseringsregeln kan inte tas bort. |
|/ Namnområden/authorizationRules/listkeys/åtgärd|Hämta hello anslutningssträngen toohello Namespace|
|/ Namnområden/authorizationRules/regenerateKeys/åtgärd|Namespace auktorisering regeln återskapa primär/sekundär nyckel, ange hello nyckel som måste toobe återskapas|
|Och namnområden/NotificationHubs/skrivning|Skapa en Meddelandehubb och uppdatera dess egenskaper. Egenskaperna är främst PNS-autentiseringsuppgifter. Auktoriseringsregler och TTL-värde|
|/ Namnområden/NotificationHubs/läsning|Hämta listan över resursbeskrivningar av Notification Hub|
|/ Namnområden/NotificationHubs/ta bort|Ta bort resurs för Notification Hub|
|/ Namnområden/NotificationHubs/authorizationRules/åtgärd|Hämta hello lista över auktoriseringsregler för Notification Hub|
|/ Namnområden/NotificationHubs/pnsCredentials/åtgärd|Hämta alla Notification Hub PNS-autentiseringsuppgifter. Detta omfattar WNS, MPNS, APN, GCM och Baidu autentiseringsuppgifter|
|/ Namnområden/NotificationHubs/debugSend/åtgärd|Skicka ett push-testmeddelande.|
|/ Namnområden/NotificationHubs/metricDefinitions/läsning|Hämta lista över Namespace mått resurs beskrivningar|
|/Namespaces/NotificationHubs /<br>authorizationRules/skrivning|Skapa auktoriseringsregler för Notification Hub och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/NotificationHubs /<br>authorizationRules/läsning|Hämta hello lista över auktoriseringsregler för Notification Hub|
|/Namespaces/NotificationHubs /<br>authorizationRules/ta bort|Ta bort auktoriseringsregler för Notification Hub|
|/Namespaces/NotificationHubs /<br>authorizationRules-listkeys-åtgärden|Hämta hello anslutningssträngen toohello Notification Hub|
|/Namespaces/NotificationHubs /<br>authorizationRules-regenerateKeys-åtgärden|Notification Hub auktorisering regeln återskapa primär/sekundär nyckel, ange hello nyckel som måste toobe återskapas|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registerresursleverantören har en prenumeration tooa.|
|/linkTargets/Read|Visar en lista över befintliga konton som inte är associerade med en Azure-prenumeration. toolink den här Azure-prenumeration tooa arbetsytan, Använd kund-id som returneras av den här åtgärden i hello kunden id-egenskapen för hello skapa arbetsytan igen.|
|/ arbetsytor/skrivning|Skapar en ny arbetsyta eller länkar tooan befintlig arbetsyta genom att tillhandahålla hello kund-id från hello befintlig arbetsyta.|
|/Workspaces/Read|Hämtar en befintlig arbetsyta|
|/Workspaces/delete|Tar bort en arbetsyta. Om arbetsytan hello länkades hello tooan befintlig arbetsyta vid skapandet sedan arbetsytan var det länkade toois tas inte bort.|
|/Workspaces/generateregistrationcertificate/Action|Genererar registreringscertifikat för hello arbetsytan. Det här certifikatet är används tooconnect Microsoft System Center Operation Manager toohello arbetsytan.|
|/Workspaces/sharedKeys/Action|Hämtar hello delade nycklar för hello arbetsytan. De här nycklarna finns används tooconnect Microsoft åtgärdsinformation agenter toohello arbetsytan.|
|/Workspaces/Search/Action|Kör en sökfråga|
|/Workspaces/DataSources/Read|Hämta datakällor i en arbetsyta.|
|/Workspaces/DataSources/Write|Skapa eller uppdatera datakällor i en arbetsyta.|
|/Workspaces/DataSources/delete|Ta bort datakällor i en arbetsyta.|
|/Workspaces/managementGroups/Read|Hämtar hello namn och metadata för System Center Operations Manager management grupper anslutna toothis arbetsyta.|
|/Workspaces/schema/Read|Hämtar hello Sök schemat för hello arbetsytan.  Sök-schemat innehåller hello exponeras fält och deras typer.|
|/Workspaces/usages/Read|Hämtar användningsdata för en arbetsyta inklusive hello mängd data läses av hello-arbetsyta.|
|/Workspaces/intelligencepacks/Read|Visar en lista över alla intelligence packs som är synliga för en viss arbetsyta och visar också om hello pack är aktiverat eller inaktiverat för den arbetsytan.|
|/Workspaces/intelligencepacks/enable/Action|Gör en informationspaketet för en viss arbetsyta.|
|/Workspaces/intelligencepacks/disable/Action|Inaktiverar en informationspaketet för en viss arbetsyta.|
|/Workspaces/sharedKeys/Read|Hämtar hello delade nycklar för hello arbetsytan. De här nycklarna finns används tooconnect Microsoft åtgärdsinformation agenter toohello arbetsytan.|
|/Workspaces/savedSearches/Read|Hämtar en sparad sökning|
|/Workspaces/savedSearches/Write|Skapar en sparad sökning|
|/Workspaces/savedSearches/delete|Tar bort en sparad sökning|
|/Workspaces/storageinsightconfigs/Write|Skapar en ny konfiguration för lagring. De här konfigurationerna har använt toopull data från en plats i ett befintligt lagringskonto.|
|/Workspaces/storageinsightconfigs/Read|Hämtar en konfiguration för lagring.|
|/Workspaces/storageinsightconfigs/delete|Tar bort en konfiguration för lagring. Microsoft åtgärdsinformation stoppas från att läsa data från hello storage-konto.|
|/Workspaces/configurationScopes/Read|Hämta konfigurationsomfång|
|/Workspaces/configurationScopes/Write|Ange konfigurationsomfång|
|/Workspaces/configurationScopes/delete|Ta bort konfigurationsomfång|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registerresursleverantören har en prenumeration tooa.|
|/ lösningar/skrivning|Skapa ny OMS-lösning|
|/Solutions/Read|Hämta avslutas OMS-lösning|
|/Solutions/delete|Ta bort befintliga OMS-lösning|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Åtgärd | Beskrivning |
|---|---|
|/ Valv/backupJobsExport/åtgärd|Exportera jobben|
|Och valv/skrivning|Med skapa valv så skapas en Azure-resurs av typen valv|
|/ Valv/läsning|hello hämta valvet igen hämtar ett objekt som representerar hello Azure resurs av typen 'valvet'|
|/ Valv/ta bort|hello ta bort valvet åtgärden tar bort hello angivna Azure-resurs av typen 'valvet'|
|/ Valv/refreshContainers/läsning|Uppdaterar hello behållaren lista|
|/ Valv/backupJobsExport/operationResults/läsning|Returnerar hello resultatet av exportera Job-åtgärden.|
|/ Valv/backupOperationResults/läsning|Returnerar resultat från säkerhetskopiering för Recovery Services-valvet.|
|/ Valv/monitoringAlerts/läsning|Hämtar hello aviseringar för hello Recovery services-valvet.|
|/Vaults/monitoringAlerts / {uniqueAlertId} / läsa|Hämtar hello information om hello avisering.|
|/ Valv/backupSecurityPIN/läsning|Returnerar säkerhet PIN-kod Information för Recovery Services-valvet.|
|/vaults/replicationEvents/Read|Läsa alla händelser|
|/ Valv/backupProtectableItems/läsning|Returnerar lista över alla objekt som ska skyddas.|
|/vaults/replicationFabrics/Read|Läsa alla Infrastrukturresurser|
|/vaults/replicationFabrics/Write|Skapa eller uppdatera alla Infrastrukturresurser|
|/vaults/replicationFabrics/Remove/Action|Ta bort Fabric|
|/vaults/replicationFabrics/checkConsistency/Action|Kontrollerar konsekvenskontroll av hello Fabric|
|/vaults/replicationFabrics/delete|Ta bort alla Infrastrukturresurser|
|/vaults/replicationFabrics/renewcertificate/Action||
|/vaults/replicationFabrics/deployProcessServerImage/Action|Distribuera Processerveravbildning|
|/vaults/replicationFabrics/reassociateGateway/Action|Skapa en ny koppling Gateway|
|/ valv/replicationFabrics/replicationRecoveryServicesProviders /<br>Läs|Läsa alla Recovery Services-Providers|
|/ valv/replicationFabrics/replicationRecoveryServicesProviders /<br>ta bort/åtgärd|Ta bort Recovery Services-Provider|
|/ valv/replicationFabrics/replicationRecoveryServicesProviders /<br>ta bort|Ta bort alla Recovery Services-Providers|
|/ valv/replicationFabrics/replicationRecoveryServicesProviders /<br>refreshProvider/åtgärd|Uppdatera providern|
|/vaults/replicationFabrics/replicationStorageClassifications/Read|Läsa alla Lagringsklassificeringar|
|/ valv/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/läsning|Läsa alla mappningar för klassificering av lagring|
|/ valv/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/skrivning|Skapa eller uppdatera alla mappningar för klassificering av lagring|
|/ valv/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/ta bort|Ta bort alla mappningar för klassificering av lagring|
|/vaults/replicationFabrics/replicationvCenters/Read|Läsa alla jobb|
|/vaults/replicationFabrics/replicationvCenters/Write|Skapa eller uppdatera jobb|
|/vaults/replicationFabrics/replicationvCenters/delete|Ta bort alla jobb|
|/vaults/replicationFabrics/replicationNetworks/Read|Läsa alla nätverk|
|/ valv/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/läsning|Läsa alla nätverksmappningar|
|/ valv/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/skrivning|Skapa eller uppdatera alla nätverksmappningar|
|/ valv/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/ta bort|Ta bort alla nätverksmappningar|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>Läs|Läsa skydd behållare|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>discoverProtectableItem/åtgärd|Identifiera skyddsobjekt|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>skriva|Skapa eller uppdatera Protection behållare|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>ta bort/åtgärd|Ta bort Skyddsbehållaren|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>switchprotection/åtgärd|Växeln-Skyddsbehållaren|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectableItems/läsning|Läsa alla objekt som kan skyddas|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/läsning|Läsa alla mappningar för behållaren skydd|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/skrivning|Skapa eller uppdatera alla mappningar för behållaren skydd|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings-remove-åtgärden|Ta bort skyddsbehållare|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/ta bort|Ta bort alla mappningar för behållaren skydd|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/läsning|Läsa alla skyddade objekt|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/skrivning|Skapa eller uppdatera alla skyddade objekt|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/ta bort|Ta bort alla skyddade objekt|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-remove-åtgärden|Ta bort skyddade objekt|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-plannedFailover-åtgärden|Planerad redundans|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-unplannedFailover-åtgärden|Redundans|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-testFailover-åtgärden|Testa redundans|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-testFailoverCleanup-åtgärden|Redundanstestningen|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-failoverCommit-åtgärden|Redundansåtgärd|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/skyddar/åtgärd|Skapa nytt skyddat objekt|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-updateMobilityService-åtgärden|Uppdatera Mobilitetstjänsten|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-repairReplication-åtgärden|Reparera replikering|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems-applyRecoveryPoint-åtgärden|Tillämpa återställningspunkt|
|/ valv/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/recoveryPoints/läsning|Läsa alla återställningspunkter för replikeringen|
|/vaults/replicationPolicies/Read|Läsa alla principer|
|/vaults/replicationPolicies/Write|Skapa eller uppdatera alla principer|
|/vaults/replicationPolicies/delete|Ta bort alla principer|
|/vaults/replicationRecoveryPlans/Read|Läsa alla Återställningsplaner|
|/vaults/replicationRecoveryPlans/Write|Skapa eller uppdatera alla Återställningsplaner|
|/vaults/replicationRecoveryPlans/delete|Ta bort alla Återställningsplaner|
|/vaults/replicationRecoveryPlans/plannedFailover/Action|Planerad redundans återställningsplan|
|/vaults/replicationRecoveryPlans/unplannedFailover/Action|Plan för växling vid fel|
|/vaults/replicationRecoveryPlans/testFailover/Action|Testa redundans återställningsplan|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/Action|Testa redundans Rensa återställningsplan|
|/vaults/replicationRecoveryPlans/failoverCommit/Action|Redundansåtgärd återställningsplan|
|/vaults/replicationRecoveryPlans/reProtect/Action|Skapa nytt återställningsplan|
|/ Valv/extendedInformation/läsning|hello Visa utökad Info åtgärden hämtar ett objekt utökad information som representerar hello Azure resurs av typen? valvet?|
|Och valv/extendedInformation/skrivning|hello Visa utökad Info åtgärden hämtar ett objekt utökad information som representerar hello Azure resurs av typen? valvet?|
|/ Valv/extendedInformation/ta bort|hello Visa utökad Info åtgärden hämtar ett objekt utökad information som representerar hello Azure resurs av typen? valvet?|
|/ Valv/backupManagementMetaData/läsning|Returnerar metadata för hantering av säkerhetskopiering för Recovery Services-valvet.|
|/ Valv/backupProtectionContainers/läsning|Returnerar alla behållare som tillhör toohello prenumeration|
|/ Valv/backupFabrics/operationResults/läsning|Returnerar status för hello åtgärd|
|/ Valv/backupFabrics/protectionContainers/läsning|Returnerar alla registrerade behållare|
|/ Valv/backupFabrics/protectionContainers /<br>operationResults/läsning|Hämtar resultat från utförd åtgärd på skyddsbehållare.|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/läsning|Returnerar objekt information om hello skyddat objekt|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/skrivning|Skapa en säkerhetskopiera skyddade objekt|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/ta bort|Tar bort skyddade objekt|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems-säkerhetskopia-åtgärden|Säkerhetskopierar ett skyddat objekt.|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/operationResults/läsning|Hämtar resultat från utförd åtgärd på skyddade objekt.|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/operationStatus/läsning|Returnerar hello status för åtgärden utförs på skyddade objekt.|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints/läsning|Hämta återställningspunkter för skyddade objekt.|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>åtgärden och Återställ|Återskapa återställningspunkter för skyddade objekt.|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>provisionInstantItemRecovery/åtgärd|Etablera omedelbar återställning för skyddade objekt|
|/ Valv/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>revokeInstantItemRecovery/åtgärd|Återkalla omedelbar återställning för skyddade objekt|
|/ Valv/användningar/läsning|Returnerar användningsinformation om Recovery Services-valvet.|
|/vaults/usages/Read|Läsa alla valvet användningsområden|
|Och valv/certifikat/skrivning|hello Update resurs certifikat åtgärden uppdaterar hello Autentiseringscertifikatet för resurs-valvet.|
|/ Valv/tokenInfo/läsning|Returnerar tokeninformation för Recovery Services-valvet.|
|/vaults/replicationAlertSettings/Read|Läsa alla aviseringsinställningar|
|/vaults/replicationAlertSettings/Write|Skapa eller uppdatera alla aviseringsinställningar|
|/ Valv/backupOperations/läsning|Returnerar Säkerhetskopieringsåtgärden Status för Recovery Services-valvet.|
|/ Valv/storageConfig/läsning|Returnerar lagringskonfigurationen för Recovery Services-valvet.|
|Och valv/storageConfig/skrivning|Konfiguration för lagring av uppdateringar för Recovery Services-valvet.|
|/ Valv/backupUsageSummaries/läsning|Returnerar sammanfattningar för skyddade objekt och skyddade servrar för en Recovery Services.|
|/ Valv/backupProtectedItems/läsning|Returnerar hello lista över alla skyddade objekt.|
|/ Valv/backupconfig/vaultconfig/läsning|Returnerar konfigurationen för Recovery Services-valvet.|
|Och valv/backupconfig/vaultconfig/skrivning|Uppdateringar konfigurationen för Recovery Services-valvet.|
|Och valv/registeredIdentities/skrivning|hello registrera tjänstbehållaren åtgärden kan vara används tooregister en behållare med återställningstjänsten.|
|/ Valv/registeredIdentities/läsning|hello hämta behållare åtgärden kan användas för att hämta hello-behållare som registrerats för en resurs.|
|/ Valv/registeredIdentities/ta bort|hello avregistrera behållaren åtgärden kan vara används toounregister en behållare.|
|/ Valv/registeredIdentities/operationResults/läsning|hello åtgärden Hämta resultaten åtgärden kan använda get hello Åtgärdsstatus och resulterar för hello asynkront har skickats igen|
|/vaults/replicationJobs/Read|Läsa alla jobb|
|/vaults/replicationJobs/Cancel/Action|Avbryta jobb|
|/vaults/replicationJobs/restart/Action|Starta om jobbet|
|/vaults/replicationJobs/Resume/Action|Återuppta jobbet|
|/ Valv/backupPolicies/läsning|Returnerar alla Protection-principer|
|Och valv/backupPolicies/skrivning|Skapar Protection-principen|
|/ Valv/backupPolicies/ta bort|Ta bort en Skyddsprincip för|
|/ Valv/backupPolicies/operationResults/läsning|Hämtar resultat från principåtgärd.|
|/ Valv/backupPolicies/operationStatus/läsning|Hämta Status för princip-åtgärd.|
|/ Valv/vaultTokens/läsning|hello valvet Token åtgärden kan vara används tooget valvet Token för valvet nivån backend-åtgärder.|
|/ Valv/monitoringConfigurations/notificationConfiguration/läsning|Hämtar meddelandekonfigurationen för hello Recovery services-valvet.|
|/ Valv/backupJobs/läsning|Returnerar alla jobbobjekt|
|/ Valv/backupJobs / / avbrottsåtgärd|Avbryt hello jobb|
|/ Valv/backupJobs/operationResults/läsning|Returnerar hello resultatet av jobbet igen.|
|/Locations/allocateStamp/Action|AllocateStamp är intern åtgärd som används av tjänsten|
|/Locations/allocatedStamp/Read|GetAllocatedStamp är en intern åtgärd som används av tjänsten|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Åtgärd | Beskrivning |
|---|---|
|/ checkNamespaceAvailability/åtgärd|Kontrollerar tillgänglighet för namnrymder i en viss prenumeration.|
|registrera/åtgärd|Registrerar hello prenumerationen för hello Relay-resursprovidern och gör det möjligt hello skapa Relay-resurser|
|/ namnområden/skrivning|Skapa en resurs för Namespace och uppdatera dess egenskaper. Taggar och status för hello Namespace är hello egenskaper som kan uppdateras.|
|/Namespaces/Read|Hämta hello lista över Namespace resurs beskrivning|
|namnområden/ta bort|Ta bort Namespace-resurs|
|/Namespaces/authorizationRules/Write|Skapa en nivå auktoriseringsregler Namespace och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/authorizationRules/delete|Ta bort Namespace auktoriseringsregeln. hello standard Namespace auktoriseringsregeln kan inte tas bort. |
|/Namespaces/authorizationRules/listkeys/Action|Hämta hello anslutningssträngen toohello Namespace|
|/Namespaces/HybridConnections/Write|Skapa eller uppdatera HybridConnection egenskaper.|
|/Namespaces/HybridConnections/Read|Hämta lista över HybridConnection resurs beskrivningar|
|/Namespaces/HybridConnections/delete|Åtgärden toodelete HybridConnection resurs|
|/Namespaces/HybridConnections/authorizationRules/Write|Skapa auktoriseringsregler för HybridConnection och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/HybridConnections/authorizationRules/delete|Åtgärden toodelete HybridConnection auktoriseringsregler|
|/Namespaces/HybridConnections/authorizationRules/listkeys/Action|Hämta hello anslutningssträngen tooHybridConnection|
|/Namespaces/WcfRelays/Write|Skapa eller uppdatera WcfRelay egenskaper.|
|/Namespaces/WcfRelays/Read|Hämta lista över WcfRelay resurs beskrivningar|
|/Namespaces/WcfRelays/delete|Åtgärden toodelete WcfRelay resurs|
|/Namespaces/WcfRelays/authorizationRules/Write|Skapa auktoriseringsregler för WcfRelay och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/WcfRelays/authorizationRules/delete|Åtgärden toodelete WcfRelay auktoriseringsregler|
|/Namespaces/WcfRelays/authorizationRules/listkeys/Action|Hämta hello anslutningssträngen tooWcfRelay|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Åtgärd | Beskrivning |
|---|---|
|/ AvailabilityStatuses/läsning|Hämtar hello tillgänglighetsstatusarna för alla resurser i hello angivna scope|
|/ AvailabilityStatuses/current/läsning|Hämtar hello Tillgänglighetsstatus för hello angivna resursen|

## <a name="microsoftresources"></a>Microsoft.Resources

| Åtgärd | Beskrivning |
|---|---|
|/ checkResourceName/åtgärd|Kontrollera hello resursnamnets giltighet.|
|/providers/Read|Hämta hello lista över leverantörer.|
|/subscriptions/Read|Hämtar hello listan över prenumerationer.|
|/subscriptions/operationresults/Read|Resultatet hello prenumerationen igen.|
|/subscriptions/providers/Read|Hämtar eller listar resursleverantörer.|
|/subscriptions/tagNames/Read|Hämtar eller listar prenumerationstaggar.|
|/subscriptions/tagNames/Write|Lägger till en prenumerationstagg.|
|/subscriptions/tagNames/delete|Tar bort en prenumerationstagg.|
|/subscriptions/tagNames/tagValues/Read|Hämtar eller listar prenumerationstaggsvärden.|
|/subscriptions/tagNames/tagValues/Write|Lägger till ett prenumerationstaggsvärde.|
|/subscriptions/tagNames/tagValues/delete|Tar bort ett prenumerationstaggsvärde.|
|/subscriptions/Resources/Read|Hämtar en prenumerations resurser.|
|/subscriptions/resourceGroups/Read|Hämtar eller listar resursgrupper.|
|/subscriptions/resourceGroups/Write|Skapar eller uppdaterar en resursgrupp.|
|/subscriptions/resourceGroups/delete|Tar bort en resursgrupp och alla dess resurser.|
|/subscriptions/resourceGroups/moveResources/Action|Flyttar resurser från en resurs grupp tooanother.|
|/subscriptions/resourceGroups/validateMoveResources/Action|Verifiera flytt av resurser från en resurs grupp tooanother.|
|/subscriptions/resourcegroups/Resources/Read|Hämtar hello resurser för hello resursgrupp.|
|/subscriptions/resourcegroups/Deployments/Read|Hämtar eller listar distributioner.|
|/subscriptions/resourcegroups/Deployments/Write|Skapar eller uppdaterar en distribution.|
|/subscriptions/resourcegroups/Deployments/operationstatuses/Read|Hämtar eller listar distribution åtgärden status.|
|/subscriptions/resourcegroups/Deployments/Operations/Read|Hämtar eller listar distributionsåtgärder.|
|/subscriptions/Locations/Read|Hämtar hello lista med platser som stöds.|
|/Links/Read|Hämtar eller listar resurslänkar.|
|/ länkar/skrivning|Skapar eller uppdaterar en resurslänk.|
|/Links/delete|Tar bort en resurslänk.|
|/tenants/Read|Hämtar hello lista med klienter.|
|/Resources/Read|Hämta hello lista över resurser baserade på filter.|
|/Deployments/Read|Hämtar eller listar distributioner.|
|/ distributioner/skrivning|Skapar eller uppdaterar en distribution.|
|/Deployments/delete|Tar bort en distribution.|
|/Deployments/Cancel/Action|Avbryter en distribution.|
|/Deployments/Validate/Action|Verifierar en distribution.|
|/Deployments/Operations/Read|Hämtar eller listar distributionsåtgärder.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Åtgärd | Beskrivning |
|---|---|
|/jobcollections/Read|Hämta Jobbsamlingen|
|/ jobbsamlingar/skrivning|Skapar eller uppdaterar en jobbsamling.|
|/jobcollections/delete|Tar bort jobbet samling.|
|/jobcollections/enable/Action|Aktiverar jobbet samling.|
|/jobcollections/disable/Action|Inaktiverar jobbet samling.|
|/jobcollections/jobs/Read|Hämtar jobbet.|
|/jobcollections/jobs/Write|Skapar eller uppdaterar jobb.|
|/jobcollections/jobs/delete|Tar bort jobbet.|
|/jobcollections/jobs/Run/Action|Kör jobbet.|
|/jobcollections/jobs/generateLogicAppDefinition/Action|Genererar en logikappsdefinition som baseras på ett Scheduler-jobb.|
|/jobcollections/jobs/jobhistories/Read|Hämtar jobbhistorik.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar hello prenumerationen för hello sökleverantör för resursen och gör det möjligt hello skapa search-tjänster.|
|/ checkNameAvailability/åtgärd|Kontrollerar tillgängligheten för hello tjänstnamn.|
|/ searchServices/skrivning|Skapar eller uppdaterar hello search-tjänsten.|
|/searchServices/Read|Läser hello search-tjänsten.|
|/searchServices/delete|Tar bort hello search-tjänsten.|
|/searchServices/Start/Action|Startar hello search-tjänsten.|
|/searchServices/stop/Action|Stoppar hello search-tjänsten.|
|/searchServices/listAdminKeys/Action|Läser hello admin nycklar.|
|/searchServices/regenerateAdminKey/Action|Återskapar hello admin-nyckel.|
|/searchServices/createQueryKey/Action|Skapar hello Frågenyckeln.|
|/searchServices/queryKey/Read|Läser hello frågan nycklar.|
|/searchServices/queryKey/delete|Tar bort hello frågan nyckel.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Åtgärd | Beskrivning |
|---|---|
|/jitNetworkAccessPolicies/Read|Hämtar hello-in-time nätverksåtkomstpolicys|
|/ jitNetworkAccessPolicies/skrivning|Skapar en ny åtkomstprincip för just-in-time-nätverk eller uppdaterar en befintlig|
|/jitNetworkAccessPolicies/initiate/Action|Initierar en åtkomstprincip för just-in-time-nätverk|
|/securitySolutionsReferenceData/Read|Hämtar hello säkerhetslösningar referensdata|
|/securityStatuses/Read|Hämtar hello security health status för Azure-resurser|
|/webApplicationFirewalls/Read|Hämtar hello web application brandväggar|
|/ webApplicationFirewalls/skrivning|Skapar en ny Brandvägg för webbaserade program eller uppdaterar en befintlig|
|/webApplicationFirewalls/delete|Tar bort en brandvägg för webbaserade program|
|/securitySolutions/Read|Hämtar hello säkerhetslösningar|
|/ securitySolutions/skrivning|Skapar en ny lösning eller uppdaterar en befintlig|
|/securitySolutions/delete|Tar bort en säkerhetslösning|
|/Tasks/Read|Hämtar alla tillgängliga säkerhetsrekommendationer|
|/Tasks/dismiss/Action|Stänga en säkerhetsrekommendation|
|/Tasks/Activate/Action|Aktivera en säkerhetsrekommendation|
|/policies/Read|Hämtar hello säkerhetsprincip|
|/ principer/skrivning|Uppdateringar hello säkerhetsprincip|
|/applicationWhitelistings/Read|Hämtar hello programmet whitelistings|
|/ applicationWhitelistings/skrivning|Skapar en ny vitlistning av program eller uppdaterar en befintlig|

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

| Åtgärd | Beskrivning |
|---|---|
|/ subscriptions/skrivning|Skapar eller uppdaterar en prenumeration|
|/ gateways/skrivning|Skapar eller uppdaterar en gateway|
|/gateways/delete|Tar bort en gateway|
|/gateways/Read|Hämtar en gateway|
|/gateways/regenerateprofile/Action|Återskapar hello gateway-profil|
|/gateways/upgradetolatest/Action|Uppgraderingar hello gateway toohello senaste versionen|
|/ noder/skrivning|skapar eller uppdaterar en nod|
|/nodes/delete|Tar bort en nod|
|/nodes/Read|Hämtar en nod|
|/ sessioner/skrivning|Skapar eller uppdaterar en session|
|/sessions/Read|Hämtar en session|
|/sessions/delete|Tar bort en sesssion|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Åtgärd | Beskrivning |
|---|---|
|/ checkNameAvailability/åtgärd|Kontrollerar tillgänglighet för namnrymder i en viss prenumeration.|
|registrera/åtgärd|Registrerar hello prenumerationen för hello ServiceBus-resursprovidern och gör det möjligt hello skapa ServiceBus-resurser|
|/ namnområden/skrivning|Skapa en resurs för Namespace och uppdatera dess egenskaper. Taggar och status för hello Namespace är hello egenskaper som kan uppdateras.|
|/Namespaces/Read|Hämta hello lista över Namespace resurs beskrivning|
|namnområden/ta bort|Ta bort Namespace-resurs|
|/Namespaces/metricDefinitions/Read|Hämta lista över Namespace mått resurs beskrivningar|
|/Namespaces/authorizationRules/Write|Skapa en nivå auktoriseringsregler Namespace och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/authorizationRules/Read|Hämta hello lista över auktoriseringsregler för namnområden beskrivning.|
|/Namespaces/authorizationRules/delete|Ta bort Namespace auktoriseringsregeln. hello standard Namespace auktoriseringsregeln kan inte tas bort. |
|/Namespaces/authorizationRules/listkeys/Action|Hämta hello anslutningssträngen toohello Namespace|
|/Namespaces/authorizationRules/regenerateKeys/Action|Återskapa hello primär eller sekundär nyckel toohello resurs|
|/Namespaces/diagnosticSettings/Read|Hämta lista över Namespace diagnostikinställningar resurs beskrivningar|
|/Namespaces/diagnosticSettings/Write|Hämta lista över Namespace diagnostikinställningar resurs beskrivningar|
|/Namespaces/queues/Write|Skapa eller uppdatera köegenskaper.|
|/Namespaces/queues/Read|Hämta lista över kön resurs beskrivningar|
|/Namespaces/queues/delete|Åtgärden toodelete kön resurs|
|/Namespaces/queues/authorizationRules/Write|Skapa auktoriseringsregler för kön och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/queues/authorizationRules/Read| Hämta hello lista över auktoriseringsregler för kön|
|/Namespaces/queues/authorizationRules/delete|Åtgärden toodelete kön auktoriseringsregler|
|/Namespaces/queues/authorizationRules/listkeys/Action|Hämta hello anslutningssträngen tooQueue|
|/Namespaces/queues/authorizationRules/regenerateKeys/Action|Återskapa hello primär eller sekundär nyckel toohello resurs|
|/Namespaces/logDefinitions/Read|Hämta en lista över Namespace loggar resurs beskrivningar|
|/Namespaces/topics/Write|Skapa eller uppdatera avsnittet Egenskaper.|
|/Namespaces/topics/Read|Hämta lista över avsnittet resurs beskrivningar|
|/Namespaces/topics/delete|Åtgärden toodelete avsnittet resurs|
|/Namespaces/topics/authorizationRules/Write|Skapa auktoriseringsregler för avsnittet och uppdatera dess egenskaper. Åtkomstbehörigheter för hello auktorisering regler, hello primära och sekundärnycklar kan uppdateras.|
|/Namespaces/topics/authorizationRules/Read| Hämta hello lista över avsnittet auktoriseringsregler|
|/Namespaces/topics/authorizationRules/delete|Åtgärden toodelete avsnittet auktoriseringsregler|
|/Namespaces/topics/authorizationRules/listkeys/Action|Hämta hello anslutningssträngen tooTopic|
|/Namespaces/topics/authorizationRules/regenerateKeys/Action|Återskapa hello primär eller sekundär nyckel toohello resurs|
|/Namespaces/topics/subscriptions/Write|Skapa eller uppdatera TopicSubscription egenskaper.|
|/Namespaces/topics/subscriptions/Read|Hämta lista över TopicSubscription resurs beskrivningar|
|/Namespaces/topics/subscriptions/delete|Åtgärden toodelete TopicSubscription resurs|
|/Namespaces/topics/subscriptions/rules/Write|Skapa eller uppdatera regelegenskaper.|
|/Namespaces/topics/subscriptions/rules/Read|Hämta lista över regeln resurs beskrivningar|
|/Namespaces/topics/subscriptions/rules/delete|Åtgärden toodelete regeln resurs|

## <a name="microsoftsql"></a>Microsoft.Sql

| Åtgärd | Beskrivning |
|---|---|
|/Servers/Read|Returnera en lista över servrar i en resursgrupp för en prenumeration|
|/ servrar/skrivning|Skapa en ny server eller ändra egenskaperna för en befintlig server i en resursgrupp för en prenumeration|
|/Servers/delete|Ta bort en server och alla inneslutna databaser och elastiska pooler|
|/Servers/import/Action|Skapa en ny databas på servern hello och distribuera schema och data från ett DacPac-paket|
|/Servers/Upgrade/Action|Aktivera nya funktioner som är tillgängliga på hello senaste versionen av server och ange konverteringskarta för databaser edition|
|/Servers/VulnerabilityAssessmentScans/Action|Köra säkerhetsproblem genomsökning för utvärdering av server|
|/Servers/operationResults/Read|Åtgärden används tootrack förloppet för serveruppgraderingen från lägre version toohigher|
|/Servers/operationResults/delete|Avbryt version-serveruppgraderingen pågår|
|/Servers/securityAlertPolicies/Read|Hämta information om hello server hot princip konfigurerats på en viss server|
|/Servers/securityAlertPolicies/Write|Ändra hello server hotidentifiering för en given server|
|/Servers/securityAlertPolicies/operationResults/Read|Hämta resultaten av hello server Hotidentifiering princip Set-åtgärd|
|/Servers/Administrators/Read|Hämta serverinformation för administratör|
|/Servers/Administrators/Write|Skapa eller uppdatera serveradministratör|
|/Servers/Administrators/delete|Ta bort serveradministratören från hello-servern|
|/Servers/recoverableDatabases/Read|Den här åtgärden används för katastrofåterställning av live databasen toorestore databasen toolast kända fungerande säkerhetskopieringspunkt. Returnerar information om hello senaste korrekta säkerhetskopian men det faktiskt återställa inte hello-databasen.|
|/Servers/serviceObjectives/Read|Hämta lista över servicenivåmål (även kallat prestandanivåer) finns på en viss server|
|/Servers/firewallRules/Read|Hämta serverinformation för brandväggen regel|
|/Servers/firewallRules/Write|Skapa eller uppdatera brandväggsregel som styr IP-adressintervall tillåtna tooconnect toohello server|
|/Servers/firewallRules/delete|Tar bort brandväggsregeln från hello-server|
|/Servers/administratorOperationResults/Read|Hämta prenumerationsåtgärdsresultaten för server-administratören|
|/Servers/recommendedElasticPools/Read|Hämta rekommendationer för elastiska pooler tooreduce databaskostnader eller förbättra prestandan, utifrån historica resursutnyttjande|
|/Servers/recommendedElasticPools/Metrics/Read|Hämta mätvärden för rekommenderade elastiska databaspoolerna för en viss server|
|/Servers/recommendedElasticPools/Databases/Read|Hämta databaser som ska läggas till i rekommenderade elastiska databaspooler för en given server|
|/Servers/elasticPools/Read|Hämta information om elastisk databaspool på en viss server|
|/Servers/elasticPools/Write|Skapa en ny eller ändra egenskaper för befintlig elastisk databaspool|
|/Servers/elasticPools/delete|Ta bort befintlig elastisk databaspool|
|/Servers/elasticPools/operationResults/Read|Hämta information om en viss elastisk pool databasåtgärd|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>metricDefinitions/läsning|Returnera typer av mätvärden som är tillgängliga för elastiska databaspooler|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings/läsning|Hämtar hello diagnostikinställningen för resursen hello|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings/skrivning|Skapar eller uppdaterar hello diagnostikinställningen för resursen hello|
|/Servers/elasticPools/Metrics/Read|Returnera elastisk databas poolen resurs användning mått|
|/Servers/elasticPools/elasticPoolDatabaseActivity/Read|Hämta aktiviteter och information om en viss databas som ingår i en elastisk databaspool|
|/Servers/elasticPools/Advisors/Read|Returnerar en lista över rådgivare som är tillgängliga för hello elastisk pool|
|/Servers/elasticPools/Advisors/Write|Uppdateringen automatiskt-köra status för en advisor på elastisk pool-nivå.|
|/Servers/elasticPools/Advisors/recommendedActions/Read|Returnerar en lista över rekommenderade åtgärder för angivna advisor för hello elastisk pool|
|/Servers/elasticPools/Advisors/recommendedActions/Write|Tillämpa hello rekommenderad åtgärd på hello elastisk pool|
|/Servers/elasticPools/elasticPoolActivity/Read|Hämta aktiviteter och information om en viss elastisk databaspool|
|/Servers/elasticPools/Databases/Read|Hämta lista och information om databaser som ingår i en elastisk databaspool på en viss server|
|/Servers/auditingPolicies/Read|Hämta information om hello server Standardtabell granskningsprincip som konfigurerats på en viss server|
|/Servers/auditingPolicies/Write|Ändra hello server Standardtabell granskning för en viss server|
|/Servers/disasterRecoveryConfiguration/operationResults/Read|Resultatet Disaster Recovery konfiguration igen|
|/Servers/Advisors/Read|Returnerar en lista över rådgivare som är tillgängliga för hello-server|
|/Servers/Advisors/Write|Uppdateringar köra automatiskt-status för en advisor på servernivå.|
|/Servers/Advisors/recommendedActions/Read|Returnerar en lista över rekommenderade åtgärder för angiven klassificering för hello-server|
|/Servers/Advisors/recommendedActions/Write|Tillämpa hello rekommenderad åtgärd på hello-server|
|/Servers/usages/Read|Returnera DTU-kvot för servern och den aktuella DTU consuption alla databaser inom hello-server|
|/Servers/elasticPoolEstimates/Read|Returnerar en lista över elastisk pool beräknar redan har skapats för den här servern|
|/Servers/elasticPoolEstimates/Write|Skapar ny elastiska pooluppskattningen lista över databaser som har angetts|
|/Servers/auditingSettings/Read|Hämta information om hello server blob granskningsprincip som konfigurerats på en viss server|
|/Servers/auditingSettings/Write|Ändra hello server blobbgranskning för en viss server|
|/Servers/auditingSettings/operationResults/Read|Hämta resultatet av hello server blob granskning princip Set-åtgärd|
|/Servers/backupLongTermRetentionVaults/Read|Denna åtgärd är används tooget en säkerhetskopiering för långsiktig kvarhållning valvet. Den returnerar information om hello valvet registrerade toothis server.|
|/Servers/backupLongTermRetentionVaults/Write|Registrera en säkerhetskopiering för långsiktig kvarhållning valvet|
|/Servers/restorableDroppedDatabases/Read|Hämta en lista över databaser som har släppts på en viss server som är fortfarande inom bevarandeprincip. Den här åtgärden returnerar en lista över databaser och associerade metadata, som datum för borttagning.|
|/Servers/Databases/Read|Returnera en lista över servrar i en resursgrupp för en prenumeration|
|/Servers/Databases/Write|Skapa en ny server eller ändra egenskaperna för en befintlig server i en resursgrupp för en prenumeration|
|/Servers/Databases/delete|Ta bort en server och alla inneslutna databaser och elastiska pooler|
|/Servers/Databases/export/Action|Skapa en ny databas på servern hello och distribuera schema och data från ett DacPac-paket|
|/Servers/Databases/VulnerabilityAssessmentScans/Action|Köra säkerhetsproblem genomsökning för utvärdering av databasen.|
|/Servers/Databases/pause/Action|Pausa en DataWarehouse edition-databas|
|/Servers/Databases/Resume/Action|Återuppta en DataWarehouse edition-databas|
|/Servers/Databases/operationResults/Read|Åtgärden används tootrack förloppet för långvarig databasåtgärd, till exempel skala.|
|/Servers/Databases/replicationLinks/Read|Returnerar information om replikeringslänkar som har upprättats för en viss databas|
|/Servers/Databases/replicationLinks/delete|Avsluta hello replikeringsrelation tvång och potentiell dataförlust|
|/Servers/Databases/replicationLinks/unlink/Action|Avsluta hello replikeringsrelation tvång eller efter synkronisering med hello partner|
|/Servers/Databases/replicationLinks/failover/Action|Växling vid fel efter synkronisering alla ändringar från hello primära, vilket gör den här databasen till hello replikeringens relation hello primära och att remote primär till en sekundär|
|/Servers/Databases/replicationLinks/forceFailoverAllowDataLoss/Action|Redundans omedelbart med potentiell dataförlust, vilket gör den här databasen till hello replikeringens relation hello primära och att remote primär till en sekundär|
|/Servers/Databases/replicationLinks/updateReplicationMode/Action|Uppdatera replikering för länken toosynchronous eller asynkron läge|
|/Servers/Databases/replicationLinks/operationResults/Read|Hämta status för långvariga åtgärder på databasreplikeringslänkar|
|/Servers/Databases/dataMaskingPolicies/Read|Hämta information om hello datamaskning princip som konfigurerats på en viss databas|
|/Servers/Databases/dataMaskingPolicies/Write|Ändra datamaskning princip för en viss databas|
|/Servers/Databases/dataMaskingPolicies/rules/Read|Hämta information om hello datamaskning principregeln som konfigurerats på en viss databas|
|/Servers/Databases/dataMaskingPolicies/rules/Write|Ändra datamaskning principregel för en viss databas|
|/Servers/Databases/securityAlertPolicies/Read|Hämta information om hello hot princip konfigurerats på en viss databas|
|/Servers/Databases/securityAlertPolicies/Write|Ändra hello hot princip för en viss databas|
|/Servers/Databases/providers/Microsoft.Insights/<br>metricDefinitions/läsning|Returnera typer av mätvärden som är tillgängliga för databaser|
|/Servers/Databases/providers/Microsoft.Insights/<br>diagnosticSettings/läsning|Hämtar hello diagnostikinställningen för resursen hello|
|/Servers/Databases/providers/Microsoft.Insights/<br>diagnosticSettings/skrivning|Skapar eller uppdaterar hello diagnostikinställningen för resursen hello|
|/Servers/Databases/providers/Microsoft.Insights/<br>logDefinitions/läsning|Hämtar hello tillgängliga loggar för databaser|
|/Servers/Databases/topQueries/Read|Returnerar samman körningsstatistik för den valda frågan i vald tidsperiod|
|/Servers/Databases/topQueries/queryText/Read|Returnerar hello Transact-SQL-text för valda fråge-ID|
|/Servers/Databases/topQueries/statistics/Read|Returnerar samman körningsstatistik för den valda frågan i vald tidsperiod|
|/Servers/Databases/connectionPolicies/Read|Hämta information om hello anslutningsprincip som konfigurerats på en viss databas|
|/Servers/Databases/connectionPolicies/Write|Ändra princip för en viss databas|
|/Servers/Databases/Metrics/Read|Returnera databasen resource användning mått|
|/Servers/Databases/auditRecords/Read|Hämta hello databasen blob granskningsposter|
|/Servers/Databases/transparentDataEncryption/Read|Hämta status och information om transparent data kryptering säkerhetsfunktion för en viss databas|
|/Servers/Databases/transparentDataEncryption/Write|Aktivera eller inaktivera transparent datakryptering för en viss databas|
|/Servers/Databases/transparentDataEncryption/operationResults/Read|Hämta status och information om transparent data kryptering säkerhetsfunktion för en viss databas|
|/Servers/Databases/auditingPolicies/Read|Hämta information om hello tabell granskningsprincip konfigureras på en viss databas|
|/Servers/Databases/auditingPolicies/Write|Ändra hello tabell granskningsprincip för en viss databas|
|/Servers/Databases/dataWarehouseQueries/Read|Returnerar hello distribution frågan datalagerinformation för valda fråge-ID|
|/ servrar/databaser/dataWarehouseQueries /<br>dataWarehouseQuerySteps/läsning|Returnerar hello distribuerade frågan steg information för data warehouse frågan för valda steget ID|
|/Servers/Databases/serviceTierAdvisors/Read|Returnera förslag om att skala databasen uppåt eller nedåt baserat på tooimprove frågeprestanda körning statistik eller minska kostnaden för|
|/Servers/Databases/Advisors/Read|Returnerar en lista över rådgivare som är tillgängliga för hello-databas|
|/Servers/Databases/Advisors/Write|Uppdateringen automatiskt-köra status för en advisor på databasnivå.|
|/Servers/Databases/Advisors/recommendedActions/Read|Returnerar en lista över rekommenderade åtgärder för angivna advisor för databasen hello|
|/Servers/Databases/Advisors/recommendedActions/Write|Tillämpa hello rekommenderad åtgärd för hello-databasen|
|/Servers/Databases/usages/Read|Returnera databasen största tillåtna storlek som kan nås och aktuell storlek upptas av data|
|/Servers/Databases/queryStore/Read|Returnerar aktuella värden för Query Store inställningar för hello-databas|
|/Servers/Databases/queryStore/Write|Uppdaterar Query Store-inställningen för hello-databas|
|/Servers/Databases/auditingSettings/Read|Hämta information om hello blob granskningsprincip konfigureras på en viss databas|
|/Servers/Databases/auditingSettings/Write|Ändra hello blob granskningsprincip för en viss databas|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Read|Hämta listan över indexrekommendationer för en databas|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Write|Tillämpa indexrekommendationen|
|/Servers/Databases/schemas/Tables/columns/Read|Hämta listan över kolumner i en tabell|
|/Servers/Databases/missingindexes/Read|Returnera förslag om databasen index toocreate, ändra eller ta bort i ordning tooimprove frågeprestanda|
|/Servers/Databases/missingindexes/Write|Använd databas indexrekommendationen i en viss databas|
|/Servers/Databases/importExportOperationResults/Read|Returnerar information om databasen åtgärden importera eller exportera från DacPac finns i lagringskontot|
|/Servers/importExportOperationResults/Read|Returnera hello lista med information om databasen importåtgärder från storage-konto på en viss server|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar hello prenumerationen för lagringsresursprovidern hello och aktiverar hello skapande av lagringskonton.|
|/checknameavailability/Read|Kontrollerar att kontonamnet är giltigt och används.|
|/ storageAccounts/skrivning|Skapar ett lagringskonto med hello angivna parametrarna eller uppdaterar hello egenskaperna eller taggarna eller lägger till anpassad domän för hello angetts storage-konto.|
|/storageAccounts/delete|Tar bort ett befintligt lagringskonto.|
|/storageAccounts/listkeys/Action|Returnerar hello snabbtangenter för hello angetts storage-konto.|
|/storageAccounts/regeneratekey/Action|Återskapar hello snabbtangenter för hello angetts storage-konto.|
|/storageAccounts/Read|Returnerar hello lista med lagringskonton eller hämtar hello egenskaper för hello angetts storage-konto.|
|/storageAccounts/listAccountSas/Action|Returnerar hello kontots SAS-token för hello angetts storage-konto.|
|/storageAccounts/listServiceSas/Action|Storage Service SAS-Token|
|/storageAccounts/Services/diagnosticSettings/Write|Skapa eller uppdatera diagnostikinställningar för storage-konto.|
|/skus/Read|Visar hello SKU: er som stöds av Microsoft.Storage.|
|/usages/Read|Returnerar hello gränsen och hello aktuella antalet användningar för resurser i hello angetts prenumeration|
|/Operations/Read|Omröstningar hello status för en asynkron åtgärd.|
|/Locations/deleteVirtualNetworkOrSubnets/Action|Meddelar Microsoft.Storage att virtuella nätverk eller undernät tas bort|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Åtgärd | Beskrivning |
|---|---|
|/Managers/clearAlerts/Action|Avmarkera alla hello aviseringar som är associerade med hello Enhetshanteraren.|
|/Managers/getActivationKey/Action|Hämta aktiveringsnyckeln för hello Enhetshanteraren.|
|/Managers/regenerateActivationKey/Action|Återskapa aktiveringsnyckeln för hello Enhetshanteraren.|
|/Managers/regenarateRegistationCertificate/Action|Återskapa registreringscertifikat för hello enhetshanterare.|
|/Managers/getEncryptionKey/Action|Hämta krypteringsnyckeln för hello Enhetshanteraren.|
|/Managers/Read|Visar eller hämtar hello enhetshanterare|
|/Managers/delete|Tar bort hello enhetshanterare|
|/ chefer och skrivning|Skapa eller uppdatera hello enhetshanterare|
|/Managers/configureDevice/Action|Konfigurerar en enhet|
|/Managers/listActivationKey/Action|Hämtar hello aktiveringsnyckeln av hello StorSimple Enhetshanteraren.|
|/Managers/listPublicEncryptionKey/Action|Lista över offentliga krypteringsnycklar en StorSimple Enhetshanteraren.|
|/Managers/listPrivateEncryptionKey/Action|Hämtar privata krypteringsnyckel för en StorSimple Device Manager.|
|/Managers/provisionCloudAppliance/Action|Skapa en ny installation av molnet.|
|Och chefer/skrivning|Med skapa valv så skapas en Azure-resurs av typen valv|
|/ Chefer/läsning|hello hämta valvet igen hämtar ett objekt som representerar hello Azure resurs av typen 'valvet'|
|/ Chefer/ta bort|hello ta bort valvet åtgärden tar bort hello angivna Azure-resurs av typen 'valvet'|
|/Managers/storageAccountCredentials/Write|Skapa eller uppdatera hello Lagringskontouppgifter|
|/Managers/storageAccountCredentials/Read|Visar eller hämtar hello Lagringskontouppgifter|
|/Managers/storageAccountCredentials/delete|Tar bort hello Lagringskontouppgifter|
|/Managers/storageAccountCredentials/listAccessKey/Action|Lista över åtkomstnycklar för lagring av autentiseringsuppgifter för konton|
|/Managers/accessControlRecords/Read|Visar eller hämtar hello Access Control-poster|
|/Managers/accessControlRecords/Write|Skapa eller uppdatera hello Access Control-poster|
|/Managers/accessControlRecords/delete|Tar bort hello Access Control-poster|
|/Managers/Metrics/Read|Visar eller hämtar hello mått|
|/Managers/bandwidthSettings/Read|Visa en lista över hello bandbreddsinställningar (endast 8000-serien)|
|/Managers/bandwidthSettings/Write|Skapar en ny eller uppdaterar bandbreddsinställningar (endast 8000-serien)|
|/Managers/bandwidthSettings/delete|Tar bort en befintlig bandbreddsinställningar (endast 8000-serien)|
|/ Chefer/extendedInformation/läsning|hello Visa utökad Info åtgärden hämtar ett objekt utökad information som representerar hello Azure resurs av typen? valvet?|
|Och chefer/extendedInformation/skrivning|hello Visa utökad Info åtgärden hämtar ett objekt utökad information som representerar hello Azure resurs av typen? valvet?|
|/ Chefer/extendedInformation/ta bort|hello Visa utökad Info åtgärden hämtar ett objekt utökad information som representerar hello Azure resurs av typen? valvet?|
|/Managers/Alerts/Read|Visar eller hämtar hello aviseringar|
|/Managers/storageDomains/Read|Visar eller hämtar hello lagring domäner|
|/Managers/storageDomains/Write|Skapa eller uppdatera hello lagring domäner|
|/Managers/storageDomains/delete|Tar bort hello lagring domäner|
|/Managers/Devices/scanForUpdates/Action|Sök efter uppdateringar i en enhet.|
|/Managers/Devices/Download/Action|Download uppdateringar för en enhet.|
|/Managers/Devices/Install/Action|Installera uppdateringar på en enhet.|
|/Managers/Devices/Read|Visar eller hämtar hello-enheter|
|/Managers/Devices/Write|Skapa eller uppdatera hello-enheter|
|/Managers/Devices/delete|Tar bort hello-enheter|
|/Managers/Devices/deactivate/Action|Inaktiverar en enhet.|
|/Managers/Devices/publishSupportPackage/Action|Publicera supportpaket för en enhet för felsökning av Microsoft-supporten.|
|/Managers/Devices/failover/Action|Redundans av hello enhet.|
|/Managers/Devices/sendTestAlertEmail/Action|Skicka testmeddelandet avisering tooconfigured e-postmottagare.|
|/Managers/Devices/installUpdates/Action|Installerar uppdateringar på hello-enheter|
|/Managers/Devices/listFailoverSets/Action|Lista hello redundans anger för en befintlig enhet.|
|/Managers/Devices/listFailoverTargets/Action|Redundans målen hello-enheter|
|/Managers/Devices/publicEncryptionKey/Action|Lista kryptering med offentlig nyckel hello Enhetshanteraren|
|/ chefer/enheter/hardwareComponentGroups /<br>Läs|Lista hello maskinvara komponentgrupper|
|/ chefer/enheter/hardwareComponentGroups /<br>changeControllerPowerState/åtgärd|Ändra domänkontrollant energisparläge av maskinvara komponentgrupper|
|/Managers/Devices/Metrics/Read|Visar eller hämtar hello mått|
|/Managers/Devices/chapSettings/Write|Skapa eller uppdatera hello Chap-inställningar|
|/Managers/Devices/chapSettings/Read|Visar eller hämtar hello Chap-inställningar|
|/Managers/Devices/chapSettings/delete|Tar bort hello Chap-inställningar|
|/Managers/Devices/backupScheduleGroups/Read|Visar eller hämtar hello säkerhetskopiering schemagrupper|
|/Managers/Devices/backupScheduleGroups/Write|Skapa eller uppdatera hello säkerhetskopiering schemagrupper|
|/Managers/Devices/backupScheduleGroups/delete|Tar bort hello säkerhetskopiering schemagrupper|
|/Managers/Devices/updateSummary/Read|Visar eller hämtar hello sammanfattning|
|/ chefer/enheter/migrationSourceConfigurations /<br>åtgärd och import|Importera källa konfigurationer för migrering|
|/ chefer/enheter/migrationSourceConfigurations /<br>startMigrationEstimate/åtgärd|Starta ett jobb tooestimate hello under hello migreringen.|
|/ chefer/enheter/migrationSourceConfigurations /<br>startMigration/åtgärd|Starta migrering med hjälp av konfigurationer för källa|
|/ chefer/enheter/migrationSourceConfigurations /<br>confirmMigration/åtgärd|Bekräftar en lyckad migrering och sparar den.|
|/ chefer/enheter/migrationSourceConfigurations /<br>fetchMigrationEstimate/åtgärd|Hämta hello status för hello beräkning av migreringsjobb.|
|/ chefer/enheter/migrationSourceConfigurations /<br>fetchMigrationStatus/åtgärd|Hämta hello status för hello migrering.|
|/ chefer/enheter/migrationSourceConfigurations /<br>fetchConfirmMigrationStatus/åtgärd|Hämta hello bekräfta status för migreringen.|
|/Managers/Devices/alertSettings/Read|Visar eller hämtar hello aviseringsinställningar|
|/Managers/Devices/alertSettings/Write|Skapa eller uppdatera hello aviseringsinställningar|
|/Managers/Devices/networkSettings/Read|Visar eller hämtar hello nätverksinställningar|
|/Managers/Devices/networkSettings/Write|Skapar en ny eller uppdaterar nätverksinställningar|
|/Managers/Devices/jobs/Read|Visar eller hämtar hello jobb|
|/Managers/Devices/jobs/Cancel/Action|Avbryta ett jobb som körs|
|/Managers/Devices/metricsDefinitions/Read|Visar eller hämtar hello mått definitioner|
|/Managers/Devices/volumeContainers/Write|Skapar en ny eller uppdaterar Volymbehållare (endast 8000-serien)|
|/Managers/Devices/volumeContainers/Read|Lista över hello Volymbehållare (endast 8000-serien)|
|/Managers/Devices/volumeContainers/delete|Tar bort en befintlig Volymbehållare (endast 8000-serien)|
|/Managers/Devices/volumeContainers/listEncryptionKeys/Action|Lista krypteringsnycklar för Volymbehållare|
|/Managers/Devices/volumeContainers/rolloverEncryptionKey/Action|Krypteringsnycklarna för förnyelse av Volymbehållare|
|/Managers/Devices/volumeContainers/Metrics/Read|Lista hello mått|
|/Managers/Devices/volumeContainers/Volumes/Read|Lista hello volymer|
|/Managers/Devices/volumeContainers/Volumes/Write|Skapar en ny eller uppdaterar volymer|
|/Managers/Devices/volumeContainers/Volumes/delete|Tar bort en befintlig volymer|
|/Managers/Devices/volumeContainers/Volumes/Metrics/Read|Lista hello mått|
|/Managers/Devices/volumeContainers/Volumes/metricsDefinitions/Read|Lista hello mått definitioner|
|/Managers/Devices/volumeContainers/metricsDefinitions/Read|Lista hello mått definitioner|
|/Managers/Devices/iscsiservers/Read|Visar eller hämtar hello iSCSI-servrar|
|/Managers/Devices/iscsiservers/Write|Skapa eller uppdatera hello iSCSI-servrar|
|/Managers/Devices/iscsiservers/delete|Tar bort hello iSCSI-servrar|
|/Managers/Devices/iscsiservers/Backup/Action|Ta säkerhetskopior av en iSCSI-server.|
|/Managers/Devices/iscsiservers/Metrics/Read|Visar eller hämtar hello mått|
|/Managers/Devices/iscsiservers/Disks/Read|Visar eller hämtar hello diskar|
|/Managers/Devices/iscsiservers/Disks/Write|Skapa eller uppdatera hello diskar|
|/Managers/Devices/iscsiservers/Disks/delete|Tar bort hello diskar|
|/Managers/Devices/iscsiservers/Disks/Metrics/Read|Visar eller hämtar hello mått|
|/Managers/Devices/iscsiservers/Disks/metricsDefinitions/Read|Visar eller hämtar hello mått definitioner|
|/Managers/Devices/iscsiservers/metricsDefinitions/Read|Visar eller hämtar hello mått definitioner|
|/Managers/Devices/Backups/Read|Visar eller hämtar hello säkerhetskopia|
|/Managers/Devices/Backups/delete|Tar bort hello säkerhetskopia|
|/Managers/Devices/Backups/Restore/Action|Återställa alla hello-volymer från en säkerhetskopia.|
|/Managers/Devices/Backups/Elements/clone/Action|Klona en resurs eller en volym med en säkerhetskopiering element.|
|/Managers/Devices/backupPolicies/Write|Skapar en ny eller uppdaterar principerna för säkerhetskopiering (endast 8000-serien)|
|/Managers/Devices/backupPolicies/Read|Lista hello principerna för säkerhetskopiering (endast 8000-serien)|
|/Managers/Devices/backupPolicies/delete|Tar bort en befintlig säkerhetskopia principer (endast 8000-serien)|
|/Managers/Devices/backupPolicies/Backup/Action|Ta en manuell säkerhetskopiering toocreate en på-begäran säkerhetskopiering av alla hello volymer som skyddas av hello princip.|
|/Managers/Devices/backupPolicies/Schedules/Write|Skapar en ny eller uppdaterar scheman|
|/Managers/Devices/backupPolicies/Schedules/Read|Lista hello scheman|
|/Managers/Devices/backupPolicies/Schedules/delete|Tar bort en befintlig scheman|
|/Managers/Devices/securitySettings/Update/action|Uppdatera hello säkerhetsinställningar.|
|/Managers/Devices/securitySettings/Read|Lista hello säkerhetsinställningar|
|/ chefer/enheter/securitySettings /<br>syncRemoteManagementCertificate/åtgärd|Synkronisera hello fjärrhantering certifikat för en enhet.|
|/Managers/Devices/securitySettings/Write|Skapar en ny eller uppdaterar säkerhetsinställningar|
|/Managers/Devices/fileservers/Read|Visar eller hämtar hello filservrar|
|/Managers/Devices/fileservers/Write|Skapa eller uppdatera hello filservrar|
|/Managers/Devices/fileservers/delete|Tar bort hello filservrar|
|/Managers/Devices/fileservers/Backup/Action|Ta säkerhetskopior av en filserver.|
|/Managers/Devices/fileservers/Metrics/Read|Visar eller hämtar hello mått|
|/Managers/Devices/fileservers/shares/Write|Skapa eller uppdatera hello resurser|
|/Managers/Devices/fileservers/shares/Read|Visar eller hämtar hello resurser|
|/Managers/Devices/fileservers/shares/delete|Tar bort hello resurser|
|/Managers/Devices/fileservers/shares/Metrics/Read|Visar eller hämtar hello mått|
|/Managers/Devices/fileservers/shares/metricsDefinitions/Read|Visar eller hämtar hello mått definitioner|
|/Managers/Devices/fileservers/metricsDefinitions/Read|Visar eller hämtar hello mått definitioner|
|/Managers/Devices/timeSettings/Read|Visar eller hämtar hello tidsinställningar|
|/Managers/Devices/timeSettings/Write|Skapar en ny eller uppdaterar tidsinställningar|
|Och chefer/certifikat/skrivning|hello Update resurs certifikat åtgärden uppdaterar hello Autentiseringscertifikatet för resurs-valvet.|
|/Managers/cloudApplianceConfigurations/Read|Lista hello molnet installation stöds konfigurationer|
|/Managers/metricsDefinitions/Read|Visar eller hämtar hello mått definitioner|
|/Managers/encryptionSettings/Read|Visar eller hämtar hello krypteringsinställningar|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Åtgärd | Beskrivning |
|---|---|
|/streamingjobs/Start/Action|Starta Stream Analytics-jobbet|
|/streamingjobs/stop/Action|Stoppa Stream Analytics-jobbet|
|/ streamingjobs/Läs|Läs Stream Analytics-jobbet|
|/ streamingjobs/skrivning|Skriva Stream Analytics-jobbet|
|/ streamingjobs/ta bort|Ta bort Stream Analytics-jobbet|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/Read|Hämtar hello tillgängliga mått för streamingjobs|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/Read|Läsa diagnostikinställningen.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/Write|Skriva diagnostikinställningen.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/Read|Hämtar hello tillgängliga loggar för streamingjobs|
|/streamingjobs/Transformations/Read|Läs Stream Analytics-jobbet omvandling|
|/streamingjobs/Transformations/Write|Skriva Stream Analytics-jobbet omvandling|
|/streamingjobs/Transformations/delete|Ta bort Stream Analytics-jobbet omvandling|
|/streamingjobs/inputs/Read|Läs Stream Analytics-jobbet indata|
|/streamingjobs/inputs/Write|Skriva Stream Analytics-jobbet indata|
|/streamingjobs/inputs/delete|Ta bort Stream Analytics-jobbet indata|
|/streamingjobs/outputs/Read|Läs Stream Analytics-Jobbutdata|
|/streamingjobs/outputs/Write|Skriva Stream Analytics-Jobbutdata|
|/streamingjobs/outputs/delete|Ta bort Stream Analytics-Jobbutdata|

## <a name="microsoftsupport"></a>Microsoft.Support

| Åtgärd | Beskrivning |
|---|---|
|registrera/åtgärd|Registrerar tooSupport Resource Provider|
|/supportTickets/Read|Hämtar information om supportärendet (inklusive status, allvarlighetsgrad, kontaktinformation och kommunikation) eller hämtar hello listan över supportärenden för alla prenumerationer.|
|/ supportTickets/skrivning|Skapar eller uppdaterar ett supportärende. Du kan skapa ett supportärende för tekniska, fakturering, kvoter eller prenumerationshantering relaterade problem. Du kan uppdatera allvarlighetsgrad, kontaktinformation och kommunikation för befintliga supportärenden.|

## <a name="microsoftweb"></a>Microsoft.Web

| Åtgärd | Beskrivning |
|---|---|
|avregistrera/åtgärd|Avregistrera Microsoft.Web resource provider för hello prenumeration.|
|verifiera/åtgärd|Validera.|
|registrera/åtgärd|Registerresursleverantören Microsoft.Web för hello prenumeration.|
|/ hostingEnvironments/Läs|Hämta hello egenskaperna för en Apptjänstmiljö|
|/ hostingEnvironments/skrivning|Skapa en ny Apptjänstmiljö eller uppdatera en befintlig|
|/ hostingEnvironments/ta bort|Ta bort Apptjänst-miljö|
|/hostingEnvironments/reboot/Action|Starta om alla datorer i en Apptjänst-miljö|
|/hostingenvironments/Resume/Action|Återuppta värdbaserade miljöer.|
|/hostingenvironments/suspend/Action|Pausa värdbaserade miljöer.|
|/hostingenvironments/metricdefinitions/Read|Hämta värd miljöer Måttdefinitioner.|
|/hostingEnvironments/workerPools/Read|Hämta hello egenskaper i en Arbetspool i en Apptjänst-miljö|
|/hostingEnvironments/workerPools/Write|Skapa en ny Arbetspool i en Apptjänst-miljö eller uppdatera en befintlig|
|/hostingenvironments/workerpools/metricdefinitions/Read|Hämta värd miljöer Workerpools mått definitioner.|
|/hostingenvironments/workerpools/Metrics/Read|Hämta värd miljöer Workerpools mått.|
|/hostingenvironments/workerpools/skus/Read|Hämta värd miljöer Workerpools SKU: er.|
|/hostingenvironments/workerpools/usages/Read|Hämta värd miljöer Workerpools användningsområden.|
|/hostingenvironments/Sites/Read|Hämta värd miljöer Web Apps.|
|/hostingenvironments/serverfarms/Read|Hämta värd miljöer App Service-planer.|
|/hostingenvironments/usages/Read|Hämta värd miljöer användningsområden.|
|/hostingenvironments/capacities/Read|Hämta värd miljöer kapacitet.|
|/hostingenvironments/Operations/Read|Hämta värd Operations miljöer.|
|/hostingEnvironments/multiRolePools/Read|Hämta hello egenskaperna för en FrontEnd-Pool i en Apptjänst-miljö|
|/hostingEnvironments/multiRolePools/Write|Skapa en ny FrontEnd-Pool i en Apptjänst-miljö eller uppdatera en befintlig|
|/hostingenvironments/multirolepools/metricdefinitions/Read|Hämta värd miljöer mellan pooler Måttdefinitioner.|
|/hostingenvironments/multirolepools/Metrics/Read|Hämta värd miljöer mellan pooler mått.|
|/hostingenvironments/multirolepools/skus/Read|Hämta värd miljöer mellan pooler SKU: er.|
|/hostingenvironments/multirolepools/usages/Read|Hämta värd miljöer mellan pooler användningsområden.|
|/hostingenvironments/Diagnostics/Read|Hämta värd miljöer diagnostik.|
|/publishingusers/Read|Hämta publicera användare.|
|/ publishingusers/skrivning|Uppdatera publicering av användare.|
|/checknameavailability/Read|Kontrollera om resursnamnet är tillgängliga.|
|/ geoRegions/Läs|Hämta hello lista över geografiska regionerna.|
|/ webbplatser/Läs|Hämta hello egenskaperna för en Webbapp|
|/ webbplatser/skrivning|Skapa ett nytt webbprogram eller uppdatera en befintlig|
|/ webbplatser/ta bort|Ta bort en befintlig Webbapp|
|/Sites/Backup/Action|Skapa en ny säkerhetskopia av web app|
|/Sites/publishxml/Action|Hämta Publicera profil-xml för en Webbapp|
|/Sites/publish/Action|Publicera ett webbprogram|
|/Sites/restart/Action|Starta om ett webbprogram|
|/Sites/Start/Action|Starta ett webbprogram|
|/Sites/stop/Action|Stoppa ett webbprogram|
|/Sites/slotsswap/Action|Växla distributionsplatser för webbprogram|
|/Sites/slotsdiffs/Action|Hämta konfigurationsavvikelser mellan webbprogrammet och platser|
|/Sites/applySlotConfig/Action|Tillämpa web app platskonfigurationen från målet fack toohello aktuella webbapp|
|/Sites/resetSlotConfig/Action|Återställ webbappkonfigurationen|
|/Sites/Functions/Action|Funktioner Web Apps.|
|/Sites/listsyncfunctiontriggerstatus/Action|Lista synkronisering funktionen utlösaren Status Web Apps.|
|/Sites/networktrace/Action|Webbprogram för spårning i nätverket.|
|/Sites/NewPassword/Action|Nytt lösenord Web Apps.|
|/Sites/Sync/Action|Synkronisera Web Apps.|
|/Sites/operationresults/Read|Hämta Åtgärdsresultat för Web Apps.|
|/Sites/webjobs/Read|Hämta Web Apps WebJobs.|
|/ platser/backup/Läs|Hämta Web Apps säkerhetskopiering.|
|/Sites/Backup/Write|Uppdatera Web Apps säkerhetskopiering.|
|/Sites/metricdefinitions/Read|Hämta definitioner för Web Apps mått.|
|/Sites/Metrics/Read|Hämta mätvärden från webben appar.|
|/Sites/continuouswebjobs/delete|Ta bort Web Apps kontinuerlig Web jobb.|
|/Sites/continuouswebjobs/Read|Hämta Web Apps kontinuerlig Webjobs.|
|/Sites/continuouswebjobs/Start/Action|Starta Web Apps kontinuerlig Web jobb.|
|/Sites/continuouswebjobs/stop/Action|Stoppa Web Apps kontinuerlig Webjobs.|
|/Sites/domainownershipidentifiers/Read|Hämta Web Apps-domän ägarskap identifierare.|
|/Sites/domainownershipidentifiers/Write|Uppdatera Web Apps-domän ägarskap identifierare.|
|/Sites/premieraddons/delete|Ta bort Web Apps Premier-tillägg.|
|/Sites/premieraddons/Read|Hämta Web Apps Premier-tillägg.|
|/Sites/premieraddons/Write|Uppdatera Web Apps Premier-tillägg.|
|/Sites/triggeredwebjobs/delete|Ta bort Web Apps utlösta WebJobs.|
|/Sites/triggeredwebjobs/Read|Hämta Web Apps utlösta WebJobs.|
|/Sites/triggeredwebjobs/Run/Action|Kör Web Apps utlöses WebJobs.|
|/Sites/hostnamebindings/delete|Ta bort Värdnamnsbindningar för Web Apps.|
|/Sites/hostnamebindings/Read|Hämta Värdnamnsbindningar för Web Apps.|
|/Sites/hostnamebindings/Write|Uppdatera Värdnamnsbindningar för Web Apps.|
|/Sites/virtualnetworkconnections/delete|Ta bort virtuella nätverksanslutningar för Web Apps.|
|/Sites/virtualnetworkconnections/Read|Hämta Web Apps virtuella nätverksanslutningar.|
|/Sites/virtualnetworkconnections/Write|Uppdatera Web Apps virtuella nätverksanslutningar.|
|/Sites/virtualnetworkconnections/gateways/Read|Hämta Web Apps virtuella anslutningar Nätverksgatewayerna.|
|/Sites/virtualnetworkconnections/gateways/Write|Uppdatera Web Apps virtuella anslutningar Nätverksgatewayerna.|
|/Sites/publishxml/Read|Hämta publicera Web Apps XML.|
|/Sites/hybridconnectionrelays/Read|Hämta Web Apps Hybrid anslutning reläer.|
|/Sites/perfcounters/Read|Hämta prestandaräknare för Web Apps.|
|/Sites/usages/Read|Hämta Web Apps användningsområden.|
|/Sites/slots/Write|Skapa en ny Webbprogramplats eller uppdatera en befintlig|
|/Sites/slots/delete|Ta bort en befintlig Webbprogramplats|
|/Sites/slots/Backup/Action|Skapa ny Webbprogramplats säkerhetskopia.|
|/Sites/slots/publishxml/Action|Hämta Publicera profil-xml för Webbprogramplats|
|/Sites/slots/publish/Action|Publicera en Webbprogramplats|
|/Sites/slots/restart/Action|Starta om en Webbprogramplats|
|/Sites/slots/Start/Action|Starta en Webbprogramplats|
|/Sites/slots/stop/Action|Stoppa en Webbprogramplats|
|/Sites/slots/slotsswap/Action|Växla distributionsplatser för webbprogram|
|/Sites/slots/slotsdiffs/Action|Hämta konfigurationsavvikelser mellan webbprogrammet och platser|
|/Sites/slots/applySlotConfig/Action|Tillämpa web app platskonfigurationen från målet fack toohello aktuella plats.|
|/Sites/slots/resetSlotConfig/Action|Återställ platskonfigurationen för web app|
|/Sites/slots/Read|Hämta hello egenskaperna för en distributionsplats för webbprogram|
|/Sites/slots/NewPassword/Action|Nytt lösenord Web Apps platser.|
|/Sites/slots/Sync/Action|Synkronisera Web Apps platser.|
|/Sites/slots/operationresults/Read|Hämta Web Apps fack Åtgärdsresultat.|
|/Sites/slots/webjobs/Read|Hämta Web Apps fack WebJobs.|
|/Sites/slots/Backup/Write|Uppdatera Web Apps fack säkerhetskopiering.|
|/Sites/slots/metricdefinitions/Read|Hämta Måttdefinitionerna för Web Apps platser.|
|/Sites/slots/Metrics/Read|Hämta Web Apps fack mått.|
|/Sites/slots/continuouswebjobs/delete|Ta bort Web Apps fack kontinuerlig Web jobb.|
|/Sites/slots/continuouswebjobs/Read|Hämta Web Apps fack kontinuerlig Webjobs.|
|/Sites/slots/continuouswebjobs/Start/Action|Starta Web Apps fack kontinuerlig Web jobb.|
|/Sites/slots/continuouswebjobs/stop/Action|Stoppa Web Apps fack kontinuerlig Webjobs.|
|/Sites/slots/premieraddons/delete|Ta bort Web Apps fack Premier-tillägg.|
|/Sites/slots/premieraddons/Read|Hämta Web Apps fack Premier-tillägg.|
|/Sites/slots/premieraddons/Write|Uppdatera Web Apps fack Premier-tillägg.|
|/Sites/slots/triggeredwebjobs/delete|Ta bort Web Apps fack utlösta WebJobs.|
|/Sites/slots/triggeredwebjobs/Read|Hämta Web Apps fack utlösta WebJobs.|
|/Sites/slots/triggeredwebjobs/Run/Action|Kör Web Apps fack utlösta WebJobs.|
|/Sites/slots/hostnamebindings/delete|Ta bort Värdnamnsbindningar för Web Apps platser.|
|/Sites/slots/hostnamebindings/Read|Hämta Värdnamnsbindningar för Web Apps platser.|
|/Sites/slots/hostnamebindings/Write|Uppdatera Värdnamnsbindningar för Web Apps platser.|
|/Sites/slots/phplogging/Read|Hämta Web Apps fack Phplogging.|
|/Sites/slots/virtualnetworkconnections/delete|Ta bort virtuella nätverksanslutningar för Web Apps-platser.|
|/Sites/slots/virtualnetworkconnections/Read|Hämta virtuella nätverksanslutningar för Web Apps-platser.|
|/Sites/slots/virtualnetworkconnections/Write|Uppdatera virtuella nätverksanslutningar för Web Apps-platser.|
|/Sites/slots/virtualnetworkconnections/gateways/Write|Uppdatera Web Apps fack virtuella anslutningar Nätverksgatewayerna.|
|/Sites/slots/usages/Read|Hämta Web Apps fack användningsområden.|
|/Sites/slots/hybridconnection/delete|Ta bort Hybridanslutningen för Web Apps platser.|
|/Sites/slots/hybridconnection/Read|Hämta Hybridanslutning för Web Apps platser.|
|/Sites/slots/hybridconnection/Write|Uppdatera Hybridanslutning för Web Apps platser.|
|/Sites/slots/config/Read|Hämta Webbprogramplatss konfigurationsinställningar|
|/Sites/slots/config/List/Action|Visa en lista med Web App fack känsliga säkerhetsinställningar, t.ex publicering autentiseringsuppgifter, app-inställningar och anslutningssträngar|
|/Sites/slots/config/Write|Uppdatera Webbprogramplatss konfigurationsinställningar|
|/Sites/slots/config/delete|Ta bort Web Apps fack Config.|
|/Sites/slots/instances/Read|Hämta Web Apps fack instanser.|
|/Sites/slots/instances/processes/Read|Hämta Web Apps fack instanser processer.|
|/Sites/slots/instances/Deployments/Read|Hämta Web Apps fack instanser distributioner.|
|/Sites/slots/sourcecontrols/Read|Hämta Webbprogramplatss källkontrollen konfigurationsinställningar|
|/Sites/slots/sourcecontrols/Write|Uppdatera inställningar för Web App fack källkontrollen configuration|
|/Sites/slots/sourcecontrols/delete|Ta bort konfigurationsinställningar för Web App fack källa kontroll|
|/Sites/slots/Restore/Read|Hämta Web Apps fack återställning.|
|/Sites/slots/analyzecustomhostname/Read|Hämta Web Apps fack analysera anpassade värdnamnet.|
|/Sites/slots/Backups/Read|Hämta hello egenskaperna för en webbprogramplatser säkerhetskopiering|
|/Sites/slots/Backups/List/Action|Lista Web Apps fack säkerhetskopiering.|
|/Sites/slots/Backups/Restore/Action|Återställa Web Apps fack säkerhetskopior.|
|/Sites/slots/Deployments/delete|Ta bort Web Apps fack distributioner.|
|/Sites/slots/Deployments/Read|Hämta Web Apps fack distributioner.|
|/Sites/slots/Deployments/Write|Uppdatera Web Apps fack distributioner.|
|/Sites/slots/Deployments/log/Read|Hämta Web Apps fack distributioner loggen.|
|/Sites/hybridconnection/delete|Ta bort Hybridanslutningen för Web Apps.|
|/Sites/hybridconnection/Read|Hämta Hybridanslutning för Web Apps.|
|/Sites/hybridconnection/Write|Uppdatera appar Hybrid-anslutning för webbprogram.|
|/Sites/recommendationhistory/Read|Hämta webbhistorik appar rekommendation.|
|/Sites/recommendations/Read|Hämta hello lista över rekommendationer för webbprogrammet.|
|/Sites/recommendations/disable/Action|Inaktivera Web Apps rekommendationer.|
|/Sites/config/Read|Hämta konfigurationsinställningar för webbprogram|
|/Sites/config/List/Action|Visa en lista med Web App känsliga säkerhetsinställningar, till exempel publicera autentiseringsuppgifter, app-inställningar och anslutningssträngar|
|/Sites/config/Write|Uppdatera Web App konfigurationsinställningar|
|/Sites/config/delete|Ta bort Web Apps Config.|
|/Sites/instances/Read|Hämta Web Apps-instanser.|
|/Sites/instances/processes/delete|Ta bort Web Apps instanser processer.|
|/Sites/instances/processes/Read|Hämta Web Apps instanser processer.|
|/Sites/instances/Deployments/Read|Hämta Web Apps instanser distributioner.|
|/Sites/sourcecontrols/Read|Hämta Web App källkontrollen konfigurationsinställningar|
|/Sites/sourcecontrols/Write|Uppdatera inställningar för Web App källkontrollen konfiguration|
|/Sites/sourcecontrols/delete|Ta bort konfigurationsinställningar för Web App käll-kontroll|
|/Sites/Restore/Read|Hämta Web Apps återställning.|
|/Sites/analyzecustomhostname/Read|Analysera anpassade värdnamnet.|
|/Sites/Backups/Read|Hämta hello egenskaper för ett webbprogram säkerhetskopiering|
|/Sites/Backups/List/Action|Lista över Web Apps säkerhetskopiering.|
|/Sites/Backups/Restore/Action|Återställa Web Apps säkerhetskopior.|
|/Sites/Snapshots/Read|Hämta ögonblicksbilder för Web Apps.|
|/Sites/Functions/delete|Ta bort Web Apps funktioner.|
|/Sites/Functions/listsecrets/Action|Lista hemligheter Web Apps funktioner.|
|/Sites/Functions/Read|Hämta Web Apps funktioner.|
|/Sites/Functions/Write|Uppdatera Web Apps funktioner.|
|/Sites/Deployments/delete|Ta bort Web Apps distributioner.|
|/Sites/Deployments/Read|Hämta Web Apps distributioner.|
|/Sites/Deployments/Write|Uppdatera Web Apps distributioner.|
|/Sites/Deployments/log/Read|Hämta Web Apps distributioner loggen.|
|/Sites/Diagnostics/Read|Hämta Web Apps diagnostik.|
|/Sites/Diagnostics/workerprocessrecycle/Read|Hämta Web Apps diagnostik Worker omarbetning av processen.|
|/Sites/Diagnostics/workeravailability/Read|Hämta Web Apps diagnostik Workeravailability.|
|/Sites/Diagnostics/runtimeavailability/Read|Hämta Web Apps diagnostik Runtime tillgänglighet.|
|/Sites/Diagnostics/cpuanalysis/Read|Hämta Web Apps diagnostik Cpuanalysis.|
|/Sites/Diagnostics/servicehealth/Read|Hämta Web Apps diagnostik tjänstens hälsa.|
|/Sites/Diagnostics/frebanalysis/Read|Hämta Web Apps diagnostik FREB analys.|
|/availablestacks/Read|Hämta tillgängliga grupper.|
|/isusernameavailable/Read|Kontrollera om användarnamnet är tillgängliga.|
|/Microsoft.Web/apiManagementAccounts/<br>API: er/läsning|Hämta hello lista över API: er.|
|/Microsoft.Web/apiManagementAccounts/<br>API: er och skrivning|Lägg till ett nytt Api eller uppdatera redan finns.|
|/Microsoft.Web/apiManagementAccounts/<br>API: er/ta bort|Ta bort en befintlig Api.|
|/Microsoft.Web/apiManagementAccounts/<br>Läs-API: er/anslutningar|Hämta hello listan över anslutningar.|
|/Microsoft.Web/apiManagementAccounts/<br>API: er och anslutningar/skrivning|Spara en ny anslutning eller uppdatera en befintlig.|
|/Microsoft.Web/apiManagementAccounts/<br>ta bort-API: er/anslutningar|Ta bort en befintlig anslutning.|
|/Microsoft.Web/apiManagementAccounts/<br>API: er/anslutningar/connectionAcls/läsning|Läs ConnectionAcls|
|/Microsoft.Web/apiManagementAccounts/<br>API: er och anslutningar/connectionAcls/skrivning|Lägg till eller uppdatera ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>API: er/anslutningar/connectionAcls/ta bort|Ta bort ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>Läs-API: er/connectionAcls|Läs ConnectionAcls för Api|
|/Microsoft.Web/apiManagementAccounts/<br>Läs-API: er/apiAcls|Läs ConnectionAcls|
|/Microsoft.Web/apiManagementAccounts/<br>API: er och apiAcls/skrivning|Skapa eller uppdatera Api-ACL: er|
|/Microsoft.Web/apiManagementAccounts/<br>API: er/apiAcls/ta bort|Ta bort Api-ACL: er|
|/ serverfarms/Läs|Få hello egenskaper för en App Service-Plan|
|/ serverfarms/skrivning|Skapa en ny App Service-Plan eller uppdatera en befintlig|
|/ serverfarms/ta bort|Ta bort en befintlig App Service-Plan|
|/serverfarms/restartSites/Action|Starta om alla webbprogram i en Apptjänstplan|
|/serverfarms/operationresults/Read|Hämta App Service-planer Åtgärdsresultat.|
|/serverfarms/Capabilities/Read|Hämta App Service-planer funktioner.|
|/serverfarms/metricdefinitions/Read|Hämta Måttdefinitionerna för App Service-planer.|
|/serverfarms/Metrics/Read|Hämta mätvärden för App Service-planer.|
|/serverfarms/hybridconnectionplanlimits/Read|Hämta Plan för App Service-planer Hybrid anslutningsbegränsningar.|
|/serverfarms/virtualnetworkconnections/Read|Hämta virtuella nätverksanslutningar för App Service-planer.|
|/serverfarms/virtualnetworkconnections/routes/delete|Ta bort Apptjänst-planer virtuella anslutningar nätverksvägar.|
|/serverfarms/virtualnetworkconnections/routes/Read|Hämta App Service-planer virtuella anslutningar nätverksvägar.|
|/serverfarms/virtualnetworkconnections/routes/Write|Uppdatera Apptjänst planer virtuellt nätverk anslutningar vägar.|
|/serverfarms/virtualnetworkconnections/gateways/Write|Uppdatera Apptjänst planer virtuella anslutningar Nätverksgatewayerna.|
|/serverfarms/firstpartyapps/Settings/delete|Ta bort Apptjänst-planer första part appar inställningar.|
|/serverfarms/firstpartyapps/Settings/Read|Hämta Apptjänst första part appar inställningar för energischeman.|
|/serverfarms/firstpartyapps/Settings/Write|Uppdatera Apptjänstplaner första part appar inställningar.|
|/serverfarms/Sites/Read|Hämta App Service-planer Web Apps.|
|/serverfarms/Workers/reboot/Action|Starta om Apptjänst-planer arbetare.|
|/serverfarms/hybridconnectionrelays/Read|Hämta App Service-planer Hybrid anslutning reläer.|
|/serverfarms/skus/Read|Hämta App Service-planer SKU: er.|
|/serverfarms/usages/Read|Hämta App Service-planer användningsområden.|
|/serverfarms/hybridconnectionnamespaces/relays/Sites/Read|Hämta App Service-planer Hybrid anslutning namnområden reläer Web Apps.|
|/ishostnameavailable/Read|Kontrollera om värdnamnet är tillgänglig.|
|/ connectionGateways/Läs|Hämta hello lista över anslutningen Gateways.|
|/ connectionGateways/skrivning|Skapar eller uppdaterar en Gateway med anslutning.|
|/ connectionGateways/ta bort|Tar bort en Gateway med anslutning.|
|/connectionGateways/JOIN/Action|Ansluter till en Gateway med anslutning.|
|/classicmobileservices/Read|Hämta klassiska Mobile Services.|
|/skus/Read|Hämta SKU: er.|
|/ certifikat/Läs|Hämta hello listan över certifikat.|
|/ certifikat/skrivning|Lägg till ett nytt certifikat eller uppdatera en befintlig.|
|/ certifikat/ta bort|Ta bort ett befintligt certifikat.|
|/Operations/Read|Hämta åtgärder.|
|rekommendationer/Läs|Hämta hello lista över rekommendationer för prenumerationer.|
|/ishostingenvironmentnameavailable/Read|Hämta om värd-Miljönamn är tillgängligt.|
|/ apiManagementAccounts/Läs|Hämta hello lista över ApiManagementAccounts.|
|/ apiManagementAccounts/skrivning|Lägg till en ny ApiManagementAccount eller uppdatera en befintlig|
|/ apiManagementAccounts/ta bort|Ta bort en befintlig ApiManagementAccount|
|/apiManagementAccounts/connectionAcls/Read|Hämta hello lista över anslutningen ACL: er.|
|/apiManagementAccounts/apiAcls/Read|Läs ConnectionAcls|
|anslutningar/Läs|Hämta hello listan över anslutningar.|
|anslutningar/skrivning|Skapar eller uppdaterar en anslutning.|
|anslutningar/ta bort|Tar bort en anslutning.|
|/Connections/JOIN/Action|Ansluter till en anslutning.|
|/Connections/confirmconsentcode/Action|Bekräfta anslutningar medgivande kod.|
|/Connections/listconsentlinks/Action|Länkar i listan medgivande för anslutningar.|
|/deploymentlocations/Read|Hämta platser för distributionen.|
|/sourcecontrols/Read|Hämta kontroller för datakällor.|
|/ sourcecontrols/skrivning|Uppdatera källa kontroller.|
|/managedhostingenvironments/Read|Hämta hanterade värdbaserade miljöer.|
|/managedhostingenvironments/Sites/Read|Hämta hanterade värd miljöer Web Apps.|
|/managedhostingenvironments/serverfarms/Read|Hämta hanterade värd miljöer App Service-planer.|
|/Locations/managedapis/Read|Hämta platser hanterade API: er.|
|/Locations/apioperations/Read|Hämta platser API-åtgärder.|
|/Locations/connectiongatewayinstallations/Read|Hämta platser anslutning Gateway installationer.|
|/ listSitesAssignedToHostName/Läs|Hämta namnen på de platser som tilldelats toohostname.|

## <a name="next-steps"></a>Nästa steg

- Lär dig hur för[skapa en anpassad roll](role-based-access-control-custom-roles.md).

- Granska hello [inbyggda RBAC-roller](role-based-access-built-in-roles.md).

- Lär dig hur toomanage åt tilldelningar [av användare](role-based-access-control-manage-assignments.md) eller [per resurs](role-based-access-control-configure.md) 
