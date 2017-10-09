---
title: "aaaHow tooconfigure användaretablering tooan Azure AD-galleriet program | Microsoft Docs"
description: "Hur du snabbt kan konfigurera omfattande användarkonto etablering och borttagning tooapplications redan visas i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 2c28e59a3ac8f221ed93b2f6b0b1221f7604af23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-user-provisioning-tooan-azure-ad-gallery-application"></a><span data-ttu-id="664f5-103">Hur tooconfigure användaretablering tooan Azure AD-galleriet program</span><span class="sxs-lookup"><span data-stu-id="664f5-103">How tooconfigure user provisioning tooan Azure AD Gallery application</span></span>

<span data-ttu-id="664f5-104">*Etablering av konto* är hello handling för att skapa, uppdatera eller inaktiverar kontot användarposter i ett program lokal profil användararkivet.</span><span class="sxs-lookup"><span data-stu-id="664f5-104">*User account provisioning* is hello act of creating, updating, and/or disabling user account records in an application’s local user profile store.</span></span> <span data-ttu-id="664f5-105">De flesta moln och SaaS-program lagrar hello användarrollen och behörigheterna i sina egna lokala användararkivet profil och förekomsten av sådana en användarpost i sina lokala arkivet är *krävs* för enkel inloggning och åtkomst toowork.</span><span class="sxs-lookup"><span data-stu-id="664f5-105">Most cloud and SaaS applications store hello users role and permissions in their own local user profile store, and presence of such a user record in their local store is *required* for single sign-on and access toowork.</span></span>

<span data-ttu-id="664f5-106">I hello Azure-portalen, hello **etablering** fliken i hello vänstra navigeringsfönstret för en Enterprise-App visar vilka etablering lägen stöds för appen.</span><span class="sxs-lookup"><span data-stu-id="664f5-106">In hello Azure portal, hello **Provisioning** tab in hello left navigation pane for an Enterprise App displays what provisioning modes are supported for that app.</span></span> <span data-ttu-id="664f5-107">Detta kan vara ett av två värden:</span><span class="sxs-lookup"><span data-stu-id="664f5-107">This can be one of two values:</span></span>

## <a name="configuring-an-application-for-manual-provisioning"></a><span data-ttu-id="664f5-108">Konfigurera ett program för manuell etablering</span><span class="sxs-lookup"><span data-stu-id="664f5-108">Configuring an application for Manual Provisioning</span></span>

<span data-ttu-id="664f5-109">*Manuell* etablering innebär att användarkonton måste skapas manuellt med hjälp av hello-metoder som tillhandahålls av appen.</span><span class="sxs-lookup"><span data-stu-id="664f5-109">*Manual* provisioning means that user accounts must be created manually using hello methods provided by that app.</span></span> <span data-ttu-id="664f5-110">Detta kan betyda att logga in på en administrativ portal för appen och lägga till användare som använder ett webbaserat användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="664f5-110">This could mean logging into an administrative portal for that app and adding users using a web-based user interface.</span></span> <span data-ttu-id="664f5-111">Eller överför ett kalkylblad med kontoinformation för användare med hjälp av en mekanism som tillhandahålls av programmet.</span><span class="sxs-lookup"><span data-stu-id="664f5-111">Or it could be uploading a spreadsheet with user account detail, using a mechanism provided by that application.</span></span> <span data-ttu-id="664f5-112">Dokumentationen hello angivna hello app eller kontakta hello app developer toodetermine wat metoder är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="664f5-112">Consult hello documentation provided by hello app, or contact hello app developer toodetermine wat mechanisms are available.</span></span>

<span data-ttu-id="664f5-113">Om manuell visas hello läge för ett visst program innebär det att ingen automatisk Azure AD connector-etablering har skapats för hello app ännu.</span><span class="sxs-lookup"><span data-stu-id="664f5-113">If Manual is hello only mode shown for a given application, it means that no automatic Azure AD provisioning connector has been created for hello app yet.</span></span> <span data-ttu-id="664f5-114">Eller innebär hello appen har inte stöd för hello förutvärdering användaren management API på vilka toobuild en automatisk etablering koppling.</span><span class="sxs-lookup"><span data-stu-id="664f5-114">Or it means hello app does not support hello pre-requisite user management API upon which toobuild an automated provisioning connector.</span></span>

<span data-ttu-id="664f5-115">Om du vill toorequest stöd för automatisk etablering för en viss app kan du fylla i en begäran i <http://aka.ms/aadapprequest>.</span><span class="sxs-lookup"><span data-stu-id="664f5-115">If you would like toorequest support for automatic provisioning for a given app, you can fill out a request at <http://aka.ms/aadapprequest>.</span></span>

## <a name="configuring-an-application-for-automatic-provisioning"></a><span data-ttu-id="664f5-116">Konfigurera ett program för automatisk etablering</span><span class="sxs-lookup"><span data-stu-id="664f5-116">Configuring an application for Automatic Provisioning</span></span>

<span data-ttu-id="664f5-117">*Automatisk* innebär att en Azure AD connector-etablering har utvecklats för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="664f5-117">*Automatic* means that an Azure AD provisioning connector has been developed for this application.</span></span> <span data-ttu-id="664f5-118">Mer information om hello Azure AD etablering tjänsten och hur det fungerar, se [automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span><span class="sxs-lookup"><span data-stu-id="664f5-118">For more information on hello Azure AD provisioning service and how it works, see [Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span></span>

<span data-ttu-id="664f5-119">Mer information om hur tooprovision specifika användare och grupper tooan program, se [hantera användaretablering konto för företagsappar](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span><span class="sxs-lookup"><span data-stu-id="664f5-119">For more information on how tooprovision specific users and groups tooan application, see [Managing user account provisioning for enterprise apps](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span></span>

<span data-ttu-id="664f5-120">Hej faktiska steg krävs tooenable och konfigurera automatisk etablering varierar beroende på programmet hello.</span><span class="sxs-lookup"><span data-stu-id="664f5-120">hello actual steps required tooenable and configure automatic provisioning vary depending on hello application.</span></span>

>[!NOTE]
><span data-ttu-id="664f5-121">Starta genom att söka efter hello installationsprogrammet självstudiekursen specifika toosetting in etablering för ditt program och följa dessa steg tooconfigure både hello-appen och Azure AD toocreate hello etablering anslutning.</span><span class="sxs-lookup"><span data-stu-id="664f5-121">You should start by finding hello setup tutorial specific toosetting up provisioning for your application, and following those steps tooconfigure both hello app and Azure AD toocreate hello provisioning connection.</span></span> 
>
>

<span data-ttu-id="664f5-122">Appen självstudier finns på [lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span><span class="sxs-lookup"><span data-stu-id="664f5-122">App tutorials can be found at [List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span></span>

<span data-ttu-id="664f5-123">En viktig sak tooconsider när du konfigurerar etablering vara tooreview och konfigurera hello attributmappning och arbetsflöden som definierar vilka egenskaper flödet för användare (eller grupp) från Azure AD toohello program.</span><span class="sxs-lookup"><span data-stu-id="664f5-123">An important thing tooconsider when setting up provisioning be tooreview and configure hello attribute mappings and workflows that define which user (or group) properties flow from Azure AD toohello application.</span></span> <span data-ttu-id="664f5-124">Detta innefattar att ställa in hello ”matchning av egenskap” som ska använda toouniquely identifiera och matchar användare eller grupper mellan hello två system.</span><span class="sxs-lookup"><span data-stu-id="664f5-124">This includes setting hello “matching property” that be used toouniquely identify and match users/groups between hello two systems.</span></span> <span data-ttu-id="664f5-125">Mer information om viktiga processen.</span><span class="sxs-lookup"><span data-stu-id="664f5-125">For more information on this important process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="664f5-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="664f5-126">Next steps</span></span>
[<span data-ttu-id="664f5-127">Anpassa attributmappning för Användaretablering för SaaS-program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="664f5-127">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

