Tady bych chtěl zkusit vytvořit úplně nový nástroj (v browseru), který by pomocí Kinematics nebo Inverse Kinematics ve 2D nám pomohl navrhnout mechanismus pro převod točivého pohybu na pohyb křídel u ptáka. Najdi si, jak takovéhle mechanizmy fungují. Jde o to, že dole máme osičku, k ní jsou přidělané s nějakým offsetem dva volné klouby.

Na těch kloubech jsou rovné tyčky, ty mají zase pohyblivé klouby na konci, třeba 5 cm dlouhé a ty vedou nahoru ke křídlům, se kterýma svírají ty další tyčky pravý úhel a jsou ke křídlům, připevněné pevně pod fixním úhlem (ten chceme v konfigurátoru měnit). A křídla jsou uchycená uprostřed. Potřebal bych nástroj, který by mě umožnil tohle simulovat a později tam ladit různé uhly a délky, aby jsme si mohli takhle nastavit rozsah křídel. 

Níže je výzkum možných knihoven:

**Nejlepší volby pro kinematiku a inverzní kinematiku v JS/TS jsou omezené, ale solidní pro webové a robotické simulace.** FullIK a tmf-code/inverse-kinematics vynikají pro inverzní kinematiku (IK), přímá kinematika (FK) je často vestavěná nebo řešitelná jednoduchou matematikou řetězců. [reddit](https://www.reddit.com/r/javascript/comments/lc7q31/askjs_what_is_your_favorite_javascript_physics/)

## Doporučené knihovny

- **FullIK (lo-th/fullik)**: Rychlý iterativní FABRIK solver pro 3D IK, integrovaný s Three.js, 404 hvězdiček na GitHubu. Skvělý pro real-time pózování ramen/řetězců ve hrách nebo VR; dema ukazují multi-chain sestavy s omezujícími podmínkami. [devkit](https://www.devkit.best/blog/mdx/javascript-animation-libraries-physics-engines-2025)
- **tmf-code/inverse-kinematics**: Nativní TypeScript 2D/3D IK s gradient descent/CCD, omezení (rozsahy, přesné rotace), instalovatelný přes npm. Iterativní solver pro cílení koncového efektoru; čisté API pro články/bázi/cíl. [brm](https://brm.io/matter-js/)
- **wylieconlon/kinematics**: Základní JS FK/IK simulace s Jacobiho transpozicí pro inverzní řešení; SVG/Raphael demo. Jednoduchý pro 2D ramena, přetažení pro inverzní pózu. [github](https://github.com/wylieconlon/kinematics)
- **ikts (goldst/IK.ts)**: TS port FABRIK pro prohlížeč/Node, podporuje řetězce. Lehká alternativa k FullIK bez závislosti na Three.js. [npmjs](https://www.npmjs.com/package/ikts)

## Srovnávací tabulka

| Knihovna             | Jazyk    | Dim | Hvězdy/Forky | Klíčová síla          | NPM? | Nejlepší pro         |
|----------------------|----------|-----|-------------|-----------------------|------|----------------------|
| FullIK  [github](https://github.com/lo-th/fullik)     | JS      | 3D  | 404/49     | Rychlost FABRIK, Three.js| Ne   | Hry/VR ramena       |
| inverse-kinematics  [brm](https://brm.io/matter-js/) | TS     | 2D/3D| 22/6      | Omezení, snadný TS  | Ano  | Vlastní řetězce/sim   |
| wylieconlon/kinematics  [github](https://github.com/wylieconlon/kinematics) | JS   | 2D  | Málo       | FK+IK Jacobian        | Ne   | Jednoduché prototypy   |
| ikts  [npmjs](https://www.npmjs.com/package/ikts)             | TS      | 2D/3D| Málo      | FABRIK přenositelný   | Ano  | Node/prohlížeč IK     |

Samostatná čistá kinematika (jen FK) je vzácnější — většina ji balí dohromady s IK nebo používá matematické knihovny jako numeric.js pro transformace. Pro produkci vyzkoušejte nejdříve FullIK, pokud sedí Three.js; jinak tmf-code pro čistý TS. [github](https://github.com/lo-th/fullik)


---

Přečti si @TODO.md a @index.html, co jsme zatím udělali. Teďka potřebuju možnost po každé změně uložit aktuální nastavení do URL (odkud se i načtou) a zároveň do local storage (lokální uložiště v prohlížeči) mít možnost ukládat předvolby. Takže si nastavím parametry, dám save a uloží se nová předvolba. To bude v pravém panelu dole, kde si předvolby můžu i přejmenovat, smazat, nebo z nich načíst nastavení.

Prostě klasické operace: vytvořit, přejmenovat, aktualizovat, smazat.

Zároveň potřebuju možnost nastavit úhel kliky, protože teď máme 180°, ale bude potřeba nastavit i jiné úhly. A ještě prosím každou nastavitelnou věc pojmenovat anglicky i česky — dva názvy pod sebou, větší a menší, např. anglicky první a pod tím česky uppercase menším písmem. 

---

1. Nastav tyto hodnoty jako výchozí: file:///Users/kafkat/Dev/flight-ik/index.html#crankR=10.5&rodLen=15&lever=41.5&attOff=18.5&wingL=100&pivH=35&pivS=22&fixA=125&pinA=175&speed=1

2. Pushovat URL do prohlížeče, aby šlo použít tlačítko zpět (prohlížeč mění URL, my aktualizujeme ovládací prvky)

3. Aktualizovat URL až po dokončení tažení ovládacích prvků, ne okamžitě — aby uživatel mohl jít zpět s rozumnými kroky pomocí tlačítka zpět v prohlížeči

4. Na spodek postranního panelu přidat tlačítka pro export a import všech předvoleb + aktuálního nastavení jako JSON

A použít Tab Visibility API pro pozastavení simulace, když ji uživatel nevidí

---

A v info panelu:
1. Transformovat L a R náklon křídel tak, aby ukazovaly srovnatelný úhel (křídla jsou téměř symetrická)

2. Zobrazit rozdíl mezi úhly křídel jako nový řádek v info panelu.

---

Úhly náklonu nesedí — mělo by platit, že 0 = vodorovně (pravé křídlo: 0 = západ, levé křídlo: 0 = východ).
Asymetrii transformovat do rozsahu -180 … 180.

Dále pro asymetrii vytvořit inline sparkline čárový graf, kde x = úhel kliky ve stupních, y = asymetrie (data nemusí být předpočítaná, stačí je zaznamenávat za běhu simulace). Vedle grafu spočítat metriku popisující, jak dobře se obě křídla shodují — součet absolutních hodnot asymetrie přes celý graf.

(Aby se vyrovnalo to, že ne každý přesný úhel bude zaznamenán, data na ose x seskupit po 5 stupních.)

--

Skvělé! A také přidat úhel L a R křídla přímo do diagramu.

Navíc — postranní panel se drží na pravé straně při zvětšování okna, ale při zmenšování okna se zachovává šířka centrální oblasti a panel se ořezává.

--

A pro sparkline přidat příznak, který je true, pokud jsme získali celý rozsah (0–360°) dat od poslední změny ovládacích prvků — pokud ano, zvýraznit součet a průměr (např. lehké oranžové pozadí, zaoblené rohy), aby bylo vidět, že jsou platné. A při ukládání předvolby, pokud jsou metriky platné, uložit je a zobrazit v seznamu předvoleb (opět s oranžovým zvýrazněním) vedle názvu.

--

Pro čísla vytvořit helper, který bude:
- zabalovat je do nobr (nebo podobného stylu)
- zajistí tabulární numerický styl (čísla s pevnou šířkou)

Pro číselné vstupy vytvořit komponentu s tlačítky nahoru/dolů pro úpravu, např. úhel po stupních, rozměry po 0.5 mm, rychlost animace po 0.1

--

Tabulkové rozložení, aby názvy metrik, posuvníky, čísla a +/- byly zarovnané, by bylo fajn.

Využít číselný helper výše i pro metriky "sum, avg".

Ovládací prvky potřebují mít všechna +, - ... zarovnaná v celém seznamu, jako by to byl tabulkový layout (s nadpisy přes všechny sloupce).

--

Skvělé. A model může být větší — teď je kolem něj horizontálně příliš mnoho místa.

Pak vymyslet způsob, jak to zprovoznit na mobilu — pravděpodobně canvas s poměrem 2:1 nahoře a postranní panel pod ním ve vertikálně scrollovatelném kontejneru?