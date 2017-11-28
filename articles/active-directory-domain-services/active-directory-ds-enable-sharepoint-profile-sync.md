---
title: "Azure Active Directory Domain Services: Aktivera stöd för användarprofil för SharePoint service | Microsoft Docs"
description: "Konfigurera Azure Active Directory Domain Services hanterade domäner för att stödja profilsynkronisering för SharePoint Server"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: c3c923b5c9cd652f0c5b0ec98c1cda740f180122
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a><span data-ttu-id="5c688-103">Konfigurera en hanterad domän för att stödja profilsynkronisering för SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="5c688-103">Configure a managed domain to support profile synchronization for SharePoint Server</span></span>
<span data-ttu-id="5c688-104">SharePoint Server innehåller en tjänsten användarprofil som används för synkronisering av användarprofiler.</span><span class="sxs-lookup"><span data-stu-id="5c688-104">SharePoint Server includes a User Profile Service that is used for user profile synchronization.</span></span> <span data-ttu-id="5c688-105">Om du vill konfigurera tjänsten användarprofil behöver rätt behörigheter beviljas på en Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="5c688-105">To set up the User Profile Service, appropriate permissions need to be granted on an Active Directory domain.</span></span> <span data-ttu-id="5c688-106">Mer information finns i [bevilja behörigheter för Active Directory Domain Services för profilsynkronisering i SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span><span class="sxs-lookup"><span data-stu-id="5c688-106">For more information, see [grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span></span>

<span data-ttu-id="5c688-107">Den här artikeln förklarar hur du kan konfigurera Azure AD Domain Services hanterade domäner om du vill distribuera tjänsten SharePoint Server synkronisering av användarprofiler.</span><span class="sxs-lookup"><span data-stu-id="5c688-107">This article explains how you can configure Azure AD Domain Services managed domains to deploy the SharePoint Server User Profile Sync service.</span></span>

## <a name="the-aad-dc-service-accounts-group"></a><span data-ttu-id="5c688-108">Gruppen 'AAD DC Service Accounts'</span><span class="sxs-lookup"><span data-stu-id="5c688-108">The 'AAD DC Service Accounts' group</span></span>
<span data-ttu-id="5c688-109">En säkerhetsgrupp som heter '**AAD DC-tjänstkonton**' är tillgänglig i organisationsenheten 'Användare' på din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="5c688-109">A security group called '**AAD DC Service Accounts**' is available within the 'Users' organizational unit on your managed domain.</span></span> <span data-ttu-id="5c688-110">Du kan se den här gruppen i den **Active Directory-användare och datorer** MMC snapin-modulen på din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="5c688-110">You can see this group in the **Active Directory Users and Computers** MMC snap-in on your managed domain.</span></span>

![AAD DC-tjänstkonton säkerhetsgrupp](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

<span data-ttu-id="5c688-112">Medlemmar i den här säkerhetsgruppen delegeras följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="5c688-112">Members of this security group are delegated the following privileges:</span></span>
- <span data-ttu-id="5c688-113">Privilegiet ”Replikera katalogändringar' på root DSE för den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="5c688-113">The 'Replicate Directory Changes' privilege on the root DSE of the managed domain.</span></span>
- <span data-ttu-id="5c688-114">Privilegiet ”Replikera katalogändringar' på konfigurationsnamngivningskontexten (cn = konfigurationsbehållare) för den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="5c688-114">The 'Replicate Directory Changes' privilege on the Configuration naming context (cn=configuration container) of the managed domain.</span></span>

<span data-ttu-id="5c688-115">Den här säkerhetsgruppen även är medlem i den inbyggda gruppen **kompatibel åtkomst innan Windows 2000**.</span><span class="sxs-lookup"><span data-stu-id="5c688-115">This security group is also a member of the built-in group **Pre-Windows 2000 Compatible Access**.</span></span>

![AAD DC-tjänstkonton säkerhetsgrupp](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a><span data-ttu-id="5c688-117">Aktivera din hanterade domän som stöd för synkronisering av SharePoint Server användarprofiler</span><span class="sxs-lookup"><span data-stu-id="5c688-117">Enable your managed domain to support SharePoint Server user profile sync</span></span>
<span data-ttu-id="5c688-118">Du kan lägga till tjänstkontot som används för synkronisering av användarprofil SharePoint till den **AAD DC-tjänstkonton** grupp.</span><span class="sxs-lookup"><span data-stu-id="5c688-118">You can add the service account used for SharePoint user profile synchronization to the **AAD DC Service Accounts** group.</span></span> <span data-ttu-id="5c688-119">Konto för synkronisering hämtar därför tillräcklig behörighet för att replikera ändringar i katalogen.</span><span class="sxs-lookup"><span data-stu-id="5c688-119">As a result, the synchronization account gets adequate privileges to replicate changes to the directory.</span></span> <span data-ttu-id="5c688-120">Det här konfigurationssteget gör det möjligt för synkronisering av SharePoint Server användarprofiler ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="5c688-120">This configuration step enables SharePoint Server user profile sync to work correctly.</span></span>

![AAD-DC-tjänstkonton - lägga till medlemmar](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD-DC-tjänstkonton - lägga till medlemmar](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a><span data-ttu-id="5c688-123">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="5c688-123">Related Content</span></span>
* [<span data-ttu-id="5c688-124">Teknisk referens - bevilja Active Directory Domain Services-behörigheter för profilsynkronisering i SharePoint Server 2013</span><span class="sxs-lookup"><span data-stu-id="5c688-124">Technical Reference - Grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013</span></span>](https://technet.microsoft.com/library/hh296982.aspx)
