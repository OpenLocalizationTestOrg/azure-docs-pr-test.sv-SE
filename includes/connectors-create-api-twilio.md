### <a name="prerequisites"></a><span data-ttu-id="80fb7-101">Krav</span><span class="sxs-lookup"><span data-stu-id="80fb7-101">Prerequisites</span></span>
* <span data-ttu-id="80fb7-102">Ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="80fb7-102">A Twilio account</span></span>
* <span data-ttu-id="80fb7-103">Ett verifierade Twilio-telefonnummer som kan ta emot SMS</span><span class="sxs-lookup"><span data-stu-id="80fb7-103">A verified Twilio phone number that can receive SMS</span></span>
* <span data-ttu-id="80fb7-104">Ett verifierade Twilio-telefonnummer som kan skicka SMS</span><span class="sxs-lookup"><span data-stu-id="80fb7-104">A verified Twilio phone number that can send SMS</span></span>

> [!NOTE]
> <span data-ttu-id="80fb7-105">Om du använder en utvärderingsversion Twilio-konto, du kan bara skicka SMS för**verifieras** telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="80fb7-105">If you are using a Twilio trial account, you can only send SMS too**verified** phone numbers.</span></span>  
> 
> 

<span data-ttu-id="80fb7-106">Innan du kan använda ditt Twilio-konto i en logikapp, måste du godkänna hello logik app tooconnect tooyour Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="80fb7-106">Before you can use your Twilio account in a Logic app, you must authorize hello Logic app tooconnect tooyour Twilio account.</span></span> <span data-ttu-id="80fb7-107">Lyckligtvis kan du göra detta direkt i din logikapp på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80fb7-107">Fortunately, you can do this easily from within your Logic app on hello Azure Portal.</span></span> 

<span data-ttu-id="80fb7-108">Här följer hello steg tooauthorize din logik app tooconnect tooyour Twilio-konto:</span><span class="sxs-lookup"><span data-stu-id="80fb7-108">Here are hello steps tooauthorize your Logic app tooconnect tooyour Twilio account:</span></span>

1. <span data-ttu-id="80fb7-109">toocreate en anslutning tooTwilio, i hello logik app designer väljer **visa Microsoft hanterade API: er** i hello listrutan och ange sedan *Twilio* i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="80fb7-109">toocreate a connection tooTwilio, in hello Logic app designer, select **Show Microsoft managed APIs** in hello drop down list then enter *Twilio* in hello search box.</span></span> <span data-ttu-id="80fb7-110">Välj hello utlösare eller åtgärden som du kommer att gilla toouse:</span><span class="sxs-lookup"><span data-stu-id="80fb7-110">Select hello trigger or action you'll like toouse:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. <span data-ttu-id="80fb7-111">Om du inte skapat några anslutningar tooTwilio innan får du ange tooprovide Twilio-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="80fb7-111">If you haven't created any connections tooTwilio before, you'll get prompted tooprovide your Twilio credentials.</span></span> <span data-ttu-id="80fb7-112">Dessa autentiseringsuppgifter att använda tooauthorize din logik app tooconnect till och komma åt data i ditt Twilio-konto:</span><span class="sxs-lookup"><span data-stu-id="80fb7-112">These credentials will be used tooauthorize your Logic app tooconnect to, and access your Twilio account's data:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. <span data-ttu-id="80fb7-113">Du behöver hello **Twilio-konto-id** och **Twilio åtkomsttoken** hello instrumentpanel i Twilio, så logga in i tooyour Twilio-konto nu toograb dessa två typer av information:</span><span class="sxs-lookup"><span data-stu-id="80fb7-113">You'll need hello **Twilio account id** and **Twilio access token**  from hello dashboard in Twilio, so log in tooyour Twilio account now toograb these two pieces of information:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. <span data-ttu-id="80fb7-114">Twilio och Logic apps använda olika namn tooidentify dessa två typer av information.</span><span class="sxs-lookup"><span data-stu-id="80fb7-114">Twilio and Logic apps use different names tooidentify these two pieces of infomation.</span></span> <span data-ttu-id="80fb7-115">Här är hur måste du mappa dem toohello Logic apps dialogrutan:![](./media/connectors-create-api-twilio/twilio-3.png)</span><span class="sxs-lookup"><span data-stu-id="80fb7-115">Here is how you must map them toohello Logic apps dialog: ![](./media/connectors-create-api-twilio/twilio-3.png)</span></span>  
5. <span data-ttu-id="80fb7-116">Välj hello **Skapa anslutning** knappen:</span><span class="sxs-lookup"><span data-stu-id="80fb7-116">Select hello **Create connection** button:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. <span data-ttu-id="80fb7-117">Observera hello anslutningen har skapats och du är nu ledigt tooproceed med hello andra steg i din logikapp:</span><span class="sxs-lookup"><span data-stu-id="80fb7-117">Notice hello connection has been created and you are now free tooproceed with hello other steps in your Logic app:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

