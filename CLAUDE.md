# CCK-BALL ZMK Keymap — Contexto y Reglas

Archivo cargado automáticamente por Claude Code. Evita reprocesos al analizar este proyecto.

---

## Contexto del proyecto

Teclado split 48 teclas con trackball (CCK-BALL), firmware ZMK 0.3, rama `main-zmk_0.3`.
Configuración en `config/cck_ball.keymap` + `config/cck_ball.conf`.

---

## Mapa de posiciones (48 teclas) — CRÍTICO

> **Nota**: pos 0 tiene `&kp ESC` en el keymap (tecla física etiquetada TAB).
> pos 12 tiene `&lt 6 TAB` (tecla física etiquetada ESC). Usar posiciones, no etiquetas físicas.

```
Fila 0:  0=ESC  1=Q   2=W   3=E   4=R   5=T  |  6=Y   7=U   8=I   9=O  10=P  11=BSPC
Fila 1: 12=TAB 13=A  14=S  15=D  16=F  17=G  | 18=H  19=J  20=K  21=L  22=;  23='"
Fila 2: 24=LSH 25=Z  26=X  27=C  28=V  29=B  | 30=N  31=M  32=,  33=.  34=UP 35=/
Fila 3: 36=MO2 37=CTL 38=ALT 39=GUI 40=MO1 41=SPC | 42=ENT 43=MO1 44=MO2 45=← 46=↓ 47=→
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
- Combos: A+X=á, E+F=é, I+J=í, O+K=ó, U+H=ú, J+N=ñ.

---

## Combos críticos ya definidos

| Combo | Posiciones | Resultado |
|-------|-----------|-----------|
| T+SPC | 5+41 | DEL |
| G+SPC | 17+41 | BSPC |
| B+SPC | 29+41 | ENTER |
| F+T | 5+16 | dead acute ´ |
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
| H+J (der) | 18+19 | LCLK |
| J+K (der) | 19+20 | RCLK |
| H+K (der) | 18+20 | MCLK |
| Y+J (der) | 6+19 | FWD (MB5) |
| H+M (der) | 18+31 | BACK (MB4) |
| U+H (der) | 7+18 | ú (acento) |

---

## Capas

| ID | Nombre | Activación |
|----|--------|-----------|
| 0 | QWERTY | Base |
| 1 | NAV_NUM | MO1 (pos 40/pos 43) |
| 2 | NUM_FN | MO2 (pos 36/pos 44) |
| 3 | BT_layers | `mo 3` (desde NUM_FN capa 2) |
| 4 | snipe-layers | toggle MO1+MO2 der (pos 43+44) |
| 5 | MIRROR | hold SPC (pos 41) — espejo lado derecho en mano izquierda |

---

## Archivos clave

| Archivo | Propósito |
|---------|-----------|
| `config/cck_ball.keymap` | Keymap principal — encoders: izq=scroll, der=escritorios |
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
