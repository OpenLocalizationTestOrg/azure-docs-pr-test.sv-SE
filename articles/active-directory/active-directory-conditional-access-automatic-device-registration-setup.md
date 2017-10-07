---
title: "aaaHow tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory | Microsoft Docs"
description: "Konfigurera din domänanslutna Windows-enheter tooregister automatiskt med Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 6a1aab753f5456ed06ba7979ab05f70f29b4ddee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a><span data-ttu-id="95fa7-103">Hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95fa7-103">How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory</span></span>

<span data-ttu-id="95fa7-104">toouse [Azure Active Directory enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-azure-portal.md), dina datorer måste vara registrerad med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="95fa7-104">toouse [Azure Active Directory device-based conditional access](active-directory-conditional-access-azure-portal.md), your computers must be registered with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="95fa7-105">Du kan hämta en lista över registrerade enheter i organisationen med hjälp av hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet i hello [Azure Active Directory PowerShell-modulen](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="95fa7-105">You can get a list of registered devices in your organization by using hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in hello [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span> 

<span data-ttu-id="95fa7-106">Den här artikeln innehåller hello steg för att konfigurera hello automatisk registrering av Windows-domänanslutna enheter med Azure AD i din organisation.</span><span class="sxs-lookup"><span data-stu-id="95fa7-106">This article provides you with hello steps for configuring hello automatic registration of Windows domain-joined devices with Azure AD in your organization.</span></span>


<span data-ttu-id="95fa7-107">Mer information om:</span><span class="sxs-lookup"><span data-stu-id="95fa7-107">For more information about:</span></span>

- <span data-ttu-id="95fa7-108">Villkorlig åtkomst finns [Azure Active Directory enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="95fa7-108">Conditional access, see [Azure Active Directory device-based conditional access](active-directory-conditional-access-azure-portal.md).</span></span> 
- <span data-ttu-id="95fa7-109">Windows 10-enheter i hello arbetsplats och hello förbättrad upplevelse när registrerad med Azure AD finns [Windows 10 för hello enterprise: Använd enheter för arbetet](active-directory-azureadjoin-windows10-devices-overview.md).</span><span class="sxs-lookup"><span data-stu-id="95fa7-109">Windows 10 devices in hello workplace and hello enhanced experiences when registered with Azure AD, see [Windows 10 for hello enterprise: Use devices for work](active-directory-azureadjoin-windows10-devices-overview.md).</span></span>
- <span data-ttu-id="95fa7-110">Windows 10 Enterprise E3 i CSP finns hello [Windows 10 Enterprise E3 i CSP översikt](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).</span><span class="sxs-lookup"><span data-stu-id="95fa7-110">Windows 10 Enterprise E3 in CSP, see hello [Windows 10 Enterprise E3 in CSP Overview](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="95fa7-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="95fa7-111">Before you begin</span></span>

<span data-ttu-id="95fa7-112">Innan du börjar konfigurera hello automatisk registrering av Windows-domänanslutna enheter i din miljö, bör du bekanta dig med hello stöds scenarier och hello begränsningar.</span><span class="sxs-lookup"><span data-stu-id="95fa7-112">Before you start configuring hello automatic registration of Windows domain-joined devices in your environment, you should familiarize yourself with hello supported scenarios and hello constraints.</span></span>  

<span data-ttu-id="95fa7-113">tooimprove hello läsbarhet hello beskrivningar, i det här avsnittet använder hello följande villkor:</span><span class="sxs-lookup"><span data-stu-id="95fa7-113">tooimprove hello readability of hello descriptions, this topic uses hello following term:</span></span> 

- <span data-ttu-id="95fa7-114">**Aktuella Windows-enheter** -termen refererar toodomain-anslutna enheter som kör Windows 10 eller Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="95fa7-114">**Windows current devices** - This term refers toodomain-joined devices running Windows 10 or Windows Server 2016.</span></span>
- <span data-ttu-id="95fa7-115">**Windows-klientversioner enheter** -termen refererar tooall **stöds** domänanslutna Windows-enheter som inte är igång Windows 10 eller Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="95fa7-115">**Windows down-level devices** - This term refers tooall **supported** domain-joined Windows devices that are neither running Windows 10 nor Windows Server 2016.</span></span>  


### <a name="windows-current-devices"></a><span data-ttu-id="95fa7-116">Aktuella Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="95fa7-116">Windows current devices</span></span>

- <span data-ttu-id="95fa7-117">För enheter som kör hello Windows-operativsystemet, bör du använda Windows 10 årsdagar Update (version 1607) eller senare.</span><span class="sxs-lookup"><span data-stu-id="95fa7-117">For devices running hello Windows desktop operating system, we recommend using Windows 10 Anniversary Update (version 1607) or later.</span></span> 
- <span data-ttu-id="95fa7-118">Hej registrering av den aktuella Windows-enheter **är** i ofedererad miljöer, till exempel lösenord hash-synkronisering konfigurationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="95fa7-118">hello registration of Windows current devices **is** supported in non-federated environments such as password hash sync configurations.</span></span>  


### <a name="windows-down-level-devices"></a><span data-ttu-id="95fa7-119">Äldre Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="95fa7-119">Windows down-level devices</span></span>

- <span data-ttu-id="95fa7-120">hello följande äldre Windows-enheter stöds:</span><span class="sxs-lookup"><span data-stu-id="95fa7-120">hello following Windows down-level devices are supported:</span></span>
    - <span data-ttu-id="95fa7-121">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="95fa7-121">Windows 8.1</span></span>
    - <span data-ttu-id="95fa7-122">Windows 7</span><span class="sxs-lookup"><span data-stu-id="95fa7-122">Windows 7</span></span>
    - <span data-ttu-id="95fa7-123">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="95fa7-123">Windows Server 2012 R2</span></span>
    - <span data-ttu-id="95fa7-124">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="95fa7-124">Windows Server 2012</span></span>
    - <span data-ttu-id="95fa7-125">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="95fa7-125">Windows Server 2008 R2</span></span>
- <span data-ttu-id="95fa7-126">Hej registrering av Windows-klientversioner enheter **är** stöds i ofedererad miljöer genom sömlös enkel inloggning [Azure Active Directory sömlös enkel inloggning](https://aka.ms/hybrid/sso).</span><span class="sxs-lookup"><span data-stu-id="95fa7-126">hello registration of Windows down-level devices **is** supported in non-federated environments through Seamless Single Sign On [Azure Active Directory Seamless Single Sign-On](https://aka.ms/hybrid/sso).</span></span>
- <span data-ttu-id="95fa7-127">Hej registrering av Windows-klientversioner enheter **är inte** stöds för enheter med hjälp av centrala profiler.</span><span class="sxs-lookup"><span data-stu-id="95fa7-127">hello registration of Windows down-level devices **is not** supported for devices using roaming profiles.</span></span> <span data-ttu-id="95fa7-128">Om du beroende av centrala profiler eller inställningar, använder du Windows 10.</span><span class="sxs-lookup"><span data-stu-id="95fa7-128">If you are relying on roaming of profiles or settings, use Windows 10.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="95fa7-129">Krav</span><span class="sxs-lookup"><span data-stu-id="95fa7-129">Prerequisites</span></span>

<span data-ttu-id="95fa7-130">Innan du börjar att aktivera hello automatisk registrering för domänanslutna enheter i din organisation, behöver du toomake till att du kör en uppdaterad version av Azure AD connect.</span><span class="sxs-lookup"><span data-stu-id="95fa7-130">Before you start enabling hello auto-registration of domain-joined devices in your organization, you need toomake sure that you are running an up-to-date version of Azure AD connect.</span></span>

<span data-ttu-id="95fa7-131">Azure AD Connect:</span><span class="sxs-lookup"><span data-stu-id="95fa7-131">Azure AD Connect:</span></span>

- <span data-ttu-id="95fa7-132">Behåller hello associationen mellan hello datorkonto i din lokala Active Directory (AD) och hello enhetsobjekt i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95fa7-132">Keeps hello association between hello computer account in your on-premises Active Directory (AD) and hello device object in Azure AD.</span></span> 
- <span data-ttu-id="95fa7-133">Gör andra enheter relaterade funktioner som Windows Hello för företag.</span><span class="sxs-lookup"><span data-stu-id="95fa7-133">Enables other device related features like Windows Hello for Business.</span></span>



## <a name="configuration-steps"></a><span data-ttu-id="95fa7-134">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="95fa7-134">Configuration steps</span></span>

<span data-ttu-id="95fa7-135">Det här avsnittet innehåller steg hello som krävs för alla scenarier för typisk konfiguration.</span><span class="sxs-lookup"><span data-stu-id="95fa7-135">This topic includes hello required steps for all typical configuration scenarios.</span></span>  
<span data-ttu-id="95fa7-136">Använd följande tabell tooget en översikt över hello steg som krävs för ditt scenario hello:</span><span class="sxs-lookup"><span data-stu-id="95fa7-136">Use hello following table tooget an overview of hello steps that are required for your scenario:</span></span>  



| <span data-ttu-id="95fa7-137">Steg</span><span class="sxs-lookup"><span data-stu-id="95fa7-137">Steps</span></span>                                      | <span data-ttu-id="95fa7-138">Windows aktuella och lösenord hash-synkronisering</span><span class="sxs-lookup"><span data-stu-id="95fa7-138">Windows current and password hash sync</span></span> | <span data-ttu-id="95fa7-139">Windows aktuella och federation</span><span class="sxs-lookup"><span data-stu-id="95fa7-139">Windows current and federation</span></span> | <span data-ttu-id="95fa7-140">Windows-klientversioner</span><span class="sxs-lookup"><span data-stu-id="95fa7-140">Windows down-level</span></span> |
| :--                                        | :-:                                    | :-:                            | :-:                |
| <span data-ttu-id="95fa7-141">Steg 1: Konfigurera tjänstanslutningspunkten</span><span class="sxs-lookup"><span data-stu-id="95fa7-141">Step 1: Configure service connection point</span></span> | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="95fa7-145">Steg 2: Konfigurera utfärdande av anspråk</span><span class="sxs-lookup"><span data-stu-id="95fa7-145">Step 2: Setup issuance of claims</span></span>           |                                        | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="95fa7-148">Steg 3: Aktivera Windows 10-enheter</span><span class="sxs-lookup"><span data-stu-id="95fa7-148">Step 3: Enable non-Windows 10 devices</span></span>      |                                        |                                | ![Markera][1]        |
| <span data-ttu-id="95fa7-150">Steg 4: Distribution av kontrollen och distribution</span><span class="sxs-lookup"><span data-stu-id="95fa7-150">Step 4: Control deployment and rollout</span></span>     | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="95fa7-154">Steg 5: Kontrollera registrerade enheter</span><span class="sxs-lookup"><span data-stu-id="95fa7-154">Step 5: Verify registered devices</span></span>          | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |



## <a name="step-1-configure-service-connection-point"></a><span data-ttu-id="95fa7-158">Steg 1: Konfigurera tjänstanslutningspunkten</span><span class="sxs-lookup"><span data-stu-id="95fa7-158">Step 1: Configure service connection point</span></span>

<span data-ttu-id="95fa7-159">hello service anslutningsobjekt punkter (SCP) används av dina enheter under hello registrering toodiscover information för Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="95fa7-159">hello service connection point (SCP) object is used by your devices during hello registration toodiscover Azure AD tenant information.</span></span> <span data-ttu-id="95fa7-160">Hello SCP-objektet för hello automatisk registrering för domänanslutna enheter måste finnas i hello namngivning kontexten konfigurationspartitionen i hello datorns skog i din lokala Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="95fa7-160">In your on-premises Active Directory (AD), hello SCP object for hello auto-registration of domain-joined devices must exist in hello configuration naming context partition of hello computer's forest.</span></span> <span data-ttu-id="95fa7-161">Det finns endast en konfigurationsnamngivningskontexten per skog.</span><span class="sxs-lookup"><span data-stu-id="95fa7-161">There is only one configuration naming context per forest.</span></span> <span data-ttu-id="95fa7-162">Hello tjänstanslutningspunkten måste finnas i alla skogar som innehåller domänanslutna datorer i en konfiguration för flera skogar Active Directory.</span><span class="sxs-lookup"><span data-stu-id="95fa7-162">In a multi-forest Active Directory configuration, hello service connection point must exist in all forests containing domain-joined computers.</span></span>

<span data-ttu-id="95fa7-163">Du kan använda hello [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) cmdlet tooretrieve hello konfigurationsnamngivningskontexten i skogen.</span><span class="sxs-lookup"><span data-stu-id="95fa7-163">You can use hello [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet tooretrieve hello configuration naming context of your forest.</span></span>  

<span data-ttu-id="95fa7-164">För en skog med hello Active Directory-domännamn *fabrikam.com*, hello konfigurationsnamngivningskontexten är:</span><span class="sxs-lookup"><span data-stu-id="95fa7-164">For a forest with hello Active Directory domain name *fabrikam.com*, hello configuration naming context is:</span></span>

`CN=Configuration,DC=fabrikam,DC=com`

<span data-ttu-id="95fa7-165">I din skog finns hello SCP-objektet för hello automatisk registrering för domänanslutna enheter här:</span><span class="sxs-lookup"><span data-stu-id="95fa7-165">In your forest, hello SCP object for hello auto-registration of domain-joined devices is located at:</span></span>  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

<span data-ttu-id="95fa7-166">Beroende på hur du har distribuerat Azure AD Connect, har hello SCP-objektet redan konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="95fa7-166">Depending on how you have deployed Azure AD Connect, hello SCP object may have already been configured.</span></span>
<span data-ttu-id="95fa7-167">Du kan verifiera att hello objektet hello och hämta hello identifiering värden med hjälp av hello följande Windows PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="95fa7-167">You can verify hello existence of hello object and retrieve hello discovery values using hello following Windows PowerShell script:</span></span> 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

<span data-ttu-id="95fa7-168">Hej **$scp. Nyckelord** utdata visar hello Azure AD-klient information, till exempel:</span><span class="sxs-lookup"><span data-stu-id="95fa7-168">hello **$scp.Keywords** output shows hello Azure AD tenant information, for example:</span></span>

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

<span data-ttu-id="95fa7-169">Om hello tjänstanslutningspunkten inte finns, kan du skapa den genom att köra hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet på Azure AD Connect-servern.</span><span class="sxs-lookup"><span data-stu-id="95fa7-169">If hello service connection point does not exist, you can create it by running hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet on your Azure AD Connect server.</span></span> <span data-ttu-id="95fa7-170">Enterprise admin-autentiseringsuppgifter är obligatoriska toorun denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="95fa7-170">Enterprise admin credential is required toorun this cmdlet.</span></span>  
<span data-ttu-id="95fa7-171">hello-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="95fa7-171">hello cmdlet:</span></span>

- <span data-ttu-id="95fa7-172">Skapar hello tjänstanslutningspunkt i Active Directory-skog för hello Azure AD Connect är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="95fa7-172">Creates hello service connection point in hello Active Directory forest Azure AD Connect is connected to.</span></span> 
- <span data-ttu-id="95fa7-173">Kräver toospecify hello `AdConnectorAccount` parameter.</span><span class="sxs-lookup"><span data-stu-id="95fa7-173">Requires you toospecify hello `AdConnectorAccount` parameter.</span></span> <span data-ttu-id="95fa7-174">Detta är hello-konto som har konfigurerats som Active Directory connector-kontot i Azure AD connect.</span><span class="sxs-lookup"><span data-stu-id="95fa7-174">This is hello account that is configured as Active Directory connector account in Azure AD connect.</span></span> 


<span data-ttu-id="95fa7-175">hello visar följande skript ett exempel för att använda hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="95fa7-175">hello following script shows an example for using hello cmdlet.</span></span> <span data-ttu-id="95fa7-176">I det här skriptet `$aadAdminCred = Get-Credential` kräver tootype ett användarnamn.</span><span class="sxs-lookup"><span data-stu-id="95fa7-176">In this script, `$aadAdminCred = Get-Credential` requires you tootype a user name.</span></span> <span data-ttu-id="95fa7-177">Du behöver tooprovide hello användarnamnet i hello användarens huvudnamn (UPN) format (`user@example.com`).</span><span class="sxs-lookup"><span data-stu-id="95fa7-177">You need tooprovide hello user name in hello user principal name (UPN) format (`user@example.com`).</span></span> 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

<span data-ttu-id="95fa7-178">Hej `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="95fa7-178">hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span></span>

- <span data-ttu-id="95fa7-179">Använder hello Active Directory PowerShell-modul, som förlitar sig på Active Directory-webbtjänster som körs på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="95fa7-179">Uses hello Active Directory PowerShell module, which relies on Active Directory Web Services running on a domain controller.</span></span> <span data-ttu-id="95fa7-180">Active Directory Web Services fungerar på domänkontrollanter som kör Windows Server 2008 R2 och senare.</span><span class="sxs-lookup"><span data-stu-id="95fa7-180">Active Directory Web Services is supported on domain controllers running Windows Server 2008 R2 and later.</span></span>
- <span data-ttu-id="95fa7-181">Stöds endast av hello **MSOnline PowerShell Modulversion 1.1.166.0**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-181">Is only supported by hello **MSOnline PowerShell module version 1.1.166.0**.</span></span> <span data-ttu-id="95fa7-182">toodownload den här modulen används [länk](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span><span class="sxs-lookup"><span data-stu-id="95fa7-182">toodownload this module, use this [link](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span></span>   

<span data-ttu-id="95fa7-183">Använd hello skriptet nedan toocreate hello tjänstanslutningspunkten för domänkontrollanter som kör Windows Server 2008 eller tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="95fa7-183">For domain controllers running Windows Server 2008 or earlier versions, use hello script below toocreate hello service connection point.</span></span>

<span data-ttu-id="95fa7-184">Du bör använda hello följande skript toocreate hello tjänstanslutningspunkten i varje skog där det finns datorer i en konfiguration för flera skogar:</span><span class="sxs-lookup"><span data-stu-id="95fa7-184">In a multi-forest configuration, you should use hello following script toocreate hello service connection point in each forest where computers exist:</span></span>
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC

    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()


## <a name="step-2-setup-issuance-of-claims"></a><span data-ttu-id="95fa7-185">Steg 2: Konfigurera utfärdande av anspråk</span><span class="sxs-lookup"><span data-stu-id="95fa7-185">Step 2: Setup issuance of claims</span></span>

<span data-ttu-id="95fa7-186">I en federerad Azure AD-konfiguration enheter förlitar sig på Active Directory Federation Services (AD FS) eller 3 part lokalt federation service tooauthenticate tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="95fa7-186">In a federated Azure AD configuration, devices rely on Active Directory Federation Services (AD FS) or a 3rd party on-premises federation service tooauthenticate tooAzure AD.</span></span> <span data-ttu-id="95fa7-187">Enheter autentiserar tooget en åtkomst-token tooregister mot hello Azure Active Directory Device Registration Service (DRS Azure).</span><span class="sxs-lookup"><span data-stu-id="95fa7-187">Devices authenticate tooget an access token tooregister against hello Azure Active Directory Device Registration Service (Azure DRS).</span></span>

<span data-ttu-id="95fa7-188">Windows aktuella enheter autentiseras med integrerad Windows-autentisering tooan aktiv WS-Trust slutpunkt (1.3 eller 2005 versioner) hos hello lokala federationstjänsten.</span><span class="sxs-lookup"><span data-stu-id="95fa7-188">Windows current devices authenticate using Integrated Windows Authentication tooan active WS-Trust endpoint (either 1.3 or 2005 versions) hosted by hello on-premises federation service.</span></span>

> [!NOTE]
> <span data-ttu-id="95fa7-189">När du använder AD FS, antingen **adfs/services/trust/13/windowstransport** eller **adfs/services/trust/2005/windowstransport** måste vara aktiverat.</span><span class="sxs-lookup"><span data-stu-id="95fa7-189">When using AD FS, either **adfs/services/trust/13/windowstransport** or **adfs/services/trust/2005/windowstransport** must be enabled.</span></span> <span data-ttu-id="95fa7-190">Om du använder hello webbproxy för autentisering kan också se till att den här slutpunkten har publicerats via hello proxy.</span><span class="sxs-lookup"><span data-stu-id="95fa7-190">If you are using hello Web Authentication Proxy, also ensure that this endpoint is published through hello proxy.</span></span> <span data-ttu-id="95fa7-191">Du kan se vilka slutpunkter aktiveras via hello AD FS-hanteringskonsol under **Service > slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-191">You can see what end-points are enabled through hello AD FS management console under **Service > Endpoints**.</span></span>
>
><span data-ttu-id="95fa7-192">Om du inte har AD FS som federationstjänsten lokalt, följ hello instruktionerna för din leverantör toomake att de stöder WS-Trust 1.3 eller 2005 slutpunkter och dessa publiceras via hello Metadata Exchange-fil (MEX).</span><span class="sxs-lookup"><span data-stu-id="95fa7-192">If you don’t have AD FS as your on-premises federation service, follow hello instructions of your vendor toomake sure they support WS-Trust 1.3 or 2005 end-points and that these are published through hello Metadata Exchange file (MEX).</span></span>

<span data-ttu-id="95fa7-193">hello måste följande anspråk finnas i hello-token som tas emot av Azure DRS för toocomplete för registrering av enheten.</span><span class="sxs-lookup"><span data-stu-id="95fa7-193">hello following claims must exist in hello token received by Azure DRS for device registration toocomplete.</span></span> <span data-ttu-id="95fa7-194">Azure DRS skapar ett enhetsobjekt i Azure AD med en del av den här informationen som sedan används av Azure AD Connect tooassociate hello nyskapad enhetsobjekt med hello datorn kontot lokalt.</span><span class="sxs-lookup"><span data-stu-id="95fa7-194">Azure DRS will create a device object in Azure AD with some of this information which is then used by Azure AD Connect tooassociate hello newly created device object with hello computer account on-premises.</span></span>

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

<span data-ttu-id="95fa7-195">Om du har mer än ett verifierat domännamn måste tooprovide hello följande anspråk för datorer:</span><span class="sxs-lookup"><span data-stu-id="95fa7-195">If you have more than one verified domain name, you need tooprovide hello following claim for computers:</span></span>

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

<span data-ttu-id="95fa7-196">Om du redan utfärdar ett ImmutableID anspråk (t.ex. Alternativt inloggnings-ID) måste tooprovide ett motsvarande anspråk för datorer:</span><span class="sxs-lookup"><span data-stu-id="95fa7-196">If you are already issuing an ImmutableID claim (e.g., alternate login ID) you need tooprovide one corresponding claim for computers:</span></span>

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

<span data-ttu-id="95fa7-197">I följande avsnitt hello, hittar du information om:</span><span class="sxs-lookup"><span data-stu-id="95fa7-197">In hello following sections, you find information about:</span></span>
 
- <span data-ttu-id="95fa7-198">hello värden varje anspråk ska ha</span><span class="sxs-lookup"><span data-stu-id="95fa7-198">hello values each claim should have</span></span>
- <span data-ttu-id="95fa7-199">Hur en definition skulle se ut i AD FS</span><span class="sxs-lookup"><span data-stu-id="95fa7-199">How a definition would look like in AD FS</span></span>

<span data-ttu-id="95fa7-200">hello definition hjälper dig att tooverify om hello värden finns eller om du behöver toocreate dem.</span><span class="sxs-lookup"><span data-stu-id="95fa7-200">hello definition helps you tooverify whether hello values are present or if you need toocreate them.</span></span>

> [!NOTE]
> <span data-ttu-id="95fa7-201">Om du inte använder AD FS för federationsservern lokalt, följer du leverantörens instruktioner toocreate hello lämplig konfiguration tooissue dessa anspråk.</span><span class="sxs-lookup"><span data-stu-id="95fa7-201">If you don’t use AD FS for your on-premises federation server, follow your vendor's instructions toocreate hello appropriate configuration tooissue these claims.</span></span>

### <a name="issue-account-type-claim"></a><span data-ttu-id="95fa7-202">Problemet konto typen anspråk</span><span class="sxs-lookup"><span data-stu-id="95fa7-202">Issue account type claim</span></span>

<span data-ttu-id="95fa7-203">**`http://schemas.microsoft.com/ws/2012/01/accounttype`**-Detta anspråk måste innehålla ett värde av **DJ**, som identifierar hello enheten som en domänansluten dator.</span><span class="sxs-lookup"><span data-stu-id="95fa7-203">**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - This claim must contain a value of **DJ**, which identifies hello device as a domain-joined computer.</span></span> <span data-ttu-id="95fa7-204">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="95fa7-204">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-hello-computer-account-on-premises"></a><span data-ttu-id="95fa7-205">Utfärda objectGUID av hello datorn kontot Lokal</span><span class="sxs-lookup"><span data-stu-id="95fa7-205">Issue objectGUID of hello computer account on-premises</span></span>

<span data-ttu-id="95fa7-206">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**-Detta anspråk måste innehålla hello **objectGUID** värdet för hello lokalt datorkonto.</span><span class="sxs-lookup"><span data-stu-id="95fa7-206">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - This claim must contain hello **objectGUID** value of hello on-premises computer account.</span></span> <span data-ttu-id="95fa7-207">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="95fa7-207">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-hello-computer-account-on-premises"></a><span data-ttu-id="95fa7-208">Utfärda objectSID av hello datorn kontot Lokal</span><span class="sxs-lookup"><span data-stu-id="95fa7-208">Issue objectSID of hello computer account on-premises</span></span>

<span data-ttu-id="95fa7-209">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**-Detta anspråk måste innehålla hello hello **objectSid** värdet för hello lokalt datorkonto.</span><span class="sxs-lookup"><span data-stu-id="95fa7-209">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - This claim must contain hello hello **objectSid** value of hello on-premises computer account.</span></span> <span data-ttu-id="95fa7-210">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="95fa7-210">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a><span data-ttu-id="95fa7-211">Utfärda issuerID för datorn när flera verifierat domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="95fa7-211">Issue issuerID for computer when multiple verified domain names in Azure AD</span></span>

<span data-ttu-id="95fa7-212">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**-Detta anspråk måste innehålla hello identifierare URI (Uniform Resource) för någon av hello verifierat domännamn som ansluter med hello lokalt federation (AD FS eller 3 part) utfärdande hello tjänsttoken.</span><span class="sxs-lookup"><span data-stu-id="95fa7-212">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - This claim must contain hello Uniform Resource Identifier (URI) of any of hello verified domain names that connect with hello on-premises federation service (AD FS or 3rd party) issuing hello token.</span></span> <span data-ttu-id="95fa7-213">I AD FS kan du lägga till regler för utfärdandetransformering som ser ut som om hello viktiga nedan i den specifika ordning efter hello de som ovan.</span><span class="sxs-lookup"><span data-stu-id="95fa7-213">In AD FS, you can add issuance transform rules that look like hello ones below in that specific order after hello ones above.</span></span> <span data-ttu-id="95fa7-214">Observera att en regel tooexplicitly problemet hello regel för användare som är nödvändiga.</span><span class="sxs-lookup"><span data-stu-id="95fa7-214">Please note that one rule tooexplicitly issue hello rule for users is necessary.</span></span> <span data-ttu-id="95fa7-215">En första regel som identifierar användare eller datorautentisering läggs till i hello reglerna nedan.</span><span class="sxs-lookup"><span data-stu-id="95fa7-215">In hello rules below, a first rule identifying user vs. computer authentication is added.</span></span>

    @RuleName = "Issue account type with hello value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


<span data-ttu-id="95fa7-216">I hello anspråket ovan</span><span class="sxs-lookup"><span data-stu-id="95fa7-216">In hello claim above,</span></span>

- <span data-ttu-id="95fa7-217">`$<domain>`är hello AD FS-tjänstens URL</span><span class="sxs-lookup"><span data-stu-id="95fa7-217">`$<domain>` is hello AD FS service URL</span></span>
- <span data-ttu-id="95fa7-218">`<verified-domain-name>`en platshållare som du behöver tooreplace med en av dina verifierat domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="95fa7-218">`<verified-domain-name>` is a placeholder you need tooreplace with one of your verified domain names in Azure AD</span></span>



<span data-ttu-id="95fa7-219">Mer information om verifierat domännamn finns [lägga till en anpassad domän namn tooAzure Active Directory](active-directory-add-domain.md).</span><span class="sxs-lookup"><span data-stu-id="95fa7-219">For more details about verified domain names, see [Add a custom domain name tooAzure Active Directory](active-directory-add-domain.md).</span></span>  
<span data-ttu-id="95fa7-220">tooget en lista över verifierade företagets-domäner som du kan använda hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="95fa7-220">tooget a list of your verified company domains, you can use hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span></span> 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a><span data-ttu-id="95fa7-222">Utfärda ImmutableID för datorn när en användare finns (t.ex. Alternativt inloggnings-ID har angetts)</span><span class="sxs-lookup"><span data-stu-id="95fa7-222">Issue ImmutableID for computer when one for users exist (e.g. alternate login ID is set)</span></span>

<span data-ttu-id="95fa7-223">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**-Detta anspråk måste innehålla ett giltigt värde för datorer.</span><span class="sxs-lookup"><span data-stu-id="95fa7-223">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - This claim must contain a valid value for computers.</span></span> <span data-ttu-id="95fa7-224">I AD FS kan du skapa en regel för omvandling av utfärdande som följer:</span><span class="sxs-lookup"><span data-stu-id="95fa7-224">In AD FS, you can create an issuance transform rule as follows:</span></span>

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-toocreate-hello-ad-fs-issuance-transform-rules"></a><span data-ttu-id="95fa7-225">Helper skriptet toocreate hello AD FS regler för utfärdandetransformering</span><span class="sxs-lookup"><span data-stu-id="95fa7-225">Helper script toocreate hello AD FS issuance transform rules</span></span>

<span data-ttu-id="95fa7-226">hello hjälper följande skript dig med hello utfärdande hello skapa regler som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="95fa7-226">hello following script helps you with hello creation of hello issuance transform rules described above.</span></span>

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with hello value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a><span data-ttu-id="95fa7-227">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="95fa7-227">Remarks</span></span> 

- <span data-ttu-id="95fa7-228">Skriptet lägger till hello toohello befintliga regler.</span><span class="sxs-lookup"><span data-stu-id="95fa7-228">This script appends hello rules toohello existing rules.</span></span> <span data-ttu-id="95fa7-229">Körs inte hello skriptet skulle två gånger eftersom hello uppsättning regler läggas till två gånger.</span><span class="sxs-lookup"><span data-stu-id="95fa7-229">Do not run hello script twice because hello set of rules would be added twice.</span></span> <span data-ttu-id="95fa7-230">Se till att motsvarande inte finns några regler för dessa anspråk (under hello motsvarande) innan du kör skriptet hello igen.</span><span class="sxs-lookup"><span data-stu-id="95fa7-230">Make sure that no corresponding rules exist for these claims (under hello corresponding conditions) before running hello script again.</span></span>

- <span data-ttu-id="95fa7-231">Om du har flera verifierat domännamn (som visas i hello Azure AD-portalen eller via hello Get-MsolDomains cmdlet), ange hello-värde **$multipleVerifiedDomainNames** i hello skript för**$true**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-231">If you have multiple verified domain names (as shown in hello Azure AD portal or via hello Get-MsolDomains cmdlet), set hello value of **$multipleVerifiedDomainNames** in hello script too**$true**.</span></span> <span data-ttu-id="95fa7-232">Kontrollera också att du tar bort alla befintliga issuerid anspråk som kan ha skapats av Azure AD Connect eller via andra sätt.</span><span class="sxs-lookup"><span data-stu-id="95fa7-232">Also make sure that you remove any existing issuerid claim that might have been created by Azure AD Connect or via other means.</span></span> <span data-ttu-id="95fa7-233">Här är ett exempel på den här regeln:</span><span class="sxs-lookup"><span data-stu-id="95fa7-233">Here is an example for this rule:</span></span>


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- <span data-ttu-id="95fa7-234">Om du redan har skickat en **ImmutableID** anspråk för användarkonton genom att ange hello värdet för **$immutableIDAlreadyIssuedforUsers** i hello skript för**$true**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-234">If you have already issued an **ImmutableID** claim  for user accounts, set hello value of **$immutableIDAlreadyIssuedforUsers** in hello script too**$true**.</span></span>

## <a name="step-3-enable-windows-down-level-devices"></a><span data-ttu-id="95fa7-235">Steg 3: Aktivera äldre Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="95fa7-235">Step 3: Enable Windows down-level devices</span></span>

<span data-ttu-id="95fa7-236">Om vissa enheter domänanslutna Windows äldre enheter, måste du:</span><span class="sxs-lookup"><span data-stu-id="95fa7-236">If some of your domain-joined devices Windows down-level devices, you need to:</span></span>

- <span data-ttu-id="95fa7-237">Ange en princip i Azure AD tooenable användare tooregister enheter.</span><span class="sxs-lookup"><span data-stu-id="95fa7-237">Set a policy in Azure AD tooenable users tooregister devices.</span></span>
 
- <span data-ttu-id="95fa7-238">Konfigurera din lokala federation service tooissue anspråk toosupport **Windows IWA (Integrated Authentication)** för registrering av enheten.</span><span class="sxs-lookup"><span data-stu-id="95fa7-238">Configure your on-premises federation service tooissue claims toosupport **Integrated Windows Authentication (IWA)** for device registration.</span></span>
 
- <span data-ttu-id="95fa7-239">Lägg till hello Azure AD enheten autentisering slutpunkt toohello lokalt intranät zoner tooavoid certifikat uppmanar vid autentisering hello enhet.</span><span class="sxs-lookup"><span data-stu-id="95fa7-239">Add hello Azure AD device authentication end-point toohello local Intranet zones tooavoid certificate prompts when authenticating hello device.</span></span>

### <a name="set-policy-in-azure-ad-tooenable-users-tooregister-devices"></a><span data-ttu-id="95fa7-240">Ange princip för i Azure AD tooenable användare tooregister enheter</span><span class="sxs-lookup"><span data-stu-id="95fa7-240">Set policy in Azure AD tooenable users tooregister devices</span></span>

<span data-ttu-id="95fa7-241">tooregister Windows äldre enheter behöver du toomake till att hello inställningen tooallow användare tooregister enheter i Azure AD har angetts.</span><span class="sxs-lookup"><span data-stu-id="95fa7-241">tooregister Windows down-level devices, you need toomake sure that hello setting tooallow users tooregister devices in Azure AD is set.</span></span> <span data-ttu-id="95fa7-242">Du hittar den här inställningen under i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="95fa7-242">In hello Azure portal, you can find this setting under:</span></span>

`Azure Active Directory > Users and groups > Device settings`
    
<span data-ttu-id="95fa7-243">hello följande princip måste anges för**alla**: **användarna kan registrera sina enheter med Azure AD**</span><span class="sxs-lookup"><span data-stu-id="95fa7-243">hello following policy must be set too**All**: **Users may register their devices with Azure AD**</span></span>

![Registrera enheter](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a><span data-ttu-id="95fa7-245">Konfigurera lokala federationstjänsten</span><span class="sxs-lookup"><span data-stu-id="95fa7-245">Configure on-premises federation service</span></span> 

<span data-ttu-id="95fa7-246">Federationstjänsten lokalt måste ha stöd för utfärdande hello **authenticationmehod** och **wiaormultiauthn** anspråk när du tar emot en autentisering begära toohello Azure AD förlitande part innehar en resouce_params parameter med ett kodat värde som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="95fa7-246">Your on-premises federation service must support issuing hello **authenticationmehod** and **wiaormultiauthn** claims when receiving an authentication request toohello Azure AD relying party holding a resouce_params parameter with an encoded value as shown below:</span></span>

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

<span data-ttu-id="95fa7-247">När en sådan begäran kommer hello lokalt federationstjänsten måste autentisera hello-användare som använder integrerad Windows-autentisering och vid lyckades, utfärda hello följande två anspråk:</span><span class="sxs-lookup"><span data-stu-id="95fa7-247">When such a request comes, hello on-premises federation service must authenticate hello user using Integrated Windows Authentication and upon success, it must issue hello following two claims:</span></span>

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

<span data-ttu-id="95fa7-248">I AD FS, du måste lägga till en regel för omvandling av utfärdande som överför via hello autentiseringsmetod.</span><span class="sxs-lookup"><span data-stu-id="95fa7-248">In AD FS, you must add an issuance transform rule that passes-through hello authentication method.</span></span>  

<span data-ttu-id="95fa7-249">**tooadd regeln:**</span><span class="sxs-lookup"><span data-stu-id="95fa7-249">**tooadd this rule:**</span></span>

1. <span data-ttu-id="95fa7-250">I hello AD FS-hanteringskonsolen gå för`AD FS > Trust Relationships > Relying Party Trusts`.</span><span class="sxs-lookup"><span data-stu-id="95fa7-250">In hello AD FS management console, go too`AD FS > Trust Relationships > Relying Party Trusts`.</span></span>
2. <span data-ttu-id="95fa7-251">Högerklicka på hello Identitetsplattformen för Microsoft Office 365 förlitande part förtroende-objekt och välj sedan **redigera Anspråksregler**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-251">Right-click hello Microsoft Office 365 Identity Platform relying party trust object, and then select **Edit Claim Rules**.</span></span>
3. <span data-ttu-id="95fa7-252">På hello **regler för Utfärdandetransformering** väljer **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-252">On hello **Issuance Transform Rules** tab, select **Add Rule**.</span></span>
4. <span data-ttu-id="95fa7-253">I hello **anspråksregel** mall-listan, Välj **skicka anspråk med en anpassad regel**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-253">In hello **Claim rule** template list, select **Send Claims Using a Custom Rule**.</span></span>
5. <span data-ttu-id="95fa7-254">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-254">Select **Next**.</span></span>
6. <span data-ttu-id="95fa7-255">I hello **Regelnamn för anspråk** skriver **Auth metoden Anspråksregel**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-255">In hello **Claim rule name** box, type **Auth Method Claim Rule**.</span></span>
7. <span data-ttu-id="95fa7-256">I hello **anspråksregel** rutan, typen hello följande regel:</span><span class="sxs-lookup"><span data-stu-id="95fa7-256">In hello **Claim rule** box, type hello following rule:</span></span>

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. <span data-ttu-id="95fa7-257">Skriv hello PowerShell-kommandot nedan på federationsservern när du ersätter  **\<RPObjectName\>**  med hello förlitande part objektnamn för din Azure AD förlitande part förtroende-objekt.</span><span class="sxs-lookup"><span data-stu-id="95fa7-257">On your federation server, type hello PowerShell command below after replacing **\<RPObjectName\>** with hello relying party object name for your Azure AD relying party trust object.</span></span> <span data-ttu-id="95fa7-258">Det här objektet vanligen namnet **Identitetsplattformen för Microsoft Office 365**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-258">This object usually is named **Microsoft Office 365 Identity Platform**.</span></span>
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-hello-azure-ad-device-authentication-end-point-toohello-local-intranet-zones"></a><span data-ttu-id="95fa7-259">Lägg till hello Azure AD device autentisering slutpunkt toohello lokalt intranät zoner</span><span class="sxs-lookup"><span data-stu-id="95fa7-259">Add hello Azure AD device authentication end-point toohello Local Intranet zones</span></span>

<span data-ttu-id="95fa7-260">tooavoid certifikat uppmanar när användare registrera enheter autentiseras tooAzure AD du skicka en princip tooyour domänanslutna enheter tooadd hello följande URL toohello lokalt intranät i Internet Explorer:</span><span class="sxs-lookup"><span data-stu-id="95fa7-260">tooavoid certificate prompts when users in register devices authenticate tooAzure AD you can push a policy tooyour domain-joined devices tooadd hello following URL toohello Local Intranet zone in Internet Explorer:</span></span>

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a><span data-ttu-id="95fa7-261">Steg 4: Distribution av kontrollen och distribution</span><span class="sxs-lookup"><span data-stu-id="95fa7-261">Step 4: Control deployment and rollout</span></span>

<span data-ttu-id="95fa7-262">När du har slutfört steg hello krävs är domänanslutna enheter klar tooautomatically registrera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95fa7-262">When you have completed hello required steps, domain-joined devices are ready tooautomatically register with Azure AD.</span></span> <span data-ttu-id="95fa7-263">Alla domänanslutna enheter som kör Windows 10 årsdagar Update och Windows Server 2016 registrera med Azure AD på enheten startas om automatiskt eller användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="95fa7-263">All domain-joined devices running Windows 10 Anniversary Update and Windows Server 2016 automatically register with Azure AD at device restart or user sign-in.</span></span> <span data-ttu-id="95fa7-264">Att registrera nya enheter med Azure AD när hello enheten startas om efter hello domän join-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="95fa7-264">New devices register with Azure AD when hello device restarts after hello domain join operation is completed.</span></span>

<span data-ttu-id="95fa7-265">Enheter som fanns tidigare arbetsplatsanslutna tooAzure AD (till exempel för Intune) övergång för ”*domänanslutna, AAD registrerade*”; men tar det lite tid för den här processen toocomplete på alla enheter på grund av toohello normal flödet av domän- och användaraktivitet.</span><span class="sxs-lookup"><span data-stu-id="95fa7-265">Devices that were previously workplace-joined tooAzure AD (for example for Intune) transition too“*Domain Joined, AAD Registered*”; however it takes some time for this process toocomplete across all devices due toohello normal flow of domain and user activity.</span></span>

### <a name="remarks"></a><span data-ttu-id="95fa7-266">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="95fa7-266">Remarks</span></span>

- <span data-ttu-id="95fa7-267">Du kan använda en Grupprincip objektet toocontrol hello distribution av automatisk registrering av Windows 10 och Windows Server 2016 domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="95fa7-267">You can use a Group Policy object toocontrol hello rollout of automatic registration of Windows 10 and Windows Server 2016 domain-joined computers.</span></span>

- <span data-ttu-id="95fa7-268">Windows 10 November 2015 Update automatiskt registrerar med Azure AD **endast** om hello grupprincipobjektet för distribution har angetts.</span><span class="sxs-lookup"><span data-stu-id="95fa7-268">Windows 10 November 2015 Update automatically registers with Azure AD **only** if hello rollout Group Policy object is set.</span></span>

- <span data-ttu-id="95fa7-269">toorollout hello automatisk registrering av äldre Windows-datorer, kan du distribuera en [Windows Installer-paketet](#windows-installer-packages-for-non-windows-10-computers) toocomputers som du väljer.</span><span class="sxs-lookup"><span data-stu-id="95fa7-269">toorollout hello automatic registration of Windows down-level computers, you can deploy a [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) toocomputers that you select.</span></span>

- <span data-ttu-id="95fa7-270">Om du överför hello gruppolicy objekt tooWindows 8.1 domänanslutna enheter görs registrering; men det rekommenderas att du använder hello [Windows Installer-paketet](#windows-installer-packages-for-non-windows-10-computers) tooregister alla dina Windows äldre enheter.</span><span class="sxs-lookup"><span data-stu-id="95fa7-270">If you push hello Group Policy object tooWindows 8.1 domain-joined devices, registration will be attempted; however it is recommended that you use hello [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) tooregister all your Windows down-level devices.</span></span> 

### <a name="create-a-group-policy-object"></a><span data-ttu-id="95fa7-271">Skapa ett grupprincipobjekt</span><span class="sxs-lookup"><span data-stu-id="95fa7-271">Create a Group Policy object</span></span> 

<span data-ttu-id="95fa7-272">toocontrol hello distributionen av automatisk registrering av aktuella Windows-datorer, bör du distribuera hello **registrera domänanslutna datorer som enheter** gruppolicy objekt toohello enheter du vill tooregister.</span><span class="sxs-lookup"><span data-stu-id="95fa7-272">toocontrol hello rollout of automatic registration of Windows current computers, you should deploy hello **Register domain-joined computers as devices** Group Policy object toohello devices you want tooregister.</span></span> <span data-ttu-id="95fa7-273">Du kan till exempel distribuera princip hello tooan organisationsenhet eller tooa säkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="95fa7-273">For example, you can deploy hello policy tooan organizational unit or tooa security group.</span></span>

<span data-ttu-id="95fa7-274">**tooset hello princip:**</span><span class="sxs-lookup"><span data-stu-id="95fa7-274">**tooset hello policy:**</span></span>

1. <span data-ttu-id="95fa7-275">Öppna **Serverhanteraren**, och sedan gå för`Tools > Group Policy Management`.</span><span class="sxs-lookup"><span data-stu-id="95fa7-275">Open **Server Manager**, and then go too`Tools > Group Policy Management`.</span></span>
2. <span data-ttu-id="95fa7-276">Gå toohello domännod som motsvarar toohello domänen där du vill tooactivate automatisk registrering av aktuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="95fa7-276">Go toohello domain node that corresponds toohello domain where you want tooactivate auto-registration of Windows current computers.</span></span>
3. <span data-ttu-id="95fa7-277">Högerklicka på **grupprincipobjekt**, och välj sedan **ny**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-277">Right-click **Group Policy Objects**, and then select **New**.</span></span>
4. <span data-ttu-id="95fa7-278">Ange ett namn för grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="95fa7-278">Type a name for your Group Policy object.</span></span> <span data-ttu-id="95fa7-279">Till exempel *automatisk registrering tooAzure AD*.</span><span class="sxs-lookup"><span data-stu-id="95fa7-279">For example, *Automatic Registration tooAzure AD*.</span></span> <span data-ttu-id="95fa7-280">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-280">Select **OK**.</span></span>
5. <span data-ttu-id="95fa7-281">Högerklicka på din nya grupprincipobjekt och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-281">Right-click your new Group Policy object, and then select **Edit**.</span></span>
6. <span data-ttu-id="95fa7-282">Gå för**Datorkonfiguration** > **principer** > **Administrationsmallar** > **Windows Komponenter** > **Enhetsregistrering**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-282">Go too**Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Device Registration**.</span></span> <span data-ttu-id="95fa7-283">Högerklicka på **registrera domänanslutna datorer som enheter**, och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-283">Right-click **Register domain-joined computers as devices**, and then select **Edit**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="95fa7-284">Den här mallen har ändrats från tidigare versioner av hello hanteringskonsolen för Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="95fa7-284">This Group Policy template has been renamed from earlier versions of hello Group Policy Management console.</span></span> <span data-ttu-id="95fa7-285">Om du använder en tidigare version av hello-konsolen, gå för`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span><span class="sxs-lookup"><span data-stu-id="95fa7-285">If you are using an earlier version of hello console, go too`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span></span> 

7. <span data-ttu-id="95fa7-286">Välj **aktiverad**, och välj sedan **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-286">Select **Enabled**, and then select **Apply**.</span></span>
8. <span data-ttu-id="95fa7-287">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-287">Select **OK**.</span></span>
9. <span data-ttu-id="95fa7-288">Länken hello gruppolicy objekt tooa önskad plats.</span><span class="sxs-lookup"><span data-stu-id="95fa7-288">Link hello Group Policy object tooa location of your choice.</span></span> <span data-ttu-id="95fa7-289">T.ex, kan du länka den tooa organisationsenheten.</span><span class="sxs-lookup"><span data-stu-id="95fa7-289">For example, you can link it tooa specific organizational unit.</span></span> <span data-ttu-id="95fa7-290">Dessutom kan du länka det tooa säkerhetsgrupp för datorer som registreras automatiskt med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95fa7-290">You also could link it tooa specific security group of computers that automatically register with Azure AD.</span></span> <span data-ttu-id="95fa7-291">tooset principen för alla domänanslutna Windows 10 och Windows Server 2016 datorer i din organisation, länk hello gruppolicy objekt toohello domän.</span><span class="sxs-lookup"><span data-stu-id="95fa7-291">tooset this policy for all domain-joined Windows 10 and Windows Server 2016 computers in your organization, link hello Group Policy object toohello domain.</span></span>

### <a name="windows-installer-packages-for-non-windows-10-computers"></a><span data-ttu-id="95fa7-292">Windows Installer-paket för Windows 10-datorer</span><span class="sxs-lookup"><span data-stu-id="95fa7-292">Windows Installer packages for non-Windows 10 computers</span></span>

<span data-ttu-id="95fa7-293">tooregister domänanslutna Windows-klientversioner datorer i en federerad miljö kan du hämta och installera de här Windows Installer-paketet (.msi) från Download Center på hello [Microsoft till arbetsplatsen för Windows 10 - datorer](https://www.microsoft.com/en-us/download/details.aspx?id=53554) sidan.</span><span class="sxs-lookup"><span data-stu-id="95fa7-293">tooregister domain-joined Windows down-level computers in a federated environment, you can download and install these Windows Installer package (.msi) from Download Center at hello [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) page.</span></span>

<span data-ttu-id="95fa7-294">Du kan distribuera hello paketet med hjälp av ett program distributionssystem som System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="95fa7-294">You can deploy hello package by using a software distribution system like System Center Configuration Manager.</span></span> <span data-ttu-id="95fa7-295">hello paketet stöder hello standard tyst installation alternativ med hello *tyst* parameter.</span><span class="sxs-lookup"><span data-stu-id="95fa7-295">hello package supports hello standard silent install options with hello *quiet* parameter.</span></span> <span data-ttu-id="95fa7-296">System Center Configuration Manager Current Branch erbjuder ytterligare fördelar jämfört med tidigare versioner som hello möjlighet tootrack slutförts registreringar.</span><span class="sxs-lookup"><span data-stu-id="95fa7-296">System Center Configuration Manager Current Branch offers additional benefits from earlier versions, like hello ability tootrack completed registrations.</span></span> <span data-ttu-id="95fa7-297">Mer information finns i [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="95fa7-297">For more information, see [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span></span>

<span data-ttu-id="95fa7-298">hello installationsprogrammet skapar en schemalagd aktivitet på hello-system som körs i hello användarens kontext.</span><span class="sxs-lookup"><span data-stu-id="95fa7-298">hello installer creates a scheduled task on hello system that runs in hello user’s context.</span></span> <span data-ttu-id="95fa7-299">hello aktiviteten utlöses när hello användare loggar in tooWindows.</span><span class="sxs-lookup"><span data-stu-id="95fa7-299">hello task is triggered when hello user signs in tooWindows.</span></span> <span data-ttu-id="95fa7-300">hello uppgiften registrerar tyst hello enhet med Azure AD med hello autentiseringsuppgifter efter autentiseras med integrerad Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="95fa7-300">hello task silently registers hello device with Azure AD with hello user credentials after authenticating using Integrated Windows Authentication.</span></span> <span data-ttu-id="95fa7-301">toosee hello schemalagd aktivitet, i hello enhet, gå för**Microsoft** > **Arbetsplatsanslutning**, och sedan gå toohello Schemaläggaren bibliotek.</span><span class="sxs-lookup"><span data-stu-id="95fa7-301">toosee hello scheduled task, in hello device, go too**Microsoft** > **Workplace Join**, and then go toohello Task Scheduler library.</span></span>

## <a name="step-5-verify-registered-devices"></a><span data-ttu-id="95fa7-302">Steg 5: Kontrollera registrerade enheter</span><span class="sxs-lookup"><span data-stu-id="95fa7-302">Step 5: Verify registered devices</span></span>

<span data-ttu-id="95fa7-303">Du kan kontrollera lyckade registrerade enheter i organisationen med hjälp av hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet i hello [Azure Active Directory PowerShell-modulen](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="95fa7-303">You can check successful registered devices in your organization by using hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in hello [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span>

<span data-ttu-id="95fa7-304">hello utdata från den här cmdleten visar enheter som har registrerats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95fa7-304">hello output of this cmdlet shows devices registered in Azure AD.</span></span> <span data-ttu-id="95fa7-305">tooget alla enheter använda hello **-alla** parameter och filtrera dem med hjälp av hello **deviceTrustType** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="95fa7-305">tooget all devices, use hello **-All** parameter, and then filter them using hello **deviceTrustType** property.</span></span> <span data-ttu-id="95fa7-306">Domänanslutna enheter har värdet **domänanslutna**.</span><span class="sxs-lookup"><span data-stu-id="95fa7-306">Domain joined devices have a value of **Domain Joined**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95fa7-307">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95fa7-307">Next steps</span></span>

* [<span data-ttu-id="95fa7-308">Automatisk enhetsregistrering vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="95fa7-308">Automatic device registration FAQ</span></span>](active-directory-device-registration-faq.md)
* [<span data-ttu-id="95fa7-309">Felsökning av automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10 och Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="95fa7-309">Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)
* [<span data-ttu-id="95fa7-310">Felsökning av automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10</span><span class="sxs-lookup"><span data-stu-id="95fa7-310">Troubleshooting auto-registration of domain joined computers tooAzure AD – non-Windows 10</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
* [<span data-ttu-id="95fa7-311">Villkorlig åtkomst i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95fa7-311">Azure Active Directory conditional access</span></span>](active-directory-conditional-access-azure-portal.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
