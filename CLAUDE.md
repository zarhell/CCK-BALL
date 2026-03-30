# CCK-BALL ZMK Keymap — Contexto y Reglas

Archivo cargado automáticamente por Claude Code. Evita reprocesos al analizar este proyecto.

---

## Contexto del proyecto

Teclado split 48 teclas con trackball (CCK-BALL), firmware ZMK 0.3, rama `main-zmk_0.3`.
Configuración en `config/cck_ball.keymap` + `config/cck_ball.conf`.

---

## Mapa de posiciones (48 teclas) — CRÍTICO

```
Fila 0:  0=TAB  1=Q   2=W   3=E   4=R   5=T  |  6=Y   7=U   8=I   9=O  10=P  11=BSPC
Fila 1: 12=ESC 13=A  14=S  15=D  16=F  17=G  | 18=H  19=J  20=K  21=L  22=;  23='"
Fila 2: 24=LSH 25=Z  26=X  27=C  28=V  29=B  | 30=N  31=M  32=,  33=.  34=UP 35=/
Fila 3: 36=CTL 37=WIN 38=ALT 39=\ 40=SPC 41=MO1 | 42=MO2 43=ENT 44=RALT 45=← 46=↓ 47=→
```

---

## Reglas antes de editar el keymap

1. **Leer el keymap completo** antes de proponer cambios.
2. **Verificar conflictos de combo**: dos combos no pueden tener exactamente los mismos `key-positions`.
3. **No hay límite Kconfig de combos** en ZMK 0.3 — `CONFIG_ZMK_COMBO_MAX` no existe como símbolo. Los combos se compilan directamente desde DTS sin límite configurable.
4. **No usar** `RA(A)`, `RA(E)`, `RA(I)`, `RA(O)`, `RA(U)` directamente para acentos — solo funciona en Windows US-Intl, falla en macOS estándar.

---

## Acentos españoles — cross-platform

Los combos de acento usan macros con **dead acute** (`RA(SQT)`) + vocal.

| Plataforma | Layout requerido |
|------------|-----------------|
| Windows | "United States-International" |
| macOS | "ABC Extended" (built-in) o "US International PC" (descargable) |

- `RA(SQT)` = dead acute en ambos layouts anteriores.
- `RA(N)` = ñ directo en Windows US-Intl; macOS necesita "US International PC".
- Combos: A+Z=á, E+D=é, I+K=í, O+L=ó, U+J=ú, J+N=ñ.

---

## Combos críticos ya definidos

| Combo | Posiciones | Resultado |
|-------|-----------|-----------|
| T+SPC | 5+40 | DEL |
| G+SPC | 17+40 | BSPC |
| B+SPC | 29+40 | ENTER |
| B+ENT | 29+43 | ENTER |
| F+T | 16+5 | dead acute ´ |
| E+T | 3+5 | sel_word |
| R+T | 4+5 | = |
| F+G | 16+17 | - |

---

## Mouse buttons

| Combo | Posiciones | Acción |
|-------|-----------|--------|
| T+G (izq) | 5+17 | LCLK |
| G+B (izq) | 17+29 | RCLK |
| G+V (izq) | 17+28 | MCLK |
| F+B (izq) | 16+29 | FWD (MB5) |
| H+J (der) | 18+19 | LCLK |
| J+K (der) | 19+20 | RCLK |
| H+K (der) | 18+20 | MCLK |
| U+H (der) | 7+18 | BACK (MB4) |
| Y+J (der) | 6+19 | FWD (MB5) |

> BACK izquierdo (T+F) fue reemplazado por dead acute ´.

---

## Capas

| ID | Nombre | Activación |
|----|--------|-----------|
| 0 | QWERTY | Base |
| 1 | NAV_NUM | MO1 (pos 41/pos 43 zona) |
| 2 | NUM_FN | MO2 |
| 3 | BT_layers | `mo 3` desde NAV_NUM (B=pos29 en layer 1) |
| 4 | scroll-layers | reservada |
| 5 | snipe-layers | reservada |

---

## Archivos clave

| Archivo | Propósito |
|---------|-----------|
| `config/cck_ball.keymap` | Keymap principal |
| `config/cck_ball.conf` | Kconfig (BLE, COMBO_MAX, mouse) |
| `config/private.dtsi` | Macros personales (email/user/pass) — NO commitear cambios |
| `config/boards/shields/cck_ball/cck_ball.dtsi` | Hardware (matrix, encoders) |

`private.dtsi`: usar `git update-index --assume-unchanged config/private.dtsi` para ocultar cambios locales.

---

## Sintaxis ZMK

```c
// Combo estándar
combo_name { timeout-ms = <35>; key-positions = <p1 p2>; bindings = <&behavior>; };

// Macro con dead key (necesita wait-ms para composición correcta)
macro_name: macro_name {
    compatible = "zmk,behavior-macro";
    #binding-cells = <0>;
    wait-ms = <30>;
    tap-ms = <30>;
    bindings = <&kp RA(SQT) &kp A>;  // dead acute + A = á
};

// &macro_press A B  →  presiona A y B simultáneamente
// &kp A &kp B       →  tapea A luego B (modo default en macros)
```
