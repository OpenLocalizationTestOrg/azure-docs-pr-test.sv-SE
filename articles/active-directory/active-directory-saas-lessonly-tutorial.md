---
title: "Självstudier: Azure Active Directory-integrering med Lesson.ly | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Lesson.ly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 23b339dcc26471b42aaa7e1baadcb42500d6b7e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a><span data-ttu-id="9110a-103">Självstudier: Azure Active Directory-integrering med Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="9110a-103">Tutorial: Azure Active Directory integration with Lesson.ly</span></span>

<span data-ttu-id="9110a-104">I kursen får du lära dig hur toointegrate Lesson.ly med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9110a-104">In this tutorial, you learn how toointegrate Lesson.ly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9110a-105">Integrera Lesson.ly med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9110a-105">Integrating Lesson.ly with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9110a-106">Du kan styra i Azure AD som har åtkomst till tooLesson.ly</span><span class="sxs-lookup"><span data-stu-id="9110a-106">You can control in Azure AD who has access tooLesson.ly</span></span>
- <span data-ttu-id="9110a-107">Du kan aktivera din användare tooautomatically get inloggade tooLesson.ly (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9110a-107">You can enable your users tooautomatically get signed-on tooLesson.ly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9110a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9110a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9110a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9110a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9110a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9110a-110">Prerequisites</span></span>

<span data-ttu-id="9110a-111">tooconfigure Azure AD-integrering med Lesson.ly, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9110a-111">tooconfigure Azure AD integration with Lesson.ly, you need hello following items:</span></span>

- <span data-ttu-id="9110a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9110a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9110a-113">En Lesson.ly enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="9110a-113">A Lesson.ly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9110a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9110a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9110a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9110a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9110a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9110a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9110a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9110a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9110a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9110a-118">Scenario description</span></span>
<span data-ttu-id="9110a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9110a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9110a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9110a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9110a-121">Att lägga till Lesson.ly från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9110a-121">Adding Lesson.ly from hello gallery</span></span>
2. <span data-ttu-id="9110a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9110a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lessonly-from-hello-gallery"></a><span data-ttu-id="9110a-123">Att lägga till Lesson.ly från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9110a-123">Adding Lesson.ly from hello gallery</span></span>
<span data-ttu-id="9110a-124">tooconfigure hello integrering av Lesson.ly i Azure AD, behöver du tooadd Lesson.ly hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9110a-124">tooconfigure hello integration of Lesson.ly into Azure AD, you need tooadd Lesson.ly from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9110a-125">**tooadd Lesson.ly från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9110a-125">**tooadd Lesson.ly from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9110a-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9110a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9110a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9110a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9110a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9110a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9110a-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9110a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9110a-133">Skriv i sökrutan hello **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="9110a-133">In hello search box, type **Lesson.ly**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_search.png)

5. <span data-ttu-id="9110a-135">Markera hello resultat på panelen **Lesson.ly**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9110a-135">In hello results panel, select **Lesson.ly**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9110a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9110a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9110a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Lesson.ly baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9110a-138">In this section, you configure and test Azure AD single sign-on with Lesson.ly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9110a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Lesson.ly är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9110a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lesson.ly is tooa user in Azure AD.</span></span> <span data-ttu-id="9110a-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Lesson.ly toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9110a-140">In other words, a link relationship between an Azure AD user and hello related user in Lesson.ly needs toobe established.</span></span>

<span data-ttu-id="9110a-141">I Lesson.ly, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9110a-141">In Lesson.ly, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9110a-142">tooconfigure och testa Azure AD enkel inloggning med Lesson.ly, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9110a-142">tooconfigure and test Azure AD single sign-on with Lesson.ly, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9110a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9110a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9110a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9110a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9110a-145">**[Skapa en testanvändare Lesson.ly](#creating-a-lessonly-test-user)**  -toohave en motsvarighet för Britta Simon i Lesson.ly som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9110a-145">**[Creating a Lesson.ly test user](#creating-a-lessonly-test-user)** - toohave a counterpart of Britta Simon in Lesson.ly that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9110a-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9110a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9110a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9110a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9110a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9110a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9110a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Lesson.ly program.</span><span class="sxs-lookup"><span data-stu-id="9110a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lesson.ly application.</span></span>

<span data-ttu-id="9110a-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Lesson.ly:**</span><span class="sxs-lookup"><span data-stu-id="9110a-150">**tooconfigure Azure AD single sign-on with Lesson.ly, perform hello following steps:**</span></span>

1. <span data-ttu-id="9110a-151">I hello Azure-portalen på hello **Lesson.ly** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9110a-151">In hello Azure portal, on hello **Lesson.ly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9110a-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9110a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_samlbase.png)

3. <span data-ttu-id="9110a-155">På hello **Lesson.ly domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9110a-155">On hello **Lesson.ly Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_url.png)

    <span data-ttu-id="9110a-157">a.</span><span class="sxs-lookup"><span data-stu-id="9110a-157">a.</span></span> <span data-ttu-id="9110a-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="9110a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/signin`|
    | `https://<companyname>.lessonly.com/signin`|

    >[!NOTE]
    ><span data-ttu-id="9110a-159">När refererar till en allmän namn som **companyname** måste toobe ersättas med ett faktiskt namn.</span><span class="sxs-lookup"><span data-stu-id="9110a-159">When referencing a generic name that **companyname** needs toobe replaced by an actual name.</span></span>
    
    <span data-ttu-id="9110a-160">b.</span><span class="sxs-lookup"><span data-stu-id="9110a-160">b.</span></span> <span data-ttu-id="9110a-161">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="9110a-161">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/auth/saml/metadata`|
    | `https://<companyname>.lessonly.com/auth/saml/metadata`|

    > [!NOTE] 
    > <span data-ttu-id="9110a-162">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="9110a-162">These values are not real.</span></span> <span data-ttu-id="9110a-163">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="9110a-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9110a-164">Kontakta [Lesson.ly klienten supportteamet](mailto:dev@lessonly.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9110a-164">Contact [Lesson.ly Client support team](mailto:dev@lessonly.com) tooget these values.</span></span> 

4. <span data-ttu-id="9110a-165">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="9110a-165">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_certificate.png)

5. <span data-ttu-id="9110a-167">Hej Lesson.ly program förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **SAML-Token attribut** configuration.hello följande skärmbild visar ett exempel på den här finns.</span><span class="sxs-lookup"><span data-stu-id="9110a-167">hello Lesson.ly application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.hello following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png)
           
6. <span data-ttu-id="9110a-169">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i föregående bild hello och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9110a-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>

    | <span data-ttu-id="9110a-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="9110a-170">Attribute Name</span></span>   | <span data-ttu-id="9110a-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="9110a-171">Attribute Value</span></span> |
    | ---------------  | ----------------|
    | <span data-ttu-id="9110a-172">urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="9110a-172">urn:oid:2.5.4.42</span></span> |<span data-ttu-id="9110a-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="9110a-173">user.givenname</span></span> |
    | <span data-ttu-id="9110a-174">urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="9110a-174">urn:oid:2.5.4.4</span></span>  |<span data-ttu-id="9110a-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="9110a-175">user.surname</span></span> |
    | <span data-ttu-id="9110a-176">urn: oid:0.9.2342.19200300.1001.3</span><span class="sxs-lookup"><span data-stu-id="9110a-176">urn:oid:0.9.2342.19200300.1001.3</span></span> |<span data-ttu-id="9110a-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="9110a-177">user.mail</span></span> |

    <span data-ttu-id="9110a-178">a.</span><span class="sxs-lookup"><span data-stu-id="9110a-178">a.</span></span> <span data-ttu-id="9110a-179">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9110a-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="9110a-182">b.</span><span class="sxs-lookup"><span data-stu-id="9110a-182">b.</span></span> <span data-ttu-id="9110a-183">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="9110a-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="9110a-184">c.</span><span class="sxs-lookup"><span data-stu-id="9110a-184">c.</span></span> <span data-ttu-id="9110a-185">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="9110a-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="9110a-186">d.</span><span class="sxs-lookup"><span data-stu-id="9110a-186">d.</span></span> <span data-ttu-id="9110a-187">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9110a-187">Click **Ok**.</span></span>     

7. <span data-ttu-id="9110a-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9110a-188">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9110a-190">På hello **Lesson.ly Configuration** klickar du på **konfigurera Lesson.ly** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9110a-190">On hello **Lesson.ly Configuration** section, click **Configure Lesson.ly** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9110a-191">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="9110a-191">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_configure.png)

9. <span data-ttu-id="9110a-193">tooconfigure enkel inloggning på **Lesson.ly** sida, behöver du toosend hello hämtas **Certificate(Base64)** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[Lesson.ly supportteamet](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="9110a-193">tooconfigure single sign-on on **Lesson.ly** side, you need toosend hello downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

> [!TIP]
> <span data-ttu-id="9110a-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9110a-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9110a-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9110a-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9110a-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9110a-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9110a-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9110a-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="9110a-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9110a-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9110a-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9110a-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9110a-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9110a-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9110a-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9110a-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9110a-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9110a-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9110a-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9110a-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9110a-209">a.</span><span class="sxs-lookup"><span data-stu-id="9110a-209">a.</span></span> <span data-ttu-id="9110a-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9110a-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9110a-211">b.</span><span class="sxs-lookup"><span data-stu-id="9110a-211">b.</span></span> <span data-ttu-id="9110a-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9110a-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9110a-213">c.</span><span class="sxs-lookup"><span data-stu-id="9110a-213">c.</span></span> <span data-ttu-id="9110a-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9110a-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9110a-215">d.</span><span class="sxs-lookup"><span data-stu-id="9110a-215">d.</span></span> <span data-ttu-id="9110a-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9110a-216">Click **Create**.</span></span>
 
### <a name="creating-a-lessonly-test-user"></a><span data-ttu-id="9110a-217">Skapa en testanvändare Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="9110a-217">Creating a Lesson.ly test user</span></span>

<span data-ttu-id="9110a-218">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="9110a-218">hello objective of this section is toocreate a user called Britta Simon in Lesson.ly.</span></span> <span data-ttu-id="9110a-219">Lesson.LY stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="9110a-219">Lesson.ly supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="9110a-220">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9110a-220">There is no action item for you in this section.</span></span> <span data-ttu-id="9110a-221">En ny användare skapas under ett försök tooaccess Lesson.ly om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="9110a-221">A new user will be created during an attempt tooaccess Lesson.ly if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="9110a-222">Om du behöver toocreate en användare manuellt, måste toocontact hello [Lesson.ly supportteamet](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="9110a-222">If you need toocreate an user manually, you need toocontact hello [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9110a-223">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9110a-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9110a-224">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLesson.ly.</span><span class="sxs-lookup"><span data-stu-id="9110a-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLesson.ly.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9110a-226">**tooassign Britta Simon tooLesson.ly utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9110a-226">**tooassign Britta Simon tooLesson.ly, perform hello following steps:**</span></span>

1. <span data-ttu-id="9110a-227">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9110a-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9110a-229">Välj i listan med program hello **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="9110a-229">In hello applications list, select **Lesson.ly**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_app.png) 

3. <span data-ttu-id="9110a-231">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9110a-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9110a-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9110a-233">Click **Add** button.</span></span> <span data-ttu-id="9110a-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9110a-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9110a-236">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9110a-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9110a-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9110a-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9110a-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9110a-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9110a-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9110a-239">Testing single sign-on</span></span>

<span data-ttu-id="9110a-240">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9110a-240">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9110a-241">Du bör få automatiskt inloggade tooyour Lesson.ly programmet när du klickar på hello Lesson.ly panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9110a-241">When you click hello Lesson.ly tile in hello Access Panel, you should get automatically signed-on tooyour Lesson.ly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9110a-242">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9110a-242">Additional resources</span></span>

* [<span data-ttu-id="9110a-243">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9110a-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9110a-244">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9110a-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png

