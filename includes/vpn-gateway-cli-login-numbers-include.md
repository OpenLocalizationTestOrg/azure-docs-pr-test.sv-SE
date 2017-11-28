1. <span data-ttu-id="1e81e-101">Logga in på Azure-prenumerationen med kommandot [az login](/cli/azure/#login) och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="1e81e-101">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> <span data-ttu-id="1e81e-102">Mer information om att logga in finns i [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1e81e-102">For more information about logging in, see [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

  ```azurecli
  az login
  ```
2. <span data-ttu-id="1e81e-103">Om du har fler än en Azure-prenumeration anger du prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="1e81e-103">If you have more than one Azure subscription, list the subscriptions for the account.</span></span>

  ```azurecli
  az account list --all
  ```
3. <span data-ttu-id="1e81e-104">Ange den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="1e81e-104">Specify the subscription that you want to use.</span></span>

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```