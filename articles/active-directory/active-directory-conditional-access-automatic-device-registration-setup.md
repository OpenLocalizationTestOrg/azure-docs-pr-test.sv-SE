---
title: "Konfigurera automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory | Microsoft Docs"
description: "Konfigurera dina Windows-domänanslutna enheter att registrera automatiskt med Azure Active Directory."
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
ms.openlocfilehash: dccd7df6a5f85df4179c7ea7cfc476cfb57f48c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a><span data-ttu-id="57d02-103">Konfigurera automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="57d02-103">How to configure automatic registration of Windows domain-joined devices with Azure Active Directory</span></span>

<span data-ttu-id="57d02-104">Att använda [Azure Active Directory enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-azure-portal.md), dina datorer måste vara registrerad med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="57d02-104">To use [Azure Active Directory device-based conditional access](active-directory-conditional-access-azure-portal.md), your computers must be registered with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="57d02-105">Du kan hämta en lista över registrerade enheter i organisationen med hjälp av den [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet i den [Azure Active Directory PowerShell-modulen](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="57d02-105">You can get a list of registered devices in your organization by using the [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in the [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span> 

<span data-ttu-id="57d02-106">Den här artikeln innehåller stegen för att konfigurera automatisk registrering av Windows-domänanslutna enheter med Azure AD i din organisation.</span><span class="sxs-lookup"><span data-stu-id="57d02-106">This article provides you with the steps for configuring the automatic registration of Windows domain-joined devices with Azure AD in your organization.</span></span>


<span data-ttu-id="57d02-107">Mer information om:</span><span class="sxs-lookup"><span data-stu-id="57d02-107">For more information about:</span></span>

- <span data-ttu-id="57d02-108">Villkorlig åtkomst finns [Azure Active Directory enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="57d02-108">Conditional access, see [Azure Active Directory device-based conditional access](active-directory-conditional-access-azure-portal.md).</span></span> 
- <span data-ttu-id="57d02-109">Windows 10-enheter till arbetsplatsen och förbättrad upplevelse när registrerad med Azure AD finns [Windows 10 för företaget: Använd enheter för arbetet](active-directory-azureadjoin-windows10-devices-overview.md).</span><span class="sxs-lookup"><span data-stu-id="57d02-109">Windows 10 devices in the workplace and the enhanced experiences when registered with Azure AD, see [Windows 10 for the enterprise: Use devices for work](active-directory-azureadjoin-windows10-devices-overview.md).</span></span>
- <span data-ttu-id="57d02-110">Windows 10 Enterprise E3 i CSP, finns det [Windows 10 Enterprise E3 i CSP översikt](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).</span><span class="sxs-lookup"><span data-stu-id="57d02-110">Windows 10 Enterprise E3 in CSP, see the [Windows 10 Enterprise E3 in CSP Overview](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="57d02-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="57d02-111">Before you begin</span></span>

<span data-ttu-id="57d02-112">Innan du börjar konfigurera automatisk registrering av Windows-domänanslutna enheter i din miljö, bör du bekanta dig med scenarierna som stöds och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="57d02-112">Before you start configuring the automatic registration of Windows domain-joined devices in your environment, you should familiarize yourself with the supported scenarios and the constraints.</span></span>  

<span data-ttu-id="57d02-113">För att förbättra läsbarhet beskrivningar, används det här avsnittet följande villkor:</span><span class="sxs-lookup"><span data-stu-id="57d02-113">To improve the readability of the descriptions, this topic uses the following term:</span></span> 

- <span data-ttu-id="57d02-114">**Aktuella Windows-enheter** -termen refererar till domänanslutna enheter som kör Windows 10 eller Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="57d02-114">**Windows current devices** - This term refers to domain-joined devices running Windows 10 or Windows Server 2016.</span></span>
- <span data-ttu-id="57d02-115">**Windows-klientversioner enheter** -termen avser alla **stöds** domänanslutna Windows-enheter som inte är igång Windows 10 eller Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="57d02-115">**Windows down-level devices** - This term refers to all **supported** domain-joined Windows devices that are neither running Windows 10 nor Windows Server 2016.</span></span>  


### <a name="windows-current-devices"></a><span data-ttu-id="57d02-116">Aktuella Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="57d02-116">Windows current devices</span></span>

- <span data-ttu-id="57d02-117">För enheter som kör Windows-operativsystemet, bör du använda Windows 10 årsdagar Update (version 1607) eller senare.</span><span class="sxs-lookup"><span data-stu-id="57d02-117">For devices running the Windows desktop operating system, we recommend using Windows 10 Anniversary Update (version 1607) or later.</span></span> 
- <span data-ttu-id="57d02-118">Registreringen av aktuella Windows-enheter **är** i ofedererad miljöer, till exempel lösenord hash-synkronisering konfigurationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="57d02-118">The registration of Windows current devices **is** supported in non-federated environments such as password hash sync configurations.</span></span>  


### <a name="windows-down-level-devices"></a><span data-ttu-id="57d02-119">Äldre Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="57d02-119">Windows down-level devices</span></span>

- <span data-ttu-id="57d02-120">Följande Windows-klientversioner enheter stöds:</span><span class="sxs-lookup"><span data-stu-id="57d02-120">The following Windows down-level devices are supported:</span></span>
    - <span data-ttu-id="57d02-121">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="57d02-121">Windows 8.1</span></span>
    - <span data-ttu-id="57d02-122">Windows 7</span><span class="sxs-lookup"><span data-stu-id="57d02-122">Windows 7</span></span>
    - <span data-ttu-id="57d02-123">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="57d02-123">Windows Server 2012 R2</span></span>
    - <span data-ttu-id="57d02-124">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="57d02-124">Windows Server 2012</span></span>
    - <span data-ttu-id="57d02-125">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="57d02-125">Windows Server 2008 R2</span></span>
- <span data-ttu-id="57d02-126">Registrering av Windows-klientversioner enheter **är** stöds i ofedererad miljöer genom sömlös enkel inloggning [Azure Active Directory sömlös enkel inloggning](https://aka.ms/hybrid/sso).</span><span class="sxs-lookup"><span data-stu-id="57d02-126">The registration of Windows down-level devices **is** supported in non-federated environments through Seamless Single Sign On [Azure Active Directory Seamless Single Sign-On](https://aka.ms/hybrid/sso).</span></span>
- <span data-ttu-id="57d02-127">Registrering av Windows-klientversioner enheter **är inte** stöds för enheter med hjälp av centrala profiler.</span><span class="sxs-lookup"><span data-stu-id="57d02-127">The registration of Windows down-level devices **is not** supported for devices using roaming profiles.</span></span> <span data-ttu-id="57d02-128">Om du beroende av centrala profiler eller inställningar, använder du Windows 10.</span><span class="sxs-lookup"><span data-stu-id="57d02-128">If you are relying on roaming of profiles or settings, use Windows 10.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="57d02-129">Krav</span><span class="sxs-lookup"><span data-stu-id="57d02-129">Prerequisites</span></span>

<span data-ttu-id="57d02-130">Innan du börjar att aktivera automatisk registrering för domänanslutna enheter i din organisation, måste du se till att du kör en uppdaterad version av Azure AD connect.</span><span class="sxs-lookup"><span data-stu-id="57d02-130">Before you start enabling the auto-registration of domain-joined devices in your organization, you need to make sure that you are running an up-to-date version of Azure AD connect.</span></span>

<span data-ttu-id="57d02-131">Azure AD Connect:</span><span class="sxs-lookup"><span data-stu-id="57d02-131">Azure AD Connect:</span></span>

- <span data-ttu-id="57d02-132">Behåller associationen mellan datorkonto i din lokala Active Directory (AD) och enhetsobjekt i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57d02-132">Keeps the association between the computer account in your on-premises Active Directory (AD) and the device object in Azure AD.</span></span> 
- <span data-ttu-id="57d02-133">Gör andra enheter relaterade funktioner som Windows Hello för företag.</span><span class="sxs-lookup"><span data-stu-id="57d02-133">Enables other device related features like Windows Hello for Business.</span></span>



## <a name="configuration-steps"></a><span data-ttu-id="57d02-134">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="57d02-134">Configuration steps</span></span>

<span data-ttu-id="57d02-135">Det här avsnittet innehåller de nödvändiga stegen för alla scenarier för typisk konfiguration.</span><span class="sxs-lookup"><span data-stu-id="57d02-135">This topic includes the required steps for all typical configuration scenarios.</span></span>  
<span data-ttu-id="57d02-136">Använd följande tabell för att få en översikt över de steg som krävs för ditt scenario:</span><span class="sxs-lookup"><span data-stu-id="57d02-136">Use the following table to get an overview of the steps that are required for your scenario:</span></span>  



| <span data-ttu-id="57d02-137">Steg</span><span class="sxs-lookup"><span data-stu-id="57d02-137">Steps</span></span>                                      | <span data-ttu-id="57d02-138">Windows aktuella och lösenord hash-synkronisering</span><span class="sxs-lookup"><span data-stu-id="57d02-138">Windows current and password hash sync</span></span> | <span data-ttu-id="57d02-139">Windows aktuella och federation</span><span class="sxs-lookup"><span data-stu-id="57d02-139">Windows current and federation</span></span> | <span data-ttu-id="57d02-140">Windows-klientversioner</span><span class="sxs-lookup"><span data-stu-id="57d02-140">Windows down-level</span></span> |
| :--                                        | :-:                                    | :-:                            | :-:                |
| <span data-ttu-id="57d02-141">Steg 1: Konfigurera tjänstanslutningspunkten</span><span class="sxs-lookup"><span data-stu-id="57d02-141">Step 1: Configure service connection point</span></span> | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="57d02-145">Steg 2: Konfigurera utfärdande av anspråk</span><span class="sxs-lookup"><span data-stu-id="57d02-145">Step 2: Setup issuance of claims</span></span>           |                                        | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="57d02-148">Steg 3: Aktivera Windows 10-enheter</span><span class="sxs-lookup"><span data-stu-id="57d02-148">Step 3: Enable non-Windows 10 devices</span></span>      |                                        |                                | ![Markera][1]        |
| <span data-ttu-id="57d02-150">Steg 4: Distribution av kontrollen och distribution</span><span class="sxs-lookup"><span data-stu-id="57d02-150">Step 4: Control deployment and rollout</span></span>     | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="57d02-154">Steg 5: Kontrollera registrerade enheter</span><span class="sxs-lookup"><span data-stu-id="57d02-154">Step 5: Verify registered devices</span></span>          | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |



## <a name="step-1-configure-service-connection-point"></a><span data-ttu-id="57d02-158">Steg 1: Konfigurera tjänstanslutningspunkten</span><span class="sxs-lookup"><span data-stu-id="57d02-158">Step 1: Configure service connection point</span></span>

<span data-ttu-id="57d02-159">Serviceobjektet anslutning punkter (SCP) används av dina enheter under registreringen identifiera information för Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="57d02-159">The service connection point (SCP) object is used by your devices during the registration to discover Azure AD tenant information.</span></span> <span data-ttu-id="57d02-160">SCP-objektet för automatisk registrering för domänanslutna enheter måste finnas i konfigurationen naming kontexten partition av datorns skog i din lokala Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="57d02-160">In your on-premises Active Directory (AD), the SCP object for the auto-registration of domain-joined devices must exist in the configuration naming context partition of the computer's forest.</span></span> <span data-ttu-id="57d02-161">Det finns endast en konfigurationsnamngivningskontexten per skog.</span><span class="sxs-lookup"><span data-stu-id="57d02-161">There is only one configuration naming context per forest.</span></span> <span data-ttu-id="57d02-162">Tjänstanslutningspunkten måste finnas i alla skogar som innehåller domänanslutna datorer i en konfiguration för flera skogar Active Directory.</span><span class="sxs-lookup"><span data-stu-id="57d02-162">In a multi-forest Active Directory configuration, the service connection point must exist in all forests containing domain-joined computers.</span></span>

<span data-ttu-id="57d02-163">Du kan använda den [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) för att hämta konfigurationsnamngivningskontexten i skogen.</span><span class="sxs-lookup"><span data-stu-id="57d02-163">You can use the [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet to retrieve the configuration naming context of your forest.</span></span>  

<span data-ttu-id="57d02-164">För en skog med Active Directory-domännamn *fabrikam.com*, konfigurationsnamngivningskontexten är:</span><span class="sxs-lookup"><span data-stu-id="57d02-164">For a forest with the Active Directory domain name *fabrikam.com*, the configuration naming context is:</span></span>

`CN=Configuration,DC=fabrikam,DC=com`

<span data-ttu-id="57d02-165">I din skog finns SCP-objektet för automatisk registrering för domänanslutna enheter här:</span><span class="sxs-lookup"><span data-stu-id="57d02-165">In your forest, the SCP object for the auto-registration of domain-joined devices is located at:</span></span>  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

<span data-ttu-id="57d02-166">Beroende på hur du har distribuerat Azure AD Connect, har SCP-objektet redan konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="57d02-166">Depending on how you have deployed Azure AD Connect, the SCP object may have already been configured.</span></span>
<span data-ttu-id="57d02-167">Du kan verifiera att objektet och hämta identifiering värden med hjälp av följande Windows PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="57d02-167">You can verify the existence of the object and retrieve the discovery values using the following Windows PowerShell script:</span></span> 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

<span data-ttu-id="57d02-168">Den **$scp. Nyckelord** utdata visar information om Azure AD-klient, till exempel:</span><span class="sxs-lookup"><span data-stu-id="57d02-168">The **$scp.Keywords** output shows the Azure AD tenant information, for example:</span></span>

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

<span data-ttu-id="57d02-169">Om tjänstanslutningspunkten inte finns, kan du skapa den genom att köra den `Initialize-ADSyncDomainJoinedComputerSync` cmdlet på Azure AD Connect-servern.</span><span class="sxs-lookup"><span data-stu-id="57d02-169">If the service connection point does not exist, you can create it by running the `Initialize-ADSyncDomainJoinedComputerSync` cmdlet on your Azure AD Connect server.</span></span> <span data-ttu-id="57d02-170">Enterprise admin-autentiseringsuppgifter krävs för att köra denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="57d02-170">Enterprise admin credential is required to run this cmdlet.</span></span>  
<span data-ttu-id="57d02-171">Cmdlet:</span><span class="sxs-lookup"><span data-stu-id="57d02-171">The cmdlet:</span></span>

- <span data-ttu-id="57d02-172">Skapar tjänstanslutningspunkten i Azure AD Connect är ansluten till Active Directory-skogen.</span><span class="sxs-lookup"><span data-stu-id="57d02-172">Creates the service connection point in the Active Directory forest Azure AD Connect is connected to.</span></span> 
- <span data-ttu-id="57d02-173">Du måste ange den `AdConnectorAccount` parameter.</span><span class="sxs-lookup"><span data-stu-id="57d02-173">Requires you to specify the `AdConnectorAccount` parameter.</span></span> <span data-ttu-id="57d02-174">Detta är det konto som har konfigurerats som Active Directory connector-kontot i Azure AD connect.</span><span class="sxs-lookup"><span data-stu-id="57d02-174">This is the account that is configured as Active Directory connector account in Azure AD connect.</span></span> 


<span data-ttu-id="57d02-175">Följande skript visar ett exempel för att använda cmdlet.</span><span class="sxs-lookup"><span data-stu-id="57d02-175">The following script shows an example for using the cmdlet.</span></span> <span data-ttu-id="57d02-176">I det här skriptet `$aadAdminCred = Get-Credential` kräver att du anger ett användarnamn.</span><span class="sxs-lookup"><span data-stu-id="57d02-176">In this script, `$aadAdminCred = Get-Credential` requires you to type a user name.</span></span> <span data-ttu-id="57d02-177">Du måste ange ett användarnamn i formatet användarens huvudnamn (UPN) (`user@example.com`).</span><span class="sxs-lookup"><span data-stu-id="57d02-177">You need to provide the user name in the user principal name (UPN) format (`user@example.com`).</span></span> 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

<span data-ttu-id="57d02-178">Den `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="57d02-178">The `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span></span>

- <span data-ttu-id="57d02-179">Använder Active Directory PowerShell-modulen som förlitar sig på Active Directory-webbtjänster som körs på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="57d02-179">Uses the Active Directory PowerShell module, which relies on Active Directory Web Services running on a domain controller.</span></span> <span data-ttu-id="57d02-180">Active Directory Web Services fungerar på domänkontrollanter som kör Windows Server 2008 R2 och senare.</span><span class="sxs-lookup"><span data-stu-id="57d02-180">Active Directory Web Services is supported on domain controllers running Windows Server 2008 R2 and later.</span></span>
- <span data-ttu-id="57d02-181">Stöds endast av den **MSOnline PowerShell Modulversion 1.1.166.0**.</span><span class="sxs-lookup"><span data-stu-id="57d02-181">Is only supported by the **MSOnline PowerShell module version 1.1.166.0**.</span></span> <span data-ttu-id="57d02-182">Använd det här alternativet om du vill hämta den här modulen [länk](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span><span class="sxs-lookup"><span data-stu-id="57d02-182">To download this module, use this [link](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span></span>   

<span data-ttu-id="57d02-183">Använda skriptet nedan för domänkontrollanter som kör Windows Server 2008 eller tidigare versioner, för att skapa tjänstanslutningspunkten.</span><span class="sxs-lookup"><span data-stu-id="57d02-183">For domain controllers running Windows Server 2008 or earlier versions, use the script below to create the service connection point.</span></span>

<span data-ttu-id="57d02-184">Du bör använda följande skript för att skapa tjänstanslutningspunkten i varje skog där det finns datorer i en konfiguration för flera skogar:</span><span class="sxs-lookup"><span data-stu-id="57d02-184">In a multi-forest configuration, you should use the following script to create the service connection point in each forest where computers exist:</span></span>
 
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


## <a name="step-2-setup-issuance-of-claims"></a><span data-ttu-id="57d02-185">Steg 2: Konfigurera utfärdande av anspråk</span><span class="sxs-lookup"><span data-stu-id="57d02-185">Step 2: Setup issuance of claims</span></span>

<span data-ttu-id="57d02-186">I en federerad Azure AD-konfiguration enheter förlitar sig på Active Directory Federation Services (AD FS) eller 3 part lokalt federationstjänsten att autentisera till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57d02-186">In a federated Azure AD configuration, devices rely on Active Directory Federation Services (AD FS) or a 3rd party on-premises federation service to authenticate to Azure AD.</span></span> <span data-ttu-id="57d02-187">Enheter autentiseras för att få en åtkomsttoken att registrera mot Azure Active Directory Device registrering Service (Azure DRS).</span><span class="sxs-lookup"><span data-stu-id="57d02-187">Devices authenticate to get an access token to register against the Azure Active Directory Device Registration Service (Azure DRS).</span></span>

<span data-ttu-id="57d02-188">Windows aktuella enheter autentiseras med integrerad Windows-autentisering till en aktiv WS-Trust-slutpunkt (1.3 eller 2005 versioner) hos den lokala federationstjänsten.</span><span class="sxs-lookup"><span data-stu-id="57d02-188">Windows current devices authenticate using Integrated Windows Authentication to an active WS-Trust endpoint (either 1.3 or 2005 versions) hosted by the on-premises federation service.</span></span>

> [!NOTE]
> <span data-ttu-id="57d02-189">När du använder AD FS, antingen **adfs/services/trust/13/windowstransport** eller **adfs/services/trust/2005/windowstransport** måste vara aktiverat.</span><span class="sxs-lookup"><span data-stu-id="57d02-189">When using AD FS, either **adfs/services/trust/13/windowstransport** or **adfs/services/trust/2005/windowstransport** must be enabled.</span></span> <span data-ttu-id="57d02-190">Om du använder en webbproxy för autentisering också se till att den här slutpunkten har publicerats via proxy.</span><span class="sxs-lookup"><span data-stu-id="57d02-190">If you are using the Web Authentication Proxy, also ensure that this endpoint is published through the proxy.</span></span> <span data-ttu-id="57d02-191">Du kan se vilka slutpunkter aktiveras via AD FS-hanteringskonsol under **Service > slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="57d02-191">You can see what end-points are enabled through the AD FS management console under **Service > Endpoints**.</span></span>
>
><span data-ttu-id="57d02-192">Om du inte har AD FS som federationstjänsten lokalt, följ instruktionerna för din leverantör för att kontrollera att de stöder WS-Trust 1.3 eller 2005 slutpunkter och dessa publiceras via Metadata Exchange-fil (MEX).</span><span class="sxs-lookup"><span data-stu-id="57d02-192">If you don’t have AD FS as your on-premises federation service, follow the instructions of your vendor to make sure they support WS-Trust 1.3 or 2005 end-points and that these are published through the Metadata Exchange file (MEX).</span></span>

<span data-ttu-id="57d02-193">Följande anspråk måste finnas i den token som tas emot av Azure DRS för registrering av enheten för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="57d02-193">The following claims must exist in the token received by Azure DRS for device registration to complete.</span></span> <span data-ttu-id="57d02-194">Azure DRS skapar ett enhetsobjekt i Azure AD med en del av den här informationen som sedan används av Azure AD Connect för att associera det nyligen skapade enhetsobjektet med datorn kontot Lokal.</span><span class="sxs-lookup"><span data-stu-id="57d02-194">Azure DRS will create a device object in Azure AD with some of this information which is then used by Azure AD Connect to associate the newly created device object with the computer account on-premises.</span></span>

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

<span data-ttu-id="57d02-195">Om du har mer än ett verifierat domännamn, måste du ange följande anspråk för datorer:</span><span class="sxs-lookup"><span data-stu-id="57d02-195">If you have more than one verified domain name, you need to provide the following claim for computers:</span></span>

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

<span data-ttu-id="57d02-196">Om du redan utfärdar ett ImmutableID anspråk (t.ex. Alternativt inloggnings-ID) måste du ange ett motsvarande anspråk för datorer:</span><span class="sxs-lookup"><span data-stu-id="57d02-196">If you are already issuing an ImmutableID claim (e.g., alternate login ID) you need to provide one corresponding claim for computers:</span></span>

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

<span data-ttu-id="57d02-197">I följande avsnitt hittar du information om:</span><span class="sxs-lookup"><span data-stu-id="57d02-197">In the following sections, you find information about:</span></span>
 
- <span data-ttu-id="57d02-198">Värdet för varje anspråk ska ha</span><span class="sxs-lookup"><span data-stu-id="57d02-198">The values each claim should have</span></span>
- <span data-ttu-id="57d02-199">Hur en definition skulle se ut i AD FS</span><span class="sxs-lookup"><span data-stu-id="57d02-199">How a definition would look like in AD FS</span></span>

<span data-ttu-id="57d02-200">Definitionen hjälper dig att kontrollera om värdena finns eller om du behöver skapa dem.</span><span class="sxs-lookup"><span data-stu-id="57d02-200">The definition helps you to verify whether the values are present or if you need to create them.</span></span>

> [!NOTE]
> <span data-ttu-id="57d02-201">Om du inte använder AD FS för federationsservern lokalt, följer du leverantörens instruktioner för att skapa inställningar för att utfärda dessa anspråk.</span><span class="sxs-lookup"><span data-stu-id="57d02-201">If you don’t use AD FS for your on-premises federation server, follow your vendor's instructions to create the appropriate configuration to issue these claims.</span></span>

### <a name="issue-account-type-claim"></a><span data-ttu-id="57d02-202">Problemet konto typen anspråk</span><span class="sxs-lookup"><span data-stu-id="57d02-202">Issue account type claim</span></span>

<span data-ttu-id="57d02-203">**`http://schemas.microsoft.com/ws/2012/01/accounttype`**-Detta anspråk måste innehålla ett värde av **DJ**, som identifierar enheten som en domänansluten dator.</span><span class="sxs-lookup"><span data-stu-id="57d02-203">**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - This claim must contain a value of **DJ**, which identifies the device as a domain-joined computer.</span></span> <span data-ttu-id="57d02-204">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="57d02-204">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a><span data-ttu-id="57d02-205">Utfärda objectGUID av datorn kontot Lokal</span><span class="sxs-lookup"><span data-stu-id="57d02-205">Issue objectGUID of the computer account on-premises</span></span>

<span data-ttu-id="57d02-206">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**-Detta anspråk måste innehålla den **objectGUID** värdet för det lokala datorkontot.</span><span class="sxs-lookup"><span data-stu-id="57d02-206">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - This claim must contain the **objectGUID** value of the on-premises computer account.</span></span> <span data-ttu-id="57d02-207">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="57d02-207">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a><span data-ttu-id="57d02-208">Utfärda objectSID för den dator kontot Lokalt</span><span class="sxs-lookup"><span data-stu-id="57d02-208">Issue objectSID of the computer account on-premises</span></span>

<span data-ttu-id="57d02-209">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**-Detta anspråk måste innehålla den den **objectSid** värdet för det lokala datorkontot.</span><span class="sxs-lookup"><span data-stu-id="57d02-209">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - This claim must contain the the **objectSid** value of the on-premises computer account.</span></span> <span data-ttu-id="57d02-210">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="57d02-210">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a><span data-ttu-id="57d02-211">Utfärda issuerID för datorn när flera verifierat domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="57d02-211">Issue issuerID for computer when multiple verified domain names in Azure AD</span></span>

<span data-ttu-id="57d02-212">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**-Detta anspråk måste innehålla den identifierare URI (Uniform Resource) för någon av de verifierade domännamn som ansluter med federationstjänsten lokalt (AD FS eller 3 part) utfärdar token.</span><span class="sxs-lookup"><span data-stu-id="57d02-212">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - This claim must contain the Uniform Resource Identifier (URI) of any of the verified domain names that connect with the on-premises federation service (AD FS or 3rd party) issuing the token.</span></span> <span data-ttu-id="57d02-213">Du kan lägga till regler för utfärdandetransformering som liknar de nedan i den specifika ordningen när de ovan i AD FS.</span><span class="sxs-lookup"><span data-stu-id="57d02-213">In AD FS, you can add issuance transform rules that look like the ones below in that specific order after the ones above.</span></span> <span data-ttu-id="57d02-214">Observera att en regel att uttryckligen utfärda regeln för användare är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="57d02-214">Please note that one rule to explicitly issue the rule for users is necessary.</span></span> <span data-ttu-id="57d02-215">En första regel som identifierar användare eller datorautentisering läggs till i reglerna nedan.</span><span class="sxs-lookup"><span data-stu-id="57d02-215">In the rules below, a first rule identifying user vs. computer authentication is added.</span></span>

    @RuleName = "Issue account type with the value User when its not a computer"
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
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
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


<span data-ttu-id="57d02-216">I anspråket ovan</span><span class="sxs-lookup"><span data-stu-id="57d02-216">In the claim above,</span></span>

- <span data-ttu-id="57d02-217">`$<domain>`är URL: en för AD FS-tjänsten</span><span class="sxs-lookup"><span data-stu-id="57d02-217">`$<domain>` is the AD FS service URL</span></span>
- <span data-ttu-id="57d02-218">`<verified-domain-name>`en platshållare som du behöver ersätta med en av dina verifierat domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="57d02-218">`<verified-domain-name>` is a placeholder you need to replace with one of your verified domain names in Azure AD</span></span>



<span data-ttu-id="57d02-219">Mer information om verifierat domännamn finns [lägga till ett anpassat domännamn i Azure Active Directory](active-directory-add-domain.md).</span><span class="sxs-lookup"><span data-stu-id="57d02-219">For more details about verified domain names, see [Add a custom domain name to Azure Active Directory](active-directory-add-domain.md).</span></span>  
<span data-ttu-id="57d02-220">Om du vill hämta en lista över företagets verifierade domäner som du kan använda den [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="57d02-220">To get a list of your verified company domains, you can use the [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span></span> 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a><span data-ttu-id="57d02-222">Utfärda ImmutableID för datorn när en användare finns (t.ex. Alternativt inloggnings-ID har angetts)</span><span class="sxs-lookup"><span data-stu-id="57d02-222">Issue ImmutableID for computer when one for users exist (e.g. alternate login ID is set)</span></span>

<span data-ttu-id="57d02-223">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**-Detta anspråk måste innehålla ett giltigt värde för datorer.</span><span class="sxs-lookup"><span data-stu-id="57d02-223">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - This claim must contain a valid value for computers.</span></span> <span data-ttu-id="57d02-224">I AD FS kan du skapa en regel för omvandling av utfärdande som följer:</span><span class="sxs-lookup"><span data-stu-id="57d02-224">In AD FS, you can create an issuance transform rule as follows:</span></span>

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

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a><span data-ttu-id="57d02-225">Helper-skript för att skapa AD FS utfärdande reglerna för omvandling</span><span class="sxs-lookup"><span data-stu-id="57d02-225">Helper script to create the AD FS issuance transform rules</span></span>

<span data-ttu-id="57d02-226">Skriptet nedan hjälper dig att skapa utfärdande regler som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="57d02-226">The following script helps you with the creation of the issuance transform rules described above.</span></span>

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
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
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
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
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

### <a name="remarks"></a><span data-ttu-id="57d02-227">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="57d02-227">Remarks</span></span> 

- <span data-ttu-id="57d02-228">Skriptet lägger till regler på befintliga regler.</span><span class="sxs-lookup"><span data-stu-id="57d02-228">This script appends the rules to the existing rules.</span></span> <span data-ttu-id="57d02-229">Kör inte skriptet två gånger eftersom uppsättningen regler skulle läggas till två gånger.</span><span class="sxs-lookup"><span data-stu-id="57d02-229">Do not run the script twice because the set of rules would be added twice.</span></span> <span data-ttu-id="57d02-230">Se till att motsvarande inte finns några regler för dessa anspråk (villkor motsvarande) innan du kör skriptet igen.</span><span class="sxs-lookup"><span data-stu-id="57d02-230">Make sure that no corresponding rules exist for these claims (under the corresponding conditions) before running the script again.</span></span>

- <span data-ttu-id="57d02-231">Om du har flera verifierat domännamn (som visas i Azure AD-portalen eller via cmdlet Get-MsolDomains), ange värdet för **$multipleVerifiedDomainNames** i skriptet till **$true**.</span><span class="sxs-lookup"><span data-stu-id="57d02-231">If you have multiple verified domain names (as shown in the Azure AD portal or via the Get-MsolDomains cmdlet), set the value of **$multipleVerifiedDomainNames** in the script to **$true**.</span></span> <span data-ttu-id="57d02-232">Kontrollera också att du tar bort alla befintliga issuerid anspråk som kan ha skapats av Azure AD Connect eller via andra sätt.</span><span class="sxs-lookup"><span data-stu-id="57d02-232">Also make sure that you remove any existing issuerid claim that might have been created by Azure AD Connect or via other means.</span></span> <span data-ttu-id="57d02-233">Här är ett exempel på den här regeln:</span><span class="sxs-lookup"><span data-stu-id="57d02-233">Here is an example for this rule:</span></span>


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- <span data-ttu-id="57d02-234">Om du redan har skickat en **ImmutableID** anspråk för användarkonton, ange värdet för **$immutableIDAlreadyIssuedforUsers** i skriptet till **$true**.</span><span class="sxs-lookup"><span data-stu-id="57d02-234">If you have already issued an **ImmutableID** claim  for user accounts, set the value of **$immutableIDAlreadyIssuedforUsers** in the script to **$true**.</span></span>

## <a name="step-3-enable-windows-down-level-devices"></a><span data-ttu-id="57d02-235">Steg 3: Aktivera äldre Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="57d02-235">Step 3: Enable Windows down-level devices</span></span>

<span data-ttu-id="57d02-236">Om vissa enheter domänanslutna Windows äldre enheter, måste du:</span><span class="sxs-lookup"><span data-stu-id="57d02-236">If some of your domain-joined devices Windows down-level devices, you need to:</span></span>

- <span data-ttu-id="57d02-237">Ange en princip i Azure AD så att användarna kan registrera enheter.</span><span class="sxs-lookup"><span data-stu-id="57d02-237">Set a policy in Azure AD to enable users to register devices.</span></span>
 
- <span data-ttu-id="57d02-238">Konfigurera federationstjänsten lokalt för att utfärda anspråk för att stödja **Windows IWA (Integrated Authentication)** för registrering av enheten.</span><span class="sxs-lookup"><span data-stu-id="57d02-238">Configure your on-premises federation service to issue claims to support **Integrated Windows Authentication (IWA)** for device registration.</span></span>
 
- <span data-ttu-id="57d02-239">Lägg till Azure AD-enheten autentiseringsslutpunkten zonerna lokala intranät för att undvika anvisningarna för certifikat vid autentisering av enheten.</span><span class="sxs-lookup"><span data-stu-id="57d02-239">Add the Azure AD device authentication end-point to the local Intranet zones to avoid certificate prompts when authenticating the device.</span></span>

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a><span data-ttu-id="57d02-240">Ange principen i Azure AD så att användarna kan registrera enheter</span><span class="sxs-lookup"><span data-stu-id="57d02-240">Set policy in Azure AD to enable users to register devices</span></span>

<span data-ttu-id="57d02-241">Om du vill registrera äldre Windows-enheter måste du se till att inställningen för att tillåta användare att registrera enheter i Azure AD har angetts.</span><span class="sxs-lookup"><span data-stu-id="57d02-241">To register Windows down-level devices, you need to make sure that the setting to allow users to register devices in Azure AD is set.</span></span> <span data-ttu-id="57d02-242">Du hittar den här inställningen under i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="57d02-242">In the Azure portal, you can find this setting under:</span></span>

`Azure Active Directory > Users and groups > Device settings`
    
<span data-ttu-id="57d02-243">Följande princip måste anges till **alla**: **användarna kan registrera sina enheter med Azure AD**</span><span class="sxs-lookup"><span data-stu-id="57d02-243">The following policy must be set to **All**: **Users may register their devices with Azure AD**</span></span>

![Registrera enheter](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a><span data-ttu-id="57d02-245">Konfigurera lokala federationstjänsten</span><span class="sxs-lookup"><span data-stu-id="57d02-245">Configure on-premises federation service</span></span> 

<span data-ttu-id="57d02-246">Federationstjänsten lokalt måste ha stöd för utfärdande av **authenticationmehod** och **wiaormultiauthn** anspråk när du tar emot en autentiseringsbegäran till den förlitande parten för Azure AD hålla en resouce_params parameter med ett kodat värde som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="57d02-246">Your on-premises federation service must support issuing the **authenticationmehod** and **wiaormultiauthn** claims when receiving an authentication request to the Azure AD relying party holding a resouce_params parameter with an encoded value as shown below:</span></span>

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

<span data-ttu-id="57d02-247">När en sådan begäran kommer den lokala federationstjänsten måste autentisera användare som använder integrerad Windows-autentisering och vid lyckades, utfärda följande två anspråk:</span><span class="sxs-lookup"><span data-stu-id="57d02-247">When such a request comes, the on-premises federation service must authenticate the user using Integrated Windows Authentication and upon success, it must issue the following two claims:</span></span>

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

<span data-ttu-id="57d02-248">Du måste lägga till transformeringen regeluppsättningarna som överför via autentiseringsmetoden i AD FS.</span><span class="sxs-lookup"><span data-stu-id="57d02-248">In AD FS, you must add an issuance transform rule that passes-through the authentication method.</span></span>  

<span data-ttu-id="57d02-249">**Lägg till den här regeln:**</span><span class="sxs-lookup"><span data-stu-id="57d02-249">**To add this rule:**</span></span>

1. <span data-ttu-id="57d02-250">I hanteringskonsolen för AD FS går du till `AD FS > Trust Relationships > Relying Party Trusts`.</span><span class="sxs-lookup"><span data-stu-id="57d02-250">In the AD FS management console, go to `AD FS > Trust Relationships > Relying Party Trusts`.</span></span>
2. <span data-ttu-id="57d02-251">Högerklicka på Microsoft Office 365-Identitetsplattformen förlitande part förtroende-objekt och välj sedan **redigera Anspråksregler**.</span><span class="sxs-lookup"><span data-stu-id="57d02-251">Right-click the Microsoft Office 365 Identity Platform relying party trust object, and then select **Edit Claim Rules**.</span></span>
3. <span data-ttu-id="57d02-252">På den **regler för Utfärdandetransformering** väljer **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="57d02-252">On the **Issuance Transform Rules** tab, select **Add Rule**.</span></span>
4. <span data-ttu-id="57d02-253">I den **anspråksregel** mall-listan, Välj **skicka anspråk med en anpassad regel**.</span><span class="sxs-lookup"><span data-stu-id="57d02-253">In the **Claim rule** template list, select **Send Claims Using a Custom Rule**.</span></span>
5. <span data-ttu-id="57d02-254">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="57d02-254">Select **Next**.</span></span>
6. <span data-ttu-id="57d02-255">I den **Regelnamn för anspråk** skriver **Auth metoden Anspråksregel**.</span><span class="sxs-lookup"><span data-stu-id="57d02-255">In the **Claim rule name** box, type **Auth Method Claim Rule**.</span></span>
7. <span data-ttu-id="57d02-256">I den **anspråksregel** skriver följande regel:</span><span class="sxs-lookup"><span data-stu-id="57d02-256">In the **Claim rule** box, type the following rule:</span></span>

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. <span data-ttu-id="57d02-257">Ange PowerShell-kommandot nedan på federationsservern när du ersätter  **\<RPObjectName\>**  med förlitande part objektnamnet för din Azure AD förlitande part förtroende-objekt.</span><span class="sxs-lookup"><span data-stu-id="57d02-257">On your federation server, type the PowerShell command below after replacing **\<RPObjectName\>** with the relying party object name for your Azure AD relying party trust object.</span></span> <span data-ttu-id="57d02-258">Det här objektet vanligen namnet **Identitetsplattformen för Microsoft Office 365**.</span><span class="sxs-lookup"><span data-stu-id="57d02-258">This object usually is named **Microsoft Office 365 Identity Platform**.</span></span>
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a><span data-ttu-id="57d02-259">Lägger till Azure AD-enheten autentiseringsslutpunkten i zonerna Lokalt intranät</span><span class="sxs-lookup"><span data-stu-id="57d02-259">Add the Azure AD device authentication end-point to the Local Intranet zones</span></span>

<span data-ttu-id="57d02-260">Uppmanar när användare registrera enheter autentiserar till Azure AD som du kan pressa en princip till dina domänanslutna enheter att lägga till följande URL i zonen Lokalt intranät i Internet Explorer för att undvika certifikat:</span><span class="sxs-lookup"><span data-stu-id="57d02-260">To avoid certificate prompts when users in register devices authenticate to Azure AD you can push a policy to your domain-joined devices to add the following URL to the Local Intranet zone in Internet Explorer:</span></span>

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a><span data-ttu-id="57d02-261">Steg 4: Distribution av kontrollen och distribution</span><span class="sxs-lookup"><span data-stu-id="57d02-261">Step 4: Control deployment and rollout</span></span>

<span data-ttu-id="57d02-262">När du har slutfört nödvändiga steg kan domänanslutna enheter registreras automatiskt med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57d02-262">When you have completed the required steps, domain-joined devices are ready to automatically register with Azure AD.</span></span> <span data-ttu-id="57d02-263">Alla domänanslutna enheter som kör Windows 10 årsdagar Update och Windows Server 2016 registrera med Azure AD på enheten startas om automatiskt eller användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="57d02-263">All domain-joined devices running Windows 10 Anniversary Update and Windows Server 2016 automatically register with Azure AD at device restart or user sign-in.</span></span> <span data-ttu-id="57d02-264">Att registrera nya enheter med Azure AD när enheten startas om efter domän join-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="57d02-264">New devices register with Azure AD when the device restarts after the domain join operation is completed.</span></span>

<span data-ttu-id="57d02-265">Enheter som fanns tidigare arbetsplats-ansluten till Azure AD (till exempel för Intune) övergången till ”*domänanslutna, AAD registrerade*”, men det tar en stund att slutföra på alla enheter på grund av det normala flödet av domän- och användaraktivitet.</span><span class="sxs-lookup"><span data-stu-id="57d02-265">Devices that were previously workplace-joined to Azure AD (for example for Intune) transition to “*Domain Joined, AAD Registered*”; however it takes some time for this process to complete across all devices due to the normal flow of domain and user activity.</span></span>

### <a name="remarks"></a><span data-ttu-id="57d02-266">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="57d02-266">Remarks</span></span>

- <span data-ttu-id="57d02-267">Du kan använda ett grupprincipobjekt för att styra distributionen av automatisk registrering av Windows 10 och Windows Server 2016 domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="57d02-267">You can use a Group Policy object to control the rollout of automatic registration of Windows 10 and Windows Server 2016 domain-joined computers.</span></span>

- <span data-ttu-id="57d02-268">Windows 10 November 2015 Update automatiskt registrerar med Azure AD **endast** om grupprincipobjektet för distribution har angetts.</span><span class="sxs-lookup"><span data-stu-id="57d02-268">Windows 10 November 2015 Update automatically registers with Azure AD **only** if the rollout Group Policy object is set.</span></span>

- <span data-ttu-id="57d02-269">Distribution av automatisk registrering av äldre Windows-datorer, du kan distribuera en [Windows Installer-paketet](#windows-installer-packages-for-non-windows-10-computers) till datorer som du väljer.</span><span class="sxs-lookup"><span data-stu-id="57d02-269">To rollout the automatic registration of Windows down-level computers, you can deploy a [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) to computers that you select.</span></span>

- <span data-ttu-id="57d02-270">Om du push grupprincipobjektet till Windows 8.1-domänanslutna enheter görs registrering; men det rekommenderas att du använder den [Windows Installer-paketet](#windows-installer-packages-for-non-windows-10-computers) att registrera alla dina Windows äldre enheter.</span><span class="sxs-lookup"><span data-stu-id="57d02-270">If you push the Group Policy object to Windows 8.1 domain-joined devices, registration will be attempted; however it is recommended that you use the [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) to register all your Windows down-level devices.</span></span> 

### <a name="create-a-group-policy-object"></a><span data-ttu-id="57d02-271">Skapa ett grupprincipobjekt</span><span class="sxs-lookup"><span data-stu-id="57d02-271">Create a Group Policy object</span></span> 

<span data-ttu-id="57d02-272">För att styra distributionen av automatisk registrering av aktuella Windows-datorer, bör du distribuera den **registrera domänanslutna datorer som enheter** grupprincipobjektet till de enheter som du vill registrera.</span><span class="sxs-lookup"><span data-stu-id="57d02-272">To control the rollout of automatic registration of Windows current computers, you should deploy the **Register domain-joined computers as devices** Group Policy object to the devices you want to register.</span></span> <span data-ttu-id="57d02-273">Du kan till exempel distribuera principen till en organisationsenhet eller till en säkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="57d02-273">For example, you can deploy the policy to an organizational unit or to a security group.</span></span>

<span data-ttu-id="57d02-274">**Att ställa in principen:**</span><span class="sxs-lookup"><span data-stu-id="57d02-274">**To set the policy:**</span></span>

1. <span data-ttu-id="57d02-275">Öppna **Serverhanteraren**, gå sedan till `Tools > Group Policy Management`.</span><span class="sxs-lookup"><span data-stu-id="57d02-275">Open **Server Manager**, and then go to `Tools > Group Policy Management`.</span></span>
2. <span data-ttu-id="57d02-276">Gå till domännoden som motsvarar den domän där du vill aktivera automatisk registrering av aktuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="57d02-276">Go to the domain node that corresponds to the domain where you want to activate auto-registration of Windows current computers.</span></span>
3. <span data-ttu-id="57d02-277">Högerklicka på **grupprincipobjekt**, och välj sedan **ny**.</span><span class="sxs-lookup"><span data-stu-id="57d02-277">Right-click **Group Policy Objects**, and then select **New**.</span></span>
4. <span data-ttu-id="57d02-278">Ange ett namn för grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="57d02-278">Type a name for your Group Policy object.</span></span> <span data-ttu-id="57d02-279">Till exempel *automatisk registrering till Azure AD*.</span><span class="sxs-lookup"><span data-stu-id="57d02-279">For example, *Automatic Registration to Azure AD*.</span></span> <span data-ttu-id="57d02-280">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="57d02-280">Select **OK**.</span></span>
5. <span data-ttu-id="57d02-281">Högerklicka på din nya grupprincipobjekt och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="57d02-281">Right-click your new Group Policy object, and then select **Edit**.</span></span>
6. <span data-ttu-id="57d02-282">Gå till **Datorkonfiguration** > **principer** > **Administrationsmallar** > **Windows-komponenter** > **Enhetsregistrering**.</span><span class="sxs-lookup"><span data-stu-id="57d02-282">Go to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Device Registration**.</span></span> <span data-ttu-id="57d02-283">Högerklicka på **registrera domänanslutna datorer som enheter**, och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="57d02-283">Right-click **Register domain-joined computers as devices**, and then select **Edit**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="57d02-284">Den här mallen har ändrats från tidigare versioner av konsolen Grupprinciphantering.</span><span class="sxs-lookup"><span data-stu-id="57d02-284">This Group Policy template has been renamed from earlier versions of the Group Policy Management console.</span></span> <span data-ttu-id="57d02-285">Om du använder en tidigare version av konsolen, gå till `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span><span class="sxs-lookup"><span data-stu-id="57d02-285">If you are using an earlier version of the console, go to `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span></span> 

7. <span data-ttu-id="57d02-286">Välj **aktiverad**, och välj sedan **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="57d02-286">Select **Enabled**, and then select **Apply**.</span></span>
8. <span data-ttu-id="57d02-287">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="57d02-287">Select **OK**.</span></span>
9. <span data-ttu-id="57d02-288">Länka grupprincipobjektet till en plats.</span><span class="sxs-lookup"><span data-stu-id="57d02-288">Link the Group Policy object to a location of your choice.</span></span> <span data-ttu-id="57d02-289">Du kan till exempel länka den till en viss organisationsenhet.</span><span class="sxs-lookup"><span data-stu-id="57d02-289">For example, you can link it to a specific organizational unit.</span></span> <span data-ttu-id="57d02-290">Dessutom kan du länka den till en viss säkerhetsgrupp för datorer som registreras automatiskt med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57d02-290">You also could link it to a specific security group of computers that automatically register with Azure AD.</span></span> <span data-ttu-id="57d02-291">Länka grupprincipobjektet till domänen för att ställa in principen för alla domänanslutna Windows 10 och Windows Server 2016 datorer i din organisation.</span><span class="sxs-lookup"><span data-stu-id="57d02-291">To set this policy for all domain-joined Windows 10 and Windows Server 2016 computers in your organization, link the Group Policy object to the domain.</span></span>

### <a name="windows-installer-packages-for-non-windows-10-computers"></a><span data-ttu-id="57d02-292">Windows Installer-paket för Windows 10-datorer</span><span class="sxs-lookup"><span data-stu-id="57d02-292">Windows Installer packages for non-Windows 10 computers</span></span>

<span data-ttu-id="57d02-293">För att registrera domänanslutna äldre Windows-datorer i en federerad miljö kan du hämta och installera de här Windows Installer-paketet (.msi) från Download Center på den [Microsoft till arbetsplatsen för Windows 10-datorer](https://www.microsoft.com/en-us/download/details.aspx?id=53554) sidan.</span><span class="sxs-lookup"><span data-stu-id="57d02-293">To register domain-joined Windows down-level computers in a federated environment, you can download and install these Windows Installer package (.msi) from Download Center at the [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) page.</span></span>

<span data-ttu-id="57d02-294">Du kan distribuera paketet med hjälp av ett program distributionssystem som System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="57d02-294">You can deploy the package by using a software distribution system like System Center Configuration Manager.</span></span> <span data-ttu-id="57d02-295">Paketet stöder alternativen standard tyst installation med den *tyst* parameter.</span><span class="sxs-lookup"><span data-stu-id="57d02-295">The package supports the standard silent install options with the *quiet* parameter.</span></span> <span data-ttu-id="57d02-296">System Center Configuration Manager Current Branch erbjuder ytterligare fördelar jämfört med tidigare versioner, t.ex. möjligheten att spåra slutförda registreringar.</span><span class="sxs-lookup"><span data-stu-id="57d02-296">System Center Configuration Manager Current Branch offers additional benefits from earlier versions, like the ability to track completed registrations.</span></span> <span data-ttu-id="57d02-297">Mer information finns i [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="57d02-297">For more information, see [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span></span>

<span data-ttu-id="57d02-298">Installationsprogrammet skapar en schemalagd aktivitet på datorn som körs i användarens kontext.</span><span class="sxs-lookup"><span data-stu-id="57d02-298">The installer creates a scheduled task on the system that runs in the user’s context.</span></span> <span data-ttu-id="57d02-299">Aktiviteten utlöses när användaren loggar in på Windows.</span><span class="sxs-lookup"><span data-stu-id="57d02-299">The task is triggered when the user signs in to Windows.</span></span> <span data-ttu-id="57d02-300">Uppgiften registrerar tyst enheten med Azure AD med autentiseringsuppgifterna för användaren när autentisering med integrerad Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="57d02-300">The task silently registers the device with Azure AD with the user credentials after authenticating using Integrated Windows Authentication.</span></span> <span data-ttu-id="57d02-301">Den schemalagda aktiviteten i den aktuella enheten, gå till **Microsoft** > **Arbetsplatsanslutning**, och gå sedan till bibliotek för Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="57d02-301">To see the scheduled task, in the device, go to **Microsoft** > **Workplace Join**, and then go to the Task Scheduler library.</span></span>

## <a name="step-5-verify-registered-devices"></a><span data-ttu-id="57d02-302">Steg 5: Kontrollera registrerade enheter</span><span class="sxs-lookup"><span data-stu-id="57d02-302">Step 5: Verify registered devices</span></span>

<span data-ttu-id="57d02-303">Du kan kontrollera lyckade registrerade enheter i organisationen med hjälp av den [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet i den [Azure Active Directory PowerShell-modulen](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="57d02-303">You can check successful registered devices in your organization by using the [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in the [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span>

<span data-ttu-id="57d02-304">Utdata från den här cmdleten visar enheter som har registrerats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57d02-304">The output of this cmdlet shows devices registered in Azure AD.</span></span> <span data-ttu-id="57d02-305">Alla enheter får använda den **-alla** parameter, och filtrera dem med hjälp av den **deviceTrustType** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="57d02-305">To get all devices, use the **-All** parameter, and then filter them using the **deviceTrustType** property.</span></span> <span data-ttu-id="57d02-306">Domänanslutna enheter har värdet **domänanslutna**.</span><span class="sxs-lookup"><span data-stu-id="57d02-306">Domain joined devices have a value of **Domain Joined**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57d02-307">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57d02-307">Next steps</span></span>

* [<span data-ttu-id="57d02-308">Automatisk enhetsregistrering vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="57d02-308">Automatic device registration FAQ</span></span>](active-directory-device-registration-faq.md)
* [<span data-ttu-id="57d02-309">Felsökning av automatisk registrering av domän domänanslutna datorer till Azure AD – Windows 10 och Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="57d02-309">Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)
* [<span data-ttu-id="57d02-310">Felsökning av automatisk registrering av domän domänanslutna datorer till Azure AD – Windows 10</span><span class="sxs-lookup"><span data-stu-id="57d02-310">Troubleshooting auto-registration of domain joined computers to Azure AD – non-Windows 10</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
* [<span data-ttu-id="57d02-311">Villkorlig åtkomst i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="57d02-311">Azure Active Directory conditional access</span></span>](active-directory-conditional-access-azure-portal.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
