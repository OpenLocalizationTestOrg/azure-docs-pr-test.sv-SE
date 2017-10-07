---
title: "Självstudier: Azure Active Directory-integrering med Tangoe kommandot Premium Mobile | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Tangoe kommandot Premium Mobile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 7bd1cd6afade58a4a47a173b249f92eb84e42112
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a><span data-ttu-id="c65d4-103">Självstudier: Azure Active Directory-integrering med Tangoe kommandot Premium Mobile</span><span class="sxs-lookup"><span data-stu-id="c65d4-103">Tutorial: Azure Active Directory integration with Tangoe Command Premium Mobile</span></span>

<span data-ttu-id="c65d4-104">I kursen får du lära dig hur toointegrate Tangoe kommandot Premium mobil med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c65d4-104">In this tutorial, you learn how toointegrate Tangoe Command Premium Mobile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c65d4-105">Integrera Tangoe kommandot Premium Mobile med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c65d4-105">Integrating Tangoe Command Premium Mobile with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c65d4-106">Du kan styra i Azure AD som har åtkomst tooTangoe kommandot Premium Mobile</span><span class="sxs-lookup"><span data-stu-id="c65d4-106">You can control in Azure AD who has access tooTangoe Command Premium Mobile</span></span>
- <span data-ttu-id="c65d4-107">Du kan aktivera din användare tooautomatically get inloggade tooTangoe kommandot Premium Mobile (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c65d4-107">You can enable your users tooautomatically get signed-on tooTangoe Command Premium Mobile (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c65d4-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c65d4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c65d4-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c65d4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c65d4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c65d4-110">Prerequisites</span></span>

<span data-ttu-id="c65d4-111">tooconfigure Azure AD-integrering med Tangoe kommandot Premium Mobile måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c65d4-111">tooconfigure Azure AD integration with Tangoe Command Premium Mobile, you need hello following items:</span></span>

- <span data-ttu-id="c65d4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c65d4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c65d4-113">En Tangoe kommandot Premium Mobile enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c65d4-113">A Tangoe Command Premium Mobile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c65d4-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c65d4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c65d4-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c65d4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c65d4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c65d4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c65d4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c65d4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c65d4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c65d4-118">Scenario description</span></span>
<span data-ttu-id="c65d4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c65d4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c65d4-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c65d4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c65d4-121">Lägg till Tangoe kommandot Premium Mobile från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c65d4-121">Add Tangoe Command Premium Mobile from hello gallery</span></span>
2. <span data-ttu-id="c65d4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c65d4-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tangoe-command-premium-mobile-from-hello-gallery"></a><span data-ttu-id="c65d4-123">Lägg till Tangoe kommandot Premium Mobile från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c65d4-123">Add Tangoe Command Premium Mobile from hello gallery</span></span>
<span data-ttu-id="c65d4-124">tooconfigure hello integrering av Tangoe kommandot Premium Mobile i Azure AD, behöver du tooadd Tangoe kommandot Premium Mobile hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c65d4-124">tooconfigure hello integration of Tangoe Command Premium Mobile into Azure AD, you need tooadd Tangoe Command Premium Mobile from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c65d4-125">**tooadd Tangoe kommando Premium Mobile från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c65d4-125">**tooadd Tangoe Command Premium Mobile from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65d4-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c65d4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c65d4-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c65d4-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c65d4-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c65d4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c65d4-133">Skriv i sökrutan hello **Tangoe kommandot Premium Mobile**väljer **Tangoe kommandot Premium Mobile** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c65d4-133">In hello search box, type **Tangoe Command Premium Mobile**, select **Tangoe Command Premium Mobile** from result panel then click **Add** button tooadd hello application.</span></span>

    ![<span data-ttu-id="c65d4-134">Lägg till Tangoe kommandot Premium Mobile från galleriet</span><span class="sxs-lookup"><span data-stu-id="c65d4-134">Add Tangoe Command Premium Mobile from gallery</span></span> ](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c65d4-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c65d4-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="c65d4-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Tangoe kommandot Premium Mobile baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c65d4-136">In this section, you configure and test Azure AD single sign-on with Tangoe Command Premium Mobile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c65d4-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Tangoe kommandot Premium Mobile är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c65d4-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tangoe Command Premium Mobile is tooa user in Azure AD.</span></span> <span data-ttu-id="c65d4-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i Tangoe kommandot Premium Mobile toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c65d4-138">In other words, a link relationship between an Azure AD user and hello related user in Tangoe Command Premium Mobile needs toobe established.</span></span>

<span data-ttu-id="c65d4-139">Tilldela hello värdet hello i Tangoe kommandot Premium Mobile **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c65d4-139">In Tangoe Command Premium Mobile, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c65d4-140">tooconfigure och testa Azure AD enkel inloggning med Tangoe kommandot Premium Mobile, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c65d4-140">tooconfigure and test Azure AD single sign-on with Tangoe Command Premium Mobile, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c65d4-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c65d4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c65d4-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c65d4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c65d4-143">**[Skapa en testanvändare Tangoe kommandot Premium Mobile](#create-a-tangoe-command-premium-mobile-test-user)**  -toohave en motsvarighet för Britta Simon i Tangoe kommandot Premium Mobile som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c65d4-143">**[Create a Tangoe Command Premium Mobile test user](#create-a-tangoe-command-premium-mobile-test-user)** - toohave a counterpart of Britta Simon in Tangoe Command Premium Mobile that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c65d4-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c65d4-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c65d4-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c65d4-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c65d4-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c65d4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c65d4-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Tangoe kommandot Premium mobila program.</span><span class="sxs-lookup"><span data-stu-id="c65d4-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tangoe Command Premium Mobile application.</span></span>

<span data-ttu-id="c65d4-148">**tooconfigure Azure AD enkel inloggning med Tangoe kommandot Premium Mobile utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c65d4-148">**tooconfigure Azure AD single sign-on with Tangoe Command Premium Mobile, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65d4-149">I hello Azure-portalen på hello **Tangoe kommandot Premium Mobile** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-149">In hello Azure portal, on hello **Tangoe Command Premium Mobile** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c65d4-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c65d4-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_samlbase.png)

3. <span data-ttu-id="c65d4-153">På hello **Tangoe kommandot Premium Mobile domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c65d4-153">On hello **Tangoe Command Premium Mobile Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Tangoe kommandot Premium mobila domän](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_url.png)

    <span data-ttu-id="c65d4-155">a.</span><span class="sxs-lookup"><span data-stu-id="c65d4-155">a.</span></span> <span data-ttu-id="c65d4-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span><span class="sxs-lookup"><span data-stu-id="c65d4-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span></span>

    <span data-ttu-id="c65d4-157">b.</span><span class="sxs-lookup"><span data-stu-id="c65d4-157">b.</span></span> <span data-ttu-id="c65d4-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://sso.tangoe.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="c65d4-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://sso.tangoe.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c65d4-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c65d4-159">These values are not real.</span></span> <span data-ttu-id="c65d4-160">Uppdatera dessa värden med hello faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="c65d4-160">Update these values with hello actual  Reply URL and Sign-On URL.</span></span> <span data-ttu-id="c65d4-161">Kontakta [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c65d4-161">Contact [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) tooget these values.</span></span> 

4. <span data-ttu-id="c65d4-162">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="c65d4-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_certificate.png) 

5. <span data-ttu-id="c65d4-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c65d4-164">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-tangoe-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="c65d4-166">På hello **Tangoe Premium Mobile konfiguration** klickar du på **konfigurera Tangoe kommandot Premium Mobile** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c65d4-166">On hello **Tangoe Command Premium Mobile Configuration** section, click **Configure Tangoe Command Premium Mobile** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c65d4-167">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c65d4-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurationsavsnittet Tangoe kommandot Premium Mobile](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_configure.png) 

7. <span data-ttu-id="c65d4-169">tooget SSO konfigurerats för ditt program bör du kontakta din [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) och ange hello följande:</span><span class="sxs-lookup"><span data-stu-id="c65d4-169">tooget SSO configured for your application, contact your [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) and provide hello following:</span></span>

   - <span data-ttu-id="c65d4-170">hello hämtade metadatafil</span><span class="sxs-lookup"><span data-stu-id="c65d4-170">hello downloaded metadata file</span></span>
   - <span data-ttu-id="c65d4-171">Hej **SAML enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="c65d4-171">hello **SAML Entity ID**</span></span>
   - <span data-ttu-id="c65d4-172">Hej **SAML enkel inloggning tjänst-URL**</span><span class="sxs-lookup"><span data-stu-id="c65d4-172">hello **SAML Single Sign-On Service URL**</span></span>
   - <span data-ttu-id="c65d4-173">Hej **Sign-Out URL**</span><span class="sxs-lookup"><span data-stu-id="c65d4-173">hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="c65d4-174">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="c65d4-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c65d4-175">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c65d4-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c65d4-176">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c65d4-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c65d4-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c65d4-177">Create an Azure AD test user</span></span>
<span data-ttu-id="c65d4-178">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c65d4-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c65d4-180">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c65d4-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65d4-181">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c65d4-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c65d4-183">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Användare och grupper -> alla användare](./media/active-directory-saas-tangoe-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c65d4-185">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c65d4-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Lägga till användare](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c65d4-187">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c65d4-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogrutan Användarsida](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c65d4-189">a.</span><span class="sxs-lookup"><span data-stu-id="c65d4-189">a.</span></span> <span data-ttu-id="c65d4-190">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c65d4-191">b.</span><span class="sxs-lookup"><span data-stu-id="c65d4-191">b.</span></span> <span data-ttu-id="c65d4-192">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c65d4-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c65d4-193">c.</span><span class="sxs-lookup"><span data-stu-id="c65d4-193">c.</span></span> <span data-ttu-id="c65d4-194">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c65d4-195">d.</span><span class="sxs-lookup"><span data-stu-id="c65d4-195">d.</span></span> <span data-ttu-id="c65d4-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-196">Click **Create**.</span></span>
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a><span data-ttu-id="c65d4-197">Skapa en Tangoe kommandot Premium mobila användare</span><span class="sxs-lookup"><span data-stu-id="c65d4-197">Create a Tangoe Command Premium Mobile test user</span></span>

<span data-ttu-id="c65d4-198">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Tangoe kommandot Premium Mobile.</span><span class="sxs-lookup"><span data-stu-id="c65d4-198">In this section, you create a user called Britta Simon in Tangoe Command Premium Mobile.</span></span> 

<span data-ttu-id="c65d4-199">Tangoe kommandot Premium Mobile-programmet måste alla hello användare toobe etableras i hello program innan du utför enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c65d4-199">Tangoe Command Premium Mobile application needs all hello users toobe provisioned in hello application before doing Single Sign On.</span></span> <span data-ttu-id="c65d4-200">Så kan du arbeta med hello [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) tooprovision dessa användare till hello program.</span><span class="sxs-lookup"><span data-stu-id="c65d4-200">So please work with hello [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) tooprovision all these users into hello application.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c65d4-201">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c65d4-201">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c65d4-202">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTangoe kommandot Premium Mobile.</span><span class="sxs-lookup"><span data-stu-id="c65d4-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTangoe Command Premium Mobile.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c65d4-204">**tooassign Britta Simon tooTangoe kommandot Premium Mobile utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c65d4-204">**tooassign Britta Simon tooTangoe Command Premium Mobile, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65d4-205">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c65d4-207">Välj i listan med program hello **Tangoe kommandot Premium Mobile**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-207">In hello applications list, select **Tangoe Command Premium Mobile**.</span></span>

    ![Tangoe kommandot Premium Mobile i listan över appar](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_app.png) 

3. <span data-ttu-id="c65d4-209">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c65d4-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c65d4-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c65d4-211">Click **Add** button.</span></span> <span data-ttu-id="c65d4-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c65d4-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c65d4-214">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c65d4-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c65d4-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c65d4-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c65d4-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c65d4-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c65d4-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c65d4-217">Test single sign-on</span></span>

<span data-ttu-id="c65d4-218">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c65d4-218">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="c65d4-219">När du klickar på hello Tangoe kommandot Premium Mobile panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Tangoe kommandot Premium Mobile-programmet.</span><span class="sxs-lookup"><span data-stu-id="c65d4-219">When you click hello Tangoe Command Premium Mobile tile in hello Access Panel, you should get automatically signed-on tooyour Tangoe Command Premium Mobile application.</span></span> <span data-ttu-id="c65d4-220">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c65d4-220">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c65d4-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c65d4-221">Additional resources</span></span>

* [<span data-ttu-id="c65d4-222">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c65d4-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c65d4-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c65d4-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png

