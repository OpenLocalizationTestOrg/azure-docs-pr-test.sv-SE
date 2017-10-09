<span data-ttu-id="559bc-101">tooenable logga in på ditt program behöver du toocreate loggar in principen.</span><span class="sxs-lookup"><span data-stu-id="559bc-101">tooenable sign-in on your application, you will need toocreate a sign-in policy.</span></span> <span data-ttu-id="559bc-102">Den här principen beskriver hello upplevelser som konsumenter går igenom under inloggning och hello innehållet i token som hello programmet får på lyckade inloggningar.</span><span class="sxs-lookup"><span data-stu-id="559bc-102">This policy describes hello experiences that consumers will go through during sign-in and hello contents of tokens that hello application will receive on successful sign-ins.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="559bc-103">Välj hello principer under inställningar **principer för registrering eller inloggning** och på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="559bc-103">In hello policies section of settings, select **Sign-up or sign-in policies** and click **+ Add**.</span></span>

![Välj registrerings- eller inloggningsprinciper och klicka på Lägg till](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-policy.png)

<span data-ttu-id="559bc-105">Ange en princip **namn** för ditt program tooreference.</span><span class="sxs-lookup"><span data-stu-id="559bc-105">Enter a policy **Name** for your application tooreference.</span></span> <span data-ttu-id="559bc-106">Ange till exempel `SiUpIn`.</span><span class="sxs-lookup"><span data-stu-id="559bc-106">For example, enter `SiUpIn`.</span></span>

<span data-ttu-id="559bc-107">Välj **Identitetsprovidrar** och markera **Registrering via e-post**.</span><span class="sxs-lookup"><span data-stu-id="559bc-107">Select **Identity providers** and check **Email signup**.</span></span> <span data-ttu-id="559bc-108">Du kan också välja leverantörer via sociala nätverk om det redan har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="559bc-108">Optionally, you can also select social identity providers, if already configured.</span></span> <span data-ttu-id="559bc-109">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="559bc-109">Click **OK**.</span></span>

![Välj e-registreringen som en identitetsleverantör och klicka på hello OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-identity-providers.png)

<span data-ttu-id="559bc-111">Välj **Registreringsattribut**.</span><span class="sxs-lookup"><span data-stu-id="559bc-111">Select **Sign-up attributes**.</span></span> <span data-ttu-id="559bc-112">Välj attribut du vill toocollect från hello konsumenten under registreringen.</span><span class="sxs-lookup"><span data-stu-id="559bc-112">Choose attributes you want toocollect from hello consumer during sign-up.</span></span> <span data-ttu-id="559bc-113">Välj till exempel **land/region**, **visningsnamn** och **postnummer**.</span><span class="sxs-lookup"><span data-stu-id="559bc-113">For example, check **Country/Region**, **Display Name**, and **Postal Code**.</span></span> <span data-ttu-id="559bc-114">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="559bc-114">Click **OK**.</span></span>

![Välj vissa attribut och klicka på hello OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-attributes.png)

<span data-ttu-id="559bc-116">Välj **Programanspråk**.</span><span class="sxs-lookup"><span data-stu-id="559bc-116">Select **Application claims**.</span></span> <span data-ttu-id="559bc-117">Välj anspråk som du vill ska returneras i hello auktorisering token skickas tillbaka tooyour program efter en lyckad registrering eller inloggning upplevelse.</span><span class="sxs-lookup"><span data-stu-id="559bc-117">Choose claims you want returned in hello authorization tokens sent back tooyour application after a successful sign-up or sign-in experience.</span></span> <span data-ttu-id="559bc-118">Välj till exempel **visningsnamn**, **identitetsprovidrar**, **postnummer**, **ny användare** och **användarobjekt-id**.</span><span class="sxs-lookup"><span data-stu-id="559bc-118">For example, select **Display Name**, **Identity Provider**, **Postal Code**, **User is new** and **User's Object ID**.</span></span>

![Välj några programanspråk och klicka på OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-application-claims.png)

<span data-ttu-id="559bc-120">Klicka på **skapa** tooadd hello princip.</span><span class="sxs-lookup"><span data-stu-id="559bc-120">Click **Create** tooadd hello policy.</span></span> <span data-ttu-id="559bc-121">hello principen anges som **B2C_1_SiUpIn**.</span><span class="sxs-lookup"><span data-stu-id="559bc-121">hello policy is listed as **B2C_1_SiUpIn**.</span></span> <span data-ttu-id="559bc-122">Hej **B2C_1_** prefix är tillagda toohello namn.</span><span class="sxs-lookup"><span data-stu-id="559bc-122">hello **B2C_1_** prefix is appended toohello name.</span></span>

<span data-ttu-id="559bc-123">Öppna hello princip genom att välja **B2C_1_SiUpIn**.</span><span class="sxs-lookup"><span data-stu-id="559bc-123">Open hello policy by selecting **B2C_1_SiUpIn**.</span></span> <span data-ttu-id="559bc-124">Kontrollera hello inställningarna i hello tabell och klicka sedan på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="559bc-124">Verify hello settings specified in hello table then click **Run now**.</span></span>

![Välj princip och kör den](media/active-directory-b2c-create-sign-in-sign-up-policy/run-b2c-signup-signin-policy.png)

| <span data-ttu-id="559bc-126">Inställning</span><span class="sxs-lookup"><span data-stu-id="559bc-126">Setting</span></span>      | <span data-ttu-id="559bc-127">Värde</span><span class="sxs-lookup"><span data-stu-id="559bc-127">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="559bc-128">**Program**</span><span class="sxs-lookup"><span data-stu-id="559bc-128">**Applications**</span></span> | <span data-ttu-id="559bc-129">Contoso B2C-app</span><span class="sxs-lookup"><span data-stu-id="559bc-129">Contoso B2C app</span></span> |
| <span data-ttu-id="559bc-130">**Välj svarswebbadress**</span><span class="sxs-lookup"><span data-stu-id="559bc-130">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="559bc-131">En ny webbläsarflik öppnas och du kan kontrollera hello registrering eller inloggning användarfunktioner som konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="559bc-131">A new browser tab opens, and you can verify hello sign-up or sign-in consumer experience as configured.</span></span>

> [!NOTE]
> <span data-ttu-id="559bc-132">Den tar upp tooa minut för att skapa en princip och uppdaterar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="559bc-132">It takes up tooa minute for policy creation and updates tootake effect.</span></span>
>