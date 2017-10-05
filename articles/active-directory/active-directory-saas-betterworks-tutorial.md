---
title: "Självstudier: Azure Active Directory-integrering med BetterWorks | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och BetterWorks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: d6a5b167c0befbd0fe2c65bdd16abc35ed0a659c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a><span data-ttu-id="3c8b8-103">Självstudier: Azure Active Directory-integrering med BetterWorks</span><span class="sxs-lookup"><span data-stu-id="3c8b8-103">Tutorial: Azure Active Directory integration with BetterWorks</span></span>

<span data-ttu-id="3c8b8-104">I kursen får lära du att integrera BetterWorks med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3c8b8-104">In this tutorial, you learn how to integrate BetterWorks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3c8b8-105">Integrera BetterWorks med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-105">Integrating BetterWorks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3c8b8-106">Du kan styra i Azure AD som har åtkomst till BetterWorks</span><span class="sxs-lookup"><span data-stu-id="3c8b8-106">You can control in Azure AD who has access to BetterWorks</span></span>
- <span data-ttu-id="3c8b8-107">Du kan aktivera användarna att automatiskt hämta loggat in på BetterWorks (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3c8b8-107">You can enable your users to automatically get signed-on to BetterWorks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3c8b8-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3c8b8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3c8b8-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3c8b8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c8b8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3c8b8-110">Prerequisites</span></span>

<span data-ttu-id="3c8b8-111">För att konfigurera Azure AD-integrering med BetterWorks, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-111">To configure Azure AD integration with BetterWorks, you need the following items:</span></span>

- <span data-ttu-id="3c8b8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3c8b8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3c8b8-113">En BetterWorks enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="3c8b8-113">A BetterWorks single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3c8b8-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3c8b8-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3c8b8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3c8b8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3c8b8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3c8b8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3c8b8-118">Scenario description</span></span>
<span data-ttu-id="3c8b8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3c8b8-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3c8b8-121">Att lägga till BetterWorks från galleriet</span><span class="sxs-lookup"><span data-stu-id="3c8b8-121">Adding BetterWorks from the gallery</span></span>
2. <span data-ttu-id="3c8b8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3c8b8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-betterworks-from-the-gallery"></a><span data-ttu-id="3c8b8-123">Att lägga till BetterWorks från galleriet</span><span class="sxs-lookup"><span data-stu-id="3c8b8-123">Adding BetterWorks from the gallery</span></span>
<span data-ttu-id="3c8b8-124">Du måste lägga till BetterWorks från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av BetterWorks i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-124">To configure the integration of BetterWorks into Azure AD, you need to add BetterWorks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3c8b8-125">**Utför följande steg för att lägga till BetterWorks från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3c8b8-125">**To add BetterWorks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3c8b8-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3c8b8-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3c8b8-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3c8b8-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3c8b8-133">I sökrutan skriver **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-133">In the search box, type **BetterWorks**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_search.png)

5. <span data-ttu-id="3c8b8-135">Välj i resultatpanelen **BetterWorks**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-135">In the results panel, select **BetterWorks**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3c8b8-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3c8b8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3c8b8-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BetterWorks baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-138">In this section, you configure and test Azure AD single sign-on with BetterWorks based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3c8b8-139">Azure AD måste du känna till användaren i BetterWorks motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BetterWorks is to a user in Azure AD.</span></span> <span data-ttu-id="3c8b8-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i BetterWorks upprättas.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-140">In other words, a link relationship between an Azure AD user and the related user in BetterWorks needs to be established.</span></span>

<span data-ttu-id="3c8b8-141">I BetterWorks, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-141">In BetterWorks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3c8b8-142">Om du vill konfigurera och testa Azure AD enkel inloggning med BetterWorks, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-142">To configure and test Azure AD single sign-on with BetterWorks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3c8b8-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3c8b8-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3c8b8-145">**[Skapa en testanvändare BetterWorks](#creating-a-betterworks-test-user)**  – du har en motsvarighet för Britta Simon i BetterWorks som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-145">**[Creating a BetterWorks test user](#creating-a-betterworks-test-user)** - to have a counterpart of Britta Simon in BetterWorks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3c8b8-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3c8b8-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3c8b8-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3c8b8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3c8b8-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt BetterWorks program.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BetterWorks application.</span></span>

<span data-ttu-id="3c8b8-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med BetterWorks:**</span><span class="sxs-lookup"><span data-stu-id="3c8b8-150">**To configure Azure AD single sign-on with BetterWorks, perform the following steps:**</span></span>

1. <span data-ttu-id="3c8b8-151">I Azure-portalen på den **BetterWorks** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-151">In the Azure portal, on the **BetterWorks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3c8b8-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_samlbase.png)

3. <span data-ttu-id="3c8b8-155">På den **BetterWorks domän och URL: er** om du vill konfigurera programmet i **IDP initierade läge**:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-155">On the **BetterWorks Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url.png)

    <span data-ttu-id="3c8b8-157">a.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-157">a.</span></span> <span data-ttu-id="3c8b8-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://app.betterworks.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="3c8b8-158">In the **Identifier** textbox, type a URL using the following pattern: `https://app.betterworks.com/saml2/metadata/`</span></span>

    <span data-ttu-id="3c8b8-159">b.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-159">b.</span></span> <span data-ttu-id="3c8b8-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://app.betterworks.com/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="3c8b8-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://app.betterworks.com/saml2/acs/`</span></span>

4. <span data-ttu-id="3c8b8-161">På den **BetterWorks domän och URL: er** om du vill konfigurera programmet i **SP initierade läge**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-161">On the **BetterWorks Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url1.png)

    <span data-ttu-id="3c8b8-163">a.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-163">a.</span></span> <span data-ttu-id="3c8b8-164">Klicka på den **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-164">Click on the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="3c8b8-165">b.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-165">b.</span></span> <span data-ttu-id="3c8b8-166">I den **logga URL** textruta Skriv en URL med följande mönster:`https://app.betterworks.com`</span><span class="sxs-lookup"><span data-stu-id="3c8b8-166">In the **Sign On URL** textbox, type a URL using the following pattern: `https://app.betterworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3c8b8-167">Detta är inte verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-167">These are not real values.</span></span> <span data-ttu-id="3c8b8-168">Uppdatera dessa värden med Reply URL, identifierare och faktiska logga URL.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-168">Update these values with the Reply URL, Identifier and actual Sign On URL.</span></span> <span data-ttu-id="3c8b8-169">Kontakta [BetterWorks supportteam](mailto:support@betterworks.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-169">Contact [BetterWorks support team](mailto:support@betterworks.com) to get these values.</span></span>
 
4. <span data-ttu-id="3c8b8-170">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-170">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_certificate.png)  

5. <span data-ttu-id="3c8b8-172">BetterWorks program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-172">BetterWorks application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="3c8b8-173">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-173">Configure the following claims for this application.</span></span> <span data-ttu-id="3c8b8-174">Du kan hantera värden för attributen från den ”**attributet**” för programmet.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-174">You can manage the values of these attributes from the "**Attribute**" tab of the application.</span></span> <span data-ttu-id="3c8b8-175">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-175">The following screenshot shows an example for this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_attribute.png)

6. <span data-ttu-id="3c8b8-177">På den **SAML-token attribut** dialogrutan för varje rad som visas i tabellen nedan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-177">On the **SAML token attributes** dialog, for each row shown in the table below, perform the following steps:</span></span>
 
   | <span data-ttu-id="3c8b8-178">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="3c8b8-178">Attribute Name</span></span> | <span data-ttu-id="3c8b8-179">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="3c8b8-179">Attribute Value</span></span> |
   | -------------- |  ------------ |
   | <span data-ttu-id="3c8b8-180">saml_token</span><span class="sxs-lookup"><span data-stu-id="3c8b8-180">saml_token</span></span>     | <span data-ttu-id="3c8b8-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span><span class="sxs-lookup"><span data-stu-id="3c8b8-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span></span> |

   <span data-ttu-id="3c8b8-182">a.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-182">a.</span></span> <span data-ttu-id="3c8b8-183">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-183">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_05.png)

   <span data-ttu-id="3c8b8-186">b.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-186">b.</span></span> <span data-ttu-id="3c8b8-187">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-187">In the **Name** textbox, type the attribute name shown for that row.</span></span> 

   <span data-ttu-id="3c8b8-188">c.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-188">c.</span></span> <span data-ttu-id="3c8b8-189">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-189">From the **Value** list, type the attribute value shown for that row.</span></span>
    
   <span data-ttu-id="3c8b8-190">d.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-190">d.</span></span> <span data-ttu-id="3c8b8-191">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-191">Click **Ok**.</span></span>

7. <span data-ttu-id="3c8b8-192">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-192">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="3c8b8-194">Konfigurera enkel inloggning på **BetterWorks** sida, måste du skicka den hämtade **XML-Metadata för** till [BetterWorks supportteam](mailto:support@betterworks.com).</span><span class="sxs-lookup"><span data-stu-id="3c8b8-194">To configure single sign-on on **BetterWorks** side, you need to send the downloaded **Metadata XML** to [BetterWorks support team](mailto:support@betterworks.com).</span></span>


> [!TIP]
> <span data-ttu-id="3c8b8-195">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3c8b8-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3c8b8-196">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3c8b8-197">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3c8b8-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3c8b8-198">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3c8b8-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="3c8b8-199">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3c8b8-201">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3c8b8-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3c8b8-202">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3c8b8-204">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3c8b8-206">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-206">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3c8b8-208">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3c8b8-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3c8b8-210">a.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-210">a.</span></span> <span data-ttu-id="3c8b8-211">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3c8b8-212">b.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-212">b.</span></span> <span data-ttu-id="3c8b8-213">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3c8b8-214">c.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-214">c.</span></span> <span data-ttu-id="3c8b8-215">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3c8b8-216">d.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-216">d.</span></span> <span data-ttu-id="3c8b8-217">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-217">Click **Create**.</span></span>
 
### <a name="creating-a-betterworks-test-user"></a><span data-ttu-id="3c8b8-218">Skapa en testanvändare BetterWorks</span><span class="sxs-lookup"><span data-stu-id="3c8b8-218">Creating a BetterWorks test user</span></span>

<span data-ttu-id="3c8b8-219">I det här avsnittet skapar du en användare som kallas Britta Simon i BetterWorks.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-219">In this section, you create a user called Britta Simon in BetterWorks.</span></span> <span data-ttu-id="3c8b8-220">Arbeta med [BetterWorks supportteam](mailto:support@betterworks.com) att lägga till användare i BetterWorks-plattformen.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-220">Work with [BetterWorks support team](mailto:support@betterworks.com) to add the users in the BetterWorks platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3c8b8-221">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3c8b8-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3c8b8-222">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till BetterWorks.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BetterWorks.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3c8b8-224">**Om du vill tilldela BetterWorks Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3c8b8-224">**To assign Britta Simon to BetterWorks, perform the following steps:**</span></span>

1. <span data-ttu-id="3c8b8-225">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3c8b8-227">Välj i listan med program **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-227">In the applications list, select **BetterWorks**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_app.png) 

3. <span data-ttu-id="3c8b8-229">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3c8b8-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-231">Click **Add** button.</span></span> <span data-ttu-id="3c8b8-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3c8b8-234">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3c8b8-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3c8b8-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3c8b8-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3c8b8-237">Testing single sign-on</span></span>

<span data-ttu-id="3c8b8-238">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-238">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3c8b8-239">När du klickar på panelen BetterWorks på åtkomstpanelen du bör få automatiskt loggat in på ditt BetterWorks program.</span><span class="sxs-lookup"><span data-stu-id="3c8b8-239">When you click the BetterWorks tile in the Access Panel, you should get automatically signed-on to your BetterWorks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c8b8-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3c8b8-240">Additional resources</span></span>

* [<span data-ttu-id="3c8b8-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3c8b8-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3c8b8-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3c8b8-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png

