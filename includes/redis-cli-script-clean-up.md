## <a name="clean-up-deployment"></a><span data-ttu-id="81264-101">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="81264-101">Clean up deployment</span></span> 

<span data-ttu-id="81264-102">Efter hello skriptexempel har körts kan hello Följ kommandot vara används tooremove hello resursgrupp, Azure Redis-Cache-instans och relaterade resurser i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="81264-102">After hello script sample has been run, hello follow command can be used tooremove hello resource group, Azure Redis Cache instance, and any related resources in hello resource group.</span></span>

```azurecli
az group delete --name contosoGroup
```