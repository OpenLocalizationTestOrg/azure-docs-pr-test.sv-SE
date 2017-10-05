---
title: "Självstudier: Azure Active Directory-integrering med ScaleX Enterprise | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ScaleX Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: 0ebed0c2605862426384c0e219e52c9d626b6246
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a><span data-ttu-id="579f9-103">Självstudier: Azure Active Directory-integrering med ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="579f9-103">Tutorial: Azure Active Directory integration with ScaleX Enterprise</span></span>

<span data-ttu-id="579f9-104">I kursen får lära du att integrera ScaleX Enterprise med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="579f9-104">In this tutorial, you learn how to integrate ScaleX Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="579f9-105">Integrera ScaleX Enterprise med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="579f9-105">Integrating ScaleX Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="579f9-106">Du kan styra i Azure AD som har åtkomst till ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="579f9-106">You can control in Azure AD who has access to ScaleX Enterprise</span></span>
- <span data-ttu-id="579f9-107">Du kan aktivera användarna att automatiskt hämta loggat in på ScaleX Enterprise (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="579f9-107">You can enable your users to automatically get signed-on to ScaleX Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="579f9-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="579f9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="579f9-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns i.</span><span class="sxs-lookup"><span data-stu-id="579f9-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="579f9-110">Vad är programåtkomst och enkel inloggning med [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="579f9-110">What is application access and single sign-on with [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="579f9-111">Krav</span><span class="sxs-lookup"><span data-stu-id="579f9-111">Prerequisites</span></span>

<span data-ttu-id="579f9-112">För att konfigurera Azure AD-integrering med ScaleX Enterprise, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="579f9-112">To configure Azure AD integration with ScaleX Enterprise, you need the following items:</span></span>

- <span data-ttu-id="579f9-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="579f9-113">An Azure AD subscription</span></span>
- <span data-ttu-id="579f9-114">En ScaleX Enterprise enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="579f9-114">A ScaleX Enterprise single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="579f9-115">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="579f9-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="579f9-116">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="579f9-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="579f9-117">Använd inte i produktionsmiljön, om detta är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="579f9-117">Do not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="579f9-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="579f9-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="579f9-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="579f9-119">Scenario description</span></span>
<span data-ttu-id="579f9-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="579f9-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="579f9-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="579f9-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="579f9-122">Att lägga till ScaleX Enterprise från galleriet</span><span class="sxs-lookup"><span data-stu-id="579f9-122">Adding ScaleX Enterprise from the gallery</span></span>
2. <span data-ttu-id="579f9-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="579f9-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-scalex-enterprise-from-the-gallery"></a><span data-ttu-id="579f9-124">Att lägga till ScaleX Enterprise från galleriet</span><span class="sxs-lookup"><span data-stu-id="579f9-124">Adding ScaleX Enterprise from the gallery</span></span>
<span data-ttu-id="579f9-125">Du måste lägga till ScaleX Enterprise från galleriet i listan över hanterade SaaS-appar för att konfigurera ScaleX företag i Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="579f9-125">To configure the integration of ScaleX Enterprise in to Azure AD, you need to add ScaleX Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="579f9-126">**Utför följande steg för att lägga till ScaleX Enterprise från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="579f9-126">**To add ScaleX Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="579f9-127">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="579f9-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="579f9-129">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="579f9-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="579f9-130">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="579f9-130">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="579f9-132">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="579f9-132">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="579f9-134">I sökrutan skriver **ScaleX Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="579f9-134">In the search box, type **ScaleX Enterprise**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. <span data-ttu-id="579f9-136">Välj i resultatpanelen **ScaleX Enterprise**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="579f9-136">In the results panel, select **ScaleX Enterprise**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="579f9-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="579f9-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="579f9-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ScaleX Enterprise baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="579f9-139">In this section, you configure and test Azure AD single sign-on with ScaleX Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="579f9-140">Azure AD måste du känna till användaren i ScaleX Enterprise motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="579f9-140">For single sign-on to work, Azure AD needs to know what the counterpart user in ScaleX Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="579f9-141">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ScaleX Enterprise upprättas.</span><span class="sxs-lookup"><span data-stu-id="579f9-141">In other words, a link relationship between an Azure AD user and the related user in ScaleX Enterprise needs to be established.</span></span>

<span data-ttu-id="579f9-142">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i ScaleX företag.</span><span class="sxs-lookup"><span data-stu-id="579f9-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ScaleX Enterprise.</span></span>

<span data-ttu-id="579f9-143">Om du vill konfigurera och testa Azure AD enkel inloggning med ScaleX Enterprise, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="579f9-143">To configure and test Azure AD single sign-on with ScaleX Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="579f9-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="579f9-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="579f9-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="579f9-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="579f9-146">**[Skapa en testanvändare ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  – du har en motsvarighet för Britta Simon i ScaleX företag som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="579f9-146">**[Creating a ScaleX Enterprise test user](#creating-a-scalex-enterprise-test-user)** - to have a counterpart of Britta Simon in ScaleX Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="579f9-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="579f9-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="579f9-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="579f9-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="579f9-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="579f9-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="579f9-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ScaleX Enterprise-programmet.</span><span class="sxs-lookup"><span data-stu-id="579f9-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ScaleX Enterprise application.</span></span>

<span data-ttu-id="579f9-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ScaleX Enterprise:**</span><span class="sxs-lookup"><span data-stu-id="579f9-151">**To configure Azure AD single sign-on with ScaleX Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="579f9-152">I Azure-portalen på den **ScaleX Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="579f9-152">In the Azure portal, on the **ScaleX Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="579f9-154">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="579f9-154">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. <span data-ttu-id="579f9-156">På den **ScaleX företagsdomänen och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="579f9-156">On the **ScaleX Enterprise Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    <span data-ttu-id="579f9-158">a.</span><span class="sxs-lookup"><span data-stu-id="579f9-158">a.</span></span> <span data-ttu-id="579f9-159">I den **identifierare** textruta Skriv det värde som använder följande mönster:`https://platform.rescale.com/saml2/<company id>/`</span><span class="sxs-lookup"><span data-stu-id="579f9-159">In the **Identifier** textbox, type the value using the following pattern: `https://platform.rescale.com/saml2/<company id>/`</span></span>

    <span data-ttu-id="579f9-160">b.</span><span class="sxs-lookup"><span data-stu-id="579f9-160">b.</span></span> <span data-ttu-id="579f9-161">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://platform.rescale.com/saml2/<company id>/acs/`</span><span class="sxs-lookup"><span data-stu-id="579f9-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://platform.rescale.com/saml2/<company id>/acs/`</span></span>

4. <span data-ttu-id="579f9-162">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="579f9-162">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    <span data-ttu-id="579f9-164">I den **inloggnings-URL** textruta Skriv det värde som använder följande mönster:`https://platform.rescale.com/saml2/<company id>/sso/`</span><span class="sxs-lookup"><span data-stu-id="579f9-164">In the **Sign-on URL** textbox, type the value using the following pattern: `https://platform.rescale.com/saml2/<company id>/sso/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="579f9-165">Detta är inte de verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="579f9-165">These are not the real values.</span></span> <span data-ttu-id="579f9-166">Uppdatera dessa värden med den faktiska identifierare, Reply URL eller inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="579f9-166">Update these values with the actual Identifier, Reply URL or Sign-On URL.</span></span> <span data-ttu-id="579f9-167">Kontakta [ScaleX Enterprise-klienten supportteamet](http://info.rescale.com/contact_sales) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="579f9-167">Contact [ScaleX Enterprise Client support team](http://info.rescale.com/contact_sales) to get these values.</span></span> 

5. <span data-ttu-id="579f9-168">Tillämpningsprogrammet ScaleX förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan ändra det anpassade attributmappningar i konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="579f9-168">Your ScaleX application expects the SAML assertions in a specific format, which requires you to modify custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="579f9-169">Klicka på **visa och redigera andra användarattribut** kryssrutan för att öppna ett anpassat attribut inställningar.</span><span class="sxs-lookup"><span data-stu-id="579f9-169">Click **View and edit all other user attributes** checkbox to open the custom attributes settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    <span data-ttu-id="579f9-171">a.</span><span class="sxs-lookup"><span data-stu-id="579f9-171">a.</span></span> <span data-ttu-id="579f9-172">Högerklicka på attributet **namnet** och klicka på Ta bort.</span><span class="sxs-lookup"><span data-stu-id="579f9-172">Right click the attribute **name** and click delete.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    <span data-ttu-id="579f9-174">b.</span><span class="sxs-lookup"><span data-stu-id="579f9-174">b.</span></span> <span data-ttu-id="579f9-175">Klicka på **e-postadress** attribut för att öppna fönstret Redigera attribut.</span><span class="sxs-lookup"><span data-stu-id="579f9-175">Click **emailaddress** attribute to open the Edit Attribute window.</span></span> <span data-ttu-id="579f9-176">Ändra dess värde från **user.mail** till **user.userprincipalname** och klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="579f9-176">Change its value from **user.mail** to **user.userprincipalname** and click Ok.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. <span data-ttu-id="579f9-178">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="579f9-178">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the Certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. <span data-ttu-id="579f9-180">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="579f9-180">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="579f9-182">På den **ScaleX företagskonfiguration** klickar du på **konfigurera ScaleX Enterprise** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="579f9-182">On the **ScaleX Enterprise Configuration** section, click **Configure ScaleX Enterprise** to open **Configure sign-on** window.</span></span> <span data-ttu-id="579f9-183">Kopiera den **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="579f9-183">Copy the **SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. <span data-ttu-id="579f9-185">Konfigurera enkel inloggning på **ScaleX Enterprise** sida, inloggning till ScaleX Enterprise webbplatsen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="579f9-185">To configure single sign-on on **ScaleX Enterprise** side, login to the ScaleX Enterprise company website as an administrator.</span></span>

9. <span data-ttu-id="579f9-186">Klicka på menyn i övre högra och välj **Contoso Administration**.</span><span class="sxs-lookup"><span data-stu-id="579f9-186">Click the menu in the upper right and select **Contoso Administration**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="579f9-187">Contoso är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="579f9-187">Contoso is just an example.</span></span> <span data-ttu-id="579f9-188">Detta bör vara faktiska företagets namn.</span><span class="sxs-lookup"><span data-stu-id="579f9-188">This should be your actual Company Name.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. <span data-ttu-id="579f9-190">Välj **integreringar** översta menyn och välj **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="579f9-190">Select **Integrations** from the top menu and select **Single Sign-On**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. <span data-ttu-id="579f9-192">Fyll i formuläret på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="579f9-192">Complete the form as follows:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    <span data-ttu-id="579f9-194">a.</span><span class="sxs-lookup"><span data-stu-id="579f9-194">a.</span></span> <span data-ttu-id="579f9-195">Välj **”skapa alla användare som kan autentisera med enkel inloggning”.**</span><span class="sxs-lookup"><span data-stu-id="579f9-195">Select **“Create any user who can authenticate with SSO.”**</span></span>

    <span data-ttu-id="579f9-196">b.</span><span class="sxs-lookup"><span data-stu-id="579f9-196">b.</span></span> <span data-ttu-id="579f9-197">**Saml-leverantör**: klistra in värdet ***urn: oasis: namn: tc: SAML:2.0:nameid-format: beständiga***</span><span class="sxs-lookup"><span data-stu-id="579f9-197">**Service Provider saml**: Paste the value ***urn:oasis:names:tc:SAML:2.0:nameid-format:persistent***</span></span>

    <span data-ttu-id="579f9-198">c.</span><span class="sxs-lookup"><span data-stu-id="579f9-198">c.</span></span> <span data-ttu-id="579f9-199">**Namnet på identitetsleverantör e-fält i ACS-svar**: klistra in värdet`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="579f9-199">**Name of Identity Provider email field in ACS response**: Paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>

    <span data-ttu-id="579f9-200">d.</span><span class="sxs-lookup"><span data-stu-id="579f9-200">d.</span></span> <span data-ttu-id="579f9-201">**Identitet providern EntityDescriptor enhets-ID:** klistra in den **SAML enhets-ID** kopieras värdet från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="579f9-201">**Identity Provider EntityDescriptor Entity ID:** Paste the **SAML Entity ID** value copied from the Azure portal.</span></span>

    <span data-ttu-id="579f9-202">e.</span><span class="sxs-lookup"><span data-stu-id="579f9-202">e.</span></span> <span data-ttu-id="579f9-203">**Identitet providern SingleSignOnService URL:** klistra in den **SAML inloggning tjänst-URL för enkel** från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="579f9-203">**Identity Provider SingleSignOnService URL:** Paste the **SAML Single Sign-On Service URL** from the Azure portal.</span></span>

    <span data-ttu-id="579f9-204">f.</span><span class="sxs-lookup"><span data-stu-id="579f9-204">f.</span></span> <span data-ttu-id="579f9-205">**Providern offentliga X509 identitetscertifikat:** öppna X509 certifikat hämtas från Azure i anteckningar och klistra in innehållet i den här rutan.</span><span class="sxs-lookup"><span data-stu-id="579f9-205">**Identity Provider public X509 certificate:** Open the X509 certificate downloaded from the Azure in notepad and paste the contents in this box.</span></span> <span data-ttu-id="579f9-206">Se till att det finns inga radbrytningar i mitten av certifikat-innehållet.</span><span class="sxs-lookup"><span data-stu-id="579f9-206">Ensure there are no line breaks in the middle of the certificate contents.</span></span>
    
    <span data-ttu-id="579f9-207">g.</span><span class="sxs-lookup"><span data-stu-id="579f9-207">g.</span></span> <span data-ttu-id="579f9-208">Markera kryssrutorna för följande: **aktiverad, kryptera NameID och logga AuthnRequests.**</span><span class="sxs-lookup"><span data-stu-id="579f9-208">Check the following checkboxes: **Enabled, Encrypt NameID and Sign AuthnRequests.**</span></span>

    <span data-ttu-id="579f9-209">h.</span><span class="sxs-lookup"><span data-stu-id="579f9-209">h.</span></span> <span data-ttu-id="579f9-210">Klicka på **SSO uppdateringsinställningar** spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="579f9-210">Click **Update SSO Settings** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="579f9-211">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="579f9-211">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="579f9-212">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="579f9-212">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="579f9-213">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="579f9-213">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="579f9-214">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="579f9-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="579f9-215">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="579f9-215">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="579f9-217">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="579f9-217">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="579f9-218">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="579f9-218">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="579f9-220">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="579f9-220">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="579f9-222">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="579f9-222">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="579f9-224">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="579f9-224">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="579f9-226">a.</span><span class="sxs-lookup"><span data-stu-id="579f9-226">a.</span></span> <span data-ttu-id="579f9-227">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="579f9-227">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="579f9-228">b.</span><span class="sxs-lookup"><span data-stu-id="579f9-228">b.</span></span> <span data-ttu-id="579f9-229">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="579f9-229">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="579f9-230">c.</span><span class="sxs-lookup"><span data-stu-id="579f9-230">c.</span></span> <span data-ttu-id="579f9-231">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="579f9-231">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="579f9-232">d.</span><span class="sxs-lookup"><span data-stu-id="579f9-232">d.</span></span> <span data-ttu-id="579f9-233">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="579f9-233">Click **Create**.</span></span>
 
### <a name="creating-a-scalex-enterprise-test-user"></a><span data-ttu-id="579f9-234">Skapa en testanvändare ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="579f9-234">Creating a ScaleX Enterprise test user</span></span>

<span data-ttu-id="579f9-235">Om du vill aktivera Azure AD-användare kan logga in på ScaleX Enterprise, måste de vara etablerade i till ScaleX företaget.</span><span class="sxs-lookup"><span data-stu-id="579f9-235">To enable Azure AD users to log in to ScaleX Enterprise, they must be provisioned in to ScaleX Enterprise.</span></span> <span data-ttu-id="579f9-236">Etablering är en automatisk uppgift vid ScaleX Enterprise, och inga manuella steg krävs.</span><span class="sxs-lookup"><span data-stu-id="579f9-236">In the case of ScaleX Enterprise, provisioning is an automatic task and no manual steps are required.</span></span> <span data-ttu-id="579f9-237">Alla användare som kan autentisera med autentiseringsuppgifter för enkel inloggning ska etableras automatiskt på ScaleX sida.</span><span class="sxs-lookup"><span data-stu-id="579f9-237">Any user who can successfully authenticate with SSO credentials will be automatically provisioned on the ScaleX side.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="579f9-238">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="579f9-238">Assigning the Azure AD test user</span></span>

<span data-ttu-id="579f9-239">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja användaråtkomst till ScaleX Enterprise.</span><span class="sxs-lookup"><span data-stu-id="579f9-239">In this section, you enable Britta Simon to use Azure single sign-on by granting user access to ScaleX Enterprise.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="579f9-241">**Om du vill tilldela ScaleX Enterprise Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="579f9-241">**To assign Britta Simon to ScaleX Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="579f9-242">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="579f9-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="579f9-244">Välj i listan med program **ScaleX Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="579f9-244">In the applications list, select **ScaleX Enterprise**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. <span data-ttu-id="579f9-246">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="579f9-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="579f9-248">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="579f9-248">Click **Add** button.</span></span> <span data-ttu-id="579f9-249">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="579f9-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="579f9-251">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="579f9-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="579f9-252">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="579f9-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="579f9-253">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="579f9-253">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="579f9-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="579f9-254">Testing single sign-on</span></span>

<span data-ttu-id="579f9-255">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="579f9-255">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="579f9-256">Klicka på panelen ScaleX Enterprise på åtkomstpanelen, du ska hämta automatiskt loggat in på ditt ScaleX företagsprogram.</span><span class="sxs-lookup"><span data-stu-id="579f9-256">Click the ScaleX Enterprise tile in the Access Panel, you will get automatically signed-on to your ScaleX Enterprise application.</span></span> <span data-ttu-id="579f9-257">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="579f9-257">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="579f9-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="579f9-258">Additional resources</span></span>

* [<span data-ttu-id="579f9-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="579f9-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="579f9-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="579f9-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

