<span data-ttu-id="eaa94-101">Innan du påbörjar den här konfigurationen måste du logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="eaa94-101">Before beginning this configuration, you must log in tooyour Azure account.</span></span> <span data-ttu-id="eaa94-102">hello cmdlet efterfrågar hello inloggningsuppgifterna för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="eaa94-102">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="eaa94-103">När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eaa94-103">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span> <span data-ttu-id="eaa94-104">Mer information finns i [Använda Windows PowerShell med Resource Manager](../articles/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="eaa94-104">For more information, see [Using Windows PowerShell with Resource Manager](../articles/powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="eaa94-105">toolog, öppna PowerShell-konsol med utökade privilegier och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="eaa94-105">toolog in, open your PowerShell console with elevated privileges, and connect tooyour account.</span></span> <span data-ttu-id="eaa94-106">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="eaa94-106">Use hello following example toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="eaa94-107">Om du har flera Azure-prenumerationer, kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="eaa94-107">If you have multiple Azure subscriptions, check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="eaa94-108">Ange hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="eaa94-108">Specify hello subscription that you want toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```