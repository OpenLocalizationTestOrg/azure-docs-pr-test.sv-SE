---
title: Konfigurera Azure Key Vault slutpunkt till slutpunkt viktiga rotation och granskning | Microsoft Docs
description: "Använd den här anvisningar som hjälper dig att börja arbeta med viktiga rotation och övervakning nyckelvalv loggar."
services: key-vault
documentationcenter: 
author: swgriffith
manager: mbaldwin
tags: 
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: jodehavi;stgriffi
ms.openlocfilehash: 38c342802ed687985ac6f84f5a590a1a0dcc6c6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a><span data-ttu-id="90472-103">Konfigurera Azure Key Vault med nyckelgranskning och -rotation från slutpunkt till slutpunkt</span><span class="sxs-lookup"><span data-stu-id="90472-103">Set up Azure Key Vault with end-to-end key rotation and auditing</span></span>
## <a name="introduction"></a><span data-ttu-id="90472-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="90472-104">Introduction</span></span>
<span data-ttu-id="90472-105">När du har skapat ditt nyckelvalv kommer du att kunna börja använda det valvet för att lagra dina nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="90472-105">After creating your key vault, you will be able to start using that vault to store your keys and secrets.</span></span> <span data-ttu-id="90472-106">Dina program inte längre behöver spara dina nycklar och hemligheter, utan i stället begär dem från nyckelvalvet efter behov.</span><span class="sxs-lookup"><span data-stu-id="90472-106">Your applications no longer need to persist your keys or secrets, but rather will request them from the key vault as needed.</span></span> <span data-ttu-id="90472-107">På så sätt kan du uppdatera nycklar och hemligheter utan att påverka beteendet för programmet, vilket öppnar ett brett möjligheter runt din nyckel och hemliga hantering.</span><span class="sxs-lookup"><span data-stu-id="90472-107">This allows you to update keys and secrets without affecting the behavior of your application, which opens up a breadth of possibilities around your key and secret management.</span></span>

<span data-ttu-id="90472-108">Den här artikeln beskriver hur ett exempel på hur Azure Key Vault för att lagra en hemlighet i det här fallet en nyckel för Azure Storage-konto som används av ett program.</span><span class="sxs-lookup"><span data-stu-id="90472-108">This article walks through an example of using Azure Key Vault to store a secret, in this case an Azure Storage Account key that is accessed by an application.</span></span> <span data-ttu-id="90472-109">Här visas också implementering av en schemalagd rotation av den lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="90472-109">It also demonstrates implementation of a scheduled rotation of that storage account key.</span></span> <span data-ttu-id="90472-110">Slutligen går den igenom en demonstration av hur du övervakar nyckelvalv granskningsloggar och generera aviseringar när oväntat begäranden som görs.</span><span class="sxs-lookup"><span data-stu-id="90472-110">Finally, it walks through a demonstration of how to monitor the key vault audit logs and raise alerts when unexpected requests are made.</span></span>

> [!NOTE]
> <span data-ttu-id="90472-111">Den här kursen är inte avsedd som förklarar i detalj installationen av nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="90472-111">This tutorial is not intended to explain in detail the initial setup of your key vault.</span></span> <span data-ttu-id="90472-112">Den här informationen finns i [Komma igång med Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="90472-112">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="90472-113">Plattformsoberoende kommandoradsgränssnittet instruktioner finns i [hantera Key Vault med hjälp av CLI](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="90472-113">For Cross-Platform Command-Line Interface instructions, see [Manage Key Vault using CLI](key-vault-manage-with-cli2.md).</span></span>
>
>

## <a name="set-up-key-vault"></a><span data-ttu-id="90472-114">Konfigurera Key Vault</span><span class="sxs-lookup"><span data-stu-id="90472-114">Set up Key Vault</span></span>
<span data-ttu-id="90472-115">Om du vill aktivera ett program att hämta en hemlighet från Nyckelvalvet, måste du först skapa hemligheten och överföra den till ditt valv.</span><span class="sxs-lookup"><span data-stu-id="90472-115">To enable an application to retrieve a secret from Key Vault, you must first create the secret and upload it to your vault.</span></span> <span data-ttu-id="90472-116">Detta kan åstadkommas genom att starta en Azure PowerShell-session och logga in på ditt Azure-konto med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="90472-116">This can be accomplished by starting an Azure PowerShell session and signing in to your Azure account with the following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="90472-117">Ange användarnamnet och lösenordet för ditt Azure-konto i popup-fönstret i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="90472-117">In the pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="90472-118">PowerShell får alla prenumerationer som är associerade med det här kontot.</span><span class="sxs-lookup"><span data-stu-id="90472-118">PowerShell will get all the subscriptions that are associated with this account.</span></span> <span data-ttu-id="90472-119">PowerShell använder den första som standard.</span><span class="sxs-lookup"><span data-stu-id="90472-119">PowerShell uses the first one by default.</span></span>

<span data-ttu-id="90472-120">Om du har flera prenumerationer kan behöva du ange det konto som användes för att skapa nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="90472-120">If you have multiple subscriptions, you might have to specify the one that was used to create your key vault.</span></span> <span data-ttu-id="90472-121">Ange följande om du vill visa prenumerationer för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="90472-121">Enter the following to see the subscriptions for your account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="90472-122">Ange om du vill ange den prenumeration som är kopplad till nyckelvalvet du loggar:</span><span class="sxs-lookup"><span data-stu-id="90472-122">To specify the subscription that's associated with the key vault you will be logging, enter:</span></span>

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

<span data-ttu-id="90472-123">Eftersom den här artikeln visar lagra en lagringskontonyckel som en hemlighet, måste du hämta den lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="90472-123">Because this article demonstrates storing a storage account key as a secret, you must get that storage account key.</span></span>

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

<span data-ttu-id="90472-124">När du har hämtat ditt hemliga (i det här fallet din lagringskontonyckel), måste du konvertera som till en säker sträng och sedan skapa en hemlighet som har värdet i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="90472-124">After retrieving your secret (in this case, your storage account key), you must convert that to a secure string and then create a secret with that value in your key vault.</span></span>

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
<span data-ttu-id="90472-125">Hämta sedan URI: N för den hemlighet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="90472-125">Next, get the URI for the secret you created.</span></span> <span data-ttu-id="90472-126">Detta används i ett senare steg när du anropar nyckelvalvet att hämta ditt hemliga.</span><span class="sxs-lookup"><span data-stu-id="90472-126">This is used in a later step when you call the key vault to retrieve your secret.</span></span> <span data-ttu-id="90472-127">Kör följande PowerShell-kommando och anteckna ID-värde, som är den hemliga URI:</span><span class="sxs-lookup"><span data-stu-id="90472-127">Run the following PowerShell command and make note of the ID value, which is the secret URI:</span></span>

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a><span data-ttu-id="90472-128">Konfigurera programmet</span><span class="sxs-lookup"><span data-stu-id="90472-128">Set up the application</span></span>
<span data-ttu-id="90472-129">Du kan använda koden för att hämta och använda den nu när du har en hemlighet som lagras.</span><span class="sxs-lookup"><span data-stu-id="90472-129">Now that you have a secret stored, you can use code to retrieve and use it.</span></span> <span data-ttu-id="90472-130">Det finns några steg som krävs för att uppnå detta.</span><span class="sxs-lookup"><span data-stu-id="90472-130">There are a few steps required to achieve this.</span></span> <span data-ttu-id="90472-131">Det första och viktigaste steget registrera ditt program med Azure Active Directory och sedan uppmanar Key Vault information om programmet så att den kan tillåta förfrågningar från ditt program.</span><span class="sxs-lookup"><span data-stu-id="90472-131">The first and most important step is registering your application with Azure Active Directory and then telling Key Vault your application information so that it can allow requests from your application.</span></span>

> [!NOTE]
> <span data-ttu-id="90472-132">Programmet måste skapas på samma Azure Active Directory-klientorganisation som ditt nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="90472-132">Your application must be created on the same Azure Active Directory tenant as your key vault.</span></span>
>
>

<span data-ttu-id="90472-133">Öppna fliken program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="90472-133">Open the applications tab of Azure Active Directory.</span></span>

![Öppna program i Azure Active Directory](./media/keyvault-keyrotation/AzureAD_Header.png)

<span data-ttu-id="90472-135">Välj **Lägg till** att lägga till ett program till Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="90472-135">Choose **ADD** to add an application to your Azure Active Directory.</span></span>

![Välj Lägg till](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

<span data-ttu-id="90472-137">Lämna programtyp som **WEB APPLICATION och/eller webb-API** och namnge ditt program.</span><span class="sxs-lookup"><span data-stu-id="90472-137">Leave the application type as **WEB APPLICATION AND/OR WEB API** and give your application a name.</span></span>

![Ett namn för programmet](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

<span data-ttu-id="90472-139">Ge programmet en **SIGN-ON-URL** och en **APP-ID URI**.</span><span class="sxs-lookup"><span data-stu-id="90472-139">Give your application a **SIGN-ON URL** and an **APP ID URI**.</span></span> <span data-ttu-id="90472-140">Dessa kan vara vad du vill använda för den här demon och de kan ändras senare om det behövs.</span><span class="sxs-lookup"><span data-stu-id="90472-140">These can be anything you want for this demo, and they can be changed later if needed.</span></span>

![Ange nödvändiga URL: er](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

<span data-ttu-id="90472-142">När programmet har lagts till Azure Active Directory, kommer du till sidan program.</span><span class="sxs-lookup"><span data-stu-id="90472-142">After the application is added to Azure Active Directory, you will be brought into the application page.</span></span> <span data-ttu-id="90472-143">Klicka på den **konfigurera** fliken och leta sedan reda på och kopiera den **klient-ID** värde.</span><span class="sxs-lookup"><span data-stu-id="90472-143">Click the **Configure** tab and then find and copy the **Client ID** value.</span></span> <span data-ttu-id="90472-144">Anteckna klient-ID för senare steg.</span><span class="sxs-lookup"><span data-stu-id="90472-144">Make note of the client ID for later steps.</span></span>

<span data-ttu-id="90472-145">Generera en nyckel för tillämpningsprogrammet bredvid, så den kan interagera med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="90472-145">Next, generate a key for your application so it can interact with your Azure Active Directory.</span></span> <span data-ttu-id="90472-146">Du kan skapa det under den **nycklar** i avsnittet den **Configuration** fliken.</span><span class="sxs-lookup"><span data-stu-id="90472-146">You can create this under the **Keys** section in the **Configuration** tab.</span></span> <span data-ttu-id="90472-147">Anteckna den nyligen skapade nyckeln från Azure Active Directory-program för användning i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="90472-147">Make note of the newly generated key from your Azure Active Directory application for use in a later step.</span></span>

![Azure Active Directory App nycklar](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

<span data-ttu-id="90472-149">Innan du upprättar ett anrop från ditt program i nyckelvalvet, måste du se nyckelvalvet om programmet och dess behörighet.</span><span class="sxs-lookup"><span data-stu-id="90472-149">Before establishing any calls from your application into the key vault, you must tell the key vault about your application and its permissions.</span></span> <span data-ttu-id="90472-150">Följande kommando tar valvnamnet och klient-ID från din app i Azure Active Directory och ger **hämta** åtkomst till nyckelvalvet för programmet.</span><span class="sxs-lookup"><span data-stu-id="90472-150">The following command takes the vault name and the client ID from your Azure Active Directory app and grants **Get** access to your key vault for the application.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

<span data-ttu-id="90472-151">Du är nu redo att börja bygga program-anrop.</span><span class="sxs-lookup"><span data-stu-id="90472-151">At this point, you are ready to start building your application calls.</span></span> <span data-ttu-id="90472-152">I ditt program måste du installera NuGet-paket som krävs för att interagera med Azure Key Vault och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="90472-152">In your application, you must install the NuGet packages required to interact with Azure Key Vault and Azure Active Directory.</span></span> <span data-ttu-id="90472-153">Ange följande kommandon från Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="90472-153">From the Visual Studio Package Manager console, enter the following commands.</span></span> <span data-ttu-id="90472-154">Den aktuella versionen av Azure Active Directory-paketet är 3.10.305231913, så du kanske vill bekräfta den senaste versionen och uppdateras vid skrivning av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="90472-154">At the writing of this article, the current version of the Azure Active Directory package is 3.10.305231913, so you might want to confirm the latest version and update accordingly.</span></span>

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

<span data-ttu-id="90472-155">Skapa en klass för att lagra metoden för Azure Active Directory-autentisering i din programkod.</span><span class="sxs-lookup"><span data-stu-id="90472-155">In your application code, create a class to hold the method for your Azure Active Directory authentication.</span></span> <span data-ttu-id="90472-156">I det här exemplet är klassen kallas **verktyg för webbplatsuppgradering**.</span><span class="sxs-lookup"><span data-stu-id="90472-156">In this example, that class is called **Utils**.</span></span> <span data-ttu-id="90472-157">Lägg till följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="90472-157">Add the following using statement:</span></span>

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="90472-158">Lägg till följande metod för att hämta JWT-token från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="90472-158">Next, add the following method to retrieve the JWT token from Azure Active Directory.</span></span> <span data-ttu-id="90472-159">Underhålla, kanske du vill flytta hårdkodade strängvärden till webb- eller konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="90472-159">For maintainability, you may want to move the hard-coded string values into your web or application configuration.</span></span>

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

<span data-ttu-id="90472-160">Lägg till den kod som behövs för att anropa Key Vault och hämta det hemliga värdet.</span><span class="sxs-lookup"><span data-stu-id="90472-160">Add the necessary code to call Key Vault and retrieve your secret value.</span></span> <span data-ttu-id="90472-161">Först måste du lägga till följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="90472-161">First you must add the following using statement:</span></span>

```csharp
using Microsoft.Azure.KeyVault;
```

<span data-ttu-id="90472-162">Lägg till metodanrop att anropa Key Vault och hämta ditt hemliga.</span><span class="sxs-lookup"><span data-stu-id="90472-162">Add the method calls to invoke Key Vault and retrieve your secret.</span></span> <span data-ttu-id="90472-163">I den här metoden kan ange du hemlighet URI som du sparade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="90472-163">In this method, you provide the secret URI that you saved in a previous step.</span></span> <span data-ttu-id="90472-164">Observera användningen av den **GetToken** metod från den **verktyg för webbplatsuppgradering** klassen som skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="90472-164">Note the use of the **GetToken** method from the **Utils** class created previously.</span></span>

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

<span data-ttu-id="90472-165">När du kör ditt program nu bör du autentiserar till Azure Active Directory och sedan hämta din hemligt värde från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="90472-165">When you run your application, you should now be authenticating to Azure Active Directory and then retrieving your secret value from Azure Key Vault.</span></span>

## <a name="key-rotation-using-azure-automation"></a><span data-ttu-id="90472-166">Viktiga rotation med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="90472-166">Key rotation using Azure Automation</span></span>
<span data-ttu-id="90472-167">Det finns olika alternativ för att implementera en rotation strategi för värden som du lagrar som Azure Key Vault hemligheter.</span><span class="sxs-lookup"><span data-stu-id="90472-167">There are various options for implementing a rotation strategy for values you store as Azure Key Vault secrets.</span></span> <span data-ttu-id="90472-168">Hemligheter kan roteras som en del av en manuell process, de kan roteras programmässigt med hjälp av API-anrop eller kan roteras med ett Automation-skript.</span><span class="sxs-lookup"><span data-stu-id="90472-168">Secrets can be rotated as part of a manual process, they may be rotated programmatically by using API calls, or they may be rotated by way of an Automation script.</span></span> <span data-ttu-id="90472-169">Vid tillämpningen av den här artikeln kommer du att använda Azure PowerShell tillsammans med Azure Automation för att ändra snabbtangent Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="90472-169">For the purposes of this article, you will be using Azure PowerShell combined with Azure Automation to change an Azure Storage Account access key.</span></span> <span data-ttu-id="90472-170">Sedan uppdaterar du en hemlighet i nyckelvalvet med den nya nyckeln.</span><span class="sxs-lookup"><span data-stu-id="90472-170">You will then update a key vault secret with that new key.</span></span>

<span data-ttu-id="90472-171">Om du vill tillåta Azure Automation att ställa in hemliga värden i nyckelvalvet, måste du hämta klient-ID för anslutningen med namnet AzureRunAsConnection som skapades när du har etablerat Azure Automation-instans.</span><span class="sxs-lookup"><span data-stu-id="90472-171">To allow Azure Automation to set secret values in your key vault, you must get the client ID for the connection named AzureRunAsConnection, which was created when you established your Azure Automation instance.</span></span> <span data-ttu-id="90472-172">Du hittar det här ID genom att välja **tillgångar** från Azure Automation-instans.</span><span class="sxs-lookup"><span data-stu-id="90472-172">You can find this ID by choosing **Assets** from your Azure Automation instance.</span></span> <span data-ttu-id="90472-173">Därifrån kan du välja **anslutningar** och välj sedan den **AzureRunAsConnection** tjänsten principen.</span><span class="sxs-lookup"><span data-stu-id="90472-173">From there, you choose **Connections** and then select the **AzureRunAsConnection** service principle.</span></span> <span data-ttu-id="90472-174">Anteckna den **program-ID**.</span><span class="sxs-lookup"><span data-stu-id="90472-174">Take note of the **Application ID**.</span></span>

![Azure Automation-klient-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

<span data-ttu-id="90472-176">I **tillgångar**, Välj **moduler**.</span><span class="sxs-lookup"><span data-stu-id="90472-176">In **Assets**, choose **Modules**.</span></span> <span data-ttu-id="90472-177">Från **moduler**väljer **galleriet**, och sök sedan efter och **importera** uppdaterade versioner av vart och ett av följande moduler:</span><span class="sxs-lookup"><span data-stu-id="90472-177">From **Modules**, select **Gallery**, and then search for and **Import** updated versions of each of the following modules:</span></span>

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> <span data-ttu-id="90472-178">Vid skrivning av den här artikeln behövs endast modulerna som tidigare meddelanden som ska uppdateras i följande skript.</span><span class="sxs-lookup"><span data-stu-id="90472-178">At the writing of this article, only the previously noted modules needed to be updated for the following script.</span></span> <span data-ttu-id="90472-179">Om du tycker att ditt automation-jobb inte fungerar kan du bekräfta att du har importerat alla moduler som krävs och deras beroenden.</span><span class="sxs-lookup"><span data-stu-id="90472-179">If you find that your automation job is failing, confirm that you have imported all necessary modules and their dependencies.</span></span>
>
>

<span data-ttu-id="90472-180">När du har hämtat program-ID för Azure Automation-anslutning, måste du se nyckelvalvet att det här programmet har åtkomst till uppdatera hemligheter i ditt valv.</span><span class="sxs-lookup"><span data-stu-id="90472-180">After you have retrieved the application ID for your Azure Automation connection, you must tell your key vault that this application has access to update secrets in your vault.</span></span> <span data-ttu-id="90472-181">Detta kan åstadkommas med följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="90472-181">This can be accomplished with the following PowerShell command:</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

<span data-ttu-id="90472-182">Välj därefter **Runbooks** under din Azure Automation-instans och välj sedan **lägga till en Runbook**.</span><span class="sxs-lookup"><span data-stu-id="90472-182">Next, select **Runbooks** under your Azure Automation instance, and then select **Add a Runbook**.</span></span> <span data-ttu-id="90472-183">Välj **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="90472-183">Select **Quick Create**.</span></span> <span data-ttu-id="90472-184">Din runbook och välj **PowerShell** som runbook-typen.</span><span class="sxs-lookup"><span data-stu-id="90472-184">Name your runbook and select **PowerShell** as the runbook type.</span></span> <span data-ttu-id="90472-185">Du har möjlighet att lägga till en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="90472-185">You have the option to add a description.</span></span> <span data-ttu-id="90472-186">Klicka slutligen på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="90472-186">Finally, click **Create**.</span></span>

![Skapa runbook](./media/keyvault-keyrotation/Create_Runbook.png)

<span data-ttu-id="90472-188">Klistra in följande PowerShell-skript i fönstret Redigeraren för din nya runbook:</span><span class="sxs-lookup"><span data-stu-id="90472-188">Paste the following PowerShell script in the editor pane for your new runbook:</span></span>

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

<span data-ttu-id="90472-189">Editor-fönstret Välj **Test fönstret** Testa skriptet.</span><span class="sxs-lookup"><span data-stu-id="90472-189">From the editor pane, choose **Test pane** to test your script.</span></span> <span data-ttu-id="90472-190">När skriptet körs utan fel, kan du välja **publicera**, och du kan koppla ett schema för runbook tillbaka i fönstret runbook konfiguration.</span><span class="sxs-lookup"><span data-stu-id="90472-190">Once the script is running without error, you can select **Publish**, and then you can apply a schedule for the runbook back in the runbook configuration pane.</span></span>

## <a name="key-vault-auditing-pipeline"></a><span data-ttu-id="90472-191">Key Vault granskning pipeline</span><span class="sxs-lookup"><span data-stu-id="90472-191">Key Vault auditing pipeline</span></span>
<span data-ttu-id="90472-192">När du ställer in ett nyckelvalv som du kan aktivera granskning för att samla in loggar på åtkomstbegäranden som görs till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="90472-192">When you set up a key vault, you can turn on auditing to collect logs on access requests made to the key vault.</span></span> <span data-ttu-id="90472-193">Dessa loggar lagras i ett avsedda Azure Storage-konto och kan hämtas, övervakas och analyseras.</span><span class="sxs-lookup"><span data-stu-id="90472-193">These logs are stored in a designated Azure Storage account and can be pulled out, monitored, and analyzed.</span></span> <span data-ttu-id="90472-194">Följande scenario använder Azure functions, Azure logikappar och nyckelvalv granskningsloggar för att skapa en pipeline för att skicka ett e-postmeddelande när en app som matchar det app-ID för webbappen hämtar hemligheter från valvet.</span><span class="sxs-lookup"><span data-stu-id="90472-194">The following scenario uses Azure functions, Azure logic apps, and key vault audit logs to create a pipeline to send an email when an app that does match the app ID of the web app retrieves secrets from the vault.</span></span>

<span data-ttu-id="90472-195">Först måste du aktivera loggning på nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="90472-195">First, you must enable logging on your key vault.</span></span> <span data-ttu-id="90472-196">Detta kan göras via följande PowerShell-kommandon (fullständig information kan visas vid [key vault loggning](key-vault-logging.md)):</span><span class="sxs-lookup"><span data-stu-id="90472-196">This can be done via the following PowerShell commands (full details can be seen at [key-vault-logging](key-vault-logging.md)):</span></span>

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

<span data-ttu-id="90472-197">När den är aktiverad, granskningsloggar start insamling till avsedda storage-konto.</span><span class="sxs-lookup"><span data-stu-id="90472-197">After this is enabled, audit logs start collecting into the designated storage account.</span></span> <span data-ttu-id="90472-198">Dessa loggar innehålla händelser om hur och när ditt nyckelvalv kan nås och av vem.</span><span class="sxs-lookup"><span data-stu-id="90472-198">These logs contain events about how and when your key vaults are accessed, and by whom.</span></span>

> [!NOTE]
> <span data-ttu-id="90472-199">Du kan komma åt din loggningsinformation 10 minuter efter nyckelvalv igen.</span><span class="sxs-lookup"><span data-stu-id="90472-199">You can access your logging information 10 minutes after the key vault operation.</span></span> <span data-ttu-id="90472-200">Det kommer vanligtvis vara snabbare än så.</span><span class="sxs-lookup"><span data-stu-id="90472-200">It will usually be quicker than this.</span></span>
>
>

<span data-ttu-id="90472-201">Nästa steg är att [skapa en Azure Service Bus-kö](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="90472-201">The next step is to [create an Azure Service Bus queue](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span> <span data-ttu-id="90472-202">Detta är där nyckelvalv granskningsloggar push.</span><span class="sxs-lookup"><span data-stu-id="90472-202">This is where key vault audit logs are pushed.</span></span> <span data-ttu-id="90472-203">När granskningsloggmeddelanden i kön logikappen hämtar dem och fungerar på dem..</span><span class="sxs-lookup"><span data-stu-id="90472-203">When the audit log messages are in the queue, the logic app picks them up and acts on them.</span></span> <span data-ttu-id="90472-204">Skapa en service bus med följande steg:</span><span class="sxs-lookup"><span data-stu-id="90472-204">Create a service bus with the following steps:</span></span>

1. <span data-ttu-id="90472-205">Skapa en Service Bus-namnrymd (om du redan har en som du vill använda för detta, hoppar du till steg 2).</span><span class="sxs-lookup"><span data-stu-id="90472-205">Create a Service Bus namespace (if you already have one that you want to use for this, skip to Step 2).</span></span>
2. <span data-ttu-id="90472-206">Bläddra till service bus i Azure-portalen och markera det namnområde som du vill skapa kön i.</span><span class="sxs-lookup"><span data-stu-id="90472-206">Browse to the service bus in the Azure portal and select the namespace you want to create the queue in.</span></span>
3. <span data-ttu-id="90472-207">Välj **ny** och välj **Service Bus > kön** och ange nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="90472-207">Select **New** and choose **Service Bus > Queue** and enter the required details.</span></span>
4. <span data-ttu-id="90472-208">Välj information för Service Bus-anslutning genom att välja namnområdet och klicka på **anslutningsinformationen**.</span><span class="sxs-lookup"><span data-stu-id="90472-208">Select the Service Bus connection information by choosing the namespace and clicking **Connection Information**.</span></span> <span data-ttu-id="90472-209">Du behöver den här informationen för nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="90472-209">You will need this information for the next section.</span></span>

<span data-ttu-id="90472-210">Nästa [skapa en Azure-funktion](../azure-functions/functions-create-first-azure-function.md) att avsöka nyckelvalv loggar i lagringskontot och hämta nya händelser.</span><span class="sxs-lookup"><span data-stu-id="90472-210">Next, [create an Azure function](../azure-functions/functions-create-first-azure-function.md) to poll key vault logs within the storage account and pick up new events.</span></span> <span data-ttu-id="90472-211">Det här är en funktion som utlöses enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="90472-211">This will be a function that is triggered on a schedule.</span></span>

<span data-ttu-id="90472-212">Om du vill skapa en Azure-funktion, Välj **New > Funktionsapp** i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="90472-212">To create an Azure function, choose **New > Function App** in the Azure portal.</span></span> <span data-ttu-id="90472-213">Du kan använda en befintlig värd plan eller skapa en ny vid skapandet.</span><span class="sxs-lookup"><span data-stu-id="90472-213">During creation, you can use an existing hosting plan or create a new one.</span></span> <span data-ttu-id="90472-214">Du kan också välja dynamisk värd.</span><span class="sxs-lookup"><span data-stu-id="90472-214">You could also opt for dynamic hosting.</span></span> <span data-ttu-id="90472-215">Mer information om funktionen som värd för alternativ finns på [så här skalar du Azure Functions](../azure-functions/functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="90472-215">More details on Function hosting options can be found at [How to scale Azure Functions](../azure-functions/functions-scale.md).</span></span>

<span data-ttu-id="90472-216">När funktionen Azure skapas, navigera till den och väljer en timer funktionen och C\#.</span><span class="sxs-lookup"><span data-stu-id="90472-216">When the Azure function is created, navigate to it and choose a timer function and C\#.</span></span> <span data-ttu-id="90472-217">Klicka på **skapa den här funktionen**.</span><span class="sxs-lookup"><span data-stu-id="90472-217">Then click **Create this function**.</span></span>

![Azure Functions starta bladet](./media/keyvault-keyrotation/Azure_Functions_Start.png)

<span data-ttu-id="90472-219">På den **utveckla** fliken ersätter run.csx koden med följande:</span><span class="sxs-lookup"><span data-stu-id="90472-219">On the **Develop** tab, replace the run.csx code with the following:</span></span>

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> <span data-ttu-id="90472-220">Se till att ersätta variabler i föregående kod så att den pekar till ditt lagringskonto där nyckelvalv loggarna skrivs, service bus som du skapade tidigare och sökvägen till nyckelvalvet lagring loggar.</span><span class="sxs-lookup"><span data-stu-id="90472-220">Make sure to replace the variables in the preceding code to point to your storage account where the key vault logs are written, the service bus you created earlier, and the specific path to the key vault storage logs.</span></span>
>
>

<span data-ttu-id="90472-221">Funktionen hämtar senaste loggfilen från lagringskontot där nyckelvalv loggarna skrivs, hämtar de senaste händelserna från filen och skickar dem till en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="90472-221">The function picks up the latest log file from the storage account where the key vault logs are written, grabs the latest events from that file, and pushes them to a Service Bus queue.</span></span> <span data-ttu-id="90472-222">Eftersom en enda fil kan ha flera händelser, bör du skapa en sync.txt-fil som funktionen också kontrollerar för att fastställa tidsstämpeln för den senaste händelse som hämtades.</span><span class="sxs-lookup"><span data-stu-id="90472-222">Since a single file could have multiple events, you should create a sync.txt file that the function also looks at to determine the time stamp of the last event that was picked up.</span></span> <span data-ttu-id="90472-223">Detta säkerställer att du inte push samma händelse flera gånger.</span><span class="sxs-lookup"><span data-stu-id="90472-223">This ensures that you don't push the same event multiple times.</span></span> <span data-ttu-id="90472-224">Sync.txt filen innehåller en tidsstämpel för senaste påträffade händelsen.</span><span class="sxs-lookup"><span data-stu-id="90472-224">This sync.txt file contains a timestamp for the last encountered event.</span></span> <span data-ttu-id="90472-225">Loggar, vid lästs in, måste sorteras utifrån tidsstämpel så de sorteras på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="90472-225">The logs, when loaded, have to be sorted based on the timestamp to ensure they are ordered correctly.</span></span>

<span data-ttu-id="90472-226">För den här funktionen referera vi några ytterligare bibliotek som inte är tillgängliga direkt i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="90472-226">For this function, we reference a couple of additional libraries that are not available out of the box in Azure Functions.</span></span> <span data-ttu-id="90472-227">Om du vill inkludera dessa måste Azure Functions kan hämta dem med hjälp av NuGet.</span><span class="sxs-lookup"><span data-stu-id="90472-227">To include these, we need Azure Functions to pull them using NuGet.</span></span> <span data-ttu-id="90472-228">Välj den **visa filer** alternativet.</span><span class="sxs-lookup"><span data-stu-id="90472-228">Choose the **View Files** option.</span></span>

![Visa filer](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

<span data-ttu-id="90472-230">Och Lägg till en fil som heter project.json med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="90472-230">And add a file called project.json with following content:</span></span>

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
<span data-ttu-id="90472-231">Vid **spara**, Azure Functions laddar ned nödvändiga binärfiler.</span><span class="sxs-lookup"><span data-stu-id="90472-231">Upon **Save**, Azure Functions will download the required binaries.</span></span>

<span data-ttu-id="90472-232">Växla till den **integrera** fliken och ge parametern timer ett beskrivande namn för att användas i funktionen.</span><span class="sxs-lookup"><span data-stu-id="90472-232">Switch to the **Integrate** tab and give the timer parameter a meaningful name to use within the function.</span></span> <span data-ttu-id="90472-233">I föregående kod den förväntar sig timern anropas *myTimer*.</span><span class="sxs-lookup"><span data-stu-id="90472-233">In the preceding code, it expects the timer to be called *myTimer*.</span></span> <span data-ttu-id="90472-234">Ange en [CRON-uttryck](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) enligt följande: 0 \* \* \* \* \* för timer som gör att funktionen ska köras en gång i minuten.</span><span class="sxs-lookup"><span data-stu-id="90472-234">Specify a [CRON expression](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) as follows: 0 \* \* \* \* \* for the timer that will cause the function to run once a minute.</span></span>

<span data-ttu-id="90472-235">På samma **integrera** lägger du till indata av typen **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="90472-235">On the same **Integrate** tab, add an input of the type **Azure Blob Storage**.</span></span> <span data-ttu-id="90472-236">Detta att peka sync.txt-fil som innehåller tidsstämpel för senaste händelsen tittat på av funktionen.</span><span class="sxs-lookup"><span data-stu-id="90472-236">This will point to the sync.txt file that contains the timestamp of the last event looked at by the function.</span></span> <span data-ttu-id="90472-237">Det här är tillgängliga i funktionen av parameternamnet.</span><span class="sxs-lookup"><span data-stu-id="90472-237">This will be available within the function by the parameter name.</span></span> <span data-ttu-id="90472-238">I föregående kod Azure Blob Storage-indata förväntar parameternamnet ska *inputBlob*.</span><span class="sxs-lookup"><span data-stu-id="90472-238">In the preceding code, the Azure Blob Storage input expects the parameter name to be *inputBlob*.</span></span> <span data-ttu-id="90472-239">Väljer det lagringskonto där sync.txt-filen ska placeras (det kan vara samma eller ett annat lagringskonto).</span><span class="sxs-lookup"><span data-stu-id="90472-239">Choose the storage account where the sync.txt file will reside (it could be the same or a different storage account).</span></span> <span data-ttu-id="90472-240">I sökvägsfältet, ange sökvägen där filen finns i formatet {container-name}/path/to/sync.txt.</span><span class="sxs-lookup"><span data-stu-id="90472-240">In the path field, provide the path where the file lives in the format {container-name}/path/to/sync.txt.</span></span>

<span data-ttu-id="90472-241">Lägga till utdata av typen *Azure Blob Storage* utdata.</span><span class="sxs-lookup"><span data-stu-id="90472-241">Add an output of the type *Azure Blob Storage* output.</span></span> <span data-ttu-id="90472-242">Detta peka mot sync.txt-filen som du har definierat i indata.</span><span class="sxs-lookup"><span data-stu-id="90472-242">This will point to the sync.txt file you defined in the input.</span></span> <span data-ttu-id="90472-243">Detta används av funktionen för att skriva tidsstämpel för senaste händelsen tittar på.</span><span class="sxs-lookup"><span data-stu-id="90472-243">This is used by the function to write the timestamp of the last event looked at.</span></span> <span data-ttu-id="90472-244">Föregående kod förväntar sig att den här parametern anropas *outputBlob*.</span><span class="sxs-lookup"><span data-stu-id="90472-244">The preceding code expects this parameter to be called *outputBlob*.</span></span>

<span data-ttu-id="90472-245">Funktionen är nu klar.</span><span class="sxs-lookup"><span data-stu-id="90472-245">At this point, the function is ready.</span></span> <span data-ttu-id="90472-246">Se till att växla tillbaka till den **utveckla** fliken och spara koden.</span><span class="sxs-lookup"><span data-stu-id="90472-246">Make sure to switch back to the **Develop** tab and save the code.</span></span> <span data-ttu-id="90472-247">Kontrollera utdatafönstret för några kompileringsfel och korrigera dem i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="90472-247">Check the output window for any compilation errors and correct them accordingly.</span></span> <span data-ttu-id="90472-248">Om koden kompilerar ska koden nu kontrollerar nyckelvalv loggar varje minut och därefter överföra alla nya händelser till definierade Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="90472-248">If the code compiles, then the code should now be checking the key vault logs every minute and pushing any new events onto the defined Service Bus queue.</span></span> <span data-ttu-id="90472-249">Du bör se loggningsinformation skriva till loggfönstret varje gång funktionen utlöses.</span><span class="sxs-lookup"><span data-stu-id="90472-249">You should see logging information write out to the log window every time the function is triggered.</span></span>

### <a name="azure-logic-app"></a><span data-ttu-id="90472-250">Azure logikapp</span><span class="sxs-lookup"><span data-stu-id="90472-250">Azure logic app</span></span>
<span data-ttu-id="90472-251">Nu måste du skapa ett Azure logikappen som hämtar händelser att funktionen är push-installation till Service Bus-kö, tolkar innehållet och skickar ett e-post baserat på ett villkor som matchas.</span><span class="sxs-lookup"><span data-stu-id="90472-251">Next you must create an Azure logic app that picks up the events that the function is pushing to the Service Bus queue, parses the content, and sends an email based on a condition being matched.</span></span>

<span data-ttu-id="90472-252">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) genom att gå till **New > Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="90472-252">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) by going to **New > Logic App**.</span></span>

<span data-ttu-id="90472-253">När logikappen har skapats, navigera till den och välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="90472-253">Once the logic app is created, navigate to it and choose **edit**.</span></span> <span data-ttu-id="90472-254">I Redigeraren för logik-app väljer **Service Bus-kö** och ange dina autentiseringsuppgifter för Service Bus för att ansluta till kön.</span><span class="sxs-lookup"><span data-stu-id="90472-254">Within the logic app editor, choose **Service Bus Queue** and enter your Service Bus credentials to connect it to the queue.</span></span>

![Logik för Azure App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

<span data-ttu-id="90472-256">Välj nästa **Lägg till ett villkor**.</span><span class="sxs-lookup"><span data-stu-id="90472-256">Next choose **Add a condition**.</span></span> <span data-ttu-id="90472-257">Växla till redigeraren i villkoret och ange följande kod, ersätta APP_ID med den faktiska APP_ID av ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="90472-257">In the condition, switch to the advanced editor and enter the following code, replacing APP_ID with the actual APP_ID of your web app:</span></span>

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

<span data-ttu-id="90472-258">Det här uttrycket returnerar i stort sett **FALSKT** om den *appid* från inkommande händelse (vilket är Service Bus-postmeddelandets brödtext) är inte den *appid* av appen.</span><span class="sxs-lookup"><span data-stu-id="90472-258">This expression essentially returns **false** if the *appid* from the incoming event (which is the body of the Service Bus message) is not the *appid* of the app.</span></span>

<span data-ttu-id="90472-259">Nu ska du skapa en åtgärd under **om Nej, gör ingenting**.</span><span class="sxs-lookup"><span data-stu-id="90472-259">Now, create an action under **If no, do nothing**.</span></span>

![Azure Logikapp välja åtgärd](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

<span data-ttu-id="90472-261">Åtgärden, Välj **Office 365 – skicka e-post**.</span><span class="sxs-lookup"><span data-stu-id="90472-261">For the action, choose **Office 365 - send email**.</span></span> <span data-ttu-id="90472-262">Fyll i fälten för att skapa ett e-postmeddelande ska skickas när det definierade villkoret returnerar **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="90472-262">Fill out the fields to create an email to send when the defined condition returns **false**.</span></span> <span data-ttu-id="90472-263">Om du inte har Office 365 kan du titta på alternativ för att uppnå samma resultat.</span><span class="sxs-lookup"><span data-stu-id="90472-263">If you do not have Office 365, you could look at alternatives to achieve the same results.</span></span>

<span data-ttu-id="90472-264">Nu har du en slutpunkt till slutpunkt-pipeline som söker efter nya nyckelvalv granskningsloggar en gång i minuten.</span><span class="sxs-lookup"><span data-stu-id="90472-264">At this point, you have an end to end pipeline that looks for new key vault audit logs once a minute.</span></span> <span data-ttu-id="90472-265">Den skickar nya loggar påträffas till en service bus-kö.</span><span class="sxs-lookup"><span data-stu-id="90472-265">It pushes new logs it finds to a service bus queue.</span></span> <span data-ttu-id="90472-266">Logikappen utlöses när ett nytt meddelande hamnar i kön.</span><span class="sxs-lookup"><span data-stu-id="90472-266">The logic app is triggered when a new message lands in the queue.</span></span> <span data-ttu-id="90472-267">Om den *appid* i händelsen inte matchar det app-ID för det anropande programmet, skickas ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="90472-267">If the *appid* within the event does not match the app ID of the calling application, it sends an email.</span></span>
