
#Importing All Required Libaries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from warnings import filterwarnings
filterwarnings(action='ignore')
Matplotlib is building the font cache; this may take a moment.
#Loading Datasets
pd.set_option('display.max_columns',10,'display.width',1000)
train = pd.read_csv('train.csv')
test = pd.read_csv('test.csv')
train.head()
PassengerId	Survived	Pclass	Name	Sex	...	Parch	Ticket	Fare	Cabin	Embarked
0	1	0	3	Braund, Mr. Owen Harris	male	...	0	A/5 21171	7.2500	NaN	S
1	2	1	1	Cumings, Mrs. John Bradley (Florence Briggs Th...	female	...	0	PC 17599	71.2833	C85	C
2	3	1	3	Heikkinen, Miss. Laina	female	...	0	STON/O2. 3101282	7.9250	NaN	S
3	4	1	1	Futrelle, Mrs. Jacques Heath (Lily May Peel)	female	...	0	113803	53.1000	C123	S
4	5	0	3	Allen, Mr. William Henry	male	...	0	373450	8.0500	NaN	S
5 rows × 12 columns

#Display shape
train.shape
(891, 12)
test.shape
(418, 11)
#Checking for Null values
train.isnull().sum()
PassengerId      0
Survived         0
Pclass           0
Name             0
Sex              0
Age            177
SibSp            0
Parch            0
Ticket           0
Fare             0
Cabin          687
Embarked         2
dtype: int64
test.isnull().sum()
PassengerId      0
Pclass           0
Name             0
Sex              0
Age             86
SibSp            0
Parch            0
Ticket           0
Fare             1
Cabin          327
Embarked         0
dtype: int64
#Description of dataset
train.describe(include="all")
PassengerId	Survived	Pclass	Name	Sex	...	Parch	Ticket	Fare	Cabin	Embarked
count	891.000000	891.000000	891.000000	891	891	...	891.000000	891	891.000000	204	889
unique	NaN	NaN	NaN	891	2	...	NaN	681	NaN	147	3
top	NaN	NaN	NaN	Braund, Mr. Owen Harris	male	...	NaN	347082	NaN	B96 B98	S
freq	NaN	NaN	NaN	1	577	...	NaN	7	NaN	4	644
mean	446.000000	0.383838	2.308642	NaN	NaN	...	0.381594	NaN	32.204208	NaN	NaN
std	257.353842	0.486592	0.836071	NaN	NaN	...	0.806057	NaN	49.693429	NaN	NaN
min	1.000000	0.000000	1.000000	NaN	NaN	...	0.000000	NaN	0.000000	NaN	NaN
25%	223.500000	0.000000	2.000000	NaN	NaN	...	0.000000	NaN	7.910400	NaN	NaN
50%	446.000000	0.000000	3.000000	NaN	NaN	...	0.000000	NaN	14.454200	NaN	NaN
75%	668.500000	1.000000	3.000000	NaN	NaN	...	0.000000	NaN	31.000000	NaN	NaN
max	891.000000	1.000000	3.000000	NaN	NaN	...	6.000000	NaN	512.329200	NaN	NaN
11 rows × 12 columns

train.groupby('Survived').mean()
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
File ~\python\Lib\site-packages\pandas\core\groupby\groupby.py:1874, in GroupBy._agg_py_fallback(self, how, values, ndim, alt)
   1873 try:
-> 1874     res_values = self.grouper.agg_series(ser, alt, preserve_dtype=True)
   1875 except Exception as err:

File ~\python\Lib\site-packages\pandas\core\groupby\ops.py:849, in BaseGrouper.agg_series(self, obj, func, preserve_dtype)
    847     preserve_dtype = True
--> 849 result = self._aggregate_series_pure_python(obj, func)
    851 if len(obj) == 0 and len(result) == 0 and isinstance(obj.dtype, ExtensionDtype):

File ~\python\Lib\site-packages\pandas\core\groupby\ops.py:877, in BaseGrouper._aggregate_series_pure_python(self, obj, func)
    876 for i, group in enumerate(splitter):
--> 877     res = func(group)
    878     res = extract_result(res)

File ~\python\Lib\site-packages\pandas\core\groupby\groupby.py:2380, in GroupBy.mean.<locals>.<lambda>(x)
   2377 else:
   2378     result = self._cython_agg_general(
   2379         "mean",
-> 2380         alt=lambda x: Series(x).mean(numeric_only=numeric_only),
   2381         numeric_only=numeric_only,
   2382     )
   2383     return result.__finalize__(self.obj, method="groupby")

File ~\python\Lib\site-packages\pandas\core\series.py:6225, in Series.mean(self, axis, skipna, numeric_only, **kwargs)
   6217 @doc(make_doc("mean", ndim=1))
   6218 def mean(
   6219     self,
   (...)
   6223     **kwargs,
   6224 ):
-> 6225     return NDFrame.mean(self, axis, skipna, numeric_only, **kwargs)

File ~\python\Lib\site-packages\pandas\core\generic.py:11992, in NDFrame.mean(self, axis, skipna, numeric_only, **kwargs)
  11985 def mean(
  11986     self,
  11987     axis: Axis | None = 0,
   (...)
  11990     **kwargs,
  11991 ) -> Series | float:
> 11992     return self._stat_function(
  11993         "mean", nanops.nanmean, axis, skipna, numeric_only, **kwargs
  11994     )

File ~\python\Lib\site-packages\pandas\core\generic.py:11949, in NDFrame._stat_function(self, name, func, axis, skipna, numeric_only, **kwargs)
  11947 validate_bool_kwarg(skipna, "skipna", none_allowed=False)
> 11949 return self._reduce(
  11950     func, name=name, axis=axis, skipna=skipna, numeric_only=numeric_only
  11951 )

File ~\python\Lib\site-packages\pandas\core\series.py:6133, in Series._reduce(self, op, name, axis, skipna, numeric_only, filter_type, **kwds)
   6129     raise TypeError(
   6130         f"Series.{name} does not allow {kwd_name}={numeric_only} "
   6131         "with non-numeric dtypes."
   6132     )
-> 6133 return op(delegate, skipna=skipna, **kwds)

File ~\python\Lib\site-packages\pandas\core\nanops.py:147, in bottleneck_switch.__call__.<locals>.f(values, axis, skipna, **kwds)
    146 else:
--> 147     result = alt(values, axis=axis, skipna=skipna, **kwds)
    149 return result

File ~\python\Lib\site-packages\pandas\core\nanops.py:404, in _datetimelike_compat.<locals>.new_func(values, axis, skipna, mask, **kwargs)
    402     mask = isna(values)
--> 404 result = func(values, axis=axis, skipna=skipna, mask=mask, **kwargs)
    406 if datetimelike:

File ~\python\Lib\site-packages\pandas\core\nanops.py:720, in nanmean(values, axis, skipna, mask)
    719 the_sum = values.sum(axis, dtype=dtype_sum)
--> 720 the_sum = _ensure_numeric(the_sum)
    722 if axis is not None and getattr(the_sum, "ndim", False):

File ~\python\Lib\site-packages\pandas\core\nanops.py:1693, in _ensure_numeric(x)
   1691 if isinstance(x, str):
   1692     # GH#44008, GH#36703 avoid casting e.g. strings to numeric
-> 1693     raise TypeError(f"Could not convert string '{x}' to numeric")
   1694 try:

TypeError: Could not convert string 'Braund, Mr. Owen HarrisAllen, Mr. William HenryMoran, Mr. JamesMcCarthy, Mr. Timothy JPalsson, Master. Gosta LeonardSaundercock, Mr. William HenryAndersson, Mr. Anders JohanVestrom, Miss. Hulda Amanda AdolfinaRice, Master. EugeneVander Planke, Mrs. Julius (Emelia Maria Vandemoortele)Fynney, Mr. Joseph JPalsson, Miss. Torborg DaniraEmir, Mr. Farred ChehabFortune, Mr. Charles AlexanderTodoroff, Mr. LalioUruchurtu, Don. Manuel EWheadon, Mr. Edward HMeyer, Mr. Edgar JosephHolverson, Mr. Alexander OskarCann, Mr. Ernest CharlesVander Planke, Miss. Augusta MariaAhlin, Mrs. Johan (Johanna Persdotter Larsson)Turpin, Mrs. William John Robert (Dorothy Ann Wonnacott)Kraeff, Mr. TheodorRogers, Mr. William JohnLennon, Mr. DenisSamaan, Mr. YoussefArnold-Franchi, Mrs. Josef (Josefine Franchi)Panula, Master. Juha NiiloNosworthy, Mr. Richard CaterOstby, Mr. Engelhart CorneliusNovel, Mr. MansouerGoodwin, Master. William FrederickSirayanian, Mr. OrsenHarris, Mr. Henry BirkhardtSkoog, Master. HaraldStewart, Mr. Albert ACrease, Mr. Ernest JamesKink, Mr. VincenzJenkin, Mr. Stephen CurnowGoodwin, Miss. Lillian AmyHood, Mr. Ambrose JrChronopoulos, Mr. ApostolosMoen, Mr. Sigurd HansenStaneff, Mr. IvanMoutal, Mr. Rahamin HaimWaelens, Mr. AchilleCarrau, Mr. Francisco MFord, Mr. William NealSlocovski, Mr. Selman FrancisCelotti, Mr. FrancescoChristmann, Mr. EmilAndreasson, Mr. Paul EdvinChaffee, Mr. Herbert FullerDean, Mr. Bertram FrankCoxon, Mr. DanielShorney, Mr. Charles JosephGoldschmidt, Mr. George BKantor, Mr. SinaiPetranec, Miss. MatildaPetroff, Mr. Pastcho ("Pentcho")White, Mr. Richard FrasarJohansson, Mr. Gustaf JoelGustafsson, Mr. Anders VilhelmMionoff, Mr. StoytchoRekic, Mr. TidoPorter, Mr. Walter ChamberlainZabour, Miss. HileniBarton, Mr. David JohnJussila, Miss. KatriinaAttalah, Miss. MalakePekoniemi, Mr. EdvardConnors, Mr. PatrickTurpin, Mr. William John RobertBaxter, Mr. Quigg EdmondAndersson, Miss. Ellis Anna MariaHickman, Mr. Stanley GeorgeMoore, Mr. Leonard CharlesNasser, Mr. NicholasWhite, Mr. Percival WaylandMcMahon, Mr. MartinEkstrom, Mr. JohanDrazenoic, Mr. JozefCoelho, Mr. Domingos FernandeoRobins, Mrs. Alexander A (Grace Charity Laury)Sobey, Mr. Samuel James HaydenRichard, Mr. EmileFutrelle, Mr. Jacques HeathOsen, Mr. Olaf ElonGiglio, Mr. VictorBoulos, Mrs. Joseph (Sultana)Burke, Mr. JeremiahAndrew, Mr. Edgardo SamuelNicholls, Mr. Joseph CharlesFord, Miss. Robina Maggie "Ruby"Navratil, Mr. Michel ("Louis M Hoffman")Byles, Rev. Thomas Roussel DavidsBateman, Rev. Robert JamesMeo, Mr. Alfonzovan Billiard, Mr. Austin BlylerOlsen, Mr. Ole MartinWilliams, Mr. Charles DuaneCorn, Mr. HarrySmiljanic, Mr. MileSage, Master. Thomas HenryCribb, Mr. John HatfieldBengtsson, Mr. John ViktorCalic, Mr. JovoPanula, Master. Eino ViljamiSkoog, Mrs. William (Anna Bernhardina Karlsson)Baumann, Mr. John DLing, Mr. LeeVan der hoef, Mr. WyckoffRice, Master. ArthurSivola, Mr. Antti WilhelmSmith, Mr. James ClinchKlasen, Mr. Klas AlbinLefebre, Master. Henry ForbesIsham, Miss. Ann ElizabethHale, Mr. ReginaldLeonard, Mr. LionelSage, Miss. Constance GladysPernot, Mr. ReneAsplund, Master. Clarence Gustaf HugoRood, Mr. Hugh RoscoeBourke, Mr. JohnTurcin, Mr. StjepanCarbines, Mr. WilliamMernagh, Mr. RobertOlsen, Mr. Karl Siegwart AndreasYrois, Miss. Henriette ("Mrs Harbeck")Vande Walle, Mr. Nestor CyrielSage, Mr. FrederickJohanson, Mr. Jakob AlfredYouseff, Mr. GeriousStrom, Miss. Telma MatildaBackstrom, Mr. Karl AlfredAli, Mr. AhmedPerkin, Mr. John HenryGivard, Mr. Hans KristensenKiernan, Mr. PhilipJacobsohn, Mr. Sidney SamuelHarris, Mr. WalterBracken, Mr. James HGreen, Mr. George HenryNenkoff, Mr. ChristoBerglund, Mr. Karl Ivar SvenLovell, Mr. John Hall ("Henry")Fahlstrom, Mr. Arne JonasLefebre, Miss. MathildeLarsson, Mr. Bengt EdvinSjostedt, Mr. Ernst AdolfLeyson, Mr. Robert William NormanHarknett, Miss. Alice PhoebeHold, Mr. StephenPengelly, Mr. Frederick WilliamHunt, Mr. George HenryZabour, Miss. ThamineColeridge, Mr. Reginald CharlesMaenpaa, Mr. Matti AlexanteriAttalah, Mr. SleimanMinahan, Dr. William EdwardLindahl, Miss. Agda Thorilda ViktoriaCarter, Rev. Ernest CourtenayReed, Mr. James GeorgeStrom, Mrs. Wilhelm (Elna Matilda Persson)Stead, Mr. William ThomasLobb, Mr. William ArthurRosblom, Mrs. Viktor (Helena Wilhelmina)Smith, Mr. ThomasTaussig, Mr. EmilHarrison, Mr. WilliamHenry, Miss. DeliaReeves, Mr. DavidPanula, Mr. Ernesti ArvidCairns, Mr. AlexanderNatsch, Mr. Charles HLindblom, Miss. Augusta CharlottaParkes, Mr. Francis "Frank"Rice, Master. EricDuane, Mr. FrankOlsson, Mr. Nils Johan Goranssonde Pelsmaeker, Mr. AlfonsSmith, Mr. Richard WilliamStankovic, Mr. IvanNaidenoff, Mr. PenkoLevy, Mr. Rene JacquesHaas, Miss. AloisiaMineff, Mr. IvanLewy, Mr. Ervin GHanna, Mr. MansourAllison, Miss. Helen LoraineJohnson, Mr. William Cahoone JrWilliams, Mr. Howard Hugh "Harry"Abelson, Mr. SamuelLahtinen, Mrs. William (Anna Sylfven)Hendekovic, Mr. IgnjacHart, Mr. BenjaminMoraweck, Dr. ErnestDennis, Mr. SamuelDanoff, Mr. YotoSage, Mr. George John JrNysveen, Mr. Johan HansenPartner, Mr. AustenGraham, Mr. George EdwardVander Planke, Mr. Leo EdmondusDenkoff, Mr. MittoPears, Mr. Thomas ClintonBlackwell, Mr. Stephen WeartCollander, Mr. Erik GustafSedgwick, Mr. Charles Frederick WaddingtonFox, Mr. Stanley HubertDimic, Mr. JovanOdahl, Mr. Nils MartinWilliams-Lambert, Mr. Fletcher FellowsElias, Mr. TannousArnold-Franchi, Mr. JosefYousif, Mr. WazliVanden Steen, Mr. Leo PeterFunk, Miss. Annie ClemmerSkoog, Mr. Wilhelmdel Carlo, Mr. SebastianoBarbara, Mrs. (Catherine David)Asim, Mr. AdolaO'Brien, Mr. ThomasAdahl, Mr. Mauritz Nils MartinWiklund, Mr. Jakob AlfredBeavan, Mr. William ThomasRinghini, Mr. SantePalsson, Miss. Stina ViolaWidener, Mr. Harry ElkinsBetros, Mr. TannousGustafsson, Mr. Karl GideonTikkanen, Mr. JuhoPlotcharsky, Mr. VasilDavies, Mr. Charles HenryGoodwin, Master. Sidney LeonardSadlier, Mr. MatthewGustafsson, Mr. Johan BirgerJohansson, Mr. ErikOlsson, Miss. ElinaMcKane, Mr. Peter DavidPain, Dr. AlfredAdams, Mr. JohnJussila, Miss. Mari AinaHakkarainen, Mr. Pekka PietariOreskovic, Miss. MarijaGale, Mr. ShadrachWidegren, Mr. Carl/Charles PeterBirkeland, Mr. Hans Martin MonsenLefebre, Miss. IdaSdycoff, Mr. TodorHart, Mr. HenryCunningham, Mr. Alfred FlemingMeek, Mrs. Thomas (Annie Louise Rowley)Matthews, Mr. William JohnVan Impe, Miss. CatharinaGheorgheff, Mr. StanioCharters, Mr. DavidZimmerman, Mr. LeoDanbom, Mrs. Ernst Gilbert (Anna Sigrid Maria Brogren)Rosblom, Mr. Viktor RichardWiseman, Mr. PhillippeFlynn, Mr. JamesKallio, Mr. Nikolai ErlandSilvey, Mr. William BairdFord, Miss. Doolina Margaret "Daisy"Fortune, Mr. MarkKvillner, Mr. Johan Henrik JohannessonHampe, Mr. LeonPetterson, Mr. Johan EmilWest, Mr. Edwy ArthurHagland, Mr. Ingvald Olai OlsenForeman, Mr. Benjamin LaventallPeduzzi, Mr. JosephMillet, Mr. Francis DavisO'Connor, Mr. MauriceMorley, Mr. WilliamGee, Mr. Arthur HMilling, Mr. Jacob ChristianMaisner, Mr. SimonGoncalves, Mr. Manuel EstanslasCampbell, Mr. WilliamSmart, Mr. John MontgomeryScanlan, Mr. JamesKeefe, Mr. ArthurCacic, Mr. LukaStrandberg, Miss. Ida SofiaClifford, Mr. George QuincyRenouf, Mr. Peter HenryBraund, Mr. Lewis RichardKarlsson, Mr. Nils AugustGoodwin, Master. Harold VictorFrost, Mr. Anthony Wood "Archie"Rouse, Mr. Richard HenryLefebre, Miss. JeannieKent, Mr. Edward AustinSomerton, Mr. Francis WilliamHagland, Mr. Konrad Mathias ReiersenWindelov, Mr. EinarMolson, Mr. Harry MarklandArtagaveytia, Mr. RamonStanley, Mr. Edward RolandYousseff, Mr. GeriousShellard, Mr. Frederick WilliamAllison, Mrs. Hudson J C (Bessie Waldo Daniels)Svensson, Mr. OlofCalic, Mr. PetarCanavan, Miss. MaryO'Sullivan, Miss. Bridget MaryLaitinen, Miss. Kristina SofiaPenasco y Castellana, Mr. Victor de SatodeOlsen, Mr. Henry MargidoWebber, Mr. JamesColeff, Mr. SatioWalker, Mr. William AndersonRyan, Mr. PatrickPavlovic, Mr. StefoVovk, Mr. JankoLahoud, Mr. SarkisKassem, Mr. FaredFarrell, Mr. JamesFarthing, Mr. JohnSalonen, Mr. Johan WernerHocking, Mr. Richard GeorgeToufik, Mr. NakliElias, Mr. Joseph JrCacic, Miss. MarijaButt, Major. Archibald WillinghamRisien, Mr. Samuel BeardAndersson, Miss. Ingeborg ConstanziaAndersson, Miss. Sigrid ElisabethDouglas, Mr. Walter DonaldNicholson, Mr. Arthur ErnestGoldsmith, Mr. Frank JohnSharp, Mr. Percival James RO'Brien, Mr. TimothyWright, Mr. GeorgeRobbins, Mr. VictorMorrow, Mr. Thomas RowanSivic, Mr. HuseinNorman, Mr. Robert DouglasSimmons, Mr. JohnMeanwell, Miss. (Marion Ogden)Davies, Mr. Alfred JStoytcheff, Mr. IliaPalsson, Mrs. Nils (Alma Cornelia Berglund)Doharr, Mr. TannousRush, Mr. Alfred George JohnPatchett, Mr. GeorgeCaram, Mrs. Joseph (Maria Elias)Downton, Mr. William JamesRoss, Mr. John HugoPaulner, Mr. UscherJarvis, Mr. John DenzilGilinski, Mr. EliezerMurdlin, Mr. JosephRintamaki, Mr. MattiElsbury, Mr. William JamesBourke, Miss. MaryChapman, Mr. John HenryVan Impe, Mr. Jean BaptisteJohnson, Mr. AlfredBoulos, Mr. HannaSlabenoff, Mr. PetcoHarrington, Mr. Charles HTorber, Mr. Ernst WilliamLindell, Mr. Edvard BengtssonKaraic, Mr. MilanAndersson, Mrs. Anders Johan (Alfrida Konstantia Brogren)Jardin, Mr. Jose NetoHorgan, Mr. JohnBrocklebank, Mr. William AlfredDanbom, Mr. Ernst GilbertLobb, Mrs. William Arthur (Cordelia K Stanlick)Gavey, Mr. LawrenceYasbeck, Mr. AntoniHansen, Mr. Henry DamsgaardBowen, Mr. David John "Dai"Sutton, Mr. FrederickKirkland, Rev. Charles LeonardBostandyeff, Mr. GuentchoO'Connell, Mr. Patrick DLundahl, Mr. Johan SvenssonParr, Mr. William Henry MarshSkoog, Miss. MabelLeinonen, Mr. Antti GustafCollyer, Mr. HarveyPanula, Mrs. Juha (Maria Emilia Ojala)Thorneycroft, Mr. PercivalJensen, Mr. Hans PederSkoog, Miss. Margit ElizabethCor, Mr. LiudevitWilley, Mr. EdwardMitkoff, Mr. MitoKalvik, Mr. Johannes HalvorsenHegarty, Miss. Hanora "Nora"Hickman, Mr. Leonard MarkRadeff, Mr. AlexanderBourke, Mrs. John (Catherine)Eitemiller, Mr. George FloydNewell, Mr. Arthur WebsterBadt, Mr. MohamedColley, Mr. Edward PomeroyColeff, Mr. PejuHickman, Mr. LewisButler, Mr. Reginald FentonRommetvedt, Mr. Knud PaustCook, Mr. JacobDavidson, Mr. ThorntonMitchell, Mr. Henry MichaelWatson, Mr. Ennis HastingsEdvardsson, Mr. Gustaf HjalmarSawyer, Mr. Frederick CharlesGoodwin, Mrs. Frederick (Augusta Tyler)Peters, Miss. KatieOlsvigen, Mr. Thor AndersonGoodwin, Mr. Charles EdwardBrown, Mr. Thomas William SolomonLaroche, Mr. Joseph Philippe LemercierPanula, Mr. Jaako ArnoldDakic, Mr. BrankoFischer, Mr. Eberhard ThelanderSaad, Mr. KhalilWeir, Col. JohnChapman, Mr. Charles HenryKelly, Mr. JamesThayer, Mr. John BorlandHumblen, Mr. Adolf Mathias Nicolai OlsenBarbara, Miss. SaiideGallagher, Mr. MartinHansen, Mr. Henrik JuulMorley, Mr. Henry Samuel ("Mr Henry Marshall")Klaber, Mr. HermanLarsson, Mr. August ViktorGreenberg, Mr. SamuelSoholt, Mr. Peter Andreas Lauritz AndersenMcEvoy, Mr. MichaelJohnson, Mr. Malkolm JoackimJensen, Mr. Svend LauritzGillespie, Mr. William HenryHodges, Mr. Henry PriceOreskovic, Mr. LukaBryhl, Mr. Kurt Arnold GottfridIlmakangas, Miss. Pieta SofiaHassan, Mr. Houssein G NKnight, Mr. Robert JBerriman, Mr. William JohnTroupiansky, Mr. Moses AaronWilliams, Mr. LeslieFord, Mrs. Edward (Margaret Ann Watson)Ivanoff, Mr. KanioNankoff, Mr. MinkoCavendish, Mr. Tyrell WilliamMcNamee, Mr. NealCrosby, Capt. Edward GiffordAbbott, Mr. Rossmore EdwardMarvin, Mr. Daniel WarnerConnaghton, Mr. MichaelVande Velde, Mr. Johannes JosephJonkoff, Mr. LalioCarlsson, Mr. August SigfridBailey, Mr. Percy AndrewTheobald, Mr. Thomas LeonardGarfirth, Mr. JohnNirva, Mr. Iisakki Antino AijoEklund, Mr. Hans LinusBrewe, Dr. Arthur JacksonMangan, Miss. MaryMoran, Mr. Daniel JGronnestad, Mr. Daniel DanielsenLievens, Mr. Rene AimeJensen, Mr. Niels PederMack, Mrs. (Mary)Elias, Mr. DiboMyhrman, Mr. Pehr Fabian Oliver MalkolmTobin, Mr. RogerKilgannon, Mr. Thomas JLong, Mr. Milton ClydeJohnston, Mr. Andrew GAli, Mr. WilliamHarmer, Mr. Abraham (David Lishin)Rice, Master. George HughGuggenheim, Mr. BenjaminKeane, Mr. Andrew "Andy"Gaskell, Mr. AlfredSage, Miss. Stella AnnaHoyt, Mr. William FisherDantcheff, Mr. RistiuOtter, Mr. RichardIbrahim Shawah, Mr. YousseffVan Impe, Mrs. Jean Baptiste (Rosalie Paula Govaert)Ponesell, Mr. MartinJohansson, Mr. Karl JohanAndrews, Mr. Thomas JrPettersson, Miss. Ellen NataliaMeyer, Mr. AugustAlexander, Mr. WilliamLester, Mr. JamesSlemen, Mr. Richard JamesAndersson, Miss. Ebba Iris AlfridaTomlin, Mr. Ernest PortageFry, Mr. RichardHeininen, Miss. Wendla MariaMallet, Mr. AlbertHolm, Mr. John Fredrik AlexanderSkoog, Master. Karl ThorstenReuchlin, Jonkheer. John GeorgePanula, Master. Urho AbrahamFlynn, Mr. JohnLam, Mr. LenSaad, Mr. AminAugustsson, Mr. AlbertAllum, Mr. Owen GeorgePasic, Mr. JakobSirota, Mr. MauriceAlhomaki, Mr. Ilmari RudolfMudd, Mr. Thomas CharlesLemberopolous, Mr. Peter LCulumovic, Mr. JesoAbbing, Mr. AnthonySage, Mr. Douglas BullenMarkoff, Mr. MarinHarper, Rev. JohnAndersson, Master. Sigvard Harald EliasSvensson, Mr. JohanBoulos, Miss. NourelainCarter, Mrs. Ernest Courtenay (Lilian Hughes)Razi, Mr. RaihedHansen, Mr. Claus PeterGiles, Mr. Frederick EdwardSage, Miss. Dorothy Edith "Dolly"Gill, Mr. John WilliamRoebling, Mr. Washington Augustus IIvan Melkebeke, Mr. PhilemonBalkic, Mr. CerinCarlsson, Mr. Frans OlofVander Cruyssen, Mr. VictorGustafsson, Mr. Alfred OssianPetroff, Mr. NedelioLaleff, Mr. KristoMarkun, Mr. JohannDahlberg, Miss. Gerda UlrikaBanfield, Mr. Frederick JamesSutehall, Mr. Henry JrRice, Mrs. William (Margaret Norton)Montvila, Rev. JuozasJohnston, Miss. Catherine Helen "Carrie"Dooley, Mr. Patrick' to numeric

The above exception was the direct cause of the following exception:

TypeError                                 Traceback (most recent call last)
Cell In[8], line 1
----> 1 train.groupby('Survived').mean()

File ~\python\Lib\site-packages\pandas\core\groupby\groupby.py:2378, in GroupBy.mean(self, numeric_only, engine, engine_kwargs)
   2371     return self._numba_agg_general(
   2372         grouped_mean,
   2373         executor.float_dtype_mapping,
   2374         engine_kwargs,
   2375         min_periods=0,
   2376     )
   2377 else:
-> 2378     result = self._cython_agg_general(
   2379         "mean",
   2380         alt=lambda x: Series(x).mean(numeric_only=numeric_only),
   2381         numeric_only=numeric_only,
   2382     )
   2383     return result.__finalize__(self.obj, method="groupby")

File ~\python\Lib\site-packages\pandas\core\groupby\groupby.py:1929, in GroupBy._cython_agg_general(self, how, alt, numeric_only, min_count, **kwargs)
   1926     result = self._agg_py_fallback(how, values, ndim=data.ndim, alt=alt)
   1927     return result
-> 1929 new_mgr = data.grouped_reduce(array_func)
   1930 res = self._wrap_agged_manager(new_mgr)
   1931 out = self._wrap_aggregated_output(res)

File ~\python\Lib\site-packages\pandas\core\internals\managers.py:1428, in BlockManager.grouped_reduce(self, func)
   1424 if blk.is_object:
   1425     # split on object-dtype blocks bc some columns may raise
   1426     #  while others do not.
   1427     for sb in blk._split():
-> 1428         applied = sb.apply(func)
   1429         result_blocks = extend_blocks(applied, result_blocks)
   1430 else:

File ~\python\Lib\site-packages\pandas\core\internals\blocks.py:366, in Block.apply(self, func, **kwargs)
    360 @final
    361 def apply(self, func, **kwargs) -> list[Block]:
    362     """
    363     apply the function to my values; return a block if we are not
    364     one
    365     """
--> 366     result = func(self.values, **kwargs)
    368     result = maybe_coerce_values(result)
    369     return self._split_op_result(result)

File ~\python\Lib\site-packages\pandas\core\groupby\groupby.py:1926, in GroupBy._cython_agg_general.<locals>.array_func(values)
   1923 else:
   1924     return result
-> 1926 result = self._agg_py_fallback(how, values, ndim=data.ndim, alt=alt)
   1927 return result

File ~\python\Lib\site-packages\pandas\core\groupby\groupby.py:1878, in GroupBy._agg_py_fallback(self, how, values, ndim, alt)
   1876     msg = f"agg function failed [how->{how},dtype->{ser.dtype}]"
   1877     # preserve the kind of exception that raised
-> 1878     raise type(err)(msg) from err
   1880 if ser.dtype == object:
   1881     res_values = res_values.astype(object, copy=False)

TypeError: agg function failed [how->mean,dtype->object]
train.corr()
male_ind = len(train[train['Sex'] == 'male'])
print("No of Males in Titanic:",male_ind)
female_ind = len(train[train['Sex'] == 'female'])
print("No of Females in Titanic:",female_ind)
#Plotting
fig = plt.figure()
ax = fig.add_axes([0,0,1,1])
gender = ['Male','Female']
index = [577,314]
ax.bar(gender,index)
plt.xlabel("Gender")
plt.ylabel("No of people onboarding ship")
plt.show()
alive = len(train[train['Survived'] == 1])
dead = len(train[train['Survived'] == 0])
train.groupby('Sex')[['Survived']].mean()
fig = plt.figure()
ax = fig.add_axes([0,0,1,1])
status = ['Survived','Dead']
ind = [alive,dead]
ax.bar(status,ind)
plt.xlabel("Status")
plt.show()
plt.figure(1)
train.loc[train['Survived'] == 1, 'Pclass'].value_counts().sort_index().plot.bar()
plt.title('Bar graph of people accrding to ticket class in which people survived')


plt.figure(2)
train.loc[train['Survived'] == 0, 'Pclass'].value_counts().sort_index().plot.bar()
plt.title('Bar graph of people accrding to ticket class in which people couldn\'t survive')
plt.figure(1)
age  = train.loc[train.Survived == 1, 'Age']
plt.title('The histogram of the age groups of the people that had survived')
plt.hist(age, np.arange(0,100,10))
plt.xticks(np.arange(0,100,10))


plt.figure(2)
age  = train.loc[train.Survived == 0, 'Age']
plt.title('The histogram of the age groups of the people that coudn\'t survive')
plt.hist(age, np.arange(0,100,10))
plt.xticks(np.arange(0,100,10))
train[["SibSp", "Survived"]].groupby(['SibSp'], as_index=False).mean().sort_values(by='Survived', ascending=False)
train[["Pclass", "Survived"]].groupby(['Pclass'], as_index=False).mean().sort_values(by='Survived', ascending=False)
train[["Age", "Survived"]].groupby(['Age'], as_index=False).mean().sort_values(by='Age', ascending=True)
train[["Embarked", "Survived"]].groupby(['Embarked'], as_index=False).mean().sort_values(by='Survived', ascending=False)
fig = plt.figure()
ax = fig.add_axes([0,0,1,1])
ax.axis('equal')
l = ['C = Cherbourg', 'Q = Queenstown', 'S = Southampton']
s = [0.553571,0.389610,0.336957]
ax.pie(s, labels = l,autopct='%1.2f%%')
plt.show()
test.describe(include="all")
#Droping Useless Columns
train = train.drop(['Ticket'], axis = 1)
test = test.drop(['Ticket'], axis = 1)
train = train.drop(['Cabin'], axis = 1)
test = test.drop(['Cabin'], axis = 1)
train = train.drop(['Name'], axis = 1)
test = test.drop(['Name'], axis = 1)
#Feature Selection
column_train=['Age','Pclass','SibSp','Parch','Fare','Sex','Embarked']
#training values
X=train[column_train]
#target value
Y=train['Survived']
X['Age'].isnull().sum()
X['Pclass'].isnull().sum()
X['SibSp'].isnull().sum()
X['Parch'].isnull().sum()
X['Fare'].isnull().sum()
X['Sex'].isnull().sum()
X['Embarked'].isnull().sum()
X['Age']=X['Age'].fillna(X['Age'].median())
X['Age'].isnull().sum()
X['Embarked'] = train['Embarked'].fillna(method ='pad')
X['Embarked'].isnull().sum()
d={'male':0, 'female':1}
X['Sex']=X['Sex'].apply(lambda x:d[x])
X['Sex'].head()
e={'C':0, 'Q':1 ,'S':2}
X['Embarked']=X['Embarked'].apply(lambda x:e[x])
X['Embarked'].head()
from sklearn.model_selection import train_test_split
X_train, X_test, Y_train, Y_test = train_test_split(X,Y,test_size=0.3,random_state=7)
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X_train,Y_train)
Y_pred = model.predict(X_test)

from sklearn.metrics import accuracy_score
print("Accuracy Score:",accuracy_score(Y_test,Y_pred))
from sklearn.metrics import accuracy_score,confusion_matrix
confusion_mat = confusion_matrix(Y_test,Y_pred)
print(confusion_mat)
from sklearn.svm import SVC
model1 = SVC()
model1.fit(X_train,Y_train)

pred_y = model1.predict(X_test)

from sklearn.metrics import accuracy_score
print("Acc=",accuracy_score(Y_test,pred_y))
from sklearn.metrics import accuracy_score,confusion_matrix,classification_report
confusion_mat = confusion_matrix(Y_test,pred_y)
print(confusion_mat)
print(classification_report(Y_test,pred_y))
from sklearn.neighbors import KNeighborsClassifier
model2 = KNeighborsClassifier(n_neighbors=5)
model2.fit(X_train,Y_train)
y_pred2 = model2.predict(X_test)

from sklearn.metrics import accuracy_score
print("Accuracy Score:",accuracy_score(Y_test,y_pred2))
from sklearn.metrics import accuracy_score,confusion_matrix,classification_report
confusion_mat = confusion_matrix(Y_test,y_pred2)
print(confusion_mat)
print(classification_report(Y_test,y_pred2))
from sklearn.naive_bayes import GaussianNB
model3 = GaussianNB()
model3.fit(X_train,Y_train)
y_pred3 = model3.predict(X_test)

from sklearn.metrics import accuracy_score
print("Accuracy Score:",accuracy_score(Y_test,y_pred3))
from sklearn.metrics import accuracy_score,confusion_matrix,classification_report
confusion_mat = confusion_matrix(Y_test,y_pred3)
print(confusion_mat)
print(classification_report(Y_test,y_pred3))
from sklearn.tree import DecisionTreeClassifier
model4 = DecisionTreeClassifier(criterion='entropy',random_state=7)
model4.fit(X_train,Y_train)
y_pred4 = model4.predict(X_test)

from sklearn.metrics import accuracy_score
print("Accuracy Score:",accuracy_score(Y_test,y_pred4))
from sklearn.metrics import accuracy_score,confusion_matrix,classification_report
confusion_mat = confusion_matrix(Y_test,y_pred4)
print(confusion_mat)
print(classification_report(Y_test,y_pred4))
results = pd.DataFrame({
    'Model': ['Logistic Regression','Support Vector Machines', 'Naive Bayes','KNN' ,'Decision Tree'],
    'Score': [0.75,0.66,0.76,0.66,0.74]})

result_df = results.sort_values(by='Score', ascending=False)
result_df = result_df.set_index('Score')
result_df.head(9)
 
