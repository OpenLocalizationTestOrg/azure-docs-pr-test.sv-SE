<span data-ttu-id="05de0-101">Använd Azure CLI för att hämta URL:en för fjärrdistribution för din API-app.</span><span class="sxs-lookup"><span data-stu-id="05de0-101">Use the Azure CLI to get the remote deployment URL for your API App.</span></span> <span data-ttu-id="05de0-102">Ersätt *\<app_name>* med din webbapps namn i följande kommando.</span><span class="sxs-lookup"><span data-stu-id="05de0-102">In the following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="05de0-103">Konfigurera din lokala Git-distribution för att kunna push-överföra till en fjärrplats.</span><span class="sxs-lookup"><span data-stu-id="05de0-103">Configure your local Git deployment to be able to push to the remote.</span></span>

```bash
git remote add azure <URI from previous step>
```

<span data-ttu-id="05de0-104">Skicka till Azure-fjärrdatabasen för att distribuera appen.</span><span class="sxs-lookup"><span data-stu-id="05de0-104">Push to the Azure remote to deploy your app.</span></span> <span data-ttu-id="05de0-105">Du uppmanas att ange lösenordet som du skapade tidigare när du skapade distributionsanvändaren.</span><span class="sxs-lookup"><span data-stu-id="05de0-105">You are prompted for the password you created earlier when you created the deployment user.</span></span> <span data-ttu-id="05de0-106">Se till att du anger det lösenord som du skapade tidigare i snabbstarten, inte lösenordet som du använde när du loggade in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="05de0-106">Make sure that you enter the password you created in earlier in the quickstart, and not the password you use to log in to the Azure portal.</span></span>

```bash
git push azure master
```
