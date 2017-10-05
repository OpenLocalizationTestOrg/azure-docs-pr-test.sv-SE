---
title: "Självstudier: Azure Active Directory-integrering med UNIFI | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och UNIFI."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 09074d4628825909f0bb961c8001e53fb06cf7c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a><span data-ttu-id="22cd5-103">Självstudier: Azure Active Directory-integrering med UNIFI</span><span class="sxs-lookup"><span data-stu-id="22cd5-103">Tutorial: Azure Active Directory integration with UNIFI</span></span>

<span data-ttu-id="22cd5-104">I kursen får lära du att integrera UNIFI med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="22cd5-104">In this tutorial, you learn how to integrate UNIFI with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="22cd5-105">Integrera UNIFI med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="22cd5-105">Integrating UNIFI with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="22cd5-106">Du kan styra i Azure AD som har åtkomst till UNIFI</span><span class="sxs-lookup"><span data-stu-id="22cd5-106">You can control in Azure AD who has access to UNIFI</span></span>
- <span data-ttu-id="22cd5-107">Du kan aktivera användarna att automatiskt hämta loggat in på UNIFI (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="22cd5-107">You can enable your users to automatically get signed-on to UNIFI (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="22cd5-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="22cd5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="22cd5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="22cd5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22cd5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="22cd5-110">Prerequisites</span></span>

<span data-ttu-id="22cd5-111">För att konfigurera Azure AD-integrering med UNIFI, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="22cd5-111">To configure Azure AD integration with UNIFI, you need the following items:</span></span>

- <span data-ttu-id="22cd5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="22cd5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="22cd5-113">En UNIFI enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="22cd5-113">A UNIFI single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="22cd5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="22cd5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="22cd5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="22cd5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="22cd5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="22cd5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="22cd5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="22cd5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="22cd5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="22cd5-118">Scenario description</span></span>
<span data-ttu-id="22cd5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="22cd5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="22cd5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="22cd5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="22cd5-121">Att lägga till UNIFI från galleriet</span><span class="sxs-lookup"><span data-stu-id="22cd5-121">Adding UNIFI from the gallery</span></span>
2. <span data-ttu-id="22cd5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22cd5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-unifi-from-the-gallery"></a><span data-ttu-id="22cd5-123">Att lägga till UNIFI från galleriet</span><span class="sxs-lookup"><span data-stu-id="22cd5-123">Adding UNIFI from the gallery</span></span>
<span data-ttu-id="22cd5-124">Du måste lägga till UNIFI från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av UNIFI i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22cd5-124">To configure the integration of UNIFI into Azure AD, you need to add UNIFI from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="22cd5-125">**Utför följande steg för att lägga till UNIFI från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="22cd5-125">**To add UNIFI from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="22cd5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="22cd5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="22cd5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="22cd5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="22cd5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22cd5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="22cd5-133">I sökrutan skriver **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-133">In the search box, type **UNIFI**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_search.png)

5. <span data-ttu-id="22cd5-135">Välj i resultatpanelen **UNIFI**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="22cd5-135">In the results panel, select **UNIFI**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="22cd5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22cd5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="22cd5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med UNIFI baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="22cd5-138">In this section, you configure and test Azure AD single sign-on with UNIFI based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="22cd5-139">Azure AD måste du känna till användaren i UNIFI motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="22cd5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in UNIFI is to a user in Azure AD.</span></span> <span data-ttu-id="22cd5-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i UNIFI upprättas.</span><span class="sxs-lookup"><span data-stu-id="22cd5-140">In other words, a link relationship between an Azure AD user and the related user in UNIFI needs to be established.</span></span>

<span data-ttu-id="22cd5-141">I UNIFI, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="22cd5-141">In UNIFI, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="22cd5-142">Om du vill konfigurera och testa Azure AD enkel inloggning med UNIFI, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="22cd5-142">To configure and test Azure AD single sign-on with UNIFI, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="22cd5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="22cd5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="22cd5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22cd5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="22cd5-145">**[Skapa en UNIFI testanvändare](#creating-a-unifi-test-user)**  – du har en motsvarighet för Britta Simon i UNIFI som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="22cd5-145">**[Creating a UNIFI test user](#creating-a-unifi-test-user)** - to have a counterpart of Britta Simon in UNIFI that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="22cd5-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="22cd5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="22cd5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="22cd5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="22cd5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22cd5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="22cd5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt UNIFI program.</span><span class="sxs-lookup"><span data-stu-id="22cd5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your UNIFI application.</span></span>

<span data-ttu-id="22cd5-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med UNIFI:**</span><span class="sxs-lookup"><span data-stu-id="22cd5-150">**To configure Azure AD single sign-on with UNIFI, perform the following steps:**</span></span>

1. <span data-ttu-id="22cd5-151">I Azure-portalen på den **UNIFI** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-151">In the Azure portal, on the **UNIFI** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="22cd5-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="22cd5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_samlbase.png)

3. <span data-ttu-id="22cd5-155">På den **UNIFI domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="22cd5-155">On the **UNIFI Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url1.png)

    <span data-ttu-id="22cd5-157">I den **identifierare** textruta Skriv värdet:`INVIEWlabs`</span><span class="sxs-lookup"><span data-stu-id="22cd5-157">In the **Identifier** textbox, type the value: `INVIEWlabs`</span></span> 

4. <span data-ttu-id="22cd5-158">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="22cd5-158">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url2.png)

    <span data-ttu-id="22cd5-160">I den **inloggnings-URL** textruta anger du URL:`https://app.discoverunifi.com/login`</span><span class="sxs-lookup"><span data-stu-id="22cd5-160">In the **Sign-on URL** textbox, type the URL: `https://app.discoverunifi.com/login`</span></span>

5. <span data-ttu-id="22cd5-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="22cd5-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_certificate.png) 

6. <span data-ttu-id="22cd5-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="22cd5-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="22cd5-165">På den **UNIFI Configuration** klickar du på **konfigurera UNIFI** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="22cd5-165">On the **UNIFI Configuration** section, click **Configure UNIFI** to open **Configure sign-on** window.</span></span> <span data-ttu-id="22cd5-166">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="22cd5-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_configure.png)

8. <span data-ttu-id="22cd5-168">I en annan webbläsarfönstret, logga in på ditt **UNIFI** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="22cd5-168">In a different web browser window, sign on to your **UNIFI** company site as administrator.</span></span>

9. <span data-ttu-id="22cd5-169">Klicka på den **användare**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-169">Click on the **Users**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app1.png) 

10. <span data-ttu-id="22cd5-171">Klicka på den **lägga till nya identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-171">Click on the **Add New Identity Provider**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app2.png)

11. <span data-ttu-id="22cd5-173">I den **lägga till identitetsleverantör** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="22cd5-173">In the **Add Identity Provider** section, perform the following steps:</span></span>   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app3.png) 

    <span data-ttu-id="22cd5-175">a.</span><span class="sxs-lookup"><span data-stu-id="22cd5-175">a.</span></span> <span data-ttu-id="22cd5-176">I den **providernamn** textruta skriver du namnet på den identitetsleverantör...</span><span class="sxs-lookup"><span data-stu-id="22cd5-176">In the **Provider Name** textbox, type the name of the Identity Provider..</span></span>

    <span data-ttu-id="22cd5-177">b.</span><span class="sxs-lookup"><span data-stu-id="22cd5-177">b.</span></span> <span data-ttu-id="22cd5-178">I den den **providern URL** textruta klistra in den **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="22cd5-178">In the the **Provider URL** textbox paste the **SAML Single Sign-On Service URL** value, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="22cd5-179">c.</span><span class="sxs-lookup"><span data-stu-id="22cd5-179">c.</span></span> <span data-ttu-id="22cd5-180">Öppna certifikatet som du har hämtat från Azure-portalen i anteckningar, ta bort den **---BEGIN CERTIFICATE---** och **---END CERTIFICATE---** tagga och klistra in det återstående innehållet i **Certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="22cd5-180">Open the Certificate that you have downloaded from the Azure portal in notepad, remove the **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste the remaining content in the **Certificate** textbox.</span></span>

    <span data-ttu-id="22cd5-181">d.</span><span class="sxs-lookup"><span data-stu-id="22cd5-181">d.</span></span> <span data-ttu-id="22cd5-182">Välj den **är som standard Provider** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="22cd5-182">Select the **is Default Provider** checkbox.</span></span>

> [!TIP]
> <span data-ttu-id="22cd5-183">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="22cd5-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="22cd5-184">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="22cd5-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="22cd5-185">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="22cd5-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="22cd5-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="22cd5-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="22cd5-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22cd5-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="22cd5-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="22cd5-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="22cd5-190">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="22cd5-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="22cd5-192">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="22cd5-194">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22cd5-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="22cd5-196">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="22cd5-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="22cd5-198">a.</span><span class="sxs-lookup"><span data-stu-id="22cd5-198">a.</span></span> <span data-ttu-id="22cd5-199">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="22cd5-200">b.</span><span class="sxs-lookup"><span data-stu-id="22cd5-200">b.</span></span> <span data-ttu-id="22cd5-201">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="22cd5-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="22cd5-202">c.</span><span class="sxs-lookup"><span data-stu-id="22cd5-202">c.</span></span> <span data-ttu-id="22cd5-203">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="22cd5-204">d.</span><span class="sxs-lookup"><span data-stu-id="22cd5-204">d.</span></span> <span data-ttu-id="22cd5-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-205">Click **Create**.</span></span>
 
### <a name="creating-a-unifi-test-user"></a><span data-ttu-id="22cd5-206">Skapa en testanvändare UNIFI</span><span class="sxs-lookup"><span data-stu-id="22cd5-206">Creating a UNIFI test user</span></span>

<span data-ttu-id="22cd5-207">I det här avsnittet skapar du en användare som kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22cd5-207">In this section, you create a user called Britta Simon.</span></span> <span data-ttu-id="22cd5-208">**UNIFI** stöder automatisk användaretablering, så inga manuella steg krävs.</span><span class="sxs-lookup"><span data-stu-id="22cd5-208">**UNIFI** supports automatic user provisioning so no manual steps are required.</span></span> <span data-ttu-id="22cd5-209">Användare skapas automatiskt efter en lyckad autentisering från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22cd5-209">Users are created automatically after successful authentication from the Azure AD.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="22cd5-210">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="22cd5-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="22cd5-211">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till UNIFI.</span><span class="sxs-lookup"><span data-stu-id="22cd5-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to UNIFI.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="22cd5-213">**Om du vill tilldela UNIFI Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="22cd5-213">**To assign Britta Simon to UNIFI, perform the following steps:**</span></span>

1. <span data-ttu-id="22cd5-214">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="22cd5-216">Välj i listan med program **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-216">In the applications list, select **UNIFI**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_app.png) 

3. <span data-ttu-id="22cd5-218">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="22cd5-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="22cd5-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="22cd5-220">Click **Add** button.</span></span> <span data-ttu-id="22cd5-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22cd5-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="22cd5-223">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="22cd5-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="22cd5-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22cd5-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="22cd5-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22cd5-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="22cd5-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22cd5-226">Testing single sign-on</span></span>

<span data-ttu-id="22cd5-227">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="22cd5-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="22cd5-228">När du klickar på panelen UNIFI på åtkomstpanelen du bör få automatiskt loggat in på ditt UNIFI program.</span><span class="sxs-lookup"><span data-stu-id="22cd5-228">When you click the UNIFI tile in the Access Panel, you should get automatically signed-on to your UNIFI application.</span></span>
<span data-ttu-id="22cd5-229">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="22cd5-229">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="22cd5-230">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="22cd5-230">Additional resources</span></span>

* [<span data-ttu-id="22cd5-231">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="22cd5-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="22cd5-232">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="22cd5-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_203.png

