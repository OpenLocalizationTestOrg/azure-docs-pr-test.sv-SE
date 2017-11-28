---
title: "aaaResources, roller och åtkomstkontroll i Azure Application Insights | Microsoft Docs"
description: "Ägare, deltagare och läsare av din organisations insikter."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: a6f6ca0443b5f60239f094606e124f856967d8ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a><span data-ttu-id="62bbe-103">Resurser, roller och åtkomstkontroll i Application Insights</span><span class="sxs-lookup"><span data-stu-id="62bbe-103">Resources, roles, and access control in Application Insights</span></span>
<span data-ttu-id="62bbe-104">Du kan styra vem som har läs- och uppdatera åtkomst tooyour data i Azure [Programinsikter][start], med hjälp av [rollbaserad åtkomstkontroll i Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="62bbe-104">You can control who has read and update access tooyour data in Azure [Application Insights][start], by using [Role-based access control in Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="62bbe-105">Tilldela åtkomst toousers i hello **resursgrupp eller prenumeration** toowhich ditt inte tillhör programresursen - hello resurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="62bbe-105">Assign access toousers in hello **resource group or subscription** toowhich your application resource belongs - not in hello resource itself.</span></span> <span data-ttu-id="62bbe-106">Tilldela hello **Application Insights component contributor** roll.</span><span class="sxs-lookup"><span data-stu-id="62bbe-106">Assign hello **Application Insights component contributor** role.</span></span> <span data-ttu-id="62bbe-107">Detta säkerställer enhetlig kontroll över åtkomst tooweb testerna och aviseringar tillsammans med din programresurs.</span><span class="sxs-lookup"><span data-stu-id="62bbe-107">This ensures uniform control of access tooweb tests and alerts along with your application resource.</span></span> <span data-ttu-id="62bbe-108">[Läs mer](#access).</span><span class="sxs-lookup"><span data-stu-id="62bbe-108">[Learn more](#access).</span></span>
> 
> 

## <a name="resources-groups-and-subscriptions"></a><span data-ttu-id="62bbe-109">Resurser, grupper och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="62bbe-109">Resources, groups and subscriptions</span></span>
<span data-ttu-id="62bbe-110">Första vissa definitioner:</span><span class="sxs-lookup"><span data-stu-id="62bbe-110">First, some definitions:</span></span>

* <span data-ttu-id="62bbe-111">**Resursen** - en instans av en Microsoft Azure-tjänst.</span><span class="sxs-lookup"><span data-stu-id="62bbe-111">**Resource** - An instance of a Microsoft Azure service.</span></span> <span data-ttu-id="62bbe-112">Application Insights-resurs samlar in, analyseras och visar hello telemetridata som skickas från ditt program.</span><span class="sxs-lookup"><span data-stu-id="62bbe-112">Your Application Insights resource collects, analyzes and displays hello telemetry data sent from your application.</span></span>  <span data-ttu-id="62bbe-113">Andra typer av Azure-resurser är webbappar, databaser och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="62bbe-113">Other types of Azure resources include web apps, databases, and VMs.</span></span>
  
    <span data-ttu-id="62bbe-114">toosee resurser, öppna hello [Azure Portal][portal], logga in och på alla resurser.</span><span class="sxs-lookup"><span data-stu-id="62bbe-114">toosee your resources, open hello [Azure Portal][portal], sign in, and click All Resources.</span></span> <span data-ttu-id="62bbe-115">toofind en resurs anger du en del av namnet i hello filterfältet.</span><span class="sxs-lookup"><span data-stu-id="62bbe-115">toofind a resource, type part of its name in hello filter field.</span></span>
  
    ![Lista över Azure-resurser](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* <span data-ttu-id="62bbe-117">[**Resursgruppen** ] [ group] -varje resurs tillhör tooone grupp.</span><span class="sxs-lookup"><span data-stu-id="62bbe-117">[**Resource group**][group] - Every resource belongs tooone group.</span></span> <span data-ttu-id="62bbe-118">En grupp är ett bekvämt sätt toomanage relaterade resurser, särskilt för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="62bbe-118">A group is a convenient way toomanage related resources, particularly for access control.</span></span> <span data-ttu-id="62bbe-119">I en resurs exporteras till exempel gruppen som du kan placera en Webbapp, appen Application Insights resurs toomonitor hello och en Storage resource tookeep data.</span><span class="sxs-lookup"><span data-stu-id="62bbe-119">For example, into one resource group you could put a Web App, an Application Insights resource toomonitor hello app, and a Storage resource tookeep exported data.</span></span>

    ![Klicka på Bläddra, resursgrupper, och välj sedan en grupp](./media/app-insights-resources-roles-access-control/11-group.png)

* <span data-ttu-id="62bbe-121">[**Prenumerationen** ](https://manage.windowsazure.com) -toouse Application Insights eller andra Azure-resurser kan du logga in tooan Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="62bbe-121">[**Subscription**](https://manage.windowsazure.com) - toouse Application Insights or other Azure resources, you sign in tooan Azure subscription.</span></span> <span data-ttu-id="62bbe-122">Varje resursgrupp tillhör tooone Azure-prenumeration, där du väljer pris-paketet och, om det är en organisation prenumeration, Välj hello medlemmar och deras behörigheter för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="62bbe-122">Every resource group belongs tooone Azure subscription, where you choose your price package and, if it's an organization subscription, choose hello members and their access permissions.</span></span>
* <span data-ttu-id="62bbe-123">[**Microsoft-konto** ] [ account] -hello användarnamn och lösenord som du använder toosign i tooMicrosoft Azure-prenumerationer, XBox Live, Outlook.com och andra Microsoft-tjänster.</span><span class="sxs-lookup"><span data-stu-id="62bbe-123">[**Microsoft account**][account] - hello username and password that you use toosign in tooMicrosoft Azure subscriptions, XBox Live, Outlook.com, and other Microsoft services.</span></span>

## <span data-ttu-id="62bbe-124"><a name="access"></a>Kontrollera åtkomst i hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="62bbe-124"><a name="access"></a> Control access in hello resource group</span></span>
<span data-ttu-id="62bbe-125">Det är viktigt toounderstand att i tillägg toohello resurs du skapat för ditt program, finns också avgränsa dolda resurser för aviseringar och webbtester.</span><span class="sxs-lookup"><span data-stu-id="62bbe-125">It's important toounderstand that in addition toohello resource you created for your application, there are also separate hidden resources for alerts and web tests.</span></span> <span data-ttu-id="62bbe-126">De är anslutna toohello samma [resursgruppen](#resource-group) som ditt program.</span><span class="sxs-lookup"><span data-stu-id="62bbe-126">They are attached toohello same [resource group](#resource-group) as your application.</span></span> <span data-ttu-id="62bbe-127">Du kan också har placerat andra Azure-tjänster i det finns till exempel webbplatser eller lagring.</span><span class="sxs-lookup"><span data-stu-id="62bbe-127">You might also have put other Azure services in there, such as websites or storage.</span></span>

![Resurser i Application Insights](./media/app-insights-resources-roles-access-control/00-resources.png)

<span data-ttu-id="62bbe-129">toocontrol åtkomst toothese resurser bör därför att:</span><span class="sxs-lookup"><span data-stu-id="62bbe-129">toocontrol access toothese resources it's therefore recommended to:</span></span>

* <span data-ttu-id="62bbe-130">Åtkomstkontroll på hello **resursgrupp eller prenumeration** nivå.</span><span class="sxs-lookup"><span data-stu-id="62bbe-130">Control access at hello **resource group or subscription** level.</span></span>
* <span data-ttu-id="62bbe-131">Tilldela hello **Application Insights Component contributor** rollen toousers.</span><span class="sxs-lookup"><span data-stu-id="62bbe-131">Assign hello **Application Insights Component contributor** role toousers.</span></span> <span data-ttu-id="62bbe-132">Detta innebär att de tooedit webbtester, aviseringar och Application Insights-resurser utan att ange åtkomst tooany andra tjänster i hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="62bbe-132">This allows them tooedit web tests, alerts, and Application Insights resources, without providing access tooany other services in hello group.</span></span>

## <a name="tooprovide-access-tooanother-user"></a><span data-ttu-id="62bbe-133">tooprovide åtkomst tooanother användare</span><span class="sxs-lookup"><span data-stu-id="62bbe-133">tooprovide access tooanother user</span></span>
<span data-ttu-id="62bbe-134">Du måste ha ägare rättigheter toohello prenumeration eller resursgrupp hello.</span><span class="sxs-lookup"><span data-stu-id="62bbe-134">You must have Owner rights toohello subscription or hello resource group.</span></span>

<span data-ttu-id="62bbe-135">hello användaren måste ha en [Account][account], eller öppna tootheir [Account Microsoft](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="62bbe-135">hello user must have a [Microsoft Account][account], or access tootheir [organizational Microsoft Account](../active-directory/sign-up-organization.md).</span></span> <span data-ttu-id="62bbe-136">Du kan ge åtkomst tooindividuals och toouser grupper som har definierats i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62bbe-136">You can provide access tooindividuals, and also toouser groups defined in Azure Active Directory.</span></span>

#### <a name="navigate-toohello-resource-group"></a><span data-ttu-id="62bbe-137">Navigera toohello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="62bbe-137">Navigate toohello resource group</span></span>
<span data-ttu-id="62bbe-138">Lägg till det hello-användare.</span><span class="sxs-lookup"><span data-stu-id="62bbe-138">Add hello user there.</span></span>

![I resursbladet för ditt program, öppna Essentials, öppna hello resursgruppen och välj det inställningar/användare.](./media/app-insights-resources-roles-access-control/01-add-user.png)

<span data-ttu-id="62bbe-141">Eller gå in en annan och lägga till hello användaren toohello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="62bbe-141">Or you could go up another level and add hello user toohello Subscription.</span></span>

#### <a name="select-a-role"></a><span data-ttu-id="62bbe-142">Välj en roll</span><span class="sxs-lookup"><span data-stu-id="62bbe-142">Select a role</span></span>
![Välj en roll för hello ny användare](./media/app-insights-resources-roles-access-control/03-role.png)

| <span data-ttu-id="62bbe-144">Roll</span><span class="sxs-lookup"><span data-stu-id="62bbe-144">Role</span></span> | <span data-ttu-id="62bbe-145">I hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="62bbe-145">In hello resource group</span></span> |
| --- | --- |
| <span data-ttu-id="62bbe-146">Ägare</span><span class="sxs-lookup"><span data-stu-id="62bbe-146">Owner</span></span> |<span data-ttu-id="62bbe-147">Ändra vad som helst, inklusive användaråtkomst</span><span class="sxs-lookup"><span data-stu-id="62bbe-147">Can change anything, including user access</span></span> |
| <span data-ttu-id="62bbe-148">Deltagare</span><span class="sxs-lookup"><span data-stu-id="62bbe-148">Contributor</span></span> |<span data-ttu-id="62bbe-149">Redigera något, inklusive alla resurser</span><span class="sxs-lookup"><span data-stu-id="62bbe-149">Can edit anything, including all resources</span></span> |
| <span data-ttu-id="62bbe-150">Application Insights Component contributor</span><span class="sxs-lookup"><span data-stu-id="62bbe-150">Application Insights Component contributor</span></span> |<span data-ttu-id="62bbe-151">Redigera Application Insights-resurser, webbtester och aviseringar</span><span class="sxs-lookup"><span data-stu-id="62bbe-151">Can edit Application Insights resources, web tests and alerts</span></span> |
| <span data-ttu-id="62bbe-152">Läsare</span><span class="sxs-lookup"><span data-stu-id="62bbe-152">Reader</span></span> |<span data-ttu-id="62bbe-153">Kan visa men inte ändra någonting</span><span class="sxs-lookup"><span data-stu-id="62bbe-153">Can view but not change anything</span></span> |

<span data-ttu-id="62bbe-154">'Redigera' omfattar att skapa, ta bort och uppdatera:</span><span class="sxs-lookup"><span data-stu-id="62bbe-154">'Editing' includes creating, deleting and updating:</span></span>

* <span data-ttu-id="62bbe-155">Resurser</span><span class="sxs-lookup"><span data-stu-id="62bbe-155">Resources</span></span>
* <span data-ttu-id="62bbe-156">Webbtester</span><span class="sxs-lookup"><span data-stu-id="62bbe-156">Web tests</span></span>
* <span data-ttu-id="62bbe-157">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="62bbe-157">Alerts</span></span>
* <span data-ttu-id="62bbe-158">Löpande export</span><span class="sxs-lookup"><span data-stu-id="62bbe-158">Continuous export</span></span>

#### <a name="select-hello-user"></a><span data-ttu-id="62bbe-159">Välj hello användare</span><span class="sxs-lookup"><span data-stu-id="62bbe-159">Select hello user</span></span>

<span data-ttu-id="62bbe-160">Om det inte är i hello directory hello-användare som du vill kan erbjuda du alla med ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="62bbe-160">If hello user you want isn't in hello directory, you can invite anyone with a Microsoft account.</span></span>
<span data-ttu-id="62bbe-161">(Om de använder tjänster som Outlook.com, OneDrive, Windows Phone och XBox Live, har ett Microsoft-konto.)</span><span class="sxs-lookup"><span data-stu-id="62bbe-161">(If they use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, they have a Microsoft account.)</span></span>

## <a name="related-content"></a><span data-ttu-id="62bbe-162">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="62bbe-162">Related content</span></span>

* [<span data-ttu-id="62bbe-163">Rollbaserad åtkomstkontroll i Azure</span><span class="sxs-lookup"><span data-stu-id="62bbe-163">Role based access control in Azure</span></span>](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
