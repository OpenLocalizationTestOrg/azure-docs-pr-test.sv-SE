---
title: "Självstudier: Azure Active Directory-integrering med TargetProcess | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och TargetProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: d15931a5d430252bbd9ae342e1f8fde1a539355b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a><span data-ttu-id="44f15-103">Självstudier: Azure Active Directory-integrering med TargetProcess</span><span class="sxs-lookup"><span data-stu-id="44f15-103">Tutorial: Azure Active Directory integration with TargetProcess</span></span>

<span data-ttu-id="44f15-104">I kursen får lära du att integrera TargetProcess med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="44f15-104">In this tutorial, you learn how to integrate TargetProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="44f15-105">Integrera TargetProcess med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="44f15-105">Integrating TargetProcess with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="44f15-106">Du kan styra i Azure AD som har åtkomst till TargetProcess</span><span class="sxs-lookup"><span data-stu-id="44f15-106">You can control in Azure AD who has access to TargetProcess</span></span>
- <span data-ttu-id="44f15-107">Du kan aktivera användarna att automatiskt hämta loggat in på TargetProcess (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="44f15-107">You can enable your users to automatically get signed-on to TargetProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="44f15-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="44f15-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="44f15-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="44f15-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44f15-110">Krav</span><span class="sxs-lookup"><span data-stu-id="44f15-110">Prerequisites</span></span>

<span data-ttu-id="44f15-111">För att konfigurera Azure AD-integrering med TargetProcess, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="44f15-111">To configure Azure AD integration with TargetProcess, you need the following items:</span></span>

- <span data-ttu-id="44f15-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="44f15-112">An Azure AD subscription</span></span>
- <span data-ttu-id="44f15-113">En TargetProcess enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="44f15-113">A TargetProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="44f15-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="44f15-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="44f15-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="44f15-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="44f15-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="44f15-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="44f15-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="44f15-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="44f15-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="44f15-118">Scenario description</span></span>
<span data-ttu-id="44f15-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="44f15-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="44f15-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="44f15-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="44f15-121">Lägg till TargetProcess från galleriet</span><span class="sxs-lookup"><span data-stu-id="44f15-121">Add TargetProcess from the gallery</span></span>
2. <span data-ttu-id="44f15-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44f15-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-targetprocess-from-the-gallery"></a><span data-ttu-id="44f15-123">Lägg till TargetProcess från galleriet</span><span class="sxs-lookup"><span data-stu-id="44f15-123">Add TargetProcess from the gallery</span></span>
<span data-ttu-id="44f15-124">Du måste lägga till TargetProcess från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av TargetProcess i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="44f15-124">To configure the integration of TargetProcess into Azure AD, you need to add TargetProcess from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="44f15-125">**Utför följande steg för att lägga till TargetProcess från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="44f15-125">**To add TargetProcess from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="44f15-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="44f15-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="44f15-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="44f15-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="44f15-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="44f15-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="44f15-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44f15-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="44f15-133">I sökrutan skriver **TargetProcess**väljer **TargetProcess** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="44f15-133">In the search box, type **TargetProcess**, select **TargetProcess**  from result panel then click **Add** button to add the application.</span></span>

    ![Lägg till TargetProcess från galleriet](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="44f15-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44f15-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="44f15-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TargetProcess baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="44f15-136">In this section, you configure and test Azure AD single sign-on with TargetProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="44f15-137">Azure AD måste du känna till användaren i TargetProcess motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="44f15-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TargetProcess is to a user in Azure AD.</span></span> <span data-ttu-id="44f15-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i TargetProcess upprättas.</span><span class="sxs-lookup"><span data-stu-id="44f15-138">In other words, a link relationship between an Azure AD user and the related user in TargetProcess needs to be established.</span></span>

<span data-ttu-id="44f15-139">I TargetProcess, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="44f15-139">In TargetProcess, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="44f15-140">Om du vill konfigurera och testa Azure AD enkel inloggning med TargetProcess, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="44f15-140">To configure and test Azure AD single sign-on with TargetProcess, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="44f15-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="44f15-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="44f15-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44f15-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="44f15-143">**[Skapa en testanvändare TargetProcess](#create-a-targetprocess-test-user)**  – du har en motsvarighet för Britta Simon i TargetProcess som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="44f15-143">**[Create a TargetProcess test user](#create-a-targetprocess-test-user)** - to have a counterpart of Britta Simon in TargetProcess that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="44f15-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="44f15-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="44f15-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="44f15-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="44f15-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44f15-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="44f15-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt TargetProcess program.</span><span class="sxs-lookup"><span data-stu-id="44f15-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TargetProcess application.</span></span>

<span data-ttu-id="44f15-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med TargetProcess:**</span><span class="sxs-lookup"><span data-stu-id="44f15-148">**To configure Azure AD single sign-on with TargetProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="44f15-149">I Azure-portalen på den **TargetProcess** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="44f15-149">In the Azure portal, on the **TargetProcess** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="44f15-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="44f15-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. <span data-ttu-id="44f15-153">På den **TargetProcess domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="44f15-153">On the **TargetProcess Domain and URLs** section, perform the following steps:</span></span>

    ![Avsnittet TargetProcess domän och URL: er](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    <span data-ttu-id="44f15-155">a.</span><span class="sxs-lookup"><span data-stu-id="44f15-155">a.</span></span> <span data-ttu-id="44f15-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="44f15-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    <span data-ttu-id="44f15-157">b.</span><span class="sxs-lookup"><span data-stu-id="44f15-157">b.</span></span> <span data-ttu-id="44f15-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="44f15-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="44f15-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="44f15-159">These values are not real.</span></span> <span data-ttu-id="44f15-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="44f15-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="44f15-161">Kontakta [TargetProcess klienten supportteamet](mailto:support@targetprocess.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="44f15-161">Contact [TargetProcess Client support team](mailto:support@targetprocess.com) to get these values.</span></span> 
 
4. <span data-ttu-id="44f15-162">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="44f15-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. <span data-ttu-id="44f15-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="44f15-164">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="44f15-166">På den **TargetProcess Configuration** klickar du på **konfigurera TargetProcess** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="44f15-166">On the **TargetProcess Configuration** section, click **Configure TargetProcess** to open **Configure sign-on** window.</span></span> <span data-ttu-id="44f15-167">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="44f15-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TargetProcess konfigurationsavsnitt](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. <span data-ttu-id="44f15-169">Inloggning till TargetProcess programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="44f15-169">Sign-on to your TargetProcess application as an administrator.</span></span>

8. <span data-ttu-id="44f15-170">Klicka på menyn högst upp **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="44f15-170">In the menu on the top, click **Setup**.</span></span>
   
    ![Konfiguration](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. <span data-ttu-id="44f15-172">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="44f15-172">Click **Settings**.</span></span>
   
    ![Inställningar](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. <span data-ttu-id="44f15-174">Klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="44f15-174">Click **Single Sign-on**.</span></span>
   
    ![Klicka på enkel inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. <span data-ttu-id="44f15-176">Utför följande steg på enkel inloggning dialogrutan Inställningar för:</span><span class="sxs-lookup"><span data-stu-id="44f15-176">On the Single Sign-on settings dialog, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    <span data-ttu-id="44f15-178">a.</span><span class="sxs-lookup"><span data-stu-id="44f15-178">a.</span></span> <span data-ttu-id="44f15-179">Klicka på **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="44f15-179">Click **Enable Single Sign-on**.</span></span>
    
    <span data-ttu-id="44f15-180">b.</span><span class="sxs-lookup"><span data-stu-id="44f15-180">b.</span></span> <span data-ttu-id="44f15-181">I **inloggnings-URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="44f15-181">In **Sign-on URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="44f15-182">c.</span><span class="sxs-lookup"><span data-stu-id="44f15-182">c.</span></span> <span data-ttu-id="44f15-183">Öppna din hämtat certifikat i anteckningar, kopiera innehållet och klistrar in det i den **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="44f15-183">Open your downloaded certificate in notepad, copy the content, and then paste it into the **Certificate** textbox.</span></span>
    
    <span data-ttu-id="44f15-184">d.</span><span class="sxs-lookup"><span data-stu-id="44f15-184">d.</span></span> <span data-ttu-id="44f15-185">Klicka på **aktivera JIT etablering**.</span><span class="sxs-lookup"><span data-stu-id="44f15-185">click **Enable JIT Provisioning**.</span></span>

    <span data-ttu-id="44f15-186">e.</span><span class="sxs-lookup"><span data-stu-id="44f15-186">e.</span></span> <span data-ttu-id="44f15-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="44f15-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="44f15-188">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="44f15-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="44f15-189">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="44f15-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="44f15-190">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="44f15-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="44f15-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="44f15-191">Create an Azure AD test user</span></span>
<span data-ttu-id="44f15-192">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44f15-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="44f15-194">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="44f15-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="44f15-195">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="44f15-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="44f15-197">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="44f15-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Visa en lista över användare](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="44f15-199">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44f15-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="44f15-201">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="44f15-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Avsnittet för användare](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="44f15-203">a.</span><span class="sxs-lookup"><span data-stu-id="44f15-203">a.</span></span> <span data-ttu-id="44f15-204">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="44f15-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="44f15-205">b.</span><span class="sxs-lookup"><span data-stu-id="44f15-205">b.</span></span> <span data-ttu-id="44f15-206">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="44f15-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="44f15-207">c.</span><span class="sxs-lookup"><span data-stu-id="44f15-207">c.</span></span> <span data-ttu-id="44f15-208">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="44f15-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="44f15-209">d.</span><span class="sxs-lookup"><span data-stu-id="44f15-209">d.</span></span> <span data-ttu-id="44f15-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="44f15-210">Click **Create**.</span></span>
 
### <a name="create-a-targetprocess-test-user"></a><span data-ttu-id="44f15-211">Skapa en testanvändare TargetProcess</span><span class="sxs-lookup"><span data-stu-id="44f15-211">Create a TargetProcess test user</span></span>

<span data-ttu-id="44f15-212">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="44f15-212">The objective of this section is to create a user called Britta Simon in TargetProcess.</span></span>

<span data-ttu-id="44f15-213">TargetProcess stöder just-in-time-etablering.</span><span class="sxs-lookup"><span data-stu-id="44f15-213">TargetProcess supports just-in-time provisioning.</span></span> <span data-ttu-id="44f15-214">Du har redan aktiverats i [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="44f15-214">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="44f15-215">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="44f15-215">There is no action item for you in this section.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="44f15-216">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="44f15-216">Assign the Azure AD test user</span></span>

<span data-ttu-id="44f15-217">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="44f15-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TargetProcess.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="44f15-219">**Om du vill tilldela TargetProcess Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44f15-219">**To assign Britta Simon to TargetProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="44f15-220">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="44f15-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="44f15-222">Välj i listan med program **TargetProcess**.</span><span class="sxs-lookup"><span data-stu-id="44f15-222">In the applications list, select **TargetProcess**.</span></span>

    ![TargetProcess i listan över appar](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. <span data-ttu-id="44f15-224">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="44f15-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="44f15-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="44f15-226">Click **Add** button.</span></span> <span data-ttu-id="44f15-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44f15-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="44f15-229">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="44f15-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="44f15-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44f15-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="44f15-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44f15-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="44f15-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44f15-232">Test single sign-on</span></span>

<span data-ttu-id="44f15-233">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="44f15-233">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="44f15-234">När du klickar på panelen TargetProcess på åtkomstpanelen du bör få automatiskt loggat in på ditt TargetProcess program.</span><span class="sxs-lookup"><span data-stu-id="44f15-234">When you click the TargetProcess tile in the Access Panel, you should get automatically signed-on to your TargetProcess application.</span></span> <span data-ttu-id="44f15-235">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="44f15-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="44f15-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="44f15-236">Additional resources</span></span>

* [<span data-ttu-id="44f15-237">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44f15-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="44f15-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="44f15-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png

