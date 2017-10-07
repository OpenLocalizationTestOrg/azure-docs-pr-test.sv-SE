---
title: aaaAzure skydda personliga data vilande med kryptering | Microsoft Docs
description: "Den här artikeln är en del av en serie som hjälper dig att använda Azure tooprotect personliga data"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 9af182b4897f1d04f5f519e6671f53b85073bae1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a>Azure krypteringstekniker: skydda personliga data vilande med kryptering

Den här artikeln hjälper dig att förstå och använda Azure kryptering tekniker toosecure data i vila.

Kryptering av vilande data är mycket viktigt som en bästa praxis tooprotect känsliga eller personliga data och toomeet kompatibilitet och krav på sekretess.
Kryptering i vila är utformad tooprevent hello angripare från att komma åt hello okrypterade data genom att säkerställa hello krypteras data på disken.

## <a name="scenario"></a>Scenario 

Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavsområdet och baltiska havet samt hello brittiska staterna. toosupport dessa ansträngningar den genererade flera mindre kryssning rader i Italien, Tyskland, Danmark och hello Storbritannien

hello företag använder Microsoft Azure toostore företagsdata i hello molnet. Detta kan inkludera medarbetare och/eller kundinformation som:

- adresser
- telefonnummer
- skatt ID-nummer
- medicinska information
- kreditkortsinformation

hello företag måste skydda hello säkerheten för de anställda och kunder data när du gör data tillgängliga toothose avdelningar som behöver den. (till exempel löneuppgifter och -reservationer avdelningar)

hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.

### <a name="problem-statement"></a>Problembeskrivning

hello företag måste skydda hello säkerheten för de anställda och kunder personliga data när du gör data tillgängliga toothose avdelningar som behöver den (till exempel löneuppgifter och -reservationer avdelningar). Den här personliga data lagras utanför hello företagets kontrollerade datacenter och är inte under hello företagets fysisk kontroll.

### <a name="company-goal"></a>Företagets mål

Som en del av en flera lager skydd på djupet säkerhetsstrategi är det en företagets mål tooensure att alla datakällor som innehåller personuppgifter krypteras, inklusive de som finns i molnet. Om obehöriga personer få åtkomst till toohello personliga data måste den vara i ett formulär som ska läsa den. Bör vara enkelt och transparent – för användare och administratörer att tillämpa kryptering.

## <a name="solutions"></a>Lösningar

Azure tillhandahåller flera verktyg och tekniker toohelp du skydda personliga data i vila genom att kryptera den.

### <a name="azure-key-vault"></a>Azure Key Vault

[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) ger säker lagring för hello nycklar används tooencrypt data i vila i Azure-tjänster och rekommenderas hello viktiga lösning för lagring och hantering. Hantering av nycklar för kryptering är viktigt toosecuring lagrade data.

#### <a name="how-do-i-use-azure-key-vault-tooprotect-keys-that-encrypt-personal-data"></a>Hur använder Azure Key Vault tooprotect för att kryptera personliga data?

toouse Azure Key Vault, behöver du en prenumeration tooan Azure-konto. Du måste också Azure PowerShell har installerats. Stegen är med hjälp av PowerShell-cmdlets toodo hello följande:

1. Ansluta tooyour prenumerationer

2. Skapa ett nyckelvalv

3. Lägga till en nyckel eller Hemlig toohello nyckelvalv

4. Registrera program som ska använda hello nyckelvalv med Azure Active Directory

5. Auktorisera hello program toouse hello nyckel eller hemlighet.

toocreate nyckelvalvet, Använd hello ny AzureRmKeyVault PowerShell-cmdleten. Du tilldelar en valvet namn, resursgruppens namn och geografisk plats. Hej valvnamnet ska du använda vid hantering av nycklar via andra cmdletar. Program som använder hello valvet via hello REST API använder hello valvet URI.

Azure Key Vault kan tillhandahålla en programvaruskyddad nyckel för dig, eller så kan du importera en befintlig nyckel i en. PFX-filen. Du kan också lagra hemligheter (lösenord) i hello-valvet.

Du kan också skapa en nyckel i din lokala HSM och överför den tooHSMs i hello Key Vault-tjänsten, utan hello nyckel lämnar hello HSM gräns.

För detaljerade anvisningar om hur du använder Azure Key Vault följer hello stegen i [Kom igång med Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)

En lista med PowerShell-Cmdlets som används med Azure Key Vault finns [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

### <a name="azure-disk-encryption-for-windows"></a>Azure Disk Encryption för Windows

[Azure Disk Encryption för Windows och Linux IaaS-VM](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) skyddar personliga data i vila på virtuella Azure-datorer och kan integreras med Azure Key Vault. Azure Disk Encryption använder [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) i Windows och [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) i Linux tooencrypt hello både Operativsystemet och hello datadiskar. Azure Disk Encryption stöds på Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 och på Windows 8 och Windows 10-klienter.

#### <a name="how-do-i-use-azure-disk-encryption-tooprotect-personal-data"></a>Hur använder Azure Disk Encryption tooprotect personliga data?

toouse Azure Disk Encryption, behöver du en prenumeration tooan Azure-konto. tooenable Azure Disk Encryption för Windows och Linux virtuella datorer, hello följande:

1. Använd hello Azure Disk Encryption Resource Manager-mall, PowerShell eller hello kommandoradsgränssnittet (CLI) tooenable diskkryptering och ange konfigurationen för kryptering. 

2. Bevilja åtkomst toohello Azure-plattformen tooread hello kryptering material från nyckelvalvet.

3. Ange ett Azure Active Directory (AAD) programmet identitet toowrite hello kryptering viktiga väsentlig tooyour nyckelvalvet.

Azure kommer att uppdateras hello VM och hello nyckelvalv konfiguration och konfigurera din krypterade VM.

När du konfigurerar ditt nyckelvalv toosupport Azure Disk Encryption du lägga till en krypteringsnyckel nyckel (KEK) för ökad säkerhet och toosupport säkerhetskopiering av krypterade virtuella datorer.

![](media/protect-personal-data-at-rest/create-key.png)

Detaljerade anvisningar för specifika distributionsscenarier och användarupplevelser ingår i [Azure Disk Encryption för Windows och Linux virtuella IaaS-datorer.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)

### <a name="azure-storage-service-encryption"></a>Kryptering av Azure-lagringstjänst

[Azure Storage Service kryptering (SSE) för Data i vila](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) hjälper dig att skydda och skydda dina data toomeet din organisations säkerhet och efterlevnad åtaganden. Azure Storage krypterar dina data med hjälp av 256-bitars AES-kryptering tidigare toopersisting toostorage, och automatiskt dekrypterar den tidigare tooretrieval. Den här tjänsten är tillgänglig för Azure-BLOB och filer.

#### <a name="how-do-i-use-storage-service-encryption-tooprotect-personal-data"></a>Hur använder Lagringstjänstens kryptering tooprotect personliga data?

tooenable Storage Service-kryptering, hello följande:

1. Logga in på hello Azure-portalen.

2. Välj ett lagringskonto.

3. Välj kryptering i inställningar under avsnittet för hello Blob-tjänsten.

4. Välj kryptering under hello Filtjänst avsnitt.

När du har klickat på hello krypteringsinställning kan du aktivera eller inaktivera Storage Service-kryptering.

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

Nya data att krypteras. Data i befintliga filer i det här lagringskontot kommer att vara okrypterad.

När du aktiverar kryptering, kopierar du data toohello storage-konto med någon av följande metoder hello:

1. Kopiera BLOB eller filer med hello [AzCopy kommandoradsverktyget](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).

2. [Montera en filresurs med hjälp av SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) så du kan använda ett verktyg som till exempel Robocopy toocopy filer.

3. Kopiera blob eller filen data tooand från blob storage eller mellan lagringskonton med [Lagringsklientbiblioteken, till exempel .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).

4.  Använd en [Lagringsutforskaren](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) tooupload blobbar tooyour storage-konto med kryptering aktiverat.

### <a name="transparent-data-encryption"></a>Transparent datakryptering

Transparent Data kryptering (TDE) är en funktion i SQL Azure som du kan kryptera data på både hello databas- och programnivå. TDE är nu aktiverad som standard på alla nya databaser. TDE utför realtid i/o-kryptering och dekryptering av hello data och loggfilerna.

#### <a name="how-do-i-use-tde-tooprotect-personal-data"></a>Hur använder TDE tooprotect personliga data?

Du kan konfigurera TDE via hello Azure-portalen med hjälp av hello REST API eller med hjälp av PowerShell. tooenable TDE i en befintlig databas med hjälp av hello Azure Portal hello följande:

1. Besök hello Azure portal på <https://portal.azure.com> och logga in med ditt Azure-administratör eller deltagare.

2. Klicka på tooBROWSE på hello vänstra banderoll, och klicka på SQL-databaser.

3. Med SQL-databaser som har valts i hello vänstra fönstret klickar du på databasen.

4. Klicka på alla inställningar i hello databasbladet.

5. Klicka på Transparent data kryptering del tooopen hello Transparent data kryptering bladet i hello inställningar-bladet.

6. Flytta hello Data kryptering knappen tooOn i hello Data kryptering bladet och klicka på Spara (överst hello på hello sidan) tooapply hello inställningen. hello krypteringsstatus kommer ungefär hello fortskrider hello transparent datakryptering.

![Aktivera kryptering av data](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

Anvisningar om hur tooenable TDE och information om dekryptera TDE-skyddade databaser och mer finns i artikeln hello [Transparent datakryptering med Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)

## <a name="summary"></a>Sammanfattning

hello företag kan göra dess mål för att kryptera personliga data som lagras i hello Azure-molnet. De kan göra detta med hjälp av Azure Disk Encryption för skydda hela volymer. Detta kan inkludera både hello operativsystemets filer och datafiler som innehåller personligt identifierbar information och andra känsliga data. Azure Storage Service-kryptering kan vara används tooprotect personliga data som lagras i BLOB och filer. För data som lagras i Azure SQL-databaser, skyddar Transparent datakryptering mot obehörig exponering av personlig information.

tooprotect hello nycklar som används tooencrypt data i Azure, hello företaget kan använda Azure Key Vault. Detta förenklar hello nyckelhanteringen och låter hello företagets toomaintain kontrollen över nycklar som kommer åt och krypterar personliga data.

## <a name="next-steps"></a>Nästa steg

- [Felsökningsguide för Azure Disk Encryption](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [Kryptera en virtuell Azure-dator](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [Kryptering av data i Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [Azure DB Cosmos databaskryptering i vila](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
