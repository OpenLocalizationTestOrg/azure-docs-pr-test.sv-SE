---
title: "Självstudier: Azure Active Directory-integrering med BenefitHub | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och BenefitHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4069fe32-a452-463f-973e-7aa0baa4c2fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 8df9c0d8443d6685253207ed1915c780275014fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefithub"></a><span data-ttu-id="aa37b-103">Självstudier: Azure Active Directory-integrering med BenefitHub</span><span class="sxs-lookup"><span data-stu-id="aa37b-103">Tutorial: Azure Active Directory integration with BenefitHub</span></span>

<span data-ttu-id="aa37b-104">I kursen får lära du att integrera BenefitHub med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="aa37b-104">In this tutorial, you learn how to integrate BenefitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="aa37b-105">Integrera BenefitHub med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="aa37b-105">Integrating BenefitHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="aa37b-106">Du kan styra i Azure AD som har åtkomst till BenefitHub</span><span class="sxs-lookup"><span data-stu-id="aa37b-106">You can control in Azure AD who has access to BenefitHub</span></span>
- <span data-ttu-id="aa37b-107">Du kan aktivera användarna att automatiskt hämta loggat in på BenefitHub (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="aa37b-107">You can enable your users to automatically get signed-on to BenefitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="aa37b-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="aa37b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="aa37b-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aa37b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa37b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="aa37b-110">Prerequisites</span></span>

<span data-ttu-id="aa37b-111">För att konfigurera Azure AD-integrering med BenefitHub, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="aa37b-111">To configure Azure AD integration with BenefitHub, you need the following items:</span></span>

- <span data-ttu-id="aa37b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="aa37b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="aa37b-113">En BenefitHub enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="aa37b-113">A BenefitHub single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="aa37b-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="aa37b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="aa37b-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="aa37b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="aa37b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="aa37b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="aa37b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aa37b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="aa37b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="aa37b-118">Scenario description</span></span>
<span data-ttu-id="aa37b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="aa37b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="aa37b-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="aa37b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="aa37b-121">Att lägga till BenefitHub från galleriet</span><span class="sxs-lookup"><span data-stu-id="aa37b-121">Adding BenefitHub from the gallery</span></span>
2. <span data-ttu-id="aa37b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa37b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benefithub-from-the-gallery"></a><span data-ttu-id="aa37b-123">Att lägga till BenefitHub från galleriet</span><span class="sxs-lookup"><span data-stu-id="aa37b-123">Adding BenefitHub from the gallery</span></span>
<span data-ttu-id="aa37b-124">Du måste lägga till BenefitHub från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av BenefitHub i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa37b-124">To configure the integration of BenefitHub into Azure AD, you need to add BenefitHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="aa37b-125">**Utför följande steg för att lägga till BenefitHub från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="aa37b-125">**To add BenefitHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="aa37b-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="aa37b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="aa37b-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="aa37b-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="aa37b-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa37b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="aa37b-133">I sökrutan skriver **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-133">In the search box, type **BenefitHub**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_search.png)

5. <span data-ttu-id="aa37b-135">Välj i resultatpanelen **BenefitHub**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="aa37b-135">In the results panel, select **BenefitHub**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="aa37b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa37b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="aa37b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BenefitHub baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="aa37b-138">In this section, you configure and test Azure AD single sign-on with BenefitHub based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="aa37b-139">Azure AD måste du känna till användaren i BenefitHub motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="aa37b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BenefitHub is to a user in Azure AD.</span></span> <span data-ttu-id="aa37b-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i BenefitHub upprättas.</span><span class="sxs-lookup"><span data-stu-id="aa37b-140">In other words, a link relationship between an Azure AD user and the related user in BenefitHub needs to be established.</span></span>

<span data-ttu-id="aa37b-141">I BenefitHub, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="aa37b-141">In BenefitHub, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="aa37b-142">Om du vill konfigurera och testa Azure AD enkel inloggning med BenefitHub, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="aa37b-142">To configure and test Azure AD single sign-on with BenefitHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="aa37b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="aa37b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="aa37b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aa37b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="aa37b-145">**[Skapa en testanvändare BenefitHub](#creating-a-benefithub-test-user)**  – du har en motsvarighet för Britta Simon i BenefitHub som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="aa37b-145">**[Creating a BenefitHub test user](#creating-a-benefithub-test-user)** - to have a counterpart of Britta Simon in BenefitHub that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="aa37b-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aa37b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="aa37b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="aa37b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="aa37b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa37b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="aa37b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt BenefitHub program.</span><span class="sxs-lookup"><span data-stu-id="aa37b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BenefitHub application.</span></span>

<span data-ttu-id="aa37b-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med BenefitHub:**</span><span class="sxs-lookup"><span data-stu-id="aa37b-150">**To configure Azure AD single sign-on with BenefitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="aa37b-151">I Azure-portalen på den **BenefitHub** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-151">In the Azure portal, on the **BenefitHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="aa37b-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aa37b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_samlbase.png)

3. <span data-ttu-id="aa37b-155">På den **BenefitHub domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="aa37b-155">On the **BenefitHub Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_url1.png)
  
    <span data-ttu-id="aa37b-157">a.</span><span class="sxs-lookup"><span data-stu-id="aa37b-157">a.</span></span> <span data-ttu-id="aa37b-158">I den **identifierare** textruta typ:`urn:benefithub:passport`</span><span class="sxs-lookup"><span data-stu-id="aa37b-158">In the **Identifier** textbox, type: `urn:benefithub:passport`</span></span>
    
    <span data-ttu-id="aa37b-159">b.</span><span class="sxs-lookup"><span data-stu-id="aa37b-159">b.</span></span> <span data-ttu-id="aa37b-160">I den **Reply URL** textruta typ:`https://passport.benefithub.info/saml/post/ac`</span><span class="sxs-lookup"><span data-stu-id="aa37b-160">In the **Reply URL** textbox, type: `https://passport.benefithub.info/saml/post/ac`</span></span>

4. <span data-ttu-id="aa37b-161">Programmet BenefitHub förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="aa37b-161">The BenefitHub application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="aa37b-162">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="aa37b-162">Configure the following claims for this application.</span></span> <span data-ttu-id="aa37b-163">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="aa37b-163">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_attribute.png)

5. <span data-ttu-id="aa37b-165">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i den föregående bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="aa37b-165">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>
    
    | <span data-ttu-id="aa37b-166">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="aa37b-166">Attribute Name</span></span> | <span data-ttu-id="aa37b-167">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="aa37b-167">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="aa37b-168">organisations-ID</span><span class="sxs-lookup"><span data-stu-id="aa37b-168">organizationid</span></span> | <span data-ttu-id="aa37b-169">organisations-< ID ></span><span class="sxs-lookup"><span data-stu-id="aa37b-169">< organizationid ></span></span> |

    > [!NOTE]
    > <span data-ttu-id="aa37b-170">Det här attributvärdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="aa37b-170">This attribute value is not real.</span></span> <span data-ttu-id="aa37b-171">Uppdatera det här värdet med faktiska organisations-ID.</span><span class="sxs-lookup"><span data-stu-id="aa37b-171">Update this value with actual organizationid.</span></span> <span data-ttu-id="aa37b-172">Kontakta [BenefitHub supportteamet](https://www.benefithub.com/Home/ContactUs) att hämta den faktiska organisations-ID.</span><span class="sxs-lookup"><span data-stu-id="aa37b-172">Contact [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) to get the actual organizationid.</span></span>
    
    <span data-ttu-id="aa37b-173">a.</span><span class="sxs-lookup"><span data-stu-id="aa37b-173">a.</span></span> <span data-ttu-id="aa37b-174">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa37b-174">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="aa37b-177">b.</span><span class="sxs-lookup"><span data-stu-id="aa37b-177">b.</span></span> <span data-ttu-id="aa37b-178">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="aa37b-178">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="aa37b-179">c.</span><span class="sxs-lookup"><span data-stu-id="aa37b-179">c.</span></span> <span data-ttu-id="aa37b-180">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="aa37b-180">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="aa37b-181">d.</span><span class="sxs-lookup"><span data-stu-id="aa37b-181">d.</span></span> <span data-ttu-id="aa37b-182">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-182">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="aa37b-183">Innan du kan konfigurera SAML-kontroll måste du kontakta din [BenefitHub stöd](https://www.benefithub.com/Home/ContactUs) och begära värdet för attributet för unik identifierare för din klient.</span><span class="sxs-lookup"><span data-stu-id="aa37b-183">Before you can configure the SAML assertion, you need to contact your [BenefitHub support](https://www.benefithub.com/Home/ContactUs) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="aa37b-184">Du behöver det här värdet för att konfigurera det anpassade anspråket för ditt program.</span><span class="sxs-lookup"><span data-stu-id="aa37b-184">You need this value to configure the custom claim for your application.</span></span>

6. <span data-ttu-id="aa37b-185">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="aa37b-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_certificate.png) 

7. <span data-ttu-id="aa37b-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="aa37b-187">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="aa37b-189">Konfigurera enkel inloggning på **BenefitHub** sida, måste du skicka den hämtade **XML-Metadata för** till [BenefitHub supportteamet](https://www.benefithub.com/Home/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="aa37b-189">To configure single sign-on on **BenefitHub** side, you need to send the downloaded **Metadata XML** to [BenefitHub support team](https://www.benefithub.com/Home/ContactUs).</span></span> <span data-ttu-id="aa37b-190">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="aa37b-190">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="aa37b-191">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="aa37b-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="aa37b-192">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="aa37b-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="aa37b-193">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="aa37b-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="aa37b-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa37b-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="aa37b-195">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aa37b-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="aa37b-197">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="aa37b-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="aa37b-198">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="aa37b-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="aa37b-200">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="aa37b-202">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa37b-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="aa37b-204">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="aa37b-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="aa37b-206">a.</span><span class="sxs-lookup"><span data-stu-id="aa37b-206">a.</span></span> <span data-ttu-id="aa37b-207">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="aa37b-208">b.</span><span class="sxs-lookup"><span data-stu-id="aa37b-208">b.</span></span> <span data-ttu-id="aa37b-209">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="aa37b-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="aa37b-210">c.</span><span class="sxs-lookup"><span data-stu-id="aa37b-210">c.</span></span> <span data-ttu-id="aa37b-211">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="aa37b-212">d.</span><span class="sxs-lookup"><span data-stu-id="aa37b-212">d.</span></span> <span data-ttu-id="aa37b-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-213">Click **Create**.</span></span>
 
### <a name="creating-a-benefithub-test-user"></a><span data-ttu-id="aa37b-214">Skapa en testanvändare BenefitHub</span><span class="sxs-lookup"><span data-stu-id="aa37b-214">Creating a BenefitHub test user</span></span>

<span data-ttu-id="aa37b-215">I det här avsnittet skapar du en användare som kallas Britta Simon i BenefitHub.</span><span class="sxs-lookup"><span data-stu-id="aa37b-215">In this section, you create a user called Britta Simon in BenefitHub.</span></span> <span data-ttu-id="aa37b-216">Arbeta med [BenefitHub supportteamet](https://www.benefithub.com/Home/ContactUs) att lägga till användare i BenefitHub-plattformen.</span><span class="sxs-lookup"><span data-stu-id="aa37b-216">Work with [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) to add the users in the BenefitHub platform.</span></span> <span data-ttu-id="aa37b-217">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aa37b-217">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="aa37b-218">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="aa37b-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="aa37b-219">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till BenefitHub.</span><span class="sxs-lookup"><span data-stu-id="aa37b-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BenefitHub.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="aa37b-221">**Om du vill tilldela BenefitHub Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aa37b-221">**To assign Britta Simon to BenefitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="aa37b-222">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="aa37b-224">Välj i listan med program **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-224">In the applications list, select **BenefitHub**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_app.png) 

3. <span data-ttu-id="aa37b-226">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="aa37b-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="aa37b-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="aa37b-228">Click **Add** button.</span></span> <span data-ttu-id="aa37b-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa37b-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="aa37b-231">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="aa37b-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="aa37b-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa37b-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="aa37b-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa37b-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="aa37b-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa37b-234">Testing single sign-on</span></span>

<span data-ttu-id="aa37b-235">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="aa37b-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="aa37b-236">När du klickar på panelen BenefitHub på åtkomstpanelen du bör få automatiskt loggat in på ditt BenefitHub program.</span><span class="sxs-lookup"><span data-stu-id="aa37b-236">When you click the BenefitHub tile in the Access Panel, you should get automatically signed-on to your BenefitHub application.</span></span>
<span data-ttu-id="aa37b-237">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="aa37b-237">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aa37b-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="aa37b-238">Additional resources</span></span>

* [<span data-ttu-id="aa37b-239">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aa37b-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="aa37b-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="aa37b-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_203.png

