---
title: "aaaData säkerhet och kryptering Metodtips | Microsoft Docs"
description: "Den här artikeln innehåller en uppsättning av bästa praxis för datasäkerhet och kryptering med hjälp av inbyggda funktioner i Azure."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: yurid
ms.openlocfilehash: 5057c85ed3107921462a40045e716675ea41e4bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Metodtips för säkerhet för Azure Data och kryptering
En av hello nycklar toodata skydd i hello molnet redovisning för hello möjliga tillstånd som kan uppstå i dina data och vilka kontroller som är tillgängliga för det aktuella tillståndet. Hello syftet med Azure data säkerhet och kryptering metodtips ska hello rekommendationer vara runt hello följande data tillstånd:

* I vila: Detta innehåller all information som lagringsobjekt, behållare och typer som finns statiskt på fysiska media som ska vara det magnetiska eller optical disk.
* Under överföring: När data överförs mellan komponenter, platser eller program, som hello nätverket mellan en service bus (från lokala toocloud och vice versa, inklusive hybridanslutningar, till exempel ExpressRoute), eller under en in-/ utdata process, den är ungefär som i rörelse.

I den här artikeln diskuteras en samling Azure data säkerhets- och bästa praxis. Följande rekommendationer härleds från våra upplevelse med Azure datasäkerhet och kryptering och hello upplever för kunder som själv.

För varje rekommenderar förklarar vi:

* Vilka hello bra
* Varför du vill tooenable som bästa praxis
* Vad kan vara hello resultat om du inte bästa praxis för tooenable hello
* Möjliga alternativ toohello bästa praxis
* Hur du kan lära dig tooenable hello bästa praxis

Den här artikeln Azure datasäkerhet och bästa praxis för kryptering baseras på en konsensus åsikt och Azure plattformsfunktioner och funktioner, som de finns på hello tid som den här artikeln skrevs. Åsikter och tekniker ändras med tiden och den här artikeln kommer att uppdateras på ett regelbundet tooreflect ändringarna.

Azure data säkerhets- och bästa praxis i den här artikeln omfattar:

* Använda multifaktorautentisering
* Använd rollbaserad åtkomstkontroll (RBAC)
* Kryptera virtuella Azure-datorer
* Använda maskinvara säkerhetsmodeller
* Hantera med säkra arbetsstationer
* Aktivera SQL-datakryptering
* Skydda data under överföring
* Tillämpa fil nivån datakryptering

## <a name="enforce-multi-factor-authentication"></a>Använda Multifaktorautentisering
hello första steget i dataåtkomst och kontroll i Microsoft Azure är tooauthenticate hello användare. [Azure Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) är en metod för att verifiera användarens identitet med hjälp av en annan metod än bara ett användarnamn och lösenord. Den här autentiseringsmetoden hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning.

Genom att aktivera Azure MFA för dina användare kan du lägger till ett andra säkerhetslager säkerhet toouser inloggningar och transaktioner. I det här fallet en transaktion kan att komma åt ett dokument som finns på en filserver eller i SharePoint Online. Azure MFA hjälper även IT tooreduce hello sannolikheten att avslöjade autentiseringsuppgifter kommer att ha åtkomst tooorganization data.

Exempel: Om du införa Azure MFA för användarna och konfigurera den toouse ett telefonsamtal eller SMS som verifiering om hello användarens autentiseringsuppgifter har komprometterats hello angripare kommer inte att kunna tooaccess alla resurser eftersom han inte har åtkomst till toouser phone. Organisationer som inte lägger till den här extra skyddslager identitet är mer känslig för autentiseringsuppgifter attack med lösenordsstöld, vilket kan leda till röjande av toodata.

Ett alternativ för organisationer som vill tookeep hello autentisering kontroll lokalt är toouse [Azure Multi-Factor Authentication-servern](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), kallas även MFA lokala. Du kommer fortfarande att kunna tooenforce multifaktorautentisering, samtidigt som hello MFA server lokalt med hjälp av den här metoden.

Mer information om Azure MFA finns hello artikel [komma igång med Azure Multi-Factor Authentication i molnet hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Använd rollbaserad åtkomstkontroll (RBAC)
Begränsa åtkomst baserat på hello [måste tooknow](https://en.wikipedia.org/wiki/Need_to_know) och [minsta privilegium](https://en.wikipedia.org/wiki/Principle_of_least_privilege) säkerhetsprinciper. Det här är viktigt för organisationer som vill tooenforce säkerhetsprinciper för dataåtkomst. Azure rollbaserad åtkomstkontroll (RBAC) kan vara används tooassign behörigheter toousers, grupper och program för ett visst område. hello omfånget för en rolltilldelning kan vara en prenumeration, resursgrupp eller en enskild resurs.

Du kan utnyttja [inbyggda RBAC-roller](../active-directory/role-based-access-built-in-roles.md) i Azure tooassign behörighet toousers. Överväg att använda *Storage-konto deltagare* för molnoperatörer som behöver toomanage storage-konton och *klassiska Storage-konto deltagare* rollen toomanage klassiska lagringskonton. För molnoperatörer som behöver toomanage virtuella datorer och storage-konto, Överväg att lägga till dem för*Virtual Machine-deltagare* roll.

Organisationer som inte behöver använda data åtkomstkontroll genom att utnyttja funktioner, till exempel RBAC kan ger fler behörigheter än vad som krävs för sina användare. Detta kan leda toodata angrepp genom att vissa användare har åtkomst toodata som de inte borde ha hello första plats.

Du kan lära dig mer om Azure RBAC genom att läsa hello artikel [rollbaserad åtkomstkontroll i](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Kryptera virtuella Azure-datorer
I många organisationer [datakryptering i viloläge](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) är ett obligatoriskt steg mot data sekretess, efterlevnad och data suveränitet. Azure Disk Encryption kan IT-administratörer tooencrypt Windows och Linux IaaS virtuell dator (VM)-diskar. Azure Disk Encryption utnyttjar hello branschen standard BitLocker för Windows och hello DM-Crypt i Linux tooprovide volymkryptering för hello OS och hello datadiskar.

Du kan utnyttja Azure Disk Encryption toohelp skydda och skydda dina data toomeet din organisations krav för säkerhet och efterlevnad. Organisationer bör överväga att använda kryptering toohelp minimera risker relaterade toounauthorized dataåtkomst. Vi rekommenderar också att du krypterar enheter tidigare toowriting känsliga data toothem.

Gör att tooencrypt datavolymer för den virtuella datorn och startvolymen i ordning tooprotect vilande data i Azure storage-konto. Skydda hello krypteringsnycklar och hemligheter genom att utnyttja [Azure Key Vault](../key-vault/key-vault-whatis.md).

Överväg att hello efter kryptering Metodtips för lokala Windows-servrar:

* Använd [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) för kryptering av data
* Lagra återställningsinformation i AD DS.
* Om det finns några problem att BitLocker-nycklar har komprometterats, rekommenderar vi att du antingen formatera hello enhet tooremove alla instanser av hello BitLocker metadata från hello enhet eller som du dekryptera och kryptera hello hela enheten igen.

Organisationer som inte behöver använda datakryptering är mer troligt toobe exponeras toodata problem, till exempel skadliga eller icke-registrerade användare stjäla data och komprometteras konton få obehörig åtkomst toodata Rensa format. Förutom dessa risker, måste du bekräfta att de är et och använder hello rätt säkerhet kontroller tooenhance datasäkerhet företag som har toocomply med industrins föreskrifter.

Du kan lära dig mer om Azure Disk Encryption genom att läsa hello artikel [Azure Disk Encryption för Windows och Linux IaaS-VM](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Använd Maskinvarusäkerhetsmoduler
Branschen krypteringslösningar använder hemliga nycklar tooencrypt data. Det är därför viktigt att nycklarna lagras på ett säkert sätt. Nyckelhantering blir en del av dataskydd, eftersom den är balanserad toostore hemliga nycklar som används tooencrypt data.

Azure disk encryption använder [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) toohelp du styra och hantera disk krypteringsnycklar och hemligheter i prenumerationen nyckelvalv samtidigt som du säkerställer att krypteras alla data i hello virtuella diskar i vila i din Azure lagring. Du bör använda Azure Key Vault tooaudit nycklar och användning av principen.

Det finns många risk-relaterade toonot med lämpliga säkerhetsåtgärder i plats tooprotect hello hemliga nycklar som används tooencrypt dina data. Om angripare har åtkomst toohello hemliga nycklar, de kommer att kunna toodecrypt hello data och eventuellt få åtkomst till tooconfidential information.

Du kan lära dig mer om allmänna rekommendationer för certifikathantering i Azure genom att läsa hello artikel [certifikathantering i Azure: bra att veta](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Mer information om Azure Key Vault [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Hantera med säkra arbetsstationer
Eftersom hello majoriteten av hello attacker mål hello slutanvändaren blir hello endpoint hello primära punkter för angrepp. Om en angripare komprometterar hello slutpunkt, kan han använda hello användarens autentiseringsuppgifter toogain åtkomst tooorganization's data. De flesta endpoint-attacker är kan tootake nytta av hello faktum att slutanvändare administratörer i sina lokala arbetsstationer.

Du kan minska riskerna med hjälp av en säker hanterings-arbetsstation. Vi rekommenderar att du använder en [privilegierad åtkomst arbetsstationer (PAW)](https://technet.microsoft.com/library/mt634654.aspx) tooreduce hello risken för angrepp i arbetsstationer. Dessa säker hantering av arbetsstationer kan hjälpa dig att minimera vissa av dessa attacker försäkra dig om dina data är säkrare. Se till att toouse PAW tooharden och lås ned din arbetsstation. Det här är ett viktigt steg tooprovide hög säkerhet garantier för känsliga konton, uppgifter och dataskydd.

Avsaknad av endpoint protection kan medföra risker för dina data, se till att tooenforce säkerhetsprinciper på alla enheter som används tooconsume data, oavsett hello Dataplats (molnet eller lokalt).

Du kan lära dig mer om privilegierad åtkomst till arbetsstationen genom att läsa hello artikel [skydda privilegierad åtkomst](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>Aktivera SQL-datakryptering
[Azure SQL Database transparent datakryptering](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) skyddar mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av hello databas, tillhörande säkerhetskopior och transaktionsloggfiler utan vilande kräver ändringar toohello program.  TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen.

Även när hello hela lagring krypteras, är det mycket viktigt tooalso kryptera din databas sig själv. Det här är en implementering av hello skydd på djupet metod för att skydda data. Om du använder [Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) och tooprotect känsliga data, till exempel kreditkort eller personnummer, kan du kryptera databaser med FIPS 140-2-verifierade 256-bitars AES-kryptering som uppfyller hello av många branschstandarder (till exempel HIPAA, PCI).

Det är viktigt toounderstand som relaterade filer för[bufferten poolen tillägget](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) är inte krypterat när en databas har krypterats med TDE. Du måste använda filen kryptering på Systemverktyg som BitLocker eller hello [Krypterande filsystem](https://technet.microsoft.com/library/cc700811.aspx) (EFS) för BPE relaterade filer.

Eftersom en behörig användare som en administratör eller en databasadministratör kan komma åt hello data även om hello databasen är krypterad med TDE, bör du också följa hello rekommendationer nedan:

* SQL-autentisering på hello databasnivå
* Azure AD-autentisering med hjälp av RBAC-roller
* Användare och program bör använda separata konton tooauthenticate. Det här sättet kan du begränsa hello behörigheterna toousers och program och minska hello risker skadlig programvara
* Implementera databasen säkerhetsnivå via fasta databasroller (till exempel db_datareader eller db_datawriter) eller du kan skapa anpassade roller för ditt program toogrant uttryckliga behörigheter tooselected databasobjekt

Organisationer som inte använder kryptering på databasen kan vara mer sårbara för attacker som kan påverka data som finns i SQL-databaser.

Mer information om SQL TDE kryptering genom att läsa hello artikel [Transparent datakryptering med Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Skydda data under överföring
Skydda data under överföringen ska väsentlig del av strategin för skydd av data. Eftersom data flyttar fram och tillbaka från många platser, rekommenderas hello allmänna att du alltid använda SSL/TLS-protokollen tooexchange data mellan olika platser. I vissa fall kanske du vill tooisolate hello hela kommunikationskanalen mellan din lokala och moln infrastruktur med hjälp av ett virtuellt privat nätverk (VPN).

För data som flyttas mellan din lokala infrastruktur och Azure, bör du lämpliga skyddsåtgärder, till exempel HTTPS eller VPN.

För organisationer som behöver toosecure åtkomst från flera arbetsstationer finnas lokalt tooAzure använder [Azure plats-till-plats-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

För organisationer som behöver toosecure åtkomst från en arbetsstation som finns lokalt tooAzure, Använd [punkt-till-plats VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Större datauppsättningar kan flyttas dedikerade snabba WAN-länk som [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Om du väljer toouse ExpressRoute, du kan också kryptera hello data på hello på programnivå med hjälp av [SSL/TLS](https://support.microsoft.com/kb/257591) eller andra protokoll för extra skydd.

Om du interagerar med Azure Storage via hello Azure-portalen, alla transaktioner ska ske via HTTPS. [Storage REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kan också vara används toointeract med [Azure Storage](https://azure.microsoft.com/services/storage/) och [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organisationer som inte tooprotect data under överföring är mer känslig för [man-in-the-middle-attacker](https://technet.microsoft.com/library/gg195821.aspx), [avlyssning](https://technet.microsoft.com/library/gg195641.aspx) och sessionskapning. Dessa attacker kan vara hello första steget i att få åtkomst till tooconfidential data.

Du kan lära dig mer om Azure VPN-alternativ genom att läsa hello artikel [planering och design för VPN-Gateway](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Tillämpa fil nivån datakryptering
Ett extra skyddslager som kan öka hello säkerhetsnivån för dina data krypterar hello själva filen, oavsett hello filens plats.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) använder kryptering, identitet och auktorisering principer toohelp skydda filer och e-post. Azure RMS fungerar över flera enheter – telefoner, surfplattor och datorer genom att skydda både inom organisationen och utanför organisationen. Den här funktionen är möjligt eftersom Azure RMS lägger till en skyddsnivå som finns kvar med hello data, även när de lämnar organisationens gränser.

När du använder Azure RMS tooprotect dina filer som används med branschstandardkryptografi fullständigt stöd för [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). När du använder Azure RMS för dataskydd, har hello garantier att hello skyddet kvar hello-fil, även om det är kopierade toostorage som inte är under hello kontroll av IT-avdelningen, till exempel en molntjänst för lagring. hello samma inträffar för filer som delas via e-post, skyddas hello-filen som en bifogad fil tooan e-postmeddelande, med instruktioner för hur tooopen hello skyddade bifogade filen.

När du planerar för införandet av Azure RMS rekommenderar vi hello följande:

* Installera hello [RMS-delningsappen](https://technet.microsoft.com/library/dn339006.aspx). Den här appen integreras med Office program genom att installera ett Office-tillägget så att användarna kan enkelt skydda filer direkt.
* Konfigurera program och tjänster toosupport Azure RMS
* Skapa [anpassade mallar](https://technet.microsoft.com/library/dn642472.aspx) som motsvarar dina affärsbehov. Exempel: en mall för övre hemlig information som ska användas i alla översta hemlighet relaterade e-postmeddelanden.

Organisationer som är beroende av på [dataklassificering](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) och Filskydd kan vara svårare toodata sprids. Organisationer inte utan rätt Filskydd vara kan tooobtain affärsinsikter, övervaka missbruk och förhindra obehörig åtkomst toofiles.

Du kan lära dig mer om Azure RMS genom att läsa hello artikel [komma igång med Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).
