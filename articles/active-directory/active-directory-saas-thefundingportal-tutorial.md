---
title: "Självstudier: Azure Active Directory-integrering med den finansiering Portal | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och det finansiering Portal."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: d0bfc793bb26c551f85706eaec857962a3415e1f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a><span data-ttu-id="5e502-103">Självstudier: Azure Active Directory-integrering med den finansiering Portal</span><span class="sxs-lookup"><span data-stu-id="5e502-103">Tutorial: Azure Active Directory integration with The Funding Portal</span></span>

<span data-ttu-id="5e502-104">I kursen får lära du att integrera den finansiering Portal med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5e502-104">In this tutorial, you learn how to integrate The Funding Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5e502-105">Integrera den finansiering Portal med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5e502-105">Integrating The Funding Portal with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5e502-106">Du kan styra i Azure AD som har åtkomst till den finansiering Portal</span><span class="sxs-lookup"><span data-stu-id="5e502-106">You can control in Azure AD who has access to The Funding Portal</span></span>
- <span data-ttu-id="5e502-107">Du kan aktivera användarna att automatiskt hämta loggat in på den finansiering Portal (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5e502-107">You can enable your users to automatically get signed-on to The Funding Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5e502-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5e502-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5e502-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5e502-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e502-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5e502-110">Prerequisites</span></span>

<span data-ttu-id="5e502-111">För att konfigurera Azure AD-integrering med den finansiering Portal, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5e502-111">To configure Azure AD integration with The Funding Portal, you need the following items:</span></span>

- <span data-ttu-id="5e502-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5e502-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5e502-113">En av finansiering Portal enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5e502-113">A The Funding Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5e502-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5e502-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5e502-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5e502-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5e502-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5e502-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5e502-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5e502-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5e502-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5e502-118">Scenario description</span></span>
<span data-ttu-id="5e502-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5e502-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5e502-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5e502-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5e502-121">Att lägga till det finansiering portalen från galleriet</span><span class="sxs-lookup"><span data-stu-id="5e502-121">Adding The Funding Portal from the gallery</span></span>
2. <span data-ttu-id="5e502-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e502-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-the-funding-portal-from-the-gallery"></a><span data-ttu-id="5e502-123">Att lägga till det finansiering portalen från galleriet</span><span class="sxs-lookup"><span data-stu-id="5e502-123">Adding The Funding Portal from the gallery</span></span>
<span data-ttu-id="5e502-124">Du måste lägga till det finansiering Portal från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av den finansiering portalen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e502-124">To configure the integration of The Funding Portal into Azure AD, you need to add The Funding Portal from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5e502-125">**Utför följande steg för att lägga till det finansiering portalen från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="5e502-125">**To add The Funding Portal from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5e502-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5e502-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5e502-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5e502-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5e502-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5e502-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5e502-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e502-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5e502-133">I sökrutan skriver **i finansiering Portal**.</span><span class="sxs-lookup"><span data-stu-id="5e502-133">In the search box, type **The Funding Portal**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_search.png)

5. <span data-ttu-id="5e502-135">Välj i resultatpanelen **i finansiering Portal**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="5e502-135">In the results panel, select **The Funding Portal**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5e502-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e502-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5e502-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med det finansiering Portal baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5e502-138">In this section, you configure and test Azure AD single sign-on with The Funding Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5e502-139">Azure AD måste du känna till motsvarande användaren i det finansiering Portal till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="5e502-139">For single sign-on to work, Azure AD needs to know what the counterpart user in The Funding Portal is to a user in Azure AD.</span></span> <span data-ttu-id="5e502-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i det finansiering Portal upprättas.</span><span class="sxs-lookup"><span data-stu-id="5e502-140">In other words, a link relationship between an Azure AD user and the related user in The Funding Portal needs to be established.</span></span>

<span data-ttu-id="5e502-141">I det finansiering Portal, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5e502-141">In The Funding Portal, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5e502-142">Om du vill konfigurera och testa Azure AD enkel inloggning med det finansiering Portal, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5e502-142">To configure and test Azure AD single sign-on with The Funding Portal, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5e502-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5e502-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5e502-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5e502-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5e502-145">**[Skapa den finansiering Portal testanvändare](#creating-the-funding-portal-test-user)**  – du har en motsvarighet för Britta Simon i det finansiering Portal som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5e502-145">**[Creating The Funding Portal test user](#creating-the-funding-portal-test-user)** - to have a counterpart of Britta Simon in The Funding Portal that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5e502-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5e502-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5e502-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5e502-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5e502-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e502-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5e502-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program i finansiering Portal.</span><span class="sxs-lookup"><span data-stu-id="5e502-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your The Funding Portal application.</span></span>

<span data-ttu-id="5e502-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med det finansiering Portal:**</span><span class="sxs-lookup"><span data-stu-id="5e502-150">**To configure Azure AD single sign-on with The Funding Portal, perform the following steps:**</span></span>

1. <span data-ttu-id="5e502-151">I Azure-portalen på den **i finansiering Portal** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5e502-151">In the Azure portal, on the **The Funding Portal** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5e502-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5e502-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

3. <span data-ttu-id="5e502-155">På den **efter finansiering Portal domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5e502-155">On the **The Funding Portal Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    <span data-ttu-id="5e502-157">a.</span><span class="sxs-lookup"><span data-stu-id="5e502-157">a.</span></span> <span data-ttu-id="5e502-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.regenteducation.net/`</span><span class="sxs-lookup"><span data-stu-id="5e502-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.regenteducation.net/`</span></span>

    <span data-ttu-id="5e502-159">b.</span><span class="sxs-lookup"><span data-stu-id="5e502-159">b.</span></span> <span data-ttu-id="5e502-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.regenteducation.net`</span><span class="sxs-lookup"><span data-stu-id="5e502-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.regenteducation.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5e502-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5e502-161">These values are not real.</span></span> <span data-ttu-id="5e502-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="5e502-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5e502-163">Kontakta [finansiering Portal klienten supportteamet](mailto:info@regenteducation.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5e502-163">Contact [The Funding Portal Client support team](mailto:info@regenteducation.com) to get these values.</span></span> 

4. <span data-ttu-id="5e502-164">Programmet finansiering Portal förväntar SAML intyg som innehåller ett attribut med namnet ”externalId1”.</span><span class="sxs-lookup"><span data-stu-id="5e502-164">The Funding Portal application expects the SAML assertions to contain an attribute named "externalId1".</span></span> <span data-ttu-id="5e502-165">Värdet för ”externalId1” ska vara en känd studentID.</span><span class="sxs-lookup"><span data-stu-id="5e502-165">The value of "externalId1" should be a recognized studentID.</span></span> <span data-ttu-id="5e502-166">Konfigurera ”externalId1”-anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="5e502-166">Configure the "externalId1" claim for this application.</span></span> <span data-ttu-id="5e502-167">Du kan hantera värden för attributen från den **användarattribut** av programmet.</span><span class="sxs-lookup"><span data-stu-id="5e502-167">You can manage the values of these attributes from the **User Attributes** of the application.</span></span> <span data-ttu-id="5e502-168">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="5e502-168">The following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

5. <span data-ttu-id="5e502-170">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5e502-170">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>

    | <span data-ttu-id="5e502-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="5e502-171">Attribute Name</span></span> | <span data-ttu-id="5e502-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="5e502-172">Attribute Value</span></span> |
    | ------------------- | ---------------- |
    | <span data-ttu-id="5e502-173">externalId1</span><span class="sxs-lookup"><span data-stu-id="5e502-173">externalId1</span></span> | <span data-ttu-id="5e502-174">User.extensionattribute1</span><span class="sxs-lookup"><span data-stu-id="5e502-174">user.extensionattribute1</span></span> |

    <span data-ttu-id="5e502-175">a.</span><span class="sxs-lookup"><span data-stu-id="5e502-175">a.</span></span> <span data-ttu-id="5e502-176">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e502-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="5e502-179">b.</span><span class="sxs-lookup"><span data-stu-id="5e502-179">b.</span></span> <span data-ttu-id="5e502-180">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="5e502-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="5e502-181">c.</span><span class="sxs-lookup"><span data-stu-id="5e502-181">c.</span></span> <span data-ttu-id="5e502-182">Från den **attributvärdet** väljer du det attribut som du vill använda för din implementering.</span><span class="sxs-lookup"><span data-stu-id="5e502-182">From the **Attribute Value** list, select the attribute you want to use for your implementation.</span></span> <span data-ttu-id="5e502-183">Till exempel om du har lagrat StudentID värdet i ExtensionAttribute1, väljer du user.extensionattribute1.</span><span class="sxs-lookup"><span data-stu-id="5e502-183">For example, if you have stored the StudentID value in the ExtensionAttribute1, then select user.extensionattribute1.</span></span>
    
    <span data-ttu-id="5e502-184">d.</span><span class="sxs-lookup"><span data-stu-id="5e502-184">d.</span></span> <span data-ttu-id="5e502-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e502-185">Click **Ok**.</span></span>
 
6. <span data-ttu-id="5e502-186">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="5e502-186">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

7. <span data-ttu-id="5e502-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5e502-188">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="5e502-190">Konfigurera enkel inloggning på **i finansiering Portal** sida, måste du skicka den hämtade **XML-Metadata för** till [i finansiering Portal supportteamet](mailto:info@regenteducation.com).</span><span class="sxs-lookup"><span data-stu-id="5e502-190">To configure single sign-on on **The Funding Portal** side, you need to send the downloaded **Metadata XML** to [The Funding Portal support team](mailto:info@regenteducation.com).</span></span> <span data-ttu-id="5e502-191">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="5e502-191">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="5e502-192">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="5e502-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5e502-193">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="5e502-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5e502-194">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5e502-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5e502-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e502-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="5e502-196">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5e502-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5e502-198">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="5e502-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5e502-199">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5e502-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5e502-201">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5e502-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5e502-203">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e502-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5e502-205">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5e502-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5e502-207">a.</span><span class="sxs-lookup"><span data-stu-id="5e502-207">a.</span></span> <span data-ttu-id="5e502-208">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5e502-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5e502-209">b.</span><span class="sxs-lookup"><span data-stu-id="5e502-209">b.</span></span> <span data-ttu-id="5e502-210">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5e502-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5e502-211">c.</span><span class="sxs-lookup"><span data-stu-id="5e502-211">c.</span></span> <span data-ttu-id="5e502-212">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5e502-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5e502-213">d.</span><span class="sxs-lookup"><span data-stu-id="5e502-213">d.</span></span> <span data-ttu-id="5e502-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5e502-214">Click **Create**.</span></span>
 
### <a name="creating-the-funding-portal-test-user"></a><span data-ttu-id="5e502-215">Skapa den finansiering Portal testanvändare</span><span class="sxs-lookup"><span data-stu-id="5e502-215">Creating The Funding Portal test user</span></span>

<span data-ttu-id="5e502-216">I det här avsnittet kan du skapa en användare som heter Britta Simon i det finansiering portalen.</span><span class="sxs-lookup"><span data-stu-id="5e502-216">In this section, you create a user called Britta Simon in The Funding Portal.</span></span> <span data-ttu-id="5e502-217">Arbeta med [i finansiering Portal supportteamet](mailto:info@regenteducation.com) att lägga till användaren och aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5e502-217">Work with [The Funding Portal support team](mailto:info@regenteducation.com) to add the test user and enable SSO.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5e502-218">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="5e502-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5e502-219">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till den finansiering Portal.</span><span class="sxs-lookup"><span data-stu-id="5e502-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to The Funding Portal.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5e502-221">**Om du vill tilldela den finansiering Portal Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5e502-221">**To assign Britta Simon to The Funding Portal, perform the following steps:**</span></span>

1. <span data-ttu-id="5e502-222">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5e502-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5e502-224">Välj i listan med program **i finansiering Portal**.</span><span class="sxs-lookup"><span data-stu-id="5e502-224">In the applications list, select **The Funding Portal**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

3. <span data-ttu-id="5e502-226">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5e502-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5e502-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5e502-228">Click **Add** button.</span></span> <span data-ttu-id="5e502-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e502-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5e502-231">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="5e502-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5e502-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e502-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5e502-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e502-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5e502-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e502-234">Testing single sign-on</span></span>

<span data-ttu-id="5e502-235">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5e502-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5e502-236">När du klickar på panelen i finansiering Portal på åtkomstpanelen du bör få automatiskt loggat in på programmet i finansiering Portal.</span><span class="sxs-lookup"><span data-stu-id="5e502-236">When you click the The Funding Portal tile in the Access Panel, you should get automatically signed-on to your The Funding Portal application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e502-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5e502-237">Additional resources</span></span>

* [<span data-ttu-id="5e502-238">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5e502-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5e502-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5e502-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png

