<span data-ttu-id="554e2-101">Konfigurera lokal Git-distribution toohello webbapp med hello [az webapp distribution källa config-lokal-git](/cli/azure/webapp/deployment/source#config-local-git) kommando.</span><span class="sxs-lookup"><span data-stu-id="554e2-101">Configure local Git deployment toohello web app with hello [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command.</span></span>

<span data-ttu-id="554e2-102">Apptjänst har stöd för flera olika sätt toodeploy innehåll tooa webbprogram, till exempel FTP, lokal Git, GitHub, Visual Studio Team Services och Bitbucket.</span><span class="sxs-lookup"><span data-stu-id="554e2-102">App Service supports several ways toodeploy content tooa web app, such as FTP, local Git, GitHub, Visual Studio Team Services, and Bitbucket.</span></span> <span data-ttu-id="554e2-103">I den här snabbstarten ska vi distribuera med hjälp av lokala Git.</span><span class="sxs-lookup"><span data-stu-id="554e2-103">For this quickstart, you deploy by using local Git.</span></span> <span data-ttu-id="554e2-104">Det innebär att du distribuerar med en Git-kommandot toopush från en lokal databas tooa databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="554e2-104">That means you deploy by using a Git command toopush from a local repository tooa repository in Azure.</span></span> 

<span data-ttu-id="554e2-105">Följande kommando, Ersätt i hello  *\<appnamn >* med namnet på ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="554e2-105">In hello following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="554e2-106">hello utdata har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="554e2-106">hello output has hello following format:</span></span>

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

<span data-ttu-id="554e2-107">Hej `<username>` är hello [distribution användaren](#configure-a-deployment-user) som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="554e2-107">hello `<username>` is hello [deployment user](#configure-a-deployment-user) that you created in a previous step.</span></span>

<span data-ttu-id="554e2-108">Kopiera hello URI visas; du ska använda i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="554e2-108">Copy hello URI shown; you'll use it in hello next step.</span></span>
