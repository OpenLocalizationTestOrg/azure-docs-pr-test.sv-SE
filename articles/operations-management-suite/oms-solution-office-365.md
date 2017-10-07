---
title: "aaaOffice 365 lösning i Operations Management Suite (OMS) | Microsoft Docs"
description: "Den här artikeln innehåller information om konfiguration och användning av hello Office 365-lösning i OMS.  Den innehåller en detaljerad beskrivning av hello Office 365-poster som skapats i logganalys."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: a1507745251ff015abb785bae8352fea7cea0734
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="office-365-solution-in-operations-management-suite-oms"></a>Office 365-lösningen i Operations Management Suite (OMS)

![Office 365-logotyp](media/oms-solution-office-365/icon.png)

hello Office 365-lösningen för Operations Management Suite (OMS) kan du toomonitor din Office 365-miljö i logganalys.  

- Övervaka användaraktiviteter på Office 365 för konton tooanalyze användningsmönster samt identifiera beteendebaserade trender. Du kan exempelvis extrahera av specifika Användningsscenarier, till exempel filer som delas utanför din organisation eller hello populäraste SharePoint-webbplatser.
- Övervaka konfigurationsändringar för administratören aktiviteter tootrack eller Privilegierade åtgärder.
- Identifiera och undersöka oönskade användarbeteende som kan anpassas efter organisationens behov.
- Visa granskning och kompatibilitet. Exempelvis kan du övervaka åtkomståtgärder på konfidentiella filer som kan hjälpa dig med hello gransknings- och efterlevnad processen.
- Utföra operativa felsökning med hjälp av OMS-sökning på Office 365 aktivitetsdata i din organisation.

## <a name="prerequisites"></a>Krav
hello följande är obligatoriska tidigare toothis lösning som ska installeras och konfigureras.

- Organisationens prenumeration på Office 365.
- Autentiseringsuppgifterna för ett konto som är en Global administratör.
- tooreceive granskningsdata måste du [konfigurera granskning](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) i Office 365-prenumeration.  Observera att [postlåda granskning](https://technet.microsoft.com/library/dn879651.aspx) konfigureras separat.  Du kan fortfarande installera hello lösning och samla in andra data om granskning inte har konfigurerats.
 


## <a name="management-packs"></a>Hanteringspaket
Den här lösningen installerar inte några management packs i anslutna hanteringsgrupper.
  

## <a name="configuration"></a>Konfiguration
När du [lägga till hello Office 365-lösningen tooyour prenumeration](../log-analytics/log-analytics-add-solutions.md), du har tooconnect den tooyour Office 365-prenumeration.

1. Lägg till hello Alert Management lösning tooyour OMS-arbetsyta med hjälp av hello process beskrivs i [lägga till lösningar](../log-analytics/log-analytics-add-solutions.md).
2. Gå för**inställningar** i hello OMS-portalen.
3. Under **anslutna källor**väljer **Office 365**.
4. Klicka på **ansluta Office 365**.<br>![Koppla de olika processtegen Office 365](media/oms-solution-office-365/configure.png)
5. Logga in tooOffice 365 med ett konto som är en Global administratör för din prenumeration. 
6. hello prenumeration visas med hello arbetsbelastningar som övervakar hello lösning.<br>![Koppla de olika processtegen Office 365](media/oms-solution-office-365/connected.png) 


## <a name="data-collection"></a>Datainsamling
### <a name="supported-agents"></a>Agenter som stöds
hello Office 365-lösningen inte hämta data från någon av hello [OMS agenter](../log-analytics/log-analytics-data-sources.md).  Den hämtar data direkt från Office 365.

### <a name="collection-frequency"></a>Insamlingsfrekvens
Office 365 skickar en [webhook meddelande](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) med detaljerade uppgifter tooLog Analytics varje gång en post har skapats.

## <a name="using-hello-solution"></a>Med hello-lösning
När du lägger till hello Office 365-lösningen tooyour OMS-arbetsytan hello **Office 365** panelen läggs tooyour OMS-instrumentpanelen. Den här panelen visar antal och grafisk representation av hello antalet datorer i din miljö och deras kompatibilitet.<br><br>
![Panelen för Office 365-sammanfattning](media/oms-solution-office-365/tile.png)  

Klicka på hello **Office 365** panelen tooopen hello **Office 365** instrumentpanelen.

![Office 365-instrumentpanelen](media/oms-solution-office-365/dashboard.png)  

hello instrumentpanelen innehåller hello kolumner i hello i den följande tabellen. Varje kolumnen visar hello översta tio aviseringar efter antal matchande att kolumnens sökvillkor för hello angivna scope och ett tidsintervall. Du kan köra en sökning i loggen som ger hello hela listan genom att klicka på se alla längst hello hello kolumnen eller genom att klicka på kolumnrubriken hello.

| Kolumn | Beskrivning |
|:--|:--|
| Åtgärder | Innehåller information om hello aktiva användare från alla övervakade Office 365-prenumerationer. Du kan också kan toosee hello antalet aktiviteter som sker över tid.
| Exchange | Visar hello uppdelning av Exchange Server-aktiviteter, till exempel Lägg till postlåda behörighet eller Set-postlåda. |
| SharePoint | Visar hello översta aktiviteter att användare utför på SharePoint-dokument. När du går nedåt från den här panelen visar hello söksidan hello detaljerad information om dessa aktiviteter, till exempel hello måldokumentet och hello platsen för den här aktiviteten. Till exempel en komma åt filen händelse du blir kan toosee hello dokument som används, associerade kontonamn och IP-adress. |
| Azure Active Directory | Innehåller översta användaraktiviteter, t.ex återställa användarlösenord och inloggningsförsök. Du kommer att kunna toosee hello information om aktiviteterna som hello resultat när du går nedåt. Detta är mest användbart om du vill toomonitor misstänkta aktiviteter i Azure Active Directory. |




## <a name="log-analytics-records"></a>Log Analytics-poster

Alla poster som skapats i hello logganalys-arbetsytan av hello Office 365-lösningen har en **typen** av **OfficeActivity**.  Hej **OfficeWorkload** egenskapen avgör vilken Office 365-tjänsten hello post refererar för Exchange, AzureActiveDirectory, SharePoint eller OneDrive.  Hej **RecordType** egenskapen anger hello typ av åtgärd.  hello egenskaper varierar för varje åtgärdstyp av och visas i hello tabellerna nedan.

### <a name="common-properties"></a>Gemensamma egenskaper
hello följande egenskaper är gemensamma tooall Office 365-poster.

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ | *OfficeActivity* |
| ClientIP | hello IP-adress hello-enhet som användes när hello aktiviteten har loggats. hello IP-adressen visas i en IPv4- eller IPv6-adress-format. |
| OfficeWorkload | Office 365-tjänst som hello post refererar till.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| Åtgärd | hello namnet på hello användare eller administratör aktiviteten.  |
| Organisations-ID | hello GUID för Office 365-klient för din organisation. Det här värdet kommer alltid att hello samma för din organisation, oavsett hello Office 365-tjänst där den inträffar. |
| RecordType | Typ av av åtgärd utförs. |
| ResultStatus | Anger om hello-åtgärd (anges i hello Åtgärdsegenskap) lyckades eller inte. Möjliga värden är slutfört, PartiallySucceded eller misslyckades. För Exchange-administratören aktiviteten hello-värdet är antingen True eller False. |
| Användar-ID | hello UPN (User Principal Name) för hello-användaren som utförde hello-åtgärd som resulterade i hello post loggas; till exempel my_name@my_domain_name. Observera att posterna för aktivitet som utförs av Systemkonton (till exempel SHAREPOINT\system eller NTAUTHORITY\SYSTEM) ingår också. | 
| UserKey | Ett alternativt ID för hello användare som identifieras i hello användar-ID-egenskapen.  Till exempel fylls den här egenskapen med hello passport unikt ID (PUID) för händelser som utförs av användare i SharePoint, OneDrive för företag och Exchange. Den här egenskapen kan också ange samma värde som hello användar-ID-egenskapen för händelser i andra tjänster och händelser som utförs av systemkonton hello|
| UserType | användaren som utförde åtgärden hello hello typ.<br><br>Admin<br>Program<br>DcAdmin<br>Vanliga<br>Reserverad<br>ServicePrincipal<br>System |


### <a name="azure-active-directory-base"></a>Azure Active Directory-bas
hello följande egenskaper är gemensamma tooall Azure Active Directory-poster.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | hello typ av Azure AD-händelse. |
| extendedProperties | hello utökade egenskaper för hello Azure AD-händelse. |


### <a name="azure-active-directory-account-logon"></a>Inloggning med Azure Active Directory-konto
Dessa poster skapas när Active Directory-användare försöker toolog på.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectoryAccountLogon |
| Program | hello-program som utlöser hello konto inloggningen händelsen, till exempel Office 15. |
| Client | Information om klientens hello enhet, enhetens operativsystem och enhetens webbläsare som användes för hello hello konto inloggningen händelse. |
| loginStatus | Den här egenskapen är direkt från OrgIdLogon.LoginStatus. hello mappningen av olika intressanta inloggningsfel kan göras av aviseringar algoritmer. |
| UserDomain | hello klient identitet Information (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Dessa poster skapas när ändringen eller tillägg görs tooAzure Active Directory-objekt.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | hello-användaren som hello-åtgärd (identifieras av hello Åtgärdsegenskap) utfördes på. |
| aktören | hello användare eller tjänstens huvudnamn som utförts hello-åtgärd. |
| ActorContextId | hello GUID för hello organisation som hello aktören tillhör. |
| ActorIpAddress | Hej skådespelare, IP-adress i IPV4- eller IPV6-adress-format. |
| InterSystemsId | hello GUID som spårar hello-åtgärder mellan komponenter i hello Office 365-tjänsten. |
| IntraSystemId |   hello GUID som genereras av Azure Active Directory tootrack hello åtgärd. |
| SupportTicketId | hello kundsupport biljett-ID för hello-åtgärden i ”act-on-behalf-of” situationer. |
| TargetContextId | hello GUID för hello organisation som hello målanvändare tillhör. |


### <a name="data-center-security"></a>Datacenter-säkerhet
Dessa poster har skapats från granskningsdata Center datasäkerhet.  

| Egenskap | Beskrivning |
|:--- |:--- |
| EffectiveOrganization | hello namnet på hello-klient som hello höjning-cmdlet är riktad mot. |
| ElevationApprovedTime | hello tidsstämpel för när hello höjning har godkänts. |
| ElevationApprover | hello namnet på en Microsoft-hanterare. |
| ElevationDuration | hello varaktighet för vilka hello höjning var aktiv. |
| ElevationRequestId |  En unik identifierare för hello höjning begäran. |
| ElevationRole | hello rollen hello höjning har begärts för. |
| ElevationTime | hello starttid för hello höjning. |
| Starttid | hello starttid för hello cmdlet som körs. |


### <a name="exchange-admin"></a>Exchange-administratören
Dessa poster skapas när ändringar görs tooExchange konfiguration.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeAdmin |
| ExternalAccess |  Anger om hello cmdlet köras av en användare i din organisation, av Microsoft datacenter-personal eller ett tjänstkonto för datacenter eller av en delegerad administratör. hello värdet False anger hello cmdleten kördes av någon i din organisation. hello värdet True anger hello cmdleten kördes av datacenter personal, ett tjänstkonto för datacenter eller en delegerad administratör. |
| ModifiedObjectResolvedName |  Detta är hello eget namn på hello-objekt som har ändrats av hello cmdlet. Detta loggas endast om hello cmdlet ändrar hello-objektet. |
| Organisationsnamn | hello namnet på hello-klient. |
| OriginatingServer | hello namnet på hello server från vilken hello cmdleten kördes. |
| Parametrar | hello namn och värde för alla parametrar som användes med hello-cmdlet som identifieras i hello Operations-egenskapen. |


### <a name="exchange-mailbox"></a>Exchange-postlåda
Dessa poster skapas när ändringar eller tillägg görs tooExchange postlådor.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | Information om hello e-postklient som var används tooperform hello åtgärden, till exempel en webbläsarversion, Outlook-versionen och information för mobila enheter. |
| Client_IPAddress | hello IP-adress hello-enhet som användes när hello åtgärden loggades. hello IP-adressen visas i en IPv4- eller IPv6-adress-format. |
| ClientMachineName | hello datornamn som är värd för hello Outlook-klienten. |
| ClientProcessName | hello e-postklient som har använt tooaccess hello postlåda. |
| ClientVersion | hello version av hello e-postklient. |
| InternalLogonType | Reserverat för intern användning. |
| Logon_Type | Anger hello typ av användare som nås hello postlåda och utfört hello-åtgärd som loggades. |
| LogonUserDisplayName |    hello användarvänligt namn på hello användare som utförde åtgärden hello. |
| LogonUserSid | hello hello användarens SID som utförde åtgärden hello. |
| MailboxGuid | hello Exchange GUID för hello-postlåda som användes. |
| MailboxOwnerMasterAccountSid | Postlåda ägare konto master kontots SID. |
| MailboxOwnerSid | hello SID för hello postlådans ägare. |
| MailboxOwnerUPN | hello e-postadress hello personen som äger hello-postlåda som användes. |


### <a name="exchange-mailbox-audit"></a>Exchange-postlåda granskning
Dessa poster skapas när en postlåda granskningspost skapas.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| Objekt | Representerar hello-objektet vid vilka hello åtgärden utfördes | 
| SendAsUserMailboxGuid | hello Exchange GUID för hello-postlåda som har åtkomst till toosend e-post som. |
| SendAsUserSmtp | SMTP-adress för hello-användare som har personifierats. |
| SendonBehalfOfUserMailboxGuid | hello Exchange GUID för hello-postlåda som har åtkomst till toosend e-post för. |
| SendOnBehalfOfUserSmtp | SMTP-adress för hello användare på vars räkning hello e-post skickas. |


### <a name="exchange-mailbox-audit-group"></a>Exchange-postlåda Audit grupp
Dessa poster skapas när ändringar eller tillägg görs tooExchange grupper.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Information om varje objekt i hello grupp. |
| CrossMailboxOperations | Anger om hello åtgärden involverad mer än en postlåda. |
| DestMailboxId | Ange endast om hello CrossMailboxOperations parametern är True. Anger hello mål postlåda GUID. |
| DestMailboxOwnerMasterAccountSid | Ange endast om hello CrossMailboxOperations parametern är True. Anger hello SID för hello master kontots SID av hello mål postlådans ägare. |
| DestMailboxOwnerSid | Ange endast om hello CrossMailboxOperations parametern är True. Anger hello SID av hello mål postlåda. |
| DestMailboxOwnerUPN | Ange endast om hello CrossMailboxOperations parametern är True. Anger hello UPN för hello ägare av hello mål postlåda. |
| DestFolder | hello målmappen för till exempel flytta. |
| Mapp | hello mapp som innehåller en grupp med objekt. |
| Mappar |     Information om hello källa mappar ingår i en funktion. Om exempelvis mappar valts och sedan ta bort. |


### <a name="sharepoint-base"></a>SharePoint-bas
Dessa egenskaper är gemensamma tooall SharePoint-poster.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| eventSource | Identifierar att en händelse har inträffat i SharePoint. Möjliga värden är SharePoint eller ObjectModel. |
| itemType | hello typ av objekt som har åtkomst till eller ändrats. Se hello ItemType tabell för information om hello typer av objekt. |
| MachineDomainInfo | Information om enheten synkroniseringsåtgärder. Denna information rapporteras endast om den finns i hello-begäran. |
| MachineId |   Information om enheten synkroniseringsåtgärder. Denna information rapporteras endast om den finns i hello-begäran. |
| Site_ | hello GUID för hello plats där hello fil eller mapp som kan nås av hello användaren finns. |
| Source_Name | hello entitet som utlöste hello granskas igen. Möjliga värden är SharePoint eller ObjectModel. |
| UserAgent | Information om hello användarens klienten eller webbläsare. Denna information tillhandahålls av hello klient eller webbläsare. |


### <a name="sharepoint-schema"></a>SharePoint-Schema
Dessa poster skapas när konfigurationsändringar görs tooSharePoint.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Valfri sträng för anpassade händelser. |
| Event_Data |  Valfria nyttolasten för anpassade händelser. |
| ModifiedProperties | hello egenskapen ingår för admin-händelser, till exempel lägga till en användare som en medlem av en webbplats eller en samling administratörsgrupp. hello-egenskapen innehåller hello namnet på hello-egenskap som har ändrats (till exempel, hello webbplatsadministratör ge gruppen) hello nya värdet för hello ändrade egenskapen (sådana hello användare har lagts till som en plats-administratör) och hello tidigare värde för hello ändrade objektet. |


### <a name="sharepoint-file-operations"></a>SharePoint-filåtgärder
Dessa poster skapas i svaret toofile åtgärder i SharePoint.

| Egenskap | Beskrivning |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | hello filnamnstillägget för en fil som kopieras eller flyttas. Den här egenskapen visas endast för FileCopied och FileMoved händelser. |
| DestinationFileName | hello namnet på hello-fil som kopieras eller flyttas. Den här egenskapen visas endast för FileCopied och FileMoved händelser. |
| DestinationRelativeUrl | hello-URL för hello målmappen där en fil kopieras eller flyttas. hello kombinationer av hello värden för parametrarna SiteURL, DestinationRelativeURL och DestinationFileName är hello samma som hello värde för hello objekt-ID-egenskap som är hello fullständig sökväg för hello-fil som har kopierats. Den här egenskapen visas endast för FileCopied och FileMoved händelser. |
| SharingType | hello typ av behörighet som tilldelats toohello användaren som hello resurs delades med. Den här användaren identifieras av hello UserSharedWith parameter. |
| Site_Url | hello-URL för hello-webbplatsen där hello fil eller mapp som kan nås av hello användaren finns. |
| SourceFileExtension | hello filnamnstillägget hello-fil som användes av hello användare. Den här egenskapen är tom om hello-objektet som öppnade är en mapp. |
| SourceFileName |  hello namnet på hello fil eller mapp som kan nås av hello användaren. |
| SourceRelativeUrl | hello-URL för hello-mappen som innehåller hello-fil som kan nås av hello användaren. hello kombinationer av hello värden för parametrarna för hello SiteURL, SourceRelativeURL och SourceFileName är hello samma som hello värde för hello objekt-ID-egenskap som är hello fullständig sökväg för hello-fil som kan nås av hello användaren. |
| UserSharedWith |  hello-användaren som en resurs har delat med. |




## <a name="sample-log-searches"></a>Exempel på loggsökningar
hello följande tabell innehåller exempel loggen söker efter uppdateringen innehåller information som samlas in av den här lösningen.

| Fråga | Beskrivning |
| --- | --- |
|Antal alla hello åtgärder på Office 365-prenumeration |' Type = OfficeActivity | mäter count() för åtgärden ' |
|Användning av SharePoint-webbplatser|' Type = OfficeActivity OfficeWorkload = sharepoint | måttet count() som antal av SiteUrl | Sortera antal asc'|
|Filåtgärder för åtkomst av typ|' Type = OfficeActivity OfficeWorkload = sharepoint åtgärden = FileAccessed | mäter count() av UserType'|
|Söka med ett specifikt nyckelord|`Type=OfficeActivity OfficeWorkload=azureactivedirectory "MyTest"`|
|Övervaka externa åtgärder i Exchange|`Type=OfficeActivity OfficeWorkload=exchange ExternalAccess = true`|



## <a name="troubleshooting"></a>Felsökning

Om din Office 365-lösningen inte samlar in data som förväntat, kontrollera statusen på hello OMS-portalen på **inställningar** -> **anslutna källor** -> **Office 365** . hello i den följande tabellen beskrivs varje status.

| Status | Beskrivning |
|:--|:--|
| Active | hello Office 365-prenumeration är aktiv och hello arbetsbelastningen är ansluten tooyour OMS-arbetsyta. |
| Väntande åtgärder | hello Office 365-prenumeration är aktiv men hello arbetsbelastning ännu inte är ansluten tooyour OMS-arbetsyta. hello första gången du ansluter hello Office 365-prenumeration, alla hello arbetsbelastningar kommer att vara den här statusen tills de har anslutits. Vänta 24 timmar för alla hello arbetsbelastningar tooswitch tooActive. |
| Inaktiva | hello Office 365-prenumeration är i ett inaktivt tillstånd. Kontrollera din Office 365 Admin information på sidan. När du har aktiverat din Office 365-prenumeration Avlänka från din OMS-arbetsyta och länka det igen toostart som tar emot data. |



## <a name="next-steps"></a>Nästa steg
* Använd loggen söker i [logganalys](../log-analytics/log-analytics-log-searches.md) tooview detaljerad data för uppdateringen.
* [Skapa dina egna instrumentpaneler](../log-analytics/log-analytics-dashboards.md) toodisplay din favorit sökfrågor för Office 365.
* [Skapa aviseringar](../log-analytics/log-analytics-alerts.md) toobe proaktivt meddelande om viktiga Office 365-aktiviteter.  
