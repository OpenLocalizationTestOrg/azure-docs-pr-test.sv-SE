---
title: "Microsoft Azure Lagringsutforskaren 0.8.7 (förhandsversion) | Microsoft Docs"
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
ms.openlocfilehash: fc3620ca19d4824bc8ffb3bbe89ef47c5127b9d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-azure-storage-explorer-087-preview"></a>Microsoft Azure Lagringsutforskaren 0.8.7 (förhandsgranskning)
## <a name="overview"></a>Översikt
Den här artikeln innehåller viktig information om Azure Lagringsutforskaren 0.8.7 förhandsversionen.

[Microsoft Azure Lagringsutforskaren (förhandsversion)](./vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som gör det enkelt att arbeta med Azure Storage-data i Windows, macOS och Linux.

## <a name="azure-storage-explorer-087-preview"></a>Azure Lagringsutforskaren 0.8.7 (förhandsgranskning)
### <a name="download-azure-storage-explorer-087-preview"></a>Hämta Azure Lagringsutforskaren 0.8.7 (förhandsgranskning)
- [Azure Storage Explorer 0.8.7 Preview för Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage Explorer 0.8.7 förhandsgranskning för Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage Explorer 0.8.7 förhandsgranskning för Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new-updates"></a>Nya uppdateringar
* Du kan välja hur du löser konflikter i början av en uppdatering, hämta eller kopiera session i den **aktiviteter** fönster.
* Hovra över en flik om du vill se den fullständiga sökvägen till resursen lagring.
* När du klickar på en flik synkronisering och dess plats i navigeringsfönstret vänster.

### <a name="fixes"></a>Korrigeringar
* Fast: Lagringsutforskaren är en betrodd app på macOS.
* Fast: Ubuntu 14.04 stöds igen.
* Fast: Ibland Gränssnittet för att lägga till kontot blinkar när inläsning av prenumerationer.
* Fast: Ibland inte alla lagringsresurser ingick i det vänstra navigeringsfönstret.
* Fast: Åtgärdsfönstret ibland visas tom åtgärder.
* Fast: Fönsterstorlek från den senaste stängd sessionen bevaras nu.
* Fast: Du kan öppna flera flikar för samma resurs med hjälp av snabbmenyn.

### <a name="known-issues"></a>Kända problem
* Snabbåtkomst fungerar bara med prenumerationen-baserade objekt. Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen.
* Snabbåtkomst kan det ta ett par sekunder att navigera till målresursen, beroende på hur många resurser som du har.
* Med fler än tre grupper av blobbar eller filer som överför samtidigt kan orsaka fel.
* Referenser för sökning efter det att söka i ungefär 50 000 noder - prestanda kan påverkas eller kan orsaka ohanterade undantag.
* Du kan se flera prompter be om användarens tillåtelse att komma åt nyckelringen för första gången med Lagringsutforskaren på macOS. Vi rekommenderar att du väljer **Tillåt alltid** så uppmaningen inte visas igen

## <a name="previous-releases"></a>Tidigare versioner
### <a name="features"></a>Funktioner
#### <a name="main-features"></a>Huvuddragen
* macOS, Linux och Windows-versioner
* Logga in att visa dina Lagringskonton grupperade efter prenumeration:
    * Använd din organisations konto, Account, 2FA osv.
    * Konfigurera och hantera proxyinställningar
    * Ta bort konton genom att logga ut
* Anslut till Storage-konton med:
    * Kontonamn och nyckel
    * Anpassade slutpunkter (inklusive Azure Kina)
    * SAS-URI för Storage-konton
* Stöd för Azure Resource Manager och klassiska lagring
* Generera en SAS-nycklar för blobbar, blob-behållare, köer, tabeller eller filresurser
* Anslut till blob-behållare, köer, tabeller eller filresurser med nyckeln för delad åtkomst signaturer (SAS)
* Hantera lagras åtkomstprinciper för blob-behållare, köer, tabeller eller filresurser
* Lokal utveckling lagring med Storage-emulatorn (endast Windows)
* Skapa och ta bort blob-behållare, köer och tabeller
* Visa $logs blob-behållare och $metrics tabeller
* Sök efter specifika blobbar, köer, tabeller eller filresurser
* Direktlänkar till storage-konton eller behållare, köer, tabeller eller filen filresurser för delning och lätt att komma åt dina resurser
* Byt namn på blob-behållare, tabeller, filresurser
* Möjlighet att hantera och konfigurera CORS-regler
* Enkelt kopiera primära och sekundära nyckeln för Storage-konton
* MD5 kontroller av överföring och hämtning för dataintegritet och konsekvens
* Sök efter din blob-behållare, tabeller, köer, filresurser eller storage-konton från sökrutan
* Du kan nu PIN-kod används oftast Snabbåtkomst-tjänster för enkel navigering
* Nu kan du öppna flera redigerare på olika flikar. Enskild klickar för att öppna en tillfällig flik; Dubbelklicka för att öppna en permanent flik. Du kan också klicka på fliken tillfälligt så att den blir en permanent flik
* Märkbar förbättringar av prestanda och stabilitet för överföringar och nedladdningar, särskilt för stora filer på snabb datorer
* Vi återinföra förbättrad omfångssökning och lagt till begreppet scope. Ange sökvägen till en nod via ikonen hovra höger Klicka -> ”Sök från här”, eller manuellt definiera omfattningen av noden. När omfång, kan du lägga till en sökterm i slutet av sökvägen till djup sökningen från noden
* Vi har lagt till olika teman: ljus (standard), mörkt, hög kontrast svart och hög kontrast vit.
* Gå till Redigera -> teman för att ändra inställningarna teman
* På Linux för, ett 64-bitars operativsystem är nu krävs
* Vi har uppdaterat vår logotypen!
#### <a name="blobs"></a>Blobar
* Visa blobbar och gå igenom kataloger
* Ladda upp, hämta, ta bort och kopiera BLOB och mappar
* Öppna och visa innehållet text- och blobbar
* Visa och redigera blob-egenskaper och metadata
* Sök efter blobbar efter prefix
* Skapa och dela lån för blobbar och blob-behållare
* Dra 'n ta bort filerna som ska överföras
* Byt namn på blobbar och mappar
* Nu kan skapa tomma ”virtuell” mappar i blob-behållare
* Du kan ändra Blob och filegenskaper
#### <a name="tables"></a>Tabeller
* Visa och fråga entiteter med ODATA eller använda Frågeverktyget för att skapa komplexa frågor
* Lägga till, redigera, ta bort enheter
* Importera och exportera innehåll i CSV-format (inklusive exportera frågeresultat)
* Anpassa kolumnordning
* Möjlighet att spara frågor
#### <a name="queues"></a>Köer
* Ta en snabb titt på de senaste 32 meddelandena
* Lägga till, har status Created, visa meddelanden
* Rensa kön
* Vi gjorde det möjligt för dig att bestämma om du vill koda/avkoda meddelanden i kö
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
* Konto inställningar panelen kan indikera att du måste ange autentiseringsuppgifter för att filtrera prenumerationer
* Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder. Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn
* Azure-stacken inte stöder filer, så försök att expandera den **filer** nod resulterar i ett fel
* Linux 14.04 installera måste gcc version uppdateras eller uppgraderas. Följande steg visar hur du uppgraderar:

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
