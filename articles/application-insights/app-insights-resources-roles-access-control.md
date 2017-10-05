---
title: "Resurser, roller och åtkomst kontroll i Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: c979a8bfbeecacc7c0bbc112e02a4b68e874c219
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a><span data-ttu-id="eaa71-103">Resurser, roller och åtkomstkontroll i Application Insights</span><span class="sxs-lookup"><span data-stu-id="eaa71-103">Resources, roles, and access control in Application Insights</span></span>
<span data-ttu-id="eaa71-104">Du kan styra vem som har läs- och uppdatera åtkomst till dina data i Azure [Programinsikter][start], med hjälp av [rollbaserad åtkomstkontroll i Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="eaa71-104">You can control who has read and update access to your data in Azure [Application Insights][start], by using [Role-based access control in Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eaa71-105">Tilldela åtkomst till användare i den **resursgrupp eller prenumeration** som programmet resurs tillhör - inte i resursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="eaa71-105">Assign access to users in the **resource group or subscription** to which your application resource belongs - not in the resource itself.</span></span> <span data-ttu-id="eaa71-106">Tilldela den **Application Insights component contributor** roll.</span><span class="sxs-lookup"><span data-stu-id="eaa71-106">Assign the **Application Insights component contributor** role.</span></span> <span data-ttu-id="eaa71-107">Detta säkerställer enhetlig kontroll över åtkomsten till webbtester och aviseringar tillsammans med din programresurs.</span><span class="sxs-lookup"><span data-stu-id="eaa71-107">This ensures uniform control of access to web tests and alerts along with your application resource.</span></span> <span data-ttu-id="eaa71-108">[Läs mer](#access).</span><span class="sxs-lookup"><span data-stu-id="eaa71-108">[Learn more](#access).</span></span>
> 
> 

## <a name="resources-groups-and-subscriptions"></a><span data-ttu-id="eaa71-109">Resurser, grupper och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="eaa71-109">Resources, groups and subscriptions</span></span>
<span data-ttu-id="eaa71-110">Första vissa definitioner:</span><span class="sxs-lookup"><span data-stu-id="eaa71-110">First, some definitions:</span></span>

* <span data-ttu-id="eaa71-111">**Resursen** - en instans av en Microsoft Azure-tjänst.</span><span class="sxs-lookup"><span data-stu-id="eaa71-111">**Resource** - An instance of a Microsoft Azure service.</span></span> <span data-ttu-id="eaa71-112">Application Insights-resurs samlar in, analyseras och visar telemetridata skickas från ditt program.</span><span class="sxs-lookup"><span data-stu-id="eaa71-112">Your Application Insights resource collects, analyzes and displays the telemetry data sent from your application.</span></span>  <span data-ttu-id="eaa71-113">Andra typer av Azure-resurser är webbappar, databaser och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eaa71-113">Other types of Azure resources include web apps, databases, and VMs.</span></span>
  
    <span data-ttu-id="eaa71-114">Om du vill se dina resurser, öppna den [Azure Portal][portal], logga in och på alla resurser.</span><span class="sxs-lookup"><span data-stu-id="eaa71-114">To see your resources, open the [Azure Portal][portal], sign in, and click All Resources.</span></span> <span data-ttu-id="eaa71-115">Om du vill hitta en resurs, skriver du datorns namn i filterfältet.</span><span class="sxs-lookup"><span data-stu-id="eaa71-115">To find a resource, type part of its name in the filter field.</span></span>
  
    ![Lista över Azure-resurser](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* <span data-ttu-id="eaa71-117">[**Resursgruppen** ] [ group] -alla resurser som tillhör en grupp.</span><span class="sxs-lookup"><span data-stu-id="eaa71-117">[**Resource group**][group] - Every resource belongs to one group.</span></span> <span data-ttu-id="eaa71-118">En grupp är ett praktiskt sätt att hantera relaterade resurser, särskilt för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="eaa71-118">A group is a convenient way to manage related resources, particularly for access control.</span></span> <span data-ttu-id="eaa71-119">Till exempel kan en resursgrupp du placera en Webbapp Application Insights-resurs för att övervaka appen och en lagringsresurs så att exporterade data.</span><span class="sxs-lookup"><span data-stu-id="eaa71-119">For example, into one resource group you could put a Web App, an Application Insights resource to monitor the app, and a Storage resource to keep exported data.</span></span>

    ![Klicka på Bläddra, resursgrupper, och välj sedan en grupp](./media/app-insights-resources-roles-access-control/11-group.png)

* <span data-ttu-id="eaa71-121">[**Prenumerationen** ](https://manage.windowsazure.com) - om du vill använda Application Insights eller andra Azure-resurser som du loggar in på en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="eaa71-121">[**Subscription**](https://manage.windowsazure.com) - To use Application Insights or other Azure resources, you sign in to an Azure subscription.</span></span> <span data-ttu-id="eaa71-122">Varje resursgrupp som hör till en Azure-prenumeration där du väljer pris-paketet och, om det är en organisation prenumeration, Välj medlemmar och deras behörigheter för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="eaa71-122">Every resource group belongs to one Azure subscription, where you choose your price package and, if it's an organization subscription, choose the members and their access permissions.</span></span>
* <span data-ttu-id="eaa71-123">[**Microsoft-konto** ] [ account] -användarnamn och lösenord som du använder för att logga in på Microsoft Azure-prenumerationer, XBox Live, Outlook.com och andra Microsoft-tjänster.</span><span class="sxs-lookup"><span data-stu-id="eaa71-123">[**Microsoft account**][account] - The username and password that you use to sign in to Microsoft Azure subscriptions, XBox Live, Outlook.com, and other Microsoft services.</span></span>

## <span data-ttu-id="eaa71-124"><a name="access"></a>Kontrollera åtkomst i resursgruppen</span><span class="sxs-lookup"><span data-stu-id="eaa71-124"><a name="access"></a> Control access in the resource group</span></span>
<span data-ttu-id="eaa71-125">Det är viktigt att förstå att förutom den resurs du skapat för ditt program, finns också separat dolda resurser för aviseringar och webbtester.</span><span class="sxs-lookup"><span data-stu-id="eaa71-125">It's important to understand that in addition to the resource you created for your application, there are also separate hidden resources for alerts and web tests.</span></span> <span data-ttu-id="eaa71-126">De är kopplade till samma [resursgruppen](#resource-group) som ditt program.</span><span class="sxs-lookup"><span data-stu-id="eaa71-126">They are attached to the same [resource group](#resource-group) as your application.</span></span> <span data-ttu-id="eaa71-127">Du kan också har placerat andra Azure-tjänster i det finns till exempel webbplatser eller lagring.</span><span class="sxs-lookup"><span data-stu-id="eaa71-127">You might also have put other Azure services in there, such as websites or storage.</span></span>

![Resurser i Application Insights](./media/app-insights-resources-roles-access-control/00-resources.png)

<span data-ttu-id="eaa71-129">Styra åtkomsten till dessa resurser bör därför att:</span><span class="sxs-lookup"><span data-stu-id="eaa71-129">To control access to these resources it's therefore recommended to:</span></span>

* <span data-ttu-id="eaa71-130">Kontrollera åtkomsten till den **resursgrupp eller prenumeration** nivå.</span><span class="sxs-lookup"><span data-stu-id="eaa71-130">Control access at the **resource group or subscription** level.</span></span>
* <span data-ttu-id="eaa71-131">Tilldela den **Application Insights Component contributor** roll till användare.</span><span class="sxs-lookup"><span data-stu-id="eaa71-131">Assign the **Application Insights Component contributor** role to users.</span></span> <span data-ttu-id="eaa71-132">Detta gör att de kan redigera webbtester, aviseringar och Application Insights-resurser utan att ge tillgång till andra tjänster i gruppen.</span><span class="sxs-lookup"><span data-stu-id="eaa71-132">This allows them to edit web tests, alerts, and Application Insights resources, without providing access to any other services in the group.</span></span>

## <a name="to-provide-access-to-another-user"></a><span data-ttu-id="eaa71-133">Att ge åtkomst till en annan användare</span><span class="sxs-lookup"><span data-stu-id="eaa71-133">To provide access to another user</span></span>
<span data-ttu-id="eaa71-134">Du måste ha behörigheten ägare till prenumerationen eller resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="eaa71-134">You must have Owner rights to the subscription or the resource group.</span></span>

<span data-ttu-id="eaa71-135">Användaren måste ha en [Account][account], eller åtkomst till sina [Account Microsoft](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="eaa71-135">The user must have a [Microsoft Account][account], or access to their [organizational Microsoft Account](../active-directory/sign-up-organization.md).</span></span> <span data-ttu-id="eaa71-136">Du kan ge åtkomst till enskilda personer och till användargrupper som definierats i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="eaa71-136">You can provide access to individuals, and also to user groups defined in Azure Active Directory.</span></span>

#### <a name="navigate-to-the-resource-group"></a><span data-ttu-id="eaa71-137">Gå till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="eaa71-137">Navigate to the resource group</span></span>
<span data-ttu-id="eaa71-138">Lägg till användaren där.</span><span class="sxs-lookup"><span data-stu-id="eaa71-138">Add the user there.</span></span>

![I resursbladet för ditt program, öppna Essentials, öppna resursgruppen och välj det inställningar och användare.](./media/app-insights-resources-roles-access-control/01-add-user.png)

<span data-ttu-id="eaa71-141">Eller gå in en annan och lägga till användaren till prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="eaa71-141">Or you could go up another level and add the user to the Subscription.</span></span>

#### <a name="select-a-role"></a><span data-ttu-id="eaa71-142">Välj en roll</span><span class="sxs-lookup"><span data-stu-id="eaa71-142">Select a role</span></span>
![Välj en roll för den nya användaren](./media/app-insights-resources-roles-access-control/03-role.png)

| <span data-ttu-id="eaa71-144">Roll</span><span class="sxs-lookup"><span data-stu-id="eaa71-144">Role</span></span> | <span data-ttu-id="eaa71-145">I resursgruppen</span><span class="sxs-lookup"><span data-stu-id="eaa71-145">In the resource group</span></span> |
| --- | --- |
| <span data-ttu-id="eaa71-146">Ägare</span><span class="sxs-lookup"><span data-stu-id="eaa71-146">Owner</span></span> |<span data-ttu-id="eaa71-147">Ändra vad som helst, inklusive användaråtkomst</span><span class="sxs-lookup"><span data-stu-id="eaa71-147">Can change anything, including user access</span></span> |
| <span data-ttu-id="eaa71-148">Deltagare</span><span class="sxs-lookup"><span data-stu-id="eaa71-148">Contributor</span></span> |<span data-ttu-id="eaa71-149">Redigera något, inklusive alla resurser</span><span class="sxs-lookup"><span data-stu-id="eaa71-149">Can edit anything, including all resources</span></span> |
| <span data-ttu-id="eaa71-150">Application Insights Component contributor</span><span class="sxs-lookup"><span data-stu-id="eaa71-150">Application Insights Component contributor</span></span> |<span data-ttu-id="eaa71-151">Redigera Application Insights-resurser, webbtester och aviseringar</span><span class="sxs-lookup"><span data-stu-id="eaa71-151">Can edit Application Insights resources, web tests and alerts</span></span> |
| <span data-ttu-id="eaa71-152">Läsare</span><span class="sxs-lookup"><span data-stu-id="eaa71-152">Reader</span></span> |<span data-ttu-id="eaa71-153">Kan visa men inte ändra någonting</span><span class="sxs-lookup"><span data-stu-id="eaa71-153">Can view but not change anything</span></span> |

<span data-ttu-id="eaa71-154">'Redigera' omfattar att skapa, ta bort och uppdatera:</span><span class="sxs-lookup"><span data-stu-id="eaa71-154">'Editing' includes creating, deleting and updating:</span></span>

* <span data-ttu-id="eaa71-155">Resurser</span><span class="sxs-lookup"><span data-stu-id="eaa71-155">Resources</span></span>
* <span data-ttu-id="eaa71-156">Webbtester</span><span class="sxs-lookup"><span data-stu-id="eaa71-156">Web tests</span></span>
* <span data-ttu-id="eaa71-157">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="eaa71-157">Alerts</span></span>
* <span data-ttu-id="eaa71-158">Löpande export</span><span class="sxs-lookup"><span data-stu-id="eaa71-158">Continuous export</span></span>

#### <a name="select-the-user"></a><span data-ttu-id="eaa71-159">Välj användaren</span><span class="sxs-lookup"><span data-stu-id="eaa71-159">Select the user</span></span>

<span data-ttu-id="eaa71-160">Om användaren inte är i katalogen, kan du bjuda in vem som helst med ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="eaa71-160">If the user you want isn't in the directory, you can invite anyone with a Microsoft account.</span></span>
<span data-ttu-id="eaa71-161">(Om de använder tjänster som Outlook.com, OneDrive, Windows Phone och XBox Live, har ett Microsoft-konto.)</span><span class="sxs-lookup"><span data-stu-id="eaa71-161">(If they use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, they have a Microsoft account.)</span></span>

## <a name="related-content"></a><span data-ttu-id="eaa71-162">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="eaa71-162">Related content</span></span>

* [<span data-ttu-id="eaa71-163">Rollbaserad åtkomstkontroll i Azure</span><span class="sxs-lookup"><span data-stu-id="eaa71-163">Role based access control in Azure</span></span>](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
