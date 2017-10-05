---
title: "Ta reda på när en viss användare kommer att kunna komma åt ett program | Microsoft Docs"
description: "Ta reda på när en ytterst viktigt användare komma åt ett program som du har konfigurerat för användaretablering med Azure AD"
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
ms.openlocfilehash: fcefb31904cfb77022db0358e9feee6a0479db81
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-to-access-an-application"></a><span data-ttu-id="6765f-103">Ta reda på när en viss användare kommer att kunna komma åt ett program</span><span class="sxs-lookup"><span data-stu-id="6765f-103">Find out when a specific user will be able to access an application</span></span>
<span data-ttu-id="6765f-104">När du använder automatisk användaretablering med ett program, Azure AD automatiskt etablera och uppdatera användarkonton i en app baserat på sådant som [användar- och tilldelning](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) vid en schemalagd tid intervall, vanligtvis var 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="6765f-104">When using automatic user provisioning with an application, Azure AD automatically provision and update user accounts in an app based on things like [user and group assignment](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) at a regularly scheduled time interval, typically every 10 minutes.</span></span>

## <a name="how-long-does-it-take"></a><span data-ttu-id="6765f-105">Hur lång tid tar det?</span><span class="sxs-lookup"><span data-stu-id="6765f-105">How long does it take?</span></span>

<span data-ttu-id="6765f-106">Den tid det tar för en användare som ska etableras beror huvudsakligen på huruvida en inledande ”full” sync redan har inträffat.</span><span class="sxs-lookup"><span data-stu-id="6765f-106">The time it takes for a given user to be provisioned depends mainly on whether or not an initial “full” sync has already occurred.</span></span>

<span data-ttu-id="6765f-107">Den första synkroniseringen mellan Azure AD och en app kan ta allt från 20 minuter till flera timmar, beroende på storleken på Azure AD-katalog och antalet användare i omfånget för etablering.</span><span class="sxs-lookup"><span data-stu-id="6765f-107">The first sync between Azure AD and an app can take anywhere from 20 minutes to several hours, depending on the size of the Azure AD directory and the number of users in scope for provisioning.</span></span> 

<span data-ttu-id="6765f-108">Efterföljande synkroniseringar efter den första synkroniseringen vara snabbare (t.ex. inom 10 minuter) som tjänsten etablering lagrar vattenstämplar som representerar tillståndet för båda systemen efter den första synkroniseringen, förbättra prestanda för efterföljande synkronisering.</span><span class="sxs-lookup"><span data-stu-id="6765f-108">Subsequent syncs after the initial sync be faster (e.g. within 10 minutes), as the provisioning service stores watermarks that represent the state of both systems after the initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-to-check-the-status-of-a-user"></a><span data-ttu-id="6765f-109">Så här kontrollerar du statusen för en användare</span><span class="sxs-lookup"><span data-stu-id="6765f-109">How to check the status of a user</span></span>

<span data-ttu-id="6765f-110">Om du vill se Etableringsstatus för en vald användare finns i granskningsloggarna i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6765f-110">To see the provisioning status for a selected user, consult the Audit logs in Azure AD.</span></span>

<span data-ttu-id="6765f-111">Etablering granskningsloggarna kan nås i Azure-portalen i den **Azure Active Directory &gt; Företagsappar &gt; \[programnamn\] &gt; granskningsloggarna** fliken.</span><span class="sxs-lookup"><span data-stu-id="6765f-111">The provisioning audit logs can be accessed in the Azure portal, in the **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt; Audit Logs** tab.</span></span> <span data-ttu-id="6765f-112">Filtrera loggarna på den **Kontoetablering** kategori bara ser de etablering händelserna för appen.</span><span class="sxs-lookup"><span data-stu-id="6765f-112">Filter the logs on the **Account Provisioning** category to only see the provisioning events for that app.</span></span> <span data-ttu-id="6765f-113">Du kan söka efter användare baserat på ”matchande ID” som konfigurerats för dem i attributet avbildningar.</span><span class="sxs-lookup"><span data-stu-id="6765f-113">You can search for users based on the “matching ID” that was configured for them in the attribute mappings.</span></span> 

<span data-ttu-id="6765f-114">Om du har konfigurerat ”huvudnamn” eller ”e-postadress” som matchar attributet på Azure AD-sida och användaren inte etablering har värdet till exempel ”audrey@contoso.com”, söka granskningsloggarna för ”audrey@contoso.com” och sedan granska poster returnerade.</span><span class="sxs-lookup"><span data-stu-id="6765f-114">For example, if you configured the “user principal name” or “email address” as the matching attribute on the Azure AD side, and the user not being provisioning has a value of “audrey@contoso.com”, then search the audit logs for “audrey@contoso.com” and review then entries returned.</span></span>

<span data-ttu-id="6765f-115">Etablering granskningsloggarna registrera alla åtgärder som utförs av tjänsten etablering, inklusive:</span><span class="sxs-lookup"><span data-stu-id="6765f-115">The provisioning audit logs record all the operations performed by the provisioning service, including:</span></span>

* <span data-ttu-id="6765f-116">Hämta Azure AD för tilldelade användare som ingår i omfånget för etablering</span><span class="sxs-lookup"><span data-stu-id="6765f-116">Querying Azure AD for assigned users that are in scope for provisioning</span></span>
* <span data-ttu-id="6765f-117">Frågar appen mål förekomsten av användare</span><span class="sxs-lookup"><span data-stu-id="6765f-117">Querying the target app for the existence of those users</span></span>
* <span data-ttu-id="6765f-118">Jämförelse mellan användarobjekt mellan systemet</span><span class="sxs-lookup"><span data-stu-id="6765f-118">Comparing the user objects between the system</span></span>
* <span data-ttu-id="6765f-119">Lägga till, uppdatera eller inaktivera användarkontot i målsystemet baserat på jämförelsen</span><span class="sxs-lookup"><span data-stu-id="6765f-119">Adding, updating, or disabling the user account in the target system based on the comparison</span></span>

## <a name="next-steps"></a><span data-ttu-id="6765f-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6765f-120">Next steps</span></span>
<span data-ttu-id="6765f-121">[Automatisera användaren etablering och avetablering för SaaS-program med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''</span><span class="sxs-lookup"><span data-stu-id="6765f-121">[Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''</span></span>
