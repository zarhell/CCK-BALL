# CCK-BALL ZMK Keymap — Contexto y Reglas

Archivo cargado automáticamente por Claude Code. Evita reprocesos al analizar este proyecto.

---

## Contexto del proyecto

Teclado split 48 teclas con trackball (CCK-BALL), firmware ZMK 0.3, rama `main-zmk_0.3`.
Configuración en `config/cck_ball.keymap` + `config/cck_ball.conf`.

---

## Mapa de posiciones (48 teclas) — CRÍTICO

Usar posiciones, no etiquetas físicas.

```
Fila 0:  0=ESC  1=Q   2=W   3=E   4=R   5=T  |  6=Y   7=U   8=I   9=O  10=P  11=BSPC
Fila 1: 12=TAB 13=A  14=S  15=D  16=F  17=G  | 18=H  19=J  20=K  21=L  22=;  23='"
Fila 2: 24=LSH 25=Z  26=X  27=C  28=V  29=B  | 30=N  31=M  32=MO2 33=Cmd← 34=UP 35=Cmd→
Fila 3: 36=CTL 37=ACCENT 38=ALT 39=CMD 40=MO1 41=lt2 SPC | 42=ENT 43=MO1 44=ALT 45=← 46=↓ 47=→
```

---

## Estilo de respuestas

- **No describir** el resumen de operaciones realizadas. Solo sintetizar el resultado de forma muy concisa.

---

## Reglas antes de editar el keymap

1. **Leer el keymap completo** antes de proponer cambios.
2. **Verificar conflictos de combo**: dos combos no pueden tener exactamente los mismos `key-positions`.
3. **No hay límite Kconfig de combos** en ZMK 0.3 — `CONFIG_ZMK_COMBO_MAX` no existe como símbolo. Los combos se compilan directamente desde DTS sin límite configurable.
4. **Acentos**: no usar `RA(A)`, `RA(I)`, `RA(O)`, `RA(U)` directamente (solo Windows US-Intl). Usar `RA(E)` (dead acute en ABC Extended) en macros; ver capa ACCENT.
5. **No agregar comentarios** en el keymap — ni inline (`//`) ni de bloque.

---

## Acentos españoles — capa 7 ACCENT (AltGr virtual)

Mantener pos 37 + letra: A=á, E=é, I=í, O=ó, U=ú, N=ñ, Y=ü; con Shift, mayúscula. Macros `accent_*` (`RA(E)`/`RA(N)`/`RA(U)` + letra) y `mod-morph` `acc_*` por Shift. Layout macOS: ABC Extended. Ya no hay combos de acento.

---

## Combos

- Combos de 2 teclas entre letras llevan `require-prior-idle-ms = <80>` y `timeout-ms = <30>`; escritorios (E+A, Q+D, W+A+D) 150 ms y solo capa 0; macros privadas 200 ms y solo capa 0.
- Edición: T+SPC=DEL, G+SPC=BSPC, B+SPC=ENTER, N+ENT=espacio.
- Símbolos: `;` E+V, `:` R+B, `=` E+T, `-` F+G, `"` W+R, `'` S+F.
- Ratón: T+G / G+B / T+B (izq: L/R/M clic), H+J / J+K / H+K (der), H+M atrás, Y+J adelante.
- Clics en NAV_NUM: pos 6 = L, pos 18 = R, combo 6+18 = M (usables junto al trackball).
- Capa snipe: combo C+B (27+29).

---

## Capas

| ID | Nombre | Activación |
|----|--------|-----------|
| 0 | QWERTY | Base |
| 1 | NAV_NUM | MO1 (pos 40/43) |
| 2 | FN | hold SPC (`lt 2`, pos 41) |
| 3 | BT_layers | `mo 3` desde FN (pos 29) |
| 4 | snipe-layers | combo C+B; el trackball hace scroll |
| 5 | scroll-layers | `mo 5` desde NAV_NUM (pos 16) |
| 6 | BRACKETS | automática con L1+L2 |
| 7 | ACCENT | mantener pos 37 |

Documentación completa: vault `wiki/conceptos/ZMK/CCK-BALL/CCK-BALL - arquitectura y mapeo.md`.

---

## Archivos clave

| Archivo | Propósito |
|---------|-----------|
| `config/cck_ball.keymap` | Keymap principal — encoders L0: historial (`hist_enc`) y escritorios (`desk_encoder`); L1: scroll V/H |
| `config/cck_ball.conf` | Kconfig (BLE, sueño 1 h, Studio, pointing) |
| `config/boards/shields/cck_ball/cck_ball_right.overlay` | Trackball PMW3610, escalas de cursor/scroll por capa |
| `config/private.dtsi` | Macros personales (email/user/pass) — NO commitear cambios |
| `config/boards/shields/cck_ball/cck_ball.dtsi` | Hardware (matrix, encoders) |

`private.dtsi`: usar `git update-index --assume-unchanged config/private.dtsi` para ocultar cambios locales.

---

## Sintaxis ZMK

```c
// Combo estándar
combo_name { timeout-ms = <35>; key-positions = <p1 p2>; bindings = <&behavior>; };

// Macro con dead key (ABC Extended: Option+E = dead acute)
macro_name: macro_name {
    compatible = "zmk,behavior-macro";
    #binding-cells = <0>;
    wait-ms = <50>;
    tap-ms = <40>;
    bindings = <&kp RA(E) &kp A>;  // dead acute + A = á
};

// mod-morph por Shift: acc_a = <&accent_a>, <&accent_a_up>
// &macro_press A B  →  presiona A y B simultáneamente
// &kp A &kp B       →  tapea A luego B (modo default en macros)
```
