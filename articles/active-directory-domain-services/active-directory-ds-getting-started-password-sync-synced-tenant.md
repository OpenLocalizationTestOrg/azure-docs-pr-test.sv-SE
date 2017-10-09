---
title: "Azure AD Domain Services: Aktivera lösenordssynkronisering | Microsoft Docs"
description: "Komma igång med Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: a7a6ee0f83d3d9bdaf236717efb39155a26934e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a><span data-ttu-id="16c31-103">Aktivera lösenord synkronisering tooAzure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="16c31-103">Enable password synchronization tooAzure Active Directory Domain Services</span></span>
<span data-ttu-id="16c31-104">I föregående uppgifter aktiverade du Azure Active Directory Domain Services för din Azure Active Directory-klient (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="16c31-104">In preceding tasks, you enabled Azure Active Directory Domain Services for your Azure Active Directory (Azure AD) tenant.</span></span> <span data-ttu-id="16c31-105">hello nästa uppgift är tooenable synkroniseringen av hashvärdena som krävs för NT LAN Manager (NTLM) och Kerberos-autentisering tooAzure AD DS.</span><span class="sxs-lookup"><span data-stu-id="16c31-105">hello next task is tooenable synchronization of credential hashes required for NT LAN Manager (NTLM) and Kerberos authentication tooAzure AD Domain Services.</span></span> <span data-ttu-id="16c31-106">När du har konfigurerat autentiseringsuppgifter synkronisering kan användarna logga in toohello hanterade domänen med sina företagsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="16c31-106">After you've set up credential synchronization, users can sign in toohello managed domain with their corporate credentials.</span></span>

<span data-ttu-id="16c31-107">hello stegen skiljer sig för endast molnbaserad användaren konton jämfört med användarkonton som synkroniseras från din lokala katalog med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="16c31-107">hello steps involved are different for cloud-only user accounts vs user accounts that are synchronized from your on-premises directory using Azure AD Connect.</span></span> <span data-ttu-id="16c31-108">Om Azure AD-klienten har en kombination av molnet endast användare och användare från din lokala AD och du behöver tooperform båda dessa steg.</span><span class="sxs-lookup"><span data-stu-id="16c31-108">If your Azure AD tenant has a combination of cloud only users and users from your on-premises AD, you need tooperform both steps.</span></span>

<br>

> [!div class="op_single_selector"]
> * <span data-ttu-id="16c31-109">**Endast molnbaserad användarkonton**: [synkronisera lösenord för endast molnbaserad användarkonton tooyour hanterad domän](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="16c31-109">**Cloud-only user accounts**: [Synchronize passwords for cloud-only user accounts tooyour managed domain](active-directory-ds-getting-started-password-sync.md)</span></span>
> * <span data-ttu-id="16c31-110">**Lokala användarkonton**: [synkronisera lösenord för användarkonton synkroniseras från din lokala AD tooyour hanterad domän](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span><span class="sxs-lookup"><span data-stu-id="16c31-110">**On-premises user accounts**: [Synchronize passwords for user accounts synced from your on-premises AD tooyour managed domain](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span></span>
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a><span data-ttu-id="16c31-111">Uppgift 5: aktivera lösenord synkronisering tooyour hanterad domän för användarkonton som har synkroniserats med din lokala AD</span><span class="sxs-lookup"><span data-stu-id="16c31-111">Task 5: enable password synchronization tooyour managed domain for user accounts synced with your on-premises AD</span></span>
<span data-ttu-id="16c31-112">En synkroniserad Azure AD-klient anges toosynchronize med din organisations lokala katalog med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="16c31-112">A synced Azure AD tenant is set toosynchronize with your organization's on-premises directory using Azure AD Connect.</span></span> <span data-ttu-id="16c31-113">Som standard synkroniseras inte NTLM och Kerberos-autentiseringsuppgifter hashvärden tooAzure AD Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="16c31-113">By default, Azure AD Connect does not synchronize NTLM and Kerberos credential hashes tooAzure AD.</span></span> <span data-ttu-id="16c31-114">toouse Azure AD Domain Services måste tooconfigure Azure AD Connect toosynchronize autentiseringshasherna som krävs för NTLM och Kerberos-autentisering.</span><span class="sxs-lookup"><span data-stu-id="16c31-114">toouse Azure AD Domain Services, you need tooconfigure Azure AD Connect toosynchronize credential hashes required for NTLM and Kerberos authentication.</span></span> <span data-ttu-id="16c31-115">hello följande aktivera synkroniseringen av hashvärdena för hello krävs från din lokala katalog tooyour Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="16c31-115">hello following steps enable synchronization of hello required credential hashes from your on-premises directory tooyour Azure AD tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="16c31-116">Om din organisation har användarkonton som synkroniseras från din lokala katalog, du måste aktivera synkroniseringen av NTLM och Kerberos-hashvärden i ordning toouse hello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="16c31-116">If your organization has user accounts that are synchronized from your on-premises directory, your must enable synchronization of NTLM and Kerberos hashes in order toouse hello managed domain.</span></span> <span data-ttu-id="16c31-117">Synkroniserade användarkonto är ett konto som har skapats i din lokala katalog och är synkroniserade med Azure AD Connect tooyour Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="16c31-117">A synced user account is an account that was created in your on-premises directory and is synchronized tooyour Azure AD tenant using Azure AD Connect.</span></span>
>
>

### <a name="install-or-update-azure-ad-connect"></a><span data-ttu-id="16c31-118">Installera eller uppdatera Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="16c31-118">Install or update Azure AD Connect</span></span>
<span data-ttu-id="16c31-119">Installera hello senaste rekommenderade versionen av Azure AD Connect på en domän anslutna datorn.</span><span class="sxs-lookup"><span data-stu-id="16c31-119">Install hello latest recommended release of Azure AD Connect on a domain joined computer.</span></span> <span data-ttu-id="16c31-120">Om du har en befintlig instans av Azure AD Connect-installationen måste tooupdate den toouse hello senaste versionen av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="16c31-120">If you have an existing instance of Azure AD Connect setup, you need tooupdate it toouse hello latest version of Azure AD Connect.</span></span> <span data-ttu-id="16c31-121">tooavoid kända problem/buggar som kanske redan har åtgärdats, se till att du alltid använda hello senaste versionen av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="16c31-121">tooavoid known issues/bugs that may have already been fixed, ensure you always use hello latest version of Azure AD Connect.</span></span>

<span data-ttu-id="16c31-122">**[Ladda ned Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**</span><span class="sxs-lookup"><span data-stu-id="16c31-122">**[Download Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**</span></span>

<span data-ttu-id="16c31-123">Rekommenderad version: **1.1.553.0** – publicerades 27 juni 2017.</span><span class="sxs-lookup"><span data-stu-id="16c31-123">Recommended version: **1.1.553.0** - published on June 27, 2017.</span></span>

> [!WARNING]
> <span data-ttu-id="16c31-124">Du måste installera hello senaste rekommenderade versionen av Azure AD Connect tooenable hello äldre lösenord autentiseringsuppgifter (krävs för NTLM och Kerberos-autentisering) toosynchronize tooyour Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="16c31-124">You MUST install hello latest recommended release of Azure AD Connect tooenable hello legacy password credentials (required for NTLM and Kerberos authentication) toosynchronize tooyour Azure AD tenant.</span></span> <span data-ttu-id="16c31-125">Den här funktionen är inte tillgänglig i tidigare versioner av Azure AD Connect eller med hello äldre DirSync-verktyget.</span><span class="sxs-lookup"><span data-stu-id="16c31-125">This functionality is not available in prior releases of Azure AD Connect or with hello legacy DirSync tool.</span></span>
>
>

<span data-ttu-id="16c31-126">Installationsinstruktioner för Azure AD Connect är tillgängliga i hello följande artikel – [komma igång med Azure AD Connect](../active-directory/active-directory-aadconnect.md)</span><span class="sxs-lookup"><span data-stu-id="16c31-126">Installation instructions for Azure AD Connect are available in hello following article - [Getting started with Azure AD Connect](../active-directory/active-directory-aadconnect.md)</span></span>

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-tooazure-ad"></a><span data-ttu-id="16c31-127">Aktivera synkroniseringen av NTLM och Kerberos-autentiseringsuppgifter hashvärden tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="16c31-127">Enable synchronization of NTLM and Kerberos credential hashes tooAzure AD</span></span>
<span data-ttu-id="16c31-128">Köra hello följande PowerShell-skript på varje AD-skog, tooforce fullständig Lösenordssynkronisering och aktivera alla lokala användares autentiseringsuppgifter hashvärden toosync tooyour Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="16c31-128">Execute hello following PowerShell script on each AD forest, tooforce full password synchronization, and enable all on-premises users’ credential hashes toosync tooyour Azure AD tenant.</span></span> <span data-ttu-id="16c31-129">Det här skriptet kan hello autentiseringshasherna som krävs för NTLM/Kerberos-autentisering toobe synkroniserade tooyour Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="16c31-129">This script enables hello credential hashes required for NTLM/Kerberos authentication toobe synchronized tooyour Azure AD tenant.</span></span>

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

<span data-ttu-id="16c31-130">Beroende på hello storleken på din katalog (antal användare, grupper osv), hashar tooAzure AD tar tid för synkronisering av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="16c31-130">Depending on hello size of your directory (number of users, groups etc.), synchronization of credential hashes tooAzure AD takes time.</span></span> <span data-ttu-id="16c31-131">hello lösenorden kan användas på hello Azure AD Domain Services hanterade domänen strax efter att hashvärdena hello har synkroniserat tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="16c31-131">hello passwords will be usable on hello Azure AD Domain Services managed domain shortly after hello credential hashes have synchronized tooAzure AD.</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="16c31-132">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="16c31-132">Related Content</span></span>
* [<span data-ttu-id="16c31-133">Aktivera lösenord synkronisering tooAAD Domain Services för en molnbaserad Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="16c31-133">Enable password synchronization tooAAD Domain Services for a cloud-only Azure AD directory</span></span>](active-directory-ds-getting-started-password-sync.md)
* [<span data-ttu-id="16c31-134">Administrera en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="16c31-134">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="16c31-135">Ansluta till en Windows virtuella tooan Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="16c31-135">Join a Windows virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* [<span data-ttu-id="16c31-136">Ansluta till en Red Hat Enterprise Linux virtuella tooan Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="16c31-136">Join a Red Hat Enterprise Linux virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
