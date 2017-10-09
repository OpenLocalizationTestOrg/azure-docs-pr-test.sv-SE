---
title: "Självstudier: Azure Active Directory-integrering med Weekdone | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Weekdone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 34921f9a-5637-4420-ab4c-9beb34421909
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f997f1b49ff1aa0659a2409fdd945c6f96413b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-weekdone"></a><span data-ttu-id="6ee7b-103">Självstudier: Azure Active Directory-integrering med Weekdone</span><span class="sxs-lookup"><span data-stu-id="6ee7b-103">Tutorial: Azure Active Directory integration with Weekdone</span></span>

<span data-ttu-id="6ee7b-104">I kursen får du lära dig hur toointegrate Weekdone med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6ee7b-104">In this tutorial, you learn how toointegrate Weekdone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6ee7b-105">Integrera Weekdone med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6ee7b-105">Integrating Weekdone with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6ee7b-106">Du kan styra i Azure AD som har åtkomst till tooWeekdone</span><span class="sxs-lookup"><span data-stu-id="6ee7b-106">You can control in Azure AD who has access tooWeekdone</span></span>
- <span data-ttu-id="6ee7b-107">Du kan aktivera din användare tooautomatically get inloggade tooWeekdone (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6ee7b-107">You can enable your users tooautomatically get signed-on tooWeekdone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6ee7b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6ee7b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6ee7b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6ee7b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ee7b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6ee7b-110">Prerequisites</span></span>

<span data-ttu-id="6ee7b-111">tooconfigure Azure AD-integrering med Weekdone, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6ee7b-111">tooconfigure Azure AD integration with Weekdone, you need hello following items:</span></span>

- <span data-ttu-id="6ee7b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6ee7b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6ee7b-113">En Weekdone enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6ee7b-113">A Weekdone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6ee7b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6ee7b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6ee7b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6ee7b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6ee7b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6ee7b-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6ee7b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6ee7b-118">Scenario description</span></span>
<span data-ttu-id="6ee7b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6ee7b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6ee7b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6ee7b-121">Att lägga till Weekdone från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6ee7b-121">Adding Weekdone from hello gallery</span></span>
2. <span data-ttu-id="6ee7b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6ee7b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-weekdone-from-hello-gallery"></a><span data-ttu-id="6ee7b-123">Att lägga till Weekdone från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6ee7b-123">Adding Weekdone from hello gallery</span></span>
<span data-ttu-id="6ee7b-124">tooconfigure hello integrering av Weekdone i Azure AD, behöver du tooadd Weekdone hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-124">tooconfigure hello integration of Weekdone into Azure AD, you need tooadd Weekdone from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6ee7b-125">**tooadd Weekdone från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6ee7b-125">**tooadd Weekdone from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ee7b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6ee7b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6ee7b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6ee7b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6ee7b-133">Skriv i sökrutan hello **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-133">In hello search box, type **Weekdone**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_search.png)

5. <span data-ttu-id="6ee7b-135">Markera hello resultat på panelen **Weekdone**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-135">In hello results panel, select **Weekdone**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6ee7b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6ee7b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6ee7b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Weekdone baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-138">In this section, you configure and test Azure AD single sign-on with Weekdone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6ee7b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Weekdone är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Weekdone is tooa user in Azure AD.</span></span> <span data-ttu-id="6ee7b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Weekdone toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-140">In other words, a link relationship between an Azure AD user and hello related user in Weekdone needs toobe established.</span></span>

<span data-ttu-id="6ee7b-141">I Weekdone, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-141">In Weekdone, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6ee7b-142">tooconfigure och testa Azure AD enkel inloggning med Weekdone, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6ee7b-142">tooconfigure and test Azure AD single sign-on with Weekdone, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6ee7b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6ee7b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6ee7b-145">**[Skapa en testanvändare Weekdone](#creating-a-weekdone-test-user)**  -toohave en motsvarighet för Britta Simon i Weekdone som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-145">**[Creating a Weekdone test user](#creating-a-weekdone-test-user)** - toohave a counterpart of Britta Simon in Weekdone that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6ee7b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6ee7b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6ee7b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6ee7b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6ee7b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Weekdone program.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Weekdone application.</span></span>

<span data-ttu-id="6ee7b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Weekdone:**</span><span class="sxs-lookup"><span data-stu-id="6ee7b-150">**tooconfigure Azure AD single sign-on with Weekdone, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ee7b-151">I hello Azure-portalen på hello **Weekdone** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-151">In hello Azure portal, on hello **Weekdone** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6ee7b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_samlbase.png)

3. <span data-ttu-id="6ee7b-155">På hello **Weekdone domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="6ee7b-155">On hello **Weekdone Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url1.png)

    <span data-ttu-id="6ee7b-157">a.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-157">a.</span></span> <span data-ttu-id="6ee7b-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="6ee7b-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

    <span data-ttu-id="6ee7b-159">b.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-159">b.</span></span> <span data-ttu-id="6ee7b-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="6ee7b-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

4. <span data-ttu-id="6ee7b-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="6ee7b-162">Om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="6ee7b-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url2.png)

    <span data-ttu-id="6ee7b-164">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="6ee7b-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://weekdone.com/a/<tenantname>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="6ee7b-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-165">These values are not real.</span></span> <span data-ttu-id="6ee7b-166">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="6ee7b-167">Kontakta [Weekdone klienten supportteamet](mailto:hello@weekdone.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-167">Contact [Weekdone Client support team](mailto:hello@weekdone.com) tooget these values.</span></span> 

5. <span data-ttu-id="6ee7b-168">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-168">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_certificate.png) 

6. <span data-ttu-id="6ee7b-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="6ee7b-172">På hello **Weekdone Configuration** klickar du på **konfigurera Weekdone** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-172">On hello **Weekdone Configuration** section, click **Configure Weekdone** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6ee7b-173">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6ee7b-173">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_configure.png) 

8. <span data-ttu-id="6ee7b-175">tooconfigure enkel inloggning på **Weekdone** sida, behöver du toosend hello hämtas **XML-Metadata, Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** för[Weekdone stöd team](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="6ee7b-175">tooconfigure single sign-on on **Weekdone** side, you need toosend hello downloaded **Metadata XML, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Weekdone support team](mailto:hello@weekdone.com).</span></span>

> [!TIP]
> <span data-ttu-id="6ee7b-176">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6ee7b-176">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6ee7b-177">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-177">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6ee7b-178">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6ee7b-178">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6ee7b-179">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6ee7b-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="6ee7b-180">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-180">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6ee7b-182">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6ee7b-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ee7b-183">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-183">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6ee7b-185">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-185">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6ee7b-187">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-187">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6ee7b-189">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6ee7b-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6ee7b-191">a.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-191">a.</span></span> <span data-ttu-id="6ee7b-192">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6ee7b-193">b.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-193">b.</span></span> <span data-ttu-id="6ee7b-194">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6ee7b-195">c.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-195">c.</span></span> <span data-ttu-id="6ee7b-196">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6ee7b-197">d.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-197">d.</span></span> <span data-ttu-id="6ee7b-198">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-198">Click **Create**.</span></span>
 
### <a name="creating-a-weekdone-test-user"></a><span data-ttu-id="6ee7b-199">Skapa en testanvändare Weekdone</span><span class="sxs-lookup"><span data-stu-id="6ee7b-199">Creating a Weekdone test user</span></span>

<span data-ttu-id="6ee7b-200">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Weekdone.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-200">hello objective of this section is toocreate a user called Britta Simon in Weekdone.</span></span> <span data-ttu-id="6ee7b-201">Weekdone stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-201">Weekdone supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="6ee7b-202">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-202">There is no action item for you in this section.</span></span> <span data-ttu-id="6ee7b-203">En ny användare skapas under ett försök tooaccess Weekdone om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-203">A new user is created during an attempt tooaccess Weekdone if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="6ee7b-204">Om du behöver toocreate en användare manuellt, måste toocontact hello [Weekdone klienten supportteamet](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="6ee7b-204">If you need toocreate a user manually, you need toocontact hello [Weekdone Client support team](mailto:hello@weekdone.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6ee7b-205">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6ee7b-205">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6ee7b-206">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWeekdone.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-206">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWeekdone.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6ee7b-208">**tooassign Britta Simon tooWeekdone utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6ee7b-208">**tooassign Britta Simon tooWeekdone, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ee7b-209">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-209">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6ee7b-211">Välj i listan med program hello **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-211">In hello applications list, select **Weekdone**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_app.png) 

3. <span data-ttu-id="6ee7b-213">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-213">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6ee7b-215">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-215">Click **Add** button.</span></span> <span data-ttu-id="6ee7b-216">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-216">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6ee7b-218">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-218">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6ee7b-219">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-219">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6ee7b-220">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-220">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6ee7b-221">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6ee7b-221">Testing single sign-on</span></span>

<span data-ttu-id="6ee7b-222">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-222">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="6ee7b-223">Du bör få automatiskt inloggade tooyour Weekdone programmet när du klickar på hello Weekdone panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6ee7b-223">When you click hello Weekdone tile in hello Access Panel, you should get automatically signed-on tooyour Weekdone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ee7b-224">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6ee7b-224">Additional resources</span></span>

* [<span data-ttu-id="6ee7b-225">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6ee7b-225">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6ee7b-226">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6ee7b-226">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_203.png

