---
title: Povolení životního cyklu rizik Finserve s Azure a R
description: ''
author: sseely
ms.author: sseely
ms.service: industry
ms.topic: overview
ms.date: 11/19/2019
ms.openlocfilehash: 03fea3996b62782c2b65e6d2edf841b5adaebcd2
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052497"
---
# <a name="enabling-the-financial-services-risk-lifecycle-with-azure-and-r"></a><span data-ttu-id="efaf9-102">Povolení životního cyklu rizik finančních služeb s Azure a R</span><span class="sxs-lookup"><span data-stu-id="efaf9-102">Enabling the financial services risk lifecycle with Azure and R</span></span>


<span data-ttu-id="efaf9-103">Výpočty rizik jsou v průběhu životního cyklu operací klíčových finančních služeb pivotované v několika fázích.</span><span class="sxs-lookup"><span data-stu-id="efaf9-103">Risk calculations are pivotal at several stages in the lifecycle of key financial services operations.</span></span> <span data-ttu-id="efaf9-104">Zjednodušená forma životního cyklu správy produktů v pojišťovnictví může například vypadat přibližně jako diagram níže.</span><span class="sxs-lookup"><span data-stu-id="efaf9-104">For example, a simplified form of the insurance product management lifecycle might look something like the diagram below.</span></span> <span data-ttu-id="efaf9-105">Aspekty výpočtu rizik se zobrazují v modrém textu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-105">The risk calculation aspects are shown in blue text.</span></span>

![](./assets/fsi-risk-modeling/image1.png)

<span data-ttu-id="efaf9-106">Scénář velkých trhů v podniku může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="efaf9-106">A scenario in a capital markets firm might look like this:</span></span>

![](./assets/fsi-risk-modeling/image2.png)

<span data-ttu-id="efaf9-107">Prostřednictvím těchto procesů existují běžné požadavky na modelování rizik, včetně:</span><span class="sxs-lookup"><span data-stu-id="efaf9-107">Through these processes there are common needs around risk modelling including:</span></span>

1. <span data-ttu-id="efaf9-108">Nutnost experimentů s ad hoc rizik pomocí rizikových analytiků; Pojistní matematici v pojišťovací firmě nebo v quants v podniku velkých trhů.</span><span class="sxs-lookup"><span data-stu-id="efaf9-108">The need for ad-hoc risk-related experimentation by risk analysts; actuaries in an insurance firm or quants in a capital markets firm.</span></span>
    <span data-ttu-id="efaf9-109">Tito analytiké obvykle pracují s oblíbenými nástroji pro kódování a modelování v jejich doméně: R a Python.</span><span class="sxs-lookup"><span data-stu-id="efaf9-109">These analysts typically work with code and modelling tools popular in their domain: R and Python.</span></span> <span data-ttu-id="efaf9-110">Mnoho studijních kurzů zahrnuje školení v R nebo Pythonu ve matematických financích a v MBAch kurzech.</span><span class="sxs-lookup"><span data-stu-id="efaf9-110">Many university curriculums include training in R or Python in mathematical finance and in MBA courses.</span></span>
    <span data-ttu-id="efaf9-111">Oba jazyky nabízejí široké spektrum Open Source knihoven, které podporují populární výpočty rizik.</span><span class="sxs-lookup"><span data-stu-id="efaf9-111">Both languages offer a wide range of open source libraries which support popular risk calculations.</span></span> <span data-ttu-id="efaf9-112">Společně s vhodným nástrojem analytik často vyžaduje přístup k:</span><span class="sxs-lookup"><span data-stu-id="efaf9-112">Along with appropriate tooling, analysts often require access to:</span></span>

    <span data-ttu-id="efaf9-113">a.</span><span class="sxs-lookup"><span data-stu-id="efaf9-113">a.</span></span>  <span data-ttu-id="efaf9-114">Přesná data o cenách trhu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-114">Accurate market pricing data.</span></span>

    <span data-ttu-id="efaf9-115">b.</span><span class="sxs-lookup"><span data-stu-id="efaf9-115">b.</span></span>  <span data-ttu-id="efaf9-116">Existující zásady a data deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="efaf9-116">Existing policy and claims data.</span></span>

    <span data-ttu-id="efaf9-117">c.</span><span class="sxs-lookup"><span data-stu-id="efaf9-117">c.</span></span>  <span data-ttu-id="efaf9-118">Existující data pozice na trhu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-118">Existing market position data.</span></span>

    <span data-ttu-id="efaf9-119">d.</span><span class="sxs-lookup"><span data-stu-id="efaf9-119">d.</span></span>  <span data-ttu-id="efaf9-120">Jiná externí data.</span><span class="sxs-lookup"><span data-stu-id="efaf9-120">Other external data.</span></span> <span data-ttu-id="efaf9-121">Zdroje obsahují strukturovaná data, jako jsou tabulky úmrtnosti a konkurenční údaje o cenách.</span><span class="sxs-lookup"><span data-stu-id="efaf9-121">Sources include structured data such as mortality tables and competitive pricing data.</span></span> <span data-ttu-id="efaf9-122">Můžou se používat i méně tradiční zdroje, jako je třeba počasí, zprávy a jiné.</span><span class="sxs-lookup"><span data-stu-id="efaf9-122">Less traditional sources such as weather, news and others may also be used.</span></span>

    <span data-ttu-id="efaf9-123">e.</span><span class="sxs-lookup"><span data-stu-id="efaf9-123">e.</span></span>  <span data-ttu-id="efaf9-124">Výpočetní kapacita umožňující rychlé interaktivní vyšetřování dat.</span><span class="sxs-lookup"><span data-stu-id="efaf9-124">Computational capacity to enable quick interactive data investigations.</span></span>

2. <span data-ttu-id="efaf9-125">Můžou také využít algoritmy služby Machine Learning pro ad hoc pro ceny nebo stanovit strategii trhu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-125">They may also make use of ad-hoc machine learning algorithms for  pricing or determining market strategy.</span></span>

3. <span data-ttu-id="efaf9-126">Nutnost vizualizovat a prezentovat data pro použití při plánování produktů, obchodní strategii a podobných diskusích.</span><span class="sxs-lookup"><span data-stu-id="efaf9-126">The need to visualize and present data for use in product planning,  trading strategy, and similar discussions.</span></span>

4. <span data-ttu-id="efaf9-127">Rychlé provádění definovaných modelů konfigurovaných analytiky, pro ceny, oceňování a rizika trhu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-127">The rapid execution of defined models, configured by the analysts, for pricing, valuations, and market risk.</span></span> <span data-ttu-id="efaf9-128">Oceňování využívají kombinaci vyhrazeného modelování rizik, nástrojů pro tržní rizika a vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="efaf9-128">The valuations use a combination of dedicated risk modelling, market risk tools, and custom code.</span></span> <span data-ttu-id="efaf9-129">Analýza se provádí v dávce s různou noc, týdně, měsíčně, čtvrtletně a ročními výpočty, které generují špičky v úlohách.</span><span class="sxs-lookup"><span data-stu-id="efaf9-129">The analysis is executed in a batch with varying nightly, weekly, monthly, quarterly, and annual calculations generating spikes in workloads.</span></span>

5. <span data-ttu-id="efaf9-130">Integrace dat s dalšími rizikovými mírami v rozlehlých sítích pro sestavy konsolidovaného rizika.</span><span class="sxs-lookup"><span data-stu-id="efaf9-130">The integration of data with other enterprise wide risk measures for consolidated risk reporting.</span></span> <span data-ttu-id="efaf9-131">V rámci větších organizací se odhad rizika na nižší úrovni může přenést do modelování podnikových rizik a nástroje pro vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="efaf9-131">In larger organizations lower level risk estimates may be transferred to an enterprise risk modelling and reporting tool.</span></span>

6. <span data-ttu-id="efaf9-132">Výsledky se musí nahlásit v definovaném formátu v požadovaném intervalu, aby se splnily požadavky na investor a zákonné předpisy.</span><span class="sxs-lookup"><span data-stu-id="efaf9-132">Results must be reported in a defined format at the required  interval to meet investor and regulatory requirements.</span></span>

<span data-ttu-id="efaf9-133">Společnost Microsoft podporuje výše uvedené otázky prostřednictvím kombinace služeb Azure a partnerských nabídek v [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely).</span><span class="sxs-lookup"><span data-stu-id="efaf9-133">Microsoft supports the above concerns through a combination of Azure services and partner offerings in the [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely).</span></span> <span data-ttu-id="efaf9-134">V tomto článku se seznámíme s praktickými příklady toho, jak provádět ad hoc experimenty pomocí jazyka R. Začneme tím, že vysvětlíme, jak experiment spustit na jednom počítači, a pak zobrazit, jak spustit stejný experiment na [Azure Batch](https://docs.microsoft.com/azure/batch/?WT.mc_id=fsiriskmodelr-docs-scseely)a zavřít tím, že využijete výhod externích služeb v našem modelování.</span><span class="sxs-lookup"><span data-stu-id="efaf9-134">In this article, we show practical examples of how to perform ad-hoc experimentation using R. We begin by explaining how to run the experiment on a single machine, then show how to run the same experiment on [Azure Batch](https://docs.microsoft.com/azure/batch/?WT.mc_id=fsiriskmodelr-docs-scseely), and close by showing how to take advantage of external services in our modelling.</span></span> <span data-ttu-id="efaf9-135">Možnosti a požadavky na spouštění definovaných modelů v Azure jsou popsány v těchto článcích, které se zaměřují na [bankovnictví](https://docs.microsoft.com/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely) a [pojišťovnictví](https://docs.microsoft.com/azure/industry/financial/actuarial-risk-analysis-and-financial-modeling-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely).</span><span class="sxs-lookup"><span data-stu-id="efaf9-135">Options and considerations for the execution of defined models on Azure are described in these articles focused on [banking](https://docs.microsoft.com/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely) and [insurance](https://docs.microsoft.com/azure/industry/financial/actuarial-risk-analysis-and-financial-modeling-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely).</span></span>

## <a name="analyst-modelling-in-r"></a><span data-ttu-id="efaf9-136">Modelování analytiků v jazyce R</span><span class="sxs-lookup"><span data-stu-id="efaf9-136">Analyst Modelling in R</span></span>

<span data-ttu-id="efaf9-137">Začněte tím, že si vyhledáme, jak může být v analytikovi ve zjednodušeném scénáři s reprezentativními kapitálovými trhy použit R.</span><span class="sxs-lookup"><span data-stu-id="efaf9-137">Let's start by looking at how R may be used by an analyst in a simplified, representative capital markets scenario.</span></span> <span data-ttu-id="efaf9-138">Můžete to vytvořit buď odkazem na existující knihovnu R pro výpočet, nebo zápisem kódu od začátku.</span><span class="sxs-lookup"><span data-stu-id="efaf9-138">You can build this either by referencing an existing R library for the calculation or by writing code from scratch.</span></span> <span data-ttu-id="efaf9-139">V našem příkladu je také nutné načíst externí data o cenách.</span><span class="sxs-lookup"><span data-stu-id="efaf9-139">In our example, we also must fetch external pricing data.</span></span> <span data-ttu-id="efaf9-140">Aby byl příklad jednoduchý, ale ilustrativní, vypočítáme potenciální budoucí expozici (PFE) akcií, který je dál uzavřený.</span><span class="sxs-lookup"><span data-stu-id="efaf9-140">To keep the example simple but illustrative, we calculate the potential future exposure (PFE) of an equity stock forward contract.</span></span>
<span data-ttu-id="efaf9-141">V tomto příkladu se vyhnete komplexním kvantitativním technikům modelování pro nástroje, jako jsou složité deriváty, a zaměřuje se na jediný rizikový faktor, který se soustředí na životní cyklus rizika.</span><span class="sxs-lookup"><span data-stu-id="efaf9-141">This example avoids complex quantitative modelling techniques for instruments like complex derivatives and focuses on a single risk factor to concentrate on the risk life cycle.</span></span> <span data-ttu-id="efaf9-142">Náš příklad provede následující:</span><span class="sxs-lookup"><span data-stu-id="efaf9-142">Our example does the following:</span></span>

1. <span data-ttu-id="efaf9-143">Vyberte instrumentaci zájmu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-143">Select an instrument of interest.</span></span>

2. <span data-ttu-id="efaf9-144">Zdroje historických cen za nástroj.</span><span class="sxs-lookup"><span data-stu-id="efaf9-144">Source historic prices for the instrument.</span></span>

3. <span data-ttu-id="efaf9-145">Modelujte cenu za jmění pomocí jednoduchého výpočtu Monte Carlo (MC), který používá geometrické Brownianové pohybu (GBM):</span><span class="sxs-lookup"><span data-stu-id="efaf9-145">Model equity price by simple Monte Carlo (MC) calculation, which  uses Geometric Brownian Motion (GBM):</span></span>

    <span data-ttu-id="efaf9-146">a.</span><span class="sxs-lookup"><span data-stu-id="efaf9-146">a.</span></span>  <span data-ttu-id="efaf9-147">Odhadované očekávané vrácení μ (mu) a σ nestálosti (théta).</span><span class="sxs-lookup"><span data-stu-id="efaf9-147">Estimate expected return μ (mu) and volatility σ (theta).</span></span>

    <span data-ttu-id="efaf9-148">b.</span><span class="sxs-lookup"><span data-stu-id="efaf9-148">b.</span></span>  <span data-ttu-id="efaf9-149">Proveďte kalibraci modelu u historických dat.</span><span class="sxs-lookup"><span data-stu-id="efaf9-149">Calibrate the model to historic data.</span></span>

4. <span data-ttu-id="efaf9-150">Vizualizujte různé cesty pro sdělování výsledků.</span><span class="sxs-lookup"><span data-stu-id="efaf9-150">Visualize the various paths to communicate the results.</span></span>

5. <span data-ttu-id="efaf9-151">Maximální hodnota grafu (0, skladová hodnota) pro předvedení významu PFE, rozdíl na riziko hodnoty rizika (VaR)</span><span class="sxs-lookup"><span data-stu-id="efaf9-151">Plot max(0,Stock Value) to demonstrate the meaning of PFE, the  difference to Value at Risk (VaR)</span></span>

    <span data-ttu-id="efaf9-152">a.</span><span class="sxs-lookup"><span data-stu-id="efaf9-152">a.</span></span>  <span data-ttu-id="efaf9-153">Postup při objasnění: PFE = Share Price (T)--Price Contract Price K</span><span class="sxs-lookup"><span data-stu-id="efaf9-153">To clarify: PFE = Share Price (T) -- Forward Contract Price K</span></span>

6. <span data-ttu-id="efaf9-154">Pořiďte 0,95 Quantile, aby se v každém časovém intervalu a konci simulace získala hodnota PFE.</span><span class="sxs-lookup"><span data-stu-id="efaf9-154">Take the 0.95 Quantile to get the PFE value at each time step / end  of simulation period</span></span>

<span data-ttu-id="efaf9-155">Vyhodnotit potenciální budoucí riziko pro jmění na základě zásob protokolu MSFT.</span><span class="sxs-lookup"><span data-stu-id="efaf9-155">We will calculate the potential future exposure for an equity forward based on MSFT stock.</span></span> <span data-ttu-id="efaf9-156">Jak je uvedeno výše, pro modelování zásob akcií se vyžadují historické ceny za sklady MSFT, abychom mohli model kalibrovat na historická data.</span><span class="sxs-lookup"><span data-stu-id="efaf9-156">As mentioned above, to model the stock prices, historic prices for the MSFT stock are required so we can calibrate the model to historical data.</span></span> <span data-ttu-id="efaf9-157">Existuje mnoho způsobů, jak získat historické ceny za cenných papírů.</span><span class="sxs-lookup"><span data-stu-id="efaf9-157">There are many ways to acquire historical stock prices.</span></span> <span data-ttu-id="efaf9-158">V našem příkladu používáme bezplatnou verzi skladové ceny služby od externího poskytovatele služeb, [Quandl](https://www.quandl.com/).</span><span class="sxs-lookup"><span data-stu-id="efaf9-158">In our example, we use a free version of a stock price service from an external service provider, [Quandl](https://www.quandl.com/).</span></span>


> <span data-ttu-id="efaf9-159">Poznámka: příklad používá [datovou sadu cen wiki](https://www.quandl.com/databases/WIKIP) , kterou je možné použít pro učení konceptů.</span><span class="sxs-lookup"><span data-stu-id="efaf9-159">Note: The example uses the [WIKI Prices dataset](https://www.quandl.com/databases/WIKIP) which can be used for learning concepts.</span></span> <span data-ttu-id="efaf9-160">Pro využívání produkčních akcií založených na USA Quandl doporučuje použít [datovou sadu cen na konci dne US](https://www.quandl.com/data/EOD-End-of-Day-US-Stock-Prices).</span><span class="sxs-lookup"><span data-stu-id="efaf9-160">For production usage of US based equities, Quandl recommends using the [End of Day US Stock Prices dataset](https://www.quandl.com/data/EOD-End-of-Day-US-Stock-Prices).</span></span>

<span data-ttu-id="efaf9-161">Abychom mohli zpracovávat data a definovat riziko spojené s tímto jměním, musíme udělat následující věci:</span><span class="sxs-lookup"><span data-stu-id="efaf9-161">To process the data and define the risk associated with the equity, we need to do the following things:</span></span>

1. <span data-ttu-id="efaf9-162">Načte data historie z vlastního jmění.</span><span class="sxs-lookup"><span data-stu-id="efaf9-162">Retrieve history data from the equity.</span></span>

2. <span data-ttu-id="efaf9-163">Z historických dat určete očekávaný návratový σ μ a nestálosti.</span><span class="sxs-lookup"><span data-stu-id="efaf9-163">Determine the expected return μ and volatility σ from the historic  data.</span></span>

3. <span data-ttu-id="efaf9-164">Vymodelujte základní ceny akcií pomocí některé simulace.</span><span class="sxs-lookup"><span data-stu-id="efaf9-164">Model the underlying stock prices using some simulation.</span></span>

4. <span data-ttu-id="efaf9-165">Spuštění modelu</span><span class="sxs-lookup"><span data-stu-id="efaf9-165">Run the model</span></span>

5. <span data-ttu-id="efaf9-166">V budoucnu určete expozici akcií.</span><span class="sxs-lookup"><span data-stu-id="efaf9-166">Determine the exposure of the equity in the future.</span></span>

<span data-ttu-id="efaf9-167">Začneme načtením akcie ze služby Quandl a vykreslíme historii uzavíracích cen za posledních 180 dní.</span><span class="sxs-lookup"><span data-stu-id="efaf9-167">We start by retrieving the stock from the Quandl service and plotting the closing price history over the last 180 days.</span></span>

````R
# Lubridate package must be installed
if (!require(lubridate)) install.packages('lubridate')
library(lubridate)

# Quandl package must be installed
if (!require(Quandl)) install.packages('Quandl')
library(Quandl)

# Get your API key from quandl.com
quandl_api = "enter your key here"

# Add the key to the Quandl keychain
Quandl.api_key(quandl_api)

quandl_get <-
    function(sym, start_date = "2018-01-01") {
        require(devtools)
        require(Quandl)
        # Retrieve the Open, High, Low, Close and Volume Column for a given Symbol
        # Column Indices can be deduced from this sample call
        # data <- Quandl(c("WIKI/MSFT"), rows = 1)

        tryCatch(Quandl(c(
        paste0("WIKI/", sym, ".8"),    # Column 8 : Open
        paste0("WIKI/", sym, ".9"),    # Column 9 : High
        paste0("WIKI/", sym, ".10"),   # Column 10: Low
        paste0("WIKI/", sym, ".11"),   # Column 11: Close
        paste0("WIKI/", sym, ".12")),  # Column 12: Volume
        start_date = start_date,
        type = "raw"
        ))
    }

# Define the Equity Forward
instrument.name <- "MSFT"
instrument.premium <- 100.1
instrument.pfeQuantile <- 0.95

# Get the stock price for the last 180 days
instrument.startDate <- today() - days(180)

# Get the quotes for an equity and transform them to a data frame
df_instrument.timeSeries <- quandl_get(instrument.name,start_date = instrument.startDate)

#Rename the columns
colnames(df_instrument.timeSeries) <- c()
colnames(df_instrument.timeSeries) <- c("Date","Open","High","Low","Close","Volume")

# Plot the closing price history to get a better feeling for the data
plot(df_instrument.timeSeries$Date, df_instrument.timeSeries$Close)
````

<span data-ttu-id="efaf9-168">S daty je k dispozici GBM model.</span><span class="sxs-lookup"><span data-stu-id="efaf9-168">With the data in hand, we calibrate the GBM model.</span></span>

````R
# Code inspired by the book Computational Finance, An Introductory Course with R by 
#    A. Arratia.

# Calculate the daily return in order to estimate sigma and mu in the Wiener Process
df_instrument.dailyReturns <- c(diff(log(df_instrument.timeSeries$Close)), NA)

# Estimate the mean of std deviation of the log returns to estimate the parameters of the Wiener Process

estimateGBM_Parameters <- function(logReturns,dt = 1/252) {

    # Volatility
    sigma_hat = sqrt(var(logReturns)) / sqrt(dt)

    # Drift
    mu_hat = mean(logReturns) / dt + sigma_hat**2 / 2.0

    # Return the parameters
    parameter.list <- list("mu" = mu_hat,"sigma" = sigma_hat)

    return(parameter.list)
}

# Calibrate the model to historic data
GBM_Parameters <- estimateGBM_Parameters(df_instrument.dailyReturns[1:length(df_instrument.dailyReturns) - 1])
````

<span data-ttu-id="efaf9-169">V dalším kroku provedeme modelování základních cen akcií.</span><span class="sxs-lookup"><span data-stu-id="efaf9-169">Next, we model the underlying stock prices.</span></span> <span data-ttu-id="efaf9-170">Můžeme buď implementovat diskrétní proces GBM od začátku, nebo využít jeden z mnoha balíčků R, které tuto funkci poskytují.</span><span class="sxs-lookup"><span data-stu-id="efaf9-170">We can either implement the discrete GBM process from scratch or utilize one of many R packages which provide this functionality.</span></span> <span data-ttu-id="efaf9-171">Používáme balíček R [ *kvalifikacemi* (simulace a odvozování pro rozdílové rovnice stochastického)](https://cran.r-project.org/web/packages/sde/index.html) , který poskytuje metodu řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="efaf9-171">We use the R package [*sde* (Simulation and Inference for Stochastic Differential Equations)](https://cran.r-project.org/web/packages/sde/index.html) which provides a method of solving this problem.</span></span> <span data-ttu-id="efaf9-172">Metoda GBM vyžaduje sadu parametrů, které jsou buď kalibrovány na historické údaje nebo zadány jako parametry simulace.</span><span class="sxs-lookup"><span data-stu-id="efaf9-172">The GBM method requires a set of parameters which are either calibrated to historic data or given as simulation parameters.</span></span> <span data-ttu-id="efaf9-173">K dispozici jsou historické údaje, které na začátku simulace (P0) poskytují μ, σ a ceny akcií.</span><span class="sxs-lookup"><span data-stu-id="efaf9-173">We use the historic data, providing μ, σ and the stock prices at the beginning of the simulation (P0).</span></span>

````R
if (!require(sde)) install.packages('sde')
library(sde)

sigma <-  GBM_Parameters$sigma
mu <- GBM_Parameters$mu
P0 <- tail(df_instrument.timeSeries$Close, 1)

# Calculate the PFE looking one month into the future
T <- 1 / 12

# Consider nt MC paths
nt=50

# Divide the time interval T into n discrete time steps
n = 2 ^ 8 

dt <- T / n
t <- seq(0,T,by=dt)
````

<span data-ttu-id="efaf9-174">Nyní jsme připraveni zahájit simulaci Monte Carlo, která bude modelovat potenciální riziko pro určitý počet cest simulace.</span><span class="sxs-lookup"><span data-stu-id="efaf9-174">We are now ready to start a Monte Carlo simulation to model the potential exposure for some number of simulation paths.</span></span> <span data-ttu-id="efaf9-175">Tuto simulaci omezíme na 50 Monte cesty Carlo a kroky 256 času.</span><span class="sxs-lookup"><span data-stu-id="efaf9-175">We will limit the simulation to 50 Monte Carlo paths and 256 time steps.</span></span> <span data-ttu-id="efaf9-176">V rámci přípravy na škálování pro účely horizontálního navýšení kapacity simulace a využití paralelismu v R se smyčka simulace Monte Carlo používá příkaz foreach.</span><span class="sxs-lookup"><span data-stu-id="efaf9-176">In preparation for scaling out the simulation and taking advantage of parallelization in R, the Monte Carlo simulation loop uses a foreach statement.</span></span>

````R
# Track the start time of the simulation
start_s <- Sys.time()

# Instead of a simple for loop to execute a simulation per MC path, call the
# simulation with the foreach package
# in order to demonstrate the similarity to the AzureBatch way to call the method.

library(foreach)
# Execute the MC simulation for the wiener process utilizing the GBM method from the sde package
exposure_mc <- foreach (i=1:nt, .combine = rbind ) %do%  GBM(x = P0, r = mu, sigma = sigma, T = T, N = n)
rownames(exposure_mc) <- c()

# Track the end time of the simulation
end_s <- Sys.time()

# Duration of the simulation

difftime(end_s, start_s) 
````

<span data-ttu-id="efaf9-177">Teď jsme simulovali cenu za základní zásobu protokolu MSFT.</span><span class="sxs-lookup"><span data-stu-id="efaf9-177">We have now simulated the price of the underlying MSFT stock.</span></span> <span data-ttu-id="efaf9-178">Pokud chcete vypočítat expozici jmění, odečte prémii a omezíme vystavení pouze na kladné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="efaf9-178">To calculate the exposure of the equity forward, we subtract the premium and limit the exposure to only positive values.</span></span>

````R
# Calculate the total Exposure as V_i(t) - K, put it to zero for negative exposures
pfe_mc <- pmax(exposure_mc - instrument.premium ,0)

ymax <- max(pfe_mc)
ymin <- min(pfe_mc)
plot(t, pfe_mc[1,], t = 'l', ylim = c(ymin, ymax), col = 1, ylab="Credit Exposure in USD", xlab="time t in Years")
for (i in 2:nt) {
    lines(t, pfe_mc[i,], t = 'l', ylim = c(ymin, ymax), col = i)
}
````

<span data-ttu-id="efaf9-179">Následující dva obrázky znázorňují výsledek simulace.</span><span class="sxs-lookup"><span data-stu-id="efaf9-179">The next two pictures show the result of the simulation.</span></span> <span data-ttu-id="efaf9-180">První obrázek zobrazuje simulaci Monte Carloe základní skladové ceny za 50 cest.</span><span class="sxs-lookup"><span data-stu-id="efaf9-180">The first picture shows the Monte Carlo simulation of the underlying stock price for 50 paths.</span></span> <span data-ttu-id="efaf9-181">Druhý obrázek znázorňuje základní úvěrové riziko pro daný jmění, po odečtení úrovně Premium jmění před a omezením ozáření na kladné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="efaf9-181">The second picture illustrates the underlying Credit Exposure for the equity forward after subtracting the premium of the equity forward and limiting the exposure to positive values.</span></span>


|       |    |
|---    |--- |
| <img src="./assets/fsi-risk-modeling/image3.png" width="400px" alt="Figure 1 - 50 Monte Carlo Paths"/> | <img src="./assets/fsi-risk-modeling/image4.png" width="400px" alt="Figure 2 - Credit Exposure for Equity Forward"/> |
| <span data-ttu-id="efaf9-182">Obrázek 1-50 Monte cesty Carlo</span><span class="sxs-lookup"><span data-stu-id="efaf9-182">Figure 1 - 50 Monte Carlo Paths</span></span> | <span data-ttu-id="efaf9-183">Obrázek 2 – vystavení kreditu pro přeposlání jmění</span><span class="sxs-lookup"><span data-stu-id="efaf9-183">Figure 2 - Credit Exposure for Equity Forward</span></span> |


<span data-ttu-id="efaf9-184">V posledním kroku se v rámci následujícího kódu vypočítá Quantile PFE o 1 měsíc 0,95.</span><span class="sxs-lookup"><span data-stu-id="efaf9-184">In the last step, the 1-Month 0.95 Quantile PFE is calculated by the following code.</span></span>

````R
# Calculate the PFE at each time step
df_pfe <- cbind(t,apply(pfe_mc,2,quantile,probs = instrument.pfeQuantile ))

resulting in the final PFE plot
plot(df_pfe, t = 'l', ylab = "Potential Future Exposure in USD", xlab = "time t in Years")
````

<img src="./assets/fsi-risk-modeling/image5.png" width="500px" alt="Potential Future Exposure for MSFT Equity Forward" /> 

<span data-ttu-id="efaf9-185">Obrázek 3 – možná budoucí expozice pro MSFT jmění dopředu</span><span class="sxs-lookup"><span data-stu-id="efaf9-185">Figure 3 Potential Future Exposure for MSFT Equity Forward</span></span>

## <a name="using-azure-batch-with-r"></a><span data-ttu-id="efaf9-186">Použití Azure Batch s R</span><span class="sxs-lookup"><span data-stu-id="efaf9-186">Using Azure Batch with R</span></span> 

<span data-ttu-id="efaf9-187">Výše popsané řešení jazyka R lze připojit k Azure Batch a využít Cloud k výpočtům rizik.</span><span class="sxs-lookup"><span data-stu-id="efaf9-187">The R solution described above can be connected to Azure Batch and leverage the cloud for risk calculations.</span></span> <span data-ttu-id="efaf9-188">To má trochu dodatečné úsilí pro paralelní výpočet, jako je náš.</span><span class="sxs-lookup"><span data-stu-id="efaf9-188">This takes little extra effort for a parallel calculation such as ours.</span></span> <span data-ttu-id="efaf9-189">V tomto kurzu [spustíte paralelní simulaci r s Azure Batch, kde](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)najdete podrobné informace o připojení R k Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="efaf9-189">The tutorial, [Run a parallel R simulation with Azure Batch](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely), provides detailed information on connecting R to Azure Batch.</span></span> <span data-ttu-id="efaf9-190">Dál zobrazujeme kód a souhrn procesu pro připojení k Azure Batch a využití tohoto rozšíření pro Cloud ve zjednodušeném PFE výpočtu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-190">Below we show the code and summary of the process to connect to Azure Batch and how to take advantage of the extension to the cloud in a simplified PFE calculation.</span></span>

<span data-ttu-id="efaf9-191">Tento příklad prochází stejný model popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="efaf9-191">This example tackles the same model described earlier.</span></span> <span data-ttu-id="efaf9-192">Jak jsme viděli dříve, tento výpočet může běžet v našem osobním počítači.</span><span class="sxs-lookup"><span data-stu-id="efaf9-192">As we have seen before, this calculation can run on our personal computer.</span></span> <span data-ttu-id="efaf9-193">Zvýšení počtu MONTECH cest Carlo nebo použití kratších časových kroků bude mít za následek mnohem delší dobu spuštění.</span><span class="sxs-lookup"><span data-stu-id="efaf9-193">Increases to the number of Monte Carlo paths or use of smaller time steps will result in much longer execution times.</span></span> <span data-ttu-id="efaf9-194">Skoro všechen kód R zůstane beze změny.</span><span class="sxs-lookup"><span data-stu-id="efaf9-194">Almost all of the R code will remain unchanged.</span></span> <span data-ttu-id="efaf9-195">Rozdíly v této části vyzvýrazníme.</span><span class="sxs-lookup"><span data-stu-id="efaf9-195">We will highlight the differences in this section.</span></span>

<span data-ttu-id="efaf9-196">Každá cesta simulace Monte Carlo běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="efaf9-196">Each path of the Monte Carlo simulation runs in Azure.</span></span> <span data-ttu-id="efaf9-197">To můžeme udělat, protože každá cesta je nezávislá na ostatních, takže nám pomůžeme vypočítat "zpracovatelné Parallel".</span><span class="sxs-lookup"><span data-stu-id="efaf9-197">We can do this because each path is independent of the others, giving us an "embarrassingly parallel" calculation.</span></span>

<span data-ttu-id="efaf9-198">Pokud chcete použít Azure Batch, definujeme základní cluster a bude na něj odkazovat v kódu předtím, než bude možné cluster použít ve výpočtech.</span><span class="sxs-lookup"><span data-stu-id="efaf9-198">To use Azure Batch, we define the underlying cluster and reference it in the code before the cluster can be used in the calculations.</span></span> <span data-ttu-id="efaf9-199">Ke spuštění výpočtů používáme následující definici typu cluster. JSON:</span><span class="sxs-lookup"><span data-stu-id="efaf9-199">To run the calculations, we use the following cluster.json definition:</span></span>

````JavaScript
{
  "name": "myMCPool",
  "vmSize": "Standard_D2_v2",
  "maxTasksPerNode": 4,
  "poolSize": {
    "dedicatedNodes": {
      "min": 1,
      "max": 1
    },
    "lowPriorityNodes": {
      "min": 3,
      "max": 3
    },
    "autoscaleFormula": "QUEUE"
  },
  "containerImage": "rocker/tidyverse:latest",
  "rPackages": {
    "cran": [],
    "github": [],
    "bioconductor": []
  },
  "commandLine": [],
  "subnetId": ""
}
````

<span data-ttu-id="efaf9-200">V této definici clusteru používá následující kód R k tomu cluster:</span><span class="sxs-lookup"><span data-stu-id="efaf9-200">With this cluster definition, the following R code makes use of the cluster:</span></span>
````R
# Define the cloud burst environment
library(doAzureParallel)

# set your credentials
setCredentials("credentials.json")

# Create your cluster if not exist
cluster <- makeCluster("cluster.json")

# register your parallel backend
registerDoAzureParallel(cluster)

# check that your workers are up
getDoParWorkers()
````

<span data-ttu-id="efaf9-201">Nakonec aktualizujeme příkaz foreach z výše na použití balíčku balíček doazureparallel.</span><span class="sxs-lookup"><span data-stu-id="efaf9-201">Finally, we update the foreach statement from earlier to use the doAzureParallel package.</span></span> <span data-ttu-id="efaf9-202">Jedná se o menší změnu, přidání odkazu na balíček kvalifikacemi a změna% do% na% dopar%:</span><span class="sxs-lookup"><span data-stu-id="efaf9-202">It's a minor change, adding a reference to the sde package and changing the %do% to %dopar%:</span></span>

````R
# Execute the MC simulation for the wiener process utilizing the GBM method from the sde package and extend the computation to the cloud
exposure_mc <- foreach(i = 1:nt, .combine = rbind, .packages = 'sde') %dopar% GBM(x = P0, r = mu, sigma = sigma, T = T, N = n)
rownames(exposure_mc) <- c()
````

<span data-ttu-id="efaf9-203">Každá simulace Monte Carlo je odeslána jako úloha Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="efaf9-203">Each Monte Carlo simulation is submitted as a task to Azure Batch.</span></span> <span data-ttu-id="efaf9-204">Úloha se spustí v cloudu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-204">The task executes in the cloud.</span></span> <span data-ttu-id="efaf9-205">Výsledky se sloučí před odesláním zpět do aplikace analytiky.</span><span class="sxs-lookup"><span data-stu-id="efaf9-205">Results are merged before being sent back to the analyst workbench.</span></span> <span data-ttu-id="efaf9-206">Těžké zvedací a výpočetní operace se provádějí v cloudu, aby plně využily výhody škálování a základní infrastruktury vyžadované požadovanými výpočty.</span><span class="sxs-lookup"><span data-stu-id="efaf9-206">The heavy lifting and computations execute in the cloud to take full advantage of scaling and the underlying infrastructure required by the requested calculations.</span></span>

<span data-ttu-id="efaf9-207">Po dokončení výpočtů můžete další prostředky snadno vypnout tím, že vyvoláte tuto jednu instrukci:</span><span class="sxs-lookup"><span data-stu-id="efaf9-207">After the calculations have finished, the additional resources can easily be shut-down by invoking the following a single instruction:</span></span>

````R
# Stop the Cloud cluster
stopCluster(cluster)
````


## <a name="use-a-saas-offering"></a><span data-ttu-id="efaf9-208">Použití nabídky SaaS</span><span class="sxs-lookup"><span data-stu-id="efaf9-208">Use a SaaS offering</span></span>

<span data-ttu-id="efaf9-209">První dva příklady ukazují, jak využít místní a cloudovou infrastrukturu pro vývoj vhodného modelu hodnocení.</span><span class="sxs-lookup"><span data-stu-id="efaf9-209">The first two examples show how to utilize local and cloud infrastructure for developing an adequate valuation model.</span></span> <span data-ttu-id="efaf9-210">Tento paradigma začal Shift.</span><span class="sxs-lookup"><span data-stu-id="efaf9-210">This paradigm has begun to shift.</span></span> <span data-ttu-id="efaf9-211">Stejným způsobem, jakým se místní infrastruktura transformuje na cloudové služby IaaS a PaaS, modelování relevantních rizikových údajů transformuje na proces orientovaný na služby.</span><span class="sxs-lookup"><span data-stu-id="efaf9-211">In the same way that on-premises infrastructure has transformed into cloud-based IaaS and PaaS services, the modelling of relevant risk figures is transforming into a service-oriented process.</span></span>
<span data-ttu-id="efaf9-212">Dnešní analytici čelí dvěma významným problémům:</span><span class="sxs-lookup"><span data-stu-id="efaf9-212">Today's analysts face two major challenges:</span></span>

1. <span data-ttu-id="efaf9-213">Zákonné požadavky využívají zvýšení výpočetní kapacity pro přidání do požadavků na modelování.</span><span class="sxs-lookup"><span data-stu-id="efaf9-213">The regulatory requirements use increasing compute capacity to add to modeling requirements.</span></span> <span data-ttu-id="efaf9-214">Regulační orgány žádají o častější a aktuální hodnoty rizik.</span><span class="sxs-lookup"><span data-stu-id="efaf9-214">The regulators are asking for more frequent and up-to date risk figures.</span></span>

2.  <span data-ttu-id="efaf9-215">Stávající riziková infrastruktura je organicky s časem a vytváří problémy při implementaci nových požadavků a pokročilejšího modelování rizik agilním způsobem.</span><span class="sxs-lookup"><span data-stu-id="efaf9-215">The existing risk infrastructure has grown organically with time and creates challenges when implementing new requirements and more advanced risk modeling in an agile manner.</span></span>

<span data-ttu-id="efaf9-216">Služby založené na cloudu můžou poskytovat požadované funkce a podporovat analýzu rizik.</span><span class="sxs-lookup"><span data-stu-id="efaf9-216">Cloud-based services can deliver the required functionality and support risk analysis.</span></span> <span data-ttu-id="efaf9-217">Tento přístup má několik výhod:</span><span class="sxs-lookup"><span data-stu-id="efaf9-217">This approach has some advantages:</span></span>

-  <span data-ttu-id="efaf9-218">Nejběžnější Výpočty rizik vyžadované regulátorem musí být implementovány všemi v rámci nařízení.</span><span class="sxs-lookup"><span data-stu-id="efaf9-218">The most common risk calculations required by the regulator must be implemented by everyone under the regulation.</span></span> <span data-ttu-id="efaf9-219">Díky využití služeb od specializovaného poskytovatele služeb se analytik výhodně dopravuje na použití, výpočty rizik kompatibilní s regulátorem.</span><span class="sxs-lookup"><span data-stu-id="efaf9-219">By utilizing services from a specialized service provider, the analyst benefits from ready to use, regulator-compliant risk calculations.</span></span> <span data-ttu-id="efaf9-220">Tyto služby můžou zahrnovat výpočty rizik trhu, výpočty rizik protistrany, úpravu hodnot X-Value (XVA) a dokonce i základní kontrolu výpočtů obchodních knih (FRTB).</span><span class="sxs-lookup"><span data-stu-id="efaf9-220">Such services may include market risk calculations, counterparty risk calculations, X-Value Adjustment (XVA), and even Fundamental Review of Trading Book (FRTB) calculations.</span></span>

- <span data-ttu-id="efaf9-221">Tyto služby zpřístupňují svá rozhraní prostřednictvím webových služeb.</span><span class="sxs-lookup"><span data-stu-id="efaf9-221">These services expose their interfaces through web services.</span></span> <span data-ttu-id="efaf9-222">Stávající rizikové infrastruktury je možné zvýšit pomocí těchto dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="efaf9-222">The existing risk infrastructure can be enhanced by these other services.</span></span>

<span data-ttu-id="efaf9-223">V našem příkladu chceme vyvolat cloudovou službu pro FRTB výpočty.</span><span class="sxs-lookup"><span data-stu-id="efaf9-223">In our example, we want to invoke a cloud-based service for FRTB calculations.</span></span> <span data-ttu-id="efaf9-224">Několik z nich najdete v [AppSource](https://appsource.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely).</span><span class="sxs-lookup"><span data-stu-id="efaf9-224">Several of these can be found on [AppSource](https://appsource.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely).</span></span> <span data-ttu-id="efaf9-225">V tomto článku jsme zvolili možnost zkušební verze z [rizika vektoru](http://www.vectorrisk.com/).</span><span class="sxs-lookup"><span data-stu-id="efaf9-225">For this article we chose a trial option from [Vector Risk](http://www.vectorrisk.com/).</span></span> <span data-ttu-id="efaf9-226">Budeme dál měnit náš systém.</span><span class="sxs-lookup"><span data-stu-id="efaf9-226">We will continue to modify our system.</span></span> <span data-ttu-id="efaf9-227">Tentokrát používáme službu k výpočtu rizikového významu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-227">This time, we use a service to calculate the risk figure of interest.</span></span> <span data-ttu-id="efaf9-228">Tento proces se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="efaf9-228">This process consists of the following steps:</span></span>

1. <span data-ttu-id="efaf9-229">Zavolejte relevantní rizikové služby a se správnými parametry.</span><span class="sxs-lookup"><span data-stu-id="efaf9-229">Call the relevant risk service and with the right parameters.</span></span>

2. <span data-ttu-id="efaf9-230">Počkejte, až služba dokončí výpočet.</span><span class="sxs-lookup"><span data-stu-id="efaf9-230">Wait until the service finishes the calculation.</span></span>

3. <span data-ttu-id="efaf9-231">Načte a zahrňte výsledky do analýzy rizik.</span><span class="sxs-lookup"><span data-stu-id="efaf9-231">Retrieve and incorporate the results into the risk analysis.</span></span>

<span data-ttu-id="efaf9-232">Přeloženo do kódu R, náš kód R lze rozšířit definicí požadovaných vstupních hodnot z připravené vstupní šablony.</span><span class="sxs-lookup"><span data-stu-id="efaf9-232">Translated into R code, our R code can be enhanced by the definition of the required input values from a prepared input template.</span></span>

````R
Template <- readLines('RequiredInputData.json')
data <- list(
# drilldown setup
  timeSteps = seq(0, n, by = 1),
  paths = as.integer(seq(0, nt, length.out = min(nt, 100))),
# calc setup
  calcDate = instrument.startDate,
  npaths = nt,
  price = P0,
  vol = sigma,
  drift = mu,
  premium = instrument.premium,
  maturityDate = today()
  )
body <- whisker.render(template, data)
````


<span data-ttu-id="efaf9-233">Dál je potřeba zavolat webovou službu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-233">Next, we need to call the web service.</span></span> <span data-ttu-id="efaf9-234">V tomto případě zavoláme metodu StartCreditExposure pro aktivaci výpočtu.</span><span class="sxs-lookup"><span data-stu-id="efaf9-234">In this case, we call the StartCreditExposure method to trigger the calculation.</span></span> <span data-ttu-id="efaf9-235">Koncový bod pro rozhraní API ukládáme do proměnné pojmenovaného *koncového bodu*.</span><span class="sxs-lookup"><span data-stu-id="efaf9-235">We store the endpoint for the API in a variable named *endpoint*.</span></span>

````R
# make the call
result <- POST( paste(endpoint, "StartCreditExposure", sep = ""),  
                authenticate(username, password, type = "basic"),
                content_type("application/json"),
                add_headers(`Ocp-Apim-Subscription-Key` = api_key),
                body = body, encode = "raw"
               )

result <- content(result)
````

<span data-ttu-id="efaf9-236">Po dokončení výpočtů načtěte výsledky.</span><span class="sxs-lookup"><span data-stu-id="efaf9-236">Once the calculations have finished, we retrieve the results.</span></span>

````R
# get back high level results
result <- POST( paste(endpoint, "GetCreditExposureResults", sep = ""), 
                authenticate(username, password, type = "basic"),
                content_type("application/json"),
                add_headers(`Ocp-Apim-Subscription-Key` = api_key),
               body = sprintf('{"getCreditExposureResults": {"token":"DataSource=Production;Organisation=Microsoft", "ticket": "%s"}}', ticket), encode = "raw")

result <- content(result)
````

<span data-ttu-id="efaf9-237">Tím se analytikovi ponechá, aby pokračoval s obdrženými výsledky.</span><span class="sxs-lookup"><span data-stu-id="efaf9-237">This leaves the analyst to continue with the results received.</span></span> <span data-ttu-id="efaf9-238">Relevantní rizikové údaje o úrocích jsou extrahovány z výsledků a vykresleny.</span><span class="sxs-lookup"><span data-stu-id="efaf9-238">The relevant risk figures of interest are extracted from the results and plotted.</span></span>

````R
if (!is.null(result$error)) {
    cat(result$error$message)
} else {
    # plot PFE
    result <- result$getCreditExposureResultsResponse$getCreditExposureResultsResult
    df <- do.call(rbind, result$exposures)
    df <- as.data.frame(df)
    df <- subset(df, term <= n)   
}

plot(as.numeric(df$term[df$statistic == 'PFE']) / 365, df$result[df$statistic == 'PFE'], type = "p", xlab = ("time t in Years"), ylab = ("Potential Future Exposure in USD"), ylim = range(c(df$result[df$statistic == 'PFE'], df$result[df$statistic == 'PFE'])), col = "red")
````


<span data-ttu-id="efaf9-239">Výsledný graf vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="efaf9-239">The resulting plots look like this:</span></span>

|       |     |
|----    |---- |
| <img src="./assets/fsi-risk-modeling/image6.png" width="400px" alt="Figure 4 - Credit Exposure for MSFT Equity Forward - Calculated with a Cloud Based Risk Engine"/> | <img src="./assets/fsi-risk-modeling/image7.png" width="400px" alt="Figure 5 - Potential Future Exposure for MSFT Equity Forward - Calculated with a Cloud  Based Risk Engine" /> |
| <span data-ttu-id="efaf9-240">Obrázek 4: vystavení kreditu pro dopředné jmění protokolu MSFT –</span><span class="sxs-lookup"><span data-stu-id="efaf9-240">Figure 4 - Credit Exposure for MSFT Equity Forward -</span></span> <br/><span data-ttu-id="efaf9-241">Vypočítáno pomocí cloudového rizikového modulu</span><span class="sxs-lookup"><span data-stu-id="efaf9-241">Calculated with a Cloud Based Risk Engine</span></span> | <span data-ttu-id="efaf9-242">Obrázek 5 – potenciální budoucí riziko pro přeposílání protokolu MSFT</span><span class="sxs-lookup"><span data-stu-id="efaf9-242">Figure 5 - Potential Future Exposure for MSFT Equity Forward -</span></span> <br/> <span data-ttu-id="efaf9-243">Vypočítáno pomocí cloudového rizikového modulu</span><span class="sxs-lookup"><span data-stu-id="efaf9-243">Calculated with a Cloud  Based Risk Engine</span></span> |



## <a name="next-steps"></a><span data-ttu-id="efaf9-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="efaf9-244">Next Steps</span></span>

<span data-ttu-id="efaf9-245">Flexibilní přístup ke cloudu prostřednictvím výpočetní infrastruktury a SaaS rizikových analytických služeb může poskytovat vylepšení rychlosti a flexibility pro rizikové analytiky, kteří pracují na velkých trzích a v pojišťovnictví.</span><span class="sxs-lookup"><span data-stu-id="efaf9-245">Flexible access to the cloud through compute infrastructure and SaaS-based risk analysis services can deliver improvements in speed and agility for risk analysts working in capital markets and insurance.</span></span> <span data-ttu-id="efaf9-246">V tomto článku jsme pracovali v příkladu, který ukazuje, jak používat Azure a další služby pomocí nástrojů, které ví o rizikových analytikích.</span><span class="sxs-lookup"><span data-stu-id="efaf9-246">In this article we worked through an example which illustrates how to use Azure and other services using tools risk analysts know.</span></span> <span data-ttu-id="efaf9-247">Zkuste využít výhody možností Azure při vytváření a vylepšit modely rizik.</span><span class="sxs-lookup"><span data-stu-id="efaf9-247">Try taking advantage of Azure's capabilities as you create and enhance your risk models.</span></span>

### <a name="tutorials"></a><span data-ttu-id="efaf9-248">Kurzy</span><span class="sxs-lookup"><span data-stu-id="efaf9-248">Tutorials</span></span>

- <span data-ttu-id="efaf9-249">Vývojáři r: [spuštění paralelní simulace r s Azure Batch](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)</span><span class="sxs-lookup"><span data-stu-id="efaf9-249">R Developers: [Run a parallel R simulation with Azure   Batch](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)</span></span>

- [<span data-ttu-id="efaf9-250">Základní příkazy R a funkce RevoScaleR: 25 běžných příkladů</span><span class="sxs-lookup"><span data-stu-id="efaf9-250">Basic R commands and RevoScaleR functions: 25 common   examples</span></span>](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler?WT.mc_id=fsiriskmodelr-docs-scseely)

- [<span data-ttu-id="efaf9-251">Vizualizace a analýza dat pomocí RevoScaleR</span><span class="sxs-lookup"><span data-stu-id="efaf9-251">Visualize and analyze data using   RevoScaleR</span></span>](https://docs.microsoft.com/machine-learning-server/r/tutorial-revoscaler-data-model-analysis?WT.mc_id=fsiriskmodelr-docs-scseely)

- [<span data-ttu-id="efaf9-252">Úvod ke službám ML a open source funkcí R v HDInsight</span><span class="sxs-lookup"><span data-stu-id="efaf9-252">Introduction to ML Services and open-source R capabilities on   HDInsight</span></span>](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview?WT.mc_id=fsiriskmodelr-docs-scseely)

<span data-ttu-id="efaf9-253">_Tento článek vytvořila služba Dr. Darko Mocelj a Rupert Nicolay._</span><span class="sxs-lookup"><span data-stu-id="efaf9-253">_This article was authored by Dr. Darko Mocelj and Rupert Nicolay._</span></span>