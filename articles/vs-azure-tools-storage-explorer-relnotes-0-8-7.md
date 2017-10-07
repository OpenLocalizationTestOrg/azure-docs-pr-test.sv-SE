---
title: "aaaMicrosoft Azure Lagringsutforskaren 0.8.7 (förhandsversion) | Microsoft Docs"
description: "Viktig information för Microsoft Azure Lagringsutforskaren 0.8.7 (förhandsgranskning)"
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
ms.date: 01/18/2017
ms.author: cawa
ms.openlocfilehash: 9fdd491a3ea838e20f9d4f82c176cfb02fbe306b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-087-preview"></a>Microsoft Azure Lagringsutforskaren 0.8.7 (förhandsgranskning)
## <a name="overview"></a>Översikt
Den här artikeln innehåller hello viktig information om Azure Lagringsutforskaren 0.8.7 förhandsversionen.

[Microsoft Azure Lagringsutforskaren (förhandsversion)](./vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux.

## <a name="azure-storage-explorer-087-preview"></a>Azure Lagringsutforskaren 0.8.7 (förhandsgranskning)
### <a name="download-azure-storage-explorer-087-preview"></a>Hämta Azure Lagringsutforskaren 0.8.7 (förhandsgranskning)
- [Azure Storage Explorer 0.8.7 Preview för Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage Explorer 0.8.7 förhandsgranskning för Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage Explorer 0.8.7 förhandsgranskning för Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new-updates"></a>Nya uppdateringar
* Du kan välja hur tooresolve konflikter hello början av en uppdatering, hämta eller kopiera session i hello **aktiviteter** fönster.
* Hovra över en fliken toosee hello fullständig sökväg till hello storage-resursen.
* När du klickar på en flik synkronisering och dess plats i navigeringsfönstret för hello vänster.

### <a name="fixes"></a>Korrigeringar
* Fast: Lagringsutforskaren är en betrodd app på macOS.
* Fast: Ubuntu 14.04 stöds igen.
* Fast: Ibland hello Lägg till konto UI blinkar när inläsning av prenumerationer.
* Fast: Ibland inte alla lagringsresurser ingick i hello vänstra navigeringsfönstret.
* Fast: hello åtgärdsfönstret ibland visas tom åtgärder.
* Fast: hello fönsterstorlek från hello senast stängdes session bevaras nu.
* Fast: Du kan öppna flera flikar för hello samma resurs med hello snabbmenyn.

### <a name="known-issues"></a>Kända problem
* Snabbåtkomst fungerar bara med prenumerationen-baserade objekt. Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen.
* Det kan ta några sekunder toonavigate toohello målresurs, beroende på hur många resurser som du har Snabbåtkomst.
* Med fler än tre grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel.
* Referenser för sökning efter det att söka i ungefär 50 000 noder - prestanda kan påverkas eller kan orsaka ohanterade undantag.
* För hello kan första gången du använder hello Lagringsutforskaren på macOS, du se flera prompter som ber om användarens behörighet tooaccess hello nyckelringar. Vi rekommenderar att du väljer **Tillåt alltid** så hello prompten inte visas igen

## <a name="previous-releases"></a>Tidigare versioner
### <a name="features"></a>Funktioner
#### <a name="main-features"></a>Huvuddragen
* macOS, Linux och Windows-versioner
* Logga in tooview dina Lagringskonton grupperade efter prenumeration:
    * Använd din organisations konto, Account, 2FA osv.
    * Konfigurera och hantera proxyinställningar
    * Ta bort konton genom att logga ut
* Anslut tooStorage konton med hjälp av:
    * Kontonamn och nyckel
    * Anpassade slutpunkter (inklusive Azure Kina)
    * SAS-URI för Storage-konton
* Stöd för Azure Resource Manager och klassiska lagring
* Generera en SAS-nycklar för blobbar, blob-behållare, köer, tabeller eller filresurser
* Ansluta tooblob behållare, köer, tabeller eller filresurser med nyckeln för delad åtkomst signaturer (SAS)
* Hantera lagras åtkomstprinciper för blob-behållare, köer, tabeller eller filresurser
* Lokal utveckling lagring med Storage-emulatorn (endast Windows)
* Skapa och ta bort blob-behållare, köer och tabeller
* Visa $logs blob-behållare och $metrics tabeller
* Sök efter specifika blobbar, köer, tabeller eller filresurser
* Direktlänkar toostorage konton eller behållare, köer, tabeller eller filen filresurser för delning och lätt att komma åt dina resurser
* Byt namn på blob-behållare, tabeller, filresurser
* Möjlighet toomanage och konfigurera CORS-regler
* Enkelt kopiera primära och sekundära nyckeln för Storage-konton
* MD5 kontroller av överföring och hämtning för dataintegritet och konsekvens
* Sök efter din blob-behållare, tabeller, köer, filresurser eller storage-konton från hello sökrutan
* Du kan nu PIN-kod används oftast services toohello Snabbåtkomst för enkel navigering
* Nu kan du öppna flera redigerare på olika flikar. Enkelklickning tooopen tillfälliga fliken. Dubbelklicka på tooopen en permanent flik. Du kan också klicka på hello tillfälliga fliken toomake den permanenta fliken
* Märkbar förbättringar av prestanda och stabilitet för överföringar och nedladdningar, särskilt för stora filer på snabb datorer
* Vi återinföra hello förbättrad omfångssökning och tillagda hello begreppet omfång. Ange hello sökvägen tooa nod via hello hovra ikon, högerklickar du på -> ”Sök från här”, eller manuellt tooscope noden. När omfång, kan du lägga till en sökning termen toohello end hello sökvägen toodeep sökningen från noden
* Vi har lagt till olika teman: ljus (standard), mörkt, hög kontrast svart och hög kontrast vit.
* Gå tooEdit -> teman toochange önskemål teman
* På Linux för, ett 64-bitars operativsystem är nu krävs
* Vi har uppdaterat vår logotypen!
#### <a name="blobs"></a>Blobar
* Visa blobbar och gå igenom kataloger
* Ladda upp, hämta, ta bort och kopiera BLOB och mappar
* Öppna och visa hello innehållet text- och blobbar
* Visa och redigera blob-egenskaper och metadata
* Sök efter blobbar efter prefix
* Skapa och dela lån för blobbar och blob-behållare
* Dra 'n släppa filer tooupload
* Byt namn på blobbar och mappar
* Nu kan skapa tomma ”virtuell” mappar i blob-behållare
* Du kan ändra hello Blob och filegenskaper
#### <a name="tables"></a>Tabeller
* Visa och fråga entiteter med ODATA eller använda frågan builder toocreate komplexa frågor
* Lägga till, redigera, ta bort enheter
* Importera och exportera innehåll i CSV-format (inklusive exportera frågeresultat)
* Anpassa kolumnordning
* Möjlighet toosave frågor
#### <a name="queues"></a>Köer
* Ta en snabb titt på de senaste 32 meddelandena
* Lägga till, har status Created, visa meddelanden
* Rensa kön
* Vi gjorde det möjligt för du toodecide om du vill tooencode/avkoda meddelanden i kö
#### <a name="file-shares"></a>Filresurser
* Visa filer och gå igenom kataloger
* Ladda upp, ladda ned, ta bort och kopiera filer och kataloger
* Visa egenskaper
* Byt namn på filer och kataloger

### <a name="bug-fixes"></a>Felkorrigeringar
* Fast: Skärmen låser problem
* Fast: Förbättrad säkerhet

### <a name="known-issues"></a>Kända problem
* Sök efter det att söka i ungefär 50 000 noder - referenser prestanda att påverkas macOS installera kanske kräver förhöjd behörighet
* Konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter toofilter prenumerationer
* Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder. Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn
* Azure-stacken inte stöder filer, så försök tooexpand hello **filer** nod resulterar i ett fel
* Linux 14.04 installera måste gcc version uppdateras eller uppgraderas. hello följande steg visar hur tooupgrade:

```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

### <a name="previous-versions"></a>Tidigare versioner
#### <a name="october-2016-release-version-085"></a>Oktober 2016-versionen (version 0.8.5)
* [Hämta för Windows](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Hämta för Mac](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Hämta för Linux](https://go.microsoft.com/fwlink/?LinkId=809308)
