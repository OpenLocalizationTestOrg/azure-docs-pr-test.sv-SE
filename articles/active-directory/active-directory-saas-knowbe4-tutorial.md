---
title: "Självstudier: Azure Active Directory-integrering med KnowBe4 medvetenhet säkerhetsutbildning | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och utbildning för KnowBe4 säkerhet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 907fa814b82c9ffb2376f73470b746a37104c66e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a><span data-ttu-id="3f126-103">Självstudier: Azure Active Directory-integrering med KnowBe4 säkerhetsutbildning medvetenhet</span><span class="sxs-lookup"><span data-stu-id="3f126-103">Tutorial: Azure Active Directory integration with KnowBe4 Security Awareness Training</span></span>

<span data-ttu-id="3f126-104">I kursen får du lära dig hur toointegrate KnowBe4 medvetenhet säkerhetsutbildning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3f126-104">In this tutorial, you learn how toointegrate KnowBe4 Security Awareness Training with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f126-105">Integrera KnowBe4 medvetenhet säkerhetsutbildning med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3f126-105">Integrating KnowBe4 Security Awareness Training with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3f126-106">Du kan styra i Azure AD som har åtkomst till tooKnowBe4 säkerhetsutbildning medvetenhet</span><span class="sxs-lookup"><span data-stu-id="3f126-106">You can control in Azure AD who has access tooKnowBe4 Security Awareness Training</span></span>
- <span data-ttu-id="3f126-107">Du kan aktivera användarna tooautomatically hämta inloggade tooKnowBe4 medvetenhet säkerhetsutbildning (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3f126-107">You can enable your users tooautomatically get signed-on tooKnowBe4 Security Awareness Training (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f126-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3f126-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3f126-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f126-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f126-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3f126-110">Prerequisites</span></span>

<span data-ttu-id="3f126-111">tooconfigure Azure AD-integrering med KnowBe4 medvetenhet säkerhetsutbildning måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3f126-111">tooconfigure Azure AD integration with KnowBe4 Security Awareness Training, you need hello following items:</span></span>

- <span data-ttu-id="3f126-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f126-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f126-113">En KnowBe4 medvetenhet säkerhetsutbildning enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f126-113">A KnowBe4 Security Awareness Training single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f126-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3f126-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f126-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3f126-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f126-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3f126-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f126-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f126-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f126-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3f126-118">Scenario description</span></span>
<span data-ttu-id="3f126-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3f126-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f126-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3f126-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f126-121">Att lägga till KnowBe4 medvetenhet säkerhetsutbildning från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3f126-121">Adding KnowBe4 Security Awareness Training from hello gallery</span></span>
2. <span data-ttu-id="3f126-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f126-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-knowbe4-security-awareness-training-from-hello-gallery"></a><span data-ttu-id="3f126-123">Att lägga till KnowBe4 medvetenhet säkerhetsutbildning från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3f126-123">Adding KnowBe4 Security Awareness Training from hello gallery</span></span>
<span data-ttu-id="3f126-124">tooconfigure hello integrering av KnowBe4 medvetenhet säkerhetsutbildning i Azure AD, behöver du tooadd KnowBe4 medvetenhet säkerhetsutbildning hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3f126-124">tooconfigure hello integration of KnowBe4 Security Awareness Training into Azure AD, you need tooadd KnowBe4 Security Awareness Training from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3f126-125">**tooadd KnowBe4 medvetenhet säkerhetsutbildning från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f126-125">**tooadd KnowBe4 Security Awareness Training from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f126-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3f126-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f126-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3f126-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3f126-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="3f126-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3f126-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f126-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3f126-133">Skriv i sökrutan hello **KnowBe4 medvetenhet säkerhetsutbildning**.</span><span class="sxs-lookup"><span data-stu-id="3f126-133">In hello search box, type **KnowBe4 Security Awareness Training**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_search.png)

5. <span data-ttu-id="3f126-135">Markera hello resultat på panelen **KnowBe4 medvetenhet säkerhetsutbildning**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="3f126-135">In hello results panel, select **KnowBe4 Security Awareness Training**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f126-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f126-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f126-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med KnowBe4 medvetenhet säkerhetsutbildning baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3f126-138">In this section, you configure and test Azure AD single sign-on with KnowBe4 Security Awareness Training based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3f126-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i KnowBe4 medvetenhet säkerhetsutbildning är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f126-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in KnowBe4 Security Awareness Training is tooa user in Azure AD.</span></span> <span data-ttu-id="3f126-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i KnowBe4 medvetenhet säkerhetsutbildning toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="3f126-140">In other words, a link relationship between an Azure AD user and hello related user in KnowBe4 Security Awareness Training needs toobe established.</span></span>

<span data-ttu-id="3f126-141">Tilldela hello värdet för hello i KnowBe4 för medvetenhet säkerhetsutbildning **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3f126-141">In KnowBe4 Security Awareness Training, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3f126-142">tooconfigure och testa Azure AD enkel inloggning med KnowBe4 säkerhetsutbildning medvetenhet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3f126-142">tooconfigure and test Azure AD single sign-on with KnowBe4 Security Awareness Training, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3f126-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3f126-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3f126-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f126-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f126-145">**[Skapa en testanvändare KnowBe4 medvetenhet säkerhetsutbildning](#creating-a-knowbe4-security-awareness-training-test-user)**  -toohave en motsvarighet för Britta Simon i KnowBe4 medvetenhet säkerhetsutbildning som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3f126-145">**[Creating a KnowBe4 Security Awareness Training test user](#creating-a-knowbe4-security-awareness-training-test-user)** - toohave a counterpart of Britta Simon in KnowBe4 Security Awareness Training that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f126-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f126-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f126-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3f126-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f126-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f126-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f126-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i KnowBe4 säkerhetsutbildning medvetenhet om programmet.</span><span class="sxs-lookup"><span data-stu-id="3f126-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your KnowBe4 Security Awareness Training application.</span></span>

<span data-ttu-id="3f126-150">**tooconfigure Azure AD enkel inloggning med KnowBe4 för medvetenhet säkerhetsutbildning utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f126-150">**tooconfigure Azure AD single sign-on with KnowBe4 Security Awareness Training, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f126-151">I hello Azure-portalen på hello **KnowBe4 medvetenhet säkerhetsutbildning** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3f126-151">In hello Azure portal, on hello **KnowBe4 Security Awareness Training** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3f126-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f126-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_samlbase.png)

3. <span data-ttu-id="3f126-155">På hello **KnowBe4 säkerhetsdomän medvetenhet utbildning och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f126-155">On hello **KnowBe4 Security Awareness Training Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_url.png)

    <span data-ttu-id="3f126-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.KnowBe4.com/auth/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="3f126-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.KnowBe4.com/auth/saml/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f126-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3f126-158">hello value is not real.</span></span> <span data-ttu-id="3f126-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="3f126-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="3f126-160">Kontakta [KnowBe4 Säkerhetsklient medvetenhet utbildning supportteamet](mailto:support@KnowBe4.com) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="3f126-160">Contact [KnowBe4 Security Awareness Training Client support team](mailto:support@KnowBe4.com) tooget hello value.</span></span> 
 

4. <span data-ttu-id="3f126-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="3f126-161">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_certificate.png) 

5. <span data-ttu-id="3f126-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f126-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f126-165">På hello **KnowBe4 säkerhetskonfiguration medvetenhet utbildning** klickar du på **konfigurera KnowBe4 medvetenhet säkerhetsutbildning** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3f126-165">On hello **KnowBe4 Security Awareness Training Configuration** section, click **Configure KnowBe4 Security Awareness Training** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3f126-166">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3f126-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_configure.png) 

7. <span data-ttu-id="3f126-168">tooconfigure enkel inloggning på **KnowBe4 medvetenhet säkerhetsutbildning** sida, behöver du toosend hello hämtas **certifikat (Raw)**, **Sign-Out URL SAML enhets-ID och SAML enkel inloggning Tjänst-URL** för[KnowBe4 Säkerhetsklient medvetenhet utbildning supportteamet](mailto:support@KnowBe4.com).</span><span class="sxs-lookup"><span data-stu-id="3f126-168">tooconfigure single sign-on on **KnowBe4 Security Awareness Training** side, you need toosend hello downloaded **Certificate (Raw)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[KnowBe4 Security Awareness Training Client support team](mailto:support@KnowBe4.com).</span></span>

> [!TIP]
> <span data-ttu-id="3f126-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="3f126-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3f126-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="3f126-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3f126-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f126-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f126-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f126-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f126-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f126-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3f126-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f126-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f126-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3f126-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f126-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3f126-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f126-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f126-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f126-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f126-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f126-184">a.</span><span class="sxs-lookup"><span data-stu-id="3f126-184">a.</span></span> <span data-ttu-id="3f126-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f126-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f126-186">b.</span><span class="sxs-lookup"><span data-stu-id="3f126-186">b.</span></span> <span data-ttu-id="3f126-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f126-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f126-188">c.</span><span class="sxs-lookup"><span data-stu-id="3f126-188">c.</span></span> <span data-ttu-id="3f126-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3f126-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3f126-190">d.</span><span class="sxs-lookup"><span data-stu-id="3f126-190">d.</span></span> <span data-ttu-id="3f126-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3f126-191">Click **Create**.</span></span>
 
### <a name="creating-a-knowbe4-security-awareness-training-test-user"></a><span data-ttu-id="3f126-192">Skapa en KnowBe4 medvetenhet säkerhetsutbildning testanvändare</span><span class="sxs-lookup"><span data-stu-id="3f126-192">Creating a KnowBe4 Security Awareness Training test user</span></span>

<span data-ttu-id="3f126-193">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i KnowBe4 medvetenhet säkerhetsutbildning.</span><span class="sxs-lookup"><span data-stu-id="3f126-193">hello objective of this section is toocreate a user called Britta Simon in KnowBe4 Security Awareness Training.</span></span> <span data-ttu-id="3f126-194">KnowBe4 medvetenhet säkerhetsutbildning stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="3f126-194">KnowBe4 Security Awareness Training supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="3f126-195">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3f126-195">There is no action item for you in this section.</span></span> <span data-ttu-id="3f126-196">En ny användare skapas under ett försök tooaccess KnowBe4 säkerhetsutbildning medvetenhet om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="3f126-196">A new user is created during an attempt tooaccess KnowBe4 Security Awareness Training if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="3f126-197">Om du behöver toocreate en användare manuellt, måste toocontact hello [KnowBe4 medvetenhet säkerhetsutbildning supportteamet](mailto:support@KnowBe4.com).</span><span class="sxs-lookup"><span data-stu-id="3f126-197">If you need toocreate a user manually, you need toocontact hello [KnowBe4 Security Awareness Training support team](mailto:support@KnowBe4.com).</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3f126-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f126-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3f126-199">I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst till tooKnowBe4 medvetenhet säkerhetsutbildning.</span><span class="sxs-lookup"><span data-stu-id="3f126-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKnowBe4 Security Awareness Training.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3f126-201">**tooassign Britta Simon tooKnowBe4 medvetenhet säkerhetsutbildning, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="3f126-201">**tooassign Britta Simon tooKnowBe4 Security Awareness Training, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f126-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3f126-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3f126-204">Välj i listan med program hello **KnowBe4 medvetenhet säkerhetsutbildning**.</span><span class="sxs-lookup"><span data-stu-id="3f126-204">In hello applications list, select **KnowBe4 Security Awareness Training**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_app.png) 

3. <span data-ttu-id="3f126-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3f126-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3f126-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f126-208">Click **Add** button.</span></span> <span data-ttu-id="3f126-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f126-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3f126-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="3f126-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3f126-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f126-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f126-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f126-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f126-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f126-214">Testing single sign-on</span></span>

<span data-ttu-id="3f126-215">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f126-215">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>
  
<span data-ttu-id="3f126-216">Du bör få automatiskt inloggade tooyour KnowBe4 medvetenhet säkerhetsutbildning programmet när du klickar på hello KnowBe4 medvetenhet säkerhetsutbildning panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f126-216">When you click hello KnowBe4 Security Awareness Training tile in hello Access Panel, you should get automatically signed-on tooyour KnowBe4 Security Awareness Training application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f126-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3f126-217">Additional resources</span></span>

* [<span data-ttu-id="3f126-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3f126-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f126-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3f126-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_203.png

