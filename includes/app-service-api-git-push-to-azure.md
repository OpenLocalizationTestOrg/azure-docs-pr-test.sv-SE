<span data-ttu-id="82f2d-101">Använd hello Azure CLI tooget hello Fjärrdistribution URL för API-appen.</span><span class="sxs-lookup"><span data-stu-id="82f2d-101">Use hello Azure CLI tooget hello remote deployment URL for your API App.</span></span> <span data-ttu-id="82f2d-102">Följande kommando, Ersätt i hello  *\<appnamn >* med namnet på ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="82f2d-102">In hello following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="82f2d-103">Konfigurera din lokala Git-distribution toobe kan toopush toohello fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="82f2d-103">Configure your local Git deployment toobe able toopush toohello remote.</span></span>

```bash
git remote add azure <URI from previous step>
```

<span data-ttu-id="82f2d-104">Skicka toohello Azure remote toodeploy din app.</span><span class="sxs-lookup"><span data-stu-id="82f2d-104">Push toohello Azure remote toodeploy your app.</span></span> <span data-ttu-id="82f2d-105">Du ombeds hello lösenord som du skapade tidigare när du skapade hello distribution användare.</span><span class="sxs-lookup"><span data-stu-id="82f2d-105">You are prompted for hello password you created earlier when you created hello deployment user.</span></span> <span data-ttu-id="82f2d-106">Kontrollera att du anger hello lösenordet du skapade i tidigare i hello quickstart och inte hello lösenord du använder toolog i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="82f2d-106">Make sure that you enter hello password you created in earlier in hello quickstart, and not hello password you use toolog in toohello Azure portal.</span></span>

```bash
git push azure master
```
