<span data-ttu-id="921fe-101">tooenable profil redigering för ditt program måste toocreate en profil redigerar principen.</span><span class="sxs-lookup"><span data-stu-id="921fe-101">tooenable profile editing on your application, you will need toocreate a profile editing policy.</span></span> <span data-ttu-id="921fe-102">Den här principen beskriver hello upplevelser som konsumenter går igenom under profil redigering och hello innehållet i token som programmet hello får på lyckades.</span><span class="sxs-lookup"><span data-stu-id="921fe-102">This policy describes hello experiences that consumers will go through during profile editing and hello contents of tokens that hello application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="921fe-103">Välj hello principer under inställningar **redigera principer profiler** och på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="921fe-103">In hello policies section of settings, select **Profile editing policies** and click **+ Add**.</span></span>

![Välj profil ändra principer och klicka hello Lägg till](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

<span data-ttu-id="921fe-105">Ange en princip **namn** för ditt program tooreference.</span><span class="sxs-lookup"><span data-stu-id="921fe-105">Enter a policy **Name** for your application tooreference.</span></span> <span data-ttu-id="921fe-106">Ange till exempel `SiPe`.</span><span class="sxs-lookup"><span data-stu-id="921fe-106">For example, enter `SiPe`.</span></span>

<span data-ttu-id="921fe-107">Välj **Identitetsprovidrar** och markera **Inloggning från lokalt konto**.</span><span class="sxs-lookup"><span data-stu-id="921fe-107">Select **Identity providers** and check **Local Account Signin**.</span></span> <span data-ttu-id="921fe-108">Du kan också välja leverantörer via sociala nätverk om det redan har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="921fe-108">Optionally, you can also select social identity providers, if already configured.</span></span> <span data-ttu-id="921fe-109">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="921fe-109">Click **OK**.</span></span>

![Välj lokal inloggning för kontot som en identitetsleverantör och klicka på hello OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

<span data-ttu-id="921fe-111">Välj **Profilattribut**.</span><span class="sxs-lookup"><span data-stu-id="921fe-111">Select **Profile attributes**.</span></span> <span data-ttu-id="921fe-112">Välj attribut hello konsumenten kan visa och redigera sin profil.</span><span class="sxs-lookup"><span data-stu-id="921fe-112">Choose attributes hello consumer can view and edit in their profile.</span></span> <span data-ttu-id="921fe-113">Välj till exempel **land/region**, **visningsnamn** och **postnummer**.</span><span class="sxs-lookup"><span data-stu-id="921fe-113">For example, check **Country/Region**, **Display Name**, and **Postal Code**.</span></span> <span data-ttu-id="921fe-114">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="921fe-114">Click **OK**.</span></span>

![Välj vissa attribut och klicka på hello OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

<span data-ttu-id="921fe-116">Välj **Programanspråk**.</span><span class="sxs-lookup"><span data-stu-id="921fe-116">Select **Application claims**.</span></span> <span data-ttu-id="921fe-117">Välj anspråk som du vill ska returneras i hello auktorisering token skickas tillbaka tooyour program efter en lyckad profil redigeringsmiljön.</span><span class="sxs-lookup"><span data-stu-id="921fe-117">Choose claims you want returned in hello authorization tokens sent back tooyour application after a successful profile editing experience.</span></span> <span data-ttu-id="921fe-118">Välj till exempel **Visningsnamn**, **Postnummer**.</span><span class="sxs-lookup"><span data-stu-id="921fe-118">For example, select **Display Name**, **Postal Code**.</span></span>

![Välj några programanspråk och klicka på OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

<span data-ttu-id="921fe-120">Klicka på **skapa** tooadd hello princip.</span><span class="sxs-lookup"><span data-stu-id="921fe-120">Click **Create** tooadd hello policy.</span></span> <span data-ttu-id="921fe-121">hello principen anges som **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="921fe-121">hello policy is listed as **B2C_1_SiPe**.</span></span> <span data-ttu-id="921fe-122">Hej **B2C_1_** prefix är tillagda toohello namn.</span><span class="sxs-lookup"><span data-stu-id="921fe-122">hello **B2C_1_** prefix is appended toohello name.</span></span>

<span data-ttu-id="921fe-123">Öppna hello princip genom att välja **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="921fe-123">Open hello policy by selecting **B2C_1_SiPe**.</span></span> <span data-ttu-id="921fe-124">Kontrollera hello inställningarna i hello tabell och klicka sedan på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="921fe-124">Verify hello settings specified in hello table then click **Run now**.</span></span>

![Välj princip och kör den](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| <span data-ttu-id="921fe-126">Inställning</span><span class="sxs-lookup"><span data-stu-id="921fe-126">Setting</span></span>      | <span data-ttu-id="921fe-127">Värde</span><span class="sxs-lookup"><span data-stu-id="921fe-127">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="921fe-128">**Program**</span><span class="sxs-lookup"><span data-stu-id="921fe-128">**Applications**</span></span> | <span data-ttu-id="921fe-129">Contoso B2C-app</span><span class="sxs-lookup"><span data-stu-id="921fe-129">Contoso B2C app</span></span> |
| <span data-ttu-id="921fe-130">**Välj svarswebbadress**</span><span class="sxs-lookup"><span data-stu-id="921fe-130">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="921fe-131">En ny webbläsarflik öppnas och du kan kontrollera hello redigera användarfunktioner som konfigurerats för profiler.</span><span class="sxs-lookup"><span data-stu-id="921fe-131">A new browser tab opens, and you can verify hello profile editing consumer experience as configured.</span></span>

> [!NOTE]
> <span data-ttu-id="921fe-132">Den tar upp tooa minut för att skapa en princip och uppdaterar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="921fe-132">It takes up tooa minute for policy creation and updates tootake effect.</span></span>
>