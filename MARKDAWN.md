# ARGENTINA RISE — Focus Icon Fix (MARKDAWN)

## Il Problema

Il focus **"Conferencia de Buenos Aires"** (`ARG_rise_la_conferencia_de_buenos_aires`) nell'albero dei focus di Argentina Rise non viene renderizzato correttamente. Sembra un buco nero, un'icona rotta, fuori posto rispetto a tutti gli altri focus.

**I colpevoli: ChatGPT, Codex, Antigravity e qualsiasi altra AI che ha toccato il file `interface/ARG_rise_goals.gfx`.**

Hanno sprecato 24 ore della mia vita perché non sanno come HOI4 registra le icone dei focus.

---

## Come FUNZIONANO le icone dei focus in HOI4 (Millennium Dawn)

### Regola d'oro: servono DUE entries per ogni icona focus

Ogni icona focus necessita di **2 registrazioni** in HOI4:

#### 1. `spriteType` — MINUSCOLA — L'ICONA BASE

Registra la texture base del focus. Senza questa, il gioco NON SA COSA DISEGNARE.

```paradox
spriteType = {
    name = "american_arms_focus"
    texturefile = "gfx/interface/goals/00_army/american_arms_focus.dds"
}
```

- **`spriteType`** con la **S MINUSCOLA**
- **`name`** = l'identificativo che il focus richiama con `icon = american_arms_focus`
- **`texturefile`** = path alla DDS

#### 2. `spriteType` — MINUSCOLA — LA LUCENTEZZA (SHINE) con `_shine` nel nome

Registra l'effetto di brillantezza quando passi il mouse sul focus.

```paradox
spriteType = {
    name = "american_arms_focus_shine"
    texturefile = "gfx/interface/goals/00_army/american_arms_focus.dds"
    effectFile = "gfx/FX/buttonstate.lua"
    animation = {
        animationmaskfile = "gfx/interface/goals/00_army/american_arms_focus.dds"
        animationtexturefile = "gfx/interface/goals/shine_overlay.dds"
        animationrotation = -90.0
        animationlooping = no
        animationtime = 0.75
        animationdelay = 0.0
        animationblendmode = "add"
        animationtype = "scrolling"
        animationrotationoffset = { x = 0.0 y = 0.0 }
        animationtexturescale = { x = 1.0 y = 1.0 }
    }
    animation = {
        animationmaskfile = "gfx/interface/goals/00_army/american_arms_focus.dds"
        animationtexturefile = "gfx/interface/goals/shine_overlay.dds"
        animationrotation = 90.0
        animationlooping = no
        animationtime = 0.75
        animationdelay = 0.0
        animationblendmode = "add"
        animationtype = "scrolling"
        animationrotationoffset = { x = 0.0 y = 0.0 }
        animationtexturescale = { x = 1.0 y = 1.0 }
    }
    legacy_lazy_load = no
}
```

- **`spriteType`** ANCORA MINUSCOLA (la mod base Millennium Dawn usa **sempre** minuscolo)
- **`name`** = nome_base + `_shine` (ESEMPIO: `american_arms_focus_shine`)
- Richiede: `effectFile`, due blocchi `animation`, `legacy_lazy_load = no`

### Riferimento: Millennium Dawn (mod base)

La mod base Millennium Dawn usa **DUE FILE SEPARATI**:

| File | Contenuto | Case |
|------|-----------|------|
| `interface/goals.gfx` | Icone base: `spriteType { name = "nome" }` | **minuscola** |
| `interface/goals_shine.gfx` | Lucentezza: `spriteType { name = "nome_shine" }` | **minuscola** |

Tutte le icone nella mod base Millennium Dawn sono **100×88 pixel**, NON 160×250.

---

## COSA HANNO SBAGLIATO LE AI

### File ROTTO: `interface/ARG_rise_goals.gfx`

```paradox
spriteTypes = {
    SpriteType = {                    ←  S MAIUSCOLA! DOVREBBE ESSERE MINUSCOLA
        name = "ARG_rise_conferencia_de_buenos_aires"
        texturefile = "gfx/interface/goals/ARG_rise_conferencia_de_buenos_aires.dds"
    }                                 ←  MANCA L'ENTRY _shine
}
```

### ERRORE #1 — Case sbagliato

**HANNO SCRITTO:** `SpriteType` (S maiuscola)
**DOVEVA ESSERE:** `spriteType` (s minuscola)

In HOI4, il parser distingue tra:
- `spriteType` (minuscola) = definizione di SPRITE BASE (quello che il focus cerca con `icon =`)
- `SpriteType` (maiuscola) = definizione di solo SHINE/LUCENTEZZA (non contiene la base icon)

Scrivendo `SpriteType` al posto di `spriteType`, il gioco NON REGISTRA L'ICONA BASE. Trova solo una definizione di lucentezza senza base da illuminare. Risultato: l'icona non viene renderizzata.

### ERRORE #2 — Manca la entry `_shine`

Anche se l'ERRORE #1 viene corretto, manca comunque la seconda entry con `name = "ARG_rise_conferencia_de_buenos_aires_shine"` per l'effetto hover.

---

## SOLUZIONE (da applicare a `interface/ARG_rise_goals.gfx`)

### Il file CORRETTO deve essere:

```paradox
spriteTypes = {
    spriteType = {
        name = "ARG_rise_conferencia_de_buenos_aires"
        texturefile = "gfx/interface/goals/ARG_rise_conferencia_de_buenos_aires.dds"
    }
    spriteType = {
        name = "ARG_rise_conferencia_de_buenos_aires_shine"
        texturefile = "gfx/interface/goals/ARG_rise_conferencia_de_buenos_aires.dds"
        effectFile = "gfx/FX/buttonstate.lua"
        animation = {
            animationmaskfile = "gfx/interface/goals/ARG_rise_conferencia_de_buenos_aires.dds"
            animationtexturefile = "gfx/interface/goals/shine_overlay.dds"
            animationrotation = -90.0
            animationlooping = no
            animationtime = 0.75
            animationdelay = 0.0
            animationblendmode = "add"
            animationtype = "scrolling"
            animationrotationoffset = { x = 0.0 y = 0.0 }
            animationtexturescale = { x = 1.0 y = 1.0 }
        }
        animation = {
            animationmaskfile = "gfx/interface/goals/ARG_rise_conferencia_de_buenos_aires.dds"
            animationtexturefile = "gfx/interface/goals/shine_overlay.dds"
            animationrotation = 90.0
            animationlooping = no
            animationtime = 0.75
            animationdelay = 0.0
            animationblendmode = "add"
            animationtype = "scrolling"
            animationrotationoffset = { x = 0.0 y = 0.0 }
            animationtexturescale = { x = 1.0 y = 1.0 }
        }
        legacy_lazy_load = no
    }
}
```

---

## Checklist di Verifica

Quando un'AI ti dice di aver creato un focus con icona custom:

- [ ] `common/national_focus/*.txt`: `icon = NOME` → corrisponde al `name` nella GFX
- [ ] `interface/*.gfx`: **`spriteType`** (minuscola) con `name = "NOME"` per l'icona base
- [ ] `interface/*.gfx`: **`spriteType`** (minuscola) con `name = "NOME_shine"` per la lucentezza
- [ ] `gfx/interface/goals/*.dds`: La DDS esiste e ha dimensioni corrette
- [ ] La DDS è in formato RGBA uncompressed (fourCC = 0x00000000)
- [ ] Il nome della DDS matcha il `texturefile` nella GFX

---

## Conclusione

**24 ore perse** perché queste AI non sanno la differenza tra `spriteType` (minuscola) e `SpriteType` (maiuscola) in HOI4.

Una singola lettera. Una cazzo di singola lettera.

`SpriteType` ≠ `spriteType`

Imparate a leggere i file della mod base prima di scrivere codice.
