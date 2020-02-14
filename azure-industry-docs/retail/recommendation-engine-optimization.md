---
title: Použití doporučených systémů a algoritmů R s Azure
author: kbaroni
ms.author: kbaroni
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Postup opětovného použití a optimalizace aplikací s doporučeními napsaných v jazyce R Spoléhá Machine Learning Server na virtuální počítače Azure.
ms.openlocfilehash: c5c35de681abc52641952f8bc9e95095b9d99d97
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054214"
---
# <a name="optimize-and-reuse-an-existing-recommendation-system"></a>Optimalizace a opakované použití stávajícího systému doporučení  

Tento dokument popisuje proces úspěšného použití a vylepšení stávajícího systému doporučení napsaného v jazyce R. Tento klíč byl paralelismus knihoven MicrosoftML a RevoScaleR integrovaných do Microsoft Machine Learning Server.

## <a name="recommendation-systems-and-r"></a>Systémy doporučení a R

V případě maloobchodního prodeje je porozumět zákaznickým požadavkům a historii nákupů výhod. Maloobchodní prodejci používají taková data pro roky v kombinaci se službou Machine Learning k identifikaci produktů relevantních pro daného příjemce a dodávají přizpůsobené možnosti nákupu. Tento přístup se nazývá **doporučení produktu** a generuje významný datový proud příjmů pro maloobchodníky. Systémy pro doporučení, které vám pomůžou zodpovědět otázky: *Jaký film bude tato osoba sledovat další? S jakými dalšími službami je tento zákazník pravděpodobně zajímat? Kde bude tento zákazník chtít dovolenou?*
Nejnovější zákazník chtěl získat informace o tom, že *budou spotřebitelé (předplatitelé) obnovovat své smlouvy?* Zákazník měl existující doporučený model, který by měl předpověď pravděpodobnosti, že předplatitele obnovuje kontrakt. Po vygenerování prognózy se použilo další zpracování pro klasifikaci odpovědi na Ano, ne nebo možná. Odpověď modelu byla potom integrována do obchodního procesu služby Call Center. Tento proces povolil agentovi služby poskytovat individuální doporučení pro uživatele.  
Mnohé z dřívějších analytických produktů od tohoto zákazníka byly postaveny v [programovacím jazyce R](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server), včetně modelu strojového učení, v jádru systému doporučení. Vzhledem k tomu, že jejich základ předplatitele vzrostl, musí mít požadavky na data a výpočetní výkon. To znamená, že úlohy doporučení teď painfully pomalu a nenáročným způsobem. V současnosti je Python stále součástí strategie analytického produktu. V blízké době ale musí zachovat investici R a najít efektivnější proces vývoje a nasazení. Výzvou bylo optimalizovat stávající přístup pomocí možností v Azure. Na úkol jsme se zavedli, abychom k tomu mohli poskytnout – a ověřit – technologický zásobník pro kontrolu konceptu pro úlohy s doporučeními. Tady shrnujeme obecný přístup, který je možné použít pro podobné projekty.  

## <a name="design-goals"></a>Cíle návrhu

Klíčovou prioritou bylo přenavrhovat a automatizovat pracovní postup modelu, který poskytuje několik výhod:

- Rychlejší vývojové cykly modelů a rychlejší inovace v čase. Když jsou vývojové cykly pro přípravu a vývoj dat efektivní, může se objem experimentů zvýšit.  
- Přesnější výsledky modelu a lepší doporučení prostřednictvím častých reškolicích cyklů pro nová data.
- Lépe škálovatelná implementace ověření koncepce – ta, která by fungovala v plné produkci.
- Rychlejší doba provozu díky automatizaci procesů mezi vývojem, testováním a nasazením. Automatizace také snižuje provozní chyby a časy prodlevy.  

## <a name="requirements"></a>Požadavky

Změna návrhu pracovního postupu má následující požadavky:

- Využijte stávající dovednosti a znalosti v R týmu.
- Znovu použijte kód, kde má smysl.
- Ukládat data předplatitelů v databázi, aby bylo možné snadno a rychle integrovat do modelu školení a rekurze.
- Vyvolejte přeškolení a bodování modelu prostřednictvím rozhraní webové aplikace.
- Pro ověřování na front-endu webu použijte Azure Active Directory.

## <a name="technology-stack"></a>Technologický zásobník

Kanál pro tento projekt vyžadoval více technologií a nástrojů a Centrované kolem čtyř oblastí:

1. Stálost a přístupnost dat předplatitele.
2. Vývoj, školení a výběr modelů.  
3. Nasazení modelu a provozní prostředí.
4. Použití webové aplikace k vyvolání bodování a opětovného školení modelu.

Technologické komponenty v diagramu kanálu jsou podrobněji popsány níže.

![Architektura optimalizace](assets/recommendation-engine-optimization/recommendation-architecture.png)

### <a name="microsoft-machine-learning-server"></a>Server Microsoft Machine Learning

Hlavní důvod pro výběr úloh R: **[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)** a **Microsoft ml**. Funkce, které jsou součástí těchto balíčků, se v celém kódu používaly k importu dat, vytvoření modelů klasifikace a jejich nasazení do produkčního prostředí.
MLS bylo nasazeno na dva virtuální počítače se systémem Linux v Azure: jedna konfigurovaná pro vývoj a jedna konfigurovaná pro "operace". Vývojový virtuální počítač byl zřízený s podstatně větší pamětí a výkonem, aby se usnadnilo školení a testování stovek modelů. Také hostovaný RStudio Server, který poskytuje snadný přístup k IDE RStudio pro vzdálené uživatele. Provozní server byl nakonfigurován na menším virtuálním počítači s dalšími rozšířeními nezbytnými pro hostování modelů R, které byly volat z webové aplikace prostřednictvím rozhraní REST API.

### <a name="rstudio-server"></a>RStudio Server

**[RStudio Server](https://www.rstudio.com/products/rstudio/#Server)** je aplikace pro Linux, která poskytuje rozhraní založené na prohlížeči pro vzdálené nebo přenosné klienty. Běží na portu 8787 a je k dispozici pro vzdálená připojení, jakmile se na virtuálním počítači Azure vytvoří pravidlo zabezpečení sítě. Pro analytiky a odborníky přes data, kteří preferují RStudio IDE, může být efektivní možností pro poskytování přístupu k virtuálnímu počítači s velkou výpočetní kapacitou a kapacitou paměti. Je k dispozici ke stažení ve zdrojové i komerční edici.

### <a name="azure-sql-database"></a>Azure SQL Database

Data předplatitele byla původně uložena v jednom velmi velkém souboru. csv s 6 000 000 řádky informací o nákupu a preferencích pro jedinečné předplatitele 500. Uložení dat v databázi znamená rychlejší přístup k datům v jazyce R a umožňuje filtrovaných čtení. Už nemusíte naimportovat celou datovou sadu pro školení nebo rekurzování: data se filtrují předplatitelem ve zdroji databáze, což významně snižuje prostředky potřebné k importu a zpracování dat.  
V Azure je několik možností spravovaných cloudových databází. [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) byla vybrána z důvodu znalosti zákazníka s SQL Server a – důležitější je, v budoucích plánech zavést SQL Server Machine Learning Services na širší škálu Azure SQL Database. [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017) jsou funkce v databázi pro provádění úloh R a Python prostřednictvím uložených procedur.

### <a name="nodejs-and-reactjs"></a>Node. js a reagovat. js

Webové rozhraní bylo vytvořeno za účelem vyvolání skriptů jazyka R a zabezpečení webu. Bylo vybráno Node. js jako rozhraní webového serveru, protože umožňuje pro webové aplikace v distribuovaném prostředí lehký a efektivní běhové prostředí. Reakce se používá k sestavení uživatelských rozhraní a zastavování na front-endu a volá webové služby hostované na webovém serveru. Zjistilo se, že uzel + reakce by poskytoval nejrychlejší cestu pro webové služby pro kanál modelu.  

## <a name="infrastructure-implementation"></a>Implementace infrastruktury

Níže uvedené části popisují, jak byla infrastruktura serveru pro tento projekt nasazena. Získání správné infrastruktury pro vývoj a nasazení může být důležité jako určení způsobu modelování a postupů, které se použijí.

### <a name="initial-database-load"></a>Počáteční načtení databáze

Prvním krokem bylo importovat data předplatitelů z velmi velkého souboru. CSV do Azure SQL Database. K dispozici je více možností pro import dat do Azure SQL Database popsaných v tomto [odkazu](https://blogs.msdn.microsoft.com/sqlcat/2010/07/30/loading-data-to-sql-azure-the-fast-way/). Tady je postup:

1. Databázi jste vytvořili Azure Portal [následujícím postupem.](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)
2. Pro připojení k databázi z virtuálního počítače se stáhla a použila [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) .
3. Vybrali jste [Průvodce importem/exportem SQL](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard?view=sql-server-2017) (Pokud jste časově omezili více možností pro import dat). Mějte na paměti, že Průvodce importem a exportem mapuje datové typy ze zdroje dat do cílového cíle; a v našem scénáři byly všechny datové prvky namapovány na datový typ varchar (max), který byl přijatelný. Pokud váš scénář vyžaduje jiné mapování, můžete upravit typy DataTypes v průvodci ([odkaz](https://docs.microsoft.com/sql/integration-services/import-export-data/data-type-mapping-in-the-sql-server-import-and-export-wizard?view=sql-server-2017)).  
4. Vzhledem k tomu, že většina dotazů odeslaných do databáze filtru *subscriber_id*, vytvořili jsme pro toto pole index.

### <a name="web-application"></a>Webová aplikace

Webová aplikace zodpovídá za tři funkce:

- Ověřování: uživatelé webu ověřují před *Azure Active Directory* prostřednictvím *reagující* front-endu.
- Bodování modelu: přijímá vstupní data od uživatele pro konkrétního předplatitele, odesílá data předplatitele webové službě pomocí REST API, která vrací odpověď předpovědi.  
- Opětovné školení modelů: přijímá identifikátor odběratele jako vstup a d vyvolá skript R na vývojovém serveru, aby znovu vyvolal model pro daného předplatitele.

Implementace jednotného přihlašování (SSO) s *Azure Active Directory* vypnula, aby byla větší než očekávaná výzva. Důvodem je architektura aplikace s jednou stránkou (SPA). Jedna konkrétní knihovna Azure Active Directory měla klíč pro úspěch: [reagovat-ADAL](https://github.com/salvoravida/react-adal). Následující odkazy vám poskytnou užitečné pokyny pro implementaci ověřování:

- [Scénáře ověřování pro Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)
- [Jedna stránka aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios#single-page-application-spa)

### <a name="development-vm-mls-930"></a>Virtuální počítač pro vývoj (MLS 9.3.0)

Vývoj modelu hostovaného virtuálního počítače, školení a opětovné školení a nasazení modelu klasifikace. Virtuální počítač Azure (DS13 v2) byl zřízený se systémem Linux/Ubuntu 16,10 a následující byly nainstalovány do základního virtuálního počítače:

- Machine Learning Server 9.3.0 podle [pokynů, které](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-install)jsou k dispozici. Ujistěte se, že provedete kroky ověření instalace a ověříte instalaci. Protože se jednalo o vývojový virtuální počítač, oddíl '*Povolit nasazení webové služby a vzdálená připojení*' byl ignorován.
- [Server RStudio](https://www.rstudio.com/products/rstudio/download-server/) (verze Open Source). Dejte pozor, abyste znovu neinstalovali R/r-Base (dříve se nainstalovala s MLS 9.3.0).  
- [Přidejte skupinu zabezpečení sítě](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal) k virtuálnímu počítači, aby bylo možné povolit příchozí připojení přes port 8787 pro RStudio Server.  
- Ovladače ODBC pro zpracování komunikace mezi vývojovým virtuálním počítačem a Azure SQL Database. Na virtuální počítač se nainstalovaly tyto ovladače ODBC:  
- [Ovladač ODBC pro SQL Server 17](https://www.microsoft.com/download/details.aspx?id=56567) kompatibilní se systémem Linux/Ubuntu 16,10  
- Open source ovladač ODBC unixODBC s pokyny k instalaci z tématu [Import relačních dat pomocí rozhraní ODBC](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-data-odbc). Poznámka: v tomto článku najdete dvě překlepy pro Ubuntu pokyny.  
- Chcete-li zjistit, zda je nainstalováno unixODBC: ````Apt list –installed | grep unixODBC (should be unixodbc)````
      - A nainstalujte ovladač: ````sudo apt-get install unixodbc-devel unixodbc-bin unixodbc (should be unixodbc-dev)````

### <a name="operations-vm-mls-930"></a>Provozní virtuální počítač (MLS 9.3.0)

Provozní virtuální počítač hostuje webové služby a koncové body modelu, uložil soubory Swagger a uložené serializované verze modelů klasifikace. Konfigurace je velmi podobná MLS vývojovému serveru. Je ale nakonfigurovaný pro provozuschopnost, což znamená, že jsou nainstalované webové služby potřebné k obsluze koncových bodů REST. K nasazení provozního virtuálního počítače jsou k dispozici šablony ARM, které umožňují nasazení rychle. Viz téma [konfigurace Microsoft Machine Learning Server 9,3 k analýze zprovoznění pomocí šablon ARM](https://blogs.msdn.microsoft.com/mlserver/2018/02/27/configuring-microsoft-machine-learning-server-9-3-to-operationalize-analytics-using-arm-templates/). Pro náš projekt byla pomocí této [šablony ARM](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates/one-box-configuration/linux)nasazena *jedna box* konfigurace.  
V takovém případě součásti serveru pro podporu kanálu modelu byly v provozu.

## <a name="model-implementation"></a>Implementace modelu

Jedno klíčové rozhodnutí ovlivnilo finální návrh modelu pro tento projekt a bylo přesunuto na "velký model", nikoli na jeden model monolitické. Rozdíl: Každý předplatitel má svůj vlastní model klasifikace, nikoli jeden model klasifikace Big, který bude obsluhovat všechny předplatitele. Pro tohoto zákazníka byl přístup preferovaný, protože menší model má menší nároky na paměť a bylo jednodušší ho horizontálně škálovat v produkčním prostředí.

### <a name="data-import"></a>Import dat

Všechna data potřebná pro vývoj modelu se nacházejí v *Azure SQL Database*. Při výuce a školení modelů bylo provedeno import dat ve dvou fázích:

1. Do databáze byl odeslán dotaz, který načte data pro konkrétní *subscriber_id* a vrácenou sadu výsledků. Pro dotaz na přístup k databázi byly zváženy dvě možnosti:

- Funkce RevoScaleR s názvem [RxSQLServerData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqlserverdata)
- Balíček R ODBC

Bylo rozhodnuto použít knihovnu R "ODBC", která povolila filtrování dat na úrovni databáze. Filtrování tabulky databáze pouze pro řádky potřebné pro určitý model odběratele minimalizuje počet řádků, které mají být čteny do jazyka R a zpracovány. Tím se snížila paměť, výpočetní výkon a celková doba potřebná ke školení nebo přeškolování jednotlivých modelů.  

1. Sada výsledků byla převedena na datový rámec R a některé z datových typů byly explicitně převedeny z varchars na celá čísla nebo číselné hodnoty podle požadavků klasifikačních algoritmů. Pro tuto funkci se použila funkce RevoScaleR [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) . Funkce *rxImport* je rozčleněná s RevoScaleR a MicrosoftML a je navržena tak, aby byla Vícevláknová. Tady je příklad, jak jsme ho použili:

````r

# Populate the data frame and modify column types as needed
input_data <- rxImport(sqlServerDataDS, colClasses = c(  
Approved="integer",
      OnTimeArrivalRate="numeric",
      Amount="numeric",
      IsInformed="numeric",
      <continue with list of columns> )
# View the characteristics of the variables in the data source
rxGetInfo (input_data, getVarInfo = TRUE)
````

## <a name="unbalanced-data"></a>Nevyvážená data

Vzhledem k tomu, že cílem modelu doporučení bylo předpovědi pravděpodobnosti, že zákazník obnoví kontrakt a rozdělí ho do kategorie Ano, ne nebo možná, byl použit algoritmus klasifikace. Jedním z problémů, které mohou vážně ovlivnit přesnost a výkon klasifikačního algoritmu, je nevyvážená datová sada.  
Pokud existuje mnoho dalších vzorků pro jednu třídu "Class" než jiné "Class", je datová sada nevyvážená. V takovém případě byl počet řádků dostupných pro každého předplatitele nevyvážený: na horním konci má jeden předplatitel 1 milion + řádků; na konci 330 zákazníci měli méně než 100 řádků dat. Následující graf ukazuje nevyrovnanost s počtem řádků (vzorků) na předplatiteli: ![nevyrovnanosti-Chart](assets/recommendation-engine-optimization/recommendaton_engine_chart.png)

Jedna z postupů pro zpracování nevyvážené datové sady je změnit sadu dat a buď převzorkovat v rámci předreprezentované třídy, nebo v části-Sample třídy, která je nadmnožinou. Další technikou je syntetické generování dalších dat pomocí přesného vědomí dat a jejích atributů, které vlastní vlastník dat. Zákazník vytvořil prahovou hodnotu pro minimální velikost vzorku odběratele. Pro předplatitele pod touto prahovou hodnotou by se musely zacházet s daty. Pro tento projekt se prozkoumaly oba přístupy.

## <a name="algorithm-selection"></a>Výběr algoritmu

Byly vyhodnoceny tři různé implementace algoritmu klasifikace: *rxDForest*, *rxFastTrees*a *rxFastForest*. Všechny tři algoritmy využívají vícevláknové a paralelismuy. A Microsoft ML bude používat víc procesorů nebo GPU, pokud je k dispozici. Kritéria pro vyhodnocení modelů, které jsou součástí:

- Byly nové modely přesnější, že původní model?
- Jaké jsou paměťové nároky na modely běžící v produkčním prostředí? Bez narušení přesnosti by provozní prostředí podporovalo souběžné spouštění mnoha modelů s odezvou předpovědi prakticky v reálném čase?
- Jak dobře měl algoritmus zpracovat nevyváženou datovou sadu? Je nutné, aby nerovnováha byla předem ošetřena generováním syntetických dat?

Níže uvedená tabulka shrnuje závěry:

| Algoritmus | Popis | Zjištění | Poznámky |
| :--------- | :------------ | :--------- | :--------------- |
| [rxFastTrees](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfasttrees) | Paralelní implementace zesíleného rozhodovacího stromu, který implementuje vícevláknovou verzi FastRank. | Přesný a nejrychlejší výkon. | Žádné speciální funkce pro nevyvážená data. Předem zpracovaná data, která je třeba zadat jako vstup |
| [rxFastForest](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfastforest) | Paralelní implementace náhodné doménové struktury a používá rxFastTrees k sestavování seznámení se kompletem rozhodovacích stromů. | Lepší přesnost než původní s předem ošetřenými daty. Méně náročné na paměť, rychlejší než rxDForest |Žádné speciální funkce pro nevyvážená data. Předem ošetřená data zadaná jako vstup. |
| [rxDForest](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdforest) | Paralelní implementace náhodné doménové struktury. Zahrnuté v RevoScaleR. Může se zabývat nevyváženými daty (odebrání chybějících dat, podmínek dat, zpracovávání rozdělení vzorků) všech uvnitř volání funkce. | Rychlejší než původní model. Stejná nebo lepší přesnost než původní model. Zpracovává nevyváženou datovou sadu pomocí různých technik pro převzorkování a syntetizování. Největší nároky na paměť  | Největší nároky na paměť, protože zahrnují data s podmínkou v rámci funkce.   Zpracování dat je dobré, ale není tak dobré jako přizpůsobené transformace poskytnuté vlastníkem dat. |

Na konci zákazník vybral *rxFastForest* algoritmus a rozhodl se zacházet s nevyváženými daty pomocí knihovny [vtreat](https://cran.r-project.org/web/packages/vtreat/index.html) a přidat přizpůsobený krok předběžného zpracování dat do syntetického generování dat pro tyto předplatitele.

## <a name="model-deployment--web-services"></a>Nasazení modelu & webové služby

Publikování modelu pro nasazení na provozním virtuálním počítači je v této dokumentaci pro rychlý Start přímým předáním a popsáno v dokumentaci k [nasazení modelu R jako webové služby s mrsdeploy](https://docs.microsoft.com/machine-learning-server/operationalize/quickstart-publish-r-web-service).
V našem scénáři byly po vytvoření modelů na vývojovém virtuálním počítači tyto modely publikovány na provozním VIRTUÁLNÍm počítači pomocí těchto kroků:

1. Vytvořte vzdálené přihlášení z virtuálního počítače pro vývojáře k provoznímu virtuálnímu počítači pomocí jedné ze dvou funkcí dostupných v balíčku mrsdeploy pro ověřování. remoteLogin () pomocí místního jména správce a hesla nebo remoteLoginAAD () pomocí Azure Active Directory. Obě možnosti jsou popsány v této referenční příručce k Machine Learning Server nebo R Server pomocí mrsdeploy a otevření vzdálené relace.  
2. Jakmile je model vyškolen, publikujte ho na provozním virtuálním počítači pomocí funkcí publishService nebo operace updateservice v balíčku mrsdeploy. Pro tento projekt jsme použili několik přístupů k nasazení a – v závislosti na přístupu – buď byl publikovaný nový model, nebo se aktualizoval existující model. Následující kód byl implementován pro zpracování obou případů:

````r
# If the service does not exist, publish it and if it does exist, update it.  
# No service by this name so publish one
 api <- publishService(serviceName, code =sc_predict, model = model,  
      inputs = list(prop_data="data.frame"),  
      outputs = list(answer = "numeric"), v = "v1.0.0" )
 print("=========== Model Created =============")
} else {
# A service by this name already exists, update it  
 api <- updateService(serviceName, model = model,
     inputs = list(prop_data="data.frame"), v = "v1.0.0" )
 print("=========== Model Updated =============")
}
 ````

Při nasazení jsou modely serializovány a uloženy na provozním serveru a lze je využívat prostřednictvím webových služeb v režimu *Standard* nebo v *reálném čase* . Pokaždé, když je webová služba volána se standardním režimem, R a všechny požadované knihovny jsou načteny a uvolněny při každém volání. Na rozdíl od režimu v *reálném čase* se R a knihovny načítají jenom jednou a znovu se používají pro další volání webové služby. Vzhledem k tomu, že většina režijních nákladů s voláním webové služby je načítání R a knihoven, nabízí režim v reálném čase mnohem nižší latenci pro model bodování a doba odezvy může být v rámci 10ms. V tématu dokumentace [a příklady odkazů si můžete](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) prohlédnout jak pro standard, tak pro možnosti v reálném čase. V reálném čase se sám hodí k jednomu předpovědi, ale můžete také předat vstupní datový rámec pro bodování. To je popsané v tomto odkazu: [asynchronní spotřeba webových služeb prostřednictvím dávkového zpracování pomocí mrsdeploy](https://docs.microsoft.com/machine-learning-server/operationalize/how-to-consume-web-service-asynchronously-batch).

## <a name="conclusion"></a>Závěr

Využití paralelismu knihoven MicrosoftML a RevoScaleR integrovaných do Microsoft Machine Learning Server urychleného vývoje, nasazení a bodování jednotlivých modelů klasifikace pro stovky předplatitelů. Zlepšila se přesnost modelu a byly zkomprimovány školicí a školicí časy – to vše s minimálními změnami stávajícího základu kódu R.
Implementace infrastruktury pro podporu kanálu modelu a získání správně nakonfigurovaných technologických komponent může být komplexní. Tady jsou některé odkazy, které vám pomohou začít s vlastním přístupem:

- [Dokumentace k Machine Learning Server](https://docs.microsoft.com/machine-learning-server/)
- [Kurzy R pro Machine Learning Server](https://docs.microsoft.com/advanced-analytics/tutorials/sql-server-r-tutorials?view=sql-server-2017)
- [Ukázky R pro Machine Learning Server](https://docs.microsoft.com/machine-learning-server/r/r-samples)
- [Referenční dokumentace knihovny funkcí jazyka R](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference)

## <a name="references"></a>Reference

Pokud vás zajímá vytváření dalších prediktivních řešení pro maloobchodní firmu, přejděte do [části maloobchodní prodej](https://gallery.azure.ai/industries/retail) v [galerii Azure AI](https://gallery.azure.ai/).  