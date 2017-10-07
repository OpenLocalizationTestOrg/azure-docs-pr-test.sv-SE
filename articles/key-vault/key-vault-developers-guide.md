---
title: aaaAzure Key Vault-utvecklare
description: "Utvecklare kan använda Azure Key Vault toomanage kryptografiska nycklar i hello Microsoft Azure-miljön."
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 631cea1315964cd0b97e8b2cf3311754230fb801
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-developers-guide"></a>Azure Key Vault-Guide för utvecklare

Key Vault kan toosecurely komma åt känslig information från i ditt program:

- Nycklar och hemligheter skyddas utan att behöva toowrite hello koden själv och du kan enkelt toouse dem från dina program.
- Du kan toohave dina egna kunder och hantera sina egna nycklar så att du kan koncentrera dig på att tillhandahålla hello kärnfunktioner programvara. På så sätt kommer dina program inte äger hello ansvar eller potentiella ansvar för dina kunders klientnycklar och hemligheter.
- Programmet kan använda nycklar för signering och kryptering ännu fortsätter hello nyckelhantering externa från ditt program så att din lösning toobe lämplig som geografiskt distribuerade app.
- Från och med hello September 2016-versionen av Nyckelvalvet, dina program kan använda Key Vault [certifikat](https://docs.microsoft.com/rest/api/keyvault/certificate-operations). Mer information finns i [om nycklar och hemligheter certifikat](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates).

Mer allmän information om Azure Key Vault finns [vad är Key Vault](key-vault-whatis.md).

## <a name="public-previews"></a>Offentliga förhandsgranskningar

Med jämna mellanrum, släpp vi en förhandsversion av en ny funktion i Nyckelvalvet. Du kan prova att använda dessa och berätta för oss vad du tycker azurekeyvault@microsoft.com, våra feedback e-postadress.

### <a name="storage-account-keys---july-10-2017"></a>Lagringskontonycklar - 10 juli 2017

>[!NOTE]
>För den här uppdateringen av Azure Key Vault endast hello **Lagringskontonycklar** funktionen är i förhandsgranskningen.

Den här förhandsgranskningen innehåller vår nya Lagringskontonycklar funktion, tillgängliga via dessa gränssnitt; [.NET / C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault/), [REST](https://docs.microsoft.com/rest/api/keyvault/) och [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/). 

Mer information om hello ny Lagringskontonycklar funktion finns [översikt över Azure Key Vault storage-konto nycklar](key-vault-ovw-storage-keys.md).

## <a name="videos"></a>Videoklipp

Den här videon visar hur toocreate din egen nyckel valvet och toouse från hello 'Hello Key Vault' exempelprogrammet.

- [Key Vault-utvecklare - Snabbstartsguide](https://channel9.msdn.com/Blogs/Azure/Azure-Key-Vault-Developer-Quick-Start/player)

Resurser i ovannämnda video:

- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure Key Vault exempelkod](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

## <a name="creating-and-managing-key-vaults"></a>Skapa och hantera Nyckelvalv

Innan du börjar arbeta med Azure Key Vault i koden kan du skapa och hantera valv via REST Resource Manager-mallar, PowerShell eller CLI, enligt beskrivningen i följande artiklar hello:

- [Skapa och hantera Nyckelvalv med övriga](https://docs.microsoft.com/rest/api/keyvault/)
- [Skapa och hantera Nyckelvalv med PowerShell](key-vault-get-started.md)
- [Skapa och hantera Nyckelvalv med CLI](key-vault-manage-with-cli2.md)
- [Skapa en nyckelvalvet och lägga till en hemlighet via en Azure Resource Manager-mall](../azure-resource-manager/resource-manager-template-keyvault.md)

> [!NOTE]
> Åtgärder mot nyckelvalv autentiseras via AAD och auktoriseras via Nyckelvalvets egna åtkomstprincipen definieras per valvet.

## <a name="coding-with-key-vault"></a>Med Key Vault-kodning

hello Key Vault hanteringssystem för programmerare består av flera gränssnitt med övriga som hello foundation. Via hello REST-gränssnittet är alla nyckelvalv resurser tillgängliga; nycklar, hemligheter och certifikat. [Nyckeln valvet REST API-referens](https://docs.microsoft.com/rest/api/keyvault/). 

### <a name="supported-programming-languages"></a>Programmeringsspråk som stöds

#### <a name="net"></a>.NET

- [.NET API refence för Key Vault](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault) 

Mer information om hello 2.x version av hello .NET SDK finns hello [viktig information](key-vault-dotnet2api-release-notes.md).

#### <a name="java"></a>Java

- [Java SDK för Key Vault](https://docs.microsoft.com/java/api/com.microsoft.azure.keyvault)

#### <a name="nodejs"></a>Node.js

I Node.js är hello valvet hanterings-API och hello valvet objektet API separat. Hantering av Key Vault kan skapa och uppdatera nyckelvalvet. Key Vault Operations API är för att arbeta med valvet objekt som; nycklar, hemligheter och certifikat. 

- [Node.js API-referens för hantering av valvet](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest/)
- [Node.js API-referens för Key Vault-åtgärder](http://azure.github.io/azure-sdk-for-node/azure-keyvault/latest/) 

### <a name="quick-start"></a>Snabbstart

- [Skapa Nyckelvalvet](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
- [Komma igång med Key Vault i Node.js](https://azure.microsoft.com/en-us/resources/samples/key-vault-node-getting-started/)

### <a name="code-examples"></a>Kodexempel

Fullständig exempel med dina program Key Vault finns:

- [Azure Key Vault-kodexempel](http://www.microsoft.com/download/details.aspx?id=45343) -.NET exempelprogrammet *HelloKeyVault* och ett Azure web service-exempel. 
- [Använda Azure Key Vault från ett webbprogram](key-vault-use-from-web-application.md) -självstudiekurs toohelp du lära dig hur toouse Azure nyckeln valvet från ett webbprogram i Azure. 

## <a name="how-tos"></a>Instruktioner

hello ge följande artiklar och scenarier uppgiftsspecifika vägledning för att arbeta med Azure Key Vault:

- [Ändra nyckelvalv klient-ID när prenumerationen flytta](key-vault-subscription-move-fix.md) – när du flyttar din Azure-prenumeration från innehavaren tootenant B, din befintliga nyckelvalv är tillgängligt som hello säkerhetsobjekt (användare och program) i klient B. Korrigera detta med hjälp av den här guiden.
- [Åtkomst till Nyckelvalvet bakom brandväggen](key-vault-access-behind-firewall.md) -tooaccess en nyckel valvet ditt nyckelvalv klienten programmet måste toobe kan tooaccess flera slutpunkter för olika funktioner.
- [Hur tooGenerate och Transfer HSM-Protected nycklar för Azure Key Vault](key-vault-hsm-protected-keys.md) -detta hjälper dig att planera för, generera och överföra din egen HSM-skyddade nycklar toouse med Azure Key Vault.
- [Hur toopass skydda värden (till exempel lösenord) under distributionen](../azure-resource-manager/resource-manager-keyvault-parameter.md) – när du behöver toopass säkert värde (till exempel ett lösenord) som en parameter under distributionen kan du lagra den som en hemlighet i ett Azure Key Vault och referens hello värde i andra Resource Manager-mallar.
- [Hur toouse Key Vault extensible för hantering av nycklar med SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) -hello SQL Server-anslutningen för Azure Key Vault kan SQL Server och SQL i en VM tooleverage hello Azure Key Vault-tjänsten som en Extensible Key Management (EKM)-provider tooprotect dess krypteringsnycklarna för program länk; Transparent datakryptering säkerhetskopiering kryptering och kryptering på kolumnen.
- [Hur toodeploy certifikat tooVMs från Nyckelvalvet](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - ett moln-program som körs på en virtuell dator på Azure måste ett certifikat. Hur du får det här certifikatet i den här virtuella datorn idag?
- [Hur tooset in Nyckelvalv med end tooend nyckeln rotation och granskning](key-vault-key-rotation-log-monitoring.md) -det går igenom hur tooset viktiga rotation och granskning med Azure Key Vault.
- [Distribuera Azure-certifikat för webbprogram via Key Vault]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) innehåller stegvisa instruktioner för att distribuera certifikat som lagras i Nyckelvalvet som en del av [App tjänstcertifikat](https://azure.microsoft.com/blog/internals-of-app-service-certificate/) erbjudande.
- [Bevilja behörighet toomany program tooaccess ett nyckelvalv](key-vault-group-permissions-for-apps.md) Key Vault principer för åtkomstkontroll stöder endast 16 poster. Du kan dock skapa en Azure Active Directory-säkerhetsgrupp. Lägg till alla hello associerade tjänsten säkerhetsobjekt toothis säkerhetsgrupp och sedan ge åtkomst toothis säkerhet grupp tooKey valvet.
- Flera uppgiftsspecifika vägledning för att integrera och med Azure Key valv finns [Ryan Jones Azure Resource Manager-mall exempel för Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
- [Hur toouse Key Vault mjuk borttagning med CLI](key-vault-soft-delete-cli.md) hjälper dig att hello användning och livscykeln för ett nyckelvalv och olika nyckelvalv objekt med aktiverad mjuk borttagning.
- [Hur toouse Key Vault mjuk borttagning med PowerShell](key-vault-soft-delete-powershell.md) hjälper dig att hello användning och livscykeln för ett nyckelvalv och olika nyckelvalv objekt med aktiverad mjuk borttagning.

## <a name="integrated-with-key-vault"></a>Integrerad med Key Vault

Dessa artiklar som handlar om andra scenarier och tjänster som använder eller integrera med Key Vault.

- [Azure Disk Encryption](../security/azure-security-disk-encryption.md) använder hello branschstandarden [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funktion i Windows och hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funktion i Linux tooprovide volymkryptering för hello OS och hello data diskar. hello-lösning är integrerad med Azure Key Vault toohelp du styra och hantera hello disk krypteringsnycklar och hemligheter i prenumerationen nyckelvalv samtidigt som du säkerställer att krypteras alla data i hello virtuella diskar i vila i ditt Azure storage.
- [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) innehåller alternativ för kryptering av data som lagras i hello-konto. Data Lake Store ger två lägen för hantering, av nycklar för att hantera master krypteringsnycklarna (MEKs) som krävs för att dekryptera data som lagras i hello Data Lake Store. Du kan antingen låta Data Lake Store hantera hello MEKs för dig, eller välj tooretain ägarskap för hello MEKs med Azure Key Vault-konto. Du kan ange hello läge för hantering av nycklar när du skapar ett Data Lake Store-konto. 
- [Azure Information Protection](/information-protection/plan-design/plan-implement-tenant-key) kan du toomanager din egen klientnyckel. Till exempel i stället för Microsoft hanterar din klientnyckel (hello standard), kan du hantera din egen nyckel toocomply klient med specifika föreskrifter som gäller tooyour organisation. Hantera din egen klientnyckel är också hänvisade tooas ta med din egen nyckel eller BYOK.

## <a name="key-vault-overviews-and-concepts"></a>Key Vault översikter och begrepp

- [Key Vault mjuk borttagning beteende](key-vault-ovw-soft-delete.md) beskrivs en funktion som gör att återställning av borttagna objekt om hello borttagning har oavsiktligt eller avsiktligt.
- [Key Vault klienten begränsning](key-vault-ovw-throttling.md) beskriver toohello grundläggande principerna för begränsning av och erbjuder en metod för din app.
- [Översikt över Key Vault storage-konto nycklar](key-vault-ovw-storage-keys.md) beskriver hello Key Vault-integrering Azure Storage-konton nycklar.
- [Key Vault säkerhetsvärldar](key-vault-ovw-security-worlds.md) beskriver hello relationer mellan regioner och säkerhetsområden.

## <a name="social"></a>Sociala medier

- [Key Vault-blogg](http://aka.ms/kvblog)
- [Key Vault-Forum](http://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Andra bibliotek

- [Microsoft Azure Key Vault Core Library](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) ger **IKey** och **IKeyResolver** gränssnitt för att hitta nycklar från identifierare och utföra åtgärder med nycklar.
- [Tillägg för Microsoft Azure Key Vault](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) ger utökade funktioner för Azure Key Vault.


