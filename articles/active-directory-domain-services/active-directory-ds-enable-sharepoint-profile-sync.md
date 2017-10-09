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
# <a name="configure-a-managed-domain-toosupport-profile-synchronization-for-sharepoint-server"></a>Konfigurera en hanterad domän toosupport profilsynkronisering för SharePoint Server
SharePoint Server innehåller en tjänsten användarprofil som används för synkronisering av användarprofiler. tooset in hello tjänsten användarprofil behörighet måste toobe beviljas för en Active Directory-domän. Mer information finns i [bevilja behörigheter för Active Directory Domain Services för profilsynkronisering i SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).

Den här artikeln förklarar hur du kan konfigurera Azure AD Domain Services hanterade domäner toodeploy hello SharePoint Server synkronisering av användarprofiler service.

## <a name="hello-aad-dc-service-accounts-group"></a>hello-tjänstkonton AAD DC-grupp
En säkerhetsgrupp som heter ”**AAD DC-tjänstkonton**' är tillgängliga i hello 'Användare' organisationsenhet på din hanterade domän. Du kan se den här gruppen i hello **Active Directory-användare och datorer** MMC snapin-modulen på din hanterade domän.

![AAD DC-tjänstkonton säkerhetsgrupp](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Medlemmar i den här säkerhetsgruppen är delegerad hello följande behörigheter:
- hello 'Replikera katalogändringar' privilegiet hello root DSE av hello hanterade domän.
- hello privilegiet ”Replikera katalogändringar' hello konfigurationsnamngivningskontexten (cn = konfigurationsbehållare) av hello hanterade domän.

Den här säkerhetsgruppen ingår också i hello inbyggd grupp **kompatibel åtkomst innan Windows 2000**.

![AAD DC-tjänstkonton säkerhetsgrupp](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-toosupport-sharepoint-server-user-profile-sync"></a>Aktivera din hanterade domän toosupport SharePoint Server synkronisering av användarprofiler
Du kan lägga till hello-tjänstkonto som används för SharePoint användarens profil synkronisering toohello **AAD DC-tjänstkonton** grupp. Därför hämtar hello synkroniseringskontot privilegier tooreplicate ändringar toohello directory. Det här konfigurationssteget gör SharePoint Server användarens profil sync toowork korrekt.

![AAD-DC-tjänstkonton - lägga till medlemmar](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD-DC-tjänstkonton - lägga till medlemmar](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Relaterat innehåll
* [Teknisk referens - bevilja Active Directory Domain Services-behörigheter för profilsynkronisering i SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx)
