Tady bych chtěl zkusit vytvořit úplně nový nástroj (v browseru), který by pomocí Kinematics nebo Inverse Kinematics ve 2D nám pomohl navrhnout mechanismus pro převod točivého pohybu na pohyb křídel u ptáka. Najdi si, jak takovéhle mechanizmy fungují. Jde o to, že dole máme osičku, k ní jsou přidělané s nějakým offsetem dva volné klouby.

Na těch kloubech jsou rovné tyčky, ty mají zase pohyblivé klouby na konci, třeba 5 cm dlouhé a ty vedou nahoru ke křídlům, se kterýma svírají ty další tyčky pravý úhel a jsou ke křídlům, připevněné pevně pod fixním úhlem (ten chceme v konfigurátoru měnit). A křídla jsou uchycená uprostřed. Potřebal bych nástroj, který by mě umožnil tohle simulovat a později tam ladit různé uhly a délky, aby jsme si mohli takhle nastavit rozsah křídel. 

Níže je výzkum možných knihoven:

**Top picks for kinematics and inverse kinematics in JS/TS are limited but solid for web and robotics sims.** FullIK and tmf-code/inverse-kinematics stand out for inverse kinematics (IK), with forward kinematics (FK) often built-in or via simple chain math. [reddit](https://www.reddit.com/r/javascript/comments/lc7q31/askjs_what_is_your_favorite_javascript_physics/)

## Recommended Libraries

- **FullIK (lo-th/fullik)**: Fast iterative FABRIK solver for 3D IK, Three.js integrated, 404 GitHub stars. Great for real-time arm/chain posing in games or VR; demos show multi-chain setups with constraints. [devkit](https://www.devkit.best/blog/mdx/javascript-animation-libraries-physics-engines-2025)
- **tmf-code/inverse-kinematics**: TypeScript-native 2D/3D IK with gradient descent/CCD, constraints (ranges, exact rotations), npm installable. Iterative solver for end-effector targeting; clean APIs for links/base/target. [brm](https://brm.io/matter-js/)
- **wylieconlon/kinematics**: Basic JS FK/IK sim with Jacobian transpose for inverse; SVG/Raphael demo. Simple for 2D arms, drag-to-pose inverse. [github](https://github.com/wylieconlon/kinematics)
- **ikts (goldst/IK.ts)**: TS port of FABRIK for browser/Node, supports chains. Lightweight alternative to FullIK without Three.js tie-in. [npmjs](https://www.npmjs.com/package/ikts)

## Comparison Table

| Library              | Language | Dim | Stars/Forks | Key Strength          | NPM? | Best For             |
|----------------------|----------|-----|-------------|-----------------------|------|----------------------|
| FullIK  [github](https://github.com/lo-th/fullik)     | JS      | 3D  | 404/49     | FABRIK speed, Three.js| No   | Games/VR arms       |
| inverse-kinematics  [brm](https://brm.io/matter-js/) | TS     | 2D/3D| 22/6      | Constraints, easy TS  | Yes  | Custom chains/sim   |
| wylieconlon/kinematics  [github](https://github.com/wylieconlon/kinematics) | JS   | 2D  | Low        | FK+IK Jacobian        | No   | Simple prototypes   |
| ikts  [npmjs](https://www.npmjs.com/package/ikts)             | TS      | 2D/3D| Low       | FABRIK portable       | Yes  | Node/browser IK     |

Pure kinematics (FK only) are rarer standalone—most bundle with IK or use math libs like numeric.js for transforms. For production, test FullIK first if Three.js fits; otherwise tmf-code for TS purity. [github](https://github.com/lo-th/fullik)


---

Přečti si @TODO.md a @index.html co jsme zatím udělali.  Tak teďka potřebuju možnost mít po každé změně uložit aktuální nastavení do URL (odkud se i načtou) a zároveň si do local storage, do lokálního uložiště v prohlížeči, mít možnost ukládat předvolby. Takže si nějak nastavím ty parametry a dám save a uloží se mi nová předvolba. To je třeba v pravém panelu dole a tam si ty předvolby můžu i přejmenovat a smazat, pokud budu chtít, anebo zase nahrát z nich ty předvolby.

Prostě obvykle create, edit, update, delete. 

Zároveň potřebuju ještě možnost mít si nastavit úhel u kliky, protože teď tam máme 180 stupňů, ale vypadá to, že ho budeme potřebovat nastavit i na jiné úhly. A ještě prosím, každou nastavitelnou věc pojmenuj anglicky i česky. Může to být třeba dva názvy pod sebou, větší a menší. Nebo třeba anglicky první a pod tím česky uppercase menším. 