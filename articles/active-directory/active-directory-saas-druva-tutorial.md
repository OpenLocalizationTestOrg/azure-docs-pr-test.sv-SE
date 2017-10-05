---
title: "Självstudier: Azure Active Directory-integrering med Druva | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Druva."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: b23e73c47b9a00893e036b67826e4b7ead819a1d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a><span data-ttu-id="a352a-103">Självstudier: Azure Active Directory-integrering med Druva</span><span class="sxs-lookup"><span data-stu-id="a352a-103">Tutorial: Azure Active Directory integration with Druva</span></span>

<span data-ttu-id="a352a-104">I kursen får lära du att integrera Druva med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a352a-104">In this tutorial, you learn how to integrate Druva with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a352a-105">Integrera Druva med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a352a-105">Integrating Druva with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a352a-106">Du kan styra i Azure AD som har åtkomst till Druva.</span><span class="sxs-lookup"><span data-stu-id="a352a-106">You can control in Azure AD who has access to Druva.</span></span>
- <span data-ttu-id="a352a-107">Du kan aktivera användarna att automatiskt hämta loggat in på Druva (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="a352a-107">You can enable your users to automatically get signed-on to Druva (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="a352a-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a352a-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="a352a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a352a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a352a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a352a-110">Prerequisites</span></span>

<span data-ttu-id="a352a-111">För att konfigurera Azure AD-integrering med Druva, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a352a-111">To configure Azure AD integration with Druva, you need the following items:</span></span>

- <span data-ttu-id="a352a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a352a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a352a-113">En Druva enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a352a-113">A Druva single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a352a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a352a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a352a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a352a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a352a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a352a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a352a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a352a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a352a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a352a-118">Scenario description</span></span>
<span data-ttu-id="a352a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a352a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a352a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a352a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a352a-121">Att lägga till Druva från galleriet</span><span class="sxs-lookup"><span data-stu-id="a352a-121">Adding Druva from the gallery</span></span>
2. <span data-ttu-id="a352a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a352a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-druva-from-the-gallery"></a><span data-ttu-id="a352a-123">Att lägga till Druva från galleriet</span><span class="sxs-lookup"><span data-stu-id="a352a-123">Adding Druva from the gallery</span></span>
<span data-ttu-id="a352a-124">Du måste lägga till Druva från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Druva i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a352a-124">To configure the integration of Druva into Azure AD, you need to add Druva from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a352a-125">**Utför följande steg för att lägga till Druva från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a352a-125">**To add Druva from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a352a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a352a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="a352a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a352a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a352a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a352a-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="a352a-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a352a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="a352a-133">I sökrutan skriver **Druva**väljer **Druva** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a352a-133">In the search box, type **Druva**, select **Druva** from result panel then click **Add** button to add the application.</span></span>

    ![Druva i resultatlistan](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a352a-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a352a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a352a-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Druva baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a352a-136">In this section, you configure and test Azure AD single sign-on with Druva based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a352a-137">Azure AD måste du känna till användaren i Druva motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a352a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Druva is to a user in Azure AD.</span></span> <span data-ttu-id="a352a-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Druva upprättas.</span><span class="sxs-lookup"><span data-stu-id="a352a-138">In other words, a link relationship between an Azure AD user and the related user in Druva needs to be established.</span></span>

<span data-ttu-id="a352a-139">I Druva, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a352a-139">In Druva, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a352a-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Druva, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a352a-140">To configure and test Azure AD single sign-on with Druva, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a352a-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a352a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a352a-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a352a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a352a-143">**[Skapa en testanvändare Druva](#create-a-druva-test-user)**  – du har en motsvarighet för Britta Simon i Druva som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a352a-143">**[Create a Druva test user](#create-a-druva-test-user)** - to have a counterpart of Britta Simon in Druva that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a352a-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a352a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a352a-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a352a-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a352a-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a352a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a352a-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Druva program.</span><span class="sxs-lookup"><span data-stu-id="a352a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Druva application.</span></span>

<span data-ttu-id="a352a-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Druva:**</span><span class="sxs-lookup"><span data-stu-id="a352a-148">**To configure Azure AD single sign-on with Druva, perform the following steps:**</span></span>

1. <span data-ttu-id="a352a-149">I Azure-portalen på den **Druva** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a352a-149">In the Azure portal, on the **Druva** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="a352a-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a352a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. <span data-ttu-id="a352a-153">På den **Druva domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a352a-153">On the **Druva Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    <span data-ttu-id="a352a-155">I den **inloggnings-URL** textruta anger du URL:`https://cloud.druva.com/home`</span><span class="sxs-lookup"><span data-stu-id="a352a-155">In the **Sign-on URL** textbox, type the URL: `https://cloud.druva.com/home`</span></span>

4. <span data-ttu-id="a352a-156">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a352a-156">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. <span data-ttu-id="a352a-158">Tillämpningsprogrammet Druva förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till attributmappningar till din **SAML-Token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a352a-158">Your Druva application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. <span data-ttu-id="a352a-160">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i den föregående bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a352a-160">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>

    | <span data-ttu-id="a352a-161">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="a352a-161">Attribute Name</span></span>      | <span data-ttu-id="a352a-162">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="a352a-162">Attribute Value</span></span>      |
    | ------------------- | -------------------- |
    | <span data-ttu-id="a352a-163">synkroniserad\_auth\_token</span><span class="sxs-lookup"><span data-stu-id="a352a-163">insync\_auth\_token</span></span> |<span data-ttu-id="a352a-164">Ange token genererade värde</span><span class="sxs-lookup"><span data-stu-id="a352a-164">Enter the token generated value</span></span> |
    
    <span data-ttu-id="a352a-165">a.</span><span class="sxs-lookup"><span data-stu-id="a352a-165">a.</span></span> <span data-ttu-id="a352a-166">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a352a-166">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="a352a-169">b.</span><span class="sxs-lookup"><span data-stu-id="a352a-169">b.</span></span> <span data-ttu-id="a352a-170">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="a352a-170">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="a352a-171">c.</span><span class="sxs-lookup"><span data-stu-id="a352a-171">c.</span></span> <span data-ttu-id="a352a-172">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="a352a-172">From the **Value** list, type the attribute value shown for that row.</span></span> <span data-ttu-id="a352a-173">Token genererat värde beskrivs senare i självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a352a-173">The token generated value is explained later in tutorial.</span></span>
    
    <span data-ttu-id="a352a-174">d.</span><span class="sxs-lookup"><span data-stu-id="a352a-174">d.</span></span> <span data-ttu-id="a352a-175">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a352a-175">Click **Ok**.</span></span>    

7. <span data-ttu-id="a352a-176">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a352a-176">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="a352a-178">På den **Druva Configuration** klickar du på **konfigurera Druva** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a352a-178">On the **Druva Configuration** section, click **Configure Druva** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a352a-179">Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a352a-179">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. <span data-ttu-id="a352a-181">I en annan webbläsarfönster loggar du in på webbplatsen Druva företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="a352a-181">In a different web browser window, log in to your Druva company site as an administrator.</span></span>

10. <span data-ttu-id="a352a-182">Gå till **hantera \> inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a352a-182">Go to **Manage \> Settings**.</span></span>

    <span data-ttu-id="a352a-183">![Inställningar för](./media/active-directory-saas-druva-tutorial/ic795091.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="a352a-183">![Settings](./media/active-directory-saas-druva-tutorial/ic795091.png "Settings")</span></span>

11. <span data-ttu-id="a352a-184">I dialogrutan Inställningar för enkel inloggning utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="a352a-184">On the Single Sign-On Settings dialog, perform the following steps:</span></span>

    <span data-ttu-id="a352a-185">![Enkel inloggning inställningar](./media/active-directory-saas-druva-tutorial/ic795092.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="a352a-185">![Single Sign-On Settings](./media/active-directory-saas-druva-tutorial/ic795092.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="a352a-186">a.</span><span class="sxs-lookup"><span data-stu-id="a352a-186">a.</span></span> <span data-ttu-id="a352a-187">Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från Azure-portalen i den **ID providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a352a-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **ID Provider Login URL** textbox.</span></span>
    
    <span data-ttu-id="a352a-188">b.</span><span class="sxs-lookup"><span data-stu-id="a352a-188">b.</span></span> <span data-ttu-id="a352a-189">Klistra in **Sign-Out URL** -värde som du har kopierat från Azure-portalen i den **ID providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a352a-189">Paste **Sign-Out URL** value, which you have copied from the Azure portal into the **ID Provider Logout URL** textbox.</span></span>
    
     <span data-ttu-id="a352a-190">c.</span><span class="sxs-lookup"><span data-stu-id="a352a-190">c.</span></span> <span data-ttu-id="a352a-191">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **ID providern certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="a352a-191">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **ID Provider Certificate** textbox</span></span>
     
     <span data-ttu-id="a352a-192">d.</span><span class="sxs-lookup"><span data-stu-id="a352a-192">d.</span></span> <span data-ttu-id="a352a-193">Öppna den **inställningar** klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="a352a-193">To open the **Settings** page, click **Save**.</span></span>

12. <span data-ttu-id="a352a-194">På den **inställningar** klickar du på **generera SSO Token**.</span><span class="sxs-lookup"><span data-stu-id="a352a-194">On the **Settings** page, click **Generate SSO Token**.</span></span>

    <span data-ttu-id="a352a-195">![Inställningar för](./media/active-directory-saas-druva-tutorial/ic795093.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="a352a-195">![Settings](./media/active-directory-saas-druva-tutorial/ic795093.png "Settings")</span></span>

13. <span data-ttu-id="a352a-196">På den **enkel inloggning autentiseringstoken** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a352a-196">On the **Single Sign-on Authentication Token** dialog, perform the following steps:</span></span>

    <span data-ttu-id="a352a-197">![SSO Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO-Token")</span><span class="sxs-lookup"><span data-stu-id="a352a-197">![SSO Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO Token")</span></span>
    
    <span data-ttu-id="a352a-198">a.</span><span class="sxs-lookup"><span data-stu-id="a352a-198">a.</span></span> <span data-ttu-id="a352a-199">Klicka på **kopia**, klistra in kopieras värdet i den **värdet** TextBox-kontroll i den **lägga till attributet** avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a352a-199">Click **Copy**, Paste copied value in the **Value** textbox in the **Add Attribute** section.</span></span>
    
    <span data-ttu-id="a352a-200">b.</span><span class="sxs-lookup"><span data-stu-id="a352a-200">b.</span></span> <span data-ttu-id="a352a-201">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="a352a-201">Click **Close**.</span></span>

> [!TIP]
> <span data-ttu-id="a352a-202">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="a352a-202">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a352a-203">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="a352a-203">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a352a-204">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a352a-204">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a352a-205">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a352a-205">Create an Azure AD test user</span></span>

<span data-ttu-id="a352a-206">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a352a-206">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="a352a-208">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a352a-208">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a352a-209">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="a352a-209">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="a352a-211">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a352a-211">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="a352a-213">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a352a-213">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="a352a-215">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a352a-215">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    <span data-ttu-id="a352a-217">a.</span><span class="sxs-lookup"><span data-stu-id="a352a-217">a.</span></span> <span data-ttu-id="a352a-218">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a352a-218">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a352a-219">b.</span><span class="sxs-lookup"><span data-stu-id="a352a-219">b.</span></span> <span data-ttu-id="a352a-220">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a352a-220">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="a352a-221">c.</span><span class="sxs-lookup"><span data-stu-id="a352a-221">c.</span></span> <span data-ttu-id="a352a-222">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="a352a-222">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="a352a-223">d.</span><span class="sxs-lookup"><span data-stu-id="a352a-223">d.</span></span> <span data-ttu-id="a352a-224">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a352a-224">Click **Create**.</span></span>
 
### <a name="create-a-druva-test-user"></a><span data-ttu-id="a352a-225">Skapa en testanvändare Druva</span><span class="sxs-lookup"><span data-stu-id="a352a-225">Create a Druva test user</span></span>

<span data-ttu-id="a352a-226">För att aktivera Azure AD-användare kan logga in på Druva etableras de i Druva.</span><span class="sxs-lookup"><span data-stu-id="a352a-226">In order to enable Azure AD users to log in to Druva, they must be provisioned into Druva.</span></span> <span data-ttu-id="a352a-227">När det gäller Druva är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a352a-227">In the case of Druva, provisioning is a manual task.</span></span>

<span data-ttu-id="a352a-228">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="a352a-228">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="a352a-229">Logga in på ditt **Druva** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="a352a-229">Log in to your **Druva** company site as administrator.</span></span>

2. <span data-ttu-id="a352a-230">Gå till **hantera \> användare**.</span><span class="sxs-lookup"><span data-stu-id="a352a-230">Go to **Manage \> Users**.</span></span>
   
   <span data-ttu-id="a352a-231">![Hantera användare](./media/active-directory-saas-druva-tutorial/ic795097.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="a352a-231">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795097.png "Manage Users")</span></span>

3. <span data-ttu-id="a352a-232">Klicka på **skapa nya**.</span><span class="sxs-lookup"><span data-stu-id="a352a-232">Click **Create New**.</span></span>
   
   <span data-ttu-id="a352a-233">![Hantera användare](./media/active-directory-saas-druva-tutorial/ic795098.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="a352a-233">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795098.png "Manage Users")</span></span>

4. <span data-ttu-id="a352a-234">I dialogrutan Skapa ny användare utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="a352a-234">On the Create New User dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="a352a-235">![Skapa ny användare](./media/active-directory-saas-druva-tutorial/ic795099.png "Skapa ny användare")</span><span class="sxs-lookup"><span data-stu-id="a352a-235">![Create NewUser](./media/active-directory-saas-druva-tutorial/ic795099.png "Create NewUser")</span></span>
   
   <span data-ttu-id="a352a-236">a.</span><span class="sxs-lookup"><span data-stu-id="a352a-236">a.</span></span> <span data-ttu-id="a352a-237">I den **e-postadress** textruta ange e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a352a-237">In the **Email address** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="a352a-238">b.</span><span class="sxs-lookup"><span data-stu-id="a352a-238">b.</span></span> <span data-ttu-id="a352a-239">I den **namn** textruta anger du namnet på användaren som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a352a-239">In the **Name** textbox, enter the name of user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="a352a-240">c.</span><span class="sxs-lookup"><span data-stu-id="a352a-240">c.</span></span> <span data-ttu-id="a352a-241">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="a352a-241">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="a352a-242">Du kan använda något annat Druva användarens konto skapas verktyg eller API: er som tillhandahålls av Druva att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="a352a-242">You can use any other Druva user account creation tools or APIs provided by Druva to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="a352a-243">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a352a-243">Assign the Azure AD test user</span></span>

<span data-ttu-id="a352a-244">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Druva.</span><span class="sxs-lookup"><span data-stu-id="a352a-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Druva.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="a352a-246">**Om du vill tilldela Druva Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a352a-246">**To assign Britta Simon to Druva, perform the following steps:**</span></span>

1. <span data-ttu-id="a352a-247">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a352a-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a352a-249">Välj i listan med program **Druva**.</span><span class="sxs-lookup"><span data-stu-id="a352a-249">In the applications list, select **Druva**.</span></span>

    ![Länken Druva i listan med program](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. <span data-ttu-id="a352a-251">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a352a-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="a352a-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a352a-253">Click **Add** button.</span></span> <span data-ttu-id="a352a-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a352a-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="a352a-256">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a352a-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a352a-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a352a-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a352a-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a352a-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a352a-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a352a-259">Test single sign-on</span></span>

<span data-ttu-id="a352a-260">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a352a-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a352a-261">När du klickar på panelen Druva på åtkomstpanelen du bör få automatiskt loggat in på ditt Druva program.</span><span class="sxs-lookup"><span data-stu-id="a352a-261">When you click the Druva tile in the Access Panel, you should get automatically signed-on to your Druva application.</span></span>
<span data-ttu-id="a352a-262">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a352a-262">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a352a-263">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a352a-263">Additional resources</span></span>

* [<span data-ttu-id="a352a-264">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a352a-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a352a-265">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a352a-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

