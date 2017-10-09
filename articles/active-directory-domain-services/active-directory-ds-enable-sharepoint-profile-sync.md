---
title: "Azure Active Directory Domain Services: Aktivera stöd för användarprofil för SharePoint service | Microsoft Docs"
description: "Konfigurera Azure Active Directory Domain Services hanterade domäner toosupport profilsynkronisering för SharePoint Server"
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
ms.openlocfilehash: 9de4f810380309e8f6436fc24412701645978f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-managed-domain-toosupport-profile-synchronization-for-sharepoint-server"></a><span data-ttu-id="8d9f1-103">Konfigurera en hanterad domän toosupport profilsynkronisering för SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="8d9f1-103">Configure a managed domain toosupport profile synchronization for SharePoint Server</span></span>
<span data-ttu-id="8d9f1-104">SharePoint Server innehåller en tjänsten användarprofil som används för synkronisering av användarprofiler.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-104">SharePoint Server includes a User Profile Service that is used for user profile synchronization.</span></span> <span data-ttu-id="8d9f1-105">tooset in hello tjänsten användarprofil behörighet måste toobe beviljas för en Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-105">tooset up hello User Profile Service, appropriate permissions need toobe granted on an Active Directory domain.</span></span> <span data-ttu-id="8d9f1-106">Mer information finns i [bevilja behörigheter för Active Directory Domain Services för profilsynkronisering i SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d9f1-106">For more information, see [grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span></span>

<span data-ttu-id="8d9f1-107">Den här artikeln förklarar hur du kan konfigurera Azure AD Domain Services hanterade domäner toodeploy hello SharePoint Server synkronisering av användarprofiler service.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-107">This article explains how you can configure Azure AD Domain Services managed domains toodeploy hello SharePoint Server User Profile Sync service.</span></span>

## <a name="hello-aad-dc-service-accounts-group"></a><span data-ttu-id="8d9f1-108">hello-tjänstkonton AAD DC-grupp</span><span class="sxs-lookup"><span data-stu-id="8d9f1-108">hello 'AAD DC Service Accounts' group</span></span>
<span data-ttu-id="8d9f1-109">En säkerhetsgrupp som heter ”**AAD DC-tjänstkonton**' är tillgängliga i hello 'Användare' organisationsenhet på din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-109">A security group called '**AAD DC Service Accounts**' is available within hello 'Users' organizational unit on your managed domain.</span></span> <span data-ttu-id="8d9f1-110">Du kan se den här gruppen i hello **Active Directory-användare och datorer** MMC snapin-modulen på din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-110">You can see this group in hello **Active Directory Users and Computers** MMC snap-in on your managed domain.</span></span>

![AAD DC-tjänstkonton säkerhetsgrupp](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

<span data-ttu-id="8d9f1-112">Medlemmar i den här säkerhetsgruppen är delegerad hello följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="8d9f1-112">Members of this security group are delegated hello following privileges:</span></span>
- <span data-ttu-id="8d9f1-113">hello 'Replikera katalogändringar' privilegiet hello root DSE av hello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-113">hello 'Replicate Directory Changes' privilege on hello root DSE of hello managed domain.</span></span>
- <span data-ttu-id="8d9f1-114">hello privilegiet ”Replikera katalogändringar' hello konfigurationsnamngivningskontexten (cn = konfigurationsbehållare) av hello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-114">hello 'Replicate Directory Changes' privilege on hello Configuration naming context (cn=configuration container) of hello managed domain.</span></span>

<span data-ttu-id="8d9f1-115">Den här säkerhetsgruppen ingår också i hello inbyggd grupp **kompatibel åtkomst innan Windows 2000**.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-115">This security group is also a member of hello built-in group **Pre-Windows 2000 Compatible Access**.</span></span>

![AAD DC-tjänstkonton säkerhetsgrupp](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-toosupport-sharepoint-server-user-profile-sync"></a><span data-ttu-id="8d9f1-117">Aktivera din hanterade domän toosupport SharePoint Server synkronisering av användarprofiler</span><span class="sxs-lookup"><span data-stu-id="8d9f1-117">Enable your managed domain toosupport SharePoint Server user profile sync</span></span>
<span data-ttu-id="8d9f1-118">Du kan lägga till hello-tjänstkonto som används för SharePoint användarens profil synkronisering toohello **AAD DC-tjänstkonton** grupp.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-118">You can add hello service account used for SharePoint user profile synchronization toohello **AAD DC Service Accounts** group.</span></span> <span data-ttu-id="8d9f1-119">Därför hämtar hello synkroniseringskontot privilegier tooreplicate ändringar toohello directory.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-119">As a result, hello synchronization account gets adequate privileges tooreplicate changes toohello directory.</span></span> <span data-ttu-id="8d9f1-120">Det här konfigurationssteget gör SharePoint Server användarens profil sync toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="8d9f1-120">This configuration step enables SharePoint Server user profile sync toowork correctly.</span></span>

![AAD-DC-tjänstkonton - lägga till medlemmar](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD-DC-tjänstkonton - lägga till medlemmar](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a><span data-ttu-id="8d9f1-123">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="8d9f1-123">Related Content</span></span>
* [<span data-ttu-id="8d9f1-124">Teknisk referens - bevilja Active Directory Domain Services-behörigheter för profilsynkronisering i SharePoint Server 2013</span><span class="sxs-lookup"><span data-stu-id="8d9f1-124">Technical Reference - Grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013</span></span>](https://technet.microsoft.com/library/hh296982.aspx)
