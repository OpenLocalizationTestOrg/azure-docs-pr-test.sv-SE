---
title: "Självstudier: Azure Active Directory-integrering med Zscaler privat åtkomst (ZPA) | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zscaler privat åtkomst (ZPA)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 5c598bfa5b6725d21a89df54dbcb3314cc631d80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a><span data-ttu-id="c4dc7-103">Självstudier: Azure Active Directory-integrering med Zscaler privat åtkomst (ZPA)</span><span class="sxs-lookup"><span data-stu-id="c4dc7-103">Tutorial: Azure Active Directory integration with Zscaler Private Access (ZPA)</span></span>

<span data-ttu-id="c4dc7-104">I kursen får lära du att integrera Zscaler privat åtkomst (ZPA) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c4dc7-104">In this tutorial, you learn how to integrate Zscaler Private Access (ZPA) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c4dc7-105">Integrera Zscaler privat åtkomst (ZPA) med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c4dc7-105">Integrating Zscaler Private Access (ZPA) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c4dc7-106">Du kan styra i Azure AD som har åtkomst till Zscaler privat åtkomst (ZPA)</span><span class="sxs-lookup"><span data-stu-id="c4dc7-106">You can control in Azure AD who has access to Zscaler Private Access (ZPA)</span></span>
- <span data-ttu-id="c4dc7-107">Du kan aktivera användarna att automatiskt hämta inloggade till Zscaler privat åtkomst (ZPA) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c4dc7-107">You can enable your users to automatically get signed-on to Zscaler Private Access (ZPA) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c4dc7-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="c4dc7-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="c4dc7-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c4dc7-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4dc7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c4dc7-110">Prerequisites</span></span>

<span data-ttu-id="c4dc7-111">Om du vill konfigurera Azure AD-integrering med Zscaler privat åtkomst (ZPA) behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c4dc7-111">To configure Azure AD integration with Zscaler Private Access (ZPA), you need the following items:</span></span>

- <span data-ttu-id="c4dc7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c4dc7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c4dc7-113">En Zscaler privat åtkomst (ZPA) enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c4dc7-113">A Zscaler Private Access (ZPA) single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="c4dc7-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="c4dc7-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c4dc7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c4dc7-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="c4dc7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c4dc7-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="c4dc7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c4dc7-118">Scenario description</span></span>
<span data-ttu-id="c4dc7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c4dc7-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c4dc7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c4dc7-121">Att lägga till Zscaler privat åtkomst (ZPA) från galleriet</span><span class="sxs-lookup"><span data-stu-id="c4dc7-121">Adding Zscaler Private Access (ZPA) from the gallery</span></span>
2. <span data-ttu-id="c4dc7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c4dc7-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a><span data-ttu-id="c4dc7-123">Att lägga till Zscaler privat åtkomst (ZPA) från galleriet</span><span class="sxs-lookup"><span data-stu-id="c4dc7-123">Adding Zscaler Private Access (ZPA) from the gallery</span></span>
<span data-ttu-id="c4dc7-124">Du måste lägga till Zscaler privat åtkomst (ZPA) från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Zscaler privat åtkomst (ZPA) i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-124">To configure the integration of Zscaler Private Access (ZPA) into Azure AD, you need to add Zscaler Private Access (ZPA) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c4dc7-125">**Utför följande steg för att lägga till Zscaler privat åtkomst (ZPA) från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c4dc7-125">**To add Zscaler Private Access (ZPA) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c4dc7-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c4dc7-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c4dc7-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c4dc7-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c4dc7-133">I sökrutan skriver **Zscaler privat åtkomst (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-133">In the search box, type **Zscaler Private Access (ZPA)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. <span data-ttu-id="c4dc7-135">Välj i resultatpanelen **Zscaler privat åtkomst (ZPA)**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-135">In the results panel, select **Zscaler Private Access (ZPA)**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c4dc7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c4dc7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c4dc7-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-138">In this section, you configure and test Azure AD single sign-on with Zscaler Private Access (ZPA) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c4dc7-139">Azure AD måste du känna till motsvarande användaren i Zscaler privat åtkomst (ZPA) till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler Private Access (ZPA) is to a user in Azure AD.</span></span> <span data-ttu-id="c4dc7-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Zscaler privat åtkomst (ZPA) upprättas.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler Private Access (ZPA) needs to be established.</span></span>

<span data-ttu-id="c4dc7-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Zscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="c4dc7-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zscaler Private Access (ZPA).</span></span>

<span data-ttu-id="c4dc7-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA), måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c4dc7-142">To configure and test Azure AD single sign-on with Zscaler Private Access (ZPA), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c4dc7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c4dc7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c4dc7-145">**[Skapa en testanvändare Zscaler privat åtkomst (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**  – du har en motsvarighet för Britta Simon i Zscaler privat åtkomst (ZPA) som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-145">**[Creating a Zscaler Private Access (ZPA) test user](#creating-a-zscaler-private-access-(zpa)-test-user)** - to have a counterpart of Britta Simon in Zscaler Private Access (ZPA) that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="c4dc7-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c4dc7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c4dc7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c4dc7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c4dc7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i tillämpningsprogrammet Zscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="c4dc7-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Zscaler Private Access (ZPA) application.</span></span>

<span data-ttu-id="c4dc7-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA):**</span><span class="sxs-lookup"><span data-stu-id="c4dc7-150">**To configure Azure AD single sign-on with Zscaler Private Access (ZPA), perform the following steps:**</span></span>

1. <span data-ttu-id="c4dc7-151">I Azure-hanteringsportalen på den **Zscaler privat åtkomst (ZPA)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-151">In the Azure Management portal, on the **Zscaler Private Access (ZPA)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c4dc7-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="c4dc7-155">På den **Zscaler privat åtkomst (ZPA)-domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c4dc7-155">On the **Zscaler Private Access (ZPA) Domain and URLs** section, perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    <span data-ttu-id="c4dc7-157">a.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-157">a.</span></span> <span data-ttu-id="c4dc7-158">I den **logga URL** textruta Skriv en URL med följande mönster:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span><span class="sxs-lookup"><span data-stu-id="c4dc7-158">In the **Sign On URL** textbox, type a URL using the following pattern: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span></span>

    <span data-ttu-id="c4dc7-159">b.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-159">b.</span></span> <span data-ttu-id="c4dc7-160">I den **identifierare** textruta typ:`https://samlsp.private.zscaler.com/auth/metadata`</span><span class="sxs-lookup"><span data-stu-id="c4dc7-160">In the **Identifier** textbox, type: `https://samlsp.private.zscaler.com/auth/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c4dc7-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-161">Please note that these are not the real values.</span></span> <span data-ttu-id="c4dc7-162">Du måste uppdatera dessa värden med den faktiska logga URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-162">You have to update these values with the actual Sign On URL and Identifier.</span></span> <span data-ttu-id="c4dc7-163">Här rekommenderar vi att du om du vill använda det unika värdet för URL: en i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-163">Here we suggest you to use the unique value of URL in the Identifier.</span></span> <span data-ttu-id="c4dc7-164">Kontakta [Zscaler privat åtkomst (ZPA) supportteamet](https://help.zscaler.com/zpa-submit-ticket) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-164">Contact [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) to get these values.</span></span>

4. <span data-ttu-id="c4dc7-165">På den **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-165">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. <span data-ttu-id="c4dc7-167">På den **skapa nya certifikat** dialogrutan, klicka på kalenderikonen och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-167">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="c4dc7-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-168">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="c4dc7-170">På den **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-170">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. <span data-ttu-id="c4dc7-172">På popup-fönstret **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-172">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="c4dc7-174">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-174">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. <span data-ttu-id="c4dc7-176">Logga in på webbplatsen för företagets Zscaler privat åtkomst (ZPA) som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-176">In a different web browser window, log into your Zscaler Private Access (ZPA) company site as an administrator.</span></span>

10. <span data-ttu-id="c4dc7-177">Gå till **administratör** och klicka sedan på **Idp Configuration**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-177">Navigate to **Administrator** and then click **Idp Configuration**.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. <span data-ttu-id="c4dc7-179">I den **Idp Configuration** klickar du på **lägga till nya IDP konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-179">In the **Idp Configuration** section, click **Add New IDP Configuration**.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. <span data-ttu-id="c4dc7-181">I den **nya IDP konfigurationen** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c4dc7-181">In the **New IDP Configuration** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    <span data-ttu-id="c4dc7-183">a.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-183">a.</span></span> <span data-ttu-id="c4dc7-184">Klicka på **Välj fil** och ladda upp din hämtade metadatafil.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-184">Click **Select File** and upload your downloaded metadata file.</span></span>

    <span data-ttu-id="c4dc7-185">b.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-185">b.</span></span> <span data-ttu-id="c4dc7-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-186">Click **Save** button.</span></span>
    


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c4dc7-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4dc7-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="c4dc7-188">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-188">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c4dc7-190">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c4dc7-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c4dc7-191">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-191">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c4dc7-193">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-193">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c4dc7-195">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-195">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c4dc7-197">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c4dc7-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c4dc7-199">a.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-199">a.</span></span> <span data-ttu-id="c4dc7-200">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c4dc7-201">b.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-201">b.</span></span> <span data-ttu-id="c4dc7-202">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c4dc7-203">c.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-203">c.</span></span> <span data-ttu-id="c4dc7-204">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c4dc7-205">d.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-205">d.</span></span> <span data-ttu-id="c4dc7-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-206">Click **Create**.</span></span> 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a><span data-ttu-id="c4dc7-207">Skapa en testanvändare Zscaler privat åtkomst (ZPA)</span><span class="sxs-lookup"><span data-stu-id="c4dc7-207">Creating a Zscaler Private Access (ZPA) test user</span></span>

<span data-ttu-id="c4dc7-208">I det här avsnittet skapar du en användare som kallas Britta Simon i Zscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="c4dc7-208">In this section, you create a user called Britta Simon in Zscaler Private Access (ZPA).</span></span> <span data-ttu-id="c4dc7-209">Se tillsammans med [Zscaler privat åtkomst (ZPA) supportteamet](https://help.zscaler.com/zpa-submit-ticket) att lägga till användare i Zscaler privat åtkomst (ZPA)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-209">Please work with [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) to add the users in the Zscaler Private Access (ZPA) platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c4dc7-210">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c4dc7-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c4dc7-211">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning med sin åtkomst beviljas till Zscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="c4dc7-211">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Zscaler Private Access (ZPA).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c4dc7-213">**Om du vill tilldela Britta Simon till Zscaler privat åtkomst (ZPA), utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c4dc7-213">**To assign Britta Simon to Zscaler Private Access (ZPA), perform the following steps:**</span></span>

1. <span data-ttu-id="c4dc7-214">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-214">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c4dc7-216">Välj i listan med program **Zscaler privat åtkomst (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-216">In the applications list, select **Zscaler Private Access (ZPA)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. <span data-ttu-id="c4dc7-218">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c4dc7-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-220">Click **Add** button.</span></span> <span data-ttu-id="c4dc7-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c4dc7-223">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c4dc7-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c4dc7-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="c4dc7-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c4dc7-226">Testing single sign-on</span></span>

<span data-ttu-id="c4dc7-227">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c4dc7-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c4dc7-228">När du klickar på panelen Zscaler privat åtkomst (ZPA) på åtkomstpanelen du bör få automatiskt loggat in på ditt program Zscaler privat åtkomst (ZPA).</span><span class="sxs-lookup"><span data-stu-id="c4dc7-228">When you click the Zscaler Private Access (ZPA) tile in the Access Panel, you should get automatically signed-on to your Zscaler Private Access (ZPA) application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c4dc7-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c4dc7-229">Additional resources</span></span>

* [<span data-ttu-id="c4dc7-230">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4dc7-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c4dc7-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c4dc7-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png