---
title: aaaHow tooconfigure hybrid Azure Active Directory-anslutna enheter | Microsoft Docs
description: "Lär dig hur tooconfigure hybrid Azure Active Directory-anslutna enheter."
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
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: f97ea436eca2833d8a9843acd19e5c633bc0fc07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-hybrid-azure-active-directory-joined-devices"></a><span data-ttu-id="51056-103">Hur tooconfigure hybrid Azure Active Directory-anslutna enheter</span><span class="sxs-lookup"><span data-stu-id="51056-103">How tooconfigure hybrid Azure Active Directory joined devices</span></span>

<span data-ttu-id="51056-104">Med hantering av enheter i Azure Active Directory (Azure AD), kan du se till att dina användare har åtkomst till dina resurser från enheter som uppfyller dina krav för säkerhet och efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="51056-104">With device management in Azure Active Directory (Azure AD), you can ensure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> <span data-ttu-id="51056-105">Mer information finns i [introduktion toodevice management i Azure Active Directory](device-management-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="51056-105">For more details, see [Introduction toodevice management in Azure Active Directory](device-management-introduction.md).</span></span>

<span data-ttu-id="51056-106">Om du har en lokal Active Directory-miljö och du vill toojoin din domänanslutna enheter tooAzure AD, kan du göra detta genom att konfigurera hybrid Azure AD anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="51056-106">If you have an on-premises Active Directory environment and you want toojoin your domain-joined devices tooAzure AD, you can accomplish this by configuring hybrid Azure AD joined devices.</span></span> <span data-ttu-id="51056-107">hello-avsnittet finns hello relaterade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="51056-107">hello topic provides you with hello related steps.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="51056-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="51056-108">Before you begin</span></span>

<span data-ttu-id="51056-109">Innan du börjar konfigurera hybrid Azure AD-anslutna enheter i din miljö, bör du bekanta dig med hello stöds scenarier och hello villkor.</span><span class="sxs-lookup"><span data-stu-id="51056-109">Before you start configuring hybrid Azure AD joined devices in your environment, you should familiarize yourself with hello supported scenarios and hello constraints.</span></span>  

<span data-ttu-id="51056-110">tooimprove hello läsbarhet hello beskrivningar, i det här avsnittet använder hello följande villkor:</span><span class="sxs-lookup"><span data-stu-id="51056-110">tooimprove hello readability of hello descriptions, this topic uses hello following term:</span></span> 

- <span data-ttu-id="51056-111">**Aktuella Windows-enheter** -termen refererar toodomain-anslutna enheter som kör Windows 10 eller Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="51056-111">**Windows current devices** - This term refers toodomain-joined devices running Windows 10 or Windows Server 2016.</span></span>
- <span data-ttu-id="51056-112">**Windows-klientversioner enheter** -termen refererar tooall **stöds** domänanslutna Windows-enheter som inte är igång Windows 10 eller Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="51056-112">**Windows down-level devices** - This term refers tooall **supported** domain-joined Windows devices that are neither running Windows 10 nor Windows Server 2016.</span></span>  


### <a name="windows-current-devices"></a><span data-ttu-id="51056-113">Aktuella Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="51056-113">Windows current devices</span></span>

- <span data-ttu-id="51056-114">För enheter som kör hello Windows-operativsystemet, bör du använda Windows 10 årsdagar Update (version 1607) eller senare.</span><span class="sxs-lookup"><span data-stu-id="51056-114">For devices running hello Windows desktop operating system, we recommend using Windows 10 Anniversary Update (version 1607) or later.</span></span> 
- <span data-ttu-id="51056-115">Hej registrering av den aktuella Windows-enheter **är** i ofedererad miljöer, till exempel lösenord hash-synkronisering konfigurationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="51056-115">hello registration of Windows current devices **is** supported in non-federated environments such as password hash sync configurations.</span></span>  


### <a name="windows-down-level-devices"></a><span data-ttu-id="51056-116">Äldre Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="51056-116">Windows down-level devices</span></span>

- <span data-ttu-id="51056-117">hello följande äldre Windows-enheter stöds:</span><span class="sxs-lookup"><span data-stu-id="51056-117">hello following Windows down-level devices are supported:</span></span>
    - <span data-ttu-id="51056-118">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="51056-118">Windows 8.1</span></span>
    - <span data-ttu-id="51056-119">Windows 7</span><span class="sxs-lookup"><span data-stu-id="51056-119">Windows 7</span></span>
    - <span data-ttu-id="51056-120">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="51056-120">Windows Server 2012 R2</span></span>
    - <span data-ttu-id="51056-121">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="51056-121">Windows Server 2012</span></span>
    - <span data-ttu-id="51056-122">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="51056-122">Windows Server 2008 R2</span></span>
- <span data-ttu-id="51056-123">Hej registrering av Windows-klientversioner enheter **är** stöds i ofedererad miljöer genom sömlös enkel inloggning [Azure Active Directory sömlös enkel inloggning](https://aka.ms/hybrid/sso).</span><span class="sxs-lookup"><span data-stu-id="51056-123">hello registration of Windows down-level devices **is** supported in non-federated environments through Seamless Single Sign On [Azure Active Directory Seamless Single Sign-On](https://aka.ms/hybrid/sso).</span></span>
- <span data-ttu-id="51056-124">Hej registrering av Windows-klientversioner enheter **är inte** stöds för enheter med hjälp av centrala profiler.</span><span class="sxs-lookup"><span data-stu-id="51056-124">hello registration of Windows down-level devices **is not** supported for devices using roaming profiles.</span></span> <span data-ttu-id="51056-125">Om du beroende av centrala profiler eller inställningar, använder du Windows 10.</span><span class="sxs-lookup"><span data-stu-id="51056-125">If you are relying on roaming of profiles or settings, use Windows 10.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="51056-126">Krav</span><span class="sxs-lookup"><span data-stu-id="51056-126">Prerequisites</span></span>

<span data-ttu-id="51056-127">Innan du börjar att aktivera hybrid Azure AD anslutna enheter i din organisation, behöver du toomake till att du kör en uppdaterad version av Azure AD connect.</span><span class="sxs-lookup"><span data-stu-id="51056-127">Before you start enabling hybrid Azure AD joined devices in your organization, you need toomake sure that you are running an up-to-date version of Azure AD connect.</span></span>

<span data-ttu-id="51056-128">Azure AD Connect:</span><span class="sxs-lookup"><span data-stu-id="51056-128">Azure AD Connect:</span></span>

- <span data-ttu-id="51056-129">Behåller hello associationen mellan hello datorkonto i din lokala Active Directory (AD) och hello enhetsobjekt i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51056-129">Keeps hello association between hello computer account in your on-premises Active Directory (AD) and hello device object in Azure AD.</span></span> 
- <span data-ttu-id="51056-130">Gör andra enheter relaterade funktioner som Windows Hello för företag.</span><span class="sxs-lookup"><span data-stu-id="51056-130">Enables other device related features like Windows Hello for Business.</span></span>



## <a name="configuration-steps"></a><span data-ttu-id="51056-131">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="51056-131">Configuration steps</span></span>

<span data-ttu-id="51056-132">Du kan konfigurera hybrid Azure AD anslutna enheter för olika typer av plattformar för Windows-enheter.</span><span class="sxs-lookup"><span data-stu-id="51056-132">You can configure hybrid Azure AD joined devices for various types of Windows device platforms.</span></span> <span data-ttu-id="51056-133">Det här avsnittet innehåller steg hello som krävs för alla scenarier för typisk konfiguration.</span><span class="sxs-lookup"><span data-stu-id="51056-133">This topic includes hello required steps for all typical configuration scenarios.</span></span>  

<span data-ttu-id="51056-134">Använd följande tabell tooget en översikt över hello steg som krävs för ditt scenario hello:</span><span class="sxs-lookup"><span data-stu-id="51056-134">Use hello following table tooget an overview of hello steps that are required for your scenario:</span></span>  



| <span data-ttu-id="51056-135">Steg</span><span class="sxs-lookup"><span data-stu-id="51056-135">Steps</span></span>                                      | <span data-ttu-id="51056-136">Windows aktuella och lösenord hash-synkronisering</span><span class="sxs-lookup"><span data-stu-id="51056-136">Windows current and password hash sync</span></span> | <span data-ttu-id="51056-137">Windows aktuella och federation</span><span class="sxs-lookup"><span data-stu-id="51056-137">Windows current and federation</span></span> | <span data-ttu-id="51056-138">Windows-klientversioner</span><span class="sxs-lookup"><span data-stu-id="51056-138">Windows down-level</span></span> |
| :--                                        | :-:                                    | :-:                            | :-:                |
| <span data-ttu-id="51056-139">Steg 1: Konfigurera tjänstanslutningspunkten</span><span class="sxs-lookup"><span data-stu-id="51056-139">Step 1: Configure service connection point</span></span> | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="51056-143">Steg 2: Konfigurera utfärdande av anspråk</span><span class="sxs-lookup"><span data-stu-id="51056-143">Step 2: Setup issuance of claims</span></span>           |                                        | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="51056-146">Steg 3: Aktivera Windows 10-enheter</span><span class="sxs-lookup"><span data-stu-id="51056-146">Step 3: Enable non-Windows 10 devices</span></span>      |                                        |                                | ![Markera][1]        |
| <span data-ttu-id="51056-148">Steg 4: Distribution av kontrollen och distribution</span><span class="sxs-lookup"><span data-stu-id="51056-148">Step 4: Control deployment and rollout</span></span>     | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |
| <span data-ttu-id="51056-152">Steg 5: Kontrollera domänanslutna enheter</span><span class="sxs-lookup"><span data-stu-id="51056-152">Step 5: Verify joined devices</span></span>          | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |



## <a name="step-1-configure-service-connection-point"></a><span data-ttu-id="51056-156">Steg 1: Konfigurera tjänstanslutningspunkten</span><span class="sxs-lookup"><span data-stu-id="51056-156">Step 1: Configure service connection point</span></span>

<span data-ttu-id="51056-157">hello service anslutningsobjekt punkter (SCP) används av dina enheter under hello registrering toodiscover information för Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="51056-157">hello service connection point (SCP) object is used by your devices during hello registration toodiscover Azure AD tenant information.</span></span> <span data-ttu-id="51056-158">Hello SCP-objektet för hello hybrid Azure AD anslutna enheter måste finnas i hello namngivning kontexten konfigurationspartitionen i hello datorns skog i din lokala Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="51056-158">In your on-premises Active Directory (AD), hello SCP object for hello hybrid Azure AD joined devices must exist in hello configuration naming context partition of hello computer's forest.</span></span> <span data-ttu-id="51056-159">Det finns endast en konfigurationsnamngivningskontexten per skog.</span><span class="sxs-lookup"><span data-stu-id="51056-159">There is only one configuration naming context per forest.</span></span> <span data-ttu-id="51056-160">Hello tjänstanslutningspunkten måste finnas i alla skogar som innehåller domänanslutna datorer i en konfiguration för flera skogar Active Directory.</span><span class="sxs-lookup"><span data-stu-id="51056-160">In a multi-forest Active Directory configuration, hello service connection point must exist in all forests containing domain-joined computers.</span></span>

<span data-ttu-id="51056-161">Du kan använda hello [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) cmdlet tooretrieve hello konfigurationsnamngivningskontexten i skogen.</span><span class="sxs-lookup"><span data-stu-id="51056-161">You can use hello [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet tooretrieve hello configuration naming context of your forest.</span></span>  

<span data-ttu-id="51056-162">För en skog med hello Active Directory-domännamn *fabrikam.com*, hello konfigurationsnamngivningskontexten är:</span><span class="sxs-lookup"><span data-stu-id="51056-162">For a forest with hello Active Directory domain name *fabrikam.com*, hello configuration naming context is:</span></span>

`CN=Configuration,DC=fabrikam,DC=com`

<span data-ttu-id="51056-163">I din skog finns hello SCP-objektet för hello automatisk registrering för domänanslutna enheter här:</span><span class="sxs-lookup"><span data-stu-id="51056-163">In your forest, hello SCP object for hello auto-registration of domain-joined devices is located at:</span></span>  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

<span data-ttu-id="51056-164">Beroende på hur du har distribuerat Azure AD Connect, har hello SCP-objektet redan konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="51056-164">Depending on how you have deployed Azure AD Connect, hello SCP object may have already been configured.</span></span>
<span data-ttu-id="51056-165">Du kan verifiera att hello objektet hello och hämta hello identifiering värden med hjälp av hello följande Windows PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="51056-165">You can verify hello existence of hello object and retrieve hello discovery values using hello following Windows PowerShell script:</span></span> 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

<span data-ttu-id="51056-166">Hej **$scp. Nyckelord** utdata visar hello Azure AD-klient information, till exempel:</span><span class="sxs-lookup"><span data-stu-id="51056-166">hello **$scp.Keywords** output shows hello Azure AD tenant information, for example:</span></span>

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

<span data-ttu-id="51056-167">Om hello tjänstanslutningspunkten inte finns, kan du skapa den genom att köra hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet på Azure AD Connect-servern.</span><span class="sxs-lookup"><span data-stu-id="51056-167">If hello service connection point does not exist, you can create it by running hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet on your Azure AD Connect server.</span></span> <span data-ttu-id="51056-168">Enterprise admin-autentiseringsuppgifter är obligatoriska toorun denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="51056-168">Enterprise admin credential is required toorun this cmdlet.</span></span>  
<span data-ttu-id="51056-169">hello-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="51056-169">hello cmdlet:</span></span>

- <span data-ttu-id="51056-170">Skapar hello tjänstanslutningspunkt i Active Directory-skog för hello Azure AD Connect är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="51056-170">Creates hello service connection point in hello Active Directory forest Azure AD Connect is connected to.</span></span> 
- <span data-ttu-id="51056-171">Kräver toospecify hello `AdConnectorAccount` parameter.</span><span class="sxs-lookup"><span data-stu-id="51056-171">Requires you toospecify hello `AdConnectorAccount` parameter.</span></span> <span data-ttu-id="51056-172">Detta är hello-konto som har konfigurerats som Active Directory connector-kontot i Azure AD connect.</span><span class="sxs-lookup"><span data-stu-id="51056-172">This is hello account that is configured as Active Directory connector account in Azure AD connect.</span></span> 


<span data-ttu-id="51056-173">hello visar följande skript ett exempel för att använda hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="51056-173">hello following script shows an example for using hello cmdlet.</span></span> <span data-ttu-id="51056-174">I det här skriptet `$aadAdminCred = Get-Credential` kräver tootype ett användarnamn.</span><span class="sxs-lookup"><span data-stu-id="51056-174">In this script, `$aadAdminCred = Get-Credential` requires you tootype a user name.</span></span> <span data-ttu-id="51056-175">Du behöver tooprovide hello användarnamnet i hello användarens huvudnamn (UPN) format (`user@example.com`).</span><span class="sxs-lookup"><span data-stu-id="51056-175">You need tooprovide hello user name in hello user principal name (UPN) format (`user@example.com`).</span></span> 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

<span data-ttu-id="51056-176">Hej `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="51056-176">hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span></span>

- <span data-ttu-id="51056-177">Använder hello Active Directory PowerShell-modul, som förlitar sig på Active Directory-webbtjänster som körs på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="51056-177">Uses hello Active Directory PowerShell module, which relies on Active Directory Web Services running on a domain controller.</span></span> <span data-ttu-id="51056-178">Active Directory Web Services fungerar på domänkontrollanter som kör Windows Server 2008 R2 och senare.</span><span class="sxs-lookup"><span data-stu-id="51056-178">Active Directory Web Services is supported on domain controllers running Windows Server 2008 R2 and later.</span></span>
- <span data-ttu-id="51056-179">Stöds endast av hello **MSOnline PowerShell Modulversion 1.1.166.0**.</span><span class="sxs-lookup"><span data-stu-id="51056-179">Is only supported by hello **MSOnline PowerShell module version 1.1.166.0**.</span></span> <span data-ttu-id="51056-180">toodownload den här modulen används [länk](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span><span class="sxs-lookup"><span data-stu-id="51056-180">toodownload this module, use this [link](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span></span>   

<span data-ttu-id="51056-181">Använd hello skriptet nedan toocreate hello tjänstanslutningspunkten för domänkontrollanter som kör Windows Server 2008 eller tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="51056-181">For domain controllers running Windows Server 2008 or earlier versions, use hello script below toocreate hello service connection point.</span></span>

<span data-ttu-id="51056-182">Du bör använda hello följande skript toocreate hello tjänstanslutningspunkten i varje skog där det finns datorer i en konfiguration för flera skogar:</span><span class="sxs-lookup"><span data-stu-id="51056-182">In a multi-forest configuration, you should use hello following script toocreate hello service connection point in each forest where computers exist:</span></span>
 
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


## <a name="step-2-setup-issuance-of-claims"></a><span data-ttu-id="51056-183">Steg 2: Konfigurera utfärdande av anspråk</span><span class="sxs-lookup"><span data-stu-id="51056-183">Step 2: Setup issuance of claims</span></span>

<span data-ttu-id="51056-184">I en federerad Azure AD-konfiguration enheter förlitar sig på Active Directory Federation Services (AD FS) eller 3 part lokalt federation service tooauthenticate tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="51056-184">In a federated Azure AD configuration, devices rely on Active Directory Federation Services (AD FS) or a 3rd party on-premises federation service tooauthenticate tooAzure AD.</span></span> <span data-ttu-id="51056-185">Enheter autentiserar tooget en åtkomst-token tooregister mot hello Azure Active Directory Device Registration Service (DRS Azure).</span><span class="sxs-lookup"><span data-stu-id="51056-185">Devices authenticate tooget an access token tooregister against hello Azure Active Directory Device Registration Service (Azure DRS).</span></span>

<span data-ttu-id="51056-186">Windows aktuella enheter autentiseras med integrerad Windows-autentisering tooan aktiv WS-Trust slutpunkt (1.3 eller 2005 versioner) hos hello lokala federationstjänsten.</span><span class="sxs-lookup"><span data-stu-id="51056-186">Windows current devices authenticate using Integrated Windows Authentication tooan active WS-Trust endpoint (either 1.3 or 2005 versions) hosted by hello on-premises federation service.</span></span>

> [!NOTE]
> <span data-ttu-id="51056-187">När du använder AD FS, antingen **adfs/services/trust/13/windowstransport** eller **adfs/services/trust/2005/windowstransport** måste vara aktiverat.</span><span class="sxs-lookup"><span data-stu-id="51056-187">When using AD FS, either **adfs/services/trust/13/windowstransport** or **adfs/services/trust/2005/windowstransport** must be enabled.</span></span> <span data-ttu-id="51056-188">Om du använder hello webbproxy för autentisering kan också se till att den här slutpunkten har publicerats via hello proxy.</span><span class="sxs-lookup"><span data-stu-id="51056-188">If you are using hello Web Authentication Proxy, also ensure that this endpoint is published through hello proxy.</span></span> <span data-ttu-id="51056-189">Du kan se vilka slutpunkter aktiveras via hello AD FS-hanteringskonsol under **Service > slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="51056-189">You can see what end-points are enabled through hello AD FS management console under **Service > Endpoints**.</span></span>
>
><span data-ttu-id="51056-190">Om du inte har AD FS som federationstjänsten lokalt, följ hello instruktionerna för din leverantör toomake att de stöder WS-Trust 1.3 eller 2005 slutpunkter och dessa publiceras via hello Metadata Exchange-fil (MEX).</span><span class="sxs-lookup"><span data-stu-id="51056-190">If you don’t have AD FS as your on-premises federation service, follow hello instructions of your vendor toomake sure they support WS-Trust 1.3 or 2005 end-points and that these are published through hello Metadata Exchange file (MEX).</span></span>

<span data-ttu-id="51056-191">hello måste följande anspråk finnas i hello-token som tas emot av Azure DRS för toocomplete för registrering av enheten.</span><span class="sxs-lookup"><span data-stu-id="51056-191">hello following claims must exist in hello token received by Azure DRS for device registration toocomplete.</span></span> <span data-ttu-id="51056-192">Azure DRS skapar ett enhetsobjekt i Azure AD med en del av den här informationen som sedan används av Azure AD Connect tooassociate hello nyskapad enhetsobjekt med hello datorn kontot lokalt.</span><span class="sxs-lookup"><span data-stu-id="51056-192">Azure DRS will create a device object in Azure AD with some of this information which is then used by Azure AD Connect tooassociate hello newly created device object with hello computer account on-premises.</span></span>

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

<span data-ttu-id="51056-193">Om du har mer än ett verifierat domännamn måste tooprovide hello följande anspråk för datorer:</span><span class="sxs-lookup"><span data-stu-id="51056-193">If you have more than one verified domain name, you need tooprovide hello following claim for computers:</span></span>

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

<span data-ttu-id="51056-194">Om du redan utfärdar ett ImmutableID anspråk (t.ex. Alternativt inloggnings-ID) måste tooprovide ett motsvarande anspråk för datorer:</span><span class="sxs-lookup"><span data-stu-id="51056-194">If you are already issuing an ImmutableID claim (e.g., alternate login ID) you need tooprovide one corresponding claim for computers:</span></span>

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

<span data-ttu-id="51056-195">I följande avsnitt hello, hittar du information om:</span><span class="sxs-lookup"><span data-stu-id="51056-195">In hello following sections, you find information about:</span></span>
 
- <span data-ttu-id="51056-196">hello värden varje anspråk ska ha</span><span class="sxs-lookup"><span data-stu-id="51056-196">hello values each claim should have</span></span>
- <span data-ttu-id="51056-197">Hur en definition skulle se ut i AD FS</span><span class="sxs-lookup"><span data-stu-id="51056-197">How a definition would look like in AD FS</span></span>

<span data-ttu-id="51056-198">hello definition hjälper dig att tooverify om hello värden finns eller om du behöver toocreate dem.</span><span class="sxs-lookup"><span data-stu-id="51056-198">hello definition helps you tooverify whether hello values are present or if you need toocreate them.</span></span>

> [!NOTE]
> <span data-ttu-id="51056-199">Om du inte använder AD FS för federationsservern lokalt, följer du leverantörens instruktioner toocreate hello lämplig konfiguration tooissue dessa anspråk.</span><span class="sxs-lookup"><span data-stu-id="51056-199">If you don’t use AD FS for your on-premises federation server, follow your vendor's instructions toocreate hello appropriate configuration tooissue these claims.</span></span>

### <a name="issue-account-type-claim"></a><span data-ttu-id="51056-200">Problemet konto typen anspråk</span><span class="sxs-lookup"><span data-stu-id="51056-200">Issue account type claim</span></span>

<span data-ttu-id="51056-201">**`http://schemas.microsoft.com/ws/2012/01/accounttype`**-Detta anspråk måste innehålla ett värde av **DJ**, som identifierar hello enheten som en domänansluten dator.</span><span class="sxs-lookup"><span data-stu-id="51056-201">**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - This claim must contain a value of **DJ**, which identifies hello device as a domain-joined computer.</span></span> <span data-ttu-id="51056-202">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="51056-202">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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

### <a name="issue-objectguid-of-hello-computer-account-on-premises"></a><span data-ttu-id="51056-203">Utfärda objectGUID av hello datorn kontot Lokal</span><span class="sxs-lookup"><span data-stu-id="51056-203">Issue objectGUID of hello computer account on-premises</span></span>

<span data-ttu-id="51056-204">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**-Detta anspråk måste innehålla hello **objectGUID** värdet för hello lokalt datorkonto.</span><span class="sxs-lookup"><span data-stu-id="51056-204">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - This claim must contain hello **objectGUID** value of hello on-premises computer account.</span></span> <span data-ttu-id="51056-205">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="51056-205">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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
 
### <a name="issue-objectsid-of-hello-computer-account-on-premises"></a><span data-ttu-id="51056-206">Utfärda objectSID av hello datorn kontot Lokal</span><span class="sxs-lookup"><span data-stu-id="51056-206">Issue objectSID of hello computer account on-premises</span></span>

<span data-ttu-id="51056-207">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**-Detta anspråk måste innehålla hello hello **objectSid** värdet för hello lokalt datorkonto.</span><span class="sxs-lookup"><span data-stu-id="51056-207">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - This claim must contain hello hello **objectSid** value of hello on-premises computer account.</span></span> <span data-ttu-id="51056-208">I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="51056-208">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a><span data-ttu-id="51056-209">Utfärda issuerID för datorn när flera verifierat domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="51056-209">Issue issuerID for computer when multiple verified domain names in Azure AD</span></span>

<span data-ttu-id="51056-210">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**-Detta anspråk måste innehålla hello identifierare URI (Uniform Resource) för någon av hello verifierat domännamn som ansluter med hello lokalt federation (AD FS eller 3 part) utfärdande hello tjänsttoken.</span><span class="sxs-lookup"><span data-stu-id="51056-210">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - This claim must contain hello Uniform Resource Identifier (URI) of any of hello verified domain names that connect with hello on-premises federation service (AD FS or 3rd party) issuing hello token.</span></span> <span data-ttu-id="51056-211">I AD FS kan du lägga till regler för utfärdandetransformering som ser ut som om hello viktiga nedan i den specifika ordning efter hello de som ovan.</span><span class="sxs-lookup"><span data-stu-id="51056-211">In AD FS, you can add issuance transform rules that look like hello ones below in that specific order after hello ones above.</span></span> <span data-ttu-id="51056-212">Observera att en regel tooexplicitly problemet hello regel för användare som är nödvändiga.</span><span class="sxs-lookup"><span data-stu-id="51056-212">Please note that one rule tooexplicitly issue hello rule for users is necessary.</span></span> <span data-ttu-id="51056-213">En första regel som identifierar användare eller datorautentisering läggs till i hello reglerna nedan.</span><span class="sxs-lookup"><span data-stu-id="51056-213">In hello rules below, a first rule identifying user vs. computer authentication is added.</span></span>

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


<span data-ttu-id="51056-214">I hello anspråket ovan</span><span class="sxs-lookup"><span data-stu-id="51056-214">In hello claim above,</span></span>

- <span data-ttu-id="51056-215">`$<domain>`är hello AD FS-tjänstens URL</span><span class="sxs-lookup"><span data-stu-id="51056-215">`$<domain>` is hello AD FS service URL</span></span>
- <span data-ttu-id="51056-216">`<verified-domain-name>`en platshållare som du behöver tooreplace med en av dina verifierat domännamn i Azure AD</span><span class="sxs-lookup"><span data-stu-id="51056-216">`<verified-domain-name>` is a placeholder you need tooreplace with one of your verified domain names in Azure AD</span></span>



<span data-ttu-id="51056-217">Mer information om verifierat domännamn finns [lägga till en anpassad domän namn tooAzure Active Directory](active-directory-add-domain.md).</span><span class="sxs-lookup"><span data-stu-id="51056-217">For more details about verified domain names, see [Add a custom domain name tooAzure Active Directory](active-directory-add-domain.md).</span></span>  
<span data-ttu-id="51056-218">tooget en lista över verifierade företagets-domäner som du kan använda hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="51056-218">tooget a list of your verified company domains, you can use hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span></span> 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a><span data-ttu-id="51056-220">Utfärda ImmutableID för datorn när en användare finns (t.ex. Alternativt inloggnings-ID har angetts)</span><span class="sxs-lookup"><span data-stu-id="51056-220">Issue ImmutableID for computer when one for users exist (e.g. alternate login ID is set)</span></span>

<span data-ttu-id="51056-221">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**-Detta anspråk måste innehålla ett giltigt värde för datorer.</span><span class="sxs-lookup"><span data-stu-id="51056-221">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - This claim must contain a valid value for computers.</span></span> <span data-ttu-id="51056-222">I AD FS kan du skapa en regel för omvandling av utfärdande som följer:</span><span class="sxs-lookup"><span data-stu-id="51056-222">In AD FS, you can create an issuance transform rule as follows:</span></span>

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

### <a name="helper-script-toocreate-hello-ad-fs-issuance-transform-rules"></a><span data-ttu-id="51056-223">Helper skriptet toocreate hello AD FS regler för utfärdandetransformering</span><span class="sxs-lookup"><span data-stu-id="51056-223">Helper script toocreate hello AD FS issuance transform rules</span></span>

<span data-ttu-id="51056-224">hello hjälper följande skript dig med hello utfärdande hello skapa regler som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="51056-224">hello following script helps you with hello creation of hello issuance transform rules described above.</span></span>

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

### <a name="remarks"></a><span data-ttu-id="51056-225">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="51056-225">Remarks</span></span> 

- <span data-ttu-id="51056-226">Skriptet lägger till hello toohello befintliga regler.</span><span class="sxs-lookup"><span data-stu-id="51056-226">This script appends hello rules toohello existing rules.</span></span> <span data-ttu-id="51056-227">Körs inte hello skriptet skulle två gånger eftersom hello uppsättning regler läggas till två gånger.</span><span class="sxs-lookup"><span data-stu-id="51056-227">Do not run hello script twice because hello set of rules would be added twice.</span></span> <span data-ttu-id="51056-228">Se till att motsvarande inte finns några regler för dessa anspråk (under hello motsvarande) innan du kör skriptet hello igen.</span><span class="sxs-lookup"><span data-stu-id="51056-228">Make sure that no corresponding rules exist for these claims (under hello corresponding conditions) before running hello script again.</span></span>

- <span data-ttu-id="51056-229">Om du har flera verifierat domännamn (som visas i hello Azure AD-portalen eller via hello Get-MsolDomains cmdlet), ange hello-värde **$multipleVerifiedDomainNames** i hello skript för**$true**.</span><span class="sxs-lookup"><span data-stu-id="51056-229">If you have multiple verified domain names (as shown in hello Azure AD portal or via hello Get-MsolDomains cmdlet), set hello value of **$multipleVerifiedDomainNames** in hello script too**$true**.</span></span> <span data-ttu-id="51056-230">Kontrollera också att du tar bort alla befintliga issuerid anspråk som kan ha skapats av Azure AD Connect eller via andra sätt.</span><span class="sxs-lookup"><span data-stu-id="51056-230">Also make sure that you remove any existing issuerid claim that might have been created by Azure AD Connect or via other means.</span></span> <span data-ttu-id="51056-231">Här är ett exempel på den här regeln:</span><span class="sxs-lookup"><span data-stu-id="51056-231">Here is an example for this rule:</span></span>


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- <span data-ttu-id="51056-232">Om du redan har skickat en **ImmutableID** anspråk för användarkonton genom att ange hello värdet för **$immutableIDAlreadyIssuedforUsers** i hello skript för**$true**.</span><span class="sxs-lookup"><span data-stu-id="51056-232">If you have already issued an **ImmutableID** claim  for user accounts, set hello value of **$immutableIDAlreadyIssuedforUsers** in hello script too**$true**.</span></span>

## <a name="step-3-enable-windows-down-level-devices"></a><span data-ttu-id="51056-233">Steg 3: Aktivera äldre Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="51056-233">Step 3: Enable Windows down-level devices</span></span>

<span data-ttu-id="51056-234">Om vissa enheter domänanslutna Windows äldre enheter, måste du:</span><span class="sxs-lookup"><span data-stu-id="51056-234">If some of your domain-joined devices Windows down-level devices, you need to:</span></span>

- <span data-ttu-id="51056-235">Ange en princip i Azure AD tooenable användare tooregister enheter.</span><span class="sxs-lookup"><span data-stu-id="51056-235">Set a policy in Azure AD tooenable users tooregister devices.</span></span>
 
- <span data-ttu-id="51056-236">Konfigurera din lokala federation service tooissue anspråk toosupport **Windows IWA (Integrated Authentication)** för registrering av enheten.</span><span class="sxs-lookup"><span data-stu-id="51056-236">Configure your on-premises federation service tooissue claims toosupport **Integrated Windows Authentication (IWA)** for device registration.</span></span>
 
- <span data-ttu-id="51056-237">Lägg till hello Azure AD enheten autentisering slutpunkt toohello lokalt intranät zoner tooavoid certifikat uppmanar vid autentisering hello enhet.</span><span class="sxs-lookup"><span data-stu-id="51056-237">Add hello Azure AD device authentication end-point toohello local Intranet zones tooavoid certificate prompts when authenticating hello device.</span></span>

### <a name="set-policy-in-azure-ad-tooenable-users-tooregister-devices"></a><span data-ttu-id="51056-238">Ange princip för i Azure AD tooenable användare tooregister enheter</span><span class="sxs-lookup"><span data-stu-id="51056-238">Set policy in Azure AD tooenable users tooregister devices</span></span>

<span data-ttu-id="51056-239">tooregister Windows äldre enheter behöver du toomake till att hello inställningen tooallow användare tooregister enheter i Azure AD har angetts.</span><span class="sxs-lookup"><span data-stu-id="51056-239">tooregister Windows down-level devices, you need toomake sure that hello setting tooallow users tooregister devices in Azure AD is set.</span></span> <span data-ttu-id="51056-240">Du hittar den här inställningen under i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="51056-240">In hello Azure portal, you can find this setting under:</span></span>

`Azure Active Directory > Users and groups > Device settings`
    
<span data-ttu-id="51056-241">hello följande princip måste anges för**alla**: **användarna kan registrera sina enheter med Azure AD**</span><span class="sxs-lookup"><span data-stu-id="51056-241">hello following policy must be set too**All**: **Users may register their devices with Azure AD**</span></span>

![Registrera enheter](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a><span data-ttu-id="51056-243">Konfigurera lokala federationstjänsten</span><span class="sxs-lookup"><span data-stu-id="51056-243">Configure on-premises federation service</span></span> 

<span data-ttu-id="51056-244">Federationstjänsten lokalt måste ha stöd för utfärdande hello **authenticationmehod** och **wiaormultiauthn** anspråk när du tar emot en autentisering begära toohello Azure AD förlitande part innehar en resouce_params parameter med ett kodat värde som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="51056-244">Your on-premises federation service must support issuing hello **authenticationmehod** and **wiaormultiauthn** claims when receiving an authentication request toohello Azure AD relying party holding a resouce_params parameter with an encoded value as shown below:</span></span>

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

<span data-ttu-id="51056-245">När en sådan begäran kommer hello lokalt federationstjänsten måste autentisera hello-användare som använder integrerad Windows-autentisering och vid lyckades, utfärda hello följande två anspråk:</span><span class="sxs-lookup"><span data-stu-id="51056-245">When such a request comes, hello on-premises federation service must authenticate hello user using Integrated Windows Authentication and upon success, it must issue hello following two claims:</span></span>

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

<span data-ttu-id="51056-246">I AD FS, du måste lägga till en regel för omvandling av utfärdande som överför via hello autentiseringsmetod.</span><span class="sxs-lookup"><span data-stu-id="51056-246">In AD FS, you must add an issuance transform rule that passes-through hello authentication method.</span></span>  

<span data-ttu-id="51056-247">**tooadd regeln:**</span><span class="sxs-lookup"><span data-stu-id="51056-247">**tooadd this rule:**</span></span>

1. <span data-ttu-id="51056-248">I hello AD FS-hanteringskonsolen gå för`AD FS > Trust Relationships > Relying Party Trusts`.</span><span class="sxs-lookup"><span data-stu-id="51056-248">In hello AD FS management console, go too`AD FS > Trust Relationships > Relying Party Trusts`.</span></span>
2. <span data-ttu-id="51056-249">Högerklicka på hello Identitetsplattformen för Microsoft Office 365 förlitande part förtroende-objekt och välj sedan **redigera Anspråksregler**.</span><span class="sxs-lookup"><span data-stu-id="51056-249">Right-click hello Microsoft Office 365 Identity Platform relying party trust object, and then select **Edit Claim Rules**.</span></span>
3. <span data-ttu-id="51056-250">På hello **regler för Utfärdandetransformering** väljer **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="51056-250">On hello **Issuance Transform Rules** tab, select **Add Rule**.</span></span>
4. <span data-ttu-id="51056-251">I hello **anspråksregel** mall-listan, Välj **skicka anspråk med en anpassad regel**.</span><span class="sxs-lookup"><span data-stu-id="51056-251">In hello **Claim rule** template list, select **Send Claims Using a Custom Rule**.</span></span>
5. <span data-ttu-id="51056-252">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="51056-252">Select **Next**.</span></span>
6. <span data-ttu-id="51056-253">I hello **Regelnamn för anspråk** skriver **Auth metoden Anspråksregel**.</span><span class="sxs-lookup"><span data-stu-id="51056-253">In hello **Claim rule name** box, type **Auth Method Claim Rule**.</span></span>
7. <span data-ttu-id="51056-254">I hello **anspråksregel** rutan, typen hello följande regel:</span><span class="sxs-lookup"><span data-stu-id="51056-254">In hello **Claim rule** box, type hello following rule:</span></span>

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. <span data-ttu-id="51056-255">Skriv hello PowerShell-kommandot nedan på federationsservern när du ersätter  **\<RPObjectName\>**  med hello förlitande part objektnamn för din Azure AD förlitande part förtroende-objekt.</span><span class="sxs-lookup"><span data-stu-id="51056-255">On your federation server, type hello PowerShell command below after replacing **\<RPObjectName\>** with hello relying party object name for your Azure AD relying party trust object.</span></span> <span data-ttu-id="51056-256">Det här objektet vanligen namnet **Identitetsplattformen för Microsoft Office 365**.</span><span class="sxs-lookup"><span data-stu-id="51056-256">This object usually is named **Microsoft Office 365 Identity Platform**.</span></span>
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-hello-azure-ad-device-authentication-end-point-toohello-local-intranet-zones"></a><span data-ttu-id="51056-257">Lägg till hello Azure AD device autentisering slutpunkt toohello lokalt intranät zoner</span><span class="sxs-lookup"><span data-stu-id="51056-257">Add hello Azure AD device authentication end-point toohello Local Intranet zones</span></span>

<span data-ttu-id="51056-258">tooavoid certifikat uppmanar när användare registrera enheter autentiseras tooAzure AD du skicka en princip tooyour domänanslutna enheter tooadd hello följande URL toohello lokalt intranät i Internet Explorer:</span><span class="sxs-lookup"><span data-stu-id="51056-258">tooavoid certificate prompts when users in register devices authenticate tooAzure AD you can push a policy tooyour domain-joined devices tooadd hello following URL toohello Local Intranet zone in Internet Explorer:</span></span>

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a><span data-ttu-id="51056-259">Steg 4: Distribution av kontrollen och distribution</span><span class="sxs-lookup"><span data-stu-id="51056-259">Step 4: Control deployment and rollout</span></span>

<span data-ttu-id="51056-260">När du har slutfört steg hello som krävs för är domänanslutna enheter klar tooautomatically anslutning till Azure AD:</span><span class="sxs-lookup"><span data-stu-id="51056-260">When you have completed hello required steps, domain-joined devices are ready tooautomatically join Azure AD:</span></span>

- <span data-ttu-id="51056-261">Alla domänanslutna enheter som kör Windows 10 årsdagar Update och Windows Server 2016 registrera med Azure AD på enheten startas om automatiskt eller användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="51056-261">All domain-joined devices running Windows 10 Anniversary Update and Windows Server 2016 automatically register with Azure AD at device restart or user sign-in.</span></span> 

- <span data-ttu-id="51056-262">Att registrera nya enheter med Azure AD när hello enheten startas om efter hello domän join-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="51056-262">New devices register with Azure AD when hello device restarts after hello domain join operation is completed.</span></span>

- <span data-ttu-id="51056-263">Enheter som fanns tidigare Azure AD som registrerade (t.ex, för Intune) övergång för ”*domänanslutna, AAD registrerade*”; men tar det lite tid för den här processen toocomplete på alla enheter på grund av toohello normala flödet av domän- och användaraktivitet.</span><span class="sxs-lookup"><span data-stu-id="51056-263">Devices that were previously Azure AD registered (for example, for Intune) transition too“*Domain Joined, AAD Registered*”; however it takes some time for this process toocomplete across all devices due toohello normal flow of domain and user activity.</span></span>

### <a name="remarks"></a><span data-ttu-id="51056-264">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="51056-264">Remarks</span></span>

- <span data-ttu-id="51056-265">Du kan använda en Grupprincip objektet toocontrol hello distribution av automatisk registrering av Windows 10 och Windows Server 2016 domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="51056-265">You can use a Group Policy object toocontrol hello rollout of automatic registration of Windows 10 and Windows Server 2016 domain-joined computers.</span></span>

- <span data-ttu-id="51056-266">Windows 10 November 2015 Update kopplas automatiskt med Azure AD **endast** om hello grupprincipobjektet för distribution har angetts.</span><span class="sxs-lookup"><span data-stu-id="51056-266">Windows 10 November 2015 Update automatically joins with Azure AD **only** if hello rollout Group Policy object is set.</span></span>

- <span data-ttu-id="51056-267">toorollout med äldre Windows-datorer som du kan distribuera en [Windows Installer-paketet](#windows-installer-packages-for-non-windows-10-computers) toocomputers som du väljer.</span><span class="sxs-lookup"><span data-stu-id="51056-267">toorollout of Windows down-level computers, you can deploy a [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) toocomputers that you select.</span></span>

- <span data-ttu-id="51056-268">Om du överför hello gruppolicy objekt tooWindows 8.1 domänanslutna enheter görs en koppling; men det rekommenderas att du använder hello [Windows Installer-paketet](#windows-installer-packages-for-non-windows-10-computers) toojoin alla dina Windows äldre enheter.</span><span class="sxs-lookup"><span data-stu-id="51056-268">If you push hello Group Policy object tooWindows 8.1 domain-joined devices, a join is attempted; however it is recommended that you use hello [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) toojoin all your Windows down-level devices.</span></span> 

### <a name="create-a-group-policy-object"></a><span data-ttu-id="51056-269">Skapa ett grupprincipobjekt</span><span class="sxs-lookup"><span data-stu-id="51056-269">Create a Group Policy object</span></span> 

<span data-ttu-id="51056-270">toocontrol hello distribution av aktuella Windows-datorer, bör du distribuera hello **registrera domänanslutna datorer som enheter** gruppolicy objekt toohello enheter du vill tooregister.</span><span class="sxs-lookup"><span data-stu-id="51056-270">toocontrol hello rollout of Windows current computers, you should deploy hello **Register domain-joined computers as devices** Group Policy object toohello devices you want tooregister.</span></span> <span data-ttu-id="51056-271">Du kan till exempel distribuera princip hello tooan organisationsenhet eller tooa säkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="51056-271">For example, you can deploy hello policy tooan organizational unit or tooa security group.</span></span>

<span data-ttu-id="51056-272">**tooset hello princip:**</span><span class="sxs-lookup"><span data-stu-id="51056-272">**tooset hello policy:**</span></span>

1. <span data-ttu-id="51056-273">Öppna **Serverhanteraren**, och sedan gå för`Tools > Group Policy Management`.</span><span class="sxs-lookup"><span data-stu-id="51056-273">Open **Server Manager**, and then go too`Tools > Group Policy Management`.</span></span>
2. <span data-ttu-id="51056-274">Gå toohello domännod som motsvarar toohello domänen där du vill tooactivate automatisk registrering av aktuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="51056-274">Go toohello domain node that corresponds toohello domain where you want tooactivate auto-registration of Windows current computers.</span></span>
3. <span data-ttu-id="51056-275">Högerklicka på **grupprincipobjekt**, och välj sedan **ny**.</span><span class="sxs-lookup"><span data-stu-id="51056-275">Right-click **Group Policy Objects**, and then select **New**.</span></span>
4. <span data-ttu-id="51056-276">Ange ett namn för grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="51056-276">Type a name for your Group Policy object.</span></span> <span data-ttu-id="51056-277">Till exempel * Hybrid Azure AD-koppling.</span><span class="sxs-lookup"><span data-stu-id="51056-277">For example, *Hybrid Azure AD join.</span></span> 
5. <span data-ttu-id="51056-278">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="51056-278">Click **OK**.</span></span>
6. <span data-ttu-id="51056-279">Högerklicka på din nya grupprincipobjekt och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="51056-279">Right-click your new Group Policy object, and then select **Edit**.</span></span>
7. <span data-ttu-id="51056-280">Gå för**Datorkonfiguration** > **principer** > **Administrationsmallar** > **Windows Komponenter** > **Enhetsregistrering**.</span><span class="sxs-lookup"><span data-stu-id="51056-280">Go too**Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Device Registration**.</span></span> 
8. <span data-ttu-id="51056-281">Högerklicka på **registrera domänanslutna datorer som enheter**, och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="51056-281">Right-click **Register domain-joined computers as devices**, and then select **Edit**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="51056-282">Den här mallen har ändrats från tidigare versioner av hello hanteringskonsolen för Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="51056-282">This Group Policy template has been renamed from earlier versions of hello Group Policy Management console.</span></span> <span data-ttu-id="51056-283">Om du använder en tidigare version av hello-konsolen, gå för`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span><span class="sxs-lookup"><span data-stu-id="51056-283">If you are using an earlier version of hello console, go too`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span></span> 

7. <span data-ttu-id="51056-284">Välj **aktiverad**, och klicka sedan på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="51056-284">Select **Enabled**, and then click **Apply**.</span></span>
8. <span data-ttu-id="51056-285">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="51056-285">Click **OK**.</span></span>
9. <span data-ttu-id="51056-286">Länken hello gruppolicy objekt tooa önskad plats.</span><span class="sxs-lookup"><span data-stu-id="51056-286">Link hello Group Policy object tooa location of your choice.</span></span> <span data-ttu-id="51056-287">T.ex, kan du länka den tooa organisationsenheten.</span><span class="sxs-lookup"><span data-stu-id="51056-287">For example, you can link it tooa specific organizational unit.</span></span> <span data-ttu-id="51056-288">Dessutom kan du länka det tooa säkerhetsgrupp för datorer som automatiskt att ansluta med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51056-288">You also could link it tooa specific security group of computers that automatically join with Azure AD.</span></span> <span data-ttu-id="51056-289">tooset principen för alla domänanslutna Windows 10 och Windows Server 2016 datorer i din organisation, länk hello gruppolicy objekt toohello domän.</span><span class="sxs-lookup"><span data-stu-id="51056-289">tooset this policy for all domain-joined Windows 10 and Windows Server 2016 computers in your organization, link hello Group Policy object toohello domain.</span></span>

### <a name="windows-installer-packages-for-non-windows-10-computers"></a><span data-ttu-id="51056-290">Windows Installer-paket för Windows 10-datorer</span><span class="sxs-lookup"><span data-stu-id="51056-290">Windows Installer packages for non-Windows 10 computers</span></span>

<span data-ttu-id="51056-291">toojoin domänanslutna Windows-klientversioner datorer i en federerad miljö kan du hämta och installera de här Windows Installer-paketet (.msi) från Download Center på hello [Microsoft till arbetsplatsen för Windows 10 - datorer](https://www.microsoft.com/en-us/download/details.aspx?id=53554)sidan.</span><span class="sxs-lookup"><span data-stu-id="51056-291">toojoin domain-joined Windows down-level computers in a federated environment, you can download and install these Windows Installer package (.msi) from Download Center at hello [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) page.</span></span>

<span data-ttu-id="51056-292">Du kan distribuera hello paketet med hjälp av ett program distributionssystem som System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="51056-292">You can deploy hello package by using a software distribution system like System Center Configuration Manager.</span></span> <span data-ttu-id="51056-293">hello paketet stöder hello standard tyst installation alternativ med hello *tyst* parameter.</span><span class="sxs-lookup"><span data-stu-id="51056-293">hello package supports hello standard silent install options with hello *quiet* parameter.</span></span> <span data-ttu-id="51056-294">System Center Configuration Manager Current Branch erbjuder ytterligare fördelar jämfört med tidigare versioner som hello möjlighet tootrack slutförts registreringar.</span><span class="sxs-lookup"><span data-stu-id="51056-294">System Center Configuration Manager Current Branch offers additional benefits from earlier versions, like hello ability tootrack completed registrations.</span></span> <span data-ttu-id="51056-295">Mer information finns i [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="51056-295">For more information, see [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span></span>

<span data-ttu-id="51056-296">hello installationsprogrammet skapar en schemalagd aktivitet på hello-system som körs i hello användarens kontext.</span><span class="sxs-lookup"><span data-stu-id="51056-296">hello installer creates a scheduled task on hello system that runs in hello user’s context.</span></span> <span data-ttu-id="51056-297">hello aktiviteten utlöses när hello användare loggar in tooWindows.</span><span class="sxs-lookup"><span data-stu-id="51056-297">hello task is triggered when hello user signs in tooWindows.</span></span> <span data-ttu-id="51056-298">hello-aktiviteten ansluter tyst hello-enhet med Azure AD med hello autentiseringsuppgifter efter autentiseras med integrerad Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="51056-298">hello task silently joins hello device with Azure AD with hello user credentials after authenticating using Integrated Windows Authentication.</span></span> <span data-ttu-id="51056-299">toosee hello schemalagd aktivitet, i hello enhet, gå för**Microsoft** > **Arbetsplatsanslutning**, och sedan gå toohello Schemaläggaren bibliotek.</span><span class="sxs-lookup"><span data-stu-id="51056-299">toosee hello scheduled task, in hello device, go too**Microsoft** > **Workplace Join**, and then go toohello Task Scheduler library.</span></span>

## <a name="step-5-verify-joined-devices"></a><span data-ttu-id="51056-300">Steg 5: Kontrollera domänanslutna enheter</span><span class="sxs-lookup"><span data-stu-id="51056-300">Step 5: Verify joined devices</span></span>

<span data-ttu-id="51056-301">Du kan kontrollera lyckade domänanslutna enheter i organisationen med hjälp av hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet i hello [Azure Active Directory PowerShell-modulen](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="51056-301">You can check successful joined devices in your organization by using hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in hello [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span>

<span data-ttu-id="51056-302">hello utdata från den här cmdleten visar enheter som registreras och kopplas till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51056-302">hello output of this cmdlet shows devices that are registered and joined with Azure AD.</span></span> <span data-ttu-id="51056-303">tooget alla enheter använda hello **-alla** parameter och filtrera dem med hjälp av hello **deviceTrustType** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="51056-303">tooget all devices, use hello **-All** parameter, and then filter them using hello **deviceTrustType** property.</span></span> <span data-ttu-id="51056-304">Domänanslutna enheter har värdet **domänanslutna**.</span><span class="sxs-lookup"><span data-stu-id="51056-304">Domain joined devices have a value of **Domain Joined**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51056-305">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51056-305">Next steps</span></span>

* [<span data-ttu-id="51056-306">Introduktion toodevice management i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="51056-306">Introduction toodevice management in Azure Active Directory</span></span>](device-management-introduction.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
