---
title: "aaaOperating systemets funktion på Azure App Service"
description: "Lär dig mer om hello OS funktioner tillgängliga tooweb appar, serverdelar för mobilappar och API apps i Azure App Service"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 695188c48102b10ece326fba8d50a5f55b03b003
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="operating-system-functionality-on-azure-app-service"></a>Operativsystemet funktioner i Azure App Service
Den här artikeln beskriver hello vanliga baslinjen operativsystemet funktioner som är tillgängliga tooall appar som körs på [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Den här funktionen innehåller filen, nätverket och till registret och diagnostik loggar och händelser. 

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a>App Service-plan nivåer
App Service kör kunden appar på en värd för flera innehavare. Appar som distribuerats i hello **lediga** och **delade** nivåer som körs i arbetsprocesser på den delade virtuella datorer när program som distribueras i hello **Standard** och  **Premium** nivåer som körs på virtuella datorer som är dedikerad för hello appar som är associerade med en kund.

Eftersom Apptjänst har stöd för skalning smidigt mellan olika nivåer, tillämpas hello säkerhetskonfiguration för Apptjänst-appar förblir hello samma. Detta säkerställer att appar inte plötsligt fungerar annorlunda, misslyckas på oväntade sätt när App Service-plan som växlar från ett lager tooanother.

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a>Utvecklingsramverk
Apptjänst priser nivåer kontrollen hello mängden beräkningsresurser (CPU, diskutrymme, minne och nätverk utgång) tillgängliga tooapps. Hello breda framework funktioner tillgängliga tooapps förblir dock hello samma oavsett hello skalning nivåer.

Apptjänst har stöd för utvecklingsramverk, bland annat ASP.NET, klassisk ASP, node.js, PHP och Python - alla körs som tillägg i IIS. I ordning toosimplify och normalisera säkerhetskonfiguration, Apptjänst-appar vanligtvis köra hello utvecklingsramverk för olika med standardinställningarna. En metod som tooconfiguring appar kunde har toocustomize hello API-ytan och funktioner för varje enskild utvecklingsramverk. Apptjänst använder i stället en generisk metod genom att aktivera en gemensam baslinje för operativsystemet funktioner oavsett utvecklingsramverk för en app.

hello sammanfattar följande avsnitt hello allmänna typer av operativsystem-funktioner tillgängliga tooApp appar.

<a id="FileAccess"></a>

## <a name="file-access"></a>Åtkomst till filen
Olika enheter finns i Apptjänst, bland annat lokala enheter och nätverksenheter.

<a id="LocalDrives"></a>

### <a name="local-drives"></a>Lokala enheter
Kärnan är Apptjänst en tjänst som körs ovanpå hello Azure PaaS (plattform som en tjänst)-infrastruktur. Som ett resultat, hello lokala enheter som är ”anslutna” tooa virtuella hello samma enhet typer tillgängliga tooany worker-rollen körs i Azure. Detta inkluderar en operativsystemenhet (hello D:\ enhet), ett program-enhet som innehåller Azure cspkg paketfilerna används endast av App Service (och tillgänglig toocustomers) och en ”användare” enhet (hello enhet C:\), vars storlek varierar beroende på hello storlek för hello VM.

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a>Nätverksenhet (aka UNC delar)
En av hello unika aspekter av App Service som gör app-distribution och underhåll enkla är att alla användarinnehåll lagras på en uppsättning UNC-resurser. Den här modellen mappar mycket snyggt toohello vanliga mönstret för lagring av innehåll används av lokala värdbaserade miljöer som har flera servrar för Utjämning av nätverksbelastning. 

I App Service finns antal UNC-resurser som har skapats i varje datacenter. En del av hello användarinnehåll för alla kunder i varje Datacenter fördelas tooeach UNC-resurs. Dessutom alla hello filinnehåll för en enskild kundens prenumeration är alltid placerad hello samma UNC-resurs. 

Observera att på grund av toohow molntjänster fungerar, hello specifik virtuell dator ansvarar för en UNC-resurs ändras med tiden. Det är säkert att UNC-resurser ska monteras av olika virtuella datorer som de förs upp och ned under hello normala molnet operations. Därför bör aldrig appar Se hårdkodade antaganden att hello informationen för datorn på en UNC-sökväg till filen förblir stabil över tid. I stället de ska använda hello praktiskt *faux* absolut sökväg **D:\home\site** som App Service tillhandahåller. Den här faux absolut sökväg ger en bärbar, oberoende för app-användare metod för att referera tooone's egna app. Med hjälp av **D:\home\site**, en kan överföra delade filer från appen tooapp utan tooconfigure en ny absolut sökväg för varje överföring.

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-tooan-app"></a>Typer av åtkomst till filen beviljas tooan app
Varje kund prenumeration har en reserverad katalogstruktur på en specifik UNC-resurs i ett datacenter. En kund kan ha flera appar som har skapats i en specifik datacenter, så alla hello kataloger som hör tooa kund prenumeration skapas på hello samma UNC-resurs. hello resursen kan omfatta kataloger som de för innehåll, fel och diagnostikloggar och tidigare versioner av hello-app som skapats av källkontroll. Som förväntat, finns en kund app kataloger för Läs- och skrivbehörighet vid körning av hello app programkod.

På hello lokala enheter kopplade toohello virtuell dator som kör en app, Apptjänst reserverar segment utrymme på hello enhet C:\ för app-specifik tillfälliga lokal lagring. Även om en app har fullständig åtkomst tooits äger tillfälliga lokal lagring, som lagring är det inte avsedda toobe användas direkt av hello programkod. Hello avsikt är snarare tooprovide tillfällig lagring för IIS- och programramverk. Apptjänst också begränsar hello tillfälliga lokal lagring tillgänglig tooeach app tooprevent enskilda appar från att konsumera alltför stora mängder lagring av lokala filer.

Två exempel på hur Apptjänst använder temporära lokal lagring är hello katalogen för temporära ASP.NET-filer och hello katalog för IIS komprimerade filer. hello ASP.NET-kompilering system använder hello ”temporära ASP.NET-filer” directory som en tillfällig kompilering cacheplats. IIS använder hello ”IIS tillfälliga komprimerade filer” directory toostore komprimeras svarsutdata. Båda typerna av filen användning (samt andra) mappas i Apptjänst tooper app tillfälliga lokal lagring. Den här ommappning säkerställer att funktioner fortfarande som förväntat.

Varje app i App Service körs som en slumpmässig unika lågprivilegierat arbetsprocessidentiteten kallas hello ”programpoolsidentiteten”, beskrivs mer här: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities ](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Programkod som använder den här identiteten för grundläggande skrivskyddad åtkomst toohello operativsystemenheten (hello D:\ enhet). Detta innebär att programkod kan visa en lista med vanliga katalogstrukturer och läsa vanliga filer på enheten med operativsystemet. Även om detta kan visas toobe något bred åtkomstnivå, hello samma kataloger och filer är tillgängliga när du etablerar en arbetsroll i en Azure värdtjänsten och läsa hello enhet innehållet. 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a>Åtkomst till filen på flera instanser
programkod kan skriva tooit hello arbetskatalog innehåller en app innehåll. Om en app som körs på flera instanser, hello hemkatalog delas med alla instanser så att alla instanser finns hello samma katalog. Så, till exempel om en app sparar överförda filer toohello hemkatalog, dessa filer är omedelbart tillgängligt tooall instanser. 

<a id="NetworkAccess"></a>

## <a name="network-access"></a>Åtkomst till nätverket
Programkod kan använda TCP/IP och UDP-baserat protokoll toomake utgående nätverk anslutningar tooInternet tillgänglig slutpunkter som exponerar externa tjänster. Appar kan använda dessa samma protokoll tooconnect tooservices inom Azure & #151, till exempel genom att etablera HTTPS-anslutningar tooSQL databas.

Det finns också en begränsad funktion för appar tooestablish en lokal loopback-anslutning och har en app som lyssnar på den lokala loopback-socketen. Den här funktionen finns i första hand tooenable appar som lyssnar på lokal loopback sockets som en del av deras funktioner. Observera att varje app ser en ”privat” loopback-anslutning. App ”A” kan inte lyssna tooa lokal loopback socket upprättas av app ”B”.

Namngivna pipes stöds också som en mekanism för kommunikation mellan processer (IPC) mellan olika processer som gemensamt kör en app. Till exempel använder hello IIS FastCGI-modulen namngivna pipes toocoordinate hello enskilda processer som körs PHP-sidor.

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a>Kodkörning, processer och minne
Som tidigare nämnts kör appar inuti lågprivilegierat arbetsprocesser med en slumpmässig programpoolsidentiteten. Programkod har åtkomst toohello minnesutrymme som är associerade med hello arbetsprocessen och alla underordnade processer som kan startas av CGI-processer eller andra program. Dock en app kan inte komma åt hello minne eller data i en annan app även om den finns på hello samma virtuella dator.

Appar kan köra skript eller sidor som skrivits med stöds web utvecklingsramverk. Apptjänst Konfigurera inte eventuella web framework inställningar toomore begränsad lägen. ASP.NET-appar som körs på App Service kör till exempel i ”full” förtroende som skillnad från tooa mer begränsat förtroende. Web ramverk, inklusive både klassiska ASP och ASP.NET kan anropa pågående COM-komponenter (men inte out-of-process COM-komponenter) som ADO (ActiveX Data Objects) som är registrerade som standard på hello Windows-operativsystem.

Appar kan starta och köra skadlig kod. Det är tillåtet för en app toodo saker som att starta en kommandotolk eller köra ett PowerShell-skript. Men även om skadlig kod och processer som startas från en app, körbara program och skript är fortfarande begränsat toohello behörigheter beviljas toohello överordnade programpoolen. En app kan till exempel starta en körbar fil som gör en utgående HTTP-anrop, men den inte kan samma körbara försöker toounbind hello IP-adressen för en virtuell dator från dess nätverkskort. Samtal utgående nätverket tillåts toolow privilegierad kod, men försök tooreconfigure nätverksinställningar på en virtuell dator kräver administratörsbehörighet.

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a>Diagnostik loggar och händelser
Logginformation är en annan uppsättning data att vissa appar försöka tooaccess. hello typer av loggen information tillgänglig toocode körs i Apptjänst innehåller diagnostik och logga information som genererats av en app som också är lättillgänglig toohello app. 

Till exempel W3C HTTP-loggar som genereras av en aktiv app är tillgängliga antingen på en loggkatalog i hello nätverk dela plats som skapats för hello app eller tillgängliga i blob storage om en kund har ställt in toostorage W3C-loggning. hello senare alternativet gör att stora mängder loggar toobe samlas in utan hello risken för att överskrida lagringsgränser för hello-fil som är kopplad till en nätverksresurs.

I en liknande vein realtid diagnostik information från .NET-appar kan också loggas med alternativ toowrite hello .NET-spårning och diagnostik infrastruktur, hello spåra information tooeither hello appens nätverksresurs eller alternativt tooa blob-lagring plats.

Delar av diagnostik loggning och spårning som inte är tillgänglig tooapps är Windows ETW-händelser och vanliga Windows-händelseloggar (t.ex., program händelseloggarna System och säkerhet). Eftersom ETW spårningsinformation kan vara synlig blockeras datoromfattande (med hello höger ACL: er), Läs- och skrivbehörighet tooETW händelser. Utvecklare märker att API-anrop tooread och skriva ETW-händelser och vanliga Windows-händelseloggar visas toowork men beror Apptjänst ”faking” hello anrop så att de visas toosucceed. I själva verket har hello programkod inga åtkomst toothis händelsedata.

<a id="RegistryAccess"></a>

## <a name="registry-access"></a>Registeråtkomst
Appar som har skrivskyddad åtkomst toomuch (även om inte alla) av hello registrering av hello virtuella datorn körs på. I praktiken innebär detta registernycklar som tillåter läsbehörighet toohello lokala användargruppen är tillgängliga för appar. En del av hello register som inte stöds för närvarande för Läs- eller skrivbehörighet är hello HKEY\_aktuella\_användarstruktur.

Skrivåtkomst toohello registret är blockerad, inklusive registernycklarna för åtkomst tooany per användare. Hello app ur skrivbehörighet toohello registret ska aldrig förlita sig på i hello Azure-miljön eftersom appar kan (och göra) migreras mellan olika virtuella datorer. hello endast beständig skrivbara lagring som kan vara beroende av en app är hello per app innehåll katalogstruktur lagras på hello App Service UNC delar. 

## <a name="more-information"></a>Mer information

[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) -hello de flesta uppdaterad information om hello körningsmiljö av App Service. Den här sidan hanteras direkt av hello Apptjänst Utvecklingsteamet.

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 


