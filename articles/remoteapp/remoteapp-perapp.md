---
title: "aaaPublish program tooindividual användare i Azure RemoteApp-samling (förhandsgranskning) | Microsoft Docs"
description: "Lär dig hur du kan publicera appar tooindividual användare, i stället för beroende av grupper i Azure RemoteApp."
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
ms.openlocfilehash: 87b435552ddbfc2c6d03aeb01d95a44985e71f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-tooindividual-users-in-an-azure-remoteapp-collection-preview"></a><span data-ttu-id="c5aca-103">Publicera program tooindividual användare i Azure RemoteApp-samling (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="c5aca-103">Publish applications tooindividual users in an Azure RemoteApp collection (Preview)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c5aca-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="c5aca-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="c5aca-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="c5aca-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="c5aca-106">Den här artikeln förklarar hur toopublish program tooindividual användare i en Azure RemoteApp-samling.</span><span class="sxs-lookup"><span data-stu-id="c5aca-106">This article explains how toopublish applications tooindividual users in an Azure RemoteApp collection.</span></span> <span data-ttu-id="c5aca-107">Detta är en ny funktion i Azure RemoteApp, för tillfället i privat förhandsvisning och finns bara tooselect Tidiga efterföljare i utvärderingssyfte.</span><span class="sxs-lookup"><span data-stu-id="c5aca-107">This is new functionality in Azure RemoteApp, currently in private preview and available only tooselect early adopters for evaluation purposes.</span></span>

<span data-ttu-id="c5aca-108">Innebar av bara ett sätt att publicera program i Azure RemoteApp: hello administratören publicerade appar från hello avbildningen och de skulle vara synliga tooall användare i hello samling.</span><span class="sxs-lookup"><span data-stu-id="c5aca-108">Originally Azure RemoteApp enabled only one way of publishing applications: hello administrator would publish apps from hello image and they would be visible tooall users in hello collection.</span></span>

<span data-ttu-id="c5aca-109">Ett vanligt scenario är tooinclude många program i en enda avbildning och distribuera en samling i ordning tooreduce hanteringskostnader.</span><span class="sxs-lookup"><span data-stu-id="c5aca-109">A common scenario is tooinclude many applications in a single image and deploy one collection in order tooreduce management costs.</span></span> <span data-ttu-id="c5aca-110">Ofta är inte alla program som är relevanta tooall användare – administratörer skulle föredra toopublish appar tooindividual användare så att de inte ser onödiga program i deras program-feed.</span><span class="sxs-lookup"><span data-stu-id="c5aca-110">Oftentimes not all applications are relevant tooall users – administrators would prefer toopublish apps tooindividual users so they don’t see unnecessary applications in their application feed.</span></span>

<span data-ttu-id="c5aca-111">Det är nu möjligt att göra i Azure RemoteApp – för tillfället är det en begränsad funktion i förhandsgranskningen .</span><span class="sxs-lookup"><span data-stu-id="c5aca-111">This is now possible in Azure RemoteApp – currently as a limited preview feature.</span></span> <span data-ttu-id="c5aca-112">Här är en kort sammanfattning av hello nya funktioner:</span><span class="sxs-lookup"><span data-stu-id="c5aca-112">Here is a brief summary of hello new functionality:</span></span>

1. <span data-ttu-id="c5aca-113">En samling kan anges till ett av två lägen:</span><span class="sxs-lookup"><span data-stu-id="c5aca-113">A collection can be set into one of two modes:</span></span>
   
   * <span data-ttu-id="c5aca-114">hello ursprungliga samling-läge, där alla användare i en samling kan se publicerade alla program.</span><span class="sxs-lookup"><span data-stu-id="c5aca-114">hello original collection mode, where all users in a collection can see all published applications.</span></span> <span data-ttu-id="c5aca-115">Det här är standardläget för hello.</span><span class="sxs-lookup"><span data-stu-id="c5aca-115">This is hello default mode.</span></span>
   * <span data-ttu-id="c5aca-116">hello nya programläge, där användarna bara ser program som har tilldelats explicit toothem</span><span class="sxs-lookup"><span data-stu-id="c5aca-116">hello new application mode, where users only see applications that have been explicitly assigned toothem</span></span>
2. <span data-ttu-id="c5aca-117">Hello tillfället kan hello programläge endast aktiveras med hjälp av Azure RemoteApp PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c5aca-117">At hello moment hello application mode can only be enabled using Azure RemoteApp PowerShell cmdlets.</span></span>
   
   * <span data-ttu-id="c5aca-118">När Användartilldelning i samlingen hello-uppsättningen tooapplication läge inte kan hanteras via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c5aca-118">When set tooapplication mode, user assignment in hello collection cannot be managed through hello Azure portal.</span></span> <span data-ttu-id="c5aca-119">Användartilldelning måste toobe hanteras via PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c5aca-119">User assignment has toobe managed through PowerShell cmdlets.</span></span>
3. <span data-ttu-id="c5aca-120">Användarna ser bara hello program publiceras direkt toothem.</span><span class="sxs-lookup"><span data-stu-id="c5aca-120">Users will only see hello applications published directly toothem.</span></span> <span data-ttu-id="c5aca-121">Men kan det fortfarande vara möjligt för en användare toolaunch hello andra program som är tillgängliga på hello avbildning genom att öppna dem direkt i hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="c5aca-121">However, it may still be possible for a user toolaunch hello other applications available on hello image by accessing them directly in hello operating system.</span></span>
   
   * <span data-ttu-id="c5aca-122">Den här funktionen innehåller inte en säker låsning av program. den begränsar bara synligheten i hello program-feed.</span><span class="sxs-lookup"><span data-stu-id="c5aca-122">This feature does not provide a secure lockdown of applications; it only limits visibility in hello application feed.</span></span>
   * <span data-ttu-id="c5aca-123">Om du behöver tooisolate användare från program behöver toouse separata samlingar för som.</span><span class="sxs-lookup"><span data-stu-id="c5aca-123">If you need tooisolate users from applications, you will need toouse separate collections for that.</span></span>

## <a name="how-tooget-azure-remoteapp-powershell-cmdlets"></a><span data-ttu-id="c5aca-124">Hur tooget Azure RemoteApp PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="c5aca-124">How tooget Azure RemoteApp PowerShell cmdlets</span></span>
<span data-ttu-id="c5aca-125">tootry hello nya funktionen för förhandsgranskning, behöver du toouse Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c5aca-125">tootry hello new preview functionality, you will need toouse Azure PowerShell cmdlets.</span></span> <span data-ttu-id="c5aca-126">Det är för närvarande inte möjligt toouse hello Azure Management portal tooenable hello nya läget för programpublicering.</span><span class="sxs-lookup"><span data-stu-id="c5aca-126">It is currently not possible toouse hello Azure Management portal tooenable hello new application publishing mode.</span></span>

<span data-ttu-id="c5aca-127">Kontrollera först att du har hello [Azure PowerShell-modulen](/powershell/azure/overview) installerad.</span><span class="sxs-lookup"><span data-stu-id="c5aca-127">First, make sure you have hello [Azure PowerShell module](/powershell/azure/overview) installed.</span></span>

<span data-ttu-id="c5aca-128">Starta hello PowerShell-konsolen i administratörsläge och kör hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="c5aca-128">Then launch hello PowerShell console in administrator mode and run hello following cmdlet:</span></span>

        Add-AzureAccount

<span data-ttu-id="c5aca-129">Du uppmanas att ange användarnamn och lösenord för Azure.</span><span class="sxs-lookup"><span data-stu-id="c5aca-129">It will prompt you for your Azure user name and password.</span></span> <span data-ttu-id="c5aca-130">När du är inloggad kommer du att kunna toorun Azure RemoteApp-cmdlets mot dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c5aca-130">Once signed in, you will be able toorun Azure RemoteApp cmdlets against your Azure subscriptions.</span></span>

## <a name="how-toocheck-which-mode-a-collection-is-in"></a><span data-ttu-id="c5aca-131">Hur toocheck vilket läge en samling är i</span><span class="sxs-lookup"><span data-stu-id="c5aca-131">How toocheck which mode a collection is in</span></span>
<span data-ttu-id="c5aca-132">Kör följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="c5aca-132">Run hello following cmdlet:</span></span>

        Get-AzureRemoteAppCollection <collectionName>

![Kontrollera samlingsläget hello](./media/remoteapp-perapp/araacllelvel.png)

<span data-ttu-id="c5aca-134">Hej AclLevel-egenskapen kan ha hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="c5aca-134">hello AclLevel property can have hello following values:</span></span>

* <span data-ttu-id="c5aca-135">Samling: hello ursprungliga publiceringsläget.</span><span class="sxs-lookup"><span data-stu-id="c5aca-135">Collection: hello original publishing mode.</span></span> <span data-ttu-id="c5aca-136">Alla användare ser alla publicerade appar.</span><span class="sxs-lookup"><span data-stu-id="c5aca-136">All users see all published apps.</span></span>
* <span data-ttu-id="c5aca-137">Program: hello nya publiceringsläget.</span><span class="sxs-lookup"><span data-stu-id="c5aca-137">Application: hello new publishing mode.</span></span> <span data-ttu-id="c5aca-138">Användarna ser bara hello appar direkt publicerade toothem.</span><span class="sxs-lookup"><span data-stu-id="c5aca-138">Users see only hello apps published directly toothem.</span></span>

## <a name="how-tooswitch-tooapplication-publishing-mode"></a><span data-ttu-id="c5aca-139">Hur tooswitch tooapplication publiceringsläget</span><span class="sxs-lookup"><span data-stu-id="c5aca-139">How tooswitch tooapplication publishing mode</span></span>
<span data-ttu-id="c5aca-140">Kör följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="c5aca-140">Run hello following cmdlet:</span></span>

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

<span data-ttu-id="c5aca-141">Programmets publiceringstillstånd bevaras: till en början ser alla användare alla hello ursprungliga publicerade appar.</span><span class="sxs-lookup"><span data-stu-id="c5aca-141">Application publishing state will be preserved: initially all users will see all of hello original published apps.</span></span>

## <a name="how-toolist-users-who-can-see-a-specific-application"></a><span data-ttu-id="c5aca-142">Hur toolist användare som kan se ett visst program</span><span class="sxs-lookup"><span data-stu-id="c5aca-142">How toolist users who can see a specific application</span></span>
<span data-ttu-id="c5aca-143">Kör följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="c5aca-143">Run hello following cmdlet:</span></span>

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

<span data-ttu-id="c5aca-144">Detta visar alla användare som kan se hello program.</span><span class="sxs-lookup"><span data-stu-id="c5aca-144">This lists all users who can see hello application.</span></span>

<span data-ttu-id="c5aca-145">Obs: Du kan se hello programalias (kallas ”app-alias” i hello ovanstående syntax) genom att köra Get-AzureRemoteAppProgram – samlingsnamn <collectionName>.</span><span class="sxs-lookup"><span data-stu-id="c5aca-145">Note: You can see hello application aliases (called "app alias" in hello syntax above) by running Get-AzureRemoteAppProgram -CollectionName <collectionName>.</span></span>

## <a name="how-tooassign-an-application-tooa-user"></a><span data-ttu-id="c5aca-146">Hur tooassign en tooa programanvändare</span><span class="sxs-lookup"><span data-stu-id="c5aca-146">How tooassign an application tooa user</span></span>
<span data-ttu-id="c5aca-147">Kör följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="c5aca-147">Run hello following cmdlet:</span></span>

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

<span data-ttu-id="c5aca-148">hello användaren ser nu programmet hello i hello Azure RemoteApp-klienten och kommer att kunna tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="c5aca-148">hello user will now see hello application in hello Azure RemoteApp client and will be able tooconnect tooit.</span></span>

## <a name="how-tooremove-an-application-from-a-user"></a><span data-ttu-id="c5aca-149">Hur tooremove ett program från en användare</span><span class="sxs-lookup"><span data-stu-id="c5aca-149">How tooremove an application from a user</span></span>
<span data-ttu-id="c5aca-150">Kör följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="c5aca-150">Run hello following cmdlet:</span></span>

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a><span data-ttu-id="c5aca-151">Skicka feedback</span><span class="sxs-lookup"><span data-stu-id="c5aca-151">Providing feedback</span></span>
<span data-ttu-id="c5aca-152">Vi uppskattar din feedback och dina förslag om den här funktionen för förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="c5aca-152">We appreciate your feedback and suggestions regarding this preview feature.</span></span> <span data-ttu-id="c5aca-153">Fyll i hello [undersökning](http://www.instant.ly/s/FDdrb) toolet oss om vad du tycker.</span><span class="sxs-lookup"><span data-stu-id="c5aca-153">Please fill out hello [survey](http://www.instant.ly/s/FDdrb) toolet us know what you think.</span></span>

## <a name="havent-had-a-chance-tootry-hello-preview-feature"></a><span data-ttu-id="c5aca-154">Har inte haft ett chansen tootry hello förhandsgranskningsfunktion?</span><span class="sxs-lookup"><span data-stu-id="c5aca-154">Haven't had a chance tootry hello preview feature?</span></span>
<span data-ttu-id="c5aca-155">Om du inte har deltagit i förhandsgranskningen hello ännu, använder du denna [undersökning](http://www.instant.ly/s/AY83p) toorequest åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c5aca-155">If you have not participated in hello preview yet, please use this [survey](http://www.instant.ly/s/AY83p) toorequest access.</span></span>

