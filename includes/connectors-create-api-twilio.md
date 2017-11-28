### <a name="prerequisites"></a><span data-ttu-id="c631c-101">Krav</span><span class="sxs-lookup"><span data-stu-id="c631c-101">Prerequisites</span></span>
* <span data-ttu-id="c631c-102">Ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="c631c-102">A Twilio account</span></span>
* <span data-ttu-id="c631c-103">Ett verifierade Twilio-telefonnummer som kan ta emot SMS</span><span class="sxs-lookup"><span data-stu-id="c631c-103">A verified Twilio phone number that can receive SMS</span></span>
* <span data-ttu-id="c631c-104">Ett verifierade Twilio-telefonnummer som kan skicka SMS</span><span class="sxs-lookup"><span data-stu-id="c631c-104">A verified Twilio phone number that can send SMS</span></span>

> [!NOTE]
> <span data-ttu-id="c631c-105">Om du använder en utvärderingsversion Twilio-konto kan du bara skicka SMS till **verifieras** telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="c631c-105">If you are using a Twilio trial account, you can only send SMS to **verified** phone numbers.</span></span>  
> 
> 

<span data-ttu-id="c631c-106">Innan du kan använda ditt Twilio-konto i en logikapp, måste du godkänna logik för att ansluta till ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="c631c-106">Before you can use your Twilio account in a Logic app, you must authorize the Logic app to connect to your Twilio account.</span></span> <span data-ttu-id="c631c-107">Lyckligtvis kan du göra detta direkt i din logikapp på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c631c-107">Fortunately, you can do this easily from within your Logic app on the Azure Portal.</span></span> 

<span data-ttu-id="c631c-108">Här följer stegen för att verifiera din logikapp för att ansluta till ditt Twilio-konto:</span><span class="sxs-lookup"><span data-stu-id="c631c-108">Here are the steps to authorize your Logic app to connect to your Twilio account:</span></span>

1. <span data-ttu-id="c631c-109">Om du vill skapa en anslutning till Twilio, i logik app designer **visa Microsoft hanterade API: er** i nedrullningsbara listan anger *Twilio* i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="c631c-109">To create a connection to Twilio, in the Logic app designer, select **Show Microsoft managed APIs** in the drop down list then enter *Twilio* in the search box.</span></span> <span data-ttu-id="c631c-110">Välj utlösaren eller åtgärd du vill använda:</span><span class="sxs-lookup"><span data-stu-id="c631c-110">Select the trigger or action you'll like to use:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. <span data-ttu-id="c631c-111">Om du inte har skapat alla anslutningar till Twilio innan du kan hämta uppmanas du att ange Twilio-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c631c-111">If you haven't created any connections to Twilio before, you'll get prompted to provide your Twilio credentials.</span></span> <span data-ttu-id="c631c-112">Dessa autentiseringsuppgifter används för att auktorisera din logikapp för att ansluta till och komma åt data i ditt Twilio-konto:</span><span class="sxs-lookup"><span data-stu-id="c631c-112">These credentials will be used to authorize your Logic app to connect to, and access your Twilio account's data:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. <span data-ttu-id="c631c-113">Du behöver den **Twilio-konto-id** och **Twilio åtkomsttoken** från instrumentpanelen i Twilio, så logga in på ditt Twilio-konto nu att hämta dessa två typer av information:</span><span class="sxs-lookup"><span data-stu-id="c631c-113">You'll need the **Twilio account id** and **Twilio access token**  from the dashboard in Twilio, so log in to your Twilio account now to grab these two pieces of information:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. <span data-ttu-id="c631c-114">Twilio och Logic apps använda olika namn för att identifiera dessa två typer av information.</span><span class="sxs-lookup"><span data-stu-id="c631c-114">Twilio and Logic apps use different names to identify these two pieces of infomation.</span></span> <span data-ttu-id="c631c-115">Här är hur måste du mappa dem till dialogrutan Logic apps:![](./media/connectors-create-api-twilio/twilio-3.png)</span><span class="sxs-lookup"><span data-stu-id="c631c-115">Here is how you must map them to the Logic apps dialog: ![](./media/connectors-create-api-twilio/twilio-3.png)</span></span>  
5. <span data-ttu-id="c631c-116">Välj den **Skapa anslutning** knappen:</span><span class="sxs-lookup"><span data-stu-id="c631c-116">Select the **Create connection** button:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. <span data-ttu-id="c631c-117">Observera att anslutningen har skapats och du kan nu välja att fortsätta med andra steg i din logikapp:</span><span class="sxs-lookup"><span data-stu-id="c631c-117">Notice the connection has been created and you are now free to proceed with the other steps in your Logic app:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

