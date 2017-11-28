---
title: "Självstudier: Azure Active Directory-integrering med Datahug | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Datahug."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: 79ccb939c7a19720bcf696af141f747db00c8a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a><span data-ttu-id="97056-103">Självstudier: Azure Active Directory-integrering med Datahug</span><span class="sxs-lookup"><span data-stu-id="97056-103">Tutorial: Azure Active Directory integration with Datahug</span></span>

<span data-ttu-id="97056-104">I kursen får du lära dig hur toointegrate Datahug med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="97056-104">In this tutorial, you learn how toointegrate Datahug with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="97056-105">Integrera Datahug med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="97056-105">Integrating Datahug with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="97056-106">Du kan styra i Azure AD som har åtkomst till tooDatahug</span><span class="sxs-lookup"><span data-stu-id="97056-106">You can control in Azure AD who has access tooDatahug</span></span>
- <span data-ttu-id="97056-107">Du kan aktivera din användare tooautomatically get inloggade tooDatahug (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="97056-107">You can enable your users tooautomatically get signed-on tooDatahug (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="97056-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="97056-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="97056-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97056-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="97056-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="97056-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97056-111">Krav</span><span class="sxs-lookup"><span data-stu-id="97056-111">Prerequisites</span></span>

<span data-ttu-id="97056-112">tooconfigure Azure AD-integrering med Datahug, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="97056-112">tooconfigure Azure AD integration with Datahug, you need hello following items:</span></span>

- <span data-ttu-id="97056-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="97056-113">An Azure AD subscription</span></span>
- <span data-ttu-id="97056-114">En Datahug enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="97056-114">A Datahug single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="97056-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="97056-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="97056-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="97056-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="97056-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="97056-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="97056-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="97056-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="97056-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="97056-119">Scenario description</span></span>
<span data-ttu-id="97056-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="97056-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="97056-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="97056-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="97056-122">Att lägga till Datahug från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="97056-122">Adding Datahug from hello gallery</span></span>
2. <span data-ttu-id="97056-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="97056-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-datahug-from-hello-gallery"></a><span data-ttu-id="97056-124">Att lägga till Datahug från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="97056-124">Adding Datahug from hello gallery</span></span>
<span data-ttu-id="97056-125">tooconfigure hello integrering av Datahug i Azure AD, behöver du tooadd Datahug hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="97056-125">tooconfigure hello integration of Datahug into Azure AD, you need tooadd Datahug from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="97056-126">**tooadd Datahug från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="97056-126">**tooadd Datahug from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="97056-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="97056-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="97056-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="97056-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="97056-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="97056-130">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="97056-132">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97056-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="97056-134">Skriv i sökrutan hello **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="97056-134">In hello search box, type **Datahug**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. <span data-ttu-id="97056-136">Markera hello resultat på panelen **Datahug**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="97056-136">In hello results panel, select **Datahug**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="97056-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="97056-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="97056-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Datahug baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="97056-139">In this section, you configure and test Azure AD single sign-on with Datahug based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="97056-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Datahug är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97056-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Datahug is tooa user in Azure AD.</span></span> <span data-ttu-id="97056-141">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Datahug toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="97056-141">In other words, a link relationship between an Azure AD user and hello related user in Datahug needs toobe established.</span></span>

<span data-ttu-id="97056-142">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Datahug.</span><span class="sxs-lookup"><span data-stu-id="97056-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Datahug.</span></span>

<span data-ttu-id="97056-143">tooconfigure och testa Azure AD enkel inloggning med Datahug, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="97056-143">tooconfigure and test Azure AD single sign-on with Datahug, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="97056-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="97056-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="97056-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97056-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="97056-146">**[Skapa en testanvändare Datahug](#creating-a-datahug-test-user)**  -toohave en motsvarighet för Britta Simon i Datahug som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="97056-146">**[Creating a Datahug test user](#creating-a-datahug-test-user)** - toohave a counterpart of Britta Simon in Datahug that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="97056-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="97056-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="97056-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="97056-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="97056-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="97056-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="97056-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Datahug program.</span><span class="sxs-lookup"><span data-stu-id="97056-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Datahug application.</span></span>

<span data-ttu-id="97056-151">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Datahug:**</span><span class="sxs-lookup"><span data-stu-id="97056-151">**tooconfigure Azure AD single sign-on with Datahug, perform hello following steps:**</span></span>

1. <span data-ttu-id="97056-152">I hello Azure-portalen på hello **Datahug** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="97056-152">In hello Azure portal, on hello **Datahug** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="97056-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="97056-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. <span data-ttu-id="97056-156">På hello **Datahug domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="97056-156">On hello **Datahug Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    <span data-ttu-id="97056-158">a.</span><span class="sxs-lookup"><span data-stu-id="97056-158">a.</span></span> <span data-ttu-id="97056-159">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://apps.datahug.com/identity/<uniqueID>`</span><span class="sxs-lookup"><span data-stu-id="97056-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://apps.datahug.com/identity/<uniqueID>`</span></span>

    <span data-ttu-id="97056-160">b.</span><span class="sxs-lookup"><span data-stu-id="97056-160">b.</span></span> <span data-ttu-id="97056-161">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://apps.datahug.com/identity/<uniqueID>/acs`</span><span class="sxs-lookup"><span data-stu-id="97056-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://apps.datahug.com/identity/<uniqueID>/acs`</span></span>

4. <span data-ttu-id="97056-162">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="97056-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="97056-163">Om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="97056-163">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    <span data-ttu-id="97056-165">I hello **inloggnings-URL** textruta Skriv en URL som:`https://apps.datahug.com/`</span><span class="sxs-lookup"><span data-stu-id="97056-165">In hello **Sign-on URL** textbox, type a URL as: `https://apps.datahug.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="97056-166">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="97056-166">These values are not hello real.</span></span> <span data-ttu-id="97056-167">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="97056-167">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="97056-168">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="97056-168">Here we suggest you toouse hello unique value of string in hello Identifier and Reply URL.</span></span> <span data-ttu-id="97056-169">Kontakta [Datahug klienten supportteamet](http://datahug.com/about/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="97056-169">Contact [Datahug Client support team](http://datahug.com/about/contact-us/) tooget these values.</span></span> 

5. <span data-ttu-id="97056-170">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="97056-170">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  <span data-ttu-id="97056-172">Kontrollera **”visa avancerade inställningar för signering av certifikat”** och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="97056-172">Check **“Show advanced certificate signing settings”** and perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    <span data-ttu-id="97056-174">a.</span><span class="sxs-lookup"><span data-stu-id="97056-174">a.</span></span> <span data-ttu-id="97056-175">I **signering alternativet**väljer **signera SAML assertion**.</span><span class="sxs-lookup"><span data-stu-id="97056-175">In **Signing Option**, select **Sign SAML assertion**.</span></span>
    
    <span data-ttu-id="97056-176">b.</span><span class="sxs-lookup"><span data-stu-id="97056-176">b.</span></span> <span data-ttu-id="97056-177">I **signering algoritmen**väljer **SHA1**.</span><span class="sxs-lookup"><span data-stu-id="97056-177">In **Signing algorithm**, select **SHA1**.</span></span>
 
7. <span data-ttu-id="97056-178">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="97056-178">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="97056-180">På hello **Datahug Configuration** klickar du på **konfigurera Datahug** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="97056-180">On hello **Datahug Configuration** section, click **Configure Datahug** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="97056-181">Kopiera hello **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="97056-181">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. <span data-ttu-id="97056-183">tooconfigure enkel inloggning på **Datahug** sida, behöver du toosend hello hämtas **XML-Metadata för**, **SAML enhets-ID** och **SAML enkel inloggning URL: en** för[Datahug support](http://datahug.com/about/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="97056-183">tooconfigure single sign-on on **Datahug** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** too[Datahug support](http://datahug.com/about/contact-us/).</span></span> <span data-ttu-id="97056-184">De kan ange det här programmet in toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="97056-184">They set this application up toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="97056-185">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="97056-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="97056-186">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="97056-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="97056-187">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="97056-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="97056-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="97056-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="97056-189">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97056-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="97056-191">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="97056-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="97056-192">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="97056-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="97056-194">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="97056-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="97056-196">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97056-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="97056-198">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="97056-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="97056-200">a.</span><span class="sxs-lookup"><span data-stu-id="97056-200">a.</span></span> <span data-ttu-id="97056-201">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="97056-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="97056-202">b.</span><span class="sxs-lookup"><span data-stu-id="97056-202">b.</span></span> <span data-ttu-id="97056-203">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="97056-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="97056-204">c.</span><span class="sxs-lookup"><span data-stu-id="97056-204">c.</span></span> <span data-ttu-id="97056-205">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="97056-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="97056-206">d.</span><span class="sxs-lookup"><span data-stu-id="97056-206">d.</span></span> <span data-ttu-id="97056-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="97056-207">Click **Create**.</span></span>
 
### <a name="creating-a-datahug-test-user"></a><span data-ttu-id="97056-208">Skapa en testanvändare Datahug</span><span class="sxs-lookup"><span data-stu-id="97056-208">Creating a Datahug test user</span></span>

<span data-ttu-id="97056-209">tooenable Azure AD-användare toolog i tooDatahug, måste de etableras i Datahug.</span><span class="sxs-lookup"><span data-stu-id="97056-209">tooenable Azure AD users toolog in tooDatahug, they must be provisioned into Datahug.</span></span>  
<span data-ttu-id="97056-210">När Datahug, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="97056-210">When Datahug, provisioning is a manual task.</span></span>

<span data-ttu-id="97056-211">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="97056-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="97056-212">Logga in tooyour Datahug företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="97056-212">Log in tooyour Datahug company site as an administrator.</span></span>

2. <span data-ttu-id="97056-213">Hovra över hello **kugge** i hello övre högra hörnet och klicka på **inställningar**</span><span class="sxs-lookup"><span data-stu-id="97056-213">Hover over hello **cog** in hello top right-hand corner and click **Settings**</span></span>
   
   ![Lägga till medarbetare](./media/active-directory-saas-datahug-tutorial/1.png)

3. <span data-ttu-id="97056-215">Välj **personer** och klicka på hello **Lägg till användare** fliken</span><span class="sxs-lookup"><span data-stu-id="97056-215">Choose **People** and click hello **Add Users** tab</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-datahug-tutorial/2.png)

4. <span data-ttu-id="97056-217">Skriv hello e-post för hello person som toocreate ett konto för och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="97056-217">Type hello email of hello person you would like toocreate an account for and click **Add**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > <span data-ttu-id="97056-219">Du kan skicka e-post toouser för registrering genom att välja **skicka välkomstmeddelandet** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="97056-219">You can send registration mail toouser by selecting **Send welcome email** checkbox.</span></span>  
    > <span data-ttu-id="97056-220">Om du skapar ett konto för Salesforce inte skicka hello välkomstmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="97056-220">If you are creating an account for Salesforce do not send hello welcome email.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="97056-221">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="97056-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="97056-222">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDatahug.</span><span class="sxs-lookup"><span data-stu-id="97056-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDatahug.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="97056-224">**tooassign Britta Simon tooDatahug utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="97056-224">**tooassign Britta Simon tooDatahug, perform hello following steps:**</span></span>

1. <span data-ttu-id="97056-225">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="97056-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="97056-227">Välj i listan med program hello **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="97056-227">In hello applications list, select **Datahug**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. <span data-ttu-id="97056-229">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="97056-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="97056-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="97056-231">Click **Add** button.</span></span> <span data-ttu-id="97056-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97056-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="97056-234">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="97056-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="97056-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97056-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="97056-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97056-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="97056-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="97056-237">Testing single sign-on</span></span>

<span data-ttu-id="97056-238">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="97056-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="97056-239">Du bör få automatiskt inloggade tooyour Datahug programmet när du klickar på hello Datahug panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="97056-239">When you click hello Datahug tile in hello Access Panel, you should get automatically signed-on tooyour Datahug application.</span></span> <span data-ttu-id="97056-240">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="97056-240">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="97056-241">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="97056-241">Additional resources</span></span>

* [<span data-ttu-id="97056-242">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="97056-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="97056-243">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="97056-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png

