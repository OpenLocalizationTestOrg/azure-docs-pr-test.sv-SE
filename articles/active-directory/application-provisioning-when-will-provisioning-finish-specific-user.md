---
title: "aaaFind ut när en viss användare kommer att kunna tooaccess ett program | Microsoft Docs"
description: "Hur toofind ut när en ytterst viktigt användare vara kan tooaccess ett program du har konfigurerat för användaretablering med Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: bb9520499dcc8bbbe6fae05c5238c8852815ea0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-tooaccess-an-application"></a><span data-ttu-id="390fb-103">Ta reda på när en viss användare kommer att kunna tooaccess ett program</span><span class="sxs-lookup"><span data-stu-id="390fb-103">Find out when a specific user will be able tooaccess an application</span></span>
<span data-ttu-id="390fb-104">När du använder automatisk användaretablering med ett program, Azure AD automatiskt etablera och uppdatera användarkonton i en app baserat på sådant som [användar- och tilldelning](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) vid en schemalagd tid intervall, vanligtvis var 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="390fb-104">When using automatic user provisioning with an application, Azure AD automatically provision and update user accounts in an app based on things like [user and group assignment](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) at a regularly scheduled time interval, typically every 10 minutes.</span></span>

## <a name="how-long-does-it-take"></a><span data-ttu-id="390fb-105">Hur lång tid tar det?</span><span class="sxs-lookup"><span data-stu-id="390fb-105">How long does it take?</span></span>

<span data-ttu-id="390fb-106">hello tid det tar för en viss användare toobe etablerats beror huvudsakligen på huruvida en inledande ”full” sync redan har inträffat.</span><span class="sxs-lookup"><span data-stu-id="390fb-106">hello time it takes for a given user toobe provisioned depends mainly on whether or not an initial “full” sync has already occurred.</span></span>

<span data-ttu-id="390fb-107">Hej första synkronisering mellan Azure AD och en app kan ta allt från 20 minuter tooseveral timmar, beroende på hello storleken på hello Azure AD-katalogen och hello antalet användare i omfånget för etablering.</span><span class="sxs-lookup"><span data-stu-id="390fb-107">hello first sync between Azure AD and an app can take anywhere from 20 minutes tooseveral hours, depending on hello size of hello Azure AD directory and hello number of users in scope for provisioning.</span></span> 

<span data-ttu-id="390fb-108">Efterföljande synkroniseringar efter hello inledande synkronisering vara snabbare (t.ex. inom 10 minuter) som hello Etablerar tjänsten lagrar vattenstämplar som representerar hello tillståndet för båda systemen efter hello inledande synkronisering, förbättra prestanda för efterföljande synkronisering.</span><span class="sxs-lookup"><span data-stu-id="390fb-108">Subsequent syncs after hello initial sync be faster (e.g. within 10 minutes), as hello provisioning service stores watermarks that represent hello state of both systems after hello initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-toocheck-hello-status-of-a-user"></a><span data-ttu-id="390fb-109">Hur toocheck hello status för en användare</span><span class="sxs-lookup"><span data-stu-id="390fb-109">How toocheck hello status of a user</span></span>

<span data-ttu-id="390fb-110">toosee hello Etableringsstatus för en vald användare se hello granskningsloggar i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="390fb-110">toosee hello provisioning status for a selected user, consult hello Audit logs in Azure AD.</span></span>

<span data-ttu-id="390fb-111">hello etablering granskningsloggar kan nås i hello Azure-portalen i hello **Azure Active Directory &gt; Företagsappar &gt; \[programnamn\] &gt; granskningsloggarna**fliken. Filter hello loggar in hello **Kontoetablering** kategori tooonly finns hello etablering händelser för appen.</span><span class="sxs-lookup"><span data-stu-id="390fb-111">hello provisioning audit logs can be accessed in hello Azure portal, in hello **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt; Audit Logs** tab. Filter hello logs on hello **Account Provisioning** category tooonly see hello provisioning events for that app.</span></span> <span data-ttu-id="390fb-112">Du kan söka efter användare baserat på hello ”matchande ID” som konfigurerats för dem i hello attributmappning.</span><span class="sxs-lookup"><span data-stu-id="390fb-112">You can search for users based on hello “matching ID” that was configured for them in hello attribute mappings.</span></span> 

<span data-ttu-id="390fb-113">Om du har konfigurerat hello ”huvudnamn” eller ”e-postadress” som hello matchar attributet på hello Azure AD-sidan och hello användaren inte etablering har värdet till exempel ”audrey@contoso.com”, sedan Sök hello granskningsloggar för ”audrey@contoso.com” och granska sedan poster som returneras.</span><span class="sxs-lookup"><span data-stu-id="390fb-113">For example, if you configured hello “user principal name” or “email address” as hello matching attribute on hello Azure AD side, and hello user not being provisioning has a value of “audrey@contoso.com”, then search hello audit logs for “audrey@contoso.com” and review then entries returned.</span></span>

<span data-ttu-id="390fb-114">hello etablering audit loggar register alla hello åtgärder som utförs av hello etableras, inklusive:</span><span class="sxs-lookup"><span data-stu-id="390fb-114">hello provisioning audit logs record all hello operations performed by hello provisioning service, including:</span></span>

* <span data-ttu-id="390fb-115">Hämta Azure AD för tilldelade användare som ingår i omfånget för etablering</span><span class="sxs-lookup"><span data-stu-id="390fb-115">Querying Azure AD for assigned users that are in scope for provisioning</span></span>
* <span data-ttu-id="390fb-116">Frågar hello mål app hello befintliga användare</span><span class="sxs-lookup"><span data-stu-id="390fb-116">Querying hello target app for hello existence of those users</span></span>
* <span data-ttu-id="390fb-117">Jämföra hello användarobjekt mellan hello system</span><span class="sxs-lookup"><span data-stu-id="390fb-117">Comparing hello user objects between hello system</span></span>
* <span data-ttu-id="390fb-118">Lägga till, uppdatera eller inaktivera hello användarkonto i hello målsystemet baserat på hello jämförelse</span><span class="sxs-lookup"><span data-stu-id="390fb-118">Adding, updating, or disabling hello user account in hello target system based on hello comparison</span></span>

## <a name="next-steps"></a><span data-ttu-id="390fb-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="390fb-119">Next steps</span></span>
<span data-ttu-id="390fb-120">[Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''</span><span class="sxs-lookup"><span data-stu-id="390fb-120">[Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''</span></span>
