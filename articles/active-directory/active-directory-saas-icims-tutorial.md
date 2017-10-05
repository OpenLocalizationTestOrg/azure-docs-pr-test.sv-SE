---
title: "Självstudier: Azure Active Directory-integrering med ICIMS | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ICIMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 72dbd649-e4b1-4d72-ad76-636d84922596
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 26a6b41a0e59924d007855ca548f22ed00bd7e23
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-icims"></a><span data-ttu-id="c2217-103">Självstudier: Azure Active Directory-integrering med ICIMS</span><span class="sxs-lookup"><span data-stu-id="c2217-103">Tutorial: Azure Active Directory integration with ICIMS</span></span>

<span data-ttu-id="c2217-104">I kursen får lära du att integrera ICIMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c2217-104">In this tutorial, you learn how to integrate ICIMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c2217-105">Integrera ICIMS med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c2217-105">Integrating ICIMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c2217-106">Du kan styra i Azure AD som har åtkomst till ICIMS</span><span class="sxs-lookup"><span data-stu-id="c2217-106">You can control in Azure AD who has access to ICIMS</span></span>
- <span data-ttu-id="c2217-107">Du kan aktivera användarna att automatiskt hämta loggat in på ICIMS (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c2217-107">You can enable your users to automatically get signed-on to ICIMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c2217-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c2217-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c2217-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c2217-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2217-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c2217-110">Prerequisites</span></span>

<span data-ttu-id="c2217-111">För att konfigurera Azure AD-integrering med ICIMS, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c2217-111">To configure Azure AD integration with ICIMS, you need the following items:</span></span>

- <span data-ttu-id="c2217-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c2217-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c2217-113">En ICIMS enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c2217-113">An ICIMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c2217-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c2217-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c2217-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c2217-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c2217-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c2217-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c2217-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c2217-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c2217-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c2217-118">Scenario description</span></span>
<span data-ttu-id="c2217-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c2217-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c2217-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c2217-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c2217-121">Att lägga till ICIMS från galleriet</span><span class="sxs-lookup"><span data-stu-id="c2217-121">Adding ICIMS from the gallery</span></span>
2. <span data-ttu-id="c2217-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2217-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-icims-from-the-gallery"></a><span data-ttu-id="c2217-123">Att lägga till ICIMS från galleriet</span><span class="sxs-lookup"><span data-stu-id="c2217-123">Adding ICIMS from the gallery</span></span>
<span data-ttu-id="c2217-124">Du måste lägga till ICIMS från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av ICIMS i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c2217-124">To configure the integration of ICIMS into Azure AD, you need to add ICIMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c2217-125">**Utför följande steg för att lägga till ICIMS från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c2217-125">**To add ICIMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c2217-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c2217-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="c2217-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c2217-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c2217-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c2217-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="c2217-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2217-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="c2217-133">I sökrutan skriver **ICIMS**väljer **ICIMS** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c2217-133">In the search box, type **ICIMS**, select **ICIMS** from result panel then click **Add** button to add the application.</span></span>

    ![ICIMS i resultatlistan](./media/active-directory-saas-icims-tutorial/tutorial_icims_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c2217-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2217-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="c2217-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ICIMS baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c2217-136">In this section, you configure and test Azure AD single sign-on with ICIMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c2217-137">Azure AD måste du känna till användaren i ICIMS motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c2217-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ICIMS is to a user in Azure AD.</span></span> <span data-ttu-id="c2217-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ICIMS upprättas.</span><span class="sxs-lookup"><span data-stu-id="c2217-138">In other words, a link relationship between an Azure AD user and the related user in ICIMS needs to be established.</span></span>

<span data-ttu-id="c2217-139">I ICIMS, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c2217-139">In ICIMS, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c2217-140">Om du vill konfigurera och testa Azure AD enkel inloggning med ICIMS, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c2217-140">To configure and test Azure AD single sign-on with ICIMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c2217-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c2217-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c2217-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2217-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c2217-143">**[Skapa en testanvändare ICIMS](#create-an-icims-test-user)**  – du har en motsvarighet för Britta Simon i ICIMS som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c2217-143">**[Create an ICIMS test user](#create-an-icims-test-user)** - to have a counterpart of Britta Simon in ICIMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c2217-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2217-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c2217-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c2217-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c2217-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2217-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c2217-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt ICIMS program.</span><span class="sxs-lookup"><span data-stu-id="c2217-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ICIMS application.</span></span>

<span data-ttu-id="c2217-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ICIMS:**</span><span class="sxs-lookup"><span data-stu-id="c2217-148">**To configure Azure AD single sign-on with ICIMS, perform the following steps:**</span></span>

1. <span data-ttu-id="c2217-149">I Azure-portalen på den **ICIMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c2217-149">In the Azure portal, on the **ICIMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c2217-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2217-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-icims-tutorial/tutorial_icims_samlbase.png)

3. <span data-ttu-id="c2217-153">På den **ICIMS domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c2217-153">On the **ICIMS Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och ICIMS domän med enkel inloggning information](./media/active-directory-saas-icims-tutorial/tutorial_icims_url.png)

    <span data-ttu-id="c2217-155">a.</span><span class="sxs-lookup"><span data-stu-id="c2217-155">a.</span></span> <span data-ttu-id="c2217-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant name>.icims.com`</span><span class="sxs-lookup"><span data-stu-id="c2217-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant name>.icims.com`</span></span>

    <span data-ttu-id="c2217-157">b.</span><span class="sxs-lookup"><span data-stu-id="c2217-157">b.</span></span> <span data-ttu-id="c2217-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<tenant name>.icims.com`</span><span class="sxs-lookup"><span data-stu-id="c2217-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant name>.icims.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c2217-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c2217-159">These values are not real.</span></span> <span data-ttu-id="c2217-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="c2217-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c2217-161">Kontakta [ICIMS klienten supportteamet](https://www.icims.com/contact-us) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c2217-161">Contact [ICIMS Client support team](https://www.icims.com/contact-us) to get these values.</span></span> 
 
4. <span data-ttu-id="c2217-162">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c2217-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-icims-tutorial/tutorial_icims_certificate.png) 

5. <span data-ttu-id="c2217-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c2217-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-icims-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c2217-166">På den **ICIMS Configuration** klickar du på **konfigurera ICIMS** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c2217-166">On the **ICIMS Configuration** section, click **Configure ICIMS** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c2217-167">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c2217-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![ICIMS konfiguration](./media/active-directory-saas-icims-tutorial/tutorial_icims_configure.png) 

7. <span data-ttu-id="c2217-169">Konfigurera enkel inloggning på **ICIMS** sida, måste du skicka den hämtade **XML-Metadata för**, **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [ICIMS supportteam](https://www.icims.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="c2217-169">To configure single sign-on on **ICIMS** side, you need to send the downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [ICIMS support team](https://www.icims.com/contact-us).</span></span> <span data-ttu-id="c2217-170">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="c2217-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c2217-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c2217-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c2217-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c2217-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c2217-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c2217-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c2217-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2217-174">Create an Azure AD test user</span></span>
<span data-ttu-id="c2217-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2217-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="c2217-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c2217-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c2217-178">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c2217-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-icims-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c2217-180">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c2217-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-icims-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c2217-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2217-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-icims-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c2217-184">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c2217-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogrutan användare](./media/active-directory-saas-icims-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c2217-186">a.</span><span class="sxs-lookup"><span data-stu-id="c2217-186">a.</span></span> <span data-ttu-id="c2217-187">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c2217-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c2217-188">b.</span><span class="sxs-lookup"><span data-stu-id="c2217-188">b.</span></span> <span data-ttu-id="c2217-189">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c2217-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c2217-190">c.</span><span class="sxs-lookup"><span data-stu-id="c2217-190">c.</span></span> <span data-ttu-id="c2217-191">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c2217-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c2217-192">d.</span><span class="sxs-lookup"><span data-stu-id="c2217-192">d.</span></span> <span data-ttu-id="c2217-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c2217-193">Click **Create**.</span></span>
 
### <a name="create-an-icims-test-user"></a><span data-ttu-id="c2217-194">Skapa en testanvändare ICIMS</span><span class="sxs-lookup"><span data-stu-id="c2217-194">Create an ICIMS test user</span></span>

<span data-ttu-id="c2217-195">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i ICIMS.</span><span class="sxs-lookup"><span data-stu-id="c2217-195">The objective of this section is to create a user called Britta Simon in ICIMS.</span></span> <span data-ttu-id="c2217-196">Arbeta med [ICIMS supportteam](https://www.icims.com/contact-us) att lägga till användare i ICIMS-konto.</span><span class="sxs-lookup"><span data-stu-id="c2217-196">Work with [ICIMS support team](https://www.icims.com/contact-us) to add the users in the ICIMS account.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c2217-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c2217-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="c2217-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ICIMS.</span><span class="sxs-lookup"><span data-stu-id="c2217-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ICIMS.</span></span>

![Tilldela rollen][200]

<span data-ttu-id="c2217-200">**Om du vill tilldela ICIMS Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2217-200">**To assign Britta Simon to ICIMS, perform the following steps:**</span></span>

1. <span data-ttu-id="c2217-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c2217-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c2217-203">Välj i listan med program **ICIMS**.</span><span class="sxs-lookup"><span data-stu-id="c2217-203">In the applications list, select **ICIMS**.</span></span>

    ![Länken ICIMS i listan med program](./media/active-directory-saas-icims-tutorial/tutorial_icims_app.png) 

3. <span data-ttu-id="c2217-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c2217-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202] 

4. <span data-ttu-id="c2217-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c2217-207">Click **Add** button.</span></span> <span data-ttu-id="c2217-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2217-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="c2217-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c2217-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c2217-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2217-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c2217-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2217-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c2217-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2217-213">Test single sign-on</span></span>

<span data-ttu-id="c2217-214">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c2217-214">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="c2217-215">När du klickar på panelen ICIMS på åtkomstpanelen du bör få automatiskt loggat in på ditt ICIMS program.</span><span class="sxs-lookup"><span data-stu-id="c2217-215">When you click the ICIMS tile in the Access Panel, you should get automatically signed-on to your ICIMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2217-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c2217-216">Additional resources</span></span>

* [<span data-ttu-id="c2217-217">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c2217-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c2217-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c2217-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icims-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icims-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icims-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icims-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-icims-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icims-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icims-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-icims-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-icims-tutorial/tutorial_general_203.png

