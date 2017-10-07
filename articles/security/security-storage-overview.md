---
title: "aaaSecurity funktioner som kan användas med Azure Storage | Microsoft Docs"
description: " Den här artikeln innehåller en översikt över hello Azure säkerhetsfunktionerna som kan användas med Azure Storage. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 663cd2705527957d21ff9475a6322b42a16c95e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-security-overview"></a>Översikt över säkerheten i Azure-lagring
Azure Storage är hello molnlagringslösningen för moderna program som förlitar sig på hållbarhet, tillgänglighet och skalbarhet toomeet hello kundernas behov. Azure Storage tillhandahåller en omfattande uppsättning säkerhetsfunktioner:

* hello storage-konto kan skyddas med hjälp av rollbaserad åtkomstkontroll och Azure Active Directory.
* Data kan skyddas vid överföring mellan en program- och Azure med hjälp av kryptering på klientsidan, HTTPS och SMB 3.0.
* Data kan ställas in toobe krypteras automatiskt när skrivas tooAzure lagring med hjälp av Storage Service-kryptering.
* Operativsystemet och datadiskarna som används av virtuella datorer kan ställas in toobe som krypterats med Azure Disk Encryption.
* Delegerad åtkomst toohello dataobjekt i Azure Storage kan tilldelas använder signaturer för delad åtkomst.
* hello autentiseringsmetod som används av någon när de har åtkomst till lagring kan spåras med Storage analytics.

En närmare titt på säkerhet i Azure Storage finns hello [säkerhetsguiden för Azure Storage](../storage/common/storage-security-guide.md). Den här guiden ger en djupdykning i hello säkerhetsfunktionerna i Azure Storage, till exempel lagringskontonycklar, datakryptering vid överföring och i vila och storage analytics.

Den här artikeln innehåller en översikt över säkerheten i Azure-funktioner som kan användas med Azure Storage. Länkar finns tooarticles som ger information om varje funktion så kan du läsa mer.

Här följer hello core funktioner toobe beskrivs i den här artikeln:

* Rollbaserad Access Control
* Delegerad åtkomst toostorage objekt
* Kryptering under överföring
* Kryptering i vila/Storage Service-kryptering
* Azure Disk Encryption
* Azure Key Vault

## <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Du kan skydda ditt lagringskonto med rollbaserad åtkomstkontroll (RBAC). Begränsa åtkomst baserat på hello [måste tooknow](https://en.wikipedia.org/wiki/Need_to_know) och [minsta privilegium](https://en.wikipedia.org/wiki/Principle_of_least_privilege) säkerhetsprinciper är viktigt för organisationer som vill tooenforce säkerhetsprinciper för dataåtkomst. Dessa behörigheter beviljas genom att tilldela hello lämpliga RBAC rollen toogroups och program för ett visst område. Du kan använda [inbyggda RBAC-roller](../active-directory/role-based-access-built-in-roles.md), till exempel lagring konto deltagare tooassign behörighet toousers.

Läs mer:

* [Azure Active Directory rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-toostorage-objects"></a>Delegerad åtkomst toostorage objekt
En signatur för delad åtkomst (SAS) ger delegerad åtkomst tooresources i ditt lagringskonto. hello SAS innebär att du ger en klient begränsade behörigheter tooobjects i ditt lagringskonto för en angiven tidsperiod och med en angiven uppsättning behörigheter. Du kan bevilja dessa begränsade behörigheter utan tooshare åtkomstnycklarna för ditt konto. hello SAS är en URI som omfattar alla hello uppgifter som är nödvändiga för autentiserad åtkomst tooa lagringsresurs i dess Frågeparametrar. tooaccess lagringsresurser med hello SAS hello klienten behöver bara tooprovide hello SAS toohello lämplig konstruktor eller metod.

Läs mer:

* [Förstå hello SAS-modellen](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Skapa och använda en SAS med Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Kryptering under överföring
Kryptering under överföring är en mekanism för att skydda data när de skickas över nätverk. Du kan skydda data med hjälp av med Azure Storage:

* [Kryptering på transportnivå](../storage/common/storage-security-guide.md#encryption-in-transit), till exempel HTTPS när du överför data till eller från Azure Storage.
* [Tråd kryptering](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), till exempel SMB 3.0-kryptering för Azure-filresurser.
* [Kryptering på klientsidan](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), tooencrypt hello data innan den överförs till lagring och toodecrypt hello data när det överförs utanför lagring.

Mer information om klientens kryptering:

* [Kryptering på klientsidan för Microsoft Azure Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Molnet säkerhetskontroller serien: kryptera Data under överföring](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Vilande kryptering
I många organisationer [datakryptering i viloläge](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) är ett obligatoriskt steg mot data sekretess-, efterlevnads- och suveränitet. Det finns tre Azure-funktioner som tillhandahåller kryptering av data som är ”i vila”:

* [Lagringstjänstens kryptering](../storage/common/storage-security-guide.md#encryption-at-rest) kan du toorequest att hello lagringstjänsten automatiskt kryptera data när du skriver den tooAzure lagring.
* [Kryptering på klientsidan](../storage/common/storage-security-guide.md#client-side-encryption) ger också hello-funktionen för kryptering i vila.
* [Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) kan du tooencrypt hello OS diskar och datadiskar som används av en virtuell IaaS-dator.

Mer information om Lagringskryptering för tjänsten:

* [Azure Storage Service-kryptering](https://azure.microsoft.com/services/storage/) är tillgänglig för [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/). Mer information om andra typer av Azure storage finns [filen](https://azure.microsoft.com/services/storage/files/), [(Premium-lagring)](https://azure.microsoft.com/services/storage/premium-storage/), [tabell](https://azure.microsoft.com/services/storage/tables/), och [kön](https://azure.microsoft.com/services/storage/queues/).
* [Azure Storage Service-kryptering av vilande Data](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption
Azure Disk Encryption för virtuella datorer (VM) hjälper dig att organisationens säkerhets- och efterlevnadskrav genom att kryptera dina Virtuella diskar (inklusive start- och datadiskar) med nycklar och principer som du styr i [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

Kryptering för virtuella datorer fungerar för Linux och Windows-operativsystem. Dessutom används Key Vault toohelp skydda, hantera och granska användning av krypteringsnycklarna disk. Alla hello data i VM-diskar är krypterad vilande genom att använda branschstandardiserad krypteringsteknik i Azure Storage-konton. hello diskkryptering lösning för Windows är baserad på [Microsoft BitLocker-diskkryptering](https://technet.microsoft.com/library/cc732774.aspx), och hello Linux-lösning baserad på [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Läs mer:

* [Azure Disk Encryption för Windows och Linux-IaaS-virtuella datorer](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Disk Encryption använder [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) toohelp du styra och hantera disk krypteringsnycklar och hemligheter i prenumerationen nyckelvalv samtidigt som du säkerställer att krypteras alla data i hello virtuella diskar i vila i din Azure Lagring. Du bör använda Key Vault tooaudit nycklar och användning av principen.

Läs mer:

* [Vad är Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md)
