---
title: "Självstudier: Azure Active Directory-integrering med Learningpool Act | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Learningpool Act."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 51e8695f-31e1-4d09-8eb3-13241999d99f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 932f5f12c75299e532d3fa2c31f1805a7df30158
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learningpool-act"></a><span data-ttu-id="08175-103">Självstudier: Azure Active Directory-integrering med Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="08175-103">Tutorial: Azure Active Directory integration with Learningpool Act</span></span>

<span data-ttu-id="08175-104">I kursen får lära du att integrera Learningpool Act med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="08175-104">In this tutorial, you learn how to integrate Learningpool Act with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="08175-105">Integrera Learningpool Act med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="08175-105">Integrating Learningpool Act with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="08175-106">Du kan styra i Azure AD som har åtkomst till Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="08175-106">You can control in Azure AD who has access to Learningpool Act</span></span>
- <span data-ttu-id="08175-107">Du kan aktivera användarna att automatiskt hämta loggat in på Learningpool Act (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="08175-107">You can enable your users to automatically get signed-on to Learningpool Act (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="08175-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="08175-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="08175-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="08175-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08175-110">Krav</span><span class="sxs-lookup"><span data-stu-id="08175-110">Prerequisites</span></span>

<span data-ttu-id="08175-111">För att konfigurera Azure AD-integrering med Learningpool Act, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="08175-111">To configure Azure AD integration with Learningpool Act, you need the following items:</span></span>

- <span data-ttu-id="08175-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="08175-112">An Azure AD subscription</span></span>
- <span data-ttu-id="08175-113">En Learningpool Act enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="08175-113">A Learningpool Act single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="08175-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="08175-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="08175-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="08175-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="08175-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="08175-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="08175-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="08175-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="08175-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="08175-118">Scenario description</span></span>
<span data-ttu-id="08175-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="08175-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="08175-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="08175-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="08175-121">Att lägga till Learningpool Act från galleriet</span><span class="sxs-lookup"><span data-stu-id="08175-121">Adding Learningpool Act from the gallery</span></span>
2. <span data-ttu-id="08175-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08175-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learningpool-act-from-the-gallery"></a><span data-ttu-id="08175-123">Att lägga till Learningpool Act från galleriet</span><span class="sxs-lookup"><span data-stu-id="08175-123">Adding Learningpool Act from the gallery</span></span>
<span data-ttu-id="08175-124">Du måste lägga till Learningpool Act från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Learningpool Act i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08175-124">To configure the integration of Learningpool Act into Azure AD, you need to add Learningpool Act from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="08175-125">**Utför följande steg för att lägga till Learningpool Act från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="08175-125">**To add Learningpool Act from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="08175-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="08175-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="08175-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="08175-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="08175-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="08175-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="08175-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08175-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="08175-133">I sökrutan skriver **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="08175-133">In the search box, type **Learningpool Act**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_search.png)

5. <span data-ttu-id="08175-135">Välj i resultatpanelen **Learningpool Act**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="08175-135">In the results panel, select **Learningpool Act**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="08175-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08175-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="08175-138">Du konfigurera och testa Azure AD enkel inloggning med Learningpool Act baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="08175-138">In this section, you configure and test Azure AD single sign-on with Learningpool Act based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="08175-139">Azure AD måste du känna till användaren i Learningpool Act motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="08175-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Learningpool Act is to a user in Azure AD.</span></span> <span data-ttu-id="08175-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Learningpool Act upprättas.</span><span class="sxs-lookup"><span data-stu-id="08175-140">In other words, a link relationship between an Azure AD user and the related user in Learningpool Act needs to be established.</span></span>

<span data-ttu-id="08175-141">I Learningpool Act, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="08175-141">In Learningpool Act, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="08175-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Learningpool Act, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="08175-142">To configure and test Azure AD single sign-on with Learningpool Act, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="08175-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="08175-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="08175-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08175-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="08175-145">**[Skapa en testanvändare Learningpool Act](#creating-a-learningpool-act-test-user)**  – har en motsvarighet för Britta Simon Learningpool Act som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="08175-145">**[Creating a Learningpool Act test user](#creating-a-learningpool-act-test-user)** - to have a counterpart of Britta Simon in Learningpool Act that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="08175-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="08175-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="08175-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="08175-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="08175-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08175-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="08175-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Learningpool Act-program.</span><span class="sxs-lookup"><span data-stu-id="08175-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Learningpool Act application.</span></span>

<span data-ttu-id="08175-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Learningpool Act:**</span><span class="sxs-lookup"><span data-stu-id="08175-150">**To configure Azure AD single sign-on with Learningpool Act, perform the following steps:**</span></span>

1. <span data-ttu-id="08175-151">I Azure-portalen på den **Learningpool Act** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="08175-151">In the Azure portal, on the **Learningpool Act** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="08175-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="08175-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_samlbase.png)

3. <span data-ttu-id="08175-155">På den **Learningpool Act domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="08175-155">On the **Learningpool Act Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_url.png)

    <span data-ttu-id="08175-157">a.</span><span class="sxs-lookup"><span data-stu-id="08175-157">a.</span></span> <span data-ttu-id="08175-158">I den **inloggnings-URL** textruta anger du URL:`https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span><span class="sxs-lookup"><span data-stu-id="08175-158">In the **Sign-on URL** textbox, type the URL: `https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span></span>

    <span data-ttu-id="08175-159">b.</span><span class="sxs-lookup"><span data-stu-id="08175-159">b.</span></span> <span data-ttu-id="08175-160">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="08175-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.Learningpool.com/shibboleth` |
    | `https://<subdomain>.preview.Learningpool.com/shibboleth` |

    > [!NOTE] 
    > <span data-ttu-id="08175-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="08175-161">These values are not real.</span></span> <span data-ttu-id="08175-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="08175-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="08175-163">Kontakta [Learningpool Act klienten supportteamet](https://www.Learningpool.com/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="08175-163">Contact [Learningpool Act Client support team](https://www.Learningpool.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="08175-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="08175-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_certificate.png) 

5. <span data-ttu-id="08175-166">Learningpool Act program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="08175-166">Learningpool Act application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="08175-167">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="08175-167">Please configure the following claims for this application.</span></span> <span data-ttu-id="08175-168">Du kan hantera värden för attributen från den **”Atrribute”** för programmet.</span><span class="sxs-lookup"><span data-stu-id="08175-168">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="08175-169">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="08175-169">The following screenshot shows an example for this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_attribute.png) 

6. <span data-ttu-id="08175-171">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="08175-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="08175-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="08175-172">Attribute Name</span></span> | <span data-ttu-id="08175-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="08175-173">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="08175-174">urn: oid:1.2.840.113556.1.4.221</span><span class="sxs-lookup"><span data-stu-id="08175-174">urn:oid:1.2.840.113556.1.4.221</span></span> | <span data-ttu-id="08175-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="08175-175">user.userprincipalname</span></span> |
    | <span data-ttu-id="08175-176">urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="08175-176">urn:oid:2.5.4.42</span></span> | <span data-ttu-id="08175-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="08175-177">user.givenname</span></span> |
    | <span data-ttu-id="08175-178">urn: oid:0.9.2342.19200300.100.1.3</span><span class="sxs-lookup"><span data-stu-id="08175-178">urn:oid:0.9.2342.19200300.100.1.3</span></span> | <span data-ttu-id="08175-179">User.Mail</span><span class="sxs-lookup"><span data-stu-id="08175-179">user.mail</span></span> |    
    | <span data-ttu-id="08175-180">urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="08175-180">urn:oid:2.5.4.4</span></span> | <span data-ttu-id="08175-181">User.surname</span><span class="sxs-lookup"><span data-stu-id="08175-181">user.surname</span></span> |
    
    <span data-ttu-id="08175-182">a.</span><span class="sxs-lookup"><span data-stu-id="08175-182">a.</span></span> <span data-ttu-id="08175-183">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08175-183">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="08175-186">b.</span><span class="sxs-lookup"><span data-stu-id="08175-186">b.</span></span> <span data-ttu-id="08175-187">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="08175-187">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="08175-188">c.</span><span class="sxs-lookup"><span data-stu-id="08175-188">c.</span></span> <span data-ttu-id="08175-189">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="08175-189">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="08175-190">d.</span><span class="sxs-lookup"><span data-stu-id="08175-190">d.</span></span> <span data-ttu-id="08175-191">Lämna den **Namespace** tomt.</span><span class="sxs-lookup"><span data-stu-id="08175-191">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="08175-192">e.</span><span class="sxs-lookup"><span data-stu-id="08175-192">e.</span></span> <span data-ttu-id="08175-193">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="08175-193">Click **Ok**.</span></span>

7. <span data-ttu-id="08175-194">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="08175-194">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Learningpool-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="08175-196">Konfigurera enkel inloggning på **Learningpool Act** sida, måste du skicka den hämtade **XML-Metadata för** till [Learningpool Act supportteamet](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="08175-196">To configure single sign-on on **Learningpool Act** side, you need to send the downloaded **Metadata XML** to [Learningpool Act support team](https://www.Learningpool.com/support).</span></span> <span data-ttu-id="08175-197">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="08175-197">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="08175-198">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="08175-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="08175-199">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="08175-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="08175-200">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="08175-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="08175-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="08175-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="08175-202">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08175-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="08175-204">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="08175-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="08175-205">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="08175-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="08175-207">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="08175-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="08175-209">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08175-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="08175-211">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="08175-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="08175-213">a.</span><span class="sxs-lookup"><span data-stu-id="08175-213">a.</span></span> <span data-ttu-id="08175-214">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="08175-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="08175-215">b.</span><span class="sxs-lookup"><span data-stu-id="08175-215">b.</span></span> <span data-ttu-id="08175-216">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="08175-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="08175-217">c.</span><span class="sxs-lookup"><span data-stu-id="08175-217">c.</span></span> <span data-ttu-id="08175-218">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="08175-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="08175-219">d.</span><span class="sxs-lookup"><span data-stu-id="08175-219">d.</span></span> <span data-ttu-id="08175-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="08175-220">Click **Create**.</span></span>
 
### <a name="creating-a-learningpool-act-test-user"></a><span data-ttu-id="08175-221">Skapa en testanvändare Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="08175-221">Creating a Learningpool Act test user</span></span>

<span data-ttu-id="08175-222">Om du vill aktivera Azure AD-användare kan logga in på Learningpool Act, måste de etableras i Learningpool Act.</span><span class="sxs-lookup"><span data-stu-id="08175-222">To enable Azure AD users to log in to Learningpool Act, they must be provisioned into Learningpool Act.</span></span>

<span data-ttu-id="08175-223">Det finns inga uppgiften som du kan konfigurera användaretablering till Learningpool Act.</span><span class="sxs-lookup"><span data-stu-id="08175-223">There is no action item for you to configure user provisioning to Learningpool Act.</span></span>  
<span data-ttu-id="08175-224">Användarna måste ha skapats genom din [Learningpool Act supportteamet](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="08175-224">Users need to be created by your [Learningpool Act support team](https://www.Learningpool.com/support).</span></span>

>[!NOTE]
><span data-ttu-id="08175-225">Du kan använda något annat Learningpool Act användarens konto skapas verktyg eller API: er som tillhandahålls av Learningpool Act etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="08175-225">You can use any other Learningpool Act user account creation tools or APIs provided by Learningpool Act to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="08175-226">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="08175-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="08175-227">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Learningpool Act.</span><span class="sxs-lookup"><span data-stu-id="08175-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Learningpool Act.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="08175-229">**Om du vill tilldela Learningpool Act Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="08175-229">**To assign Britta Simon to Learningpool Act, perform the following steps:**</span></span>

1. <span data-ttu-id="08175-230">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="08175-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="08175-232">Välj i listan med program **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="08175-232">In the applications list, select **Learningpool Act**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_app.png) 

3. <span data-ttu-id="08175-234">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="08175-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="08175-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="08175-236">Click **Add** button.</span></span> <span data-ttu-id="08175-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08175-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="08175-239">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="08175-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="08175-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08175-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="08175-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08175-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="08175-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08175-242">Testing single sign-on</span></span>

<span data-ttu-id="08175-243">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="08175-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="08175-244">När du klickar på panelen Learningpool Act på åtkomstpanelen du bör få automatiskt loggat in på ditt Learningpool Act-program.</span><span class="sxs-lookup"><span data-stu-id="08175-244">When you click the Learningpool Act tile in the Access Panel, you should get automatically signed-on to your Learningpool Act application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08175-245">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="08175-245">Additional resources</span></span>

* [<span data-ttu-id="08175-246">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="08175-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="08175-247">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="08175-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_203.png

