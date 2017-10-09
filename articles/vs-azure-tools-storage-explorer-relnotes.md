---
title: "viktig information för aaaMicrosoft Azure Lagringsutforskaren (förhandsversion) | Microsoft Docs"
description: "Viktig information för Microsoft Azure Lagringsutforskaren (förhandsversion)"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: release-notes
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 44ff6dc8e2015f4eb71fa13098b832fbbf48ccac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a>Viktig information för Microsoft Azure Lagringsutforskaren (förhandsversion)

Den här artikeln innehåller hello versionen viktig information för Azure Lagringsutforskaren 0.8.16 (förhandsgranskning) samt viktig information för tidigare versioner.

[Microsoft Azure Lagringsutforskaren (förhandsversion)](./vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux.

## <a name="version-0816-preview"></a>Version 0.8.16 (förhandsgranskning)
8/21/2017

### <a name="download-azure-storage-explorer-0816-preview"></a>Hämta Azure Lagringsutforskaren 0.8.16 (förhandsgranskning)
- [Azure Lagringsutforskaren 0.8.16 (förhandsversion) för Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Lagringsutforskaren (förhandsversion) för 0.8.16 för Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Lagringsutforskaren (förhandsversion) för 0.8.16 för Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Ny
* När du öppnar en blob uppmanas Lagringsutforskaren du tooupload hello ned filen om en ändring identifieras
* Förbättrad Azure Stack inloggning
* Förbättrad hello prestanda för att ladda upp/hämta många små filer på hello samma tid


### <a name="fixes"></a>Korrigeringar
* För vissa typer av blob leder väljer för ”Ersätt” under en överför hamnar i konflikt ibland hello överför startas. 
* I version 0.8.15 skulle överföringar ibland av stopp vid 99%.
* När du överför filer tooa filresurs, om du väljer tooupload tooa directory som ännu inte finns, misslyckas överföra.
* Lagringsutforskaren felaktigt generera tidsstämplar för signaturer för delad åtkomst och tabellen.


Kända problem
* Med hjälp av ett namn och nyckel anslutningssträngen fungerar inte för närvarande. Den kommer att åtgärdas i hello nästa utgåva. Fram till dess kan du använda bifoga med namn och nyckel.
* Om du försöker tooopen en fil med ett ogiltigt filnamn för Windows resulterar hello download i en fil hittades inte.
* När du klickar på ”Avbryt” för en uppgift, kan det ta ett tag för att aktiviteten toocancel. Detta är en begränsning för hello Azure lagringsnod bibliotek.
* När du har slutfört en blob-överför uppdateras hello fliken som initierade överföringen hello. Detta är en förändring jämfört med tidigare beteende och medför även du toobe tas tillbaka toohello rot hello behållare i.
* Om du väljer hello fel PIN-kod/smartkort-certifikat och du behöver toorestart i ordning toohave Lagringsutforskaren Glöm detta beslut.
* hello konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter toofilter prenumerationer.
* Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder. Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.
* Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.
* För användare på Ubuntu 14.04 måste tooensure GCC är toodate – kan du göra det genom att köra hello följande kommandon och sedan starta om datorn:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Du behöver tooinstall GConf – detta kan göras genom att köra hello följande kommandon och sedan starta om datorn för användare på Ubuntu nr 17.04 från:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0814-preview"></a>Version 0.8.14 (förhandsgranskning)
06/22/2017

### <a name="download-azure-storage-explorer-0814-preview"></a>Hämta Azure Lagringsutforskaren 0.8.14 (förhandsgranskning)
* [Hämta Azure Lagringsutforskaren 0.8.14 (förhandsversion) för Windows](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Hämta Azure Lagringsutforskaren (förhandsversion) för 0.8.14 för Mac](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Hämta Azure Lagringsutforskaren (förhandsversion) för 0.8.14 för Linux](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a>Ny

* Uppdaterade Electron version too1.7.2 i ordning tootake nytta av flera viktiga säkerhetsuppdateringar
* Du kan snabbt ansluta hello online felsökningsguiden hello Hjälp-menyn
* Lagringsutforskaren felsökning [Guide][2]
* [Instruktioner] [ 3] ansluta tooan Stack för Azure-prenumeration

### <a name="known-issues"></a>Kända problem

* Knapparna i hello ta bort mappen bekräftelsedialogruta registrera inte med hello musklickningar på Linux. Lösningen är toouse hello RETUR-tangenten
* Om du väljer hello fel PIN-kod/smartkortcertifikat sedan behöver du toorestart i ordning toohave Lagringsutforskaren Glöm hello beslut
* Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel
* hello konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter i ordning toofilter prenumerationer
* Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder. Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.
* Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto. 
* Ubuntu 14.04 installera behov gcc version uppdateras eller uppgraderas – steg tooupgrade finns nedan:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```




## <a name="previous-releases"></a>Tidigare versioner

* [Version 0.8.13](#version-0813)
* [Version 0.8.12 / 0.8.11 / 0.8.10](#version-0812--0811--0810)
* [Version 0.8.9 / 0.8.8](#version-089--088)
* [Version 0.8.7](#version-087)
* [Version 0.8.6](#version-086)
* [Version 0.8.5](#version-085)
* [Version 0.8.4](#version-084)
* [Version 0.8.3](#version-083)
* [Version 0.8.2](#version-082)
* [Version 0.8.0](#version-080)
* [Version 0.7.20160509.0](#version-07201605090)
* [Version 0.7.20160325.0](#version-07201603250)
* [Version 0.7.20160129.1](#version-07201601291)
* [Version 0.7.20160105.0](#version-07201601050)
* [Version 0.7.20151116.0](#version-07201511160)


### <a name="version-0813"></a>Version 0.8.13
05/12/2017

#### <a name="new"></a>Ny

* Lagringsutforskaren felsökning [Guide][2]
* [Instruktioner] [ 3] ansluta tooan Stack för Azure-prenumeration

#### <a name="fixes"></a>Korrigeringar

* Fast: Ladda upp filen hunnit hög orsaka en out-of-minnesfel
* Fast: Du kan nu logga in med PIN-kod/smartkort
* Fast: Öppna i portalen nu fungerar med Azure Kina, Tyskland Azure, Azure som tillhör amerikanska myndigheter och Azure-stacken
* Fast: Går inte att överföra en mapp tooa blob-behållare, ”otillåten åtgärd” skulle ibland uppstå fel
* Fast: Markera alla inaktiverades vid hantering av ögonblicksbilder
* Fast: hello metadata för grundläggande hello-blob kan komma att skrivas över när du har visat hello egenskaper av dess ögonblicksbilder

#### <a name="known-issues"></a>Kända problem

* Om du väljer hello fel PIN-kod/smartkortcertifikat sedan behöver du toorestart i ordning toohave Lagringsutforskaren Glöm hello beslut
* När du har zoomat in eller ut får hello zoomningsnivån tillfälligt återställa toohello Standardnivå
* Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel
* hello konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter i ordning toofilter prenumerationer
* Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder. Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.
* Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto. 
* Ubuntu 14.04 installera behov gcc version uppdateras eller uppgraderas – steg tooupgrade finns nedan:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a>Version 0.8.12 / 0.8.11 / 0.8.10
04/07/2017

#### <a name="new"></a>Ny

* Lagringsutforskaren stängs nu automatiskt när du installerar en uppdatering från hello uppdateringsmeddelande
* Lokalt Snabbåtkomst ger en förbättrad upplevelse för att arbeta med resurserna används ofta
* I Redigeraren för hello Blob-behållare, kan du nu se vilken virtuell dator som tillhör en lånade blob
* Du kan nu komprimera hello vänster panel
* Identifiering nu körs vid hello samma tid som nedladdning
* Använd statistik i hello Blob-behållaren och filresursen tabell redigerare toosee hello storleken på ditt val eller resurs
* Du kan nu logga in tooAzure Active Directory (AAD) baserad Azure Stack-konton. 
* Du kan nu arkivera Överför filer över 32 MB tooPremium storage-konton
* Förbättrat stöd för hjälpmedel
* Nu kan du lägga till betrodda Base64-kodat X.509-SSL-certifikat genom att gå tooEdit -&gt; SSL-certifikat -&gt; Importera certifikat

#### <a name="fixes"></a>Korrigeringar

* Fast: när du har uppdaterat en kontoautentiseringsuppgifter hello trädvyn kan ibland uppdateras inte automatiskt
* Fast: Generera en SAS för emulatorn köer och tabeller skulle leda till en ogiltig URL
* Fast: premium storage-konton kan nu utökas när en proxy är aktiverat
* Fast: hello knappen Tillämpa på hello konton hanteringssidan inte fungerar om du hade 1 eller 0 konton som har valts
* Fast: överföring av blobbar som kräver konfliktlösningar kan misslyckas - fast i 0.8.11 
* Fast: skicka feedback har brutits i 0.8.11 - fast i 0.8.12 

#### <a name="known-issues"></a>Kända problem

* När du har uppgraderat too0.8.10 toorefresh måste alla dina autentiseringsuppgifter.
* När du har zoomat in eller ut kan tillfälligt hello zoomningsnivån återställa toohello Standardnivå.
* Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel.
* hello konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter i ordning toofilter prenumerationer.
* Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder. Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.
* Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto. 
* Ubuntu 14.04 installera behov gcc version uppdateras eller uppgraderas – steg tooupgrade finns nedan:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a>Version 0.8.9 / 0.8.8
02/23/2017

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6gonK3cYAc?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrRPCm94mfE?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a>Ny

* Lagringsutforskaren 0.8.9 kommer automatiskt att hämta hello senaste versionen för uppdateringar.
* Snabbkorrigering: med hjälp av en portal generera SAS URI tooattach ett lagringskonto skulle resultera i ett fel.
* Du kan nu skapa, hantera och främja blob ögonblicksbilder.
* Du kan nu logga in tooAzure Kina, Azure Tyskland och Azure som tillhör amerikanska myndigheter konton.
* Du kan nu ändra hello zoomningsnivån. Använd hello alternativen i hello Visa-menyn tooZoom i Zooma ut och Återställ Zoom.
* Unicode-tecken stöds nu i Användarmetadata för blobbar och filer.
* Hjälpmedelsförbättringar.
* hello nästa version viktig information kan visas från hello uppdateringsmeddelande. Du kan också visa hello aktuella viktig information hello Hjälp-menyn.

#### <a name="fixes"></a>Korrigeringar

* Fast: hello versionsnumret nu visas korrekt på Kontrollpanelen i Windows
* Fast: sökning är inte längre begränsade too50, 000 noder
* Fast: Överför tooa filresurs roterade alltid hello målkatalogen inte redan finns
* Fast: förbättrad stabilitet för lång överföring

#### <a name="known-issues"></a>Kända problem

* När du har zoomat in eller ut kan tillfälligt hello zoomningsnivån återställa toohello Standardnivå.
* Snabbåtkomst fungerar bara med prenumerationen baserat objekt. Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen.
* Det kan ta några sekunder toonavigate toohello målresurs, beroende på hur många resurser som du har Snabbåtkomst.
* Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel.

12/16/2016
### <a name="version-087"></a>Version 0.8.7

<iframe width="560" height="315" src="https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Ny

* Du kan välja hur tooresolve konflikter hello början av en uppdatering, hämta eller kopiera session i hello aktiviteter fönster
* Hovra över en fliken toosee hello fullständig sökväg till hello lagringsresurs
* När du klickar på en flik, synkronisering och dess plats i navigeringsfönstret för hello vänster

#### <a name="fixes"></a>Korrigeringar

* Fast: Lagringsutforskaren är nu ett betrott app på Mac
* Fast: Ubuntu 14.04 igen stöds
* Fast: Ibland hello Lägg till konto UI blinkar när inläsning av prenumerationer
* Fast: Ibland inte alla lagringsresurser ingick i navigeringsfönstret för hello vänster
* Fast: hello åtgärdsfönstret ibland visas tom åtgärder
* Fast: hello fönsterstorlek från hello senast stängdes session nu sparas
* Fast: Du kan öppna flera flikar för hello samma resurs med hello snabbmenyn

#### <a name="known-issues"></a>Kända problem

* Snabbåtkomst fungerar bara med prenumerationen baserat objekt. Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen
* Det kan ta Snabbåtkomst några sekunder toonavigate toohello målresurs, beroende på hur många resurser som du har
* Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel
* Referenser för sökning efter det att söka i ungefär 50 000 noder - prestanda kan påverkas eller kan orsaka undantag
* För hello första gången du använder hello Lagringsutforskaren på macOS, kan det finnas flera prompter som ber om användarens behörighet tooaccess nyckelringar. Vi rekommenderar att du väljer Tillåt alltid så hello prompten inte visas igen

11/18/2016
### <a name="version-086"></a>Version 0.8.6

#### <a name="new"></a>Ny

* Du kan nu PIN-kod används oftast services toohello Snabbåtkomst för enkel navigering
* Nu kan du öppna flera redigerare på olika flikar. Enkelklickning tooopen tillfälliga fliken. Dubbelklicka på tooopen en permanent flik. Du kan också klicka på hello tillfälliga fliken toomake den permanenta fliken
* Vi har gjort märkbar prestanda och stabilitetsförbättringar för överföring och hämtning, särskilt för stora filer på snabb datorer
* Nu kan skapa tomma ”virtuell” mappar i blob-behållare
* Vi har nytt introducerades bred sökning med vår nya förbättrad sökningen så att du nu har två alternativ för att söka efter: 
    * Globala Sök - bara ange en sökterm i hello Sök textruta
    * Bred sökning - Klicka på hello förstoringsglas ikonen nästa tooa nod, och sedan lägga till en sökning termen toohello end hello sökväg eller högerklicka och välj ”Sök från hit”
* Vi har lagt till olika teman: ljus (standard), mörkt, hög kontrast svart och hög kontrast vit. Gå tooEdit -&gt; teman toochange önskemål teman
* Du kan ändra Blob och filegenskaper
* Vi stöder nu kodade (base64) och kodat Kömeddelanden
* På Linux för, ett 64-bitars operativsystem är nu krävs. Den här versionen stöder vi endast 64-bitars Ubuntu 16.04.1 LTS
* Vi har uppdaterat vår logotypen!

#### <a name="fixes"></a>Korrigeringar

* Fast: Skärmen låser problem
* Fast: Förbättrad säkerhet
* Fast: Ibland dubbla bifogade konton kan visas
* Fast: En blob med en odefinierad innehållstyp generera ett undantag
* Fast: Öppna hello frågan panelen på en tom tabell var inte möjligt
* Fast: Varierar buggar i sökningen
* Fast: Ökade hello antal resurser som lästs in från 50 too100 när du klickar på ”mer belastning”
* Fast: Första körningen om ett konto som har loggat in, vi nu välja alla prenumerationer för kontot som standard 

#### <a name="known-issues"></a>Kända problem

* Den här versionen av hello Lagringsutforskaren körs inte på Ubuntu 14.04
* tooopen flera flikar för hello samma resurs, vill inte kontinuerligt klickar du på hello samma resurs. Klicka på en annan resurs och sedan gå tillbaka och klicka sedan på hello ursprungliga resurs tooopen den igen i en annan flik 
* Snabbåtkomst fungerar bara med prenumerationen baserat objekt. Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen
* Det kan ta Snabbåtkomst några sekunder toonavigate toohello målresurs, beroende på hur många resurser som du har
* Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel
* Referenser för sökning efter det att söka i ungefär 50 000 noder - prestanda kan påverkas eller kan orsaka undantag

10/03/2016
### <a name="version-085"></a>Version 0.8.5

#### <a name="new"></a>Ny

* Kan nu använda Portal-genererade SAS nycklar tooattach tooStorage konton och resurser

#### <a name="fixes"></a>Korrigeringar

* Fast: konkurrenstillstånd under sökningen ibland orsakade noder toobecome inte är expanderbara
* Fast: ”Använd HTTP” fungerar inte när du ansluter tooStorage konton med kontonamn och nyckel
* Fast: SAS-nycklar (som är särskilt Portal-genererade) returnerar ett ”avslutande snedstreck” fel
* Fast: importera tabell problem
    * Ibland har partitionsnyckel och radnyckel återförts
    * Det går inte tooread ”null” partitionsnycklar

#### <a name="known-issues"></a>Kända problem

* Referenser för sökning efter det att söka i ungefär 50 000 noder - kan prestanda påverkas
* Azure-stacken stöder inte filer, så försök tooexpand filer visas ett fel

09/12/2016
### <a name="version-084"></a>Version 0.8.4

<iframe width="560" height="315" src="https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Ny

* Generera Direktlänkar toostorage konton, behållare, köer, tabeller, eller filresurser för delning och enkel åtkomst till resurser för tooyour - Windows och Mac OS x-stöd
* Sök efter din blob-behållare, tabeller, köer, filresurser eller storage-konton från hello sökrutan
* Nu kan du gruppera-satser i hello tabell Frågebyggaren
* Byt namn på och kopiera och klistra in blob-behållare, filresurser, tabeller, BLOB, blob mappar, filer och kataloger från i SAS-anslutna konton och behållare
* Byta namn på och kopiera blob-behållare och filresurser bevara nu egenskaper och metadata

#### <a name="fixes"></a>Korrigeringar

* Fast: Det går inte att redigera tabellentiteter om de innehåller boolean eller binära egenskaper

#### <a name="known-issues"></a>Kända problem

* Referenser för sökning efter det att söka i ungefär 50 000 noder - kan prestanda påverkas

08/03/2016
### <a name="version-083"></a>Version 0.8.3

<iframe width="560" height="315" src="https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Ny

* Byt namn på behållare, tabeller, filresurser
* Bättre frågan builder upplevelse
* Möjlighet toosave och Läs in frågor
* Direkta länkar toostorage konton eller behållare, köer, tabeller eller filresurser för delning och lätt att komma åt dina resurser (endast för Windows - macOS stöder kommer snart!)
* Möjlighet toomanage och konfigurera CORS-regler

#### <a name="fixes"></a>Korrigeringar

* Fast: Microsoft Accounts kräver omautentisering var 8 – 12: e timme

#### <a name="known-issues"></a>Kända problem

* Ibland hello Användargränssnittet verka fryst – maximerar fönstret hello hjälper dig att lösa problemet
* installera macOS kan kräva förhöjd behörighet
* Konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter i ordning toofilter prenumerationer
* Byta namn på filresurser, blob-behållare och tabeller bevaras inte metadata eller andra egenskaper hello behållare, till exempel filresurskvoten, offentliga åtkomstnivå eller åtkomstprinciper
* Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder. Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn
* Kopiera eller byta namn på resurser fungerar inte i SAS-anslutna konton

07/07/2016
### <a name="version-082"></a>Version 0.8.2

<iframe width="560" height="315" src="https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Ny

* Storage-konton är grupperade efter prenumerationer; utveckling lagring och resurser som är anslutna via SAS eller nyckeln visas under noden (lokala och bifogad)
* Logga ut från konton ”Azure kontoinställningar” Kontrollpanelen
* Konfigurera proxy-inställningar tooenable och hantera inloggning
* Skapa och ta bort blob-lån
* Öppna blob-behållare, köer, tabeller och filer med enkelklickning

#### <a name="fixes"></a>Korrigeringar

* Fast: Kömeddelanden infogas med .NET eller Java-bibliotek är inte korrekt avkoda från base64
* Fast: $metrics tabeller visas inte för Blob Storage-konton
* Fast: tabeller noden fungerar inte för lokal lagring för (utveckling)

#### <a name="known-issues"></a>Kända problem

* installera macOS kan kräva förhöjd behörighet

06/15/2016
### <a name="version-080"></a>Version 0.8.0

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycfQhKztSIY?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>Ny

* Stöd för resursen: visa, överföra, hämta, kopiera filer och kataloger, SAS-URI: er (skapa och ansluta)
* Förbättrad användarupplevelse för att ansluta tooStorage med SAS URI: er eller nycklar
* Exportera tabell frågeresultat
* Tabell kolumnordning och anpassning
* Visa $logs blob-behållare och $metrics tabeller för Storage-konton med aktiverad
* Förbättrad exportera och importera beteende, innehåller nu egenskapsvärdetypen

#### <a name="fixes"></a>Korrigeringar

* Fast: överföra eller hämta stora blobbar kan resultera i ofullständiga överföringar/hämtningar
* Fast: redigera, lägga till eller importera en entitet med ett numeriskt värde (”1”) konverteras den toodouble
* Fast: Tooexpand hello tabell nod i hello lokala utvecklingsmiljö

#### <a name="known-issues"></a>Kända problem

* $metrics tabeller är inte synliga för Blob Storage-konton
* Kömeddelanden som lagts till via programmering kan inte visas korrekt om hälsningsmeddelande kodas med Base64-kodning

05/17/2016
### <a name="version-07201605090"></a>Version 0.7.20160509.0

#### <a name="new"></a>Ny

* Bättre felhantering för appen kraschar

#### <a name="fixes"></a>Korrigeringar

* Fast bugg där Informationsfältet meddelanden ibland inte visas när behövde inloggningsuppgifter

#### <a name="known-issues"></a>Kända problem

* Tabeller: Lägga till, redigera eller importera en entitet som har en egenskap med ett ambiguously numeriska värde, till exempel ”1” eller ”1.0”, och hello användaren försöker toosend det som ett `Edm.String`, hello värdet kommer tillbaka via hello klient API som en Edm.Double

03/31/2016

### <a name="version-07201603250"></a>Version 0.7.20160325.0

<iframe width="560" height="315" src="https://www.youtube.com/embed/imbgBRHX65A?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a>Ny

* Tabell stöd: visa, frågor, exportera, importera och CRUD-åtgärder för entiteter
* Kö stöd: visa, lägga till dequeueing meddelanden
* Generera SAS URI: er för Storage-konton
* Ansluta tooStorage konton med SAS URI: er
* Meddelanden om uppdateringar för framtida uppdateringar tooStorage Explorer
* Uppdaterade känsla och utseende

#### <a name="fixes"></a>Korrigeringar

* Förbättringar av prestanda och tillförlitlighet

### <a name="known-issues-amp-mitigations"></a>Kända problem &amp; åtgärder

* Hämta stora blob filer fungerar inte - vi rekommenderar att du använder AzCopy medan vi löser problemet 
* Autentiseringsuppgifter att inte hämta och cachelagras om hello arbetsmapp hittas inte eller kan inte skrivas till
* Om vi lägger till, redigera eller importera en entitet som har en egenskap med ett ambiguously numeriska värde, till exempel ”1” eller ”1.0”, och hello användare försöker toosend det som ett `Edm.String`, hello värdet kommer tillbaka via hello klient API som en Edm.Double
* När du importerar CSV-filer med flera rader poster kan hello data hämta hackas eller kodade

02/03/2016

### <a name="version-07201601291"></a>Version 0.7.20160129.1

#### <a name="fixes"></a>Korrigeringar

* Förbättrad prestanda när du laddar upp, hämta och kopiera BLOB

01/14/2016

### <a name="version-07201601050"></a>Version 0.7.20160105.0

#### <a name="new"></a>Ny

* Linux-support (paritet funktioner tooOSX)
* Lägga till blob-behållare med delad åtkomst signaturer (SAS)-nyckel
* Lägg till Lagringskonton för Azure Kina
* Lägg till Storage-konton med anpassade slutpunkter
* Öppna och visa hello innehållet text- och blobbar
* Visa och redigera blob-egenskaper och metadata

#### <a name="fixes"></a>Korrigeringar

* Fast: överföra eller hämta ett stort antal blobbar (500 +) kan ibland orsaka hello app toohave en vit skärm 
* Fast: när du ställer in blob-behållaren offentliga åtkomstnivå hello nya värdet uppdateras inte förrän du har angett hello fokus på hello behållare igen. Dessutom hello dialogrutan alltid som standard för ”ingen offentlig åtkomst” och inte hello faktiska aktuella värde.
* Stöd för bättre övergripande tangentbord/tillgänglighet och UI
* Spår historik texten ska radbrytas när den är lång med ett blanksteg
* SAS-dialogrutan har stöd för verifiering av indata
* Lokal lagring fortsätter toobe tillgängliga även om användarens autentiseringsuppgifter har upphört att gälla
* När du tar bort en öppen blobbehållare stängs hello blob explorer hello höger

#### <a name="known-issues"></a>Kända problem

* Installera Linux måste gcc version uppdateras eller uppgraderas – steg tooupgrade finns nedan: 
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Version 0.7.20151116.0

#### <a name="new"></a>Ny

* macOS och versioner av Windows
* Logga in tooview Storage-konton – använda din organisations konto, Account, 2FA osv.
* Lokal utveckling lagring (Använd storage-emulatorn endast för Windows)
* Stöd för Azure Resource Manager och klassisk resurs
* Skapa och ta bort blobbar, köer och tabeller
* Sök efter specifika blobbar, köer och tabeller
* Utforska hello innehållet i blob-behållare
* Visa och navigera genom kataloger
* Ladda upp, hämta och ta bort blobbar och mappar
* Visa och redigera blob-egenskaper och metadata
* Generera en SAS-nycklar
* Hantera och skapa lagras åtkomst principer SAP)
* Sök efter blobbar efter prefix
* Dra 'n släpper filer tooupload eller ladda ned

#### <a name="known-issues"></a>Kända problem

* När du ställer in blob-behållaren offentliga åtkomstnivå uppdateras hello nya värdet inte förrän du har angett hello fokus på hello behållare igen
* När du öppnar hello dialogrutan tooset hello offentliga åtkomstnivå visas alltid ”ingen offentlig åtkomst” som standard hello och inte hello faktiska aktuella värde
* Det går inte att byta namn på hämtade blobbar
* Aktiviteten loggposter kommer ibland ”fastna” i en pågående tillstånd när ett fel inträffar och hello fel visas inte
* Ibland krascher och aktiverar eller inaktiverar helt vit vid tooupload eller hämta ett stort antal blobbar
* Ibland avbryter en kopieringsåtgärd fungerar inte
* När du skapar en behållare (tabell-blob/kön), om du vill ange ett ogiltigt namn och fortsätta toocreate en annan under en annan Behållartyp du kan inte ange fokus på hello ny typ
* Det går inte att skapa en ny mapp eller byta namn på mappen




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md