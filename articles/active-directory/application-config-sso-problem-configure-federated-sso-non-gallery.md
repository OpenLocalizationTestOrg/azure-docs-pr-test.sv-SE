---
title: "aaaProblem konfigurera federerad enkel inloggning för ett icke-galleriet program | Microsoft Docs"
description: "Vissa av hello vanliga problem som kan uppstå när du konfigurerar federerade tooyour för enkel inloggning anpassat SAML program som inte finns med i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 8c80f0001de01cbc9930ef0441cd804082ee8578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="23cea-103">Konfigurera federerad enkel inloggning för ett icke-galleriet program problem</span><span class="sxs-lookup"><span data-stu-id="23cea-103">Problem configuring federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="23cea-104">Om du stöter på problem när du konfigurerar ett program.</span><span class="sxs-lookup"><span data-stu-id="23cea-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="23cea-105">Kontrollera att du har följt alla hello stegen i artikeln hello [Konfigurera enkel inloggning tooapplications som inte ingår i hello Azure Active Directory-programgalleriet.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span><span class="sxs-lookup"><span data-stu-id="23cea-105">Verify you have followed all hello steps in hello article [Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span></span>

## <a name="cant-add-another-instance-of-hello-application"></a><span data-ttu-id="23cea-106">Det går inte att lägga till en annan instans av programmet hello</span><span class="sxs-lookup"><span data-stu-id="23cea-106">Can’t add another instance of hello application</span></span>

<span data-ttu-id="23cea-107">tooadd en andra instans av ett program måste toobe kunna:</span><span class="sxs-lookup"><span data-stu-id="23cea-107">tooadd a second instance of an application, you need toobe able to:</span></span>

-   <span data-ttu-id="23cea-108">Konfigurera en unik identifierare för andra instans av hello.</span><span class="sxs-lookup"><span data-stu-id="23cea-108">Configure a unique identifier for hello second instance.</span></span> <span data-ttu-id="23cea-109">Du kommer inte att kunna tooconfigure hello samma ID som används för hello första instansen.</span><span class="sxs-lookup"><span data-stu-id="23cea-109">You won’t be able tooconfigure hello same identifier used for hello first instance.</span></span>

-   <span data-ttu-id="23cea-110">Konfigurera ett annat certifikat än hello som används för hello första instansen.</span><span class="sxs-lookup"><span data-stu-id="23cea-110">Configure a different certificate than hello one used for hello first instance.</span></span>

<span data-ttu-id="23cea-111">Om hello stöder programmet inte någon av hello ovan.</span><span class="sxs-lookup"><span data-stu-id="23cea-111">If hello application doesn’t support any of hello above.</span></span> <span data-ttu-id="23cea-112">Du kan sedan inte kan tooconfigure en andra instans.</span><span class="sxs-lookup"><span data-stu-id="23cea-112">Then, you won’t be able tooconfigure a second instance.</span></span>

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a><span data-ttu-id="23cea-113">Där anger hello ID för entiteterna (användar-ID)-formatet</span><span class="sxs-lookup"><span data-stu-id="23cea-113">Where do I set hello EntityID (User Identifier) format</span></span>

<span data-ttu-id="23cea-114">Du kommer inte att kunna tooselect hello ID för entiteterna (användar-ID) format att Azure AD skickar toohello programmet hello svar efter autentisering av användare.</span><span class="sxs-lookup"><span data-stu-id="23cea-114">You won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="23cea-115">Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="23cea-115">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="23cea-116">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="23cea-116">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy,</span></span>

## <a name="where-do-i-get-hello-application-metadata-or-certificate-from-azure-ad"></a><span data-ttu-id="23cea-117">Var hittar jag hello programmetadata eller certifikat från Azure AD</span><span class="sxs-lookup"><span data-stu-id="23cea-117">Where do I get hello application metadata or certificate from Azure AD</span></span>

<span data-ttu-id="23cea-118">toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="23cea-118">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="23cea-119">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="23cea-119">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23cea-120">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="23cea-120">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23cea-121">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="23cea-121">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23cea-122">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="23cea-122">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23cea-123">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="23cea-123">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="23cea-124">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="23cea-124">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="23cea-125">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="23cea-125">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="23cea-126">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="23cea-126">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23cea-127">Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="23cea-127">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="23cea-128">Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="23cea-128">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="23cea-129">Azure AD Ange inte en URL tooget hello metadata.</span><span class="sxs-lookup"><span data-stu-id="23cea-129">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="23cea-130">hello metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="23cea-130">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a><span data-ttu-id="23cea-131">Vet inte hur toocustomize SAML anspråk skickas tooan program</span><span class="sxs-lookup"><span data-stu-id="23cea-131">Don't know how toocustomize SAML claims sent tooan application</span></span>

<span data-ttu-id="23cea-132">toolearn hur toocustomize hello SAML attributet anspråk skickas tooyour program finns i [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="23cea-132">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23cea-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23cea-133">Next steps</span></span>
[<span data-ttu-id="23cea-134">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="23cea-134">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
