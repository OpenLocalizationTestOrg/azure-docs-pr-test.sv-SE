---
title: "Självstudier: Azure Active Directory-integrering med Litmos | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Litmos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ef1b5860ba0a406022bbd11afb366d14bee2c57d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a><span data-ttu-id="e7f19-103">Självstudier: Azure Active Directory-integrering med Litmos</span><span class="sxs-lookup"><span data-stu-id="e7f19-103">Tutorial: Azure Active Directory integration with Litmos</span></span>

<span data-ttu-id="e7f19-104">I kursen får lära du att integrera Litmos med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e7f19-104">In this tutorial, you learn how to integrate Litmos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e7f19-105">Integrera Litmos med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e7f19-105">Integrating Litmos with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e7f19-106">Du kan styra i Azure AD som har åtkomst till Litmos.</span><span class="sxs-lookup"><span data-stu-id="e7f19-106">You can control in Azure AD who has access to Litmos.</span></span>
- <span data-ttu-id="e7f19-107">Du kan aktivera användarna att automatiskt hämta loggat in på Litmos (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="e7f19-107">You can enable your users to automatically get signed-on to Litmos (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e7f19-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e7f19-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="e7f19-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e7f19-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7f19-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e7f19-110">Prerequisites</span></span>

<span data-ttu-id="e7f19-111">För att konfigurera Azure AD-integrering med Litmos, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e7f19-111">To configure Azure AD integration with Litmos, you need the following items:</span></span>

- <span data-ttu-id="e7f19-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e7f19-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e7f19-113">En Litmos enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e7f19-113">A Litmos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e7f19-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e7f19-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e7f19-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e7f19-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e7f19-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e7f19-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e7f19-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e7f19-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e7f19-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e7f19-118">Scenario description</span></span>
<span data-ttu-id="e7f19-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e7f19-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e7f19-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e7f19-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e7f19-121">Att lägga till Litmos från galleriet</span><span class="sxs-lookup"><span data-stu-id="e7f19-121">Adding Litmos from the gallery</span></span>
2. <span data-ttu-id="e7f19-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e7f19-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-litmos-from-the-gallery"></a><span data-ttu-id="e7f19-123">Att lägga till Litmos från galleriet</span><span class="sxs-lookup"><span data-stu-id="e7f19-123">Adding Litmos from the gallery</span></span>
<span data-ttu-id="e7f19-124">Du måste lägga till Litmos från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Litmos i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7f19-124">To configure the integration of Litmos into Azure AD, you need to add Litmos from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e7f19-125">**Utför följande steg för att lägga till Litmos från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e7f19-125">**To add Litmos from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e7f19-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e7f19-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="e7f19-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e7f19-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="e7f19-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7f19-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="e7f19-133">I sökrutan skriver **Litmos**väljer **Litmos** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e7f19-133">In the search box, type **Litmos**, select **Litmos** from result panel then click **Add** button to add the application.</span></span>

    ![Litmos i resultatlistan](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e7f19-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e7f19-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e7f19-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Litmos baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e7f19-136">In this section, you configure and test Azure AD single sign-on with Litmos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e7f19-137">Azure AD måste du känna till användaren i Litmos motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e7f19-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Litmos is to a user in Azure AD.</span></span> <span data-ttu-id="e7f19-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Litmos upprättas.</span><span class="sxs-lookup"><span data-stu-id="e7f19-138">In other words, a link relationship between an Azure AD user and the related user in Litmos needs to be established.</span></span>

<span data-ttu-id="e7f19-139">I Litmos, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e7f19-139">In Litmos, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e7f19-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Litmos, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e7f19-140">To configure and test Azure AD single sign-on with Litmos, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e7f19-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e7f19-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e7f19-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e7f19-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e7f19-143">**[Skapa en testanvändare Litmos](#create-a-litmos-test-user)**  – du har en motsvarighet för Britta Simon i Litmos som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e7f19-143">**[Create a Litmos test user](#create-a-litmos-test-user)** - to have a counterpart of Britta Simon in Litmos that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e7f19-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e7f19-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e7f19-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e7f19-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e7f19-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e7f19-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e7f19-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Litmos program.</span><span class="sxs-lookup"><span data-stu-id="e7f19-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Litmos application.</span></span>

<span data-ttu-id="e7f19-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Litmos:**</span><span class="sxs-lookup"><span data-stu-id="e7f19-148">**To configure Azure AD single sign-on with Litmos, perform the following steps:**</span></span>

1. <span data-ttu-id="e7f19-149">I Azure-portalen på den **Litmos** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-149">In the Azure portal, on the **Litmos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="e7f19-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e7f19-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_samlbase.png)

3. <span data-ttu-id="e7f19-153">På den **Litmos domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e7f19-153">On the **Litmos Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Litmos domän med enkel inloggning information](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_url.png)

    <span data-ttu-id="e7f19-155">a.</span><span class="sxs-lookup"><span data-stu-id="e7f19-155">a.</span></span> <span data-ttu-id="e7f19-156">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.litmos.com/account/Login`</span><span class="sxs-lookup"><span data-stu-id="e7f19-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.litmos.com/account/Login`</span></span>

    <span data-ttu-id="e7f19-157">b.</span><span class="sxs-lookup"><span data-stu-id="e7f19-157">b.</span></span> <span data-ttu-id="e7f19-158">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<companyname>.litmos.com/integration/samllogin`</span><span class="sxs-lookup"><span data-stu-id="e7f19-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.litmos.com/integration/samllogin`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e7f19-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e7f19-159">These values are not real.</span></span> <span data-ttu-id="e7f19-160">Uppdatera dessa värden med den faktiska identifierare och Reply-URL som beskrivs senare i självstudiekursen eller kontakta [Litmos supportteam](https://www.litmos.com/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e7f19-160">Update these values with the actual Identifier and Reply URL, which are explained later in tutorial or contact [Litmos support team](https://www.litmos.com/contact-us/) to get these values.</span></span>

4. <span data-ttu-id="e7f19-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e7f19-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_certificate.png)

5. <span data-ttu-id="e7f19-163">Som en del av konfigurationen, måste du anpassa den **SAML-Token attribut** för tillämpningsprogrammet Litmos.</span><span class="sxs-lookup"><span data-stu-id="e7f19-163">As part of the configuration, you need to customize the **SAML Token Attributes** for your Litmos application.</span></span>

    ![Attributet avsnitt](./media/active-directory-saas-litmos-tutorial/tutorial_attribute.png)
           
    | <span data-ttu-id="e7f19-165">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="e7f19-165">Attribute Name</span></span>   | <span data-ttu-id="e7f19-166">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="e7f19-166">Attribute Value</span></span> |   
    | ---------------  | ----------------|
    | <span data-ttu-id="e7f19-167">Förnamn</span><span class="sxs-lookup"><span data-stu-id="e7f19-167">FirstName</span></span> |<span data-ttu-id="e7f19-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="e7f19-168">user.givenname</span></span> |
    | <span data-ttu-id="e7f19-169">Efternamn</span><span class="sxs-lookup"><span data-stu-id="e7f19-169">LastName</span></span>  |<span data-ttu-id="e7f19-170">User.surname</span><span class="sxs-lookup"><span data-stu-id="e7f19-170">user.surname</span></span> |
    | <span data-ttu-id="e7f19-171">E-post</span><span class="sxs-lookup"><span data-stu-id="e7f19-171">Email</span></span> |<span data-ttu-id="e7f19-172">User.Mail</span><span class="sxs-lookup"><span data-stu-id="e7f19-172">user.mail</span></span> |

    <span data-ttu-id="e7f19-173">a.</span><span class="sxs-lookup"><span data-stu-id="e7f19-173">a.</span></span> <span data-ttu-id="e7f19-174">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7f19-174">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Lägg till attribut](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_04.png)

    ![Lägg till attribut Dailog](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="e7f19-177">b.</span><span class="sxs-lookup"><span data-stu-id="e7f19-177">b.</span></span> <span data-ttu-id="e7f19-178">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="e7f19-178">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="e7f19-179">c.</span><span class="sxs-lookup"><span data-stu-id="e7f19-179">c.</span></span> <span data-ttu-id="e7f19-180">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="e7f19-180">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="e7f19-181">d.</span><span class="sxs-lookup"><span data-stu-id="e7f19-181">d.</span></span> <span data-ttu-id="e7f19-182">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-182">Click **Ok**.</span></span>     

6. <span data-ttu-id="e7f19-183">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e7f19-183">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-litmos-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="e7f19-185">I ett annat webbläsarfönster inloggning till webbplatsen Litmos företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="e7f19-185">In a different browser window, sign-on to your Litmos company site as administrator.</span></span>

8. <span data-ttu-id="e7f19-186">I navigeringsfältet till vänster klickar du på **konton**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-186">In the navigation bar on the left side, click **Accounts**.</span></span>
   
    ![Kontoavsnittet på App-sida][22] 

9. <span data-ttu-id="e7f19-188">Klicka på den **integreringar** fliken.</span><span class="sxs-lookup"><span data-stu-id="e7f19-188">Click the **Integrations** tab.</span></span>
   
    ![Fliken Integration][23] 

10. <span data-ttu-id="e7f19-190">På den **integreringar** , bläddrar du ned till **3 part integreringar**, och klicka sedan på **SAML 2.0** fliken.</span><span class="sxs-lookup"><span data-stu-id="e7f19-190">On the **Integrations** tab, scroll down to **3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0 avsnitt][24] 

11. <span data-ttu-id="e7f19-192">Kopiera värdet under **i SAML-slutpunkten för litmos är:** och klistrar in det i den **Reply URL** TextBox-kontroll i den **Litmos domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e7f19-192">Copy the value under **The SAML endpoint for litmos is:** and paste it into the **Reply URL** textbox in the **Litmos Domain and URLs** section in Azure portal.</span></span> 
   
    ![SAML-slutpunkt][26] 

12. <span data-ttu-id="e7f19-194">I din **Litmos** program, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e7f19-194">In your **Litmos** application, perform the following steps:</span></span>
    
     ![Litmos program][25] 
     
     <span data-ttu-id="e7f19-196">a.</span><span class="sxs-lookup"><span data-stu-id="e7f19-196">a.</span></span> <span data-ttu-id="e7f19-197">Klicka på **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-197">Click **Enable SAML**.</span></span>
    
     <span data-ttu-id="e7f19-198">b.</span><span class="sxs-lookup"><span data-stu-id="e7f19-198">b.</span></span> <span data-ttu-id="e7f19-199">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **SAML X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="e7f19-199">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **SAML X.509 Certificate** textbox.</span></span>
     
     <span data-ttu-id="e7f19-200">c.</span><span class="sxs-lookup"><span data-stu-id="e7f19-200">c.</span></span> <span data-ttu-id="e7f19-201">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-201">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="e7f19-202">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e7f19-202">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e7f19-203">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e7f19-203">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e7f19-204">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e7f19-204">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e7f19-205">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e7f19-205">Create an Azure AD test user</span></span>

<span data-ttu-id="e7f19-206">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e7f19-206">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="e7f19-208">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e7f19-208">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e7f19-209">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="e7f19-209">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-litmos-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e7f19-211">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-211">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-litmos-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e7f19-213">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7f19-213">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e7f19-215">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e7f19-215">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e7f19-217">a.</span><span class="sxs-lookup"><span data-stu-id="e7f19-217">a.</span></span> <span data-ttu-id="e7f19-218">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-218">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e7f19-219">b.</span><span class="sxs-lookup"><span data-stu-id="e7f19-219">b.</span></span> <span data-ttu-id="e7f19-220">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="e7f19-220">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="e7f19-221">c.</span><span class="sxs-lookup"><span data-stu-id="e7f19-221">c.</span></span> <span data-ttu-id="e7f19-222">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="e7f19-222">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="e7f19-223">d.</span><span class="sxs-lookup"><span data-stu-id="e7f19-223">d.</span></span> <span data-ttu-id="e7f19-224">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-224">Click **Create**.</span></span>
  
### <a name="create-a-litmos-test-user"></a><span data-ttu-id="e7f19-225">Skapa en testanvändare Litmos</span><span class="sxs-lookup"><span data-stu-id="e7f19-225">Create a Litmos test user</span></span>

<span data-ttu-id="e7f19-226">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Litmos.</span><span class="sxs-lookup"><span data-stu-id="e7f19-226">The objective of this section is to create a user called Britta Simon in Litmos.</span></span>  
<span data-ttu-id="e7f19-227">Litmos programmet stöder Just-in-Time-etablering.</span><span class="sxs-lookup"><span data-stu-id="e7f19-227">The Litmos application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="e7f19-228">Det innebär ett konto skapas automatiskt om det behövs vid ett försök att komma åt programmet med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e7f19-228">This means, a user account is automatically created if necessary during an attempt to access the application using the Access Panel.</span></span>

<span data-ttu-id="e7f19-229">**Utför följande steg för att skapa en användare som kallas Britta Simon i Litmos:**</span><span class="sxs-lookup"><span data-stu-id="e7f19-229">**To create a user called Britta Simon in Litmos, perform the following steps:**</span></span>

1. <span data-ttu-id="e7f19-230">I ett annat webbläsarfönster inloggning till webbplatsen Litmos företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="e7f19-230">In a different browser window, sign-on to your Litmos company site as administrator.</span></span>

2. <span data-ttu-id="e7f19-231">I navigeringsfältet till vänster klickar du på **konton**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-231">In the navigation bar on the left side, click **Accounts**.</span></span>
   
    ![Kontoavsnittet på App-sida][22] 

3. <span data-ttu-id="e7f19-233">Klicka på den **integreringar** fliken.</span><span class="sxs-lookup"><span data-stu-id="e7f19-233">Click the **Integrations** tab.</span></span>
   
    ![Fliken integreringar][23] 

4. <span data-ttu-id="e7f19-235">På den **integreringar** , bläddrar du ned till **3 part integreringar**, och klicka sedan på **SAML 2.0** fliken.</span><span class="sxs-lookup"><span data-stu-id="e7f19-235">On the **Integrations** tab, scroll down to **3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0][24] 
    
5. <span data-ttu-id="e7f19-237">Välj **Autogenerate användare**</span><span class="sxs-lookup"><span data-stu-id="e7f19-237">Select **Autogenerate Users**</span></span>
   
    ![Automatiskt genererat användare][27] 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="e7f19-239">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e7f19-239">Assign the Azure AD test user</span></span>

<span data-ttu-id="e7f19-240">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Litmos.</span><span class="sxs-lookup"><span data-stu-id="e7f19-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Litmos.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="e7f19-242">**Om du vill tilldela Litmos Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e7f19-242">**To assign Britta Simon to Litmos, perform the following steps:**</span></span>

1. <span data-ttu-id="e7f19-243">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e7f19-245">Välj i listan med program **Litmos**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-245">In the applications list, select **Litmos**.</span></span>

    ![Länken Litmos i listan med program](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_app.png)  

3. <span data-ttu-id="e7f19-247">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e7f19-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="e7f19-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e7f19-249">Click **Add** button.</span></span> <span data-ttu-id="e7f19-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7f19-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="e7f19-252">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e7f19-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e7f19-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7f19-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e7f19-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7f19-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e7f19-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e7f19-255">Test single sign-on</span></span>

<span data-ttu-id="e7f19-256">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e7f19-256">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="e7f19-257">När du klickar på panelen Litmos på åtkomstpanelen du bör få automatiskt loggat in på ditt Litmos program.</span><span class="sxs-lookup"><span data-stu-id="e7f19-257">When you click the Litmos tile in the Access Panel, you should get automatically signed-on to your Litmos application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e7f19-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e7f19-258">Additional resources</span></span>

* [<span data-ttu-id="e7f19-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e7f19-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e7f19-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e7f19-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png

