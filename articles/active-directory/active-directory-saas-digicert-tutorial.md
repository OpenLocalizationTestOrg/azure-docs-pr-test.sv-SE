---
title: "Självstudier: Azure Active Directory-integrering med DigiCert | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och DigiCert."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 646f3129-aa67-4875-9073-1d0b6a3173d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 2ceb3c0833edcd4ecd875c5e8006961ed7216c66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-digicert"></a><span data-ttu-id="a2217-103">Självstudier: Azure Active Directory-integrering med DigiCert</span><span class="sxs-lookup"><span data-stu-id="a2217-103">Tutorial: Azure Active Directory integration with DigiCert</span></span>

<span data-ttu-id="a2217-104">I kursen får lära du att integrera DigiCert med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a2217-104">In this tutorial, you learn how to integrate DigiCert with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a2217-105">Integrera DigiCert med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a2217-105">Integrating DigiCert with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a2217-106">Du kan styra i Azure AD som har åtkomst till DigiCert</span><span class="sxs-lookup"><span data-stu-id="a2217-106">You can control in Azure AD who has access to DigiCert</span></span>
- <span data-ttu-id="a2217-107">Du kan aktivera användarna att automatiskt hämta loggat in på DigiCert (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a2217-107">You can enable your users to automatically get signed-on to DigiCert (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a2217-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a2217-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a2217-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a2217-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a2217-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a2217-110">Prerequisites</span></span>

<span data-ttu-id="a2217-111">För att konfigurera Azure AD-integrering med DigiCert, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a2217-111">To configure Azure AD integration with DigiCert, you need the following items:</span></span>

- <span data-ttu-id="a2217-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a2217-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a2217-113">En DigiCert enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a2217-113">A DigiCert single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a2217-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a2217-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a2217-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a2217-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a2217-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a2217-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a2217-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a2217-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a2217-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a2217-118">Scenario description</span></span>
<span data-ttu-id="a2217-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a2217-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a2217-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a2217-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a2217-121">Att lägga till DigiCert från galleriet</span><span class="sxs-lookup"><span data-stu-id="a2217-121">Adding DigiCert from the gallery</span></span>
2. <span data-ttu-id="a2217-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a2217-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-digicert-from-the-gallery"></a><span data-ttu-id="a2217-123">Att lägga till DigiCert från galleriet</span><span class="sxs-lookup"><span data-stu-id="a2217-123">Adding DigiCert from the gallery</span></span>
<span data-ttu-id="a2217-124">Du måste lägga till DigiCert från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av DigiCert i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a2217-124">To configure the integration of DigiCert into Azure AD, you need to add DigiCert from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a2217-125">**Utför följande steg för att lägga till DigiCert från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a2217-125">**To add DigiCert from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a2217-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a2217-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a2217-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a2217-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a2217-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a2217-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a2217-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2217-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a2217-133">I sökrutan skriver **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="a2217-133">In the search box, type **DigiCert**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_search.png)

5. <span data-ttu-id="a2217-135">Välj i resultatpanelen **DigiCert**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a2217-135">In the results panel, select **DigiCert**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a2217-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a2217-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a2217-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med DigiCert baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a2217-138">In this section, you configure and test Azure AD single sign-on with DigiCert based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a2217-139">Azure AD måste du känna till användaren i DigiCert motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a2217-139">For single sign-on to work, Azure AD needs to know what the counterpart user in DigiCert is to a user in Azure AD.</span></span> <span data-ttu-id="a2217-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i DigiCert upprättas.</span><span class="sxs-lookup"><span data-stu-id="a2217-140">In other words, a link relationship between an Azure AD user and the related user in DigiCert needs to be established.</span></span>

<span data-ttu-id="a2217-141">I DigiCert, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a2217-141">In DigiCert, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a2217-142">Om du vill konfigurera och testa Azure AD enkel inloggning med DigiCert, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a2217-142">To configure and test Azure AD single sign-on with DigiCert, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a2217-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a2217-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a2217-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a2217-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a2217-145">**[Skapa en testanvändare DigiCert](#creating-a-digicert-test-user)**  – du har en motsvarighet för Britta Simon i DigiCert som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a2217-145">**[Creating a DigiCert test user](#creating-a-digicert-test-user)** - to have a counterpart of Britta Simon in DigiCert that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a2217-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a2217-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a2217-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a2217-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a2217-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a2217-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a2217-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt DigiCert-program.</span><span class="sxs-lookup"><span data-stu-id="a2217-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your DigiCert application.</span></span>

<span data-ttu-id="a2217-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med DigiCert:**</span><span class="sxs-lookup"><span data-stu-id="a2217-150">**To configure Azure AD single sign-on with DigiCert, perform the following steps:**</span></span>

1. <span data-ttu-id="a2217-151">I Azure-portalen på den **DigiCert** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a2217-151">In the Azure portal, on the **DigiCert** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a2217-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a2217-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_samlbase.png)

3. <span data-ttu-id="a2217-155">På den **DigiCert domän och URL: er** avsnittet användaren behöver inte utföra några steg som appen före redan är integrerad med Azure.</span><span class="sxs-lookup"><span data-stu-id="a2217-155">On the **DigiCert Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_url.png)

4. <span data-ttu-id="a2217-157">DigiCert program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="a2217-157">DigiCert application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="a2217-158">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="a2217-158">Configure the following claims for this application.</span></span> <span data-ttu-id="a2217-159">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="a2217-159">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="a2217-160">Följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a2217-160">The following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_attributes.png)
    
5. <span data-ttu-id="a2217-162">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a2217-162">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="a2217-163">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="a2217-163">Attribute Name</span></span> | <span data-ttu-id="a2217-164">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="a2217-164">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="a2217-165">Företag</span><span class="sxs-lookup"><span data-stu-id="a2217-165">company</span></span> | <span data-ttu-id="a2217-166">< companycode ></span><span class="sxs-lookup"><span data-stu-id="a2217-166">< companycode ></span></span> |
    | <span data-ttu-id="a2217-167">digicertrole</span><span class="sxs-lookup"><span data-stu-id="a2217-167">digicertrole</span></span> | <span data-ttu-id="a2217-168">CanAccessCertCentral</span><span class="sxs-lookup"><span data-stu-id="a2217-168">CanAccessCertCentral</span></span> |

    > [!Note]
    > <span data-ttu-id="a2217-169">Värdet för **företagets** attributet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a2217-169">The value of **company** attribute is not real.</span></span> <span data-ttu-id="a2217-170">Uppdatera det här värdet med verkligt företagskod.</span><span class="sxs-lookup"><span data-stu-id="a2217-170">Update this value with actual company code.</span></span> <span data-ttu-id="a2217-171">Att hämta värdet för **företagets** attributet Kontakta [DigiCert supportteamet](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="a2217-171">To get the value of **company** attribute contact [DigiCert support team](mailto:support@digicert.com).</span></span>

    <span data-ttu-id="a2217-172">a.</span><span class="sxs-lookup"><span data-stu-id="a2217-172">a.</span></span> <span data-ttu-id="a2217-173">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2217-173">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="a2217-176">b.</span><span class="sxs-lookup"><span data-stu-id="a2217-176">b.</span></span> <span data-ttu-id="a2217-177">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="a2217-177">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="a2217-178">c.</span><span class="sxs-lookup"><span data-stu-id="a2217-178">c.</span></span> <span data-ttu-id="a2217-179">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="a2217-179">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="a2217-180">d.</span><span class="sxs-lookup"><span data-stu-id="a2217-180">d.</span></span> <span data-ttu-id="a2217-181">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2217-181">Click **Ok**.</span></span> 

6. <span data-ttu-id="a2217-182">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a2217-182">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_certificate.png) 

7. <span data-ttu-id="a2217-184">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a2217-184">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="a2217-186">Konfigurera enkel inloggning på **DigiCert** sida, måste du skicka den hämtade **XML-Metadata för** till [DigiCert supportteamet](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="a2217-186">To configure single sign-on on **DigiCert** side, you need to send the downloaded **Metadata XML** to [DigiCert support team](mailto:support@digicert.com).</span></span> <span data-ttu-id="a2217-187">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="a2217-187">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="a2217-188">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="a2217-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a2217-189">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="a2217-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a2217-190">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a2217-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a2217-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a2217-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="a2217-192">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a2217-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a2217-194">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a2217-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a2217-195">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a2217-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a2217-197">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a2217-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a2217-199">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2217-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a2217-201">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a2217-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a2217-203">a.</span><span class="sxs-lookup"><span data-stu-id="a2217-203">a.</span></span> <span data-ttu-id="a2217-204">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a2217-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a2217-205">b.</span><span class="sxs-lookup"><span data-stu-id="a2217-205">b.</span></span> <span data-ttu-id="a2217-206">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a2217-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a2217-207">c.</span><span class="sxs-lookup"><span data-stu-id="a2217-207">c.</span></span> <span data-ttu-id="a2217-208">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a2217-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a2217-209">d.</span><span class="sxs-lookup"><span data-stu-id="a2217-209">d.</span></span> <span data-ttu-id="a2217-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a2217-210">Click **Create**.</span></span>
 
### <a name="creating-a-digicert-test-user"></a><span data-ttu-id="a2217-211">Skapa en testanvändare DigiCert</span><span class="sxs-lookup"><span data-stu-id="a2217-211">Creating a DigiCert test user</span></span>

<span data-ttu-id="a2217-212">I det här avsnittet skapar du en användare som kallas Britta Simon i DigiCert.</span><span class="sxs-lookup"><span data-stu-id="a2217-212">In this section, you create a user called Britta Simon in DigiCert.</span></span> <span data-ttu-id="a2217-213">Se tillsammans med [DigiCert supportteamet](mailto:support@digicert.com) att lägga till användare i DigiCert.</span><span class="sxs-lookup"><span data-stu-id="a2217-213">Please work with [DigiCert support team](mailto:support@digicert.com) to add the users in DigiCert.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a2217-214">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a2217-214">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a2217-215">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till DigiCert.</span><span class="sxs-lookup"><span data-stu-id="a2217-215">In this section, you enable Britta Simon to use Azure single sign-on by granting access to DigiCert.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a2217-217">**Om du vill tilldela DigiCert Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a2217-217">**To assign Britta Simon to DigiCert, perform the following steps:**</span></span>

1. <span data-ttu-id="a2217-218">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a2217-218">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a2217-220">Välj i listan med program **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="a2217-220">In the applications list, select **DigiCert**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_app.png) 

3. <span data-ttu-id="a2217-222">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a2217-222">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a2217-224">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a2217-224">Click **Add** button.</span></span> <span data-ttu-id="a2217-225">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2217-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a2217-227">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a2217-227">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a2217-228">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2217-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a2217-229">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2217-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a2217-230">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a2217-230">Testing single sign-on</span></span>

<span data-ttu-id="a2217-231">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a2217-231">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a2217-232">När du klickar på panelen DigiCert på åtkomstpanelen du bör få automatiskt loggat in på ditt DeigiCert program.</span><span class="sxs-lookup"><span data-stu-id="a2217-232">When you click the DigiCert tile in the Access Panel, you should get automatically signed-on to your DeigiCert application.</span></span>
<span data-ttu-id="a2217-233">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a2217-233">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a2217-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a2217-234">Additional resources</span></span>

* [<span data-ttu-id="a2217-235">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a2217-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a2217-236">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a2217-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_203.png

