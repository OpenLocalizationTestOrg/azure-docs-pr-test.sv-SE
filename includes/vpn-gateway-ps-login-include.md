<span data-ttu-id="f6e4e-101">Innan du påbörjar konfigurationen måste du logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f6e4e-101">Before beginning this configuration, you must log in to your Azure account.</span></span> <span data-ttu-id="f6e4e-102">Den här cmdleten uppmanar dig att ange inloggningsuppgifterna för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f6e4e-102">The cmdlet prompts you for the login credentials for your Azure account.</span></span> <span data-ttu-id="f6e4e-103">När du har loggat in hämtas dina kontoinställningar så att de blir tillgängliga för Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f6e4e-103">After logging in, it downloads your account settings so they are available to Azure PowerShell.</span></span> <span data-ttu-id="f6e4e-104">Mer information finns i [Använda Windows PowerShell med Resource Manager](../articles/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="f6e4e-104">For more information, see [Using Windows PowerShell with Resource Manager](../articles/powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="f6e4e-105">Logga in genom att öppna PowerShell-konsolen med utökad behörighet och anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="f6e4e-105">To log in, open your PowerShell console with elevated privileges, and connect to your account.</span></span> <span data-ttu-id="f6e4e-106">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="f6e4e-106">Use the following example to help you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="f6e4e-107">Om du har flera Azure-prenumerationer anger du prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="f6e4e-107">If you have multiple Azure subscriptions, check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="f6e4e-108">Ange den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="f6e4e-108">Specify the subscription that you want to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```