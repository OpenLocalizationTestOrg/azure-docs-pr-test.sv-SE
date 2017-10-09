---
title: aaaSet in Azure Key Vault slutpunkt till slutpunkt viktiga rotation och granskning | Microsoft Docs
description: "Använd den här hur tootoohelp du att börja arbeta med viktiga rotation och övervakning nyckelvalv loggar."
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
ms.openlocfilehash: e0c393873077e3b91adc9fa7f39128bc1b6abe26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a><span data-ttu-id="efe05-103">Konfigurera Azure Key Vault med nyckelgranskning och -rotation från slutpunkt till slutpunkt</span><span class="sxs-lookup"><span data-stu-id="efe05-103">Set up Azure Key Vault with end-to-end key rotation and auditing</span></span>
## <a name="introduction"></a><span data-ttu-id="efe05-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="efe05-104">Introduction</span></span>
<span data-ttu-id="efe05-105">När du har skapat nyckelvalvet, kommer du att kunna toostart använder den valvet toostore dina nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="efe05-105">After creating your key vault, you will be able toostart using that vault toostore your keys and secrets.</span></span> <span data-ttu-id="efe05-106">Dina program inte längre behöver toopersist dina nycklar och hemligheter, men i stället begär dem från hello nyckelvalv efter behov.</span><span class="sxs-lookup"><span data-stu-id="efe05-106">Your applications no longer need toopersist your keys or secrets, but rather will request them from hello key vault as needed.</span></span> <span data-ttu-id="efe05-107">Detta ger dig tooupdate nycklar och hemligheter utan att påverka hello beteendet för programmet, vilket öppnar ett brett möjligheter runt din nyckel och hemliga hantering.</span><span class="sxs-lookup"><span data-stu-id="efe05-107">This allows you tooupdate keys and secrets without affecting hello behavior of your application, which opens up a breadth of possibilities around your key and secret management.</span></span>

<span data-ttu-id="efe05-108">Den här artikeln beskriver hur ett exempel på hur Azure Key Vault toostore en hemlighet i det här fallet en nyckel för Azure Storage-konto som används av ett program.</span><span class="sxs-lookup"><span data-stu-id="efe05-108">This article walks through an example of using Azure Key Vault toostore a secret, in this case an Azure Storage Account key that is accessed by an application.</span></span> <span data-ttu-id="efe05-109">Här visas också implementering av en schemalagd rotation av den lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="efe05-109">It also demonstrates implementation of a scheduled rotation of that storage account key.</span></span> <span data-ttu-id="efe05-110">Slutligen går den igenom en demonstration av hur toomonitor hello nyckelvalv granskningsloggar och generera aviseringar när oväntat begäranden som görs.</span><span class="sxs-lookup"><span data-stu-id="efe05-110">Finally, it walks through a demonstration of how toomonitor hello key vault audit logs and raise alerts when unexpected requests are made.</span></span>

> [!NOTE]
> <span data-ttu-id="efe05-111">Den här kursen är inte avsedda tooexplain i detalj hello installationen av nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="efe05-111">This tutorial is not intended tooexplain in detail hello initial setup of your key vault.</span></span> <span data-ttu-id="efe05-112">Den här informationen finns i [Komma igång med Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="efe05-112">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="efe05-113">Plattformsoberoende kommandoradsgränssnittet instruktioner finns i [hantera Key Vault med hjälp av CLI](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="efe05-113">For Cross-Platform Command-Line Interface instructions, see [Manage Key Vault using CLI](key-vault-manage-with-cli2.md).</span></span>
>
>

## <a name="set-up-key-vault"></a><span data-ttu-id="efe05-114">Konfigurera Key Vault</span><span class="sxs-lookup"><span data-stu-id="efe05-114">Set up Key Vault</span></span>
<span data-ttu-id="efe05-115">tooenable ett program tooretrieve en hemlighet från Nyckelvalvet, måste du först skapa hello hemlighet och överföra den tooyour valvet.</span><span class="sxs-lookup"><span data-stu-id="efe05-115">tooenable an application tooretrieve a secret from Key Vault, you must first create hello secret and upload it tooyour vault.</span></span> <span data-ttu-id="efe05-116">Detta kan åstadkommas genom att starta en Azure PowerShell-session och logga in tooyour Azure-konto med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="efe05-116">This can be accomplished by starting an Azure PowerShell session and signing in tooyour Azure account with hello following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="efe05-117">Ange ditt användarnamn för Azure-konto och lösenord i hello popup-webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="efe05-117">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="efe05-118">PowerShell får alla hello-prenumerationer som är associerade med det här kontot.</span><span class="sxs-lookup"><span data-stu-id="efe05-118">PowerShell will get all hello subscriptions that are associated with this account.</span></span> <span data-ttu-id="efe05-119">PowerShell använder hello förstnämnda som standard.</span><span class="sxs-lookup"><span data-stu-id="efe05-119">PowerShell uses hello first one by default.</span></span>

<span data-ttu-id="efe05-120">Om du har flera prenumerationer kanske toospecify hello ett som har använt toocreate nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="efe05-120">If you have multiple subscriptions, you might have toospecify hello one that was used toocreate your key vault.</span></span> <span data-ttu-id="efe05-121">Ange hello följande toosee hello prenumerationer för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="efe05-121">Enter hello following toosee hello subscriptions for your account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="efe05-122">toospecify hello prenumeration som är kopplad till hello nyckelvalv som du loggar, ange:</span><span class="sxs-lookup"><span data-stu-id="efe05-122">toospecify hello subscription that's associated with hello key vault you will be logging, enter:</span></span>

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

<span data-ttu-id="efe05-123">Eftersom den här artikeln visar lagra en lagringskontonyckel som en hemlighet, måste du hämta den lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="efe05-123">Because this article demonstrates storing a storage account key as a secret, you must get that storage account key.</span></span>

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

<span data-ttu-id="efe05-124">Efter hämtning av ditt hemliga (i det här fallet din lagringskontonyckel), måste du konvertera den tooa säker strängen och sedan skapa en hemlighet som har värdet i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="efe05-124">After retrieving your secret (in this case, your storage account key), you must convert that tooa secure string and then create a secret with that value in your key vault.</span></span>

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
<span data-ttu-id="efe05-125">Hämta sedan hello URI för hello hemlighet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="efe05-125">Next, get hello URI for hello secret you created.</span></span> <span data-ttu-id="efe05-126">Detta används i ett senare steg när du anropar hello nyckelvalv tooretrieve ditt hemliga.</span><span class="sxs-lookup"><span data-stu-id="efe05-126">This is used in a later step when you call hello key vault tooretrieve your secret.</span></span> <span data-ttu-id="efe05-127">Kör följande PowerShell-kommando hello och anteckna hello ID-värde som är hello hemlighet URI:</span><span class="sxs-lookup"><span data-stu-id="efe05-127">Run hello following PowerShell command and make note of hello ID value, which is hello secret URI:</span></span>

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-hello-application"></a><span data-ttu-id="efe05-128">Ställ in hello program</span><span class="sxs-lookup"><span data-stu-id="efe05-128">Set up hello application</span></span>
<span data-ttu-id="efe05-129">Nu när du har en hemlighet som lagras, kan du använda koden tooretrieve och använda den.</span><span class="sxs-lookup"><span data-stu-id="efe05-129">Now that you have a secret stored, you can use code tooretrieve and use it.</span></span> <span data-ttu-id="efe05-130">Det finns några steg krävs tooachieve detta.</span><span class="sxs-lookup"><span data-stu-id="efe05-130">There are a few steps required tooachieve this.</span></span> <span data-ttu-id="efe05-131">hello är första och viktigaste steget registrera ditt program med Azure Active Directory och sedan uppmanar Key Vault information om programmet så att den kan tillåta förfrågningar från ditt program.</span><span class="sxs-lookup"><span data-stu-id="efe05-131">hello first and most important step is registering your application with Azure Active Directory and then telling Key Vault your application information so that it can allow requests from your application.</span></span>

> [!NOTE]
> <span data-ttu-id="efe05-132">Programmet måste skapas på hello samma Azure Active Directory-klient som ditt nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="efe05-132">Your application must be created on hello same Azure Active Directory tenant as your key vault.</span></span>
>
>

<span data-ttu-id="efe05-133">Öppna fliken för hello-program för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="efe05-133">Open hello applications tab of Azure Active Directory.</span></span>

![Öppna program i Azure Active Directory](./media/keyvault-keyrotation/AzureAD_Header.png)

<span data-ttu-id="efe05-135">Välj **lägga till** tooadd ett program tooyour Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="efe05-135">Choose **ADD** tooadd an application tooyour Azure Active Directory.</span></span>

![Välj Lägg till](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

<span data-ttu-id="efe05-137">Lämna hello programtyp som **WEB APPLICATION och/eller webb-API** och namnge ditt program.</span><span class="sxs-lookup"><span data-stu-id="efe05-137">Leave hello application type as **WEB APPLICATION AND/OR WEB API** and give your application a name.</span></span>

![Namnet hello program](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

<span data-ttu-id="efe05-139">Ge programmet en **SIGN-ON-URL** och en **APP-ID URI**.</span><span class="sxs-lookup"><span data-stu-id="efe05-139">Give your application a **SIGN-ON URL** and an **APP ID URI**.</span></span> <span data-ttu-id="efe05-140">Dessa kan vara vad du vill använda för den här demon och de kan ändras senare om det behövs.</span><span class="sxs-lookup"><span data-stu-id="efe05-140">These can be anything you want for this demo, and they can be changed later if needed.</span></span>

![Ange nödvändiga URL: er](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

<span data-ttu-id="efe05-142">När programmet hello läggs tooAzure Active Directory, kommer du till sidan för hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="efe05-142">After hello application is added tooAzure Active Directory, you will be brought into hello application page.</span></span> <span data-ttu-id="efe05-143">Klicka på hello **konfigurera** fliken och leta sedan reda på och kopiera hello **klient-ID** värde.</span><span class="sxs-lookup"><span data-stu-id="efe05-143">Click hello **Configure** tab and then find and copy hello **Client ID** value.</span></span> <span data-ttu-id="efe05-144">Anteckna hello klient-ID för senare steg.</span><span class="sxs-lookup"><span data-stu-id="efe05-144">Make note of hello client ID for later steps.</span></span>

<span data-ttu-id="efe05-145">Generera en nyckel för tillämpningsprogrammet bredvid, så den kan interagera med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="efe05-145">Next, generate a key for your application so it can interact with your Azure Active Directory.</span></span> <span data-ttu-id="efe05-146">Du kan skapa det under hello **nycklar** avsnitt i hello **Configuration** fliken. Anteckna hello nya nyckeln från Azure Active Directory-program för användning i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="efe05-146">You can create this under hello **Keys** section in hello **Configuration** tab. Make note of hello newly generated key from your Azure Active Directory application for use in a later step.</span></span>

![Azure Active Directory App nycklar](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

<span data-ttu-id="efe05-148">Innan du upprättar varje anrop från ditt program i hello nyckelvalv måste du se hello nyckelvalv om programmet och dess behörighet.</span><span class="sxs-lookup"><span data-stu-id="efe05-148">Before establishing any calls from your application into hello key vault, you must tell hello key vault about your application and its permissions.</span></span> <span data-ttu-id="efe05-149">hello följande kommando tar hello valvnamnet och hello klient-ID från din app i Azure Active Directory och ger **hämta** åtkomst tooyour nyckelvalv för hello program.</span><span class="sxs-lookup"><span data-stu-id="efe05-149">hello following command takes hello vault name and hello client ID from your Azure Active Directory app and grants **Get** access tooyour key vault for hello application.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

<span data-ttu-id="efe05-150">Nu är du redo toostart bygga program-anrop.</span><span class="sxs-lookup"><span data-stu-id="efe05-150">At this point, you are ready toostart building your application calls.</span></span> <span data-ttu-id="efe05-151">Du måste installera nödvändiga toointeract för hello NuGet-paket med Azure Key Vault och Azure Active Directory i ditt program.</span><span class="sxs-lookup"><span data-stu-id="efe05-151">In your application, you must install hello NuGet packages required toointeract with Azure Key Vault and Azure Active Directory.</span></span> <span data-ttu-id="efe05-152">Ange hello följande kommandon från hello Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="efe05-152">From hello Visual Studio Package Manager console, enter hello following commands.</span></span> <span data-ttu-id="efe05-153">Hello aktuella versionen av hello Azure Active Directory-paketet är 3.10.305231913, så att du kanske vill tooconfirm hello senaste versionen och uppdateras vid hello skrivning i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="efe05-153">At hello writing of this article, hello current version of hello Azure Active Directory package is 3.10.305231913, so you might want tooconfirm hello latest version and update accordingly.</span></span>

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

<span data-ttu-id="efe05-154">Skapa en klass toohold hello metod för din Azure Active Directory-autentisering i din programkod.</span><span class="sxs-lookup"><span data-stu-id="efe05-154">In your application code, create a class toohold hello method for your Azure Active Directory authentication.</span></span> <span data-ttu-id="efe05-155">I det här exemplet är klassen kallas **verktyg för webbplatsuppgradering**.</span><span class="sxs-lookup"><span data-stu-id="efe05-155">In this example, that class is called **Utils**.</span></span> <span data-ttu-id="efe05-156">Lägg till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="efe05-156">Add hello following using statement:</span></span>

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="efe05-157">Lägg till följande metod tooretrieve hello JWT-token från Azure Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="efe05-157">Next, add hello following method tooretrieve hello JWT token from Azure Active Directory.</span></span> <span data-ttu-id="efe05-158">Du kanske vill toomove hello hårdkodade strängvärden till webb- eller konfigurationen för underhålla.</span><span class="sxs-lookup"><span data-stu-id="efe05-158">For maintainability, you may want toomove hello hard-coded string values into your web or application configuration.</span></span>

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed tooobtain hello JWT token");

    return result.AccessToken;
}
```

<span data-ttu-id="efe05-159">Lägg till hello nödvändig kod toocall Key Vault och hämta det hemliga värdet.</span><span class="sxs-lookup"><span data-stu-id="efe05-159">Add hello necessary code toocall Key Vault and retrieve your secret value.</span></span> <span data-ttu-id="efe05-160">Först måste du lägga till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="efe05-160">First you must add hello following using statement:</span></span>

```csharp
using Microsoft.Azure.KeyVault;
```

<span data-ttu-id="efe05-161">Lägg till hello metoden anrop tooinvoke Key Vault och hämta ditt hemliga.</span><span class="sxs-lookup"><span data-stu-id="efe05-161">Add hello method calls tooinvoke Key Vault and retrieve your secret.</span></span> <span data-ttu-id="efe05-162">I den här metoden kan ange du hello hemlighet URI som du sparade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="efe05-162">In this method, you provide hello secret URI that you saved in a previous step.</span></span> <span data-ttu-id="efe05-163">Observera hello användning av hello **GetToken** metod från hello **verktyg för webbplatsuppgradering** klassen som skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="efe05-163">Note hello use of hello **GetToken** method from hello **Utils** class created previously.</span></span>

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

<span data-ttu-id="efe05-164">När du kör programmet bör du nu autentisera tooAzure Active Directory och sedan hämta din hemligt värde från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="efe05-164">When you run your application, you should now be authenticating tooAzure Active Directory and then retrieving your secret value from Azure Key Vault.</span></span>

## <a name="key-rotation-using-azure-automation"></a><span data-ttu-id="efe05-165">Viktiga rotation med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="efe05-165">Key rotation using Azure Automation</span></span>
<span data-ttu-id="efe05-166">Det finns olika alternativ för att implementera en rotation strategi för värden som du lagrar som Azure Key Vault hemligheter.</span><span class="sxs-lookup"><span data-stu-id="efe05-166">There are various options for implementing a rotation strategy for values you store as Azure Key Vault secrets.</span></span> <span data-ttu-id="efe05-167">Hemligheter kan roteras som en del av en manuell process, de kan roteras programmässigt med hjälp av API-anrop eller kan roteras med ett Automation-skript.</span><span class="sxs-lookup"><span data-stu-id="efe05-167">Secrets can be rotated as part of a manual process, they may be rotated programmatically by using API calls, or they may be rotated by way of an Automation script.</span></span> <span data-ttu-id="efe05-168">Hello enligt den här artikeln, du kan använda Azure PowerShell tillsammans med Azure Automation toochange snabbtangent Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="efe05-168">For hello purposes of this article, you will be using Azure PowerShell combined with Azure Automation toochange an Azure Storage Account access key.</span></span> <span data-ttu-id="efe05-169">Sedan uppdaterar du en hemlighet i nyckelvalvet med den nya nyckeln.</span><span class="sxs-lookup"><span data-stu-id="efe05-169">You will then update a key vault secret with that new key.</span></span>

<span data-ttu-id="efe05-170">tooallow Azure Automation tooset hemliga värden i nyckelvalvet, måste du hämta hello klient-ID för hello anslutningen med namnet AzureRunAsConnection som skapades när du har etablerat Azure Automation-instans.</span><span class="sxs-lookup"><span data-stu-id="efe05-170">tooallow Azure Automation tooset secret values in your key vault, you must get hello client ID for hello connection named AzureRunAsConnection, which was created when you established your Azure Automation instance.</span></span> <span data-ttu-id="efe05-171">Du hittar det här ID genom att välja **tillgångar** från Azure Automation-instans.</span><span class="sxs-lookup"><span data-stu-id="efe05-171">You can find this ID by choosing **Assets** from your Azure Automation instance.</span></span> <span data-ttu-id="efe05-172">Därifrån kan du välja **anslutningar** och välj sedan hello **AzureRunAsConnection** tjänsten principen.</span><span class="sxs-lookup"><span data-stu-id="efe05-172">From there, you choose **Connections** and then select hello **AzureRunAsConnection** service principle.</span></span> <span data-ttu-id="efe05-173">Anteckna hello **program-ID**.</span><span class="sxs-lookup"><span data-stu-id="efe05-173">Take note of hello **Application ID**.</span></span>

![Azure Automation-klient-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

<span data-ttu-id="efe05-175">I **tillgångar**, Välj **moduler**.</span><span class="sxs-lookup"><span data-stu-id="efe05-175">In **Assets**, choose **Modules**.</span></span> <span data-ttu-id="efe05-176">Från **moduler**väljer **galleriet**, och sök sedan efter och **importera** uppdaterade versioner av vart och ett av följande moduler hello:</span><span class="sxs-lookup"><span data-stu-id="efe05-176">From **Modules**, select **Gallery**, and then search for and **Import** updated versions of each of hello following modules:</span></span>

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> <span data-ttu-id="efe05-177">Vid hello skrivning av den här artikeln, hello tidigare noterats moduler som krävs för toobe uppdateras bara efter Hej följande skript.</span><span class="sxs-lookup"><span data-stu-id="efe05-177">At hello writing of this article, only hello previously noted modules needed toobe updated for hello following script.</span></span> <span data-ttu-id="efe05-178">Om du tycker att ditt automation-jobb inte fungerar kan du bekräfta att du har importerat alla moduler som krävs och deras beroenden.</span><span class="sxs-lookup"><span data-stu-id="efe05-178">If you find that your automation job is failing, confirm that you have imported all necessary modules and their dependencies.</span></span>
>
>

<span data-ttu-id="efe05-179">När du har hämtat hello program-ID för Azure Automation-anslutning, måste du se nyckelvalvet som det här programmet har åtkomst tooupdate hemligheter i ditt valv.</span><span class="sxs-lookup"><span data-stu-id="efe05-179">After you have retrieved hello application ID for your Azure Automation connection, you must tell your key vault that this application has access tooupdate secrets in your vault.</span></span> <span data-ttu-id="efe05-180">Detta kan åstadkommas med hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="efe05-180">This can be accomplished with hello following PowerShell command:</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

<span data-ttu-id="efe05-181">Välj därefter **Runbooks** under din Azure Automation-instans och välj sedan **lägga till en Runbook**.</span><span class="sxs-lookup"><span data-stu-id="efe05-181">Next, select **Runbooks** under your Azure Automation instance, and then select **Add a Runbook**.</span></span> <span data-ttu-id="efe05-182">Välj **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="efe05-182">Select **Quick Create**.</span></span> <span data-ttu-id="efe05-183">Din runbook och välj **PowerShell** som hello runbooktyp.</span><span class="sxs-lookup"><span data-stu-id="efe05-183">Name your runbook and select **PowerShell** as hello runbook type.</span></span> <span data-ttu-id="efe05-184">Du har hello alternativet tooadd en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="efe05-184">You have hello option tooadd a description.</span></span> <span data-ttu-id="efe05-185">Klicka slutligen på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="efe05-185">Finally, click **Create**.</span></span>

![Skapa runbook](./media/keyvault-keyrotation/Create_Runbook.png)

<span data-ttu-id="efe05-187">Klistra in följande PowerShell-skript i hello editor-fönstret för din nya runbook hello:</span><span class="sxs-lookup"><span data-stu-id="efe05-187">Paste hello following PowerShell script in hello editor pane for your new runbook:</span></span>

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get hello connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in tooAzure..."
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

#Optionally you may set hello following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for hello storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

<span data-ttu-id="efe05-188">Hello editor-fönstret, Välj **Test fönstret** tootest skriptet.</span><span class="sxs-lookup"><span data-stu-id="efe05-188">From hello editor pane, choose **Test pane** tootest your script.</span></span> <span data-ttu-id="efe05-189">När hello skriptet körs utan fel, kan du välja **publicera**, och du kan koppla ett schema för hello runbook tillbaka i hello runbook configuration rutan.</span><span class="sxs-lookup"><span data-stu-id="efe05-189">Once hello script is running without error, you can select **Publish**, and then you can apply a schedule for hello runbook back in hello runbook configuration pane.</span></span>

## <a name="key-vault-auditing-pipeline"></a><span data-ttu-id="efe05-190">Key Vault granskning pipeline</span><span class="sxs-lookup"><span data-stu-id="efe05-190">Key Vault auditing pipeline</span></span>
<span data-ttu-id="efe05-191">När du ställer in ett nyckelvalv som du kan aktivera granskningsloggar toocollect på toohello nyckelvalv för av åtkomstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="efe05-191">When you set up a key vault, you can turn on auditing toocollect logs on access requests made toohello key vault.</span></span> <span data-ttu-id="efe05-192">Dessa loggar lagras i ett avsedda Azure Storage-konto och kan hämtas, övervakas och analyseras.</span><span class="sxs-lookup"><span data-stu-id="efe05-192">These logs are stored in a designated Azure Storage account and can be pulled out, monitored, and analyzed.</span></span> <span data-ttu-id="efe05-193">hello använder följande scenario Azure functions, Azure logikappar och nyckelvalv granska loggarna toocreate pipeline-toosend ett e-postmeddelande när en app som matchar hello app-ID för hello webbapp hämtar hemligheter från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="efe05-193">hello following scenario uses Azure functions, Azure logic apps, and key vault audit logs toocreate a pipeline toosend an email when an app that does match hello app ID of hello web app retrieves secrets from hello vault.</span></span>

<span data-ttu-id="efe05-194">Först måste du aktivera loggning på nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="efe05-194">First, you must enable logging on your key vault.</span></span> <span data-ttu-id="efe05-195">Detta kan göras via hello följande PowerShell-kommandon (fullständig information kan visas vid [key vault loggning](key-vault-logging.md)):</span><span class="sxs-lookup"><span data-stu-id="efe05-195">This can be done via hello following PowerShell commands (full details can be seen at [key-vault-logging](key-vault-logging.md)):</span></span>

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

<span data-ttu-id="efe05-196">När den är aktiverad, granskningsloggar start samla i hello avses storage-konto.</span><span class="sxs-lookup"><span data-stu-id="efe05-196">After this is enabled, audit logs start collecting into hello designated storage account.</span></span> <span data-ttu-id="efe05-197">Dessa loggar innehålla händelser om hur och när ditt nyckelvalv kan nås och av vem.</span><span class="sxs-lookup"><span data-stu-id="efe05-197">These logs contain events about how and when your key vaults are accessed, and by whom.</span></span>

> [!NOTE]
> <span data-ttu-id="efe05-198">Du kan komma åt din loggningsinformation 10 minuter efter hello nyckelvalv igen.</span><span class="sxs-lookup"><span data-stu-id="efe05-198">You can access your logging information 10 minutes after hello key vault operation.</span></span> <span data-ttu-id="efe05-199">Det kommer vanligtvis vara snabbare än så.</span><span class="sxs-lookup"><span data-stu-id="efe05-199">It will usually be quicker than this.</span></span>
>
>

<span data-ttu-id="efe05-200">hello nästa steg är för[skapa en Azure Service Bus-kö](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="efe05-200">hello next step is too[create an Azure Service Bus queue](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span> <span data-ttu-id="efe05-201">Detta är där nyckelvalv granskningsloggar push.</span><span class="sxs-lookup"><span data-stu-id="efe05-201">This is where key vault audit logs are pushed.</span></span> <span data-ttu-id="efe05-202">När hello granskningsloggmeddelanden hello kön, hello logikapp hämtar dem och fungerar på dem..</span><span class="sxs-lookup"><span data-stu-id="efe05-202">When hello audit log messages are in hello queue, hello logic app picks them up and acts on them.</span></span> <span data-ttu-id="efe05-203">Skapa en service bus med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="efe05-203">Create a service bus with hello following steps:</span></span>

1. <span data-ttu-id="efe05-204">Skapa ett namnområde för Service Bus (om du redan har en som du vill toouse för det här hoppa över tooStep 2).</span><span class="sxs-lookup"><span data-stu-id="efe05-204">Create a Service Bus namespace (if you already have one that you want toouse for this, skip tooStep 2).</span></span>
2. <span data-ttu-id="efe05-205">Bläddra toohello service bus i hello Azure-portalen och väljer hello namnområde som du vill toocreate hello kön i.</span><span class="sxs-lookup"><span data-stu-id="efe05-205">Browse toohello service bus in hello Azure portal and select hello namespace you want toocreate hello queue in.</span></span>
3. <span data-ttu-id="efe05-206">Välj **ny** och välj **Service Bus > kön** och ange information om hello krävs.</span><span class="sxs-lookup"><span data-stu-id="efe05-206">Select **New** and choose **Service Bus > Queue** and enter hello required details.</span></span>
4. <span data-ttu-id="efe05-207">Markerar hello anslutningsinformationen för Service Bus genom att välja hello namnområde **anslutningsinformationen**.</span><span class="sxs-lookup"><span data-stu-id="efe05-207">Select hello Service Bus connection information by choosing hello namespace and clicking **Connection Information**.</span></span> <span data-ttu-id="efe05-208">Du behöver den här informationen för hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="efe05-208">You will need this information for hello next section.</span></span>

<span data-ttu-id="efe05-209">Nästa [skapa en Azure-funktion](../azure-functions/functions-create-first-azure-function.md) toopoll nyckelvalvet i hello storage-konto och hämta nya händelser.</span><span class="sxs-lookup"><span data-stu-id="efe05-209">Next, [create an Azure function](../azure-functions/functions-create-first-azure-function.md) toopoll key vault logs within hello storage account and pick up new events.</span></span> <span data-ttu-id="efe05-210">Det här är en funktion som utlöses enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="efe05-210">This will be a function that is triggered on a schedule.</span></span>

<span data-ttu-id="efe05-211">Välj toocreate en Azure-funktion **New > Funktionsapp** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="efe05-211">toocreate an Azure function, choose **New > Function App** in hello Azure portal.</span></span> <span data-ttu-id="efe05-212">Du kan använda en befintlig värd plan eller skapa en ny vid skapandet.</span><span class="sxs-lookup"><span data-stu-id="efe05-212">During creation, you can use an existing hosting plan or create a new one.</span></span> <span data-ttu-id="efe05-213">Du kan också välja dynamisk värd.</span><span class="sxs-lookup"><span data-stu-id="efe05-213">You could also opt for dynamic hosting.</span></span> <span data-ttu-id="efe05-214">Mer information om funktionen som värd för alternativ finns på [hur tooscale Azure Functions](../azure-functions/functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="efe05-214">More details on Function hosting options can be found at [How tooscale Azure Functions](../azure-functions/functions-scale.md).</span></span>

<span data-ttu-id="efe05-215">När hello Azure-funktion skapas navigerar tooit och väljer en timer funktionen och C\#.</span><span class="sxs-lookup"><span data-stu-id="efe05-215">When hello Azure function is created, navigate tooit and choose a timer function and C\#.</span></span> <span data-ttu-id="efe05-216">Klicka på **skapa den här funktionen**.</span><span class="sxs-lookup"><span data-stu-id="efe05-216">Then click **Create this function**.</span></span>

![Azure Functions starta bladet](./media/keyvault-keyrotation/Azure_Functions_Start.png)

<span data-ttu-id="efe05-218">På hello **utveckla** fliken ersätter hello run.csx kod med hello följande:</span><span class="sxs-lookup"><span data-stu-id="efe05-218">On hello **Develop** tab, replace hello run.csx code with hello following:</span></span>

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
            log.Verbose($"Sync point file didnt have a date. Setting toonow.");
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

            //required tooorder by time as they may not be in hello file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending tooServiceBus, use hello payloadStream and set keeporiginal tootrue
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

    //Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> <span data-ttu-id="efe05-219">Se till att tooreplace hello variabler i föregående kod toopoint tooyour storage-konto där hello nyckelvalv loggarna skrivs hello hello service bus som du skapade tidigare, och hello angiven sökväg toohello nyckelvalv lagring loggar.</span><span class="sxs-lookup"><span data-stu-id="efe05-219">Make sure tooreplace hello variables in hello preceding code toopoint tooyour storage account where hello key vault logs are written, hello service bus you created earlier, and hello specific path toohello key vault storage logs.</span></span>
>
>

<span data-ttu-id="efe05-220">hello funktionen hämtar hello senaste loggfilen från hello lagringskonto där hello nyckelvalv loggarna skrivs, grabs hello senaste händelser från filen, och skickar dem tooa Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="efe05-220">hello function picks up hello latest log file from hello storage account where hello key vault logs are written, grabs hello latest events from that file, and pushes them tooa Service Bus queue.</span></span> <span data-ttu-id="efe05-221">Eftersom en enda fil kan ha flera händelser, bör du skapa en sync.txt-fil som hello funktionen kontrollerar också toodetermine hello tidsstämpel hello senaste händelse som hämtades.</span><span class="sxs-lookup"><span data-stu-id="efe05-221">Since a single file could have multiple events, you should create a sync.txt file that hello function also looks at toodetermine hello time stamp of hello last event that was picked up.</span></span> <span data-ttu-id="efe05-222">Detta säkerställer att du inte push hello flera gånger för samma händelse.</span><span class="sxs-lookup"><span data-stu-id="efe05-222">This ensures that you don't push hello same event multiple times.</span></span> <span data-ttu-id="efe05-223">Sync.txt filen innehåller en tidsstämpel för senaste påträffade hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="efe05-223">This sync.txt file contains a timestamp for hello last encountered event.</span></span> <span data-ttu-id="efe05-224">hello loggar när har lästs in, har toobe sorteras utifrån hello tidsstämpel tooensure de sorteras på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="efe05-224">hello logs, when loaded, have toobe sorted based on hello timestamp tooensure they are ordered correctly.</span></span>

<span data-ttu-id="efe05-225">För den här funktionen referera vi några ytterligare bibliotek som inte är tillgängliga för out of box hello i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="efe05-225">For this function, we reference a couple of additional libraries that are not available out of hello box in Azure Functions.</span></span> <span data-ttu-id="efe05-226">tooinclude, behöver vi Azure Functions toopull dem med hjälp av NuGet.</span><span class="sxs-lookup"><span data-stu-id="efe05-226">tooinclude these, we need Azure Functions toopull them using NuGet.</span></span> <span data-ttu-id="efe05-227">Välj hello **visa filer** alternativet.</span><span class="sxs-lookup"><span data-stu-id="efe05-227">Choose hello **View Files** option.</span></span>

![Visa filer](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

<span data-ttu-id="efe05-229">Och Lägg till en fil som heter project.json med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="efe05-229">And add a file called project.json with following content:</span></span>

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
<span data-ttu-id="efe05-230">Vid **spara**, Azure Functions hämtar hello krävs binärfiler.</span><span class="sxs-lookup"><span data-stu-id="efe05-230">Upon **Save**, Azure Functions will download hello required binaries.</span></span>

<span data-ttu-id="efe05-231">Växla toohello **integrera** fliken och ge hello timer parametern ett beskrivande namn toouse i hello-funktion.</span><span class="sxs-lookup"><span data-stu-id="efe05-231">Switch toohello **Integrate** tab and give hello timer parameter a meaningful name toouse within hello function.</span></span> <span data-ttu-id="efe05-232">Hello föregående kod, den förväntar sig hello timer toobe kallas *myTimer*.</span><span class="sxs-lookup"><span data-stu-id="efe05-232">In hello preceding code, it expects hello timer toobe called *myTimer*.</span></span> <span data-ttu-id="efe05-233">Ange en [CRON-uttryck](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) enligt följande: 0 \* \* \* \* \* för hello timer som gör att hello funktionen toorun en gång i minuten.</span><span class="sxs-lookup"><span data-stu-id="efe05-233">Specify a [CRON expression](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) as follows: 0 \* \* \* \* \* for hello timer that will cause hello function toorun once a minute.</span></span>

<span data-ttu-id="efe05-234">Hej på samma **integrera** lägger du till indata av typen hello **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="efe05-234">On hello same **Integrate** tab, add an input of hello type **Azure Blob Storage**.</span></span> <span data-ttu-id="efe05-235">Detta kommer att peka toohello sync.txt-fil som innehåller hello tidsstämpel för hello sista händelsen granskats av hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="efe05-235">This will point toohello sync.txt file that contains hello timestamp of hello last event looked at by hello function.</span></span> <span data-ttu-id="efe05-236">Det här är tillgängliga i hello-funktion av hello parameternamnet.</span><span class="sxs-lookup"><span data-stu-id="efe05-236">This will be available within hello function by hello parameter name.</span></span> <span data-ttu-id="efe05-237">Hello föregående kod, hello Azure Blob Storage indata förväntar sig hello parametern name toobe *inputBlob*.</span><span class="sxs-lookup"><span data-stu-id="efe05-237">In hello preceding code, hello Azure Blob Storage input expects hello parameter name toobe *inputBlob*.</span></span> <span data-ttu-id="efe05-238">Välj hello storage-konto där hello sync.txt filen ska placeras (det kan vara hello samma eller ett annat lagringskonto).</span><span class="sxs-lookup"><span data-stu-id="efe05-238">Choose hello storage account where hello sync.txt file will reside (it could be hello same or a different storage account).</span></span> <span data-ttu-id="efe05-239">Ange i hello sökvägsfältet hello sökväg där filen hello bor i hello formatet {container-name}/path/to/sync.txt.</span><span class="sxs-lookup"><span data-stu-id="efe05-239">In hello path field, provide hello path where hello file lives in hello format {container-name}/path/to/sync.txt.</span></span>

<span data-ttu-id="efe05-240">Lägga till utdata av hello typen *Azure Blob Storage* utdata.</span><span class="sxs-lookup"><span data-stu-id="efe05-240">Add an output of hello type *Azure Blob Storage* output.</span></span> <span data-ttu-id="efe05-241">Detta kommer att peka toohello sync.txt filen som du har definierat i hello indata.</span><span class="sxs-lookup"><span data-stu-id="efe05-241">This will point toohello sync.txt file you defined in hello input.</span></span> <span data-ttu-id="efe05-242">Detta används av hello funktionen toowrite hello tidsstämpel för hello sista händelsen tittar på.</span><span class="sxs-lookup"><span data-stu-id="efe05-242">This is used by hello function toowrite hello timestamp of hello last event looked at.</span></span> <span data-ttu-id="efe05-243">hello föregående kod förväntar sig den här parametern toobe kallas *outputBlob*.</span><span class="sxs-lookup"><span data-stu-id="efe05-243">hello preceding code expects this parameter toobe called *outputBlob*.</span></span>

<span data-ttu-id="efe05-244">Hello funktion är nu klar.</span><span class="sxs-lookup"><span data-stu-id="efe05-244">At this point, hello function is ready.</span></span> <span data-ttu-id="efe05-245">Se till att tooswitch tillbaka toohello **utveckla** fliken och spara hello kod.</span><span class="sxs-lookup"><span data-stu-id="efe05-245">Make sure tooswitch back toohello **Develop** tab and save hello code.</span></span> <span data-ttu-id="efe05-246">Kontrollera hello utdatafönstret för några kompileringsfel och korrigera dem i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="efe05-246">Check hello output window for any compilation errors and correct them accordingly.</span></span> <span data-ttu-id="efe05-247">Om hello kod kompileras hello koden ska nu kontrollera hello nyckelvalv loggar varje minut och trycka på alla nya händelser till hello definierats Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="efe05-247">If hello code compiles, then hello code should now be checking hello key vault logs every minute and pushing any new events onto hello defined Service Bus queue.</span></span> <span data-ttu-id="efe05-248">Du bör se loggningsinformation skriva ut toohello fönstret varje gång hello funktionen utlöses.</span><span class="sxs-lookup"><span data-stu-id="efe05-248">You should see logging information write out toohello log window every time hello function is triggered.</span></span>

### <a name="azure-logic-app"></a><span data-ttu-id="efe05-249">Azure logikapp</span><span class="sxs-lookup"><span data-stu-id="efe05-249">Azure logic app</span></span>
<span data-ttu-id="efe05-250">Nu måste du skapa ett Azure logikappen som hämtar hello händelser att hello funktion gör att toohello Service Bus-kö Parsar hello innehåll och skickar ett e-post baserat på ett villkor som matchas.</span><span class="sxs-lookup"><span data-stu-id="efe05-250">Next you must create an Azure logic app that picks up hello events that hello function is pushing toohello Service Bus queue, parses hello content, and sends an email based on a condition being matched.</span></span>

<span data-ttu-id="efe05-251">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) genom att gå för**New > Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="efe05-251">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) by going too**New > Logic App**.</span></span>

<span data-ttu-id="efe05-252">När du har skapat hello logikapp navigerar tooit och väljer **redigera**.</span><span class="sxs-lookup"><span data-stu-id="efe05-252">Once hello logic app is created, navigate tooit and choose **edit**.</span></span> <span data-ttu-id="efe05-253">Hello logik app Editor väljer **Service Bus-kö** och ange dina autentiseringsuppgifter för Service Bus-tooconnect den toohello kön.</span><span class="sxs-lookup"><span data-stu-id="efe05-253">Within hello logic app editor, choose **Service Bus Queue** and enter your Service Bus credentials tooconnect it toohello queue.</span></span>

![Logik för Azure App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

<span data-ttu-id="efe05-255">Välj nästa **Lägg till ett villkor**.</span><span class="sxs-lookup"><span data-stu-id="efe05-255">Next choose **Add a condition**.</span></span> <span data-ttu-id="efe05-256">Växla toohello Avancerad redigerare i hello villkor och ange hello följande kod, ersätta APP_ID med hello faktiska APP_ID av ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="efe05-256">In hello condition, switch toohello advanced editor and enter hello following code, replacing APP_ID with hello actual APP_ID of your web app:</span></span>

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

<span data-ttu-id="efe05-257">Det här uttrycket returnerar i stort sett **FALSKT** om hello *appid* från hello inkommande händelse (vilket är hello hello Service Bus meddelandets text) är inte hello *appid* av hello App.</span><span class="sxs-lookup"><span data-stu-id="efe05-257">This expression essentially returns **false** if hello *appid* from hello incoming event (which is hello body of hello Service Bus message) is not hello *appid* of hello app.</span></span>

<span data-ttu-id="efe05-258">Nu ska du skapa en åtgärd under **om Nej, gör ingenting**.</span><span class="sxs-lookup"><span data-stu-id="efe05-258">Now, create an action under **If no, do nothing**.</span></span>

![Azure Logikapp välja åtgärd](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

<span data-ttu-id="efe05-260">Hello-åtgärd, Välj **Office 365 – skicka e-post**.</span><span class="sxs-lookup"><span data-stu-id="efe05-260">For hello action, choose **Office 365 - send email**.</span></span> <span data-ttu-id="efe05-261">Fyll i hello fält toocreate toosend en e-post när hello definierade villkoret returnerar **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="efe05-261">Fill out hello fields toocreate an email toosend when hello defined condition returns **false**.</span></span> <span data-ttu-id="efe05-262">Om du inte har Office 365 kan du titta på alternativ tooachieve hello samma resultat.</span><span class="sxs-lookup"><span data-stu-id="efe05-262">If you do not have Office 365, you could look at alternatives tooachieve hello same results.</span></span>

<span data-ttu-id="efe05-263">Du har nu en end tooend pipeline som söker efter nya nyckelvalv granskningsloggar en gång i minuten.</span><span class="sxs-lookup"><span data-stu-id="efe05-263">At this point, you have an end tooend pipeline that looks for new key vault audit logs once a minute.</span></span> <span data-ttu-id="efe05-264">Den skickar nya loggar påträffas tooa service bus-kö.</span><span class="sxs-lookup"><span data-stu-id="efe05-264">It pushes new logs it finds tooa service bus queue.</span></span> <span data-ttu-id="efe05-265">Hej logikapp utlöses när ett nytt meddelande hamnar i hello kön.</span><span class="sxs-lookup"><span data-stu-id="efe05-265">hello logic app is triggered when a new message lands in hello queue.</span></span> <span data-ttu-id="efe05-266">Om hello *appid* inom hello matchar inte händelse hello app-ID för hello anropar programmet, skickas ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="efe05-266">If hello *appid* within hello event does not match hello app ID of hello calling application, it sends an email.</span></span>
