---
title: "Självstudier: Azure Active Directory-integrering med Cisco Spark | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Cisco Spark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 386c4fd816095e1c61de01dd1dee1bbd00311a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a><span data-ttu-id="334de-103">Självstudier: Azure Active Directory-integrering med Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="334de-103">Tutorial: Azure Active Directory integration with Cisco Spark</span></span>

<span data-ttu-id="334de-104">I kursen får du lära dig hur toointegrate Cisco Väck med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="334de-104">In this tutorial, you learn how toointegrate Cisco Spark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="334de-105">Integrera Cisco Spark med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="334de-105">Integrating Cisco Spark with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="334de-106">Du kan styra i Azure AD som har åtkomst tooCisco Spark</span><span class="sxs-lookup"><span data-stu-id="334de-106">You can control in Azure AD who has access tooCisco Spark</span></span>
- <span data-ttu-id="334de-107">Du kan aktivera din användare tooautomatically get inloggade tooCisco Spark (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="334de-107">You can enable your users tooautomatically get signed-on tooCisco Spark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="334de-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="334de-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="334de-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="334de-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="334de-110">Krav</span><span class="sxs-lookup"><span data-stu-id="334de-110">Prerequisites</span></span>

<span data-ttu-id="334de-111">tooconfigure Azure AD-integrering med Cisco Spark, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="334de-111">tooconfigure Azure AD integration with Cisco Spark, you need hello following items:</span></span>

- <span data-ttu-id="334de-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="334de-112">An Azure AD subscription</span></span>
- <span data-ttu-id="334de-113">En Cisco-Spark enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="334de-113">A Cisco Spark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="334de-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="334de-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="334de-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="334de-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="334de-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="334de-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="334de-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="334de-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="334de-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="334de-118">Scenario description</span></span>
<span data-ttu-id="334de-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="334de-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="334de-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="334de-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="334de-121">Att lägga till Cisco Spark från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="334de-121">Adding Cisco Spark from hello gallery</span></span>
2. <span data-ttu-id="334de-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="334de-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cisco-spark-from-hello-gallery"></a><span data-ttu-id="334de-123">Att lägga till Cisco Spark från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="334de-123">Adding Cisco Spark from hello gallery</span></span>
<span data-ttu-id="334de-124">tooconfigure hello integrering av Cisco Spark i Azure AD, behöver du tooadd Cisco Spark hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="334de-124">tooconfigure hello integration of Cisco Spark into Azure AD, you need tooadd Cisco Spark from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="334de-125">**tooadd Cisco Spark från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="334de-125">**tooadd Cisco Spark from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="334de-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="334de-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="334de-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="334de-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="334de-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="334de-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="334de-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="334de-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="334de-133">Skriv i sökrutan hello **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="334de-133">In hello search box, type **Cisco Spark**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. <span data-ttu-id="334de-135">Markera hello resultat på panelen **Cisco Spark**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="334de-135">In hello results panel, select **Cisco Spark**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="334de-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="334de-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="334de-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Cisco Spark baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="334de-138">In this section, you configure and test Azure AD single sign-on with Cisco Spark based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="334de-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Cisco Spark är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="334de-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cisco Spark is tooa user in Azure AD.</span></span> <span data-ttu-id="334de-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Cisco Spark toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="334de-140">In other words, a link relationship between an Azure AD user and hello related user in Cisco Spark needs toobe established.</span></span>

<span data-ttu-id="334de-141">Tilldela hello värdet för hello i Cisco Spark **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="334de-141">In Cisco Spark, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="334de-142">tooconfigure och testa Azure AD enkel inloggning med Cisco Spark, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="334de-142">tooconfigure and test Azure AD single sign-on with Cisco Spark, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="334de-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="334de-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="334de-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="334de-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="334de-145">**[Skapa en testanvändare Cisco Spark](#creating-a-cisco-spark-test-user)**  -toohave en motsvarighet för Britta Simon i Cisco Spark som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="334de-145">**[Creating a Cisco Spark test user](#creating-a-cisco-spark-test-user)** - toohave a counterpart of Britta Simon in Cisco Spark that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="334de-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="334de-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="334de-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="334de-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="334de-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="334de-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="334de-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Cisco Spark-program.</span><span class="sxs-lookup"><span data-stu-id="334de-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cisco Spark application.</span></span>

<span data-ttu-id="334de-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Cisco Spark:**</span><span class="sxs-lookup"><span data-stu-id="334de-150">**tooconfigure Azure AD single sign-on with Cisco Spark, perform hello following steps:**</span></span>

1. <span data-ttu-id="334de-151">I hello Azure-portalen på hello **Cisco Spark** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="334de-151">In hello Azure portal, on hello **Cisco Spark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="334de-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="334de-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. <span data-ttu-id="334de-155">På hello **Cisco Spark domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="334de-155">On hello **Cisco Spark Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    <span data-ttu-id="334de-157">a.</span><span class="sxs-lookup"><span data-stu-id="334de-157">a.</span></span> <span data-ttu-id="334de-158">I hello **inloggnings-URL** textruta Skriv en URL som:`https://web.ciscospark.com/#/signin`</span><span class="sxs-lookup"><span data-stu-id="334de-158">In hello **Sign-on URL** textbox, type a URL as: `https://web.ciscospark.com/#/signin`</span></span>

    <span data-ttu-id="334de-159">b.</span><span class="sxs-lookup"><span data-stu-id="334de-159">b.</span></span> <span data-ttu-id="334de-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://idbroker.webex.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="334de-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://idbroker.webex.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="334de-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="334de-161">This value is not real.</span></span> <span data-ttu-id="334de-162">Uppdatera det här värdet med hello faktiska identifierare.</span><span class="sxs-lookup"><span data-stu-id="334de-162">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="334de-163">Kontakta [Cisco Spark klient supportteamet](https://support.ciscospark.com/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="334de-163">Contact [Cisco Spark Client support team](https://support.ciscospark.com/) tooget this value.</span></span> 
 
4. <span data-ttu-id="334de-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="334de-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. <span data-ttu-id="334de-166">Cisco Spark program förväntar hello SAML intyg toocontain specifika attribut.</span><span class="sxs-lookup"><span data-stu-id="334de-166">Cisco Spark application expects hello SAML assertions toocontain specific attributes.</span></span> <span data-ttu-id="334de-167">Konfigurera hello följande attribut för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="334de-167">Configure hello following attributes  for this application.</span></span> <span data-ttu-id="334de-168">Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="334de-168">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="334de-169">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="334de-169">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. <span data-ttu-id="334de-171">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="334de-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="334de-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="334de-172">Attribute Name</span></span>  | <span data-ttu-id="334de-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="334de-173">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    |   <span data-ttu-id="334de-174">UID</span><span class="sxs-lookup"><span data-stu-id="334de-174">uid</span></span>    | <span data-ttu-id="334de-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="334de-175">user.userprincipalname</span></span> |   

    <span data-ttu-id="334de-176">a.</span><span class="sxs-lookup"><span data-stu-id="334de-176">a.</span></span> <span data-ttu-id="334de-177">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="334de-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="334de-180">b.</span><span class="sxs-lookup"><span data-stu-id="334de-180">b.</span></span> <span data-ttu-id="334de-181">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="334de-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="334de-182">c.</span><span class="sxs-lookup"><span data-stu-id="334de-182">c.</span></span> <span data-ttu-id="334de-183">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="334de-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="334de-184">d.</span><span class="sxs-lookup"><span data-stu-id="334de-184">d.</span></span> <span data-ttu-id="334de-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="334de-185">Click **Ok**.</span></span>

7. <span data-ttu-id="334de-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="334de-186">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="334de-188">Logga in för[Cisco Molnhantering samarbete](https://admin.ciscospark.com/) med din fullständiga administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="334de-188">Sign in too[Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

9. <span data-ttu-id="334de-189">Välj **inställningar** och under hello **autentisering** klickar du på **ändra**.</span><span class="sxs-lookup"><span data-stu-id="334de-189">Select **Settings** and under hello **Authentication** section, click **Modify**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. <span data-ttu-id="334de-191">Välj **integrera en identitetsleverantör med 3 part. (Avancerat)**  och gå toohello nästa skärm.</span><span class="sxs-lookup"><span data-stu-id="334de-191">Select **Integrate a 3rd-party identity provider. (Advanced)** and go toohello next screen.</span></span>

11. <span data-ttu-id="334de-192">På hello **importera Idp Metadata** sidan antingen dra och släppa hello Azure AD-metadatafil på hello sida eller använda hello filen webbläsare alternativet toolocate och överför hello Azure AD-metadatafil.</span><span class="sxs-lookup"><span data-stu-id="334de-192">On hello **Import Idp Metadata** page, either drag and drop hello Azure AD metadata file onto hello page or use hello file browser option toolocate and upload hello Azure AD metadata file.</span></span> <span data-ttu-id="334de-193">Markera **kräver certifikat som signerats av en certifikatutfärdare i Metadata (säkrare)** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="334de-193">Then, select **Require certificate signed by a certificate authority in Metadata (more secure)** and click **Next**.</span></span> 
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. <span data-ttu-id="334de-195">Välj **SSO Testanslutningen**, och när en ny webbläsarflik öppnas autentisera med Azure AD genom att logga in.</span><span class="sxs-lookup"><span data-stu-id="334de-195">Select **Test SSO Connection**, and when a new browser tab opens, authenticate with Azure AD by signing in.</span></span>

13. <span data-ttu-id="334de-196">Returnera toohello **Cisco samarbete Molnhantering** flik i webbläsaren. Om hello testet lyckades, väljer **det här testet lyckades. Aktivera enkel inloggning alternativet** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="334de-196">Return toohello **Cisco Cloud Collaboration Management** browser tab. If hello test was successful, select **This test was successful. Enable Single Sign-On option** and click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="334de-197">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="334de-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="334de-198">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="334de-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="334de-199">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="334de-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="334de-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="334de-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="334de-201">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="334de-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="334de-203">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="334de-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="334de-204">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="334de-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="334de-206">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="334de-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="334de-208">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="334de-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="334de-210">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="334de-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="334de-212">a.</span><span class="sxs-lookup"><span data-stu-id="334de-212">a.</span></span> <span data-ttu-id="334de-213">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="334de-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="334de-214">b.</span><span class="sxs-lookup"><span data-stu-id="334de-214">b.</span></span> <span data-ttu-id="334de-215">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="334de-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="334de-216">c.</span><span class="sxs-lookup"><span data-stu-id="334de-216">c.</span></span> <span data-ttu-id="334de-217">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="334de-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="334de-218">d.</span><span class="sxs-lookup"><span data-stu-id="334de-218">d.</span></span> <span data-ttu-id="334de-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="334de-219">Click **Create**.</span></span>
 
### <a name="creating-a-cisco-spark-test-user"></a><span data-ttu-id="334de-220">Skapa en testanvändare Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="334de-220">Creating a Cisco Spark test user</span></span>

<span data-ttu-id="334de-221">I det här avsnittet skapar du en användare som kallas Britta Simon i Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="334de-221">In this section, you create a user called Britta Simon in Cisco Spark.</span></span> <span data-ttu-id="334de-222">I det här avsnittet skapar du en användare som kallas Britta Simon i Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="334de-222">In this section, you create a user called Britta Simon in Cisco Spark.</span></span>

1. <span data-ttu-id="334de-223">Gå toohello [Cisco Molnhantering samarbete](https://admin.ciscospark.com/) med din fullständiga administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="334de-223">Go toohello [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

2. <span data-ttu-id="334de-224">Klicka på **användare** och sedan **hantera användare**.</span><span class="sxs-lookup"><span data-stu-id="334de-224">Click **Users** and then **Manage Users**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. <span data-ttu-id="334de-226">I hello **hantera användare** väljer **manuellt lägga till eller ändra användare** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="334de-226">In hello **Manage User** window, select **Manually add or modify users** and click **Next**.</span></span>

4. <span data-ttu-id="334de-227">Välj **namn och e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="334de-227">Select **Names and Email address**.</span></span> <span data-ttu-id="334de-228">Sedan fylla hello textrutan på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="334de-228">Then, fill out hello textbox as follows:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    <span data-ttu-id="334de-230">a.</span><span class="sxs-lookup"><span data-stu-id="334de-230">a.</span></span> <span data-ttu-id="334de-231">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="334de-231">In hello **First Name** textbox, type **Britta**.</span></span> 
    
    <span data-ttu-id="334de-232">b.</span><span class="sxs-lookup"><span data-stu-id="334de-232">b.</span></span> <span data-ttu-id="334de-233">I hello **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="334de-233">In hello **Last Name** textbox, type **Simon**.</span></span>
    
    <span data-ttu-id="334de-234">c.</span><span class="sxs-lookup"><span data-stu-id="334de-234">c.</span></span> <span data-ttu-id="334de-235">I hello **e-postadress** textruta typen  **britta.simon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="334de-235">In hello **Email address** textbox, type **britta.simon@contoso.com**.</span></span>

5. <span data-ttu-id="334de-236">Klicka på hello plus logga tooadd Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="334de-236">Click hello plus sign tooadd Britta Simon.</span></span> <span data-ttu-id="334de-237">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="334de-237">Then, click **Next**.</span></span>

6. <span data-ttu-id="334de-238">I hello **Lägg till tjänster för användare** -fönstret klickar du på **spara** och sedan **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="334de-238">In hello **Add Services for Users** window, click **Save** and then **Finish**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="334de-239">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="334de-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="334de-240">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCisco Spark.</span><span class="sxs-lookup"><span data-stu-id="334de-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCisco Spark.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="334de-242">**tooassign Britta Simon tooCisco Spark, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="334de-242">**tooassign Britta Simon tooCisco Spark, perform hello following steps:**</span></span>

1. <span data-ttu-id="334de-243">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="334de-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="334de-245">Välj i listan med program hello **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="334de-245">In hello applications list, select **Cisco Spark**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. <span data-ttu-id="334de-247">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="334de-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="334de-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="334de-249">Click **Add** button.</span></span> <span data-ttu-id="334de-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="334de-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="334de-252">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="334de-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="334de-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="334de-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="334de-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="334de-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="334de-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="334de-255">Testing single sign-on</span></span>

<span data-ttu-id="334de-256">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="334de-256">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="334de-257">När du klickar på hello Cisco Spark-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Cisco Spark-program.</span><span class="sxs-lookup"><span data-stu-id="334de-257">When you click hello Cisco Spark tile in hello Access Panel, you should get automatically signed-on tooyour Cisco Spark application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="334de-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="334de-258">Additional resources</span></span>

* [<span data-ttu-id="334de-259">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="334de-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="334de-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="334de-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

