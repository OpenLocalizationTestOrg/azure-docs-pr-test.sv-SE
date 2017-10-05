---
title: "Självstudier: Azure Active Directory-integrering med PurelyHR | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och PurelyHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: a9075b1759ebd39f164bfe288fb0a365acdcc44c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a><span data-ttu-id="4f220-103">Självstudier: Azure Active Directory-integrering med PurelyHR</span><span class="sxs-lookup"><span data-stu-id="4f220-103">Tutorial: Azure Active Directory integration with PurelyHR</span></span>

<span data-ttu-id="4f220-104">I kursen får lära du att integrera PurelyHR med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4f220-104">In this tutorial, you learn how to integrate PurelyHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4f220-105">Integrera PurelyHR med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4f220-105">Integrating PurelyHR with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4f220-106">Du kan styra i Azure AD som har åtkomst till PurelyHR</span><span class="sxs-lookup"><span data-stu-id="4f220-106">You can control in Azure AD who has access to PurelyHR</span></span>
- <span data-ttu-id="4f220-107">Du kan aktivera användarna att automatiskt hämta loggat in på PurelyHR (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4f220-107">You can enable your users to automatically get signed-on to PurelyHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4f220-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4f220-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4f220-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4f220-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f220-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4f220-110">Prerequisites</span></span>

<span data-ttu-id="4f220-111">För att konfigurera Azure AD-integrering med PurelyHR, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="4f220-111">To configure Azure AD integration with PurelyHR, you need the following items:</span></span>

- <span data-ttu-id="4f220-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4f220-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4f220-113">En PurelyHR enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="4f220-113">A PurelyHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4f220-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4f220-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4f220-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4f220-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4f220-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4f220-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4f220-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4f220-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4f220-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4f220-118">Scenario description</span></span>
<span data-ttu-id="4f220-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4f220-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4f220-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4f220-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4f220-121">Att lägga till PurelyHR från galleriet</span><span class="sxs-lookup"><span data-stu-id="4f220-121">Adding PurelyHR from the gallery</span></span>
2. <span data-ttu-id="4f220-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f220-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-purelyhr-from-the-gallery"></a><span data-ttu-id="4f220-123">Att lägga till PurelyHR från galleriet</span><span class="sxs-lookup"><span data-stu-id="4f220-123">Adding PurelyHR from the gallery</span></span>
<span data-ttu-id="4f220-124">Du måste lägga till PurelyHR från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av PurelyHR i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4f220-124">To configure the integration of PurelyHR into Azure AD, you need to add PurelyHR from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4f220-125">**Utför följande steg för att lägga till PurelyHR från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="4f220-125">**To add PurelyHR from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4f220-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4f220-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4f220-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4f220-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4f220-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4f220-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4f220-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f220-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4f220-133">I sökrutan skriver **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="4f220-133">In the search box, type **PurelyHR**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_search.png)

5. <span data-ttu-id="4f220-135">Välj i resultatpanelen **PurelyHR**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="4f220-135">In the results panel, select **PurelyHR**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4f220-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f220-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4f220-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PurelyHR baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4f220-138">In this section, you configure and test Azure AD single sign-on with PurelyHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4f220-139">Azure AD måste du känna till användaren i PurelyHR motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="4f220-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PurelyHR is to a user in Azure AD.</span></span> <span data-ttu-id="4f220-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i PurelyHR upprättas.</span><span class="sxs-lookup"><span data-stu-id="4f220-140">In other words, a link relationship between an Azure AD user and the related user in PurelyHR needs to be established.</span></span>

<span data-ttu-id="4f220-141">I PurelyHR, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4f220-141">In PurelyHR, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4f220-142">Om du vill konfigurera och testa Azure AD enkel inloggning med PurelyHR, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4f220-142">To configure and test Azure AD single sign-on with PurelyHR, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4f220-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4f220-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4f220-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4f220-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4f220-145">**[Skapa en testanvändare PurelyHR](#creating-a-purelyhr-test-user)**  – du har en motsvarighet för Britta Simon i PurelyHR som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4f220-145">**[Creating a PurelyHR test user](#creating-a-purelyhr-test-user)** - to have a counterpart of Britta Simon in PurelyHR that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4f220-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4f220-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4f220-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4f220-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4f220-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f220-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4f220-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt PurelyHR program.</span><span class="sxs-lookup"><span data-stu-id="4f220-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PurelyHR application.</span></span>

<span data-ttu-id="4f220-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med PurelyHR:**</span><span class="sxs-lookup"><span data-stu-id="4f220-150">**To configure Azure AD single sign-on with PurelyHR, perform the following steps:**</span></span>

1. <span data-ttu-id="4f220-151">I Azure-portalen på den **PurelyHR** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4f220-151">In the Azure portal, on the **PurelyHR** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4f220-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4f220-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

3. <span data-ttu-id="4f220-155">På den **PurelyHR domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="4f220-155">On the **PurelyHR Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    <span data-ttu-id="4f220-157">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<companyID>.purelyhr.com/sso-consume`</span><span class="sxs-lookup"><span data-stu-id="4f220-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyID>.purelyhr.com/sso-consume`</span></span>

4. <span data-ttu-id="4f220-158">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="4f220-158">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    <span data-ttu-id="4f220-160">I den **inloggnings-URL** textruta Skriv det värde som använder följande mönster:`https://<companyID>.purelyhr.com/sso-initiate`</span><span class="sxs-lookup"><span data-stu-id="4f220-160">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<companyID>.purelyhr.com/sso-initiate`</span></span>
     
    > [!NOTE]
    > <span data-ttu-id="4f220-161">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="4f220-161">These values are not the real.</span></span> <span data-ttu-id="4f220-162">Uppdatera dessa värden med den faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="4f220-162">Update these values with the actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="4f220-163">Kontakta [PurelyHR klienten supportteamet](http://support.purelyhr.com/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4f220-163">Contact [PurelyHR Client support team](http://support.purelyhr.com/) to get these values.</span></span> 

5. <span data-ttu-id="4f220-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="4f220-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

6. <span data-ttu-id="4f220-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4f220-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="4f220-168">På den **PurelyHR Configuration** klickar du på **konfigurera PurelyHR** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4f220-168">On the **PurelyHR Configuration** section, click **Configure PurelyHR** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4f220-169">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4f220-169">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_configure.png) 

8. <span data-ttu-id="4f220-171">Konfigurera enkel inloggning på **PurelyHR** sida, logga in på webbplatsen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="4f220-171">To configure single sign-on on **PurelyHR** side, login to their website as an administrator.</span></span>

9. <span data-ttu-id="4f220-172">Öppna den **instrumentpanelen** från alternativen i verktygsfältet och på **SSO-inställningarna**.</span><span class="sxs-lookup"><span data-stu-id="4f220-172">Open the **Dashboard** from the options in the toolbar and click **SSO Settings**.</span></span>

10. <span data-ttu-id="4f220-173">Klistra in värden i rutorna enligt beskrivningen nedan-</span><span class="sxs-lookup"><span data-stu-id="4f220-173">Paste the values in the boxes as described below-</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)    

    <span data-ttu-id="4f220-175">a.</span><span class="sxs-lookup"><span data-stu-id="4f220-175">a.</span></span> <span data-ttu-id="4f220-176">Öppna den **Certificate(Bas64)** hämtas från Azure-portalen i anteckningar och kopiera certifikatvärdet.</span><span class="sxs-lookup"><span data-stu-id="4f220-176">Open the **Certificate(Bas64)** downloaded from the Azure portal in notepad and copy the certificate value.</span></span> <span data-ttu-id="4f220-177">Klistra in det kopierade värdet till den **X.509-certifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="4f220-177">Paste the copied value into the **X.509 Certificate** box.</span></span>

    <span data-ttu-id="4f220-178">b.</span><span class="sxs-lookup"><span data-stu-id="4f220-178">b.</span></span> <span data-ttu-id="4f220-179">I den **Idp utfärdar-URL** klistra in den **SAML enhets-ID** kopieras från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4f220-179">In the **Idp Issuer URL** box, paste the **SAML Entity ID** copied from the Azure portal.</span></span>

    <span data-ttu-id="4f220-180">c.</span><span class="sxs-lookup"><span data-stu-id="4f220-180">c.</span></span> <span data-ttu-id="4f220-181">I den **Idp slutpunkts-URL** klistra in den **SAML inloggning tjänst-URL för enkel** kopieras från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4f220-181">In the **Idp Endpoint URL** box, paste the **SAML Single Sign-On Service URL** copied from the Azure portal.</span></span> 

    <span data-ttu-id="4f220-182">d.</span><span class="sxs-lookup"><span data-stu-id="4f220-182">d.</span></span> <span data-ttu-id="4f220-183">Kontrollera den **skapa automatiskt användare** kryssrutan för att aktivera automatisk användaretablering i PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="4f220-183">Check the **Auto-Create Users** checkbox to enable automatic user provisioning in PurelyHR.</span></span>

    <span data-ttu-id="4f220-184">e.</span><span class="sxs-lookup"><span data-stu-id="4f220-184">e.</span></span> <span data-ttu-id="4f220-185">Klicka på **spara ändringar** spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="4f220-185">Click **Save Changes** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="4f220-186">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="4f220-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4f220-187">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="4f220-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4f220-188">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4f220-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4f220-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4f220-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="4f220-190">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4f220-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4f220-192">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="4f220-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4f220-193">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4f220-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4f220-195">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4f220-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4f220-197">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f220-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4f220-199">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4f220-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4f220-201">a.</span><span class="sxs-lookup"><span data-stu-id="4f220-201">a.</span></span> <span data-ttu-id="4f220-202">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4f220-202">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4f220-203">b.</span><span class="sxs-lookup"><span data-stu-id="4f220-203">b.</span></span> <span data-ttu-id="4f220-204">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4f220-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4f220-205">c.</span><span class="sxs-lookup"><span data-stu-id="4f220-205">c.</span></span> <span data-ttu-id="4f220-206">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4f220-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4f220-207">d.</span><span class="sxs-lookup"><span data-stu-id="4f220-207">d.</span></span> <span data-ttu-id="4f220-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4f220-208">Click **Create**.</span></span>
 
### <a name="creating-a-purelyhr-test-user"></a><span data-ttu-id="4f220-209">Skapa en testanvändare PurelyHR</span><span class="sxs-lookup"><span data-stu-id="4f220-209">Creating a PurelyHR test user</span></span>

<span data-ttu-id="4f220-210">Om du vill aktivera Azure AD-användare kan logga in på PurelyHR etableras de i PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="4f220-210">To enable Azure AD users to log in to PurelyHR, they must be provisioned into PurelyHR.</span></span> <span data-ttu-id="4f220-211">Etablering är en automatisk uppgift i PurelyHR, och inga manuella steg krävs när automatisk användaretablering är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="4f220-211">In PurelyHR, provisioning is an automatic task and no manual steps are required when automatic user provisioning is enabled.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4f220-212">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="4f220-212">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4f220-213">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="4f220-213">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PurelyHR.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4f220-215">**Om du vill tilldela PurelyHR Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4f220-215">**To assign Britta Simon to PurelyHR, perform the following steps:**</span></span>

1. <span data-ttu-id="4f220-216">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4f220-216">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4f220-218">Välj i listan med program **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="4f220-218">In the applications list, select **PurelyHR**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_app.png) 

3. <span data-ttu-id="4f220-220">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4f220-220">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4f220-222">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4f220-222">Click **Add** button.</span></span> <span data-ttu-id="4f220-223">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f220-223">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4f220-225">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="4f220-225">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4f220-226">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f220-226">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4f220-227">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f220-227">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4f220-228">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f220-228">Testing single sign-on</span></span>

<span data-ttu-id="4f220-229">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4f220-229">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4f220-230">Klicka på panelen upptar LMS på åtkomstpanelen, du får automatiskt loggat in på ditt program skulle kunna ta emot LMS.</span><span class="sxs-lookup"><span data-stu-id="4f220-230">Click the Absorb LMS tile in the Access Panel, you get automatically signed-on to your Absorb LMS application.</span></span>

<span data-ttu-id="4f220-231">Mer information om åtkomstpanelen finns i.</span><span class="sxs-lookup"><span data-stu-id="4f220-231">For more information about the Access Panel, see.</span></span> <span data-ttu-id="4f220-232">[Introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="4f220-232">[Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f220-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4f220-233">Additional resources</span></span>

* [<span data-ttu-id="4f220-234">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4f220-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4f220-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4f220-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_203.png

