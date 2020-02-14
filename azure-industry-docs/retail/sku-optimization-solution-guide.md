---
title: Optimalizace SKU pro zákaznické značky pomocí Azure ML a analýz
author: scseely
ms.author: scseely
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Optimalizace sortimentu maloobchodního odvětví Optimalizace SKU prostřednictvím přehledů z AI a ML.
ms.openlocfilehash: 22411776e830bb3c71f8c1277b30ec4331a3ef17
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054367"
---
# <a name="sku-optimization-for-consumer-brands-solution-guide"></a>Příručka k řešení pro optimalizaci SKU pro zákazníky

## <a name="introduction"></a>Úvod

Maloobchodní prodejci a zákaznické značky se zaměřují na to, že mají správné produkty a služby, které se zákazníci snaží koupit v rámci Marketplace. Je také možné říci, že při pohledu na maximalizaci prodejů, produktů (nebo kombinací produktů) jsou hlavní součástí možností nákupu. Dostupnost nabídek – inventarizace – je konstantním problémem pro zákaznické značky.

Inventář produktů, označovaný také jako sortiment SKU, je složité téma, které pokrývá řetězec dodací a logistické hodnoty. V tomto dokumentu se zaměřujeme konkrétně na problém optimalizace sortimentu SKU, aby se maximalizovaly výnosy z pohledu zákaznického zboží. Prokladová skládanka optimalizace sortimentů SKU se dá vyřešit vyvoláním algoritmů, které odpovídají na následující otázky:

- Které skladové jednotky jsou pro daný trh nebo obchod nejlépe vykonatelné? 
- Které skladové jednotky by se měly přidělit na daný trh nebo uložit na základě jejich výkonu?
- Které skladové jednotky jsou nízké a měly by být nahrazeny vyššími položkami SKU?
- Jaké další přehledy můžeme odvodit o našich segmentech uživatelů a trhu?

## <a name="automate-decision-making"></a>Automatizace rozhodování

Obvykle spotřebitel přináší přístup k poptávce uživatelů zvýšením počtu SKU v portfoliu SKU. Vzhledem k počtu skladových položek v bublinách a konkurenci se zvýšila, odhaduje se 90 procent výnosů jenom na 10 procent SKU produktů v rámci portfolia. 80 procento výnosů obvykle vyplyne z 20 procent SKU. A tento poměr je kandidátem na zlepšení ziskovosti. 

Tradiční metody statických sestav využívají historické údaje, které omezují přehledy. V tomto případě se rozhodnutí stále provádějí ručně a implementují. To znamená, že lidský zásah a čas zpracování. S využitím špičkových inteligentních funkcí (AI) a cloud computingu je možné využít pokročilou analýzu k poskytnutí rozsahu možností a předpovědi. Tento druh automatizace zlepšuje výsledky a rychlost pro zákazníky.

## <a name="sku-assortment-optimization"></a>Optimalizace sortimentu SKU

Řešení sortimentu SKU musí zpracovávat miliony SKU tím, že segmentuje prodejní data do smysluplného a podrobného porovnání. Cílem tohoto řešení je maximalizovat prodej na každém výstupu nebo v obchodě vyladěním sortimentu produktů pomocí pokročilých analýz. Druhým cílem je eliminovat nedostatek zásob a zlepšit sortimenty. Fiskální cíl je zvýšení prodeje o 5 až 10 procent. K tomuto účelu vám Insights umožní:

- Pochopení výkonu portfolia SKU a Správa nízkých výkonů.
- Optimalizuje distribuci SKU, aby se snížily množství akcií.
- Pochopte, jak nové SKU podporují krátkodobé a dlouhodobé strategie.
- Vytvářejte opakující se, škálovatelné a užitečné poznatky z existujících dat.

## <a name="descriptive-analytics"></a>Popisné analýzy

Popisné modely agregují datové body a prozkoumávat vztahy mezi faktory, které mohou ovlivnit prodej produktu. Informace mohou být rozšířeny s některými externími datovými body, jako je umístění, počasí, sčítání dat atd. Vizualizace usnadňují osobě odvodit přehledy tím, že interpretují data. V takovém případě je však osoba omezená na to, co se stalo nad předchozím prodejním cyklem, případně s tím, co se děje v aktuálním období (v závislosti na tom, jak často se data aktualizují).

V tomto případě je v tomto případě k dispozici tradiční přístup k datovým skladům a vytváření sestav, abyste pochopili, jaké skladové jednotky byly nejlepší a nejhorším způsobem provedeným v časovém intervalu.

Následující obrázek ukazuje typickou sestavu historických dat – Sales data. Obsahuje několik bloků s zaškrtávacími políčky pro výběr kritérií pro filtrování výsledků. V centru se zobrazují dva pruhové grafy, které ukazují prodeje v průběhu času. První graf znázorňuje průměrné tržby za týden; druhý zobrazuje množství po týdnech.

 ![Dashbord Příklad znázorňující historické údaje o prodeji.](assets/sku-optimization-solution-guide/sku-max-model.png)
  
## <a name="predictive-analytics"></a>Prediktivní analýza

Historické sestavy se hodí k tomu, co se stalo. Nakonec chceme předpověď toho, co se může stát. K tomuto účelu mohou být užitečné informace v minulosti. Můžete například identifikovat sezónní trendy. Ale nemůže pokrývat scénáře "Co je", například k modelování zavedení nového produktu. Abychom to mohli udělat, musíme tento fokus posunout na chování modelování zákazníka, protože to je konečný faktor, který určuje prodej.

### <a name="an-in-depth-look-at-the-problem-choice-models"></a>Podrobný pohled na problém: modely voleb

Pojďme začít tím, že definujeme, co hledáte a jaká data máme:

Optimalizace sortimentů znamená nalezení podmnožiny produktů, které se mají převést na prodej, který maximalizuje očekávané tržby. To je to, co hledáte.

**Data transakce** jsou pravidelně shromažďována pro finanční účely. 

**Data sortimentu** zahrnují potenciálně všechno, co se týká SKU: tady je příklad toho, co chceme: 

- počet SKU
- Popisy SKU
- přidělená množství
- Zakoupené SKU a množství
- časová razítka událostí (například nákup)
- Cena SKU
- Cena za SKU na POS
- úroveň akcie všech SKU v jakémkoli okamžiku

Tato data bohužel nejsou shromažďována jako spolehlivá jako transakční data. 

V tomto dokumentu se pro jednoduchost považuje jenom data transakcí a data SKU, ne externí faktory.

I tak si všimněte, že pro sadu n produktů jsou dostupné 2nelné sortimenty. Díky tomu je optimalizace problematická výpočetně náročný proces. Vyhodnocení všech možných kombinací je pro velký počet produktů nepraktické. Proto jsou sortimenty rozděleny podle kategorií (například obilovin), umístění a dalších kritérií pro omezení počtu proměnných. Optimalizační modely se pokusí "vyřadit" počet permutací pro funkční podmnožinu.

Crux problému spočívají v tom, že se efektivně **namodelovací chování spotřebitelů** . V dokonalém světě se jim uvedené produkty shodují s tím, které chtějí koupit.

Matematické modely pro předpověď výběru spotřebitelů byly vyvinuty v průběhu desetiletí. Volba modelu nakonec určí nejvhodnější technologii implementace; Proto je shrnujeme a nabídne vám několik důležitých informací.

## <a name="parametric-models"></a>Modely ukazatelů

Modely parametrů se blíží chování zákazníků pomocí funkce s konečnou sadou parametrů. Odhadneme sadu parametrů tak, aby co nejlépe vyhovovala vašemu vyřazení dat. Jednou z nejstarších a nejlepších známek je [MULTINOMIAL – Logistická regrese](https://en.wikipedia.org/wiki/Multinomial_logistic_regression) (neboli MNL, multi-Class logit nebo softmax regrese). Slouží k výpočtu pravděpodobnosti několika možných výsledků při potížích s klasifikací. V takovém případě můžete použít MNL k výpočtu:

- Pravděpodobnost, že příjemce (c) zvolí položku (i) v určitém čase (t) s ohledem na sadu položek této kategorie v sortimentu (a) se známým nástrojem pro zákazníka (v). 

Předpokládáme také, že nástroj položky může být funkcí jeho funkcí. Externí informace mohou být také součástí měření nástroje (například zastřešující je užitečnější, pokud se jedná o své přístavení).

Často používáme MNL jako srovnávací testy pro jiné modely z důvodu jejich odvolání ve odhadu parametrů a vyhodnocení výsledků. Jinými slovy, pokud je horší než MNL, váš algoritmus není používán.
Několik modelů bylo odvozeno od MNL, ale je nad rámec tohoto dokumentu, aby je bylo možné diskutovat. 

K dispozici jsou knihovny pro programovací jazyky R a Python. Pro R můžete použít GLM (a deriváty).  Pro Python jsou k dispozici [scikit-učení](http://scikit-learn.org/stable/), [biogeme](http://biogeme.epfl.ch/)a [Larch](https://pypi.org/project/larch/). Tyto knihovny nabízejí nástroje pro určení problémů MNL a paralelních řešitelů pro hledání řešení na různých platformách.

V poslední době byla implementace MNL modelů na GPU navržena pro výpočty složitých modelů s řadou parametrů, které by mohly být nerušivé jinak.

Neuronové sítě s výstupní vrstvou softmax se efektivně používaly na velkých problémech s více třídami. Tyto sítě vytvoří vektor výstupů, který představuje rozdělení pravděpodobnosti v řadě různých výsledků. V porovnání s jinými implementacemi jsou pomaleji, ale mohou zpracovat velký počet tříd a parametrů. 

## <a name="non-parametric-models"></a>Modely bez parametry

Bez ohledu na oblíbenou velikost MNL ukládá některé významné předpoklady k lidskému chování, které může omezit jeho užitečnost. Konkrétně předpokládá, že relativní pravděpodobnost, kterou uživatel zvolí mezi dvěma možnostmi, je nezávislá na dalších alternativách zavedených v sadě později. Ve většině případů je to nepraktické.

Například pokud chcete, aby produkt "A" a produkt "B" byl stejně, pak si vyberete jednu z dalších 50% času. Pojďme do směsi zavádět produkt "C". Stále můžete zvolit "A" 50% času, ale nyní rozdělíte preference 25% na "B" a 25% na "C". Relativní pravděpodobnost se změnila.

MNL a deriváty nemají jednoduchý způsob, jak si vyhodnotit substituce z důvodu odrůdy na skladě nebo v sortimentu (to znamená, pokud nemáte jasný nápad a nemusíte vybírat náhodnou položku mezi položkami v police).
Modely bez parametrů jsou navržené tak, aby se přihlédly k náhradě a neobsahovaly méně omezení na chování zákazníků. 

Představují koncept "hodnocení", kde spotřebitelé vyjadřují striktní předvolby pro produkty v sortimentu. Jejich chování při nákupu je proto možné modelovat seřazením produktů v sestupném pořadí podle preference. 

Problém s optimalizací sortimentů můžete vyjádřit jako maximální množství výnosů:

<center><img style="float: center;" src="assets/sku-optimization-solution-guide/assortment-optimization-problem.png" width="150"/></center>
 
- $r _i $ označuje tržby produktu i. 
- $y _i ^ k $ je 1, pokud je vybraný produkt v hodnocení k, 0 jinak.  
- $ \ lambda_k $ je pravděpodobnost, že zákazník zvolí podle hodnocení k
- $x _i $ je 1, pokud je produkt zahrnutý v sortimentu, 0 jinak
- K je počet pořadí 
- n je počet produktů

v závislosti na omezeních:

- pro každé hodnocení může být přesně 1 volba.
- v rámci hodnocení k může být produkt vybraný jenom v případě, že je součástí sortimentu.
- Pokud je produkt zahrnutý v sortimentu, nedá se zvolit žádná z méně preferovaných možností v hodnocení k. 
- možnost bez nákupu je možnost, takže se dá zvolit žádná z méně upřednostňovaných možností v rámci hodnocení.

V takovém složení se problém dá považovat za optimalizaci na základě [smíšeného rozsahu](https://en.wikipedia.org/wiki/Integer_programming).

Vezměte v úvahu, že pokud existují n produktů, maximální počet možných hodnocení, včetně možnosti bez výběru, je faktoriál: (n + 1)!

I když omezení v rámci formulace umožňují relativně efektivní vyřazení možných možností (pro instanci je zvolená jenom nejvýhodnější možnost a je nastavená na 1, zbytek je nastavená na 0), můžete si představit, že škálovatelnost implementace bude důležité, s ohledem na počet možných alternativ.

### <a name="the-importance-of-data"></a>Důležitost dat

Dříve jsme uvedli, že data o prodeji budou snadno dostupná. Chceme je použít k informování našeho modelu optimalizace sortimentů. Konkrétně chceme najít λ rozdělení pravděpodobnosti.

Údaje o prodeji z bodu prodeje budou tvořeny transakcemi s časovými razítky a sadou produktů zobrazeným zákazníkům v daném čase a umístění. Z těchto hodnot můžeme vytvořit vektor skutečných prodejů, jejichž prvky VI, m označují pravděpodobnost, že prodávající položky i pro zákazníka přestanou být v rámci sortimentu $S _m $

Také můžeme vytvořit matici:

<center><img style="float: center;" src="assets/sku-optimization-solution-guide/matrix-construction.png" width="300"/></center>

Hledání naší pravděpodobnosti rozdělení pravděpodobnosti λ, že naše prodejní data se staly dalším problémem s optimalizací. Chceme najít λ vektoru pro minimalizaci chyby odhadu prodeje:

$ $min _ \ lambda | \Lambda\lambda-v | $ $

Počítejte s tím, že výpočet se dá také vyjádřit jako regrese a jako takový se dají použít modely, jako jsou variate rozhodovací stromy. 

## <a name="implementation-details"></a>Podrobnosti implementace

Jak můžeme odečíst od výše uvedeného složení, optimalizační modely jsou založené na datech i náročné na výpočetní výkon.

Partneři Microsoftu, jako je Neal Analytics, vyvinuly robustní architektury, které tyto podmínky splní. Viz [položka Max](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview?WT.mc_id=invopt-article-gmarchet). Tyto architektury použijeme jako příklad a nabídne vám několik důležitých informací.

- Za prvé se spoléhají na (1) robustní a Škálovatelný datový kanál, který zakládá modelům, a (2) robustní a škálovatelnou infrastrukturu spouštění pro jejich spouštění.
- Za druhé jsou výsledky snadno použitelné pomocí plánovače prostřednictvím řídicího panelu.

Obrázek 2 ukazuje příklad architektury. Obsahuje čtyři hlavní bloky, zachycení, zpracování, model a zprovoznění. Každý blok obsahuje hlavní procesy. Capture zahrnuje "předzpracování dat"; proces zahrnuje funkci "úložiště dat"; model obsahuje funkci "model strojového učení"; a zprovoznění obsahují "uložená data" a možnosti vytváření sestav (například řídicích panelů).

Architektura ![ve čtyřech částech: zachycení, zpracování, model a zprovoznění. ](assets/sku-optimization-solution-guide/architecture-sku-optimization.png)<center> <font size="1"> _Obrázek 2: architektura pro optimalizaci SKU – zdvořilostní analýza Neal_</font></center>

## <a name="the-data-pipeline"></a>Datový kanál

Architektura zdůrazňuje důležitost vytvoření datového kanálu pro školení a provoz modelu. Organizace organizuje aktivity v kanálu pomocí [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction?WT.mc_id=invopt-article-gmarchet), spravované služby extrakce, transformace a načítání (ETL), která vám umožní navrhnout a spustit vaše pracovní postupy pro integraci.

Azure Data Factory je spravovaná služba se součástmi nazvanými "aktivity", které využívají nebo vytváří datové sady.

Aktivity lze rozdělit do:

- přesun dat (například kopírování ze zdroje do cíle)
- transformace dat (například agregace s dotazem SQL nebo spuštění uložené procedury)

Pracovní postupy, které odkazují na společné sady aktivit, se dají naplánovat, monitorovat a spravovat pomocí služby Data Factory. Úplný pracovní postup se označuje jako kanál.

Ve fázi zachycení můžeme využít aktivity kopírování (integrované do Data Factory) k přenosu dat z nejrůznějších zdrojů (místních i cloudových) do Azure SQL Data Warehouse. Příklady toho, jak postupovat, najdete v dokumentaci:

- [Kopírování dat z Azure SQL DW nebo z něj](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse?WT.mc_id=invopt-article-gmarchet)
- [Načtení dat do Azure SQL DW](https://docs.microsoft.com/azure/data-factory/load-azure-sql-data-warehouse?WT.mc_id=invopt-article-gmarchet)

Následující obrázek ukazuje definici kanálu. Skládá se ze tří stejných tří bloků na řádku. První dvě jsou datová sada a aktivita připojená pomocí šipek k označení toků dat. Třetí blok je označený jako "kanál" a jednoduše odkazuje na první dvě, aby označovaly zapouzdření. 

 ![Azure Data Factory koncepty: datové sady spotřebované pomocí kanálu aktivit. ](assets/sku-optimization-solution-guide/azure-data-factory.png)<center> <font size="1"> _Obrázek 3: základní koncepty Azure Data Factory_</font></center>

Příklad formátu dat, který používá řešení Neal Analytics, najdete na stránce Appsource Microsoftu. Řešení zahrnuje následující datové sady:

- Údaje o historii prodeje pro každou kombinaci obchodu a SKU
- Záznamy ze Storu a příjemců
- Kódy a popis SKU
- Atributy SKU, které zachycují funkce produktů (například velikost, materiál). Ty se obvykle používají v modelech parametry k rozlišení mezi variantami produktu.

Pokud zdroje dat nejsou vyjádřeny v konkrétním formátu, Data Factory nabízí řadu transformačních aktivit. 

Ve fázi procesu je SQL Data Warehouse hlavní modul úložiště. Proto můžete chtít vyjádřit takovou transformační aktivitu jako uloženou proceduru SQL, kterou lze automaticky vyvolat jako součást kanálu. Dokumentace poskytuje podrobné pokyny:

- [Transformace dat pomocí uložené procedury SQL](https://docs.microsoft.com/azure/data-factory/transform-data-using-stored-procedure?WT.mc_id=invopt-article-gmarchet)

Všimněte si, že Data Factory neomezuje na SQL Data Warehouse a uložené procedury SQL. Ve skutečnosti se integruje s různými platformami. Můžete si například zvolit, že chcete použít datacihly a spustit skript Pythonu místo pro transformaci. To je výhodou, protože můžete použít jednu platformu pro ukládání, transformaci a školení algoritmů strojového učení v následující fázi modelu.

## <a name="training-the-ml-algorithm"></a>Školení algoritmu ML

K dispozici je několik nástrojů, které vám pomohou s implementací modelů ukazatelů a bez ukazatelů. Vaše volba závisí na požadavcích na škálovatelnost a výkon.

[Azure ml Studio](https://studio.azureml.net/?WT.mc_id=invopt-article-gmarchet) je skvělým nástrojem pro vytváření prototypů. Poskytuje snadný způsob, jak vytvořit a spustit školicí pracovní postup s moduly kódu (v R nebo Pythonu) nebo s předem definovanými komponentami ML (například klasifikátory s více třídami, zesílení regrese rozhodovacího stromu) v grafickém prostředí. Umožňuje také publikovat vyškolený model jako webovou službu pro další spotřebu několika kliknutími a vygenerováním rozhraní REST pro vás. 

Velikost dat, kterou může zpracovat, je ale omezená na 10 GB (pro teď) a počet jader dostupných pro jednotlivé komponenty je taky omezený na 2 (pro teď).

Následující obrázek ukazuje příklad používaného nástroje ML Studio. Je to grafické znázornění experimentu strojového učení. Obrázek ukazuje několik skupin bloků. Každá sada bloků představuje některé fáze v experimentu a každý blok je připojen k jednomu nebo více bloků k označení vstupu a výstupu dat.

![příklad použití pro Machine Learning Studio. ](assets/sku-optimization-solution-guide/ml-training-pipeline-example.png)<center> <font size="1"> _Obrázek 4: příklad školicího kanálu ml s R a předem sestavenými komponentami_</font></center>

Pokud potřebujete další škálování, ale přesto chcete používat některé z rychlých a paralelních implementací běžných algoritmů strojového učení od Microsoftu (například MULTINOMIAL logistické regrese), můžete se podívat na Microsoft ML Server běžící na virtuálním počítači Azure pro datové vědy. Počítačové.

Pro velké objemy dat (TBs) dává smysl zvolit platformu, kde může úložiště a výpočetní element:

- Škálujte nezávisle, abyste omezili náklady, když modely nebudete poučením.
- Rozdělte výpočet napříč více jádry.
- Spusťte výpočet "Zavřít" do úložiště pro omezení přesunu dat.

Služby Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/?WT.mc_id=invopt-article-gmarchet) a [datacihly](https://azure.microsoft.com/services/databricks/?WT.mc_id=invopt-article-gmarchet) obě tyto požadavky splňují. Kromě toho jsou obě platformy pro spouštění podporované v editoru Azure Data Factory. Integraci jednoho pracovního postupu je relativně jednoduché.

ML Server a jeho knihovny se dají nasadit na HDInsight, ale pokud plně využili výhod platforem, možná budete chtít implementovat svůj algoritmus ML podle vlastního výběru pomocí SparkML, knihoven Microsoft ML Spark v Pythonu nebo jiného specializovaného trendu. programování řešitelů, jako jsou TFoCS, Spark-LP nebo SolveDF. 

Spuštění procesu školení se pak bude dotazem na vyvolání příslušného skriptu nebo poznámkového bloku pySpark z pracovního postupu Data Factory. To je plně podporované v grafickém editoru. Další podrobnosti najdete [v tématu spuštění poznámkového bloku datacihly s aktivitou poznámkového bloku datacihly v Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook?WT.mc_id=invopt-article-gmarchet).

Následující obrázek ukazuje Data Factory uživatelské rozhraní, které je k dispozici prostřednictvím Azure Portal. Obsahuje bloky pro různé procesy v pracovním postupu. 

![Data Factory rozhraní ukazující aktivitu poznámkového bloku datacihly. ](assets/sku-optimization-solution-guide/data-factory-pipeline-databricks.png)<center> <font size="1"> _Obrázek 5: příklad kanálu Data Factory s aktivitou poznámkového bloku datacihly_</font></center>

Všimněte si také, že v našem [řešení pro optimalizaci inventáře](https://gallery.azure.ai/Solution/Inventory-Optimization-3?WT.mc_id=invopt-article-gmarchet) navrhneme implementaci řešitelů založených na kontejnerech, které jsou škálované prostřednictvím [Azure Batch](https://azure.microsoft.com/services/batch/?WT.mc_id=invopt-article-gmarchet). Knihovny pro optimalizaci specializované na to, jako je [pyomo](http://www.pyomo.org/about/) , umožňují vyjádřit problém s optimalizací v programovacím jazyce Python a pak vyvolat nezávislé řešitele, jako je [bonmin](https://projects.coin-or.org/Bonmin) (Open Source) nebo [Gurobi](http://www.gurobi.com/) (komerční) k vyhledání řešení.

Dokumentace k optimalizaci inventáře se zabývá různými problémy (množstvími objednávek) než s optimalizací sortimentů, ale v Azure se obdobně hodí implementace řešitelů v Azure.

I když je to složitější než u těch, které jsme navrhli, tato technika umožňuje maximální škálovatelnost, která je omezená hlavně o množství jader, které můžete dovolit.

## <a name="running-the-model-operationalize"></a>Spuštění modelu (zprovoznění)

Jakmile je model vyškolený, jeho spuštění obvykle vyžaduje jinou infrastrukturu než tu, která se používá pro nasazení. Aby bylo možné ho snadno využít, můžete se rozhodnout ho nasadit jako webovou službu s rozhraním REST. Jak Azure ML Studio, tak ML Server automatizaci procesu vytváření takových služeb. V případě ML Server poskytujeme šablony pro nasazení podpůrné infrastruktury. Podívejte se prosím na příslušnou [dokumentaci](https://docs.microsoft.com/machine-learning-server/what-is-operationalization?WT.mc_id=invopt-article-gmarchet).

Následující obrázek ukazuje architekturu nasazení. Obsahuje reprezentace serverů, na kterých běží jazyk R, a Python. Oba servery komunikují v dílčí části webových uzlů, které provádějí výpočty. K bloku výpočtů je připojeno velké úložiště dat.

Diagram nasazení serveru ![ML Nástroj pro vyrovnávání zatížení před několika uzly ke spuštění. ](assets/sku-optimization-solution-guide/ml-server-deployment-example.png)<center> <font size="1"> _Obrázek 6: příklad nasazení serveru ml_</font></center>


Pro modely vytvořené v HDInsight nebo datacihly, které jsou závislé na prostředí Spark (knihovny, paralelní možnosti atd.), je možné, že byste je měli zvážit při jejich spuštění v clusteru. [Tady najdete](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/spark-model-consumption?WT.mc_id=invopt-article-gmarchet)doprovodné materiály.

To má výhodu, že operační model může být sám vyvolán prostřednictvím aktivity Data Factory kanálu pro vyhodnocování.

Chcete-li použít kontejnery, můžete zabalit modely a nasadit je do služby Azure Kubernetes. Prototyp bude vyžadovat použití [Azure Data Science VM](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/?WT.mc_id=invopt-article-gmarchet); na virtuálním počítači musíte taky nainstalovat nástroje [příkazového řádku](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-service-deploy?WT.mc_id=invopt-article-gmarchet) Azure ml.

## <a name="data-output-and-reporting"></a>Výstup dat a generování sestav

Po nasazení bude model moci zpracovat workflowy finančních transakcí a čtení zásob za účelem generování optimálního sortimentu předpovědi. Takto vytvořená data je možné uložit zpět do Azure SQL Data Warehouse k další analýze. Konkrétně bude možné zkoumat historický výkon různých SKU a identifikovat nejlepší zdroje příjmů a jejich tvůrce. Pak je budete moct porovnat s sortimenty navrženými modely a vyhodnocovat výkon a potřebovat znovu školení.

[PowerBI](https://powerbi.microsoft.com/get-started/?&OCID=AID719832_SEM_uhlWLg3x&lnkd=Google_PowerBI_Brand&gclid=CjwKCAjw5ZPcBRBkEiwA-avvkyOLMJCrhqH8iac84aLX7EcUQIirSSqUCostzGi8y_XntJTCD73ZixoCQ4sQAvD_BwE?WT.mc_id=invopt-article-gmarchet) nabízí způsob, jak analyzovat a zobrazovat data vytvořená v procesu. 

Následující obrázek ukazuje typický řídicí panel Power BI. Obsahuje dva grafy, které zobrazují informace o skladových položkách SKU. 

![příklad řídicího panelu, který zobrazuje výsledky po dobu 12 měsíců. ](assets/sku-optimization-solution-guide/sku-max-model.png)<center> <font size="1"> _Obrázek 7: příklad sestavy výsledků modelu – zdvořilostní analýza Neal_</font></center>

## <a name="security-considerations"></a>Aspekty zabezpečení

Řešení, které se zabývá citlivými daty, obsahuje finanční záznamy, úrovně akcií a informace o cenách. Tato citlivá data musí být chráněná. Aby allay obavy týkající se zabezpečení a ochrany soukromí dat, pamatujte na to, že:

- Můžete spustit některé z Azure Data Factoryho kanálu místně pomocí Azure Integration Runtime. Modul runtime spouští aktivity přesunu dat do a ze zdrojů místně. Bude také odesílat aktivity pro spuštění místně.
  - Konkrétně můžete chtít vyvinout vlastní aktivitu, která anonymizovat data, která se mají přenést do Azure (a spustit místně).
- Všechny zmíněné služby podporují šifrování při přenosu a v klidovém provozu. Pokud se rozhodnete ukládat data na Azure Data Lake, bude ve výchozím nastavení povolené šifrování. Pokud používáte Azure SQL Data Warehouse, můžete povolit transparentní šifrování dat (TDE).
- Všechny zmíněné služby, s výjimkou nástroje ML Studio, podporují integraci s Azure Active Directory pro ověřování a autorizaci. Pokud píšete vlastní kód, musíte tuto integraci sestavit do své aplikace.

Další informace o GDPR najdete na naší stránce věnované [dodržování předpisů](https://www.microsoft.com/trustcenter?WT.mc_id=invopt-article-gmarchet) .

## <a name="technologies-mentioned"></a>Uvedené technologie

- [Azure Batch](https://azure.microsoft.com/services/batch/?WT.mc_id=invopt-article-gmarchet)
- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/?&OCID=AID719825_SEM_w1MNAVjn&lnkd=Google_Azure_Brand&gclid=CjwKCAjw5ZPcBRBkEiwA-avvk4bGtyQo11KBY-u2skor1SydsSl1vrYUmhyGhhwyJhDlAYpnMmIcRRoCTfsQAvD_BwE&dclid=CMn6lvfRkd0CFRwBrQYdtIoJOA?WT.mc_id=invopt-article-gmarchet)
- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction?WT.mc_id=invopt-article-gmarchet)
- [Azure Integration Runtime](https://docs.microsoft.com/azure/data-factory/concepts-integration-runtime?WT.mc_id=invopt-article-gmarchet)
- [HDInsight](https://azure.microsoft.com/services/hdinsight/?WT.mc_id=invopt-article-gmarchet)
- [Databricks](https://azure.microsoft.com/services/databricks/?WT.mc_id=invopt-article-gmarchet)
- [Azure SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is?WT.mc_id=invopt-article-gmarchet)
- [ML Studio Azure](https://studio.azureml.net/?WT.mc_id=invopt-article-gmarchet)
- [ML Server Microsoftu](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server?WT.mc_id=invopt-article-gmarchet)
- [Data Science VM Azure](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/?WT.mc_id=invopt-article-gmarchet)
- [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/?WT.mc_id=invopt-article-gmarchet)
- [Microsoft PowerBI](https://powerbi.microsoft.com/?WT.mc_id=invopt-article-gmarchet)
- [Jazyk modelování optimalizace Pyomo](http://www.pyomo.org/)
- [Řešitel Bonmin](https://projects.coin-or.org/Bonmin)
- [TFoCS Řešitel pro Spark](https://github.com/databricks/spark-tfocs)
