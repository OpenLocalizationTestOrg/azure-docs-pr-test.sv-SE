---
title: "Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7d7897f6-b88e-4dd5-8f3a-e612337b1413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 46a5da7504806202a5ec29f73c504e772c61bc2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a><span data-ttu-id="6b729-103">Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="6b729-103">Tutorial: Azure Active Directory integration with Citrix GoToMeeting</span></span>

<span data-ttu-id="6b729-104">I kursen får du lära dig hur toointegrate Citrix GoToMeeting med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6b729-104">In this tutorial, you learn how toointegrate Citrix GoToMeeting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6b729-105">Integrera Citrix GoToMeeting med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6b729-105">Integrating Citrix GoToMeeting with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6b729-106">Du kan styra i Azure AD som har åtkomst tooCitrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="6b729-106">You can control in Azure AD who has access tooCitrix GoToMeeting</span></span>
- <span data-ttu-id="6b729-107">Du kan aktivera din användare tooautomatically get inloggade tooCitrix GoToMeeting (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6b729-107">You can enable your users tooautomatically get signed-on tooCitrix GoToMeeting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6b729-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6b729-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6b729-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6b729-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b729-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6b729-110">Prerequisites</span></span>

<span data-ttu-id="6b729-111">tooconfigure Azure AD-integrering med Citrix GoToMeeting måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6b729-111">tooconfigure Azure AD integration with Citrix GoToMeeting, you need hello following items:</span></span>

- <span data-ttu-id="6b729-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6b729-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6b729-113">En Citrix GoToMeeting enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="6b729-113">A Citrix GoToMeeting single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6b729-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6b729-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6b729-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6b729-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6b729-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6b729-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6b729-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6b729-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6b729-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6b729-118">Scenario description</span></span>
<span data-ttu-id="6b729-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6b729-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6b729-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6b729-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6b729-121">Att lägga till Citrix GoToMeeting från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6b729-121">Adding Citrix GoToMeeting from hello gallery</span></span>
2. <span data-ttu-id="6b729-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b729-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-citrix-gotomeeting-from-hello-gallery"></a><span data-ttu-id="6b729-123">Att lägga till Citrix GoToMeeting från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6b729-123">Adding Citrix GoToMeeting from hello gallery</span></span>
<span data-ttu-id="6b729-124">tooconfigure hello integrering av Citrix GoToMeeting i Azure AD, behöver du tooadd Citrix GoToMeeting hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6b729-124">tooconfigure hello integration of Citrix GoToMeeting into Azure AD, you need tooadd Citrix GoToMeeting from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6b729-125">**tooadd Citrix GoToMeeting från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6b729-125">**tooadd Citrix GoToMeeting from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b729-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6b729-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6b729-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6b729-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6b729-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6b729-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6b729-131">Klicka på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b729-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6b729-133">Skriv i sökrutan hello **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="6b729-133">In hello search box, type **Citrix GoToMeeting**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_search.png)

5. <span data-ttu-id="6b729-135">Markera hello resultat på panelen **Citrix GoToMeeting**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6b729-135">In hello results panel, select **Citrix GoToMeeting**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6b729-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b729-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6b729-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Citrix GoToMeeting baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6b729-138">In this section, you configure and test Azure AD single sign-on with Citrix GoToMeeting based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6b729-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Citrix GoToMeeting är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6b729-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Citrix GoToMeeting is tooa user in Azure AD.</span></span> <span data-ttu-id="6b729-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Citrix GoToMeeting toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6b729-140">In other words, a link relationship between an Azure AD user and hello related user in Citrix GoToMeeting needs toobe established.</span></span>

<span data-ttu-id="6b729-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="6b729-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Citrix GoToMeeting.</span></span>

<span data-ttu-id="6b729-142">tooconfigure och testa Azure AD enkel inloggning med Citrix GoToMeeting, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6b729-142">tooconfigure and test Azure AD single sign-on with Citrix GoToMeeting, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6b729-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6b729-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6b729-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b729-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6b729-145">**[Skapa en testanvändare Citrix GoToMeeting](#creating-a-citrix-gotomeeting-test-user)**  -toohave en motsvarighet för Britta Simon i Citrix GoToMeeting som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6b729-145">**[Creating a Citrix GoToMeeting test user](#creating-a-citrix-gotomeeting-test-user)** - toohave a counterpart of Britta Simon in Citrix GoToMeeting that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6b729-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b729-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6b729-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6b729-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6b729-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b729-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6b729-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Citrix GoToMeeting-program.</span><span class="sxs-lookup"><span data-stu-id="6b729-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Citrix GoToMeeting application.</span></span>

<span data-ttu-id="6b729-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Citrix GoToMeeting:**</span><span class="sxs-lookup"><span data-stu-id="6b729-150">**tooconfigure Azure AD single sign-on with Citrix GoToMeeting, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b729-151">I hello Azure-portalen på hello **Citrix GoToMeeting** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6b729-151">In hello Azure portal, on hello **Citrix GoToMeeting** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6b729-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b729-153">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_samlbase.png)

3. <span data-ttu-id="6b729-155">På hello **Citrix GoToMeeting domän och URL: er** avsnitt, utan behov av tooperform några steg.</span><span class="sxs-lookup"><span data-stu-id="6b729-155">On hello **Citrix GoToMeeting Domain and URLs** section, no need tooperform any steps.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_url.png)


3. <span data-ttu-id="6b729-157">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="6b729-157">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_certificate.png) 

4. <span data-ttu-id="6b729-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6b729-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_400.png)
    
5. <span data-ttu-id="6b729-161">Öppna hello Citrix GoToMeeting SAML-konfigurationsavsnittet, klicka på Konfigurera Citrix GoToMeeting SAML tooopen konfigurera inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b729-161">On hello Citrix GoToMeeting SAML Configuration section, click Configure Citrix GoToMeeting SAML tooopen Configure sign-on window.</span></span> <span data-ttu-id="6b729-162">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6b729-162">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

6. <span data-ttu-id="6b729-163">I en annan webbläsare och logga in tooyour [Citrix organisation Center](https://account.citrixonline.com/organization/administration/).</span><span class="sxs-lookup"><span data-stu-id="6b729-163">In a different browser window, log in tooyour [Citrix Organization Center](https://account.citrixonline.com/organization/administration/).</span></span>

7. <span data-ttu-id="6b729-164">Klicka på hello **identitetsleverantör** fliken och utför sedan hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6b729-164">Click hello **Identity Provider** tab, and then perform hello following steps:</span></span>  
   
    <span data-ttu-id="6b729-165">![SAML-installationsprogrammet](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-installationen")</span><span class="sxs-lookup"><span data-stu-id="6b729-165">![SAML setup](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML setup")</span></span>
   
    <span data-ttu-id="6b729-166">a.</span><span class="sxs-lookup"><span data-stu-id="6b729-166">a.</span></span> <span data-ttu-id="6b729-167">Välj **manuell**</span><span class="sxs-lookup"><span data-stu-id="6b729-167">Select **Manual**</span></span>

    <span data-ttu-id="6b729-168">b.</span><span class="sxs-lookup"><span data-stu-id="6b729-168">b.</span></span> <span data-ttu-id="6b729-169">I hello Azure-portalen på hello **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sida, kopiera hello **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in den i hello **inloggningssidan URL: en** textruta.</span><span class="sxs-lookup"><span data-stu-id="6b729-169">In hello Azure portal, on hello **Configure single sign-on at Citrix GoToMeeting** dialog page, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **Sign-in page URL** textbox.</span></span> 

    <span data-ttu-id="6b729-170">c.</span><span class="sxs-lookup"><span data-stu-id="6b729-170">c.</span></span> <span data-ttu-id="6b729-171">I hello Azure-portalen på hello **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sida, kopiera hello **Sign-Out URL** värdet och klistrar in den i hello **utloggning Sidadress**textruta.</span><span class="sxs-lookup"><span data-stu-id="6b729-171">In hello Azure portal, on hello **Configure single sign-on at Citrix GoToMeeting** dialog page, copy hello **Sign-Out URL** value, and then paste it into hello **Sign-out page URL** textbox.</span></span>

    <span data-ttu-id="6b729-172">d.</span><span class="sxs-lookup"><span data-stu-id="6b729-172">d.</span></span> <span data-ttu-id="6b729-173">I hello Azure-portalen på hello **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sida, kopiera hello **SAML enhets-ID** värdet och klistrar in den i hello **identitet providern enhets-ID**  textruta.</span><span class="sxs-lookup"><span data-stu-id="6b729-173">In hello Azure portal, on hello **Configure single sign-on at Citrix GoToMeeting** dialog page, copy hello **SAML Entity ID** value, and then paste it into hello **Identity Provider Entity ID** textbox.</span></span>

    <span data-ttu-id="6b729-174">e.</span><span class="sxs-lookup"><span data-stu-id="6b729-174">e.</span></span> <span data-ttu-id="6b729-175">tooupload hämtade certifikatet, klicka på **överför certifikat**.</span><span class="sxs-lookup"><span data-stu-id="6b729-175">tooupload your downloaded certificate, click **Upload Certificate**.</span></span>

    <span data-ttu-id="6b729-176">f.</span><span class="sxs-lookup"><span data-stu-id="6b729-176">f.</span></span> <span data-ttu-id="6b729-177">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6b729-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="6b729-178">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6b729-178">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6b729-179">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6b729-179">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6b729-180">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6b729-180">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6b729-181">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6b729-181">Creating an Azure AD test user</span></span>
<span data-ttu-id="6b729-182">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b729-182">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6b729-184">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6b729-184">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b729-185">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6b729-185">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6b729-187">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6b729-187">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6b729-189">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b729-189">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6b729-191">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6b729-191">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6b729-193">a.</span><span class="sxs-lookup"><span data-stu-id="6b729-193">a.</span></span> <span data-ttu-id="6b729-194">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6b729-194">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6b729-195">b.</span><span class="sxs-lookup"><span data-stu-id="6b729-195">b.</span></span> <span data-ttu-id="6b729-196">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6b729-196">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6b729-197">c.</span><span class="sxs-lookup"><span data-stu-id="6b729-197">c.</span></span> <span data-ttu-id="6b729-198">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6b729-198">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6b729-199">d.</span><span class="sxs-lookup"><span data-stu-id="6b729-199">d.</span></span> <span data-ttu-id="6b729-200">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6b729-200">Click **Create**.</span></span>
 
### <a name="creating-a-citrix-gotomeeting-test-user"></a><span data-ttu-id="6b729-201">Skapa en testanvändare Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="6b729-201">Creating a Citrix GoToMeeting test user</span></span>

<span data-ttu-id="6b729-202">I det här avsnittet skapas en användare som kallas Britta Simon i Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="6b729-202">In this section, a user called Britta Simon is created in Citrix GoToMeeting.</span></span> <span data-ttu-id="6b729-203">Citrix GoToMeeting stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="6b729-203">Citrix GoToMeeting supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="6b729-204">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6b729-204">There is no action item for you in this section.</span></span> <span data-ttu-id="6b729-205">Om en användare inte redan finns i Citrix GoToMeeting, skapas en ny när du försöker tooaccess Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="6b729-205">If a user doesn't already exist in Citrix GoToMeeting, a new one is created when you attempt tooaccess Citrix GoToMeeting.</span></span>

>[!Note]
><span data-ttu-id="6b729-206">Om du behöver en användare manuellt, kontakta toocreate [Citrix GoToMeeting supportteamet](https://care.citrixonline.com/gotomeeting)</span><span class="sxs-lookup"><span data-stu-id="6b729-206">If you need toocreate a user manually, Contact [Citrix GoToMeeting support team](https://care.citrixonline.com/gotomeeting)</span></span> 


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6b729-207">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6b729-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6b729-208">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCitrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="6b729-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCitrix GoToMeeting.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6b729-210">**tooassign Britta Simon tooCitrix GoToMeeting, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="6b729-210">**tooassign Britta Simon tooCitrix GoToMeeting, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b729-211">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6b729-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6b729-213">Välj i listan med program hello **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="6b729-213">In hello applications list, select **Citrix GoToMeeting**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_app.png) 

3. <span data-ttu-id="6b729-215">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6b729-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6b729-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6b729-217">Click **Add** button.</span></span> <span data-ttu-id="6b729-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b729-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6b729-220">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6b729-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6b729-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b729-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6b729-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b729-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6b729-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b729-223">Testing single sign-on</span></span>

<span data-ttu-id="6b729-224">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6b729-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6b729-225">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6b729-225">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="6b729-226">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6b729-226">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6b729-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6b729-227">Additional resources</span></span>

* [<span data-ttu-id="6b729-228">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6b729-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6b729-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6b729-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="6b729-230">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="6b729-230">Configure User Provisioning</span></span>](active-directory-saas-citrixgotomeeting-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_203.png

