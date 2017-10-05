---
title: "Publicera program till enskilda användare i en Azure RemoteApp-samling (förhandsgranskning) | Microsoft Docs"
description: "Lär dig hur du kan publicera appar till enskilda användare, i stället för att vara beroende av grupper i Azure RemoteApp."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: piotrci
ms.openlocfilehash: c94ffffdec3e46ed08d941ee58dcf17b432e1dad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a><span data-ttu-id="4667d-103">Publicera program till enskilda användare i en Azure RemoteApp-samling (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="4667d-103">Publish applications to individual users in an Azure RemoteApp collection (Preview)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4667d-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="4667d-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="4667d-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="4667d-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="4667d-106">Den här artikeln beskriver hur du publicerar program till enskilda användare i en Azure RemoteApp-samling.</span><span class="sxs-lookup"><span data-stu-id="4667d-106">This article explains how to publish applications to individual users in an Azure RemoteApp collection.</span></span> <span data-ttu-id="4667d-107">Det här är en ny funktion i Azure RemoteApp. För tillfället är den endast tillgänglig för tidiga användare som en ”privat förhandsversion” för utvärderingsändamål.</span><span class="sxs-lookup"><span data-stu-id="4667d-107">This is new functionality in Azure RemoteApp, currently in private preview and available only to select early adopters for evaluation purposes.</span></span>

<span data-ttu-id="4667d-108">Från början gick det bara att publicera program på ett sätt med Azure RemoteApp: Administratören publicerade appar från avbildningen som sedan var tillgängliga för alla användare i samlingen.</span><span class="sxs-lookup"><span data-stu-id="4667d-108">Originally Azure RemoteApp enabled only one way of publishing applications: the administrator would publish apps from the image and they would be visible to all users in the collection.</span></span>

<span data-ttu-id="4667d-109">Ett vanligt scenario är att inkludera många program i en enda avbildning och distribuera en samling. På så sätt kan man minska hanteringskostnaderna.</span><span class="sxs-lookup"><span data-stu-id="4667d-109">A common scenario is to include many applications in a single image and deploy one collection in order to reduce management costs.</span></span> <span data-ttu-id="4667d-110">Ofta är inte alla program relevanta för alla användare – administratörer skulle föredra att publicera appar till enskilda användare så att de inte ser onödiga program i sin program-feed.</span><span class="sxs-lookup"><span data-stu-id="4667d-110">Oftentimes not all applications are relevant to all users – administrators would prefer to publish apps to individual users so they don’t see unnecessary applications in their application feed.</span></span>

<span data-ttu-id="4667d-111">Det är nu möjligt att göra i Azure RemoteApp – för tillfället är det en begränsad funktion i förhandsgranskningen .</span><span class="sxs-lookup"><span data-stu-id="4667d-111">This is now possible in Azure RemoteApp – currently as a limited preview feature.</span></span> <span data-ttu-id="4667d-112">Här är en kort sammanfattning av de nya funktionerna:</span><span class="sxs-lookup"><span data-stu-id="4667d-112">Here is a brief summary of the new functionality:</span></span>

1. <span data-ttu-id="4667d-113">En samling kan anges till ett av två lägen:</span><span class="sxs-lookup"><span data-stu-id="4667d-113">A collection can be set into one of two modes:</span></span>
   
   * <span data-ttu-id="4667d-114">Det ursprungliga ”samlingsläget” där alla användare i en samling kan se alla publicerade program.</span><span class="sxs-lookup"><span data-stu-id="4667d-114">the original collection mode, where all users in a collection can see all published applications.</span></span> <span data-ttu-id="4667d-115">Det är standardläget.</span><span class="sxs-lookup"><span data-stu-id="4667d-115">This is the default mode.</span></span>
   * <span data-ttu-id="4667d-116">Det nya ”programläget” där användarna bara ser program som uttryckligen har tilldelats dem.</span><span class="sxs-lookup"><span data-stu-id="4667d-116">the new application mode, where users only see applications that have been explicitly assigned to them</span></span>
2. <span data-ttu-id="4667d-117">Just nu kan programläget bara aktiveras med Azure RemoteApp PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="4667d-117">At the moment the application mode can only be enabled using Azure RemoteApp PowerShell cmdlets.</span></span>
   
   * <span data-ttu-id="4667d-118">I programläget går det inte att hantera användartilldelning i samlingen via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4667d-118">When set to application mode, user assignment in the collection cannot be managed through the Azure portal.</span></span> <span data-ttu-id="4667d-119">Användartilldelning måste hanteras via PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="4667d-119">User assignment has to be managed through PowerShell cmdlets.</span></span>
3. <span data-ttu-id="4667d-120">Användarna ser bara de program som publicerats direkt till dem.</span><span class="sxs-lookup"><span data-stu-id="4667d-120">Users will only see the applications published directly to them.</span></span> <span data-ttu-id="4667d-121">Men det kan fortfarande vara möjligt för användaren att starta andra program som är tillgängliga på bilden genom att öppna dem direkt i operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="4667d-121">However, it may still be possible for a user to launch the other applications available on the image by accessing them directly in the operating system.</span></span>
   
   * <span data-ttu-id="4667d-122">Den här funktionen ger inte en säker låsning av program, den begränsar bara synligheten i denna program-feed.</span><span class="sxs-lookup"><span data-stu-id="4667d-122">This feature does not provide a secure lockdown of applications; it only limits visibility in the application feed.</span></span>
   * <span data-ttu-id="4667d-123">Om du behöver isolera användare från program måste du använda separata samlingar för det.</span><span class="sxs-lookup"><span data-stu-id="4667d-123">If you need to isolate users from applications, you will need to use separate collections for that.</span></span>

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a><span data-ttu-id="4667d-124">Hur du hämtar Azure RemoteApp PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="4667d-124">How to get Azure RemoteApp PowerShell cmdlets</span></span>
<span data-ttu-id="4667d-125">Om du vill prova den nya funktionen för förhandsgranskning behöver du använda Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="4667d-125">To try the new preview functionality, you will need to use Azure PowerShell cmdlets.</span></span> <span data-ttu-id="4667d-126">För tillfället går det inte att aktivera det nya läget för programpublicering via Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="4667d-126">It is currently not possible to use the Azure Management portal to enable the new application publishing mode.</span></span>

<span data-ttu-id="4667d-127">Kontrollera först att du har [Azure PowerShell-modulen](/powershell/azure/overview) installerad.</span><span class="sxs-lookup"><span data-stu-id="4667d-127">First, make sure you have the [Azure PowerShell module](/powershell/azure/overview) installed.</span></span>

<span data-ttu-id="4667d-128">Starta sedan PowerShell-konsolen i administratörsläge och kör följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="4667d-128">Then launch the PowerShell console in administrator mode and run the following cmdlet:</span></span>

        Add-AzureAccount

<span data-ttu-id="4667d-129">Du uppmanas att ange användarnamn och lösenord för Azure.</span><span class="sxs-lookup"><span data-stu-id="4667d-129">It will prompt you for your Azure user name and password.</span></span> <span data-ttu-id="4667d-130">När du har loggat in kommer du att kunna köra Azure RemoteApp-cmdlets mot dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="4667d-130">Once signed in, you will be able to run Azure RemoteApp cmdlets against your Azure subscriptions.</span></span>

## <a name="how-to-check-which-mode-a-collection-is-in"></a><span data-ttu-id="4667d-131">Kontrollera vilket läge en samling är i</span><span class="sxs-lookup"><span data-stu-id="4667d-131">How to check which mode a collection is in</span></span>
<span data-ttu-id="4667d-132">Kör följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="4667d-132">Run the following cmdlet:</span></span>

        Get-AzureRemoteAppCollection <collectionName>

![Kontrollera samlingsläget](./media/remoteapp-perapp/araacllelvel.png)

<span data-ttu-id="4667d-134">AclLevel-egenskapen kan ha följande värden:</span><span class="sxs-lookup"><span data-stu-id="4667d-134">The AclLevel property can have the following values:</span></span>

* <span data-ttu-id="4667d-135">Samling: det ursprungliga publiceringsläget.</span><span class="sxs-lookup"><span data-stu-id="4667d-135">Collection: the original publishing mode.</span></span> <span data-ttu-id="4667d-136">Alla användare ser alla publicerade appar.</span><span class="sxs-lookup"><span data-stu-id="4667d-136">All users see all published apps.</span></span>
* <span data-ttu-id="4667d-137">Program: det nya publiceringsläget.</span><span class="sxs-lookup"><span data-stu-id="4667d-137">Application: the new publishing mode.</span></span> <span data-ttu-id="4667d-138">Användarna ser bara de appar som publiceras direkt till dem.</span><span class="sxs-lookup"><span data-stu-id="4667d-138">Users see only the apps published directly to them.</span></span>

## <a name="how-to-switch-to-application-publishing-mode"></a><span data-ttu-id="4667d-139">Växla till läget för programpublicering</span><span class="sxs-lookup"><span data-stu-id="4667d-139">How to switch to application publishing mode</span></span>
<span data-ttu-id="4667d-140">Kör följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="4667d-140">Run the following cmdlet:</span></span>

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

<span data-ttu-id="4667d-141">Programmets publiceringstillstånd bevaras: Till en början ser alla användare alla appar som ursprungligen har publicerats.</span><span class="sxs-lookup"><span data-stu-id="4667d-141">Application publishing state will be preserved: initially all users will see all of the original published apps.</span></span>

## <a name="how-to-list-users-who-can-see-a-specific-application"></a><span data-ttu-id="4667d-142">Visa en lista över användare som kan se ett visst program</span><span class="sxs-lookup"><span data-stu-id="4667d-142">How to list users who can see a specific application</span></span>
<span data-ttu-id="4667d-143">Kör följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="4667d-143">Run the following cmdlet:</span></span>

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

<span data-ttu-id="4667d-144">Detta visar alla användare som kan se programmet.</span><span class="sxs-lookup"><span data-stu-id="4667d-144">This lists all users who can see the application.</span></span>

<span data-ttu-id="4667d-145">Obs! Du kan visa programalias (kallas ”app-alias” i ovanstående syntax) genom att köra Get-AzureRemoteAppProgram – Samlingsnamn <collectionName>.</span><span class="sxs-lookup"><span data-stu-id="4667d-145">Note: You can see the application aliases (called "app alias" in the syntax above) by running Get-AzureRemoteAppProgram -CollectionName <collectionName>.</span></span>

## <a name="how-to-assign-an-application-to-a-user"></a><span data-ttu-id="4667d-146">Tilldela ett program till en användare</span><span class="sxs-lookup"><span data-stu-id="4667d-146">How to assign an application to a user</span></span>
<span data-ttu-id="4667d-147">Kör följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="4667d-147">Run the following cmdlet:</span></span>

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

<span data-ttu-id="4667d-148">Användaren ser nu programmet i Azure RemoteApp-klienten och kan ansluta till det.</span><span class="sxs-lookup"><span data-stu-id="4667d-148">The user will now see the application in the Azure RemoteApp client and will be able to connect to it.</span></span>

## <a name="how-to-remove-an-application-from-a-user"></a><span data-ttu-id="4667d-149">Ta bort ett program från en användare</span><span class="sxs-lookup"><span data-stu-id="4667d-149">How to remove an application from a user</span></span>
<span data-ttu-id="4667d-150">Kör följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="4667d-150">Run the following cmdlet:</span></span>

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a><span data-ttu-id="4667d-151">Skicka feedback</span><span class="sxs-lookup"><span data-stu-id="4667d-151">Providing feedback</span></span>
<span data-ttu-id="4667d-152">Vi uppskattar din feedback och dina förslag om den här funktionen för förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="4667d-152">We appreciate your feedback and suggestions regarding this preview feature.</span></span> <span data-ttu-id="4667d-153">Fyll i vår [undersökning](http://www.instant.ly/s/FDdrb) och berätta vad du tycker.</span><span class="sxs-lookup"><span data-stu-id="4667d-153">Please fill out the [survey](http://www.instant.ly/s/FDdrb) to let us know what you think.</span></span>

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a><span data-ttu-id="4667d-154">Har inte haft möjlighet att prova förhandsgranskningen?</span><span class="sxs-lookup"><span data-stu-id="4667d-154">Haven't had a chance to try the preview feature?</span></span>
<span data-ttu-id="4667d-155">Om du ännu inte har deltagit i förhandsgranskningen använder du denna [undersökning](http://www.instant.ly/s/AY83p) om du vill begära åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4667d-155">If you have not participated in the preview yet, please use this [survey](http://www.instant.ly/s/AY83p) to request access.</span></span>

