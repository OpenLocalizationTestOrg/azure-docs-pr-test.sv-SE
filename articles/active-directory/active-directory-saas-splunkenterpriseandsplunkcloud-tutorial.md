---
title: "Självstudier: Azure Active Directory-integrering med Splunk Enterprise och Splunk molntjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Splunk Enterprise och Splunk moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 848e0485131321479f2375501b330c798627e7f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a><span data-ttu-id="61877-103">Självstudier: Azure Active Directory-integrering med Splunk Enterprise och Splunk moln</span><span class="sxs-lookup"><span data-stu-id="61877-103">Tutorial: Azure Active Directory integration with Splunk Enterprise and Splunk Cloud</span></span>

<span data-ttu-id="61877-104">I kursen får du lära dig hur toointegrate Splunk Enterprise och Splunk moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="61877-104">In this tutorial, you learn how toointegrate Splunk Enterprise and Splunk Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="61877-105">Integrera Splunk Enterprise och Splunk moln med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="61877-105">Integrating Splunk Enterprise and Splunk Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="61877-106">Du kan styra i Azure AD som har åtkomst tooSplunk Enterprise och Splunk moln</span><span class="sxs-lookup"><span data-stu-id="61877-106">You can control in Azure AD who has access tooSplunk Enterprise and Splunk Cloud</span></span>
- <span data-ttu-id="61877-107">Du kan aktivera din användare tooautomatically get inloggade tooSplunk Enterprise och Splunk molnet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="61877-107">You can enable your users tooautomatically get signed-on tooSplunk Enterprise and Splunk Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="61877-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="61877-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="61877-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="61877-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61877-110">Krav</span><span class="sxs-lookup"><span data-stu-id="61877-110">Prerequisites</span></span>

<span data-ttu-id="61877-111">tooconfigure Azure AD-integrering med Splunk Enterprise och Splunk molnet, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="61877-111">tooconfigure Azure AD integration with Splunk Enterprise and Splunk Cloud, you need hello following items:</span></span>

- <span data-ttu-id="61877-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="61877-112">An Azure AD subscription</span></span>
- <span data-ttu-id="61877-113">En Splunk Enterprise och Splunk moln enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="61877-113">A Splunk Enterprise and Splunk Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="61877-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="61877-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="61877-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="61877-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="61877-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="61877-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="61877-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="61877-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="61877-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="61877-118">Scenario description</span></span>
<span data-ttu-id="61877-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="61877-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="61877-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="61877-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="61877-121">Lägga till Splunk Enterprise och Splunk moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="61877-121">Adding Splunk Enterprise and Splunk Cloud from hello gallery</span></span>
2. <span data-ttu-id="61877-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="61877-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-splunk-enterprise-and-splunk-cloud-from-hello-gallery"></a><span data-ttu-id="61877-123">Lägga till Splunk Enterprise och Splunk moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="61877-123">Adding Splunk Enterprise and Splunk Cloud from hello gallery</span></span>
<span data-ttu-id="61877-124">tooconfigure hello integrering av Splunk Enterprise och Splunk moln i Azure AD, behöver du tooadd Splunk Enterprise och Splunk moln hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="61877-124">tooconfigure hello integration of Splunk Enterprise and Splunk Cloud into Azure AD, you need tooadd Splunk Enterprise and Splunk Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="61877-125">**tooadd Splunk Enterprise och Splunk moln från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="61877-125">**tooadd Splunk Enterprise and Splunk Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="61877-126">I hello ** [Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="61877-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="61877-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="61877-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="61877-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="61877-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="61877-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="61877-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="61877-133">Skriv i sökrutan hello **Splunk Enterprise och Splunk moln**.</span><span class="sxs-lookup"><span data-stu-id="61877-133">In hello search box, type **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_search.png)

5. <span data-ttu-id="61877-135">Markera hello resultat på panelen **Splunk Enterprise och Splunk moln**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="61877-135">In hello results panel, select **Splunk Enterprise and Splunk Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="61877-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="61877-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="61877-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Splunk Enterprise och Splunk molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="61877-138">In this section, you configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="61877-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Splunk Enterprise och Splunk molnet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="61877-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Splunk Enterprise and Splunk Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="61877-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Splunk Enterprise och Splunk molnet toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="61877-140">In other words, a link relationship between an Azure AD user and hello related user in Splunk Enterprise and Splunk Cloud needs toobe established.</span></span>

<span data-ttu-id="61877-141">I Splunk Enterprise och Splunk molnet, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="61877-141">In Splunk Enterprise and Splunk Cloud, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="61877-142">tooconfigure och testa Azure AD enkel inloggning med Splunk Enterprise och Splunk molnet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="61877-142">tooconfigure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="61877-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="61877-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="61877-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="61877-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="61877-145">**[Skapa en testanvändare Splunk Enterprise och Splunk moln](#creating-a-splunk-enterprise-and-splunk-cloud-test-user) ** -toohave en motsvarighet för Britta Simon i Splunk företag och Splunk moln som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="61877-145">**[Creating a Splunk Enterprise and Splunk Cloud test user](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)** - toohave a counterpart of Britta Simon in Splunk Enterprise and Splunk Cloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="61877-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="61877-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="61877-147">**[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="61877-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="61877-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="61877-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="61877-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Splunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="61877-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Splunk Enterprise and Splunk Cloud application.</span></span>

<span data-ttu-id="61877-150">**tooconfigure Azure AD enkel inloggning med Splunk Enterprise och Splunk moln, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="61877-150">**tooconfigure Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="61877-151">I hello Azure-portalen på hello **Splunk Enterprise och Splunk moln** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="61877-151">In hello Azure portal, on hello **Splunk Enterprise and Splunk Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="61877-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="61877-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_samlbase.png)

3. <span data-ttu-id="61877-155">På hello **Splunk Enterprise och Splunk moln domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="61877-155">On hello **Splunk Enterprise and Splunk Cloud Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_url.png)

    <span data-ttu-id="61877-157">a.</span><span class="sxs-lookup"><span data-stu-id="61877-157">a.</span></span> <span data-ttu-id="61877-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<splunkserverUrl>/en-US/app/launcher/home`</span><span class="sxs-lookup"><span data-stu-id="61877-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<splunkserverUrl>/en-US/app/launcher/home`</span></span>
    
    <span data-ttu-id="61877-159">b.</span><span class="sxs-lookup"><span data-stu-id="61877-159">b.</span></span> <span data-ttu-id="61877-160">I hello **identifierare** textruta typen hello URL-Adressen till din Splunk.</span><span class="sxs-lookup"><span data-stu-id="61877-160">In hello **Identifier** textbox, type hello URL of your Splunk Server.</span></span>

    <span data-ttu-id="61877-161">c.</span><span class="sxs-lookup"><span data-stu-id="61877-161">c.</span></span> <span data-ttu-id="61877-162">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<splunkserver>/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="61877-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<splunkserver>/saml/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="61877-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="61877-163">These values are not real.</span></span> <span data-ttu-id="61877-164">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="61877-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="61877-165">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="61877-165">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="61877-166">Kontakta [Splunk Enterprise och Splunk Cloud klienten supportteam](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="61877-166">Contact [Splunk Enterprise and Splunk Cloud Client support team](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) tooget these values.</span></span> 

4. <span data-ttu-id="61877-167">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="61877-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_certificate.png) 

5. <span data-ttu-id="61877-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="61877-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="61877-171">tooconfigure enkel inloggning på **Splunk Enterprise och Splunk moln** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Splunk Enterprise och Splunk moln supportteam](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).</span><span class="sxs-lookup"><span data-stu-id="61877-171">tooconfigure single sign-on on **Splunk Enterprise and Splunk Cloud** side, you need toosend hello downloaded **Metadata XML** too[Splunk Enterprise and Splunk Cloud support team](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).</span></span>

> [!TIP]
> <span data-ttu-id="61877-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="61877-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="61877-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello ** Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="61877-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="61877-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="61877-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="61877-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="61877-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="61877-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="61877-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="61877-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="61877-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="61877-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="61877-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="61877-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="61877-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="61877-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="61877-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="61877-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="61877-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="61877-187">a.</span><span class="sxs-lookup"><span data-stu-id="61877-187">a.</span></span> <span data-ttu-id="61877-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="61877-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="61877-189">b.</span><span class="sxs-lookup"><span data-stu-id="61877-189">b.</span></span> <span data-ttu-id="61877-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="61877-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="61877-191">c.</span><span class="sxs-lookup"><span data-stu-id="61877-191">c.</span></span> <span data-ttu-id="61877-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="61877-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="61877-193">d.</span><span class="sxs-lookup"><span data-stu-id="61877-193">d.</span></span> <span data-ttu-id="61877-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="61877-194">Click **Create**.</span></span>
 
### <a name="creating-a-splunk-enterprise-and-splunk-cloud-test-user"></a><span data-ttu-id="61877-195">Skapa en testanvändare Splunk Enterprise och Splunk moln</span><span class="sxs-lookup"><span data-stu-id="61877-195">Creating a Splunk Enterprise and Splunk Cloud test user</span></span>

<span data-ttu-id="61877-196">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Splunk företag och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="61877-196">In this section, you create a user called Britta Simon in Splunk Enterprise and Splunk Cloud.</span></span> <span data-ttu-id="61877-197">Arbeta med [Splunk Enterprise och Splunk moln supportteam](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) tooadd hello användare i hello Splunk Enterprise och Splunk molnplattform.</span><span class="sxs-lookup"><span data-stu-id="61877-197">Work with  [Splunk Enterprise and Splunk Cloud support team](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) tooadd hello users in hello Splunk Enterprise and Splunk Cloud platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="61877-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="61877-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="61877-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSplunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="61877-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSplunk Enterprise and Splunk Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="61877-201">**tooassign Britta Simon tooSplunk Enterprise och Splunk moln, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="61877-201">**tooassign Britta Simon tooSplunk Enterprise and Splunk Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="61877-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="61877-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="61877-204">Välj i listan med program hello **Splunk Enterprise och Splunk moln**.</span><span class="sxs-lookup"><span data-stu-id="61877-204">In hello applications list, select **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_app.png) 

3. <span data-ttu-id="61877-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="61877-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="61877-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="61877-208">Click **Add** button.</span></span> <span data-ttu-id="61877-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="61877-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="61877-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="61877-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="61877-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="61877-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="61877-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="61877-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="61877-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="61877-214">Testing single sign-on</span></span>

<span data-ttu-id="61877-215">I det här avsnittet kan du testa din Azure AD-SSOonfiguration med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="61877-215">In this section, you test your Azure AD SSOonfiguration using hello Access Panel.</span></span>

<span data-ttu-id="61877-216">Du bör få automatiskt inloggade tooyour Splunk Enterprise och Splunk moln när du klickar på hello Splunk Enterprise och Splunk moln-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="61877-216">When you click hello Splunk Enterprise and Splunk Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Splunk Enterprise and Splunk Cloud application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61877-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="61877-217">Additional resources</span></span>

* [<span data-ttu-id="61877-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="61877-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="61877-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="61877-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_203.png

