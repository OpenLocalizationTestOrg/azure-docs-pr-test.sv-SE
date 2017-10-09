---
title: "aaaAzure Storage Service-kryptering för Data i vila | Microsoft Docs"
description: "Använd hello Azure Storage Service-kryptering funktionen tooencrypt Azure Blob Storage på tjänstsidan hello när hello data lagras och dekryptera den vid hämtning av hello data."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: robinsh
ms.openlocfilehash: 4e03c5704071281a798936d41d86456afcfdec77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage-tjänstens kryptering av vilande data
Azure Storage Service kryptering (SSE) för Data i vila hjälper dig att skydda och skydda dina data toomeet din organisations säkerhet och efterlevnad åtaganden. Med den här funktionen Azure Storage krypterar dina data tidigare toopersisting toostorage och automatiskt dekrypterar tidigare tooretrieval. är helt transparent toousers hello kryptering, dekryptering och hantering av nycklar.

hello följande avsnitt innehåller detaljerad information om hur toouse hello Lagringstjänstens kryptering funktioner samt hello stöds scenarier och användarupplevelser.

## <a name="overview"></a>Översikt
Azure Storage tillhandahåller en omfattande uppsättning säkerhetsfunktioner som tillsammans ger utvecklare möjligheten toobuild säkra program. Data kan skyddas vid överföring mellan en program- och Azure med hjälp av [Client Side Encryption](../storage-client-side-encryption.md), HTTPs- eller SMB 3.0. Lagringstjänstens kryptering tillhandahåller kryptering i vila, hantering av kryptering, dekryptering och hantering av nycklar på ett helt transparent sätt. Krypteras alla data med hjälp av 256-bitars [AES-kryptering](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), en hello starkaste block chiffer tillgängliga.

SSE fungerar genom att kryptera hello data när de skrivs tooAzure lagring och kan användas för Azure Blob Storage och File Storage. Det fungerar hello följande:

* Standardlagring: Allmänna lagringskonton för BLOB- och lagrings- och Blob storage-konton
* Premium Storage 
* Alla redundans nivåer (LRS, ZRS, GRS och RA-GRS)
* Azure Resource Manager lagringskonton (men inte klassiska) 
* Alla regioner.

toolearn finns fler toohello vanliga frågor och svar.

tooenable eller inaktivera Storage Service-kryptering för ett lagringskonto, logga in på hello [Azure-portalen](https://portal.azure.com) och välj ett lagringskonto. Leta efter hello Blob-tjänsten avsnittet som visas i den här skärmbilden hello inställningar-bladet och klicka på kryptering.

![Skärmbild som visar Portal krypteringsalternativet](./media/storage-service-encryption/image1.png)
<br/>*Bild 1: Aktivera SSE för Blob-tjänsten (steg 1)*

![Skärmbild som visar Portal krypteringsalternativet](./media/storage-service-encryption/image3.png)
<br/>*Bild 2: Aktivera SSE för tjänsten File (steg 1)*

När du har klickat på hello krypteringsinställning kan du aktivera eller inaktivera Storage Service-kryptering.

![Portalen skärmbild som visar egenskaper för kryptering](./media/storage-service-encryption/image2.png)
<br/>*Bild 3: Aktivera SSE för Blob och Filtjänst (steg 2)*

## <a name="encryption-scenarios"></a>Scenarier för kryptering
Lagringstjänstens kryptering kan aktiveras på en nivå för storage-konto. När du har aktiverat, kommer kunderna välja vilka tjänster tooencrypt. Den stöder hello efter kundscenarier:

* Kryptering av Blob Storage och File Storage i Resource Manager-konton.
* Kryptering av Blob och tjänsten File i klassiska lagringskonton när migreras tooResource Manager storage-konton.

SSE har hello följande begränsningar:

* Kryptering av klassiska lagringskonton stöds inte.
* Befintliga Data - SSE krypterar endast nyskapade data efter hello kryptering har aktiverats. Om till exempel du skapar ett nytt lagringskonto för Resource Manager men inte aktiverar kryptering, och sedan ladda upp blobbar eller arkiverade virtuella hårddiskar toothat storage-konto och sedan aktivera SSE, krypteras inte de blobbar om de om eller kopieras.
* Stöd för Marketplace - Aktivera kryptering av virtuella datorer skapas från hello marknadsplats med hello [Azure-portalen](https://portal.azure.com), PowerShell och Azure CLI. grundläggande hello VHD-avbildningen kommer att vara okrypterad; alla skrivningar utföras när hello VM har kommer att krypteras.
* Tabellen och köer data krypteras inte.

## <a name="getting-started"></a>Komma igång
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Steg 1: [skapa ett nytt lagringskonto](../storage-create-storage-account.md).
### <a name="step-2-enable-encryption"></a>Steg 2: Aktivera kryptering.
Du kan aktivera kryptering med hello [Azure-portalen](https://portal.azure.com).

> [!NOTE]
> Om du vill tooprogrammatically aktivera eller inaktivera hello Lagringstjänstens kryptering på ett lagringskonto, kan du använda hello [Azure Storage Resource Provider REST API](https://msdn.microsoft.com/library/azure/mt163683.aspx), hello [klientbibliotek för Storage Resource Provider för .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](/powershell/azureps-cmdlets-docs), eller hello [Azure CLI](../storage-azure-cli.md).
> 
> 

### <a name="step-3-copy-data-toostorage-account"></a>Steg 3: Kopiera data toostorage konto
Om du aktiverar SSE för hello Blob-tjänsten kommer att krypteras alla blobbar som skrivs toothat storage-konto. Alla blobbar som redan finns i detta lagringskonto krypteras inte förrän de på nytt. Du kan kopiera hello data från en storage-konto tooone med SSE krypterade eller även aktivera SSE och kopiera hello blobbar från en behållare tooanother toosure tidigare data som är krypterade. Du kan använda någon av följande verktyg tooaccomplish hello den. Detta är även hello samma beteende för lagring av filer.

#### <a name="using-azcopy"></a>Med hjälp av AzCopy
AzCopy är en Windows-kommandoradsverktyg för att kopiera data tooand från Microsoft Azure Blob-, fil- och Table storage med hjälp av enkla kommandon med optimala prestanda. Du kan använda den här toocopy din blobbar eller filer från en storage-konto tooanother har SSE aktiverad. 

toolearn fler besök [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).

#### <a name="using-smb"></a>Med hjälp av SMB
Azure File storage erbjuder filresurser i hello molnet med hello SMB-standardprotokollet. Du kan montera en filresurs från en klient lokalt eller i Azure. När monterad, vara verktyg, exempelvis Robocopy används toocopy filer över tooAzure filresurser. Mer information finns i [hur toomount Azure-filresurs på Windows](../files/storage-how-to-use-files-windows.md) och [hur toomount Azure File dela på Linux](../storage-how-to-use-files-linux.md).


#### <a name="using-hello-storage-client-libraries"></a>Med hjälp av hello Storage-klientbibliotek
Du kan kopiera blob eller filen data tooand från blob storage eller mellan lagringskonton med hjälp av vår omfattande uppsättning Lagringsklientbiblioteken som .NET, C++, Java, Android, Node.js, PHP, Python eller Ruby.

toolearn fler Besök vår [komma igång med Azure Blob storage med hjälp av .NET](../blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Med en lagringsutforskare
Du kan använda en lagring explorer toocreate storage-konton, ladda upp och hämta data, visa innehållet i BLOB och gå igenom kataloger. Du kan använda något av dessa tooupload blobbar tooyour storage-konto med kryptering aktiverat. Med vissa lagringsutforskare kan du också kopiera data från befintliga tooa olika behållare för blob storage i hello storage-konto eller ett nytt lagringskonto som har aktiverat SSE.

toolearn fler besök [Azure lagringsutforskare](../storage-explorers.md).

### <a name="step-4-query-hello-status-of-hello-encrypted-data"></a>Steg 4: Fråga hello status för hello krypterade data
En uppdaterad version av hello Storage-klientbibliotek har som gör att du tooquery hello tillståndet för ett objekt toodetermine om den är krypterad eller inte distribuerats. Detta är för närvarande bara tillgänglig för Blob storage. Stöd för File storage finns på hello Översikt. 

I hello tiden kan du anropa [hämta kontoegenskaperna](https://msdn.microsoft.com/library/azure/mt163553.aspx) tooverify som hello storage-konto har kryptering aktiverat eller visa hello lagringskontoegenskaperna i hello Azure-portalen.

## <a name="encryption-and-decryption-workflow"></a>Kryptering och dekryptering arbetsflöde
Här är en kort beskrivning av arbetsflödet för kryptering och dekryptering av hello:

* hello kunden aktiverar kryptering på hello storage-konto.
* När hello kunden skriver nya data (PLACERA Blob, PLACERA Block, PLACERA sidan, PLACERA filen etc.) tooBlob eller fillagring; varje skrivning har krypterats med 256-bitars [AES-kryptering](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), en hello starkaste block chiffer tillgängliga.
* När hello kunden måste tooaccess data (hämta Blob, etc.), dekrypteras data automatiskt innan den returnerar toohello användare.
* Om kryptering har inaktiverats kan nya skrivningar krypteras inte längre och befintliga krypterade data förblir krypterade tills omarbetning hello användare. Även om kryptering har aktiverats, skriver du tooBlob eller fillagring krypteras. hello tillståndet för data ändras inte med hello användaren växla mellan aktivering/inaktivering av kryptering för hello storage-konto.
* Alla krypteringsnycklar lagras, krypteras och hanteras av Microsoft.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Vanliga frågor om Storage Service-kryptering för Data i vila
**F: Jag har ett befintligt klassiska lagringskonto. Kan jag aktivera SSE på den?**

S: inte stöds SSE bara på Resource Manager storage-konton.

**F: hur kan jag för att kryptera data i mitt klassiska storage-konto?**

S: du kan skapa ett nytt lagringskonto för hanteraren för filserverresurser och kopiera dina data med hjälp av [AzCopy](storage-use-azcopy.md) från ditt befintliga klassiska lagringskonto tooyour nyligen skapade lagringskontot för hanteraren för filserverresurser. 

Om du migrerar din klassiska storage-konto tooa Resource Manager lagringskonto åtgärden är omedelbar, ändrar hello typ av ditt konto, men påverkas inte befintliga data. Alla nya data skrivs krypteras endast efter att aktivera kryptering. Mer information finns i [plattformen stöds migrering av IaaS-resurser från klassiska tooResource Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Observera att detta stöds endast för Blob- och -tjänster.

**F: Jag har ett befintligt lagringskonto i hanteraren för filserverresurser. Kan jag aktivera SSE på den?**

S: Ja, men endast nyligen skrivs data att krypteras. Det inte gå tillbaka och kryptera data som redan finns. Detta stöds inte ännu för hello förhandsgranskning för lagring av filer.

**F: Jag vill tooencrypt hello aktuella data i ett befintligt lagringskonto i Resource Manager?**

S: du kan aktivera SSE när som helst i ett lagringskonto för hanteraren för filserverresurser. Data som redan fanns kommer inte krypteras. tooencrypt befintliga data, kan du kopiera dem tooanother namn eller en annan behållare och ta sedan bort hello okrypterade versioner.

**F: Jag använder Premium-lagring. kan jag använda SSE?**

S: Ja stöds SSE både standardlagring och Premium-lagring.  Premium-lagring stöds inte för hello File Service.

**F: om jag skapa ett nytt lagringskonto och aktivera SSE och sedan skapa en ny virtuell dator med hjälp av detta lagringskonto, betyder det den virtuella datorn är krypterad?**

S: Ja. Alla diskar som skapats med hello nytt lagringskonto krypteras, förutsatt att de skapas efter SSE har aktiverats. Om hello VM har skapats med Azure-marknadsplatsen hello VHD basavbildning kommer att vara okrypterad; alla skrivningar utföras när hello VM har kommer att krypteras.

**F: kan jag skapa ny storage-konton med SSE aktiverad med Azure PowerShell och Azure CLI?**

S: Ja.

**F: hur mycket kostar Azure Storage om SSE aktiveras?**

S: finns utan extra kostnad.

**F: som hanterar hello krypteringsnycklar?**

S: hello nycklar som hanteras av Microsoft.

**F: kan jag använda min egen krypteringsnycklar?**

S: Vi arbetar på tillhandahåller funktioner för kunder toobring sina egna krypteringsnycklar.

**F: kan jag återkalla åtkomst toohello krypteringsnycklar?**

S: inte vid detta tillfälle. hello nycklar hanteras helt av Microsoft.

**F: är SSE aktiverad som standard när jag skapar ett nytt lagringskonto?**

S: SSE är inte aktiverad som standard. Du kan använda hello Azure portal tooenable den. Du kan också programmässigt aktivera den här funktionen med hello Storage Resource Provider REST API.

**F: hur skiljer detta sig från Azure Disk Encryption?**

S: funktionen är används tooencrypt data i Azure Blob storage. hello Azure Disk Encryption är används tooencrypt Operativsystemet och datadiskarna i IaaS-VM. Mer information finns våra [säkerhetsguiden för lagring](../storage-security-guide.md).

**F: Vad händer om jag aktivera SSE, och sedan gå in i och aktivera Azure Disk Encryption på hello diskar?**

S: Detta fungerar sömlöst. Data krypteras med båda metoderna.

**F: mitt lagringskonto har ställts in toobe replikeras geo-redundant. Om jag aktiverar SSE min redundant kopia även krypteras?**

S: Ja, krypteras alla kopior av hello storage-konto och alla redundansalternativ för – lokalt Redundant lagring (LRS), zon-Redundant lagring (ZRS), Geo-Redundant lagring (GRS) och Geo-Redundant lagring med läsbehörighet (RA-GRS) – stöds.

**F: Jag kan inte aktivera kryptering på mitt lagringskonto.**

S: är det en Resource Manager storage-konto? Klassiska lagringskonton stöds inte. 

**F: SSE tillåts endast i vissa regioner?**

S: hello SSE är tillgänglig i alla regioner för Blob storage. Kontrollera hello Tillgänglighetssektion för lagring av filer. 

**F: hur kan jag få någon om jag har problem eller vill tooprovide feedback?**

S: Kontakta [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) för eventuella problem med tooStorage Service-kryptering.

## <a name="next-steps"></a>Nästa steg
Azure Storage tillhandahåller en omfattande uppsättning säkerhetsfunktioner som tillsammans ger utvecklare möjligheten toobuild säkra program. Mer information finns i hello [säkerhetsguiden för lagring](../storage-security-guide.md).

