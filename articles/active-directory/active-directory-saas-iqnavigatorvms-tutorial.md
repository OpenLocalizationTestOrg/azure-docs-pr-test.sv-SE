---
title: "Självstudier: Azure Active Directory-integrering med IQNavigator VMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och IQNavigator VMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 5a5a7dd3f427acfba2f0ae10552a7179db730118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a><span data-ttu-id="14eb2-103">Självstudier: Azure Active Directory-integrering med IQNavigator VMS</span><span class="sxs-lookup"><span data-stu-id="14eb2-103">Tutorial: Azure Active Directory integration with IQNavigator VMS</span></span>

<span data-ttu-id="14eb2-104">I kursen får du lära dig hur toointegrate IQNavigator VMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="14eb2-104">In this tutorial, you learn how toointegrate IQNavigator VMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="14eb2-105">Integrera IQNavigator VMS med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="14eb2-105">Integrating IQNavigator VMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="14eb2-106">Du kan styra i Azure AD som har åtkomst tooIQNavigator virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="14eb2-106">You can control in Azure AD who has access tooIQNavigator VMS</span></span>
- <span data-ttu-id="14eb2-107">Du kan aktivera din användare tooautomatically get inloggade tooIQNavigator virtuella datorer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="14eb2-107">You can enable your users tooautomatically get signed-on tooIQNavigator VMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="14eb2-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="14eb2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="14eb2-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="14eb2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="14eb2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="14eb2-110">Prerequisites</span></span>

<span data-ttu-id="14eb2-111">tooconfigure Azure AD-integrering med IQNavigator VMS, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="14eb2-111">tooconfigure Azure AD integration with IQNavigator VMS, you need hello following items:</span></span>

- <span data-ttu-id="14eb2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="14eb2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="14eb2-113">En IQNavigator VMS enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="14eb2-113">A IQNavigator VMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="14eb2-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="14eb2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="14eb2-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="14eb2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="14eb2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="14eb2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="14eb2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="14eb2-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="14eb2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="14eb2-118">Scenario description</span></span>
<span data-ttu-id="14eb2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="14eb2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="14eb2-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="14eb2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="14eb2-121">Att lägga till IQNavigator VMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="14eb2-121">Adding IQNavigator VMS from hello gallery</span></span>
2. <span data-ttu-id="14eb2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="14eb2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-iqnavigator-vms-from-hello-gallery"></a><span data-ttu-id="14eb2-123">Att lägga till IQNavigator VMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="14eb2-123">Adding IQNavigator VMS from hello gallery</span></span>
<span data-ttu-id="14eb2-124">tooconfigure hello integrering av IQNavigator VMS i Azure AD, behöver du tooadd IQNavigator VMS hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="14eb2-124">tooconfigure hello integration of IQNavigator VMS into Azure AD, you need tooadd IQNavigator VMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="14eb2-125">**tooadd IQNavigator VMS från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="14eb2-125">**tooadd IQNavigator VMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="14eb2-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="14eb2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="14eb2-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="14eb2-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="14eb2-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="14eb2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="14eb2-133">Skriv i sökrutan hello **IQNavigator VMS**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-133">In hello search box, type **IQNavigator VMS**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

5. <span data-ttu-id="14eb2-135">Markera hello resultat på panelen **IQNavigator VMS**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="14eb2-135">In hello results panel, select **IQNavigator VMS**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="14eb2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="14eb2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="14eb2-138">Du konfigurera och testa Azure AD enkel inloggning med IQNavigator VMS baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="14eb2-138">In this section, you configure and test Azure AD single sign-on with IQNavigator VMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="14eb2-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i IQNavigator VMS är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="14eb2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in IQNavigator VMS is tooa user in Azure AD.</span></span> <span data-ttu-id="14eb2-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i IQNavigator VMS toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="14eb2-140">In other words, a link relationship between an Azure AD user and hello related user in IQNavigator VMS needs toobe established.</span></span>

<span data-ttu-id="14eb2-141">Tilldela hello värdet för hello i IQNavigator VMS **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="14eb2-141">In IQNavigator VMS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="14eb2-142">tooconfigure och testa Azure AD enkel inloggning med IQNavigator VMS, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="14eb2-142">tooconfigure and test Azure AD single sign-on with IQNavigator VMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="14eb2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="14eb2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="14eb2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="14eb2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="14eb2-145">**[Skapa en testanvändare IQNavigator VMS](#creating-a-iqnavigator-vms-test-user)**  -toohave en motsvarighet för Britta Simon i IQNavigator VMS som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="14eb2-145">**[Creating a IQNavigator VMS test user](#creating-a-iqnavigator-vms-test-user)** - toohave a counterpart of Britta Simon in IQNavigator VMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="14eb2-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="14eb2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="14eb2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="14eb2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="14eb2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="14eb2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="14eb2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt IQNavigator VMS-program.</span><span class="sxs-lookup"><span data-stu-id="14eb2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your IQNavigator VMS application.</span></span>

<span data-ttu-id="14eb2-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med IQNavigator VMS:**</span><span class="sxs-lookup"><span data-stu-id="14eb2-150">**tooconfigure Azure AD single sign-on with IQNavigator VMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="14eb2-151">I hello Azure-portalen på hello **IQNavigator VMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-151">In hello Azure portal, on hello **IQNavigator VMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="14eb2-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="14eb2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

3. <span data-ttu-id="14eb2-155">På hello **IQNavigator VMS domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="14eb2-155">On hello **IQNavigator VMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    <span data-ttu-id="14eb2-157">a.</span><span class="sxs-lookup"><span data-stu-id="14eb2-157">a.</span></span> <span data-ttu-id="14eb2-158">I hello **identifierare** textruta typen hello URL:`iqn.com`</span><span class="sxs-lookup"><span data-stu-id="14eb2-158">In hello **Identifier** textbox, type hello URL:`iqn.com`</span></span>

    <span data-ttu-id="14eb2-159">b.</span><span class="sxs-lookup"><span data-stu-id="14eb2-159">b.</span></span> <span data-ttu-id="14eb2-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="14eb2-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span></span>

4. <span data-ttu-id="14eb2-161">Kontrollera **visa avancerade inställningar för URL: en**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="14eb2-161">Check **Show advanced URL settings**, perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    <span data-ttu-id="14eb2-163">I hello **Relay tillstånd** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.iqnavigator.com`</span><span class="sxs-lookup"><span data-stu-id="14eb2-163">In hello **Relay state** textbox, type a URL using hello following pattern:`https://<subdomain>.iqnavigator.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="14eb2-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="14eb2-164">These values are not real.</span></span> <span data-ttu-id="14eb2-165">Uppdatera dessa värden med hello faktiska Reply URL och Relay tillstånd.</span><span class="sxs-lookup"><span data-stu-id="14eb2-165">Update these values with hello actual Reply URL and Relay state.</span></span> <span data-ttu-id="14eb2-166">Kontakta [IQNavigator VMS klienten supportteamet](https://www.beeline.com/iqn-product-support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="14eb2-166">Contact [IQNavigator VMS Client support team](https://www.beeline.com/iqn-product-support/) tooget these values.</span></span> 

5. <span data-ttu-id="14eb2-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="14eb2-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="14eb2-169">toogenerate hello **Metadata** url, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="14eb2-169">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="14eb2-170">a.</span><span class="sxs-lookup"><span data-stu-id="14eb2-170">a.</span></span> <span data-ttu-id="14eb2-171">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-171">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appregistrations.png)
   
    <span data-ttu-id="14eb2-173">b.</span><span class="sxs-lookup"><span data-stu-id="14eb2-173">b.</span></span> <span data-ttu-id="14eb2-174">Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="14eb2-174">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpointicon.png)

    <span data-ttu-id="14eb2-176">c.</span><span class="sxs-lookup"><span data-stu-id="14eb2-176">c.</span></span> <span data-ttu-id="14eb2-177">Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="14eb2-177">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpoint.png)
     
    <span data-ttu-id="14eb2-179">d.</span><span class="sxs-lookup"><span data-stu-id="14eb2-179">d.</span></span> <span data-ttu-id="14eb2-180">Gå nu toohello egenskapssida **IQNavigator VMS** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="14eb2-180">Now go toohello property page of **IQNavigator VMS** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appid.png)

    <span data-ttu-id="14eb2-182">e.</span><span class="sxs-lookup"><span data-stu-id="14eb2-182">e.</span></span> <span data-ttu-id="14eb2-183">Generera hello **URL för tjänstmetadata** med hello följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="14eb2-183">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

7. <span data-ttu-id="14eb2-184">IQNavigator program förväntar dig hello unikt ID-värde i hello namnidentifierare anspråk.</span><span class="sxs-lookup"><span data-stu-id="14eb2-184">IQNavigator application expect hello unique user identifier value in hello Name Identifier claim.</span></span> <span data-ttu-id="14eb2-185">Kunden kan mappa hello rätt värde för hello namnidentifierare anspråk.</span><span class="sxs-lookup"><span data-stu-id="14eb2-185">Customer can map hello correct value for hello Name Identifier claim.</span></span> <span data-ttu-id="14eb2-186">I det här fallet har vi mappat hello användare. UserPrincipalName för hello demo ändamål.</span><span class="sxs-lookup"><span data-stu-id="14eb2-186">In this case we have mapped hello user.UserPrincipalName for hello demo purpose.</span></span> <span data-ttu-id="14eb2-187">Men enligt tooyour organisationsinställningar ska du mappa hello rätt värde för den.</span><span class="sxs-lookup"><span data-stu-id="14eb2-187">But according tooyour organization settings you should map hello correct value for it.</span></span>   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

8. <span data-ttu-id="14eb2-189">På hello **IQNavigator VMS Configuration** klickar du på **konfigurera IQNavigator VMS** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="14eb2-189">On hello **IQNavigator VMS Configuration** section, click **Configure IQNavigator VMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="14eb2-190">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="14eb2-190">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png) 

9. <span data-ttu-id="14eb2-192">tooconfigure enkel inloggning på **IQNavigator VMS** sida, behöver du toosend hello **URL för tjänstmetadata**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för [IQNavigator VMS supportteamet](https://www.beeline.com/iqn-product-support/).</span><span class="sxs-lookup"><span data-stu-id="14eb2-192">tooconfigure single sign-on on **IQNavigator VMS** side, you need toosend hello **Metadata URL**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/).</span></span> <span data-ttu-id="14eb2-193">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="14eb2-193">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="14eb2-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="14eb2-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="14eb2-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="14eb2-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="14eb2-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="14eb2-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="14eb2-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="14eb2-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="14eb2-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="14eb2-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="14eb2-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="14eb2-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="14eb2-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="14eb2-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="14eb2-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="14eb2-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="14eb2-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="14eb2-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="14eb2-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="14eb2-209">a.</span><span class="sxs-lookup"><span data-stu-id="14eb2-209">a.</span></span> <span data-ttu-id="14eb2-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="14eb2-211">b.</span><span class="sxs-lookup"><span data-stu-id="14eb2-211">b.</span></span> <span data-ttu-id="14eb2-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="14eb2-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="14eb2-213">c.</span><span class="sxs-lookup"><span data-stu-id="14eb2-213">c.</span></span> <span data-ttu-id="14eb2-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="14eb2-215">d.</span><span class="sxs-lookup"><span data-stu-id="14eb2-215">d.</span></span> <span data-ttu-id="14eb2-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-216">Click **Create**.</span></span>
 
### <a name="creating-a-iqnavigator-vms-test-user"></a><span data-ttu-id="14eb2-217">Skapa en IQNavigator VMS testanvändare</span><span class="sxs-lookup"><span data-stu-id="14eb2-217">Creating a IQNavigator VMS test user</span></span>

<span data-ttu-id="14eb2-218">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i IQNavigator VMS.</span><span class="sxs-lookup"><span data-stu-id="14eb2-218">hello objective of this section is toocreate a user called Britta Simon in IQNavigator VMS.</span></span> <span data-ttu-id="14eb2-219">Arbeta med [IQNavigator VMS supportteamet](https://www.beeline.com/iqn-product-support/) tooadd hello användare i hello IQNavigator VMS-konto.</span><span class="sxs-lookup"><span data-stu-id="14eb2-219">Work with  [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/) tooadd hello users in hello IQNavigator VMS account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="14eb2-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="14eb2-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="14eb2-221">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIQNavigator virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="14eb2-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIQNavigator VMS.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="14eb2-223">**tooassign Britta Simon tooIQNavigator virtuella datorer, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="14eb2-223">**tooassign Britta Simon tooIQNavigator VMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="14eb2-224">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="14eb2-226">Välj i listan med program hello **IQNavigator VMS**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-226">In hello applications list, select **IQNavigator VMS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png) 

3. <span data-ttu-id="14eb2-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="14eb2-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="14eb2-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="14eb2-230">Click **Add** button.</span></span> <span data-ttu-id="14eb2-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="14eb2-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="14eb2-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="14eb2-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="14eb2-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="14eb2-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="14eb2-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="14eb2-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="14eb2-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="14eb2-236">Testing single sign-on</span></span>

<span data-ttu-id="14eb2-237">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="14eb2-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="14eb2-238">Du bör få automatiskt inloggade tooyour IQNavigator VMS programmet när du klickar på hello IQNavigator VMS-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="14eb2-238">When you click hello IQNavigator VMS tile in hello Access Panel, you should get automatically signed-on tooyour IQNavigator VMS application.</span></span>
<span data-ttu-id="14eb2-239">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="14eb2-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="14eb2-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="14eb2-240">Additional resources</span></span>

* [<span data-ttu-id="14eb2-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="14eb2-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="14eb2-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="14eb2-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_203.png

