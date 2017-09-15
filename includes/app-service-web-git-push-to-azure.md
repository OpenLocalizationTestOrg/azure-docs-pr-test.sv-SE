## <a name="push-to-azure-from-git"></a><span data-ttu-id="6d9b1-101">Skicka till Azure från Git</span><span class="sxs-lookup"><span data-stu-id="6d9b1-101">Push to Azure from Git</span></span>

<span data-ttu-id="6d9b1-102">Lägg till en Azure-fjärrdatabas till din lokala Git-databas.</span><span class="sxs-lookup"><span data-stu-id="6d9b1-102">Add an Azure remote to your local Git repository.</span></span>

```bash
git remote add azure <URI from previous step>
```

<span data-ttu-id="6d9b1-103">Skicka till Azure-fjärrdatabasen för att distribuera appen.</span><span class="sxs-lookup"><span data-stu-id="6d9b1-103">Push to the Azure remote to deploy your app.</span></span> <span data-ttu-id="6d9b1-104">Du uppmanas att ange lösenordet som du skapade tidigare när du skapade distributionsanvändaren.</span><span class="sxs-lookup"><span data-stu-id="6d9b1-104">You are prompted for the password you created earlier when you created the deployment user.</span></span> <span data-ttu-id="6d9b1-105">Se till att du anger det lösenord som du skapade i [Konfigurera en distributionsanvändare](#configure-a-deployment-user), inte lösenordet du använde när du loggade in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="6d9b1-105">Make sure that you enter the password you created in [Configure a deployment user](#configure-a-deployment-user), not the password you use to log in to the Azure portal.</span></span>

```bash
git push azure master
```

<span data-ttu-id="6d9b1-106">Föregående kommando visar information liknande den i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="6d9b1-106">The preceding command displays information similar to the following example:</span></span>
