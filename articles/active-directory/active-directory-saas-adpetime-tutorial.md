---
title: "Självstudier: Azure Active Directory-integrering med ADP eTime | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ADP eTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 9a5c7d14a18220f8c7a5b14055c30662ecd8f14d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a><span data-ttu-id="103d7-103">Självstudier: Azure Active Directory-integrering med ADP eTime</span><span class="sxs-lookup"><span data-stu-id="103d7-103">Tutorial: Azure Active Directory integration with ADP eTime</span></span>

<span data-ttu-id="103d7-104">I kursen får du lära dig hur toointegrate ADP eTime med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="103d7-104">In this tutorial, you learn how toointegrate ADP eTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="103d7-105">Integrera ADP eTime med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="103d7-105">Integrating ADP eTime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="103d7-106">Du kan styra i Azure AD som har åtkomst tooADP eTime</span><span class="sxs-lookup"><span data-stu-id="103d7-106">You can control in Azure AD who has access tooADP eTime</span></span>
- <span data-ttu-id="103d7-107">Du kan aktivera din användare tooautomatically get inloggade tooADP eTime (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="103d7-107">You can enable your users tooautomatically get signed-on tooADP eTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="103d7-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="103d7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="103d7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="103d7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="103d7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="103d7-110">Prerequisites</span></span>

<span data-ttu-id="103d7-111">tooconfigure Azure AD-integrering med ADP eTime måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="103d7-111">tooconfigure Azure AD integration with ADP eTime, you need hello following items:</span></span>

- <span data-ttu-id="103d7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="103d7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="103d7-113">En ADP eTime enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="103d7-113">An ADP eTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="103d7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="103d7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="103d7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="103d7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="103d7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="103d7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="103d7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="103d7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="103d7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="103d7-118">Scenario description</span></span>
<span data-ttu-id="103d7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="103d7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="103d7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="103d7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="103d7-121">Att lägga till ADP eTime från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="103d7-121">Adding ADP eTime from hello gallery</span></span>
2. <span data-ttu-id="103d7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="103d7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-etime-from-hello-gallery"></a><span data-ttu-id="103d7-123">Att lägga till ADP eTime från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="103d7-123">Adding ADP eTime from hello gallery</span></span>
<span data-ttu-id="103d7-124">tooconfigure hello integrering av ADP eTime i Azure AD, behöver du tooadd ADP eTime hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="103d7-124">tooconfigure hello integration of ADP eTime into Azure AD, you need tooadd ADP eTime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="103d7-125">**tooadd ADP eTime från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="103d7-125">**tooadd ADP eTime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="103d7-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="103d7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="103d7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="103d7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="103d7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="103d7-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="103d7-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="103d7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="103d7-133">Skriv i sökrutan hello **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="103d7-133">In hello search box, type **ADP eTime**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. <span data-ttu-id="103d7-135">Markera hello resultat på panelen **ADP eTime**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="103d7-135">In hello results panel, select **ADP eTime**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="103d7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="103d7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="103d7-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ADP eTime baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="103d7-138">In this section, you configure and test Azure AD single sign-on with ADP eTime based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="103d7-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ADP eTime är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="103d7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ADP eTime is tooa user in Azure AD.</span></span> <span data-ttu-id="103d7-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ADP eTime toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="103d7-140">In other words, a link relationship between an Azure AD user and hello related user in ADP eTime needs toobe established.</span></span>

<span data-ttu-id="103d7-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ADP eTime.</span><span class="sxs-lookup"><span data-stu-id="103d7-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ADP eTime.</span></span>

<span data-ttu-id="103d7-142">tooconfigure och testa Azure AD enkel inloggning med ADP eTime, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="103d7-142">tooconfigure and test Azure AD single sign-on with ADP eTime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="103d7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="103d7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="103d7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="103d7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="103d7-145">**[Skapa en testanvändare för ADP eTime](#creating-an-adp-etime-test-user)**  -toohave en motsvarighet för Britta Simon i ADP eTime som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="103d7-145">**[Creating an ADP eTime test user](#creating-an-adp-etime-test-user)** - toohave a counterpart of Britta Simon in ADP eTime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="103d7-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="103d7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="103d7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="103d7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="103d7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="103d7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="103d7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt ADP eTime program.</span><span class="sxs-lookup"><span data-stu-id="103d7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ADP eTime application.</span></span>

<span data-ttu-id="103d7-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ADP eTime:**</span><span class="sxs-lookup"><span data-stu-id="103d7-150">**tooconfigure Azure AD single sign-on with ADP eTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="103d7-151">I hello Azure-portalen på hello **ADP eTime** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="103d7-151">In hello Azure portal, on hello **ADP eTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="103d7-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="103d7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. <span data-ttu-id="103d7-155">På hello **ADP eTime domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="103d7-155">On hello **ADP eTime Domain and URLs** section, perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    <span data-ttu-id="103d7-157">a.</span><span class="sxs-lookup"><span data-stu-id="103d7-157">a.</span></span> <span data-ttu-id="103d7-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<servername>.adp.com`</span><span class="sxs-lookup"><span data-stu-id="103d7-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<servername>.adp.com`</span></span>

    <span data-ttu-id="103d7-159">b.</span><span class="sxs-lookup"><span data-stu-id="103d7-159">b.</span></span> <span data-ttu-id="103d7-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="103d7-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span></span> 
 
    > [!NOTE] 
    > <span data-ttu-id="103d7-161">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="103d7-161">These values are not hello real.</span></span> <span data-ttu-id="103d7-162">Uppdatera dessa värden med hello faktiska Reply URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="103d7-162">Update these values with hello actual Reply URL and Identifier.</span></span> <span data-ttu-id="103d7-163">Kontakta [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="103d7-163">Contact [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) tooget these values.</span></span>

4. <span data-ttu-id="103d7-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="103d7-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. <span data-ttu-id="103d7-166">hello ADP eTime program förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="103d7-166">hello ADP eTime application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="103d7-167">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="103d7-167">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="103d7-168">hello anspråkets namn kommer alltid att **”PersonImmutableID”** och hello-värde som vi har mappat tooExtensionAttribute2 som innehåller hello EmployeeID för hello användare.</span><span class="sxs-lookup"><span data-stu-id="103d7-168">hello claim name will always be **"PersonImmutableID"** and hello value of which we have mapped tooExtensionAttribute2 which contains hello EmployeeID of hello user.</span></span> 

    <span data-ttu-id="103d7-169">Här hello mappning från Azure AD tooADP eTime kommer att utföras på hello EmployeeID, men du kan mappa tooa olika värdet också baserat på dina inställningar för programmet.</span><span class="sxs-lookup"><span data-stu-id="103d7-169">Here hello user mapping from Azure AD tooADP eTime will be done on hello EmployeeID but you can map this tooa different value also based on your application settings.</span></span> <span data-ttu-id="103d7-170">Så kan du arbeta med [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx) första toouse hello rätt ID för en användare och mappa värdet med hello **”PersonImmutableID”** anspråk.</span><span class="sxs-lookup"><span data-stu-id="103d7-170">So please work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) first toouse hello correct identifier of a user and map that value with hello **"PersonImmutableID"** claim.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. <span data-ttu-id="103d7-172">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="103d7-172">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="103d7-173">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="103d7-173">Attribute Name</span></span> | <span data-ttu-id="103d7-174">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="103d7-174">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="103d7-175">PersonImmutableID</span><span class="sxs-lookup"><span data-stu-id="103d7-175">PersonImmutableID</span></span> | <span data-ttu-id="103d7-176">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="103d7-176">user.extensionattribute2</span></span> |
    
    <span data-ttu-id="103d7-177">a.</span><span class="sxs-lookup"><span data-stu-id="103d7-177">a.</span></span> <span data-ttu-id="103d7-178">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="103d7-178">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="103d7-181">b.</span><span class="sxs-lookup"><span data-stu-id="103d7-181">b.</span></span> <span data-ttu-id="103d7-182">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="103d7-182">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="103d7-183">c.</span><span class="sxs-lookup"><span data-stu-id="103d7-183">c.</span></span> <span data-ttu-id="103d7-184">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="103d7-184">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="103d7-185">d.</span><span class="sxs-lookup"><span data-stu-id="103d7-185">d.</span></span> <span data-ttu-id="103d7-186">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="103d7-186">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="103d7-187">Innan du kan konfigurera hello SAML-kontroll måste toocontact din [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx) och begära hello värdet för attributet för hello-Unik identifierare för din klient.</span><span class="sxs-lookup"><span data-stu-id="103d7-187">Before you can configure hello SAML assertion, you need toocontact your [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="103d7-188">Du behöver det här värdet tooconfigure hello anpassat anspråk för ditt program.</span><span class="sxs-lookup"><span data-stu-id="103d7-188">You need this value tooconfigure hello custom claim for your application.</span></span> 

7. <span data-ttu-id="103d7-189">På hello **ADP eTime Configuration** klickar du på **konfigurera ADP eTime** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="103d7-189">On hello **ADP eTime Configuration** section, click **Configure ADP eTime** tooopen **Configure sign-on** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. <span data-ttu-id="103d7-191">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="103d7-191">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="103d7-193">tooconfigure enkel inloggning på **ADP eTime** sida, behöver du toosend hello hämtas **XML-Metadata för** för[ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="103d7-193">tooconfigure single sign-on on **ADP eTime** side, you need toosend hello downloaded **Metadata XML** too[ADP eTime support team](https://www.adp.com/contact-us/overview.aspx).</span></span> 

> [!TIP]
> <span data-ttu-id="103d7-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="103d7-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="103d7-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="103d7-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="103d7-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="103d7-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="103d7-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="103d7-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="103d7-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="103d7-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="103d7-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="103d7-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="103d7-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="103d7-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="103d7-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="103d7-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="103d7-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="103d7-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="103d7-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="103d7-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="103d7-209">a.</span><span class="sxs-lookup"><span data-stu-id="103d7-209">a.</span></span> <span data-ttu-id="103d7-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="103d7-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="103d7-211">b.</span><span class="sxs-lookup"><span data-stu-id="103d7-211">b.</span></span> <span data-ttu-id="103d7-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="103d7-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="103d7-213">c.</span><span class="sxs-lookup"><span data-stu-id="103d7-213">c.</span></span> <span data-ttu-id="103d7-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="103d7-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="103d7-215">d.</span><span class="sxs-lookup"><span data-stu-id="103d7-215">d.</span></span> <span data-ttu-id="103d7-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="103d7-216">Click **Create**.</span></span>
 
### <a name="creating-an-adp-etime-test-user"></a><span data-ttu-id="103d7-217">Skapa en ADP eTime testanvändare</span><span class="sxs-lookup"><span data-stu-id="103d7-217">Creating an ADP eTime test user</span></span>

<span data-ttu-id="103d7-218">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i ADP eTime.</span><span class="sxs-lookup"><span data-stu-id="103d7-218">hello objective of this section is toocreate a user called Britta Simon in ADP eTime.</span></span> <span data-ttu-id="103d7-219">Arbeta med [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx) tooadd hello användare i hello ADP eTime konto.</span><span class="sxs-lookup"><span data-stu-id="103d7-219">Work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) tooadd hello users in hello ADP eTime account.</span></span> 
   
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="103d7-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="103d7-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="103d7-221">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooADP eTime.</span><span class="sxs-lookup"><span data-stu-id="103d7-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooADP eTime.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="103d7-223">**tooassign Britta Simon tooADP eTime utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="103d7-223">**tooassign Britta Simon tooADP eTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="103d7-224">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="103d7-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="103d7-226">Välj i listan med program hello **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="103d7-226">In hello applications list, select **ADP eTime**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. <span data-ttu-id="103d7-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="103d7-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="103d7-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="103d7-230">Click **Add** button.</span></span> <span data-ttu-id="103d7-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="103d7-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="103d7-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="103d7-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="103d7-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="103d7-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="103d7-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="103d7-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="103d7-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="103d7-236">Testing single sign-on</span></span>

<span data-ttu-id="103d7-237">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="103d7-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="103d7-238">Du bör få automatiskt inloggade tooyour ADP eTime programmet när du klickar på hello ADP eTime panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="103d7-238">When you click hello ADP eTime tile in hello Access Panel, you should get automatically signed-on tooyour ADP eTime application.</span></span>
<span data-ttu-id="103d7-239">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="103d7-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="103d7-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="103d7-240">Additional resources</span></span>

* [<span data-ttu-id="103d7-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="103d7-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="103d7-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="103d7-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

