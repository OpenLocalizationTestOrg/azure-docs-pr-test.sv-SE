---
title: aaaAzure Storage Service-kryptering med kunden hanterade nycklar i Azure Key Vault | Microsoft Docs
description: "Använd hello Azure Storage Service-kryptering funktionen tooencrypt Azure Blob Storage på tjänstsidan hello när hello data lagras och dekryptera den när hämtar hello data med hjälp av kunden hanterade nycklar."
services: storage
documentationcenter: .net
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: lakasa
ms.openlocfilehash: 870cae2f258b356aa234f8bba65a023ac389be10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Lagringstjänstens kryptering med hjälp av kunden hanterade nycklar i Azure Key Vault

Microsoft Azure är starkt allokerat toohelping du skyddar och skydda dina data toomeet din organisations säkerhet och efterlevnad åtaganden.  Ett sätt som du kan skydda data i vila är toouse Storage Service kryptering (SSE), som automatiskt krypterar dina data när du skriver den toostorage och dekrypterar data vid hämtning av den. Hej kryptering och dekryptering är automatisk och helt transparent och använder 256-bitars [AES-kryptering](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), en hello starkaste block chiffer tillgängliga.

Du kan använda Microsoft-hanterad krypteringsnycklarna med SSE eller du kan använda dina egna krypteringsnycklar. Den här artikeln ska tala om hello senare. Mer information om hur du använder Microsoft-hanterad nycklar eller om SSE i allmänhet finns [Lagringstjänstens kryptering av vilande Data](storage-service-encryption.md).

tooprovide hello möjlighet toouse egna krypteringsnycklar SSE för Blob storage är integrerat med Azure Key Vault (AKV). Du kan skapa egna krypteringsnycklar och lagrar dem i AKV eller du kan använda AKVS API: er toogenerate krypteringsnycklar. Inte bara AKV Tillåt toomanage och styra dina nycklar, det gör också att du tooaudit din nyckelanvändning. 

Varför kan det vara bra toocreate egna nycklar? Den ger dig större flexibilitet, inklusive hello möjlighet toocreate, rotera, inaktivera och definiera åtkomstkontroller och tooaudit hello kryptering nycklar som används tooprotect dina data.

## <a name="sse-with-customer-managed-keys-preview"></a>SSE med kunden hanterade nycklar förhandsgranskning

Den här funktionen är för närvarande en förhandsversion. toouse denna funktion, behöver du toocreate ett nytt lagringskonto. Du kan antingen skapa ett nytt nyckelvalv och nyckel eller du kan använda en befintlig nyckelvalvet och nyckel. hello storage-konto och hello nyckelvalv måste vara i hello samma region, men de kan ha olika prenumerationer.

tooparticipate i hello preview Kontakta [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com). Vi ger en länken tooparticipate i hello preview.

toolearn mer, se toohello [vanliga frågor och svar](#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).

> [!IMPORTANT]
> Du måste registrera dig för förhandsgranskning hello tidigare toofollowing hello stegen i den här artikeln. Utan förhandsåtkomst, kommer inte att kunna tooenable funktionen hello-portalen.

## <a name="getting-started"></a>Komma igång
## <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Steg 1: [skapa ett nytt lagringskonto](storage-create-storage-account.md)

## <a name="step-2-enable-encryption"></a>Steg 2: Aktivera kryptering
Du kan aktivera SSE för hello storage-konto med hello [Azure-portalen](https://portal.azure.com). Leta efter hello Blob-tjänsten avsnittet som visas i bilden nedan och klicka på kryptering hello inställningsbladet för hello storage-konto.

![Skärmbild som visar Portal krypteringsalternativet](./media/storage-service-encryption/image1.png)
<br/>*Aktivera SSE för Blob-tjänsten*

Om du vill tooprogrammatically aktivera eller inaktivera hello Lagringstjänstens kryptering på ett lagringskonto, kan du använda hello [Azure Storage Resource Provider REST API](https://docs.microsoft.com/en-us/rest/api/storagerp/?redirectedfrom=MSDN), hello [klientbibliotek för Storage Resource Provider för .NET](https://docs.microsoft.com/en-us/dotnet/api/?redirectedfrom=MSDN), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-4.0.0), eller hello [Azure CLI](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli).

På den här skärmen om du inte ser hello ”använda din egen nyckel” om har du inte godkänts för hello förhandsgranskning. Skicka ett e-postmeddelande för[ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) och begära godkännande.

![Portalen skärmbild som visar kryptering Preview](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

Som standard använder SSE Microsoft-hanterad nycklar. toouse egna nycklar hello kryssrutan. Och kan du ange en nyckel URI eller välj en nyckel och Key Vault hello väljare.

## <a name="step-3-select-your-key"></a>Steg 3: Välj din nyckel

![Portalen skärmbild visar kryptering alternativet din egen nyckel](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

![Portalen skärmbild som visar kryptering med viktiga uri-alternativ](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Om hello storage-konto inte har åtkomst toohello Key Vault kan du köra följande hello med Azure Powershell toogrant toohello lagring åtkomstkonton toohello krävs nyckelvalvet.

![Portalen skärmbild som visar åtkomst nekades för nyckelvalvet](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Du kan också ge åtkomst via hello Azure-portalen genom att gå toohello Azure Key Vault i hello Azure-portalen och bevilja åtkomst toohello storage-konto.

## <a name="step-4-copy-data-toostorage-account"></a>Steg 4: Kopiera data toostorage konto
Om du vill att tootransfer data till det nya kontot så att den är krypterad, se för[steg 3 av komma igång i Lagringstjänstens kryptering av vilande Data](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-3-copy-data-to-storage-account).

## <a name="step-5-query-hello-status-of-hello-encrypted-data"></a>Steg 5: Fråga hello status för hello krypterade data
tooquery hello statusen för hello krypterade data finns för[steg 4 i komma igång i Lagringstjänstens kryptering av vilande Data](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-4-query-the-status-of-the-encrypted-data).

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Vanliga frågor om Storage Service-kryptering för Data i vila
**F: Jag använder Premium-lagring. kan jag använda SSE med hanterade Kundnycklar?**

S: Ja SSE med Microsoft-hanterad och kunden hanterade nycklar stöds både standardlagring och Premium-lagring. 

**F: kan jag skapa ny storage-konton med SSE med hanterade Kundnycklar aktiverad med Azure PowerShell och Azure CLI?**

S: Ja.

**F: hur mycket mer kostar Azure Storage om SSE med kunden hanterade nycklar är aktiverad?**

S: det kostar för att använda Azure Key Vault. Mer information finns [Key Vault-priser](https://azure.microsoft.com/en-us/pricing/details/key-vault/). Det finns utan extra kostnad för att använda SSE.

**F: kan jag återkalla åtkomst toohello krypteringsnycklar?**

S: Ja, du kan återkalla åtkomst när som helst. Det finns flera sätt toorevoke tooyour åtkomstnycklar. Se för[Azure Key Vault PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.0.0) och [Azure Key Vault CLI](https://docs.microsoft.com/en-us/cli/azure/keyvault) för mer information. Återkalla åtkomst blockerar effektivt åtkomst tooall blobbar i hello storage-konto som hello konto krypteringsnyckeln är otillgänglig genom Azure Storage.

**F: kan jag skapa ett lagringskonto och en nyckel i annan region?**

S: inte hello hello storage-konto och nyckel valvnyckeln måste toobe i samma region. 

**F: kan jag aktivera SSE med hanterade Kundnycklar när du skapar hello storage-konto?**

S: Nej. När du aktiverar SSE när du skapar hello storage-konto kan använda du bara Microsoft-hanterad nycklar. Om du vill att toouse kunden hanterade nycklar måste tooupdate hello lagringskontoegenskaperna. Du kan använda REST eller något av hello lagring klienten bibliotek tooprogrammatically uppdatera ditt lagringskonto eller uppdatera hello lagringskontoegenskaperna med hello Azure-portalen när du har skapat hello-konto.

**F: kan jag inaktivera kryptering medan med kunden SSE hanterade nycklar?**

A: du kan inte Nej, inaktivera kryptering när med kunden SSE hanterade nycklar. toodisable kryptering, behöver du tooswitch toousing Microsoft-hanterad nycklar. Du kan göra detta med hjälp av hello Azure-portalen eller PowerShell.

**F: är SSE aktiverad som standard när jag skapar ett nytt lagringskonto?**

S: SSE är inte aktiverad som standard. Du kan använda hello Azure portal tooenable den. Du kan också programmässigt aktivera den här funktionen med hello Storage Resource Provider REST API. 

**F: Jag kan inte aktivera kryptering på mitt lagringskonto.**

S: är det en Resource Manager storage-konto? Klassiska lagringskonton stöds inte. SSE med hanterade Kundnycklar kan även aktiveras endast nyskapade Resource Manager storage-konton.

**F: är SSE med kunden hanterade nycklar får endast i vissa områden?**

S: SSE är tillgänglig i endast vissa regioner för Blob storage för den här förhandsversionen. Kontakta e- [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) toocheck för tillgänglighet och information om förhandsgranskning. 

**F: hur kan jag få någon om jag har problem eller vill tooprovide feedback?**

S: Kontakta [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) för eventuella problem med tooStorage Service-kryptering. 

## <a name="next-steps"></a>Nästa steg

*   Mer information om hello omfattande uppsättning säkerhet funktioner som hjälper utvecklare att skapa säkra program, se hello [säkerhetsguiden för lagring](https://docs.microsoft.com/en-us/azure/storage/storage-security-guide).
*   Mer information om Azure Key Vault finns [vad är Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)?
*   Komma igång på Azure Key Vault finns [komma igång med Azure Key Vault](../key-vault/key-vault-get-started.md).
