---
title: "Självstudier: Azure Active Directory-integrering med Wdesk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Wdesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 2c278a5e4f50cc362663efc0f826ff0c0fcf6e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a><span data-ttu-id="e0558-103">Självstudier: Azure Active Directory-integrering med Wdesk</span><span class="sxs-lookup"><span data-stu-id="e0558-103">Tutorial: Azure Active Directory integration with Wdesk</span></span>

<span data-ttu-id="e0558-104">I kursen får du lära dig hur toointegrate Wdesk med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e0558-104">In this tutorial, you learn how toointegrate Wdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e0558-105">Integrera Wdesk med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e0558-105">Integrating Wdesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e0558-106">Du kan styra i Azure AD som har åtkomst till tooWdesk</span><span class="sxs-lookup"><span data-stu-id="e0558-106">You can control in Azure AD who has access tooWdesk</span></span>
- <span data-ttu-id="e0558-107">Du kan aktivera din användare tooautomatically get inloggade tooWdesk (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e0558-107">You can enable your users tooautomatically get signed-on tooWdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e0558-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e0558-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e0558-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0558-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="e0558-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e0558-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0558-111">Krav</span><span class="sxs-lookup"><span data-stu-id="e0558-111">Prerequisites</span></span>

<span data-ttu-id="e0558-112">tooconfigure Azure AD-integrering med Wdesk, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e0558-112">tooconfigure Azure AD integration with Wdesk, you need hello following items:</span></span>

- <span data-ttu-id="e0558-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e0558-113">An Azure AD subscription</span></span>
- <span data-ttu-id="e0558-114">En Wdesk enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e0558-114">A Wdesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e0558-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e0558-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e0558-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e0558-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e0558-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e0558-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e0558-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e0558-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e0558-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e0558-119">Scenario description</span></span>
<span data-ttu-id="e0558-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e0558-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e0558-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e0558-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e0558-122">Att lägga till Wdesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e0558-122">Adding Wdesk from hello gallery</span></span>
2. <span data-ttu-id="e0558-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0558-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wdesk-from-hello-gallery"></a><span data-ttu-id="e0558-124">Att lägga till Wdesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e0558-124">Adding Wdesk from hello gallery</span></span>
<span data-ttu-id="e0558-125">tooconfigure hello integrering av Wdesk i Azure AD, behöver du tooadd Wdesk hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e0558-125">tooconfigure hello integration of Wdesk into Azure AD, you need tooadd Wdesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e0558-126">**tooadd Wdesk från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e0558-126">**tooadd Wdesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0558-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e0558-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e0558-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e0558-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e0558-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e0558-130">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e0558-132">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0558-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e0558-134">Skriv i sökrutan hello **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="e0558-134">In hello search box, type **Wdesk**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. <span data-ttu-id="e0558-136">Markera hello resultat på panelen **Wdesk**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e0558-136">In hello results panel, select **Wdesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e0558-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0558-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e0558-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Wdesk baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e0558-139">In this section, you configure and test Azure AD single sign-on with Wdesk based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e0558-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Wdesk är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0558-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Wdesk is tooa user in Azure AD.</span></span> <span data-ttu-id="e0558-141">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Wdesk toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e0558-141">In other words, a link relationship between an Azure AD user and hello related user in Wdesk needs toobe established.</span></span>

<span data-ttu-id="e0558-142">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Wdesk.</span><span class="sxs-lookup"><span data-stu-id="e0558-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Wdesk.</span></span>

<span data-ttu-id="e0558-143">tooconfigure och testa Azure AD enkel inloggning med Wdesk, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e0558-143">tooconfigure and test Azure AD single sign-on with Wdesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e0558-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e0558-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e0558-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0558-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e0558-146">**[Skapa en testanvändare Wdesk](#creating-a-wdesk-test-user)**  -toohave en motsvarighet för Britta Simon i Wdesk som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e0558-146">**[Creating a Wdesk test user](#creating-a-wdesk-test-user)** - toohave a counterpart of Britta Simon in Wdesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e0558-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e0558-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e0558-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e0558-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e0558-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0558-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e0558-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Wdesk program.</span><span class="sxs-lookup"><span data-stu-id="e0558-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Wdesk application.</span></span>

<span data-ttu-id="e0558-151">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Wdesk:**</span><span class="sxs-lookup"><span data-stu-id="e0558-151">**tooconfigure Azure AD single sign-on with Wdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0558-152">I hello Azure-portalen på hello **Wdesk** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e0558-152">In hello Azure portal, on hello **Wdesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e0558-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e0558-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. <span data-ttu-id="e0558-156">På hello **Wdesk domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0558-156">On hello **Wdesk Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    <span data-ttu-id="e0558-158">a.</span><span class="sxs-lookup"><span data-stu-id="e0558-158">a.</span></span> <span data-ttu-id="e0558-159">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="e0558-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span></span>

    <span data-ttu-id="e0558-160">b.</span><span class="sxs-lookup"><span data-stu-id="e0558-160">b.</span></span> <span data-ttu-id="e0558-161">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="e0558-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span></span>

4. <span data-ttu-id="e0558-162">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="e0558-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="e0558-163">Om du inte vill tooconfigure hello programmet i **SP** initierade läge, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0558-163">If you wish tooconfigure hello application in **SP** initiated mode, perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    <span data-ttu-id="e0558-165">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="e0558-165">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="e0558-166">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e0558-166">These values are not real.</span></span> <span data-ttu-id="e0558-167">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="e0558-167">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="e0558-168">Du kan få värdena från WDesk portal när du konfigurerar hello enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e0558-168">You get these values from WDesk portal when you configure hello SSO.</span></span> 
  
5. <span data-ttu-id="e0558-169">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="e0558-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. <span data-ttu-id="e0558-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0558-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="e0558-173">I en annan webbläsarfönstret, inloggning tooWdesk som en administratör.</span><span class="sxs-lookup"><span data-stu-id="e0558-173">In a different web browser window, login tooWdesk as a Security Administrator.</span></span>

8. <span data-ttu-id="e0558-174">I hello längst ned till vänster, klickar du på **Admin** och välj **kontoadministratören**:</span><span class="sxs-lookup"><span data-stu-id="e0558-174">In hello bottom left, click **Admin** and choose **Account Admin**:</span></span>
 
     ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. <span data-ttu-id="e0558-176">Wdesk Admin, navigera för**säkerhet**, sedan **SAML** > **SAML inställningar**:</span><span class="sxs-lookup"><span data-stu-id="e0558-176">In Wdesk Admin, navigate too**Security**, then **SAML** > **SAML Settings**:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. <span data-ttu-id="e0558-178">Under **allmänna inställningar**, kontrollera hello **aktivera SAML enkel inloggning**:</span><span class="sxs-lookup"><span data-stu-id="e0558-178">Under **General Settings**, check hello **Enable SAML Single Sign On**:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. <span data-ttu-id="e0558-180">Under **information om Service Provider**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0558-180">Under **Service Provider Details**, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      <span data-ttu-id="e0558-182">a.</span><span class="sxs-lookup"><span data-stu-id="e0558-182">a.</span></span> <span data-ttu-id="e0558-183">Kopiera hello **Inloggningswebbadressen** och klistra in den i **inloggnings-Url** textruta på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0558-183">Copy hello **Login URL** and paste it in **Sign-on Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="e0558-184">b.</span><span class="sxs-lookup"><span data-stu-id="e0558-184">b.</span></span> <span data-ttu-id="e0558-185">Kopiera hello **Url för tjänstmetadata** och klistra in den i **identifierare** textruta på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0558-185">Copy hello **Metadata Url** and paste it in **Identifier** textbox on Azure portal.</span></span>
       
      <span data-ttu-id="e0558-186">c.</span><span class="sxs-lookup"><span data-stu-id="e0558-186">c.</span></span> <span data-ttu-id="e0558-187">Kopiera hello **konsumenten url** och klistra in den i **Reply Url** textruta på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0558-187">Copy hello **Consumer url** and paste it in **Reply Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="e0558-188">d.</span><span class="sxs-lookup"><span data-stu-id="e0558-188">d.</span></span> <span data-ttu-id="e0558-189">Klicka på **spara** på Azure portal toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="e0558-189">Click **Save** on Azure portal toosave hello changes.</span></span>      

12. <span data-ttu-id="e0558-190">Klicka på **konfigurera inställningarna för IdP** tooopen **redigera inställningar för IdP** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0558-190">Click **Configure IdP Settings** tooopen **Edit IdP Settings** dialog.</span></span> <span data-ttu-id="e0558-191">Klicka på **Välj fil** toolocate hello **Metadata.xml** fil som du sparade från Azure-portalen sedan ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="e0558-191">Click **Choose File** toolocate hello **Metadata.xml** file you saved from Azure portal, then upload it.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. <span data-ttu-id="e0558-193">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="e0558-193">Click **Save changes**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> <span data-ttu-id="e0558-195">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="e0558-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e0558-196">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="e0558-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e0558-197">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e0558-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e0558-198">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0558-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="e0558-199">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0558-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e0558-201">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e0558-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0558-202">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e0558-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e0558-204">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e0558-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e0558-206">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0558-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e0558-208">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0558-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e0558-210">a.</span><span class="sxs-lookup"><span data-stu-id="e0558-210">a.</span></span> <span data-ttu-id="e0558-211">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e0558-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e0558-212">b.</span><span class="sxs-lookup"><span data-stu-id="e0558-212">b.</span></span> <span data-ttu-id="e0558-213">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e0558-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e0558-214">c.</span><span class="sxs-lookup"><span data-stu-id="e0558-214">c.</span></span> <span data-ttu-id="e0558-215">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e0558-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e0558-216">d.</span><span class="sxs-lookup"><span data-stu-id="e0558-216">d.</span></span> <span data-ttu-id="e0558-217">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e0558-217">Click **Create**.</span></span>
 
### <a name="creating-a-wdesk-test-user"></a><span data-ttu-id="e0558-218">Skapa en testanvändare Wdesk</span><span class="sxs-lookup"><span data-stu-id="e0558-218">Creating a Wdesk test user</span></span>

<span data-ttu-id="e0558-219">tooenable Azure AD-användare toolog i tooWdesk, måste de etableras i Wdesk.</span><span class="sxs-lookup"><span data-stu-id="e0558-219">tooenable Azure AD users toolog in tooWdesk, they must be provisioned into Wdesk.</span></span> <span data-ttu-id="e0558-220">I Wdesk är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e0558-220">In Wdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="e0558-221">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="e0558-221">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0558-222">Logga in tooWdesk som en administratör.</span><span class="sxs-lookup"><span data-stu-id="e0558-222">Log in tooWdesk as a Security Administrator.</span></span>
2. <span data-ttu-id="e0558-223">Navigera för**Admin** > **kontoadministratören**.</span><span class="sxs-lookup"><span data-stu-id="e0558-223">Navigate too**Admin** > **Account Admin**.</span></span>

     ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. <span data-ttu-id="e0558-225">Klicka på **medlemmar** under **personer**.</span><span class="sxs-lookup"><span data-stu-id="e0558-225">Click **Members** under **People**.</span></span>

4. <span data-ttu-id="e0558-226">Klicka på **Lägg till medlem** tooopen **Lägg till medlem** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0558-226">Now click **Add Member** tooopen **Add Member** dialog box.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. <span data-ttu-id="e0558-228">I **användare** text Ange hello användarnamnet för användaren som  **brittasimon@contoso.com**  och på **Fortsätt** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0558-228">In **User** text box, enter hello username of user like **brittasimon@contoso.com** and click **Continue** button.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  <span data-ttu-id="e0558-230">Ange hello information som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="e0558-230">Enter hello details as shown below:</span></span>
  
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    <span data-ttu-id="e0558-232">a.</span><span class="sxs-lookup"><span data-stu-id="e0558-232">a.</span></span> <span data-ttu-id="e0558-233">I **e-post** text Ange hello e-postadressen för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="e0558-233">In **E-mail** text box, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="e0558-234">b.</span><span class="sxs-lookup"><span data-stu-id="e0558-234">b.</span></span> <span data-ttu-id="e0558-235">I **Förnamn** text Ange hello första namn för användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="e0558-235">In **First Name** text box, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="e0558-236">c.</span><span class="sxs-lookup"><span data-stu-id="e0558-236">c.</span></span> <span data-ttu-id="e0558-237">I **efternamn** text Ange hello efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="e0558-237">In **Last Name** text box, enter hello last name of user like **Simon**.</span></span>

7. <span data-ttu-id="e0558-238">Klicka på **spara medlemmen** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0558-238">Click **Save Member** button.</span></span>  

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e0558-240">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0558-240">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e0558-241">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWdesk.</span><span class="sxs-lookup"><span data-stu-id="e0558-241">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWdesk.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e0558-243">**tooassign Britta Simon tooWdesk utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e0558-243">**tooassign Britta Simon tooWdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0558-244">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e0558-244">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e0558-246">Välj i listan med program hello **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="e0558-246">In hello applications list, select **Wdesk**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. <span data-ttu-id="e0558-248">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e0558-248">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e0558-250">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0558-250">Click **Add** button.</span></span> <span data-ttu-id="e0558-251">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0558-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e0558-253">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e0558-253">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e0558-254">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0558-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e0558-255">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0558-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e0558-256">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0558-256">Testing single sign-on</span></span>

<span data-ttu-id="e0558-257">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e0558-257">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e0558-258">Du bör få automatiskt inloggade tooyour Wdesk programmet när du klickar på hello Wdesk panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e0558-258">When you click hello Wdesk tile in hello Access Panel, you should get automatically signed-on tooyour Wdesk application.</span></span>
<span data-ttu-id="e0558-259">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e0558-259">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="e0558-260">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e0558-260">Additional resources</span></span>

* [<span data-ttu-id="e0558-261">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e0558-261">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e0558-262">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e0558-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

