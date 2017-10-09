1. <span data-ttu-id="60b60-101">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="60b60-101">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> <span data-ttu-id="60b60-102">Mer information om att logga in finns i [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="60b60-102">For more information about logging in, see [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

  ```azurecli
  az login
  ```
2. <span data-ttu-id="60b60-103">Om du har mer än en Azure-prenumeration, lista hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="60b60-103">If you have more than one Azure subscription, list hello subscriptions for hello account.</span></span>

  ```azurecli
  az account list --all
  ```
3. <span data-ttu-id="60b60-104">Ange hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="60b60-104">Specify hello subscription that you want toouse.</span></span>

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```