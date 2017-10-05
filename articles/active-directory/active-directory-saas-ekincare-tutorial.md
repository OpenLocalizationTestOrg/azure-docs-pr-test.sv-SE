---
title: "Självstudier: Azure Active Directory-integrering med eKincare | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och eKincare."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 57f56d14-83cf-4cbb-b342-fac4fc60078f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 014eaff14974bb6cd551b6fe53409ede6a6dfea1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ekincare"></a><span data-ttu-id="c9319-103">Självstudier: Azure Active Directory-integrering med eKincare</span><span class="sxs-lookup"><span data-stu-id="c9319-103">Tutorial: Azure Active Directory integration with eKincare</span></span>

<span data-ttu-id="c9319-104">I kursen får lära du att integrera eKincare med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c9319-104">In this tutorial, you learn how to integrate eKincare with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c9319-105">Integrera eKincare med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c9319-105">Integrating eKincare with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c9319-106">Du kan styra i Azure AD som har åtkomst till eKincare</span><span class="sxs-lookup"><span data-stu-id="c9319-106">You can control in Azure AD who has access to eKincare</span></span>
- <span data-ttu-id="c9319-107">Du kan aktivera användarna att automatiskt hämta loggat in på eKincare (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c9319-107">You can enable your users to automatically get signed-on to eKincare (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c9319-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c9319-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c9319-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9319-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9319-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c9319-110">Prerequisites</span></span>

<span data-ttu-id="c9319-111">För att konfigurera Azure AD-integrering med eKincare, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c9319-111">To configure Azure AD integration with eKincare, you need the following items:</span></span>

- <span data-ttu-id="c9319-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c9319-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9319-113">En eKincare enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c9319-113">A eKincare single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c9319-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c9319-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c9319-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c9319-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9319-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c9319-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9319-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9319-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9319-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c9319-118">Scenario description</span></span>
<span data-ttu-id="c9319-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c9319-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c9319-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c9319-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9319-121">Att lägga till eKincare från galleriet</span><span class="sxs-lookup"><span data-stu-id="c9319-121">Adding eKincare from the gallery</span></span>
2. <span data-ttu-id="c9319-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9319-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ekincare-from-the-gallery"></a><span data-ttu-id="c9319-123">Att lägga till eKincare från galleriet</span><span class="sxs-lookup"><span data-stu-id="c9319-123">Adding eKincare from the gallery</span></span>
<span data-ttu-id="c9319-124">Du måste lägga till eKincare från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av eKincare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9319-124">To configure the integration of eKincare into Azure AD, you need to add eKincare from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c9319-125">**Utför följande steg för att lägga till eKincare från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c9319-125">**To add eKincare from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c9319-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c9319-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c9319-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c9319-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c9319-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c9319-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c9319-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9319-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c9319-133">I sökrutan skriver **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="c9319-133">In the search box, type **eKincare**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_search.png)

5. <span data-ttu-id="c9319-135">Välj i resultatpanelen **eKincare**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c9319-135">In the results panel, select **eKincare**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c9319-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9319-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c9319-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med eKincare baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c9319-138">In this section, you configure and test Azure AD single sign-on with eKincare based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c9319-139">Azure AD måste du känna till användaren i eKincare motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c9319-139">For single sign-on to work, Azure AD needs to know what the counterpart user in eKincare is to a user in Azure AD.</span></span> <span data-ttu-id="c9319-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i eKincare upprättas.</span><span class="sxs-lookup"><span data-stu-id="c9319-140">In other words, a link relationship between an Azure AD user and the related user in eKincare needs to be established.</span></span>

<span data-ttu-id="c9319-141">I eKincare, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c9319-141">In eKincare, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c9319-142">Om du vill konfigurera och testa Azure AD enkel inloggning med eKincare, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c9319-142">To configure and test Azure AD single sign-on with eKincare, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c9319-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c9319-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c9319-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9319-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9319-145">**[Skapa en testanvändare eKincare](#creating-a-ekincare-test-user)**  – du har en motsvarighet för Britta Simon i eKincare som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c9319-145">**[Creating a eKincare test user](#creating-a-ekincare-test-user)** - to have a counterpart of Britta Simon in eKincare that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c9319-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9319-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9319-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c9319-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c9319-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9319-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c9319-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt eKincare program.</span><span class="sxs-lookup"><span data-stu-id="c9319-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your eKincare application.</span></span>

<span data-ttu-id="c9319-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med eKincare:**</span><span class="sxs-lookup"><span data-stu-id="c9319-150">**To configure Azure AD single sign-on with eKincare, perform the following steps:**</span></span>

1. <span data-ttu-id="c9319-151">I Azure-portalen på den **eKincare** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c9319-151">In the Azure portal, on the **eKincare** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c9319-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9319-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_samlbase.png)

3. <span data-ttu-id="c9319-155">På den **eKincare domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c9319-155">On the **eKincare Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_url.png)

    <span data-ttu-id="c9319-157">a.</span><span class="sxs-lookup"><span data-stu-id="c9319-157">a.</span></span> <span data-ttu-id="c9319-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<instancename>.ekincare.com/`</span><span class="sxs-lookup"><span data-stu-id="c9319-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.ekincare.com/`</span></span>

    <span data-ttu-id="c9319-159">b.</span><span class="sxs-lookup"><span data-stu-id="c9319-159">b.</span></span> <span data-ttu-id="c9319-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<instancename>.ekincare.com/hul/saml`</span><span class="sxs-lookup"><span data-stu-id="c9319-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<instancename>.ekincare.com/hul/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c9319-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c9319-161">These values are not real.</span></span> <span data-ttu-id="c9319-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="c9319-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="c9319-163">Kontakta [eKincare supportteamet](mailto:tech@ekincare.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c9319-163">Contact [eKincare support team](mailto:tech@ekincare.com) to get these values.</span></span>
 
4. <span data-ttu-id="c9319-164">eKincare program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="c9319-164">eKincare application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="c9319-165">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="c9319-165">Configure the following claims for this application.</span></span> <span data-ttu-id="c9319-166">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="c9319-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="c9319-167">Följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c9319-167">The following screenshot shows an example for this configuration.</span></span>

    <span data-ttu-id="c9319-168">Anspråkets namn kommer alltid att **”employeeid”** och värdet som vi har mappats till user.extensionattribute1 som innehåller employeeid för användaren.</span><span class="sxs-lookup"><span data-stu-id="c9319-168">The claim name will always be **"employeeid"** and the value of which we have mapped to user.extensionattribute1, that contains the employeeid of the user.</span></span> <span data-ttu-id="c9319-169">De andra två anspråk name engångsfaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="c9319-169">The other two claims' name i.e</span></span> <span data-ttu-id="c9319-170">**”organisations-ID”** och **”organisationsnamn”** är alltid samma och deras värden innehåller information om organisationen för användaren.</span><span class="sxs-lookup"><span data-stu-id="c9319-170">**"organizationid"** and **"organizationname"** will always be same and their values contain the details of the organization of the user respectively.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/attribute.png)
    
5. <span data-ttu-id="c9319-172">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c9319-172">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="c9319-173">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="c9319-173">Attribute Name</span></span> | <span data-ttu-id="c9319-174">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="c9319-174">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="c9319-175">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="c9319-175">employeeid</span></span> | <span data-ttu-id="c9319-176">*User.extensionattribute1*</span><span class="sxs-lookup"><span data-stu-id="c9319-176">*user.extensionattribute1*</span></span> |
    | <span data-ttu-id="c9319-177">organisations-ID</span><span class="sxs-lookup"><span data-stu-id="c9319-177">organizationid</span></span> | <span data-ttu-id="c9319-178">*”uniquevalue”*</span><span class="sxs-lookup"><span data-stu-id="c9319-178">*"uniquevalue"*</span></span> |
    | <span data-ttu-id="c9319-179">organisationsnamn</span><span class="sxs-lookup"><span data-stu-id="c9319-179">organizationname</span></span> | <span data-ttu-id="c9319-180">*User.CompanyName*</span><span class="sxs-lookup"><span data-stu-id="c9319-180">*user.companyname*</span></span> |

    <span data-ttu-id="c9319-181">a.</span><span class="sxs-lookup"><span data-stu-id="c9319-181">a.</span></span> <span data-ttu-id="c9319-182">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9319-182">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/05.png)
    
    <span data-ttu-id="c9319-185">b.</span><span class="sxs-lookup"><span data-stu-id="c9319-185">b.</span></span> <span data-ttu-id="c9319-186">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c9319-186">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="c9319-187">c.</span><span class="sxs-lookup"><span data-stu-id="c9319-187">c.</span></span> <span data-ttu-id="c9319-188">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c9319-188">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c9319-189">d.</span><span class="sxs-lookup"><span data-stu-id="c9319-189">d.</span></span> <span data-ttu-id="c9319-190">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="c9319-190">Click **Ok**</span></span>

6. <span data-ttu-id="c9319-191">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c9319-191">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_certificate.png) 

7. <span data-ttu-id="c9319-193">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c9319-193">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c9319-195">Konfigurera enkel inloggning på **eKincare** sida, måste du skicka den hämtade **XML-Metadata för** till [eKincare supportteamet](mailto:tech@ekincare.com).</span><span class="sxs-lookup"><span data-stu-id="c9319-195">To configure single sign-on on **eKincare** side, you need to send the downloaded **Metadata XML** to [eKincare support team](mailto:tech@ekincare.com).</span></span> <span data-ttu-id="c9319-196">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="c9319-196">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c9319-197">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c9319-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c9319-198">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c9319-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c9319-199">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c9319-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c9319-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9319-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="c9319-201">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9319-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c9319-203">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c9319-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c9319-204">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c9319-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c9319-206">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c9319-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c9319-208">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9319-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c9319-210">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c9319-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c9319-212">a.</span><span class="sxs-lookup"><span data-stu-id="c9319-212">a.</span></span> <span data-ttu-id="c9319-213">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9319-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9319-214">b.</span><span class="sxs-lookup"><span data-stu-id="c9319-214">b.</span></span> <span data-ttu-id="c9319-215">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c9319-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c9319-216">c.</span><span class="sxs-lookup"><span data-stu-id="c9319-216">c.</span></span> <span data-ttu-id="c9319-217">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c9319-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c9319-218">d.</span><span class="sxs-lookup"><span data-stu-id="c9319-218">d.</span></span> <span data-ttu-id="c9319-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c9319-219">Click **Create**.</span></span>
 
### <a name="creating-a-ekincare-test-user"></a><span data-ttu-id="c9319-220">Skapa en testanvändare eKincare</span><span class="sxs-lookup"><span data-stu-id="c9319-220">Creating a eKincare test user</span></span>

<span data-ttu-id="c9319-221">Programmet stöder bara i tid användaretablering och authentication-användare skapas automatiskt i programmet.</span><span class="sxs-lookup"><span data-stu-id="c9319-221">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c9319-222">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c9319-222">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c9319-223">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till eKincare.</span><span class="sxs-lookup"><span data-stu-id="c9319-223">In this section, you enable Britta Simon to use Azure single sign-on by granting access to eKincare.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c9319-225">**Om du vill tilldela eKincare Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c9319-225">**To assign Britta Simon to eKincare, perform the following steps:**</span></span>

1. <span data-ttu-id="c9319-226">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c9319-226">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c9319-228">Välj i listan med program **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="c9319-228">In the applications list, select **eKincare**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_app.png) 

3. <span data-ttu-id="c9319-230">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c9319-230">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c9319-232">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c9319-232">Click **Add** button.</span></span> <span data-ttu-id="c9319-233">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9319-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c9319-235">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c9319-235">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c9319-236">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9319-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9319-237">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9319-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c9319-238">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9319-238">Testing single sign-on</span></span>

<span data-ttu-id="c9319-239">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c9319-239">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c9319-240">När du klickar på panelen eKincare på åtkomstpanelen du bör få automatiskt loggat in på ditt eKincare program.</span><span class="sxs-lookup"><span data-stu-id="c9319-240">When you click the eKincare tile in the Access Panel, you should get automatically signed-on to your eKincare application.</span></span>
<span data-ttu-id="c9319-241">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="c9319-241">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9319-242">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c9319-242">Additional resources</span></span>

* [<span data-ttu-id="c9319-243">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c9319-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9319-244">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c9319-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_203.png

