---
title: "aaaHow toodiagnose fel med hello guiden för Azure Active Directory-anslutning"
description: "hello active directory-Anslutningsguiden upptäckte ett inkompatibelt autentiseringstyp"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/05/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: f71c5b41457c0c8db05042e8d5f723e58ad11844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnosing-errors-with-hello-azure-active-directory-connection-wizard"></a><span data-ttu-id="ad9d4-103">Diagnostisera fel med hello guiden för Azure Active Directory-anslutning</span><span class="sxs-lookup"><span data-stu-id="ad9d4-103">Diagnosing errors with hello Azure Active Directory Connection Wizard</span></span>
<span data-ttu-id="ad9d4-104">Vid identifiering av föregående Autentiseringskod upptäckte hello guiden ett inkompatibelt autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-104">While detecting previous authentication code, hello wizard detected an incompatible authentication type.</span></span>   

## <a name="what-is-being-checked"></a><span data-ttu-id="ad9d4-105">Vad är kontrolleras?</span><span class="sxs-lookup"><span data-stu-id="ad9d4-105">What is being checked?</span></span>
<span data-ttu-id="ad9d4-106">**Obs:** toocorrectly identifiera tidigare Autentiseringskod i ett projekt, hello projektet måste skapas.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-106">**Note:** toocorrectly detect previous authentication code in a project, hello project must be built.</span></span>  <span data-ttu-id="ad9d4-107">Om detta fel uppstod och du inte har en tidigare Autentiseringskod i projektet, återskapa och försök igen.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-107">If you encountered this error and you don't have a previous authentication code in your project, rebuild and try again.</span></span>

### <a name="project-types"></a><span data-ttu-id="ad9d4-108">Projekttyper</span><span class="sxs-lookup"><span data-stu-id="ad9d4-108">Project Types</span></span>
<span data-ttu-id="ad9d4-109">hello guiden kontrollerar hello typ av projekt som du utvecklar så att den kan mata in hello rätt autentiseringslogiken i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-109">hello wizard checks hello type of project you’re developing so it can inject hello right authentication logic into hello project.</span></span>  <span data-ttu-id="ad9d4-110">Om det inte finns några domänkontrollanter som härleds från `ApiController` i hello-projektet hello projekt betraktas som ett WebAPI-projekt.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-110">If there is any controller that derives from `ApiController` in hello project, hello project is considered a WebAPI project.</span></span>  <span data-ttu-id="ad9d4-111">Om det finns endast domänkontrollanter som härleds från `MVC.Controller` i hello-projektet hello projekt betraktas som en MVC-projektet.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-111">If there are only controllers that derive from `MVC.Controller` in hello project, hello project is considered an MVC project.</span></span>  <span data-ttu-id="ad9d4-112">Något annat stöds inte av hello guiden.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-112">Anything else is not supported by hello wizard.</span></span>

### <a name="compatible-authentication-code"></a><span data-ttu-id="ad9d4-113">Kompatibel Autentiseringskod</span><span class="sxs-lookup"><span data-stu-id="ad9d4-113">Compatible Authentication Code</span></span>
<span data-ttu-id="ad9d4-114">hello guiden kontrollerar också autentiseringsinställningar som tidigare har konfigurerats med hello guiden eller som är kompatibel med hello guiden.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-114">hello wizard also checks for authentication settings that have been previously configured with hello wizard or are compatible with hello wizard.</span></span>  <span data-ttu-id="ad9d4-115">Om alla inställningar finns Reentrant fall är det och hello guiden öppnas visa hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-115">If all settings are present, it is considered a re-entrant case, and hello wizard opens display hello settings.</span></span>  <span data-ttu-id="ad9d4-116">Om endast vissa hello inställningar finns anses ett ärende för fel.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-116">If only some of hello settings are present, it is considered an error case.</span></span>

<span data-ttu-id="ad9d4-117">I ett projekt med MVC söker hello guiden efter någon av följande inställningar, som kommer från tidigare användning av hello guiden hello:</span><span class="sxs-lookup"><span data-stu-id="ad9d4-117">In an MVC project, hello wizard checks for any of hello following settings, which result from previous use of hello wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

<span data-ttu-id="ad9d4-118">Dessutom söker hello guiden efter någon av följande inställningar i ett Web API-projekt som kommer från tidigare användning av hello guiden hello:</span><span class="sxs-lookup"><span data-stu-id="ad9d4-118">In addition, hello wizard checks for any of hello following settings in a Web API project, which result from previous use of hello wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a><span data-ttu-id="ad9d4-119">Inkompatibel Autentiseringskod</span><span class="sxs-lookup"><span data-stu-id="ad9d4-119">Incompatible Authentication Code</span></span>
<span data-ttu-id="ad9d4-120">Slutligen försöker hello guiden toodetect versioner av Autentiseringskod som har konfigurerats med tidigare versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-120">Finally, hello wizard attempts toodetect versions of authentication code that have been configured with previous versions of Visual Studio.</span></span> <span data-ttu-id="ad9d4-121">Om du har fått det här felet betyder det att projektet innehåller en inkompatibel autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-121">If you received this error, it means your project contains an incompatible authentication type.</span></span> <span data-ttu-id="ad9d4-122">hello guiden identifierar hello följande typer av autentisering från tidigare versioner av Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ad9d4-122">hello wizard detects hello following types of authentication from previous versions of Visual Studio:</span></span>

* <span data-ttu-id="ad9d4-123">Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="ad9d4-123">Windows Authentication</span></span> 
* <span data-ttu-id="ad9d4-124">Enskilda användarkonton</span><span class="sxs-lookup"><span data-stu-id="ad9d4-124">Individual User Accounts</span></span> 
* <span data-ttu-id="ad9d4-125">Organisationskonton</span><span class="sxs-lookup"><span data-stu-id="ad9d4-125">Organizational Accounts</span></span> 

<span data-ttu-id="ad9d4-126">toodetect Windows-autentisering i ett projekt med MVC hello guiden söker efter hello `authentication` element från din **web.config** fil.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-126">toodetect Windows Authentication in an MVC project, hello wizard looks for hello `authentication` element from your **web.config** file.</span></span>

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="ad9d4-127">toodetect Windows-autentisering i ett Web API-projekt hello guiden söker efter hello `IISExpressWindowsAuthentication` element från ditt projekt **.csproj** fil:</span><span class="sxs-lookup"><span data-stu-id="ad9d4-127">toodetect Windows Authentication in a Web API project, hello wizard looks for hello `IISExpressWindowsAuthentication` element from your project's **.csproj** file:</span></span>

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

<span data-ttu-id="ad9d4-128">toodetect enskilda användarkonton autentisering hello guiden söker efter hello paketet element från din **Packages.config-fil** fil.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-128">toodetect Individual User Accounts authentication, hello wizard looks for hello package element from your **Packages.config** file.</span></span>

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

<span data-ttu-id="ad9d4-129">toodetect en gamla form av autentisering av Organisationskonto hello guiden söker efter hello följande element från **web.config**:</span><span class="sxs-lookup"><span data-stu-id="ad9d4-129">toodetect an old form of Organizational Account authentication, hello wizard looks for hello following element from **web.config**:</span></span>

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="ad9d4-130">toochange hello autentiseringstypen, ta bort hello inkompatibla autentiseringstyp och kör hello guiden igen.</span><span class="sxs-lookup"><span data-stu-id="ad9d4-130">toochange hello authentication type, remove hello incompatible authentication type and run hello wizard again.</span></span>

<span data-ttu-id="ad9d4-131">Mer information finns i [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="ad9d4-131">For more information, see [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md).</span></span>

#<a name="next-steps"></a><span data-ttu-id="ad9d4-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ad9d4-132">Next steps</span></span>
- [<span data-ttu-id="ad9d4-133">Autentiseringsscenarier för Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad9d4-133">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)