---
title: "Självstudier: Azure Active Directory-integrering med AirWatch | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och AirWatch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e5230d5a36824778a4d9804dadf9f13a0d11a68d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a><span data-ttu-id="73e3d-103">Självstudier: Azure Active Directory-integrering med AirWatch</span><span class="sxs-lookup"><span data-stu-id="73e3d-103">Tutorial: Azure Active Directory integration with AirWatch</span></span>

<span data-ttu-id="73e3d-104">I kursen får du lära dig hur toointegrate AirWatch med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="73e3d-104">In this tutorial, you learn how toointegrate AirWatch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="73e3d-105">Integrera AirWatch med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="73e3d-105">Integrating AirWatch with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="73e3d-106">Du kan styra i Azure AD som har åtkomst till tooAirWatch</span><span class="sxs-lookup"><span data-stu-id="73e3d-106">You can control in Azure AD who has access tooAirWatch</span></span>
- <span data-ttu-id="73e3d-107">Du kan aktivera din användare tooautomatically get inloggade tooAirWatch (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="73e3d-107">You can enable your users tooautomatically get signed-on tooAirWatch (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="73e3d-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="73e3d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="73e3d-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="73e3d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73e3d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="73e3d-110">Prerequisites</span></span>

<span data-ttu-id="73e3d-111">tooconfigure Azure AD-integrering med AirWatch, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="73e3d-111">tooconfigure Azure AD integration with AirWatch, you need hello following items:</span></span>

- <span data-ttu-id="73e3d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="73e3d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="73e3d-113">En AirWatch enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="73e3d-113">An AirWatch single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="73e3d-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="73e3d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="73e3d-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="73e3d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="73e3d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="73e3d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="73e3d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="73e3d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="73e3d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="73e3d-118">Scenario description</span></span>
<span data-ttu-id="73e3d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="73e3d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="73e3d-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="73e3d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="73e3d-121">Att lägga till AirWatch från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="73e3d-121">Adding AirWatch from hello gallery</span></span>
2. <span data-ttu-id="73e3d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73e3d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-airwatch-from-hello-gallery"></a><span data-ttu-id="73e3d-123">Att lägga till AirWatch från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="73e3d-123">Adding AirWatch from hello gallery</span></span>
<span data-ttu-id="73e3d-124">tooconfigure hello integrering av AirWatch i Azure AD, behöver du tooadd AirWatch hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="73e3d-124">tooconfigure hello integration of AirWatch into Azure AD, you need tooadd AirWatch from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="73e3d-125">**tooadd AirWatch från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="73e3d-125">**tooadd AirWatch from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="73e3d-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="73e3d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="73e3d-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="73e3d-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="73e3d-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73e3d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="73e3d-133">Skriv i sökrutan hello **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-133">In hello search box, type **AirWatch**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. <span data-ttu-id="73e3d-135">Markera hello resultat på panelen **AirWatch**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="73e3d-135">In hello results panel, select **AirWatch**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="73e3d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73e3d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="73e3d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AirWatch baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="73e3d-138">In this section, you configure and test Azure AD single sign-on with AirWatch based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="73e3d-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i AirWatch är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="73e3d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AirWatch is tooa user in Azure AD.</span></span> <span data-ttu-id="73e3d-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i AirWatch toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="73e3d-140">In other words, a link relationship between an Azure AD user and hello related user in AirWatch needs toobe established.</span></span>

<span data-ttu-id="73e3d-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i AirWatch.</span><span class="sxs-lookup"><span data-stu-id="73e3d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in AirWatch.</span></span>

<span data-ttu-id="73e3d-142">tooconfigure och testa Azure AD enkel inloggning med AirWatch, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="73e3d-142">tooconfigure and test Azure AD single sign-on with AirWatch, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="73e3d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="73e3d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="73e3d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="73e3d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="73e3d-145">**[Skapa en testanvändare AirWatch](#creating-a-airwatch-test-user)**  -toohave en motsvarighet för Britta Simon i AirWatch som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="73e3d-145">**[Creating a AirWatch test user](#creating-a-airwatch-test-user)** - toohave a counterpart of Britta Simon in AirWatch that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="73e3d-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="73e3d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="73e3d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="73e3d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="73e3d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73e3d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="73e3d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt AirWatch program.</span><span class="sxs-lookup"><span data-stu-id="73e3d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AirWatch application.</span></span>

<span data-ttu-id="73e3d-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med AirWatch:**</span><span class="sxs-lookup"><span data-stu-id="73e3d-150">**tooconfigure Azure AD single sign-on with AirWatch, perform hello following steps:**</span></span>

1. <span data-ttu-id="73e3d-151">I hello Azure-portalen på hello **AirWatch** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-151">In hello Azure portal, on hello **AirWatch** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="73e3d-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="73e3d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. <span data-ttu-id="73e3d-155">På hello **AirWatch domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73e3d-155">On hello **AirWatch Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    <span data-ttu-id="73e3d-157">a.</span><span class="sxs-lookup"><span data-stu-id="73e3d-157">a.</span></span> <span data-ttu-id="73e3d-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span><span class="sxs-lookup"><span data-stu-id="73e3d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span></span>

    <span data-ttu-id="73e3d-159">b.</span><span class="sxs-lookup"><span data-stu-id="73e3d-159">b.</span></span> <span data-ttu-id="73e3d-160">I hello **identifierare** textruta hello TYPVÄRDE som`AirWatch`</span><span class="sxs-lookup"><span data-stu-id="73e3d-160">In hello **Identifier** textbox, type hello value as `AirWatch`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="73e3d-161">Det här värdet är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="73e3d-161">This value is not hello real.</span></span> <span data-ttu-id="73e3d-162">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="73e3d-162">Update this value with hello actual Sign-on URL.</span></span> <span data-ttu-id="73e3d-163">Kontakta [AirWatch klienten supportteamet](http://www.air-watch.com/company/contact-us/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="73e3d-163">Contact [AirWatch Client support team](http://www.air-watch.com/company/contact-us/) tooget this value.</span></span> 
 
4. <span data-ttu-id="73e3d-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="73e3d-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. <span data-ttu-id="73e3d-166">På hello **AirWatch Configuration** klickar du på **konfigurera AirWatch** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="73e3d-166">On hello **AirWatch Configuration** section, click **Configure AirWatch** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="73e3d-167">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="73e3d-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. <span data-ttu-id="73e3d-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="73e3d-169">Click **Save** button.</span></span>

    <span data-ttu-id="73e3d-170">![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="73e3d-170">![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span></span>
7. <span data-ttu-id="73e3d-171">Logga in tooyour AirWatch företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="73e3d-171">In a different web browser window, log in tooyour AirWatch company site as an administrator.</span></span>

8. <span data-ttu-id="73e3d-172">Hello vänstra navigeringsfönstret, klicka på **konton**, och klicka sedan på **administratörer**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-172">In hello left navigation pane, click **Accounts**, and then click **Administrators**.</span></span>
   
   <span data-ttu-id="73e3d-173">![Administratörer](./media/active-directory-saas-airwatch-tutorial/ic791920.png "administratörer")</span><span class="sxs-lookup"><span data-stu-id="73e3d-173">![Administrators](./media/active-directory-saas-airwatch-tutorial/ic791920.png "Administrators")</span></span>

9. <span data-ttu-id="73e3d-174">Expandera hello **inställningar** -menyn och klicka sedan på **Directory Services**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-174">Expand hello **Settings** menu, and then click **Directory Services**.</span></span>
   
   <span data-ttu-id="73e3d-175">![Inställningar för](./media/active-directory-saas-airwatch-tutorial/ic791921.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="73e3d-175">![Settings](./media/active-directory-saas-airwatch-tutorial/ic791921.png "Settings")</span></span>

10. <span data-ttu-id="73e3d-176">Klicka på hello **användare** på fliken hello **Bas-DN** textruta Skriv ditt domännamn och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-176">Click hello **User** tab, in hello **Base DN** textbox, type your domain name, and then click **Save**.</span></span>
   
   <span data-ttu-id="73e3d-177">![Användaren](./media/active-directory-saas-airwatch-tutorial/ic791922.png "användare")</span><span class="sxs-lookup"><span data-stu-id="73e3d-177">![User](./media/active-directory-saas-airwatch-tutorial/ic791922.png "User")</span></span>

11. <span data-ttu-id="73e3d-178">Klicka på hello **Server** fliken.</span><span class="sxs-lookup"><span data-stu-id="73e3d-178">Click hello **Server** tab.</span></span>
   
   <span data-ttu-id="73e3d-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")</span><span class="sxs-lookup"><span data-stu-id="73e3d-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")</span></span>

12. <span data-ttu-id="73e3d-180">Utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="73e3d-180">Perform hello following steps:</span></span>
    
    <span data-ttu-id="73e3d-181">![Överför](./media/active-directory-saas-airwatch-tutorial/ic791924.png "överför")</span><span class="sxs-lookup"><span data-stu-id="73e3d-181">![Upload](./media/active-directory-saas-airwatch-tutorial/ic791924.png "Upload")</span></span>   
    
    <span data-ttu-id="73e3d-182">a.</span><span class="sxs-lookup"><span data-stu-id="73e3d-182">a.</span></span> <span data-ttu-id="73e3d-183">Som **katalogtyp**väljer **ingen**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-183">As **Directory Type**, select **None**.</span></span>

    <span data-ttu-id="73e3d-184">b.</span><span class="sxs-lookup"><span data-stu-id="73e3d-184">b.</span></span> <span data-ttu-id="73e3d-185">Välj **använder SAML för autentisering**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-185">Select **Use SAML For Authentication**.</span></span>

    <span data-ttu-id="73e3d-186">c.</span><span class="sxs-lookup"><span data-stu-id="73e3d-186">c.</span></span> <span data-ttu-id="73e3d-187">tooupload hello hämtat certifikat klickar du på **överför**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-187">tooupload hello downloaded certificate, click **Upload**.</span></span>

13. <span data-ttu-id="73e3d-188">I hello **begära** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73e3d-188">In hello **Request** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="73e3d-189">![Begära](./media/active-directory-saas-airwatch-tutorial/ic791925.png "begäran")</span><span class="sxs-lookup"><span data-stu-id="73e3d-189">![Request](./media/active-directory-saas-airwatch-tutorial/ic791925.png "Request")</span></span>  

    <span data-ttu-id="73e3d-190">a.</span><span class="sxs-lookup"><span data-stu-id="73e3d-190">a.</span></span> <span data-ttu-id="73e3d-191">Som **begära bindning av typen**väljer **efter**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-191">As **Request Binding Type**, select **POST**.</span></span>

    <span data-ttu-id="73e3d-192">b.</span><span class="sxs-lookup"><span data-stu-id="73e3d-192">b.</span></span> <span data-ttu-id="73e3d-193">I hello Azure-portalen på hello **Konfigurera enkel inloggning på Airwatch** dialogrutan sida, kopiera hello **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in den i hello **identitetsleverantören. Enkel inloggning på URL: en** textruta.</span><span class="sxs-lookup"><span data-stu-id="73e3d-193">In hello Azure portal, on hello **Configure single sign-on at Airwatch** dialog page, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **Identity Provider Single Sign On URL** textbox.</span></span>

    <span data-ttu-id="73e3d-194">c.</span><span class="sxs-lookup"><span data-stu-id="73e3d-194">c.</span></span> <span data-ttu-id="73e3d-195">Som **NameID Format**väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-195">As **NameID Format**, select **Email Address**.</span></span>

    <span data-ttu-id="73e3d-196">d.</span><span class="sxs-lookup"><span data-stu-id="73e3d-196">d.</span></span> <span data-ttu-id="73e3d-197">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-197">Click **Save**.</span></span>

14. <span data-ttu-id="73e3d-198">Klicka på hello **användaren** fliken igen.</span><span class="sxs-lookup"><span data-stu-id="73e3d-198">Click hello **User** tab again.</span></span>
    
    <span data-ttu-id="73e3d-199">![Användaren](./media/active-directory-saas-airwatch-tutorial/ic791926.png "användare")</span><span class="sxs-lookup"><span data-stu-id="73e3d-199">![User](./media/active-directory-saas-airwatch-tutorial/ic791926.png "User")</span></span>

15. <span data-ttu-id="73e3d-200">I hello **attributet** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73e3d-200">In hello **Attribute** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="73e3d-201">![Attributet](./media/active-directory-saas-airwatch-tutorial/ic791927.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="73e3d-201">![Attribute](./media/active-directory-saas-airwatch-tutorial/ic791927.png "Attribute")</span></span>

    <span data-ttu-id="73e3d-202">a.</span><span class="sxs-lookup"><span data-stu-id="73e3d-202">a.</span></span> <span data-ttu-id="73e3d-203">I hello **objektidentifierare** textruta typen **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-203">In hello **Object Identifier** textbox, type **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span></span>

    <span data-ttu-id="73e3d-204">b.</span><span class="sxs-lookup"><span data-stu-id="73e3d-204">b.</span></span> <span data-ttu-id="73e3d-205">I hello **användarnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-205">In hello **Username** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="73e3d-206">c.</span><span class="sxs-lookup"><span data-stu-id="73e3d-206">c.</span></span> <span data-ttu-id="73e3d-207">I hello **visningsnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-207">In hello **Display Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="73e3d-208">d.</span><span class="sxs-lookup"><span data-stu-id="73e3d-208">d.</span></span> <span data-ttu-id="73e3d-209">I hello **Förnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-209">In hello **First Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="73e3d-210">e.</span><span class="sxs-lookup"><span data-stu-id="73e3d-210">e.</span></span> <span data-ttu-id="73e3d-211">I hello **efternamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-211">In hello **Last Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

    <span data-ttu-id="73e3d-212">f.</span><span class="sxs-lookup"><span data-stu-id="73e3d-212">f.</span></span> <span data-ttu-id="73e3d-213">I hello **e-post** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-213">In hello **Email** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="73e3d-214">g.</span><span class="sxs-lookup"><span data-stu-id="73e3d-214">g.</span></span> <span data-ttu-id="73e3d-215">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-215">Click **Save**.</span></span>

<CE>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="73e3d-216">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="73e3d-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="73e3d-217">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="73e3d-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="73e3d-219">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="73e3d-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="73e3d-220">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="73e3d-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="73e3d-222">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="73e3d-224">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73e3d-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="73e3d-226">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73e3d-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="73e3d-228">a.</span><span class="sxs-lookup"><span data-stu-id="73e3d-228">a.</span></span> <span data-ttu-id="73e3d-229">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="73e3d-230">b.</span><span class="sxs-lookup"><span data-stu-id="73e3d-230">b.</span></span> <span data-ttu-id="73e3d-231">I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="73e3d-231">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="73e3d-232">c.</span><span class="sxs-lookup"><span data-stu-id="73e3d-232">c.</span></span> <span data-ttu-id="73e3d-233">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="73e3d-234">d.</span><span class="sxs-lookup"><span data-stu-id="73e3d-234">d.</span></span> <span data-ttu-id="73e3d-235">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-235">Click **Create**.</span></span>
 
### <a name="creating-a-airwatch-test-user"></a><span data-ttu-id="73e3d-236">Skapa en testanvändare AirWatch</span><span class="sxs-lookup"><span data-stu-id="73e3d-236">Creating a AirWatch test user</span></span>

<span data-ttu-id="73e3d-237">tooenable Azure AD-användare toolog i tooAirWatch, måste de vara etablerade i tooAirWatch.</span><span class="sxs-lookup"><span data-stu-id="73e3d-237">tooenable Azure AD users toolog in tooAirWatch, they must be provisioned in tooAirWatch.</span></span>

* <span data-ttu-id="73e3d-238">När AirWatch, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="73e3d-238">When AirWatch, provisioning is a manual task.</span></span>

<span data-ttu-id="73e3d-239">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="73e3d-239">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="73e3d-240">Logga in tooyour **AirWatch** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="73e3d-240">Log in tooyour **AirWatch** company site as administrator.</span></span>
2. <span data-ttu-id="73e3d-241">Hello navigeringsfönstret hello vänster, klicka på **konton**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-241">In hello navigation pane on hello left side, click **Accounts**, and then click **Users**.</span></span>
   
   <span data-ttu-id="73e3d-242">![Användare](./media/active-directory-saas-airwatch-tutorial/ic791929.png "användare")</span><span class="sxs-lookup"><span data-stu-id="73e3d-242">![Users](./media/active-directory-saas-airwatch-tutorial/ic791929.png "Users")</span></span>
3. <span data-ttu-id="73e3d-243">I hello **användare** -menyn klickar du på **listvyn**, och klicka sedan på **Lägg till \> Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-243">In hello **Users** menu, click **List View**, and then click **Add \> Add User**.</span></span>
   
   <span data-ttu-id="73e3d-244">![Lägg till användare](./media/active-directory-saas-airwatch-tutorial/ic791930.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="73e3d-244">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791930.png "Add User")</span></span>
4. <span data-ttu-id="73e3d-245">På hello **Lägg till / redigera användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73e3d-245">On hello **Add / Edit User** dialog, perform hello following steps:</span></span>

   <span data-ttu-id="73e3d-246">![Lägg till användare](./media/active-directory-saas-airwatch-tutorial/ic791931.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="73e3d-246">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791931.png "Add User")</span></span>   
   1. <span data-ttu-id="73e3d-247">Typen hello **användarnamn**, **lösenord**, **Bekräfta lösenord**, **Förnamn**, **efternamn**,  **E-postadress** av en giltig Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="73e3d-247">Type hello **Username**, **Password**, **Confirm Password**, **First Name**, **Last Name**, **Email Address** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   2. <span data-ttu-id="73e3d-248">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-248">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="73e3d-249">Du kan använda något annat AirWatch användarens konto skapas verktyg eller API: er som tillhandahålls av AirWatch tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="73e3d-249">You can use any other AirWatch user account creation tools or APIs provided by AirWatch tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="73e3d-250">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="73e3d-250">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="73e3d-251">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAirWatch.</span><span class="sxs-lookup"><span data-stu-id="73e3d-251">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAirWatch.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="73e3d-253">**tooassign Britta Simon tooAirWatch utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="73e3d-253">**tooassign Britta Simon tooAirWatch, perform hello following steps:**</span></span>

1. <span data-ttu-id="73e3d-254">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-254">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="73e3d-256">Välj i listan med program hello **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-256">In hello applications list, select **AirWatch**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. <span data-ttu-id="73e3d-258">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="73e3d-258">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="73e3d-260">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="73e3d-260">Click **Add** button.</span></span> <span data-ttu-id="73e3d-261">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73e3d-261">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="73e3d-263">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="73e3d-263">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="73e3d-264">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73e3d-264">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="73e3d-265">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73e3d-265">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="73e3d-266">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73e3d-266">Testing single sign-on</span></span>

<span data-ttu-id="73e3d-267">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="73e3d-267">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="73e3d-268">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="73e3d-268">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="73e3d-269">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="73e3d-269">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="73e3d-270">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="73e3d-270">Additional resources</span></span>

* [<span data-ttu-id="73e3d-271">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="73e3d-271">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="73e3d-272">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="73e3d-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

