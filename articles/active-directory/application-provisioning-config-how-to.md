---
title: "Konfigurera användaretablering till ett program för Azure AD-galleriet | Microsoft Docs"
description: "Hur du snabbt kan konfigurera omfattande användarkonto etablering och borttagning till program som redan finns i Azure AD Application Gallery"
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
ms.openlocfilehash: 2e38fcb30ea7632339a3ba8815a536872cfcc69e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-user-provisioning-to-an-azure-ad-gallery-application"></a><span data-ttu-id="5a0a4-103">Konfigurera användaretablering till ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="5a0a4-103">How to configure user provisioning to an Azure AD Gallery application</span></span>

<span data-ttu-id="5a0a4-104">*Etablering av konto* innebär att skapa, uppdatera och/eller inaktivera konto användarposter i ett program lokal profil användararkivet.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-104">*User account provisioning* is the act of creating, updating, and/or disabling user account records in an application’s local user profile store.</span></span> <span data-ttu-id="5a0a4-105">De flesta moln och SaaS-program lagrar användarrollen och behörigheterna i sina egna lokala användararkivet profil och förekomsten av sådana en användarpost i sina lokala arkivet är *krävs* för enkel inloggning och åtkomst till arbetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-105">Most cloud and SaaS applications store the users role and permissions in their own local user profile store, and presence of such a user record in their local store is *required* for single sign-on and access to work.</span></span>

<span data-ttu-id="5a0a4-106">I Azure-portalen på **etablering** fliken i det vänstra navigeringsfönstret för en Enterprise-App visar vilka etablering lägen stöds för appen.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-106">In the Azure portal, the **Provisioning** tab in the left navigation pane for an Enterprise App displays what provisioning modes are supported for that app.</span></span> <span data-ttu-id="5a0a4-107">Detta kan vara ett av två värden:</span><span class="sxs-lookup"><span data-stu-id="5a0a4-107">This can be one of two values:</span></span>

## <a name="configuring-an-application-for-manual-provisioning"></a><span data-ttu-id="5a0a4-108">Konfigurera ett program för manuell etablering</span><span class="sxs-lookup"><span data-stu-id="5a0a4-108">Configuring an application for Manual Provisioning</span></span>

<span data-ttu-id="5a0a4-109">*Manuell* etablering innebär att användarkonton måste skapas manuellt med hjälp av de metoder som tillhandahålls av appen.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-109">*Manual* provisioning means that user accounts must be created manually using the methods provided by that app.</span></span> <span data-ttu-id="5a0a4-110">Detta kan betyda att logga in på en administrativ portal för appen och lägga till användare som använder ett webbaserat användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-110">This could mean logging into an administrative portal for that app and adding users using a web-based user interface.</span></span> <span data-ttu-id="5a0a4-111">Eller överför ett kalkylblad med kontoinformation för användare med hjälp av en mekanism som tillhandahålls av programmet.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-111">Or it could be uploading a spreadsheet with user account detail, using a mechanism provided by that application.</span></span> <span data-ttu-id="5a0a4-112">I dokumentationen som tillhandahålls av appen eller kontakta apputvecklaren för att fastställa wat metoder är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-112">Consult the documentation provided by the app, or contact the app developer to determine wat mechanisms are available.</span></span>

<span data-ttu-id="5a0a4-113">Om manuell är det enda läge som visas för ett visst program, innebär det att ingen automatisk Azure AD connector-etablering har skapats för appen ännu.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-113">If Manual is the only mode shown for a given application, it means that no automatic Azure AD provisioning connector has been created for the app yet.</span></span> <span data-ttu-id="5a0a4-114">Eller det innebär att appen inte har stöd för förutvärdering användaren management API som du vill skapa en automatisk etablering koppling.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-114">Or it means the app does not support the pre-requisite user management API upon which to build an automated provisioning connector.</span></span>

<span data-ttu-id="5a0a4-115">Om du vill begära stöd för automatisk etablering för en viss app kan du fylla i en begäran i <http://aka.ms/aadapprequest>.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-115">If you would like to request support for automatic provisioning for a given app, you can fill out a request at <http://aka.ms/aadapprequest>.</span></span>

## <a name="configuring-an-application-for-automatic-provisioning"></a><span data-ttu-id="5a0a4-116">Konfigurera ett program för automatisk etablering</span><span class="sxs-lookup"><span data-stu-id="5a0a4-116">Configuring an application for Automatic Provisioning</span></span>

<span data-ttu-id="5a0a4-117">*Automatisk* innebär att en Azure AD connector-etablering har utvecklats för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-117">*Automatic* means that an Azure AD provisioning connector has been developed for this application.</span></span> <span data-ttu-id="5a0a4-118">Läs mer om Azure AD etablering tjänsten och hur det fungerar [automatisera Användaretablering och avetablering för SaaS-program med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span><span class="sxs-lookup"><span data-stu-id="5a0a4-118">For more information on the Azure AD provisioning service and how it works, see [Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span></span>

<span data-ttu-id="5a0a4-119">Mer information om hur du etablerar specifika användare och grupper till ett program finns i [hantera användaretablering konto för företagsappar](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span><span class="sxs-lookup"><span data-stu-id="5a0a4-119">For more information on how to provision specific users and groups to an application, see [Managing user account provisioning for enterprise apps](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span></span>

<span data-ttu-id="5a0a4-120">De faktiska steg som krävs för att aktivera och konfigurera automatisk etablering varierar beroende på programmet.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-120">The actual steps required to enable and configure automatic provisioning vary depending on the application.</span></span>

>[!NOTE]
><span data-ttu-id="5a0a4-121">Du bör börja med att söka efter installationen kursen specifika för att skapa etablering för ditt program och följa dem om du vill konfigurera både appen och Azure AD för att skapa etablering anslutningen.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-121">You should start by finding the setup tutorial specific to setting up provisioning for your application, and following those steps to configure both the app and Azure AD to create the provisioning connection.</span></span> 
>
>

<span data-ttu-id="5a0a4-122">Appen självstudier finns på [lista över självstudier om hur man integrerar SaaS-appar med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span><span class="sxs-lookup"><span data-stu-id="5a0a4-122">App tutorials can be found at [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span></span>

<span data-ttu-id="5a0a4-123">En viktig sak att tänka på när du ställer in etablering att granska och konfigurera attributmappning och arbetsflöden som definierar vilka användare (eller grupp) egenskaper flödar från Azure AD till programmet.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-123">An important thing to consider when setting up provisioning be to review and configure the attribute mappings and workflows that define which user (or group) properties flow from Azure AD to the application.</span></span> <span data-ttu-id="5a0a4-124">Detta inkluderar egenskapen ”matchande” som används för att identifiera och matchar användare eller grupper mellan de två systemen.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-124">This includes setting the “matching property” that be used to uniquely identify and match users/groups between the two systems.</span></span> <span data-ttu-id="5a0a4-125">Mer information om viktiga processen.</span><span class="sxs-lookup"><span data-stu-id="5a0a4-125">For more information on this important process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a0a4-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a0a4-126">Next steps</span></span>
[<span data-ttu-id="5a0a4-127">Anpassa attributmappning för Användaretablering för SaaS-program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5a0a4-127">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

