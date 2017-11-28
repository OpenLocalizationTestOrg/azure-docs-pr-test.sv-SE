---
title: "Självstudier: Azure Active Directory-integrering med Innotas | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Innotas."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eb9e9c2c-4b09-4177-bbab-423fd657448e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 31d787a351fe9362e35afee28a292c927f43702d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-innotas"></a><span data-ttu-id="25dd0-103">Självstudier: Azure Active Directory-integrering med Innotas</span><span class="sxs-lookup"><span data-stu-id="25dd0-103">Tutorial: Azure Active Directory integration with Innotas</span></span>

<span data-ttu-id="25dd0-104">I kursen får du lära dig hur toointegrate Innotas med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="25dd0-104">In this tutorial, you learn how toointegrate Innotas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="25dd0-105">Integrera Innotas med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="25dd0-105">Integrating Innotas with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="25dd0-106">Du kan styra i Azure AD som har åtkomst till tooInnotas</span><span class="sxs-lookup"><span data-stu-id="25dd0-106">You can control in Azure AD who has access tooInnotas</span></span>
- <span data-ttu-id="25dd0-107">Du kan aktivera din användare tooautomatically get inloggade tooInnotas (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="25dd0-107">You can enable your users tooautomatically get signed-on tooInnotas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="25dd0-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="25dd0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="25dd0-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="25dd0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25dd0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="25dd0-110">Prerequisites</span></span>

<span data-ttu-id="25dd0-111">tooconfigure Azure AD-integrering med Innotas, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="25dd0-111">tooconfigure Azure AD integration with Innotas, you need hello following items:</span></span>

- <span data-ttu-id="25dd0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="25dd0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="25dd0-113">En Innotas enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="25dd0-113">An Innotas single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="25dd0-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="25dd0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="25dd0-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="25dd0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="25dd0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="25dd0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="25dd0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="25dd0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="25dd0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="25dd0-118">Scenario description</span></span>

<span data-ttu-id="25dd0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="25dd0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="25dd0-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="25dd0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="25dd0-121">Att lägga till Innotas från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="25dd0-121">Adding Innotas from hello gallery</span></span>
2. <span data-ttu-id="25dd0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="25dd0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-innotas-from-hello-gallery"></a><span data-ttu-id="25dd0-123">Att lägga till Innotas från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="25dd0-123">Adding Innotas from hello gallery</span></span>
<span data-ttu-id="25dd0-124">tooconfigure hello integrering av Innotas i Azure AD, behöver du tooadd Innotas hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="25dd0-124">tooconfigure hello integration of Innotas into Azure AD, you need tooadd Innotas from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="25dd0-125">**tooadd Innotas från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="25dd0-125">**tooadd Innotas from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="25dd0-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="25dd0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="25dd0-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="25dd0-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="25dd0-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="25dd0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="25dd0-133">Skriv i sökrutan hello **Innotas**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-133">In hello search box, type **Innotas**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_search.png)

5. <span data-ttu-id="25dd0-135">Markera hello resultat på panelen **Innotas**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="25dd0-135">In hello results panel, select **Innotas**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="25dd0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="25dd0-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="25dd0-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Innotas baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="25dd0-138">In this section, you configure and test Azure AD single sign-on with Innotas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="25dd0-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Innotas är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="25dd0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Innotas is tooa user in Azure AD.</span></span> <span data-ttu-id="25dd0-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Innotas toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="25dd0-140">In other words, a link relationship between an Azure AD user and hello related user in Innotas needs toobe established.</span></span>

<span data-ttu-id="25dd0-141">I Innotas, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="25dd0-141">In Innotas, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="25dd0-142">tooconfigure och testa Azure AD enkel inloggning med Innotas, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="25dd0-142">tooconfigure and test Azure AD single sign-on with Innotas, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="25dd0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="25dd0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="25dd0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="25dd0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="25dd0-145">**[Skapa en testanvändare Innotas](#creating-an-innotas-test-user)**  -toohave en motsvarighet för Britta Simon i Innotas som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="25dd0-145">**[Creating an Innotas test user](#creating-an-innotas-test-user)** - toohave a counterpart of Britta Simon in Innotas that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="25dd0-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="25dd0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="25dd0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="25dd0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="25dd0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="25dd0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="25dd0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Innotas program.</span><span class="sxs-lookup"><span data-stu-id="25dd0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Innotas application.</span></span>

<span data-ttu-id="25dd0-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Innotas:**</span><span class="sxs-lookup"><span data-stu-id="25dd0-150">**tooconfigure Azure AD single sign-on with Innotas, perform hello following steps:**</span></span>

1. <span data-ttu-id="25dd0-151">I hello Azure-portalen på hello **Innotas** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-151">In hello Azure portal, on hello **Innotas** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="25dd0-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="25dd0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_samlbase.png)

3. <span data-ttu-id="25dd0-155">På hello **Innotas domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="25dd0-155">On hello **Innotas Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_url.png)

    <span data-ttu-id="25dd0-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.Innotas.com`</span><span class="sxs-lookup"><span data-stu-id="25dd0-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.Innotas.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="25dd0-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="25dd0-158">This value is not real.</span></span> <span data-ttu-id="25dd0-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="25dd0-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="25dd0-160">Kontakta [Innotas klienten supportteamet](https://www.innotas.com/contact) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="25dd0-160">Contact [Innotas Client support team](https://www.innotas.com/contact) tooget this value.</span></span> 
 
4. <span data-ttu-id="25dd0-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="25dd0-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_certificate.png) 

5. <span data-ttu-id="25dd0-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="25dd0-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="25dd0-165">tooconfigure enkel inloggning på **Innotas** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Innotas supportteam](https://www.innotas.com/contact).</span><span class="sxs-lookup"><span data-stu-id="25dd0-165">tooconfigure single sign-on on **Innotas** side, you need toosend hello downloaded **Metadata XML** too[Innotas support team](https://www.innotas.com/contact).</span></span> <span data-ttu-id="25dd0-166">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="25dd0-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="25dd0-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="25dd0-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="25dd0-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="25dd0-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="25dd0-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="25dd0-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="25dd0-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="25dd0-170">Creating an Azure AD test user</span></span>

<span data-ttu-id="25dd0-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="25dd0-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="25dd0-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="25dd0-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="25dd0-174">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="25dd0-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="25dd0-176">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="25dd0-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="25dd0-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="25dd0-180">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="25dd0-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="25dd0-182">a.</span><span class="sxs-lookup"><span data-stu-id="25dd0-182">a.</span></span> <span data-ttu-id="25dd0-183">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="25dd0-184">b.</span><span class="sxs-lookup"><span data-stu-id="25dd0-184">b.</span></span> <span data-ttu-id="25dd0-185">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="25dd0-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="25dd0-186">c.</span><span class="sxs-lookup"><span data-stu-id="25dd0-186">c.</span></span> <span data-ttu-id="25dd0-187">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="25dd0-188">d.</span><span class="sxs-lookup"><span data-stu-id="25dd0-188">d.</span></span> <span data-ttu-id="25dd0-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-189">Click **Create**.</span></span>
 
### <a name="creating-an-innotas-test-user"></a><span data-ttu-id="25dd0-190">Skapa en testanvändare Innotas</span><span class="sxs-lookup"><span data-stu-id="25dd0-190">Creating an Innotas test user</span></span>

<span data-ttu-id="25dd0-191">Det finns inga uppgiften för du tooconfigure användaretablering tooInnotas.</span><span class="sxs-lookup"><span data-stu-id="25dd0-191">There is no action item for you tooconfigure user provisioning tooInnotas.</span></span>  
<span data-ttu-id="25dd0-192">När en tilldelad användare försöker toolog i tooInnotas med hello åtkomstpanelen, kontrollerar Innotas om hello användaren finns.</span><span class="sxs-lookup"><span data-stu-id="25dd0-192">When an assigned user tries toolog in tooInnotas using hello access panel, Innotas checks whether hello user exists.</span></span>  

>[!NOTE]
><span data-ttu-id="25dd0-193">Om det finns inget användarkonto ännu, skapas den automatiskt av Innotas.</span><span class="sxs-lookup"><span data-stu-id="25dd0-193">If there is no user account available yet, it is automatically created by Innotas.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="25dd0-194">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="25dd0-194">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="25dd0-195">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooInnotas.</span><span class="sxs-lookup"><span data-stu-id="25dd0-195">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInnotas.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="25dd0-197">**tooassign Britta Simon tooInnotas utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="25dd0-197">**tooassign Britta Simon tooInnotas, perform hello following steps:**</span></span>

1. <span data-ttu-id="25dd0-198">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-198">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="25dd0-200">Välj i listan med program hello **Innotas**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-200">In hello applications list, select **Innotas**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_app.png) 

3. <span data-ttu-id="25dd0-202">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="25dd0-202">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="25dd0-204">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="25dd0-204">Click **Add** button.</span></span> <span data-ttu-id="25dd0-205">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="25dd0-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="25dd0-207">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="25dd0-207">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="25dd0-208">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="25dd0-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="25dd0-209">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="25dd0-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="25dd0-210">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="25dd0-210">Testing single sign-on</span></span>

<span data-ttu-id="25dd0-211">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="25dd0-211">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="25dd0-212">Du bör få automatiskt inloggade tooyour Innotas programmet när du klickar på hello Innotas panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="25dd0-212">When you click hello Innotas tile in hello Access Panel, you should get automatically signed-on tooyour Innotas application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25dd0-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="25dd0-213">Additional resources</span></span>

* [<span data-ttu-id="25dd0-214">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="25dd0-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="25dd0-215">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="25dd0-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_203.png

