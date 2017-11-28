<span data-ttu-id="fa595-101">När posterna för domännamnet har spridits måste du koppla dem till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fa595-101">After the records for your domain name have propagated, you must associate them with your Web App.</span></span> <span data-ttu-id="fa595-102">Använd följande steg för att aktivera domännamn med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="fa595-102">Use the following steps to enable the domain names using your web browser.</span></span>

> [!NOTE]
> <span data-ttu-id="fa595-103">Det kan ta lite tid för TXT-poster som skapats i föregående steg och spridas via DNS-systemet.</span><span class="sxs-lookup"><span data-stu-id="fa595-103">It can take some time for TXT records created in the previous steps to propagate through the DNS system.</span></span> <span data-ttu-id="fa595-104">Du kan inte lägga till domännamnet för din webbapp tills TXT-posten har spridits.</span><span class="sxs-lookup"><span data-stu-id="fa595-104">You cannot add the domain name of to your web app until the TXT record has propagated.</span></span> <span data-ttu-id="fa595-105">Om du använder en A-post kan du inte lägga till domännamnet för en post ditt webbprogram tills du skapade i föregående steg TXT-posten har spridits.</span><span class="sxs-lookup"><span data-stu-id="fa595-105">If you are using an A record, you cannot add the A record domain name to your web app until the TXT record created in the previous step has propagated.</span></span>
> 
> <span data-ttu-id="fa595-106">Du kan använda en tjänst som <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> att verifiera att TXT-posten är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="fa595-106">You can use a service such as <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> to verify that the TXT record is available.</span></span>
> 
> 

1. <span data-ttu-id="fa595-107">I webbläsaren och öppna den [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa595-107">In your browser, open the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fa595-108">I den **Web Apps** , klicka på namnet på ditt webbprogram, och välj sedan **anpassade domäner**</span><span class="sxs-lookup"><span data-stu-id="fa595-108">In the **Web Apps** tab, click the name of your web app, and then select **Custom domains**</span></span>
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. <span data-ttu-id="fa595-109">I den **anpassade domäner** bladet, klickar du på **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="fa595-109">In the **Custom domains** blade, click **Add hostname**.</span></span>
4. <span data-ttu-id="fa595-110">Använd den **värdnamn** textrutor för att ange de domännamn som ska associeras med det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="fa595-110">Use the **Hostname** text boxes to enter the domain names to associate with this web app.</span></span>
   
    ![](./media/custom-dns-web-site/add-custom-domain.png)
5. <span data-ttu-id="fa595-111">Klicka på **Validera**.</span><span class="sxs-lookup"><span data-stu-id="fa595-111">Click **Validate**.</span></span>
6. <span data-ttu-id="fa595-112">När du klickar på **verifiera** Azure kommer startar domänverifiering arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="fa595-112">Upon clicking **Validate** Azure will kick off Domain Verification workflow.</span></span> <span data-ttu-id="fa595-113">Detta kommer att söka efter ägarskap för domänen som värdnamn tillgänglighet och rapporten slutfört eller detaljerade fel med normativ guidence om hur du åtgärdar felet.</span><span class="sxs-lookup"><span data-stu-id="fa595-113">This will check for Domain ownership as well as Hostname availability and report success or detailed error with prescriptive guidence on how to fix the error.</span></span>    

<span data-ttu-id="fa595-114">Nu ska du kunna ange det anpassade domännamnet i din webbläsare och se att det har tar dig till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fa595-114">At this point, you should be able to enter the custom domain name in your browser and see that it successfully takes you to your web app.</span></span>

