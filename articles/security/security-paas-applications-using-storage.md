---
title: "aaaSecuring PaaS-program med hjälp av Azure Storage | Microsoft Docs"
description: " Läs mer om Azure Storage Säkerhet Metodtips för att skydda din PaaS webb- och mobilprogram. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: TomShinder
ms.openlocfilehash: 3fed75cb121e7f32eb8b948ee12ca35fb25eca7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-storage"></a>Att säkra PaaS-webb- och mobila program med Azure Storage
I den här artikeln tar vi upp en samling Azure Storage säkerhetsmetoder för att skydda din PaaS webb- och mobilprogram. Följande rekommendationer härleds från våra erfarenhet av Azure och hello erfarenheter från kunder som dig själv.

Hej [säkerhetsguiden för Azure Storage](../storage/common/storage-security-guide.md) hittar du mer detaljerad information om Azure Storage och säkerhet.  Den här artikeln tar på en hög nivå vissa av hello begrepp som hittades i hello säkerhetsguiden och länkar toohello säkerhetsguiden samt andra källor, mer information.

## <a name="azure-storage"></a>Azure Storage
Azure gör det möjligt toodeploy och Använd lagring på sätt inte enkelt kan ske lokalt. Du kan nå hög skalbarhet och tillgänglighet relativt visningsläge med Azure storage. Azure storage hello foundation är inte bara för Windows och Linux virtuella Azure-datorer stöder även stora distribuerade program.

Azure storage tillhandahåller hello följande fyra tjänster: Blob storage, Table storage, Queue storage och File storage. Det finns fler toolearn [introduktion tooMicrosoft Azure Storage](../storage/storage-introduction.md).

## <a name="best-practices"></a>Bästa praxis
Den här artikeln tar hello följande metodtips:

- NAP:
   - Signaturer för delad åtkomst (SAS)
   - Hanterade diskar
   - Rollbaserad åtkomstkontroll (RBAC)

- Lagringskryptering:
   - Klient-kryptering för data med högt värde på serversidan
   - Azure Disk Encryption för virtuella datorer (VM)
   - Kryptering av lagringstjänst

## <a name="access-protection"></a>NAP
### <a name="use-shared-access-signature-instead-of-a-storage-account-key"></a>Använda signatur för delad åtkomst i stället för en lagringskontonyckel

I en IaaS-lösning som kör Windows Server- eller Linux virtuella datorer vanligtvis, skyddas filer från avslöjande och manipulering hot med hjälp av mekanismer för kontrollen. På Windows använder du [åtkomstkontrollistor (ACL)](../virtual-network/virtual-networks-acl.md) och på Linux du förmodligen använda [chmod](https://en.wikipedia.org/wiki/Chmod). I princip, detta är exakt vad du skulle göra om du idag skydda filer på en server i ditt eget datacenter.

PaaS är olika. En av hello vanligaste sätt toostore filerna i Microsoft Azure är toouse [Azure Blob storage](../storage/storage-dotnet-how-to-use-blobs.md). Skillnad mellan Blob storage och andra fillagring är hello i/o och hello skyddsmetod som följer med i/o.

Åtkomstkontroll är viktigt. toohelp som du styr åtkomst tooAzure lagring hello genererar systemet två 512-bitars lagringskontonycklar (SAKs) när du [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md). hello andelen viktiga redundans gör det möjligt för du tooavoid tjänst avbrott under rutinunderhåll viktiga rotation.

Åtkomstnycklar för lagring bör är hög prioritet hemligheter och endast komma åt toothose ansvarar för åtkomstkontroll för lagring. Om hello fel personer får åtkomst toothese nycklar kan ska de ha fullständig kontroll av lagring och kan ersätta, ta bort eller lägga till filer toostorage. Detta inkluderar skadlig kod och andra typer av innehåll som potentiellt kan påverka din organisation eller dina kunder.

Du måste fortfarande sätt tooprovide åtkomst tooobjects i lagringen. Mer detaljerade tooprovide åt du kan dra nytta av [signatur för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). hello SAS gör det möjligt för dig tooshare specifika objekt i lagring för en fördefinierad tidsintervall och särskilda behörigheter. En signatur för delad åtkomst kan du toodefine:

- hello intervall över vilka hello SAS är giltiga, inklusive hello starttid och hello förfallotiden.
- hello behörigheterna av hello SAS. Till exempel kan en SAS för en blob ge en användare läsa och skriva behörigheter toothat blob, men inte att ta bort behörigheter.
- En valfri IP-adress eller intervall av IP-adresser som Azure Storage accepterar hello SAS. Du kan till exempel ange ett intervall IP-adresser som tillhör tooyour organisation. Detta ger ett annat mått av säkerhet för din SAS.
- hello protokoll som accepterar Azure Storage hello SAS. Du kan använda den här valfria parametern toorestrict åtkomst tooclients med hjälp av HTTPS.

SAS kan du tooshare innehåll hello önskemål tooshare den utan att ge bort din Lagringskontonycklar. Alltid med hjälp av SAS i ditt program är ett säkert sätt tooshare dina lagringsresurser utan att kompromissa med dina nycklar för lagringskonto.

Det finns fler toolearn [använder signaturer för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). Mer information om potentiella risker och rekommendationer toomitigate toolearn dessa risker finns [metodtips när du använder SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="use-managed-disks-for-vms"></a>Använda hanterade diskar för virtuella datorer

När du väljer [Azure hanterade diskar](../storage/storage-managed-disks-overview.md), Azure hanterar hello storage-konton som du använder för din Virtuella diskar. Allt du behöver toodo är Välj hello typ av disk (Standard eller Premium) och hello diskstorleken; Azure storage gör hello rest. Du har inte tooworry om skalbarhetsbegränsningar som kan ha krävt tooyou toomultiple storage-konton på annat sätt.

Det finns fler toolearn [vanliga frågor och svar om hanterade och ohanterade premiumdiskar](../storage/storage-faq-for-disks.md).

### <a name="use-role-based-access-control"></a>Använda rollbaserad åtkomstkontroll

Vi diskuterade tidigare med hjälp av delade signatur åtkomst (SAS) toogrant begränsad åtkomst tooobjects i din lagring konto tooother klienter, utan att utsätta din lagringskontonyckel för kontot. Ibland uppväger hello risker associerade med en viss åtgärd mot ditt lagringskonto hello fördelarna med SAS. Ibland är det enklare toomanage åtkomst på annat sätt.

Ett annat sätt toomanage åtkomst är toouse [rollbaserad åtkomstkontroll i](../active-directory/role-based-access-control-what-is.md) (RBAC). Baserat på hello måste tooknow och minsta privilegium säkerhetsprinciper med RBAC fokusera på och ger anställda hello exakt behörigheter de behöver. För många behörigheter kan exponera en konto tooattackers. För få behörigheter innebär att anställda kan få arbetet gjort effektivt. RBAC kan du lösa det här problemet genom att erbjuda detaljerad åtkomsthantering för Azure. Det här är viktigt för organisationer som vill tooenforce säkerhetsprinciper för dataåtkomst.

Du kan utnyttja inbyggda RBAC-roller i Azure tooassign privilegier toousers. Överväg att använda Storage-konto deltagare för molnoperatörer som behöver toomanage storage-konton och klassiska Storage-konto deltagare rollen toomanage klassiska lagringskonton. Överväg att lägga till dem toohello virtuella deltagarrollen för molnoperatörer som behöver toomanage virtuella datorer men inte hello virtuellt nätverk eller storage-konto toowhich de är anslutna.

Organisationer som inte behöver använda data åtkomstkontroll genom att utnyttja funktioner, till exempel RBAC kan ger fler behörigheter än vad som krävs för sina användare. Detta kan leda toodata angrepp genom att tillåta att vissa användare åtkomst toodata som de inte borde ha hello första plats.

toolearn om RBAC finns:

- [Rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-control-configure.md)
- [Inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md)
- [Azure Storage-säkerhetsguiden](../storage/common/storage-security-guide.md) för information om hur toosecure lagringen konto med RBAC

## <a name="storage-encryption"></a>Lagringskryptering
### <a name="use-client-side-encryption-for-high-value-data"></a>Använd klientsidans kryptering för data med högt värde

Kryptering på klientsidan aktiverar du tooprogrammatically krypterar data under överföring innan du laddar upp tooAzure lagring och dekryptera data programmässigt när hämtas från lagringsplatsen.  Detta tillhandahåller kryptering av data under överföring, men det ger också kryptering av vilande data.  Kryptering på klientsidan hello säkraste metod för att kryptera data men kräver toomake programmässiga ändringar tooyour program och gjorda nyckelhantering processer.

Kryptering på klientsidan kan du också toohave ensam kontroll över dina krypteringsnycklar.  Du kan skapa och hantera egna krypteringsnycklar.  Kryptering på klientsidan använder en kuvert teknik där hello Azure storage-klientbibliotek genererar en innehåll krypteringsnyckel (CEK) som sedan omslutna (krypterade) med hjälp av krypteringsnyckeln för hello nyckel (KEK). Hej KEK identifieras med en nyckelidentifierare och kan vara ett asymmetriskt nyckelpar eller en symmetrisk nyckel och kan hanteras lokalt eller lagras i [Azure Key Vault](../key-vault/key-vault-whatis.md).

Kryptering på klientsidan är inbyggd i hello Java och hello lagringsklientbiblioteken för .NET.  Se [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](../storage/storage-client-side-encryption.md) information om kryptering av data i klientprogram och generera och hantera egna krypteringsnycklar.

### <a name="azure-disk-encryption-for-vms"></a>Azure Disk Encryption för virtuella datorer
Azure Disk Encryption är en funktion som hjälper dig att kryptera din Windows- och Linux IaaS virtuella diskar. Azure Disk Encryption utnyttjar hello branschen standard BitLocker för Windows och hello DM-Crypt i Linux tooprovide volymkryptering för hello OS och hello datadiskar. hello-lösning är integrerad med Azure Key Vault toohelp du styra och hantera hello-diskkryptering nycklar och hemligheter i nyckelvalvet-prenumeration. hello lösning betyder också att krypteras alla data på hello virtuella diskar i vila i ditt Azure storage.

Se [Azure Disk Encryption för Windows och Linux-IaaS-VM](azure-security-disk-encryption.md).

### <a name="storage-service-encryption"></a>Kryptering av lagringstjänst
När [Lagringstjänstens kryptering](../storage/storage-service-encryption.md) för File storage är aktiverat hello data krypteras automatiskt med hjälp av AES 256-kryptering. Microsoft hanterar alla hello kryptering, dekryptering och hantering av nycklar. Den här funktionen är tillgänglig för LRS- och GRS redundans-typer.

## <a name="next-steps"></a>Nästa steg
Den här artikeln introduceras tooa mängd Azure Storage säkerhetsmetoder för att skydda din PaaS webb- och mobilprogram. toolearn mer om hur du skyddar dina PaaS-distributioner finns:

- [Skydda PaaS-distributioner](security-paas-deployments.md)
- [Att säkra PaaS-webb- och mobila program med Azure App Service](security-paas-applications-using-app-services.md)
- [Att säkra PaaS-databaser i Azure](security-paas-applications-using-sql.md)
