<span data-ttu-id="f687d-101">tooenable detaljerade för återställning av lösenord för ditt program, behöver du toocreate en princip för lösenordsåterställning.</span><span class="sxs-lookup"><span data-stu-id="f687d-101">tooenable fine-grained password reset on your application, you will need toocreate a password reset policy.</span></span> <span data-ttu-id="f687d-102">Observera att hello klient hela lösenordsåterställning alternativ har angetts [här](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span><span class="sxs-lookup"><span data-stu-id="f687d-102">Note that hello tenant-wide password reset option specified [here](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span></span> <span data-ttu-id="f687d-103">Den här principen beskriver hello upplevelser som hello konsumenter går igenom under återställning av lösenord och hello innehållet i token som hello programmet får genomförts på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="f687d-103">This policy describes hello experiences that hello consumers will go through during password reset and hello contents of tokens that hello application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="f687d-104">Välj hello principer under inställningar **principer för lösenordsåterställning** och på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f687d-104">In hello policies section of settings, select **Password reset policies** and click **+ Add**.</span></span>

![Välj principer för registrering eller inloggning och klicka på knappen hello](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

<span data-ttu-id="f687d-106">Ange en princip **namn** för ditt program tooreference.</span><span class="sxs-lookup"><span data-stu-id="f687d-106">Enter a policy **Name** for your application tooreference.</span></span> <span data-ttu-id="f687d-107">Ange till exempel `SSPR`.</span><span class="sxs-lookup"><span data-stu-id="f687d-107">For example, enter `SSPR`.</span></span>

<span data-ttu-id="f687d-108">Välj **Identitetsprovidrar** och markera **Återställ lösenord med e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="f687d-108">Select **Identity providers** and check **Reset password using email address**.</span></span> <span data-ttu-id="f687d-109">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f687d-109">Click **OK**.</span></span>

![Välj återställning av lösenord med hjälp av e-postadress som en identitetsleverantör och klicka på hello OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

<span data-ttu-id="f687d-111">Välj **Programanspråk**.</span><span class="sxs-lookup"><span data-stu-id="f687d-111">Select **Application claims**.</span></span> <span data-ttu-id="f687d-112">Välj anspråk som du vill ska returneras i hello auktorisering token skickas tillbaka tooyour program efter en lyckad lösenordsåterställning upplevelse.</span><span class="sxs-lookup"><span data-stu-id="f687d-112">Choose claims you want returned in hello authorization tokens sent back tooyour application after a successful password reset experience.</span></span> <span data-ttu-id="f687d-113">Välj till exempel **Användarobjekt-id**.</span><span class="sxs-lookup"><span data-stu-id="f687d-113">For example, select **User's Object ID**.</span></span>

![Välj några programanspråk och klicka på OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

<span data-ttu-id="f687d-115">Klicka på **skapa** tooadd hello princip.</span><span class="sxs-lookup"><span data-stu-id="f687d-115">Click **Create** tooadd hello policy.</span></span> <span data-ttu-id="f687d-116">hello principen anges som **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="f687d-116">hello policy is listed as **B2C_1_SSPR**.</span></span> <span data-ttu-id="f687d-117">Hej **B2C_1_** prefix är tillagda toohello namn.</span><span class="sxs-lookup"><span data-stu-id="f687d-117">hello **B2C_1_** prefix is appended toohello name.</span></span>

<span data-ttu-id="f687d-118">Öppna hello princip genom att välja **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="f687d-118">Open hello policy by selecting **B2C_1_SSPR**.</span></span> <span data-ttu-id="f687d-119">Kontrollera hello inställningarna i hello tabell och klicka sedan på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="f687d-119">Verify hello settings specified in hello table then click **Run now**.</span></span>

![Välj princip och kör den](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| <span data-ttu-id="f687d-121">Inställning</span><span class="sxs-lookup"><span data-stu-id="f687d-121">Setting</span></span>      | <span data-ttu-id="f687d-122">Värde</span><span class="sxs-lookup"><span data-stu-id="f687d-122">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="f687d-123">**Program**</span><span class="sxs-lookup"><span data-stu-id="f687d-123">**Applications**</span></span> | <span data-ttu-id="f687d-124">Contoso B2C-app</span><span class="sxs-lookup"><span data-stu-id="f687d-124">Contoso B2C app</span></span> |
| <span data-ttu-id="f687d-125">**Välj svarswebbadress**</span><span class="sxs-lookup"><span data-stu-id="f687d-125">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="f687d-126">En ny webbläsarflik öppnas och du kan kontrollera hello lösenordsåterställning användarfunktioner i ditt program.</span><span class="sxs-lookup"><span data-stu-id="f687d-126">A new browser tab opens, and you can verify hello password reset consumer experience in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="f687d-127">Den tar upp tooa minut för att skapa en princip och uppdaterar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="f687d-127">It takes up tooa minute for policy creation and updates tootake effect.</span></span>
>
