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
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a><span data-ttu-id="e5cb5-103">Azure krypteringstekniker: skydda personliga data vilande med kryptering</span><span class="sxs-lookup"><span data-stu-id="e5cb5-103">Azure encryption technologies: Protect personal data at rest with encryption</span></span>

<span data-ttu-id="e5cb5-104">Den här artikeln hjälper dig att förstå och använda Azure kryptering tekniker toosecure data i vila.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-104">This article helps you understand and use Azure encryption technologies toosecure data at rest.</span></span>

<span data-ttu-id="e5cb5-105">Kryptering av vilande data är mycket viktigt som en bästa praxis tooprotect känsliga eller personliga data och toomeet kompatibilitet och krav på sekretess.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-105">Encryption of data at rest is essential as a best practice tooprotect sensitive or personal data and toomeet compliance and data privacy requirements.</span></span>
<span data-ttu-id="e5cb5-106">Kryptering i vila är utformad tooprevent hello angripare från att komma åt hello okrypterade data genom att säkerställa hello krypteras data på disken.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-106">Encryption at rest is designed tooprevent hello attacker from accessing hello unencrypted data by ensuring hello data is encrypted when on disk.</span></span>

## <a name="scenario"></a><span data-ttu-id="e5cb5-107">Scenario</span><span class="sxs-lookup"><span data-stu-id="e5cb5-107">Scenario</span></span> 

<span data-ttu-id="e5cb5-108">Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavsområdet och baltiska havet samt hello brittiska staterna.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="e5cb5-109">toosupport dessa ansträngningar den genererade flera mindre kryssning rader i Italien, Tyskland, Danmark och hello Storbritannien</span><span class="sxs-lookup"><span data-stu-id="e5cb5-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and hello U.K.</span></span>

<span data-ttu-id="e5cb5-110">hello företag använder Microsoft Azure toostore företagsdata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="e5cb5-111">Detta kan inkludera medarbetare och/eller kundinformation som:</span><span class="sxs-lookup"><span data-stu-id="e5cb5-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="e5cb5-112">adresser</span><span class="sxs-lookup"><span data-stu-id="e5cb5-112">addresses</span></span>
- <span data-ttu-id="e5cb5-113">telefonnummer</span><span class="sxs-lookup"><span data-stu-id="e5cb5-113">phone numbers</span></span>
- <span data-ttu-id="e5cb5-114">skatt ID-nummer</span><span class="sxs-lookup"><span data-stu-id="e5cb5-114">tax identification numbers</span></span>
- <span data-ttu-id="e5cb5-115">medicinska information</span><span class="sxs-lookup"><span data-stu-id="e5cb5-115">medical information</span></span>
- <span data-ttu-id="e5cb5-116">kreditkortsinformation</span><span class="sxs-lookup"><span data-stu-id="e5cb5-116">credit card information</span></span>

<span data-ttu-id="e5cb5-117">hello företag måste skydda hello säkerheten för de anställda och kunder data när du gör data tillgängliga toothose avdelningar som behöver den.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-117">hello company must protect hello privacy of employee and customer data while making data accessible toothose departments that need it.</span></span> <span data-ttu-id="e5cb5-118">(till exempel löneuppgifter och -reservationer avdelningar)</span><span class="sxs-lookup"><span data-stu-id="e5cb5-118">(such as payroll and reservations departments)</span></span>

<span data-ttu-id="e5cb5-119">hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-119">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

### <a name="problem-statement"></a><span data-ttu-id="e5cb5-120">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="e5cb5-120">Problem statement</span></span>

<span data-ttu-id="e5cb5-121">hello företag måste skydda hello säkerheten för de anställda och kunder personliga data när du gör data tillgängliga toothose avdelningar som behöver den (till exempel löneuppgifter och -reservationer avdelningar).</span><span class="sxs-lookup"><span data-stu-id="e5cb5-121">hello company must protect hello privacy of employees’ and customers’ personal data while making data accessible toothose departments that need it (such as payroll and reservations departments).</span></span> <span data-ttu-id="e5cb5-122">Den här personliga data lagras utanför hello företagets kontrollerade datacenter och är inte under hello företagets fysisk kontroll.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-122">This personal data is stored outside of hello corporate-controlled data center and is not under hello company’s physical control.</span></span>

### <a name="company-goal"></a><span data-ttu-id="e5cb5-123">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="e5cb5-123">Company goal</span></span>

<span data-ttu-id="e5cb5-124">Som en del av en flera lager skydd på djupet säkerhetsstrategi är det en företagets mål tooensure att alla datakällor som innehåller personuppgifter krypteras, inklusive de som finns i molnet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-124">As part of a multi-layered defense-in-depth security strategy, it is a company goal tooensure that all data sources that contain personal data are encrypted, including those residing in cloud storage.</span></span> <span data-ttu-id="e5cb5-125">Om obehöriga personer få åtkomst till toohello personliga data måste den vara i ett formulär som ska läsa den.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-125">If unauthorized persons gain access toohello personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="e5cb5-126">Bör vara enkelt och transparent – för användare och administratörer att tillämpa kryptering.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-126">Applying encryption should be easy, or transparent – for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="e5cb5-127">Lösningar</span><span class="sxs-lookup"><span data-stu-id="e5cb5-127">Solutions</span></span>

<span data-ttu-id="e5cb5-128">Azure tillhandahåller flera verktyg och tekniker toohelp du skydda personliga data i vila genom att kryptera den.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-128">Azure services provide multiple tools and technologies toohelp you protect personal data at rest by encrypting it.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="e5cb5-129">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e5cb5-129">Azure Key Vault</span></span>

<span data-ttu-id="e5cb5-130">[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) ger säker lagring för hello nycklar används tooencrypt data i vila i Azure-tjänster och rekommenderas hello viktiga lösning för lagring och hantering.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-130">[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) provides secure storage for hello keys used tooencrypt data at rest in Azure services and is hello recommended key storage and management solution.</span></span> <span data-ttu-id="e5cb5-131">Hantering av nycklar för kryptering är viktigt toosecuring lagrade data.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-131">Encryption key management is essential toosecuring stored data.</span></span>

#### <a name="how-do-i-use-azure-key-vault-tooprotect-keys-that-encrypt-personal-data"></a><span data-ttu-id="e5cb5-132">Hur använder Azure Key Vault tooprotect för att kryptera personliga data?</span><span class="sxs-lookup"><span data-stu-id="e5cb5-132">How do I use Azure Key Vault tooprotect keys that encrypt personal data?</span></span>

<span data-ttu-id="e5cb5-133">toouse Azure Key Vault, behöver du en prenumeration tooan Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-133">toouse Azure Key Vault, you need a subscription tooan Azure account.</span></span> <span data-ttu-id="e5cb5-134">Du måste också Azure PowerShell har installerats.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-134">You also need Azure PowerShell installed.</span></span> <span data-ttu-id="e5cb5-135">Stegen är med hjälp av PowerShell-cmdlets toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="e5cb5-135">Steps include using PowerShell cmdlets toodo hello following:</span></span>

1. <span data-ttu-id="e5cb5-136">Ansluta tooyour prenumerationer</span><span class="sxs-lookup"><span data-stu-id="e5cb5-136">Connect tooyour subscriptions</span></span>

2. <span data-ttu-id="e5cb5-137">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="e5cb5-137">Create a key vault</span></span>

3. <span data-ttu-id="e5cb5-138">Lägga till en nyckel eller Hemlig toohello nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="e5cb5-138">Add a key or secret toohello key vault</span></span>

4. <span data-ttu-id="e5cb5-139">Registrera program som ska använda hello nyckelvalv med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e5cb5-139">Register applications that will use hello key vault with Azure Active Directory</span></span>

5. <span data-ttu-id="e5cb5-140">Auktorisera hello program toouse hello nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-140">Authorize hello applications toouse hello key or secret</span></span>

<span data-ttu-id="e5cb5-141">toocreate nyckelvalvet, Använd hello ny AzureRmKeyVault PowerShell-cmdleten.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-141">toocreate a key vault, use hello New-AzureRmKeyVault PowerShell CmDlt.</span></span> <span data-ttu-id="e5cb5-142">Du tilldelar en valvet namn, resursgruppens namn och geografisk plats.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-142">You will assign a vault name, resource group name, and geographic location.</span></span> <span data-ttu-id="e5cb5-143">Hej valvnamnet ska du använda vid hantering av nycklar via andra cmdletar.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-143">You’ll use hello vault name when managing keys via other Cmdlets.</span></span> <span data-ttu-id="e5cb5-144">Program som använder hello valvet via hello REST API använder hello valvet URI.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-144">Applications that use hello vault through hello REST API will use hello vault URI.</span></span>

<span data-ttu-id="e5cb5-145">Azure Key Vault kan tillhandahålla en programvaruskyddad nyckel för dig, eller så kan du importera en befintlig nyckel i en. PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-145">Azure Key Vault can provide a software-protected key for you, or you can import an existing key in a .PFX file.</span></span> <span data-ttu-id="e5cb5-146">Du kan också lagra hemligheter (lösenord) i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-146">You can also store secrets (passwords) in hello vault.</span></span>

<span data-ttu-id="e5cb5-147">Du kan också skapa en nyckel i din lokala HSM och överför den tooHSMs i hello Key Vault-tjänsten, utan hello nyckel lämnar hello HSM gräns.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-147">You can also generate a key in your local HSM and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary.</span></span>

<span data-ttu-id="e5cb5-148">För detaljerade anvisningar om hur du använder Azure Key Vault följer hello stegen i [Kom igång med Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span><span class="sxs-lookup"><span data-stu-id="e5cb5-148">For detailed instructions on using Azure Key Vault, follow hello steps in [Get Started with Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span></span>

<span data-ttu-id="e5cb5-149">En lista med PowerShell-Cmdlets som används med Azure Key Vault finns [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="e5cb5-149">For a list of PowerShell Cmdlets used with Azure Key Vault, see [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

### <a name="azure-disk-encryption-for-windows"></a><span data-ttu-id="e5cb5-150">Azure Disk Encryption för Windows</span><span class="sxs-lookup"><span data-stu-id="e5cb5-150">Azure Disk Encryption for Windows</span></span>

<span data-ttu-id="e5cb5-151">[Azure Disk Encryption för Windows och Linux IaaS-VM](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) skyddar personliga data i vila på virtuella Azure-datorer och kan integreras med Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-151">[Azure Disk Encryption for Windows and Linux IaaS VMs](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) protects personal data at rest on Azure virtual machines and integrates with Azure Key Vault.</span></span> <span data-ttu-id="e5cb5-152">Azure Disk Encryption använder [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) i Windows och [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) i Linux tooencrypt hello både Operativsystemet och hello datadiskar.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-152">Azure Disk Encryption uses [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) in Windows and [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) in Linux tooencrypt both hello OS and hello data disks.</span></span> <span data-ttu-id="e5cb5-153">Azure Disk Encryption stöds på Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 och på Windows 8 och Windows 10-klienter.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-153">Azure Disk Encryption is supported on Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, and on Windows 8 and Windows 10 clients.</span></span>

#### <a name="how-do-i-use-azure-disk-encryption-tooprotect-personal-data"></a><span data-ttu-id="e5cb5-154">Hur använder Azure Disk Encryption tooprotect personliga data?</span><span class="sxs-lookup"><span data-stu-id="e5cb5-154">How do I use Azure Disk Encryption tooprotect personal data?</span></span>

<span data-ttu-id="e5cb5-155">toouse Azure Disk Encryption, behöver du en prenumeration tooan Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-155">toouse Azure Disk Encryption, you need a subscription tooan Azure account.</span></span> <span data-ttu-id="e5cb5-156">tooenable Azure Disk Encryption för Windows och Linux virtuella datorer, hello följande:</span><span class="sxs-lookup"><span data-stu-id="e5cb5-156">tooenable Azure Disk Encryption for Windows and Linux VMs, do hello following:</span></span>

1. <span data-ttu-id="e5cb5-157">Använd hello Azure Disk Encryption Resource Manager-mall, PowerShell eller hello kommandoradsgränssnittet (CLI) tooenable diskkryptering och ange konfigurationen för kryptering.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-157">Use hello Azure Disk Encryption Resource Manager template, PowerShell, or hello command line interface (CLI) tooenable disk encryption and specify the  encryption configuration.</span></span> 

2. <span data-ttu-id="e5cb5-158">Bevilja åtkomst toohello Azure-plattformen tooread hello kryptering material från nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-158">Grant access toohello Azure platform tooread hello encryption material from your key vault.</span></span>

3. <span data-ttu-id="e5cb5-159">Ange ett Azure Active Directory (AAD) programmet identitet toowrite hello kryptering viktiga väsentlig tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-159">Provide an Azure Active Directory (AAD) application identity toowrite hello encryption key material tooyour key vault.</span></span>

<span data-ttu-id="e5cb5-160">Azure kommer att uppdateras hello VM och hello nyckelvalv konfiguration och konfigurera din krypterade VM.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-160">Azure will update hello VM and hello key vault configuration, and set up your encrypted VM.</span></span>

<span data-ttu-id="e5cb5-161">När du konfigurerar ditt nyckelvalv toosupport Azure Disk Encryption du lägga till en krypteringsnyckel nyckel (KEK) för ökad säkerhet och toosupport säkerhetskopiering av krypterade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-161">When you set up your key vault toosupport Azure Disk Encryption, you can add a key encryption key (KEK) for added security and toosupport backup of encrypted virtual machines.</span></span>

![](media/protect-personal-data-at-rest/create-key.png)

<span data-ttu-id="e5cb5-162">Detaljerade anvisningar för specifika distributionsscenarier och användarupplevelser ingår i [Azure Disk Encryption för Windows och Linux virtuella IaaS-datorer.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span><span class="sxs-lookup"><span data-stu-id="e5cb5-162">Detailed instructions for specific deployment scenarios and user experiences are included in [Azure Disk Encryption for Windows and Linux IaaS VMs.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span></span>

### <a name="azure-storage-service-encryption"></a><span data-ttu-id="e5cb5-163">Kryptering av Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="e5cb5-163">Azure Storage Service Encryption</span></span>

<span data-ttu-id="e5cb5-164">[Azure Storage Service kryptering (SSE) för Data i vila](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) hjälper dig att skydda och skydda dina data toomeet din organisations säkerhet och efterlevnad åtaganden.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-164">[Azure Storage Service Encryption (SSE) for Data at Rest](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) helps you protect and safeguard your data toomeet your organizational security and compliance commitments.</span></span> <span data-ttu-id="e5cb5-165">Azure Storage krypterar dina data med hjälp av 256-bitars AES-kryptering tidigare toopersisting toostorage, och automatiskt dekrypterar den tidigare tooretrieval.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-165">Azure Storage automatically encrypts your data using 256-bit AES encryption prior toopersisting toostorage, and decrypts it prior tooretrieval.</span></span> <span data-ttu-id="e5cb5-166">Den här tjänsten är tillgänglig för Azure-BLOB och filer.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-166">This service is available for Azure Blobs and Files.</span></span>

#### <a name="how-do-i-use-storage-service-encryption-tooprotect-personal-data"></a><span data-ttu-id="e5cb5-167">Hur använder Lagringstjänstens kryptering tooprotect personliga data?</span><span class="sxs-lookup"><span data-stu-id="e5cb5-167">How do I use Storage Service Encryption tooprotect personal data?</span></span>

<span data-ttu-id="e5cb5-168">tooenable Storage Service-kryptering, hello följande:</span><span class="sxs-lookup"><span data-stu-id="e5cb5-168">tooenable Storage Service Encryption, do hello following:</span></span>

1. <span data-ttu-id="e5cb5-169">Logga in på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-169">Log into hello Azure portal.</span></span>

2. <span data-ttu-id="e5cb5-170">Välj ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-170">Select a storage account.</span></span>

3. <span data-ttu-id="e5cb5-171">Välj kryptering i inställningar under avsnittet för hello Blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-171">In Settings, under hello Blob Service section, select Encryption.</span></span>

4. <span data-ttu-id="e5cb5-172">Välj kryptering under hello Filtjänst avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-172">Under hello File Service section, select Encryption.</span></span>

<span data-ttu-id="e5cb5-173">När du har klickat på hello krypteringsinställning kan du aktivera eller inaktivera Storage Service-kryptering.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-173">After you click hello Encryption setting, you can enable or disable Storage Service Encryption.</span></span>

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

<span data-ttu-id="e5cb5-174">Nya data att krypteras.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-174">New data will be encrypted.</span></span> <span data-ttu-id="e5cb5-175">Data i befintliga filer i det här lagringskontot kommer att vara okrypterad.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-175">Data in existing files in this storage account will remain unencrypted.</span></span>

<span data-ttu-id="e5cb5-176">När du aktiverar kryptering, kopierar du data toohello storage-konto med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="e5cb5-176">After enabling encryption, copy data toohello storage account using one of hello following methods:</span></span>

1. <span data-ttu-id="e5cb5-177">Kopiera BLOB eller filer med hello [AzCopy kommandoradsverktyget](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span><span class="sxs-lookup"><span data-stu-id="e5cb5-177">Copy blobs or files with hello [AzCopy Command Line utility](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span></span>

2. <span data-ttu-id="e5cb5-178">[Montera en filresurs med hjälp av SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) så du kan använda ett verktyg som till exempel Robocopy toocopy filer.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-178">[Mount a file share using SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) so you can use a utility such as Robocopy toocopy files.</span></span>

3. <span data-ttu-id="e5cb5-179">Kopiera blob eller filen data tooand från blob storage eller mellan lagringskonton med [Lagringsklientbiblioteken, till exempel .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="e5cb5-179">Copy blob or file data tooand from blob storage or between storage accounts using [Storage Client Libraries such as .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span></span>

4.  <span data-ttu-id="e5cb5-180">Använd en [Lagringsutforskaren](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) tooupload blobbar tooyour storage-konto med kryptering aktiverat.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-180">Use a [Storage Explorer](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) tooupload blobs tooyour storage account with encryption enabled.</span></span>

### <a name="transparent-data-encryption"></a><span data-ttu-id="e5cb5-181">Transparent datakryptering</span><span class="sxs-lookup"><span data-stu-id="e5cb5-181">Transparent Data Encryption</span></span>

<span data-ttu-id="e5cb5-182">Transparent Data kryptering (TDE) är en funktion i SQL Azure som du kan kryptera data på både hello databas- och programnivå.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-182">Transparent Data Encryption (TDE) is a feature in SQL Azure by which you can encrypt data at both hello database and server levels.</span></span> <span data-ttu-id="e5cb5-183">TDE är nu aktiverad som standard på alla nya databaser.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-183">TDE is now enabled by default on all newly created databases.</span></span> <span data-ttu-id="e5cb5-184">TDE utför realtid i/o-kryptering och dekryptering av hello data och loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-184">TDE performs real-time I/O encryption and decryption of hello data and log files.</span></span>

#### <a name="how-do-i-use-tde-tooprotect-personal-data"></a><span data-ttu-id="e5cb5-185">Hur använder TDE tooprotect personliga data?</span><span class="sxs-lookup"><span data-stu-id="e5cb5-185">How do I use TDE tooprotect personal data?</span></span>

<span data-ttu-id="e5cb5-186">Du kan konfigurera TDE via hello Azure-portalen med hjälp av hello REST API eller med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-186">You can configure TDE through hello Azure portal, by using hello REST API, or by using PowerShell.</span></span> <span data-ttu-id="e5cb5-187">tooenable TDE i en befintlig databas med hjälp av hello Azure Portal hello följande:</span><span class="sxs-lookup"><span data-stu-id="e5cb5-187">tooenable TDE on an existing database using hello Azure Portal, do hello following:</span></span>

1. <span data-ttu-id="e5cb5-188">Besök hello Azure portal på <https://portal.azure.com> och logga in med ditt Azure-administratör eller deltagare.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-188">Visit hello Azure portal at <https://portal.azure.com> and sign-in with your Azure Administrator or Contributor account.</span></span>

2. <span data-ttu-id="e5cb5-189">Klicka på tooBROWSE på hello vänstra banderoll, och klicka på SQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-189">On hello left banner, click tooBROWSE, and then click SQL databases.</span></span>

3. <span data-ttu-id="e5cb5-190">Med SQL-databaser som har valts i hello vänstra fönstret klickar du på databasen.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-190">With SQL databases selected in hello left pane, click your user database.</span></span>

4. <span data-ttu-id="e5cb5-191">Klicka på alla inställningar i hello databasbladet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-191">In hello database blade, click All settings.</span></span>

5. <span data-ttu-id="e5cb5-192">Klicka på Transparent data kryptering del tooopen hello Transparent data kryptering bladet i hello inställningar-bladet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-192">In hello Settings blade, click Transparent data encryption part tooopen hello Transparent data encryption blade.</span></span>

6. <span data-ttu-id="e5cb5-193">Flytta hello Data kryptering knappen tooOn i hello Data kryptering bladet och klicka på Spara (överst hello på hello sidan) tooapply hello inställningen.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-193">In hello Data encryption blade, move hello Data encryption button tooOn, and then click Save (at hello top of hello page) tooapply hello setting.</span></span> <span data-ttu-id="e5cb5-194">hello krypteringsstatus kommer ungefär hello fortskrider hello transparent datakryptering.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-194">hello Encryption status will approximate hello progress of hello transparent data encryption.</span></span>

![Aktivera kryptering av data](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

<span data-ttu-id="e5cb5-196">Anvisningar om hur tooenable TDE och information om dekryptera TDE-skyddade databaser och mer finns i artikeln hello [Transparent datakryptering med Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span><span class="sxs-lookup"><span data-stu-id="e5cb5-196">Instructions on how tooenable TDE and information on decrypting TDE-protected databases and more can be found in hello article [Transparent Data Encryption with Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span></span>

## <a name="summary"></a><span data-ttu-id="e5cb5-197">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e5cb5-197">Summary</span></span>

<span data-ttu-id="e5cb5-198">hello företag kan göra dess mål för att kryptera personliga data som lagras i hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-198">hello company can accomplish its goal of encrypting personal data stored in hello Azure cloud.</span></span> <span data-ttu-id="e5cb5-199">De kan göra detta med hjälp av Azure Disk Encryption för skydda hela volymer.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-199">They can do this by using Azure Disk Encryption too protect entire volumes.</span></span> <span data-ttu-id="e5cb5-200">Detta kan inkludera både hello operativsystemets filer och datafiler som innehåller personligt identifierbar information och andra känsliga data.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-200">This may include both hello operating system files and data files that hold personal identifiable information and other sensitive data.</span></span> <span data-ttu-id="e5cb5-201">Azure Storage Service-kryptering kan vara används tooprotect personliga data som lagras i BLOB och filer.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-201">Azure Storage Service encryption can be used tooprotect personal data that is stored in blobs and files.</span></span> <span data-ttu-id="e5cb5-202">För data som lagras i Azure SQL-databaser, skyddar Transparent datakryptering mot obehörig exponering av personlig information.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-202">For data that is stored in Azure SQL databases, Transparent Data Encryption provides protection from unauthorized exposure of personal information.</span></span>

<span data-ttu-id="e5cb5-203">tooprotect hello nycklar som används tooencrypt data i Azure, hello företaget kan använda Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-203">tooprotect hello keys that are used tooencrypt data in Azure, hello company can use Azure Key Vault.</span></span> <span data-ttu-id="e5cb5-204">Detta förenklar hello nyckelhanteringen och låter hello företagets toomaintain kontrollen över nycklar som kommer åt och krypterar personliga data.</span><span class="sxs-lookup"><span data-stu-id="e5cb5-204">This streamlines hello key management process and enables hello company toomaintain control of keys that access and encrypt personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5cb5-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5cb5-205">Next steps</span></span>

- [<span data-ttu-id="e5cb5-206">Felsökningsguide för Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="e5cb5-206">Azure Disk Encryption Troubleshooting Guide</span></span>](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [<span data-ttu-id="e5cb5-207">Kryptera en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="e5cb5-207">Encrypt an Azure Virtual Machine</span></span>](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [<span data-ttu-id="e5cb5-208">Kryptering av data i Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e5cb5-208">Encryption of data in Azure Data Lake Store</span></span>](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [<span data-ttu-id="e5cb5-209">Azure DB Cosmos databaskryptering i vila</span><span class="sxs-lookup"><span data-stu-id="e5cb5-209">Azure Cosmos DB database encryption at rest</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
