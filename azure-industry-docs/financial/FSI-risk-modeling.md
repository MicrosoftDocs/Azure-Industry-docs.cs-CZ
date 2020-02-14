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
# <a name="enabling-the-financial-services-risk-lifecycle-with-azure-and-r"></a>Povolení životního cyklu rizik finančních služeb s Azure a R


Výpočty rizik jsou v průběhu životního cyklu operací klíčových finančních služeb pivotované v několika fázích. Zjednodušená forma životního cyklu správy produktů v pojišťovnictví může například vypadat přibližně jako diagram níže. Aspekty výpočtu rizik se zobrazují v modrém textu.

![](./assets/fsi-risk-modeling/image1.png)

Scénář velkých trhů v podniku může vypadat takto:

![](./assets/fsi-risk-modeling/image2.png)

Prostřednictvím těchto procesů existují běžné požadavky na modelování rizik, včetně:

1. Nutnost experimentů s ad hoc rizik pomocí rizikových analytiků; Pojistní matematici v pojišťovací firmě nebo v quants v podniku velkých trhů.
    Tito analytiké obvykle pracují s oblíbenými nástroji pro kódování a modelování v jejich doméně: R a Python. Mnoho studijních kurzů zahrnuje školení v R nebo Pythonu ve matematických financích a v MBAch kurzech.
    Oba jazyky nabízejí široké spektrum Open Source knihoven, které podporují populární výpočty rizik. Společně s vhodným nástrojem analytik často vyžaduje přístup k:

    a.  Přesná data o cenách trhu.

    b.  Existující zásady a data deklarací identity.

    c.  Existující data pozice na trhu.

    d.  Jiná externí data. Zdroje obsahují strukturovaná data, jako jsou tabulky úmrtnosti a konkurenční údaje o cenách. Můžou se používat i méně tradiční zdroje, jako je třeba počasí, zprávy a jiné.

    e.  Výpočetní kapacita umožňující rychlé interaktivní vyšetřování dat.

2. Můžou také využít algoritmy služby Machine Learning pro ad hoc pro ceny nebo stanovit strategii trhu.

3. Nutnost vizualizovat a prezentovat data pro použití při plánování produktů, obchodní strategii a podobných diskusích.

4. Rychlé provádění definovaných modelů konfigurovaných analytiky, pro ceny, oceňování a rizika trhu. Oceňování využívají kombinaci vyhrazeného modelování rizik, nástrojů pro tržní rizika a vlastní kód. Analýza se provádí v dávce s různou noc, týdně, měsíčně, čtvrtletně a ročními výpočty, které generují špičky v úlohách.

5. Integrace dat s dalšími rizikovými mírami v rozlehlých sítích pro sestavy konsolidovaného rizika. V rámci větších organizací se odhad rizika na nižší úrovni může přenést do modelování podnikových rizik a nástroje pro vytváření sestav.

6. Výsledky se musí nahlásit v definovaném formátu v požadovaném intervalu, aby se splnily požadavky na investor a zákonné předpisy.

Společnost Microsoft podporuje výše uvedené otázky prostřednictvím kombinace služeb Azure a partnerských nabídek v [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely). V tomto článku se seznámíme s praktickými příklady toho, jak provádět ad hoc experimenty pomocí jazyka R. Začneme tím, že vysvětlíme, jak experiment spustit na jednom počítači, a pak zobrazit, jak spustit stejný experiment na [Azure Batch](https://docs.microsoft.com/azure/batch/?WT.mc_id=fsiriskmodelr-docs-scseely)a zavřít tím, že využijete výhod externích služeb v našem modelování. Možnosti a požadavky na spouštění definovaných modelů v Azure jsou popsány v těchto článcích, které se zaměřují na [bankovnictví](https://docs.microsoft.com/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely) a [pojišťovnictví](https://docs.microsoft.com/azure/industry/financial/actuarial-risk-analysis-and-financial-modeling-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely).

## <a name="analyst-modelling-in-r"></a>Modelování analytiků v jazyce R

Začněte tím, že si vyhledáme, jak může být v analytikovi ve zjednodušeném scénáři s reprezentativními kapitálovými trhy použit R. Můžete to vytvořit buď odkazem na existující knihovnu R pro výpočet, nebo zápisem kódu od začátku. V našem příkladu je také nutné načíst externí data o cenách. Aby byl příklad jednoduchý, ale ilustrativní, vypočítáme potenciální budoucí expozici (PFE) akcií, který je dál uzavřený.
V tomto příkladu se vyhnete komplexním kvantitativním technikům modelování pro nástroje, jako jsou složité deriváty, a zaměřuje se na jediný rizikový faktor, který se soustředí na životní cyklus rizika. Náš příklad provede následující:

1. Vyberte instrumentaci zájmu.

2. Zdroje historických cen za nástroj.

3. Modelujte cenu za jmění pomocí jednoduchého výpočtu Monte Carlo (MC), který používá geometrické Brownianové pohybu (GBM):

    a.  Odhadované očekávané vrácení μ (mu) a σ nestálosti (théta).

    b.  Proveďte kalibraci modelu u historických dat.

4. Vizualizujte různé cesty pro sdělování výsledků.

5. Maximální hodnota grafu (0, skladová hodnota) pro předvedení významu PFE, rozdíl na riziko hodnoty rizika (VaR)

    a.  Postup při objasnění: PFE = Share Price (T)--Price Contract Price K

6. Pořiďte 0,95 Quantile, aby se v každém časovém intervalu a konci simulace získala hodnota PFE.

Vyhodnotit potenciální budoucí riziko pro jmění na základě zásob protokolu MSFT. Jak je uvedeno výše, pro modelování zásob akcií se vyžadují historické ceny za sklady MSFT, abychom mohli model kalibrovat na historická data. Existuje mnoho způsobů, jak získat historické ceny za cenných papírů. V našem příkladu používáme bezplatnou verzi skladové ceny služby od externího poskytovatele služeb, [Quandl](https://www.quandl.com/).


> Poznámka: příklad používá [datovou sadu cen wiki](https://www.quandl.com/databases/WIKIP) , kterou je možné použít pro učení konceptů. Pro využívání produkčních akcií založených na USA Quandl doporučuje použít [datovou sadu cen na konci dne US](https://www.quandl.com/data/EOD-End-of-Day-US-Stock-Prices).

Abychom mohli zpracovávat data a definovat riziko spojené s tímto jměním, musíme udělat následující věci:

1. Načte data historie z vlastního jmění.

2. Z historických dat určete očekávaný návratový σ μ a nestálosti.

3. Vymodelujte základní ceny akcií pomocí některé simulace.

4. Spuštění modelu

5. V budoucnu určete expozici akcií.

Začneme načtením akcie ze služby Quandl a vykreslíme historii uzavíracích cen za posledních 180 dní.

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

S daty je k dispozici GBM model.

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

V dalším kroku provedeme modelování základních cen akcií. Můžeme buď implementovat diskrétní proces GBM od začátku, nebo využít jeden z mnoha balíčků R, které tuto funkci poskytují. Používáme balíček R [ *kvalifikacemi* (simulace a odvozování pro rozdílové rovnice stochastického)](https://cran.r-project.org/web/packages/sde/index.html) , který poskytuje metodu řešení tohoto problému. Metoda GBM vyžaduje sadu parametrů, které jsou buď kalibrovány na historické údaje nebo zadány jako parametry simulace. K dispozici jsou historické údaje, které na začátku simulace (P0) poskytují μ, σ a ceny akcií.

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

Nyní jsme připraveni zahájit simulaci Monte Carlo, která bude modelovat potenciální riziko pro určitý počet cest simulace. Tuto simulaci omezíme na 50 Monte cesty Carlo a kroky 256 času. V rámci přípravy na škálování pro účely horizontálního navýšení kapacity simulace a využití paralelismu v R se smyčka simulace Monte Carlo používá příkaz foreach.

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

Teď jsme simulovali cenu za základní zásobu protokolu MSFT. Pokud chcete vypočítat expozici jmění, odečte prémii a omezíme vystavení pouze na kladné hodnoty.

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

Následující dva obrázky znázorňují výsledek simulace. První obrázek zobrazuje simulaci Monte Carloe základní skladové ceny za 50 cest. Druhý obrázek znázorňuje základní úvěrové riziko pro daný jmění, po odečtení úrovně Premium jmění před a omezením ozáření na kladné hodnoty.


|       |    |
|---    |--- |
| <img src="./assets/fsi-risk-modeling/image3.png" width="400px" alt="Figure 1 - 50 Monte Carlo Paths"/> | <img src="./assets/fsi-risk-modeling/image4.png" width="400px" alt="Figure 2 - Credit Exposure for Equity Forward"/> |
| Obrázek 1-50 Monte cesty Carlo | Obrázek 2 – vystavení kreditu pro přeposlání jmění |


V posledním kroku se v rámci následujícího kódu vypočítá Quantile PFE o 1 měsíc 0,95.

````R
# Calculate the PFE at each time step
df_pfe <- cbind(t,apply(pfe_mc,2,quantile,probs = instrument.pfeQuantile ))

resulting in the final PFE plot
plot(df_pfe, t = 'l', ylab = "Potential Future Exposure in USD", xlab = "time t in Years")
````

<img src="./assets/fsi-risk-modeling/image5.png" width="500px" alt="Potential Future Exposure for MSFT Equity Forward" /> 

Obrázek 3 – možná budoucí expozice pro MSFT jmění dopředu

## <a name="using-azure-batch-with-r"></a>Použití Azure Batch s R 

Výše popsané řešení jazyka R lze připojit k Azure Batch a využít Cloud k výpočtům rizik. To má trochu dodatečné úsilí pro paralelní výpočet, jako je náš. V tomto kurzu [spustíte paralelní simulaci r s Azure Batch, kde](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)najdete podrobné informace o připojení R k Azure Batch. Dál zobrazujeme kód a souhrn procesu pro připojení k Azure Batch a využití tohoto rozšíření pro Cloud ve zjednodušeném PFE výpočtu.

Tento příklad prochází stejný model popsaný výše. Jak jsme viděli dříve, tento výpočet může běžet v našem osobním počítači. Zvýšení počtu MONTECH cest Carlo nebo použití kratších časových kroků bude mít za následek mnohem delší dobu spuštění. Skoro všechen kód R zůstane beze změny. Rozdíly v této části vyzvýrazníme.

Každá cesta simulace Monte Carlo běží v Azure. To můžeme udělat, protože každá cesta je nezávislá na ostatních, takže nám pomůžeme vypočítat "zpracovatelné Parallel".

Pokud chcete použít Azure Batch, definujeme základní cluster a bude na něj odkazovat v kódu předtím, než bude možné cluster použít ve výpočtech. Ke spuštění výpočtů používáme následující definici typu cluster. JSON:

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

V této definici clusteru používá následující kód R k tomu cluster:
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

Nakonec aktualizujeme příkaz foreach z výše na použití balíčku balíček doazureparallel. Jedná se o menší změnu, přidání odkazu na balíček kvalifikacemi a změna% do% na% dopar%:

````R
# Execute the MC simulation for the wiener process utilizing the GBM method from the sde package and extend the computation to the cloud
exposure_mc <- foreach(i = 1:nt, .combine = rbind, .packages = 'sde') %dopar% GBM(x = P0, r = mu, sigma = sigma, T = T, N = n)
rownames(exposure_mc) <- c()
````

Každá simulace Monte Carlo je odeslána jako úloha Azure Batch. Úloha se spustí v cloudu. Výsledky se sloučí před odesláním zpět do aplikace analytiky. Těžké zvedací a výpočetní operace se provádějí v cloudu, aby plně využily výhody škálování a základní infrastruktury vyžadované požadovanými výpočty.

Po dokončení výpočtů můžete další prostředky snadno vypnout tím, že vyvoláte tuto jednu instrukci:

````R
# Stop the Cloud cluster
stopCluster(cluster)
````


## <a name="use-a-saas-offering"></a>Použití nabídky SaaS

První dva příklady ukazují, jak využít místní a cloudovou infrastrukturu pro vývoj vhodného modelu hodnocení. Tento paradigma začal Shift. Stejným způsobem, jakým se místní infrastruktura transformuje na cloudové služby IaaS a PaaS, modelování relevantních rizikových údajů transformuje na proces orientovaný na služby.
Dnešní analytici čelí dvěma významným problémům:

1. Zákonné požadavky využívají zvýšení výpočetní kapacity pro přidání do požadavků na modelování. Regulační orgány žádají o častější a aktuální hodnoty rizik.

2.  Stávající riziková infrastruktura je organicky s časem a vytváří problémy při implementaci nových požadavků a pokročilejšího modelování rizik agilním způsobem.

Služby založené na cloudu můžou poskytovat požadované funkce a podporovat analýzu rizik. Tento přístup má několik výhod:

-  Nejběžnější Výpočty rizik vyžadované regulátorem musí být implementovány všemi v rámci nařízení. Díky využití služeb od specializovaného poskytovatele služeb se analytik výhodně dopravuje na použití, výpočty rizik kompatibilní s regulátorem. Tyto služby můžou zahrnovat výpočty rizik trhu, výpočty rizik protistrany, úpravu hodnot X-Value (XVA) a dokonce i základní kontrolu výpočtů obchodních knih (FRTB).

- Tyto služby zpřístupňují svá rozhraní prostřednictvím webových služeb. Stávající rizikové infrastruktury je možné zvýšit pomocí těchto dalších služeb.

V našem příkladu chceme vyvolat cloudovou službu pro FRTB výpočty. Několik z nich najdete v [AppSource](https://appsource.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely). V tomto článku jsme zvolili možnost zkušební verze z [rizika vektoru](http://www.vectorrisk.com/). Budeme dál měnit náš systém. Tentokrát používáme službu k výpočtu rizikového významu. Tento proces se skládá z následujících kroků:

1. Zavolejte relevantní rizikové služby a se správnými parametry.

2. Počkejte, až služba dokončí výpočet.

3. Načte a zahrňte výsledky do analýzy rizik.

Přeloženo do kódu R, náš kód R lze rozšířit definicí požadovaných vstupních hodnot z připravené vstupní šablony.

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


Dál je potřeba zavolat webovou službu. V tomto případě zavoláme metodu StartCreditExposure pro aktivaci výpočtu. Koncový bod pro rozhraní API ukládáme do proměnné pojmenovaného *koncového bodu*.

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

Po dokončení výpočtů načtěte výsledky.

````R
# get back high level results
result <- POST( paste(endpoint, "GetCreditExposureResults", sep = ""), 
                authenticate(username, password, type = "basic"),
                content_type("application/json"),
                add_headers(`Ocp-Apim-Subscription-Key` = api_key),
               body = sprintf('{"getCreditExposureResults": {"token":"DataSource=Production;Organisation=Microsoft", "ticket": "%s"}}', ticket), encode = "raw")

result <- content(result)
````

Tím se analytikovi ponechá, aby pokračoval s obdrženými výsledky. Relevantní rizikové údaje o úrocích jsou extrahovány z výsledků a vykresleny.

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


Výsledný graf vypadá takto:

|       |     |
|----    |---- |
| <img src="./assets/fsi-risk-modeling/image6.png" width="400px" alt="Figure 4 - Credit Exposure for MSFT Equity Forward - Calculated with a Cloud Based Risk Engine"/> | <img src="./assets/fsi-risk-modeling/image7.png" width="400px" alt="Figure 5 - Potential Future Exposure for MSFT Equity Forward - Calculated with a Cloud  Based Risk Engine" /> |
| Obrázek 4: vystavení kreditu pro dopředné jmění protokolu MSFT – <br/>Vypočítáno pomocí cloudového rizikového modulu | Obrázek 5 – potenciální budoucí riziko pro přeposílání protokolu MSFT <br/> Vypočítáno pomocí cloudového rizikového modulu |



## <a name="next-steps"></a>Další kroky

Flexibilní přístup ke cloudu prostřednictvím výpočetní infrastruktury a SaaS rizikových analytických služeb může poskytovat vylepšení rychlosti a flexibility pro rizikové analytiky, kteří pracují na velkých trzích a v pojišťovnictví. V tomto článku jsme pracovali v příkladu, který ukazuje, jak používat Azure a další služby pomocí nástrojů, které ví o rizikových analytikích. Zkuste využít výhody možností Azure při vytváření a vylepšit modely rizik.

### <a name="tutorials"></a>Kurzy

- Vývojáři r: [spuštění paralelní simulace r s Azure Batch](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)

- [Základní příkazy R a funkce RevoScaleR: 25 běžných příkladů](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler?WT.mc_id=fsiriskmodelr-docs-scseely)

- [Vizualizace a analýza dat pomocí RevoScaleR](https://docs.microsoft.com/machine-learning-server/r/tutorial-revoscaler-data-model-analysis?WT.mc_id=fsiriskmodelr-docs-scseely)

- [Úvod ke službám ML a open source funkcí R v HDInsight](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview?WT.mc_id=fsiriskmodelr-docs-scseely)

_Tento článek vytvořila služba Dr. Darko Mocelj a Rupert Nicolay._