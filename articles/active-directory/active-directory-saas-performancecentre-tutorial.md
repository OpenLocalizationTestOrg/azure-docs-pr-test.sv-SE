---
title: "Självstudier: Azure Active Directory-integrering med PerformanceCentre | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och PerformanceCentre."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 19781c0087093a67c70dc90072cf1a119bb2ade0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a><span data-ttu-id="760e5-103">Självstudier: Azure Active Directory-integrering med PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="760e5-103">Tutorial: Azure Active Directory integration with PerformanceCentre</span></span>

<span data-ttu-id="760e5-104">I kursen får du lära dig hur toointegrate PerformanceCentre med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="760e5-104">In this tutorial, you learn how toointegrate PerformanceCentre with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="760e5-105">Integrera PerformanceCentre med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="760e5-105">Integrating PerformanceCentre with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="760e5-106">Du kan styra i Azure AD som har åtkomst till tooPerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="760e5-106">You can control in Azure AD who has access tooPerformanceCentre</span></span>
- <span data-ttu-id="760e5-107">Du kan aktivera din användare tooautomatically get inloggade tooPerformanceCentre (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="760e5-107">You can enable your users tooautomatically get signed-on tooPerformanceCentre (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="760e5-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="760e5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="760e5-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="760e5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="760e5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="760e5-110">Prerequisites</span></span>

<span data-ttu-id="760e5-111">tooconfigure Azure AD-integrering med PerformanceCentre, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="760e5-111">tooconfigure Azure AD integration with PerformanceCentre, you need hello following items:</span></span>

- <span data-ttu-id="760e5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="760e5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="760e5-113">En PerformanceCentre enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="760e5-113">A PerformanceCentre single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="760e5-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="760e5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="760e5-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="760e5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="760e5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="760e5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="760e5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="760e5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="760e5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="760e5-118">Scenario description</span></span>
<span data-ttu-id="760e5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="760e5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="760e5-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="760e5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="760e5-121">Att lägga till PerformanceCentre från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="760e5-121">Adding PerformanceCentre from hello gallery</span></span>
2. <span data-ttu-id="760e5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="760e5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-performancecentre-from-hello-gallery"></a><span data-ttu-id="760e5-123">Att lägga till PerformanceCentre från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="760e5-123">Adding PerformanceCentre from hello gallery</span></span>
<span data-ttu-id="760e5-124">tooconfigure hello integrering av PerformanceCentre i Azure AD, behöver du tooadd PerformanceCentre hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="760e5-124">tooconfigure hello integration of PerformanceCentre into Azure AD, you need tooadd PerformanceCentre from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="760e5-125">**tooadd PerformanceCentre från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="760e5-125">**tooadd PerformanceCentre from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="760e5-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="760e5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="760e5-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="760e5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="760e5-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="760e5-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="760e5-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="760e5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="760e5-133">Skriv i sökrutan hello **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="760e5-133">In hello search box, type **PerformanceCentre**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_search.png)

5. <span data-ttu-id="760e5-135">Markera hello resultat på panelen **PerformanceCentre**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="760e5-135">In hello results panel, select **PerformanceCentre**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="760e5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="760e5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="760e5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PerformanceCentre baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="760e5-138">In this section, you configure and test Azure AD single sign-on with PerformanceCentre based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="760e5-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i PerformanceCentre är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="760e5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PerformanceCentre is tooa user in Azure AD.</span></span> <span data-ttu-id="760e5-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i PerformanceCentre toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="760e5-140">In other words, a link relationship between an Azure AD user and hello related user in PerformanceCentre needs toobe established.</span></span>

<span data-ttu-id="760e5-141">I PerformanceCentre, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="760e5-141">In PerformanceCentre, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="760e5-142">tooconfigure och testa Azure AD enkel inloggning med PerformanceCentre, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="760e5-142">tooconfigure and test Azure AD single sign-on with PerformanceCentre, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="760e5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="760e5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="760e5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="760e5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="760e5-145">**[Skapa en testanvändare PerformanceCentre](#creating-a-performancecentre-test-user)**  -toohave en motsvarighet för Britta Simon i PerformanceCentre som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="760e5-145">**[Creating a PerformanceCentre test user](#creating-a-performancecentre-test-user)** - toohave a counterpart of Britta Simon in PerformanceCentre that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="760e5-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="760e5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="760e5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="760e5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="760e5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="760e5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="760e5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt PerformanceCentre program.</span><span class="sxs-lookup"><span data-stu-id="760e5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PerformanceCentre application.</span></span>

<span data-ttu-id="760e5-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med PerformanceCentre:**</span><span class="sxs-lookup"><span data-stu-id="760e5-150">**tooconfigure Azure AD single sign-on with PerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="760e5-151">I hello Azure-portalen på hello **PerformanceCentre** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="760e5-151">In hello Azure portal, on hello **PerformanceCentre** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="760e5-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="760e5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_samlbase.png)

3. <span data-ttu-id="760e5-155">På hello **PerformanceCentre domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="760e5-155">On hello **PerformanceCentre Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_url.png)

    <span data-ttu-id="760e5-157">a.</span><span class="sxs-lookup"><span data-stu-id="760e5-157">a.</span></span> <span data-ttu-id="760e5-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`http://companyname.performancecentre.com/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="760e5-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://companyname.performancecentre.com/saml/SSO`</span></span>

    <span data-ttu-id="760e5-159">b.</span><span class="sxs-lookup"><span data-stu-id="760e5-159">b.</span></span> <span data-ttu-id="760e5-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`http://companyname.performancecentre.com`</span><span class="sxs-lookup"><span data-stu-id="760e5-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://companyname.performancecentre.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="760e5-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="760e5-161">These values are not real.</span></span> <span data-ttu-id="760e5-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="760e5-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="760e5-163">Kontakta [PerformanceCentre klienten supportteamet](https://www.performancecentre.com/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="760e5-163">Contact [PerformanceCentre Client support team](https://www.performancecentre.com/contact-us/) tooget these values.</span></span> 

4. <span data-ttu-id="760e5-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="760e5-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_certificate.png) 

5. <span data-ttu-id="760e5-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="760e5-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="760e5-168">På hello **PerformanceCentre Configuration** klickar du på **konfigurera PerformanceCentre** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="760e5-168">On hello **PerformanceCentre Configuration** section, click **Configure PerformanceCentre** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="760e5-169">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="760e5-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_configure.png) 

7. <span data-ttu-id="760e5-171">Inloggning tooyour **PerformanceCentre** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="760e5-171">Sign-on tooyour **PerformanceCentre** company site as administrator.</span></span>

8. <span data-ttu-id="760e5-172">I fliken hello hello vänster, klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="760e5-172">In hello tab on hello left side, click **Configure**.</span></span>
   
    ![Azure AD-Single Sign-On][10]

9. <span data-ttu-id="760e5-174">I fliken hello hello vänster, klickar du på **diverse**, och klicka sedan på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="760e5-174">In hello tab on hello left side, click **Miscellaneous**, and then click **Single Sign On**.</span></span>
   
    ![Azure AD-Single Sign-On][11]

10. <span data-ttu-id="760e5-176">Som **protokollet**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="760e5-176">As **Protocol**, select **SAML**.</span></span>
   
    ![Azure AD-Single Sign-On][12]

11. <span data-ttu-id="760e5-178">Öppna metadatafilen i anteckningar, kopiera hello innehåll hämtas klistra in den i hello **identitet providern Metadata** textruta och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="760e5-178">Open your downloaded metadata file in notepad, copy hello content, paste it into hello **Identity Provider Metadata** textbox, and then click **Save**.</span></span>
   
    ![Azure AD-Single Sign-On][13]

12. <span data-ttu-id="760e5-180">Kontrollera att hello värden för hello **entitet bas-URL** och **entitet ID URL** är korrekta.</span><span class="sxs-lookup"><span data-stu-id="760e5-180">Verify that hello values for hello **Entity Base URL** and **Entity ID URL** are correct.</span></span>
    
     ![Azure AD-Single Sign-On][14]

> [!TIP]
> <span data-ttu-id="760e5-182">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="760e5-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="760e5-183">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="760e5-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="760e5-184">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="760e5-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="760e5-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="760e5-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="760e5-186">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="760e5-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="760e5-188">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="760e5-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="760e5-189">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="760e5-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="760e5-191">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="760e5-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="760e5-193">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="760e5-193">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="760e5-195">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="760e5-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="760e5-197">a.</span><span class="sxs-lookup"><span data-stu-id="760e5-197">a.</span></span> <span data-ttu-id="760e5-198">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="760e5-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="760e5-199">b.</span><span class="sxs-lookup"><span data-stu-id="760e5-199">b.</span></span> <span data-ttu-id="760e5-200">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="760e5-200">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="760e5-201">c.</span><span class="sxs-lookup"><span data-stu-id="760e5-201">c.</span></span> <span data-ttu-id="760e5-202">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="760e5-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="760e5-203">d.</span><span class="sxs-lookup"><span data-stu-id="760e5-203">d.</span></span> <span data-ttu-id="760e5-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="760e5-204">Click **Create**.</span></span>
 
### <a name="creating-a-performancecentre-test-user"></a><span data-ttu-id="760e5-205">Skapa en testanvändare PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="760e5-205">Creating a PerformanceCentre test user</span></span>

<span data-ttu-id="760e5-206">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="760e5-206">hello objective of this section is toocreate a user called Britta Simon in PerformanceCentre.</span></span>

<span data-ttu-id="760e5-207">**toocreate en användare som kallas Britta Simon i PerformanceCentre, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="760e5-207">**toocreate a user called Britta Simon in PerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="760e5-208">Inloggning tooyour PerformanceCentre företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="760e5-208">Sign on tooyour PerformanceCentre company site as administrator.</span></span>

2. <span data-ttu-id="760e5-209">Hello-menyn hello vänster **Interrelate**, och klicka sedan på **skapa deltagare**.</span><span class="sxs-lookup"><span data-stu-id="760e5-209">In hello menu on hello left, click **Interrelate**, and then click **Create Participant**.</span></span>
   
    ![Skapa användare][400]

3. <span data-ttu-id="760e5-211">På hello **samband - skapa deltagare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="760e5-211">On hello **Interrelate - Create Participant** dialog, perform hello following steps:</span></span>
   
    ![Skapa användare][401]
    
    <span data-ttu-id="760e5-213">a.</span><span class="sxs-lookup"><span data-stu-id="760e5-213">a.</span></span> <span data-ttu-id="760e5-214">Ange hello krävs attribut för Britta Simon till relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="760e5-214">Type hello required attributes for Britta Simon into related textboxes.</span></span>
    
    >[!IMPORTANT]
    ><span data-ttu-id="760e5-215">Britta's User Name-attribut i PerformanceCentre måste vara hello samma som hello användarnamn i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="760e5-215">Britta's User Name attribute in PerformanceCentre must be hello same as hello User Name in Azure AD.</span></span>
    
    <span data-ttu-id="760e5-216">b.</span><span class="sxs-lookup"><span data-stu-id="760e5-216">b.</span></span> <span data-ttu-id="760e5-217">Välj **administratör** som **Välj rollen**.</span><span class="sxs-lookup"><span data-stu-id="760e5-217">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="760e5-218">c.</span><span class="sxs-lookup"><span data-stu-id="760e5-218">c.</span></span> <span data-ttu-id="760e5-219">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="760e5-219">Click **Save**.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="760e5-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="760e5-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="760e5-221">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="760e5-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPerformanceCentre.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="760e5-223">**tooassign Britta Simon tooPerformanceCentre utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="760e5-223">**tooassign Britta Simon tooPerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="760e5-224">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="760e5-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="760e5-226">Välj i listan med program hello **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="760e5-226">In hello applications list, select **PerformanceCentre**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_app.png) 

3. <span data-ttu-id="760e5-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="760e5-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="760e5-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="760e5-230">Click **Add** button.</span></span> <span data-ttu-id="760e5-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="760e5-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="760e5-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="760e5-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="760e5-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="760e5-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="760e5-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="760e5-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="760e5-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="760e5-236">Testing single sign-on</span></span>

<span data-ttu-id="760e5-237">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="760e5-237">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="760e5-238">Du bör få automatiskt inloggade tooyour PerformanceCentre programmet när du klickar på hello PerformanceCentre panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="760e5-238">When you click hello PerformanceCentre tile in hello Access Panel, you should get automatically signed-on tooyour PerformanceCentre application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="760e5-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="760e5-239">Additional resources</span></span>

* [<span data-ttu-id="760e5-240">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="760e5-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="760e5-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="760e5-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png

[100]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png

