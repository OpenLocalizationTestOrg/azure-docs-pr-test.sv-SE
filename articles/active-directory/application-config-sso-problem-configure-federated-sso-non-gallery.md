---
title: "Problem som konfigurerar federerad enkel inloggning för ett icke-galleriet program | Microsoft Docs"
description: "Vissa av de vanliga problem som kan uppstå när du konfigurerar federerad enkel inloggning till din anpassade SAML-program som inte finns med i Azure AD Application Gallery"
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
ms.openlocfilehash: 4eb2c04c940dd6ad15a491a331aed76c237f0387
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="3b4d3-103">Konfigurera federerad enkel inloggning för ett icke-galleriet program problem</span><span class="sxs-lookup"><span data-stu-id="3b4d3-103">Problem configuring federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="3b4d3-104">Om du stöter på problem när du konfigurerar ett program.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="3b4d3-105">Kontrollera att du har följt alla steg i artikeln [Konfigurera enkel inloggning för program som inte ingår i Azure Active Directory-programgalleriet.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span><span class="sxs-lookup"><span data-stu-id="3b4d3-105">Verify you have followed all the steps in the article [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span></span>

## <a name="cant-add-another-instance-of-the-application"></a><span data-ttu-id="3b4d3-106">Det går inte att lägga till en annan instans av programmet</span><span class="sxs-lookup"><span data-stu-id="3b4d3-106">Can’t add another instance of the application</span></span>

<span data-ttu-id="3b4d3-107">Om du vill lägga till en andra instans av ett program, måste du kunna:</span><span class="sxs-lookup"><span data-stu-id="3b4d3-107">To add a second instance of an application, you need to be able to:</span></span>

-   <span data-ttu-id="3b4d3-108">Konfigurera en unik identifierare för den andra instansen.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-108">Configure a unique identifier for the second instance.</span></span> <span data-ttu-id="3b4d3-109">Du kan inte konfigurera den samma identifierare som används för den första instansen.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-109">You won’t be able to configure the same identifier used for the first instance.</span></span>

-   <span data-ttu-id="3b4d3-110">Konfigurera ett annat certifikat än den som används för den första instansen.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-110">Configure a different certificate than the one used for the first instance.</span></span>

<span data-ttu-id="3b4d3-111">Om programmet inte stöder någon av ovanstående.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-111">If the application doesn’t support any of the above.</span></span> <span data-ttu-id="3b4d3-112">Sedan kan du inte konfigurera en andra instans.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-112">Then, you won’t be able to configure a second instance.</span></span>

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a><span data-ttu-id="3b4d3-113">Där anger formatet ID för entiteterna (användar-ID)</span><span class="sxs-lookup"><span data-stu-id="3b4d3-113">Where do I set the EntityID (User Identifier) format</span></span>

<span data-ttu-id="3b4d3-114">Du kommer inte att kunna välja det ID för entiteterna (användar-ID)-format som Azure AD skickar till programmet i svaret efter autentisering av användare.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-114">You won’t be able to select the EntityID (User Identifier) format that Azure AD sends to the application in the response after user authentication.</span></span>

<span data-ttu-id="3b4d3-115">Azure AD-Välj format för attributet NameID (användar-ID) baserat på värdet valt eller formatet programmet har begärt i SAML-AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-115">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="3b4d3-116">Mer information finns i artikeln [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under avsnittet NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="3b4d3-116">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy,</span></span>

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a><span data-ttu-id="3b4d3-117">Var hittar jag programmetadata eller certifikat från Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b4d3-117">Where do I get the application metadata or certificate from Azure AD</span></span>

<span data-ttu-id="3b4d3-118">Följ stegen nedan för att ladda ned programmetadata eller certifikat från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="3b4d3-118">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="3b4d3-119">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="3b4d3-119">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3b4d3-120">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-120">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3b4d3-121">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-121">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3b4d3-122">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-122">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3b4d3-123">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-123">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="3b4d3-124">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="3b4d3-124">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3b4d3-125">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-125">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="3b4d3-126">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-126">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3b4d3-127">Gå till **SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-127">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="3b4d3-128">Beroende på vilka programmet kräver att konfigurera enkel inloggning, finns antingen alternativet för att hämta Metadata XML eller certifikatet.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-128">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="3b4d3-129">Azure AD Ange inte en URL för att hämta metadata.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-129">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="3b4d3-130">Metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-130">The metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a><span data-ttu-id="3b4d3-131">Vet inte hur du anpassar SAML anspråk skickas till ett program</span><span class="sxs-lookup"><span data-stu-id="3b4d3-131">Don't know how to customize SAML claims sent to an application</span></span>

<span data-ttu-id="3b4d3-132">Information om hur du anpassar SAML attributet anspråk som skickas till ditt program finns [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3b4d3-132">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b4d3-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b4d3-133">Next steps</span></span>
[<span data-ttu-id="3b4d3-134">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b4d3-134">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
