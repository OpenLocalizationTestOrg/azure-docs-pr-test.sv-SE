---
title: "Självstudier: Azure Active Directory-integrering med Infor Retail – informationshantering | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Infor Retail – informationshantering."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ff49168-ef81-4169-8e5e-dc86e24dd5e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 1ab8b7e98324ba4f4ae95775f89df0461058fe4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-infor-retail--information-management"></a><span data-ttu-id="ad7a5-103">Självstudier: Azure Active Directory-integrering med Infor Retail – informationshantering</span><span class="sxs-lookup"><span data-stu-id="ad7a5-103">Tutorial: Azure Active Directory integration with Infor Retail – Information Management</span></span>

<span data-ttu-id="ad7a5-104">I kursen får lära du att integrera Infor Retail – informationshantering med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ad7a5-104">In this tutorial, you learn how to integrate Infor Retail – Information Management with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ad7a5-105">Integrera Infor Retail – informationshantering med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-105">Integrating Infor Retail – Information Management with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ad7a5-106">Du kan styra i Azure AD som har åtkomst till Infor Retail – informationshantering.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-106">You can control in Azure AD who has access to Infor Retail – Information Management.</span></span>
- <span data-ttu-id="ad7a5-107">Du kan aktivera användarna att automatiskt hämta loggat in på Infor Retail – informationshantering (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-107">You can enable your users to automatically get signed-on to Infor Retail – Information Management (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="ad7a5-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="ad7a5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ad7a5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad7a5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ad7a5-110">Prerequisites</span></span>

<span data-ttu-id="ad7a5-111">Om du vill konfigurera Azure AD-integrering med Infor Retail-hantering, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-111">To configure Azure AD integration with Infor Retail – Information Management, you need the following items:</span></span>

- <span data-ttu-id="ad7a5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ad7a5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ad7a5-113">En Infor Retail – informationshantering enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ad7a5-113">An Infor Retail – Information Management single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ad7a5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ad7a5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ad7a5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ad7a5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ad7a5-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ad7a5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ad7a5-118">Scenario description</span></span>
<span data-ttu-id="ad7a5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ad7a5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ad7a5-121">Lägga till Infor Retail – informationshantering från galleriet</span><span class="sxs-lookup"><span data-stu-id="ad7a5-121">Adding Infor Retail – Information Management from the gallery</span></span>
2. <span data-ttu-id="ad7a5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad7a5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-infor-retail--information-management-from-the-gallery"></a><span data-ttu-id="ad7a5-123">Lägga till Infor Retail – informationshantering från galleriet</span><span class="sxs-lookup"><span data-stu-id="ad7a5-123">Adding Infor Retail – Information Management from the gallery</span></span>
<span data-ttu-id="ad7a5-124">Du måste lägga till Infor Retail – informationshantering från galleriet i listan över hanterade SaaS-appar för att konfigurera Infor Retail – informationshantering till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-124">To configure the integration of Infor Retail – Information Management into Azure AD, you need to add Infor Retail – Information Management from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ad7a5-125">**Utför följande steg för att lägga till Infor Retail – informationshantering från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ad7a5-125">**To add Infor Retail – Information Management from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ad7a5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="ad7a5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ad7a5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="ad7a5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="ad7a5-133">I sökrutan skriver **Infor Retail – informationshantering**väljer **Infor Retail – informationshantering** resultatet-panelen klickar **Lägg till** för att lägga till den programmet.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-133">In the search box, type **Infor Retail – Information Management**, select **Infor Retail – Information Management** from result panel then click **Add** button to add the application.</span></span>

    ![Infor Retail – informationshantering i resultatlistan](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ad7a5-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad7a5-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="ad7a5-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Infor Retail – informationshantering baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-136">In this section, you configure and test Azure AD single sign-on with Infor Retail – Information Management based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ad7a5-137">Azure AD måste du känna till motsvarande användaren i Infor Retail – informationshantering till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Infor Retail – Information Management is to a user in Azure AD.</span></span> <span data-ttu-id="ad7a5-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Infor Retail – informationshantering upprättas.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-138">In other words, a link relationship between an Azure AD user and the related user in Infor Retail – Information Management needs to be established.</span></span>

<span data-ttu-id="ad7a5-139">I Infor Retail-hantering, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-139">In Infor Retail – Information Management, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ad7a5-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Infor Retail-hantering, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-140">To configure and test Azure AD single sign-on with Infor Retail – Information Management, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ad7a5-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ad7a5-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ad7a5-143">**[Skapa en Infor Retail – informationshantering testanvändare](#create-an-infor-retail--information-management-test-user)**  – har en motsvarighet för Britta Simon Infor Retail – hantering av Information som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-143">**[Create an Infor Retail – Information Management test user](#create-an-infor-retail--information-management-test-user)** - to have a counterpart of Britta Simon in Infor Retail – Information Management that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ad7a5-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ad7a5-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ad7a5-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad7a5-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ad7a5-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din Infor Retail – Information hanteringsprogram.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Infor Retail – Information Management application.</span></span>

<span data-ttu-id="ad7a5-148">**Om du vill konfigurera Azure AD enkel inloggning med Infor Retail-hantering, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ad7a5-148">**To configure Azure AD single sign-on with Infor Retail – Information Management, perform the following steps:**</span></span>

1. <span data-ttu-id="ad7a5-149">I Azure-portalen på den **Infor Retail – informationshantering** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-149">In the Azure portal, on the **Infor Retail – Information Management** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="ad7a5-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_samlbase.png)

3. <span data-ttu-id="ad7a5-153">På den **Infor Retail-URL: er och domänen för hantering av Information** avsnittet, utför följande steg om du vill konfigurera programmet i IDP initierade läge:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-153">On the **Infor Retail – Information Management Domain and URLs** section, perform the following steps if you wish to configure the application in IDP initiated mode:</span></span>

    ![Med enkel inloggning information IDP Infor Retail-URL: er och domänen för hantering av Information](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url.png)

    <span data-ttu-id="ad7a5-155">a.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-155">a.</span></span> <span data-ttu-id="ad7a5-156">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-156">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
    |   |
    | -- |
    | `https://<company name>.mingle.infor.com` |
    | `http://<company name>.mingledev.infor.com` |

    <span data-ttu-id="ad7a5-157">b.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-157">b.</span></span> <span data-ttu-id="ad7a5-158">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.mingle.infor.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="ad7a5-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.mingle.infor.com/sp/ACS.saml2`</span></span>

4. <span data-ttu-id="ad7a5-159">Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-159">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Infor Retail-domänen för hantering av Information och URL: er enkel inloggning information SP](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url1.png)

    <span data-ttu-id="ad7a5-161">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.mingle.infor.com/<company code>`</span><span class="sxs-lookup"><span data-stu-id="ad7a5-161">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.mingle.infor.com/<company code>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="ad7a5-162">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-162">These values are not real.</span></span> <span data-ttu-id="ad7a5-163">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-163">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="ad7a5-164">Kontakta [Infor Retail – Information Hanteringsklient supportteamet](mailto:innovate@infor.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-164">Contact [Infor Retail – Information Management Client support team](mailto:innovate@infor.com) to get these values.</span></span> 

5. <span data-ttu-id="ad7a5-165">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_certificate.png) 

6. <span data-ttu-id="ad7a5-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="ad7a5-169">Konfigurera enkel inloggning på **Infor Retail – informationshantering** sida, måste du skicka den hämtade **XML-Metadata för** till [Infor Retail – informationshantering supportteamet](mailto:innovate@infor.com).</span><span class="sxs-lookup"><span data-stu-id="ad7a5-169">To configure single sign-on on **Infor Retail – Information Management** side, you need to send the downloaded **Metadata XML** to [Infor Retail – Information Management support team](mailto:innovate@infor.com).</span></span> <span data-ttu-id="ad7a5-170">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="ad7a5-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ad7a5-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ad7a5-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ad7a5-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ad7a5-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ad7a5-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad7a5-174">Create an Azure AD test user</span></span>

<span data-ttu-id="ad7a5-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="ad7a5-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ad7a5-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ad7a5-178">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="ad7a5-180">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="ad7a5-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="ad7a5-184">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ad7a5-184">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_04.png)

    <span data-ttu-id="ad7a5-186">a.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-186">a.</span></span> <span data-ttu-id="ad7a5-187">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-187">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ad7a5-188">b.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-188">b.</span></span> <span data-ttu-id="ad7a5-189">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="ad7a5-190">c.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-190">c.</span></span> <span data-ttu-id="ad7a5-191">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="ad7a5-192">d.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-192">d.</span></span> <span data-ttu-id="ad7a5-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-193">Click **Create**.</span></span>
 
### <a name="create-an-infor-retail--information-management-test-user"></a><span data-ttu-id="ad7a5-194">Skapa en Infor Retail – informationshantering testanvändare</span><span class="sxs-lookup"><span data-stu-id="ad7a5-194">Create an Infor Retail – Information Management test user</span></span>

<span data-ttu-id="ad7a5-195">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Infor Retail – informationshantering.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-195">In this section, you create a user called Britta Simon in Infor Retail – Information Management.</span></span> <span data-ttu-id="ad7a5-196">Se tillsammans med [Infor Retail – informationshantering supportteamet](mailto:innovate@infor.com) att lägga till användare i Infor Retail-plattform för Information.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-196">Please work with [Infor Retail – Information Management support team](mailto:innovate@infor.com) to add the users in the Infor Retail – Information Management platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="ad7a5-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ad7a5-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="ad7a5-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Infor Retail – informationshantering.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Infor Retail – Information Management.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="ad7a5-200">**Om du vill tilldela Britta Simon Infor Retail-hantering, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ad7a5-200">**To assign Britta Simon to Infor Retail – Information Management, perform the following steps:**</span></span>

1. <span data-ttu-id="ad7a5-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ad7a5-203">Välj i listan med program **Infor Retail – informationshantering**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-203">In the applications list, select **Infor Retail – Information Management**.</span></span>

    ![Infor Retail – informationshantering länken i listan med program](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_app.png)  

3. <span data-ttu-id="ad7a5-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="ad7a5-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-207">Click **Add** button.</span></span> <span data-ttu-id="ad7a5-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="ad7a5-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ad7a5-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ad7a5-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ad7a5-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad7a5-213">Test single sign-on</span></span>

<span data-ttu-id="ad7a5-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ad7a5-215">När du klickar på Infor Retail – informationshantering panelen på panelen åtkomst du ska hämta automatiskt loggat in på ditt Infor Retail – Information hanteringsprogram.</span><span class="sxs-lookup"><span data-stu-id="ad7a5-215">When you click the Infor Retail – Information Management tile in the Access Panel, you should get automatically signed-on to your Infor Retail – Information Management application.</span></span>
<span data-ttu-id="ad7a5-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ad7a5-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ad7a5-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ad7a5-217">Additional resources</span></span>

* [<span data-ttu-id="ad7a5-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad7a5-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ad7a5-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ad7a5-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_203.png

