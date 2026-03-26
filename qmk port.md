 ---                                                                                                                                                                                          Guía de Porteo: zrzChords QMK → ZMK
                                                                                                                                                                                             
  Índice

  2. #2-configuración
  3. #3-capas
  4. #4-hold-taps
  5. #5-combos
  6. #6-especiales
  7. #7-macros
  8. #8-español
  9. #9-rgb
  10. #10-oled
  11. #11-gaps
  12. #12-equivalencias
  13. #13-fases


  ---
  2. Configuración inicial

  corne.conf

  # Tapping term (QMK: TAPPING_TERM = 130)
  CONFIG_ZMK_BEHAVIOR_HOLD_TAP_DEFAULT_FLAVOR="balanced"
  CONFIG_ZMK_BEHAVIOR_HOLD_TAP_TAPPING_TERM_MS=130

  # Combos
  CONFIG_ZMK_COMBO_MAX=50

  # RGB Underglow
  CONFIG_ZMK_RGB_UNDERGLOW=y
  CONFIG_ZMK_RGB_UNDERGLOW_AUTO_OFF_IDLE_MS=10000

  # OLED
  CONFIG_ZMK_DISPLAY=y

  # Mouse
  CONFIG_ZMK_MOUSE=y

  # Split BLE (nice!nano) o USB (Pro Micro)
  CONFIG_ZMK_SPLIT=y
  CONFIG_ZMK_SPLIT_BLE=y

  build.yaml

  ---
  include:
    - board: nice_nano_v2
      shield: corne_left
    - board: nice_nano_v2
      shield: corne_right

  ---
  3. Capas

  Equivalencias básicas

  ┌──────────────────┬───────────────────────────────┐
  │       QMK        │              ZMK              │
  ├──────────────────┼───────────────────────────────┤
  │ ______ / KC_TRNS │ &trans                        │
  ├──────────────────┼───────────────────────────────┤
  │ XXXXXXX          │ &none                         │
  ├──────────────────┼───────────────────────────────┤
  │ MO(1)            │ &mo 1                         │
  ├──────────────────┼───────────────────────────────┤
  │ LT(2, KC_TAB)    │ &lt 2 TAB (o behavior propio) │
  ├──────────────────┼───────────────────────────────┤
  │ TG(1)            │ &tog 1                        │
  ├──────────────────┼───────────────────────────────┤
  │ TO(0)            │ &to 0                         │
  └──────────────────┴───────────────────────────────┘

  Mapa de posiciones Corne (posiciones 0–41)

  ╭──────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
  │  0   │  1  │  2  │  3  │  4  │  5  │   │  6  │  7  │  8  │  9  │ 10  │ 11  │
  ├──────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
  │ 12   │ 13  │ 14  │ 15  │ 16  │ 17  │   │ 18  │ 19  │ 20  │ 21  │ 22  │ 23  │
  ├──────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
  │ 24   │ 25  │ 26  │ 27  │ 28  │ 29  │   │ 30  │ 31  │ 32  │ 33  │ 34  │ 35  │
  ╰──────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────┴─────╯
                     │ 36  │ 37  │ 38  │   │ 39  │ 40  │ 41  │
                     ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯

  Mapeo base:
   0=GUI_ESC  1=Q   2=W   3=E   4=R   5=T    6=Y   7=U   8=I   9=O  10=P  11=BSPC
  12=L2_TAB  13=A  14=S  15=D  16=F  17=G   18=H  19=J  20=K  21=L  22=RCTL 23=GUI_QT
  24=LSFT   25=Z  26=X  27=C  28=V  29=B   30=N  31=M  32=,  33=.  34=RALT 35=RSFT
  36=LCTL   37=MO1 38=SPC                  39=RET 40=MO1 41=L2_PSCR

  Esqueleto de capas en corne.keymap

  #include <behaviors.dtsi>
  #include <dt-bindings/zmk/keys.h>
  #include <dt-bindings/zmk/rgb.h>
  #include <dt-bindings/zmk/mouse.h>

  / {
      keymap {
          compatible = "zmk,keymap";

          // LAYER 0 — BASE QWERTY
          default_layer {
              bindings = <
  // ╭──────────┬──────┬──────┬──────┬──────┬──────╮   ╭──────┬──────┬──────┬──────┬──────┬──────╮
      &gui_esc   &kp Q  &kp W  &kp E  &kp R  &kp T      &kp Y  &kp U  &kp I  &kp O  &kp P  &bspc_del
  // ├──────────┼──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┼──────┤
      &l2_tab    &kp A  &kp S  &kp D  &kp F  &alt_g     &kp H  &kp J  &kp K  &kp L  &kp RCTRL &gui_qt
  // ├──────────┼──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┼──────┤
      &kp LSHFT  &kp Z  &kp X  &kp C  &kp V  &kp B      &kp N  &kp M  &kp COMMA &kp DOT &kp RALT &kp RSHFT
  // ╰──────────┴──────┴──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┴──────┴──────╯
                               &kp LCTRL &mo 1 &kp SPACE  &kp RET &mo 1 &l2_pscr
  //                          ╰──────┴──────┴──────╯   ╰──────┴──────┴──────╯
              >;
          };

          // LAYER 1 — NAV (izq) + NUMPAD (der)
          nav_layer {
              bindings = <
      &dev_home  &kp UP    &dev_end  &trans  &trans   &trans      &trans  &kp N7  &kp N8  &kp N9  &trans  &trans
      &win_swap  &kp LEFT  &kp DOWN  &kp RIGHT &trans &kp PG_UP   &trans  &kp N4  &kp N5  &kp N6  &trans  &trans
      &trans     &wd_left  &trans    &wd_rght  &trans &kp PG_DN   &trans  &kp N1  &kp N2  &kp N3  &trans  &trans
                                      &trans  &trans  &trans       &trans  &trans  &kp N0
              >;
          };

          // LAYER 2 — MOUSE (izq) + FUNCIÓN (der)
          fn_layer {
              bindings = <
      &tog_os    &trans          &mmv MOVE_UP    &trans           &mkp MCLK  &msc SCRL_UP    &kp F7  &kp F8  &kp F9  &kp F10  &kp F11  &kp F12
      &trans     &mmv MOVE_LEFT  &mmv MOVE_DOWN  &mmv MOVE_RIGHT  &trans     &msc SCRL_DOWN  &kp F1  &kp F2  &kp F3  &kp F4   &kp F5   &kp F6
      &kp PAUSE_BREAK &kp K_BACK &kp K_FORWARD  &trans           &trans     &trans          &trans  &trans  &trans  &trans   &trans   &trans
                                                 &trans           &trans     &trans          &tog 2  &trans  &trans
              >;
          };
      };
  };

  ---
  4. Mod-taps y Layer-taps

  En ZMK los mod-taps se definen como behaviors hold-tap personalizados en el nodo behaviors.

  ▎ Clave: L2_TAB y ALT_G en QMK tienen PERMISSIVE_HOLD=false para no interferir con combos (E+T, G+SPC). El equivalente ZMK es flavor = "tap-unless-interrupted".

  / {
      behaviors {

          // GUI_ESC: tap=ESC, hold=LGUI
          gui_esc: gui_esc {
              compatible = "zmk,behavior-hold-tap";
              #binding-cells = <0>;
              tapping-term-ms = <130>;
              flavor = "balanced";
              bindings = <&kp LGUI>, <&kp ESC>;
          };

          // L2_TAB: tap=TAB, hold=Layer2
          // flavor conservador — protege combos E+T (SELWORD), etc.
          l2_tab: l2_tab {
              compatible = "zmk,behavior-hold-tap";
              #binding-cells = <0>;
              tapping-term-ms = <130>;
              quick-tap-ms = <100>;
              flavor = "tap-unless-interrupted";
              bindings = <&mo 2>, <&kp TAB>;
          };

          // ALT_G: tap=G, hold=LALT
          // flavor conservador — protege combos G+SPC (BSPC)
          alt_g: alt_g {
              compatible = "zmk,behavior-hold-tap";
              #binding-cells = <0>;
              tapping-term-ms = <130>;
              quick-tap-ms = <100>;
              flavor = "tap-unless-interrupted";
              bindings = <&kp LALT>, <&kp G>;
          };

          // GUI_QT: tap=', hold=RGUI
          gui_qt: gui_qt {
              compatible = "zmk,behavior-hold-tap";
              #binding-cells = <0>;
              tapping-term-ms = <130>;
              flavor = "balanced";
              bindings = <&kp RGUI>, <&kp SQT>;
          };

          // L2_PSCR: tap=PRTSC, hold=Layer2
          l2_pscr: l2_pscr {
              compatible = "zmk,behavior-hold-tap";
              #binding-cells = <0>;
              tapping-term-ms = <130>;
              quick-tap-ms = <100>;
              flavor = "tap-unless-interrupted";
              bindings = <&mo 2>, <&kp PSCRN>;
          };

          // SHIFT+BSPC → DEL (mod-morph)
          bspc_del: backspace_delete {
              compatible = "zmk,behavior-mod-morph";
              #binding-cells = <0>;
              bindings = <&kp BSPC>, <&kp DEL>;
              mods = <(MOD_LSFT|MOD_RSFT)>;
          };
      };
  };

  ---
  5. Combos

  En ZMK los combos se definen por posición en la matrix (0–41). Timeout equivalente a COMBO_TERM=35.

  / {
      combos {
          compatible = "zmk,combos";

          // ───── CONTROL ─────────────────────────────────
          combo_selword  { timeout-ms = <35>; key-positions = <3 5>;       bindings = <&sel_word>; };
          combo_enter    { timeout-ms = <35>; key-positions = <29 38>;     bindings = <&kp RET>; };
          combo_bspc     { timeout-ms = <35>; key-positions = <17 38>;     bindings = <&kp BSPC>; };
          combo_del      { timeout-ms = <35>; key-positions = <5 38>;      bindings = <&kp DEL>; };
          combo_space    { timeout-ms = <35>; key-positions = <30 39>;     bindings = <&kp SPACE>; };
          combo_caps     { timeout-ms = <35>; key-positions = <24 25 26>;  bindings = <&kp CAPS>; };
          combo_app      { timeout-ms = <35>; key-positions = <30 31>;     bindings = <&kp K_APP>; };
          combo_comma    { timeout-ms = <35>; key-positions = <14 26>;     bindings = <&kp COMMA>; };
          combo_dot      { timeout-ms = <35>; key-positions = <15 27>;     bindings = <&kp DOT>; };
          combo_semi     { timeout-ms = <35>; key-positions = <16 28>;     bindings = <&kp SEMI>; };

          // ───── BRACKETS (ancla ESC=pos 0) ──────────────
          combo_lbrc     { timeout-ms = <35>; key-positions = <0 1>;       bindings = <&kp LBKT>; };
          combo_rbrc     { timeout-ms = <35>; key-positions = <0 2>;       bindings = <&kp RBKT>; };
          combo_lcbr     { timeout-ms = <35>; key-positions = <0 13>;      bindings = <&kp LBRC>; };
          combo_rcbr     { timeout-ms = <35>; key-positions = <0 14>;      bindings = <&kp RBRC>; };
          combo_lprn     { timeout-ms = <35>; key-positions = <0 25>;      bindings = <&kp LPAR>; };
          combo_rprn     { timeout-ms = <35>; key-positions = <0 26>;      bindings = <&kp RPAR>; };

          // ───── SÍMBOLOS ─────────────────────────────────
          combo_slash    { timeout-ms = <35>; key-positions = <4 15 26>;   bindings = <&kp FSLH>; };
          combo_bslash   { timeout-ms = <35>; key-positions = <2 15 28>;   bindings = <&kp BSLH>; };
          combo_pipe     { timeout-ms = <35>; key-positions = <3 15 27>;   bindings = <&kp PIPE>; };
          combo_caret    { timeout-ms = <35>; key-positions = <3 14 16>;   bindings = <&kp CARET>; };
          combo_lt       { timeout-ms = <35>; key-positions = <3 14 27>;   bindings = <&kp LT>; };
          combo_gt       { timeout-ms = <35>; key-positions = <3 16 27>;   bindings = <&kp GT>; };
          combo_grave    { timeout-ms = <35>; key-positions = <4 17>;      bindings = <&kp GRAVE>; };
          combo_acute    { timeout-ms = <35>; key-positions = <4 15>;      bindings = <&macro_acute>; };
          combo_comment  { timeout-ms = <35>; key-positions = <15 16 26>;  bindings = <&dev_comment>; };
          combo_tilde    { timeout-ms = <35>; key-positions = <1 16>;      bindings = <&kp TILDE>; };
          combo_quote    { timeout-ms = <35>; key-positions = <2 4>;       bindings = <&kp SQT>; };
          combo_hash     { timeout-ms = <35>; key-positions = <3 4 5>;     bindings = <&kp HASH>; };
          combo_perc     { timeout-ms = <35>; key-positions = <2 28>;      bindings = <&kp PRCNT>; };
          combo_excl     { timeout-ms = <35>; key-positions = <2 3 16>;    bindings = <&kp EXCL>; };
          combo_ques     { timeout-ms = <35>; key-positions = <4 14 15>;   bindings = <&kp QMARK>; };
          combo_dollar   { timeout-ms = <35>; key-positions = <4 26 27>;   bindings = <&kp DLLR>; };
          combo_at       { timeout-ms = <35>; key-positions = <2 15 16>;   bindings = <&kp AT>; };
          combo_amp      { timeout-ms = <35>; key-positions = <15 17>;     bindings = <&kp AMPS>; };
          combo_star     { timeout-ms = <35>; key-positions = <27 28 29>;  bindings = <&kp STAR>; };
          combo_equal    { timeout-ms = <35>; key-positions = <4 5>;       bindings = <&kp EQUAL>; };
          combo_minus    { timeout-ms = <35>; key-positions = <16 17>;     bindings = <&kp MINUS>; };
          combo_under    { timeout-ms = <35>; key-positions = <28 29>;     bindings = <&kp UNDER>; };
          combo_plus     { timeout-ms = <35>; key-positions = <15 16 17>;  bindings = <&kp PLUS>; };

          // ───── ESPAÑOL ─────────────────────────────────
          combo_tilde_a  { timeout-ms = <35>; key-positions = <13 25>;     bindings = <&tilde_a>; };
          // NOTA: E(3)+D(15) ya usado por combo_pipe (3+15+27). No hay conflicto
          // porque combo_pipe requiere 3 teclas. Sin embargo verificar en práctica.
          combo_tilde_e  { timeout-ms = <50>; key-positions = <3 15>;      bindings = <&tilde_e>; };
          combo_tilde_i  { timeout-ms = <35>; key-positions = <8 20>;      bindings = <&tilde_i>; };
          combo_tilde_o  { timeout-ms = <35>; key-positions = <9 21>;      bindings = <&tilde_o>; };
          combo_tilde_u  { timeout-ms = <35>; key-positions = <7 19>;      bindings = <&tilde_u>; };
          combo_enie     { timeout-ms = <35>; key-positions = <19 30>;     bindings = <&enie>; };

          // ───── MISC ─────────────────────────────────────
          combo_email_gmail { timeout-ms = <50>; key-positions = <13 14 15 16>; bindings = <&macro_email_gmail>; };
          combo_email_work  { timeout-ms = <50>; key-positions = <18 19 20 21>; bindings = <&macro_email_work>; };
          combo_username    { timeout-ms = <50>; key-positions = <4 7 14>;      bindings = <&macro_username>; };
          combo_password    { timeout-ms = <50>; key-positions = <2 10>;        bindings = <&macro_password>; };
      };
  };

  ---
  6. Comportamientos especiales

  Layer Lock

  ZMK no lo tiene nativo. Opciones:

  - &tog 2 en la posición de LLOCK — toggle simple de la capa
  - Módulo externo zmk-layer-lock de urob (recomendado para fidelidad completa):

  # west.yml
  manifest:
    remotes:
      - name: zmkfirmware
        url-base: https://github.com/zmkfirmware
      - name: urob
        url-base: https://github.com/urob
    projects:
      - name: zmk
        remote: zmkfirmware
        revision: main
      - name: zmk-layer-lock
        remote: urob
        revision: main

  Select Word

  Aproximación con macro:
  macros {
      sel_word: sel_word {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings =
              <&macro_press &kp LCTRL &kp LSHFT>,
              <&kp RIGHT>,
              <&macro_release &kp LCTRL &kp LSHFT>;
      };
  };

  OS-aware shortcuts (solo Windows, por defecto)

  macros {
      dev_home: dev_home {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&kp HOME>;
      };
      dev_end: dev_end {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&kp END>;
      };
      wd_left: wd_left {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp LCTRL>, <&kp LEFT>, <&macro_release &kp LCTRL>;
      };
      wd_rght: wd_rght {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp LCTRL>, <&kp RIGHT>, <&macro_release &kp LCTRL>;
      };
      dev_comment: dev_comment {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp LCTRL>, <&kp FSLH>, <&macro_release &kp LCTRL>;
      };
      win_swap: win_swap {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp LALT>, <&kp TAB>, <&macro_release &kp LALT>;
      };
      // Toggle OS: en ZMK implementar como tog de una capa de OS alternativa
      tog_os: tog_os {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&tog 3>;   // Layer 3 = modo macOS (con shortcuts Mac)
      };
  };

  ---
  7. Macros de texto

  macros {
      macro_email_gmail: macro_email_gmail {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          // Reemplazar con las teclas de tu email real
          bindings = <&kp U &kp S &kp E &kp R &kp AT &kp G &kp M &kp A &kp I &kp L &kp DOT &kp C &kp O &kp M>;
      };
      macro_email_work: macro_email_work {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = < /* teclas del email de trabajo */ >;
      };
      macro_username: macro_username {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = < /* teclas del username */ >;
      };
      // ADVERTENCIA: no guardes contraseñas reales — el repo es público
      macro_password: macro_password {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = < /* omitir o usar placeholder */ >;
      };
  };

  ---
  8. Caracteres españoles (Unicode — Windows Alt+Numpad)

  Replica la lógica de keycode_handler.c (Windows: Alt + códigos numpad).

  macros {
      // á = Alt+0160
      tilde_a: tilde_a {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp RALT>,
                     <&macro_tap &kp KP_N0 &kp KP_N1 &kp KP_N6 &kp KP_N0>,
                     <&macro_release &kp RALT>;
      };
      // é = Alt+0130
      tilde_e: tilde_e {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp RALT>,
                     <&macro_tap &kp KP_N0 &kp KP_N1 &kp KP_N3 &kp KP_N0>,
                     <&macro_release &kp RALT>;
      };
      // í = Alt+0161
      tilde_i: tilde_i {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp RALT>,
                     <&macro_tap &kp KP_N0 &kp KP_N1 &kp KP_N6 &kp KP_N1>,
                     <&macro_release &kp RALT>;
      };
      // ó = Alt+0163
      tilde_o: tilde_o {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp RALT>,
                     <&macro_tap &kp KP_N0 &kp KP_N1 &kp KP_N6 &kp KP_N3>,
                     <&macro_release &kp RALT>;
      };
      // ú = Alt+0150
      tilde_u: tilde_u {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp RALT>,
                     <&macro_tap &kp KP_N0 &kp KP_N1 &kp KP_N5 &kp KP_N0>,
                     <&macro_release &kp RALT>;
      };
      // ñ = Alt+0164
      enie: enie {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp RALT>,
                     <&macro_tap &kp KP_N0 &kp KP_N1 &kp KP_N6 &kp KP_N4>,
                     <&macro_release &kp RALT>;
      };
      // ´ = Alt+0180
      macro_acute: macro_acute {
          compatible = "zmk,behavior-macro";
          #binding-cells = <0>;
          bindings = <&macro_press &kp RALT>,
                     <&macro_tap &kp KP_N0 &kp KP_N1 &kp KP_N8 &kp KP_N0>,
                     <&macro_release &kp RALT>;
      };
  };

  Tabla de códigos Alt (Windows)

  ┌──────┬──────────┬─────────────────────────┐
  │ Char │ Alt Code │     ZMK KP sequence     │
  ├──────┼──────────┼─────────────────────────┤
  │ á    │ 0160     │ KP_N0 KP_N1 KP_N6 KP_N0 │
  ├──────┼──────────┼─────────────────────────┤
  │ é    │ 0130     │ KP_N0 KP_N1 KP_N3 KP_N0 │
  ├──────┼──────────┼─────────────────────────┤
  │ í    │ 0161     │ KP_N0 KP_N1 KP_N6 KP_N1 │
  ├──────┼──────────┼─────────────────────────┤
  │ ó    │ 0163     │ KP_N0 KP_N1 KP_N6 KP_N3 │
  ├──────┼──────────┼─────────────────────────┤
  │ ú    │ 0150     │ KP_N0 KP_N1 KP_N5 KP_N0 │
  ├──────┼──────────┼─────────────────────────┤
  │ ñ    │ 0164     │ KP_N0 KP_N1 KP_N6 KP_N4 │
  ├──────┼──────────┼─────────────────────────┤
  │ Á    │ 0193     │ KP_N0 KP_N1 KP_N9 KP_N3 │
  ├──────┼──────────┼─────────────────────────┤
  │ É    │ 0201     │ KP_N0 KP_N2 KP_N0 KP_N1 │
  ├──────┼──────────┼─────────────────────────┤
  │ Í    │ 0205     │ KP_N0 KP_N2 KP_N0 KP_N5 │
  ├──────┼──────────┼─────────────────────────┤
  │ Ó    │ 0211     │ KP_N0 KP_N2 KP_N1 KP_N1 │
  ├──────┼──────────┼─────────────────────────┤
  │ Ú    │ 0218     │ KP_N0 KP_N2 KP_N1 KP_N8 │
  ├──────┼──────────┼─────────────────────────┤
  │ Ñ    │ 0209     │ KP_N0 KP_N2 KP_N0 KP_N9 │
  └──────┴──────────┴─────────────────────────┘

  ▎ Para mayúsculas (Á, É, etc.) se necesitan macros adicionales, o bien detectar Shift con un behavior mod-morph wrapping la macro de minúscula.

  ---
  9. RGB / Underglow

  # corne.conf
  CONFIG_ZMK_RGB_UNDERGLOW=y
  CONFIG_ZMK_RGB_UNDERGLOW_ON_START=y
  CONFIG_ZMK_RGB_UNDERGLOW_AUTO_OFF_IDLE=y
  CONFIG_ZMK_RGB_UNDERGLOW_AUTO_OFF_IDLE_MS=10000

  Para cambiar color por capa, usar macros que disparen &rgb_ug RGB_COLOR_HSB(h,s,b) al activar la capa (via layer-change event o binding en el keymap).

  ▎ Limitación importante: ZMK para Corne no soporta per-key RGB en el firmware estándar. La funcionalidad ledmap[][42][3] de rgb_indicators.c no tiene equivalente directo. Solo se puede   
  controlar el underglow (tira de LEDs) por capa completa.

  ---
  10. OLED

  # corne.conf
  CONFIG_ZMK_DISPLAY=y
  CONFIG_ZMK_DISPLAY_WORK_QUEUE_DEDICATED=y

  ┌───────────────────────────┬─────────────────────────────────────────────┐
  │        Función QMK        │               Equivalente ZMK               │
  ├───────────────────────────┼─────────────────────────────────────────────┤
  │ oled_render_layer_state() │ Widget zmk,widget-layer-status (builtin)    │
  ├───────────────────────────┼─────────────────────────────────────────────┤
  │ oled_render_logo()        │ Widget zmk,widget-logo (builtin)            │
  ├───────────────────────────┼─────────────────────────────────────────────┤
  │ oled_render_keylog()      │ No disponible — requiere widget Zephyr en C │
  └───────────────────────────┴─────────────────────────────────────────────┘

  El display de capa y logo funcionan automáticamente. El keylog requiere implementación custom en C (más complejo que en QMK). Recomendación: dejar para fase final.

  ---
  11. Funciones sin equivalente directo

  ┌─────────────────────────┬───────────────────┬────────────────────────────┐
  │       Función QMK       │    Estado ZMK     │          Solución          │
  ├─────────────────────────┼───────────────────┼────────────────────────────┤
  │ LLOCK Layer Lock        │ No nativo         │ Módulo urob/zmk-layer-lock │
  ├─────────────────────────┼───────────────────┼────────────────────────────┤
  │ SELWORD Select Word     │ No nativo         │ Macro Ctrl+Shift+→ básica  │
  ├─────────────────────────┼───────────────────┼────────────────────────────┤
  │ WIN_SWAP sticky Alt+Tab │ Parcial           │ Macro Alt+Tab simple       │
  ├─────────────────────────┼───────────────────┼────────────────────────────┤
  │ Toggle macOS/Windows    │ No nativo         │ Capa separada por OS       │
  ├─────────────────────────┼───────────────────┼────────────────────────────┤
  │ ledmap per-key RGB      │ No disponible     │ Solo underglow por capa    │
  ├─────────────────────────┼───────────────────┼────────────────────────────┤
  │ Keylog OLED             │ Requiere widget C │ Implementar en fase final  │
  ├─────────────────────────┼───────────────────┼────────────────────────────┤
  │ NumLock automático      │ No nativo         │ Tecla KP_NUM manual        │
  └─────────────────────────┴───────────────────┴────────────────────────────┘

  ---
  12. Mapa de equivalencias QMK → ZMK

  Keycodes

  ┌───────────────────────┬─────────────────────────────┐
  │          QMK          │             ZMK             │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_LCTL               │ LCTRL                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_LSFT               │ LSHFT                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_LALT               │ LALT                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_LGUI               │ LGUI                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_BSPC               │ BSPC                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_ENT                │ RET                         │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_SPC                │ SPACE                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_CAPS               │ CAPS                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_PSCR               │ PSCRN                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_PAUSE              │ PAUSE_BREAK                 │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_WBAK               │ K_BACK                      │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_WFWD               │ K_FORWARD                   │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_APP                │ K_APP                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_LBRC               │ LBKT                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_RBRC               │ RBKT                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_LCBR               │ LBRC                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_RCBR               │ RBRC                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_LPRN               │ LPAR                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_RPRN               │ RPAR                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_SLSH               │ FSLH                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_BSLS               │ BSLH                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_SCLN               │ SEMI                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_QUOT               │ SQT                         │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_GRV                │ GRAVE                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_TILD               │ TILDE                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_PIPE               │ PIPE                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_CIRC               │ CARET                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_AMPR               │ AMPS                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_ASTR               │ STAR                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_EXLM               │ EXCL                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_QUES               │ QMARK                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_DLR                │ DLLR                        │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_PERC               │ PRCNT                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_EQL                │ EQUAL                       │
  ├───────────────────────┼─────────────────────────────┤
  │ KC_UNDS               │ UNDER                       │
  ├───────────────────────┼─────────────────────────────┤
  │ MS_BTN3               │ mkp MCLK                    │
  ├───────────────────────┼─────────────────────────────┤
  │ MS_UP/DOWN/LEFT/RIGHT │ mmv MOVE_UP/DOWN/LEFT/RIGHT │
  ├───────────────────────┼─────────────────────────────┤
  │ MS_WHLU/WHLD          │ msc SCRL_UP/SCRL_DOWN       │
  └───────────────────────┴─────────────────────────────┘

  Behaviors

  ┌──────────────┬─────────────────────────────────────┐
  │     QMK      │                 ZMK                 │
  ├──────────────┼─────────────────────────────────────┤
  │ LGUI_T(KC_X) │ &mt LGUI X o behavior personalizado │
  ├──────────────┼─────────────────────────────────────┤
  │ LT(n, KC_X)  │ &lt n X o behavior personalizado    │
  ├──────────────┼─────────────────────────────────────┤
  │ MO(n)        │ &mo n                               │
  ├──────────────┼─────────────────────────────────────┤
  │ TG(n)        │ &tog n                              │
  ├──────────────┼─────────────────────────────────────┤
  │ TO(n)        │ &to n                               │
  ├──────────────┼─────────────────────────────────────┤
  │ OSL(n)       │ &sl n                               │
  ├──────────────┼─────────────────────────────────────┤
  │ KC_TRNS      │ &trans                              │
  ├──────────────┼─────────────────────────────────────┤
  │ XXXXXXX      │ &none                               │
  ├──────────────┼─────────────────────────────────────┤
  │ RGB_TOG      │ &rgb_ug RGB_TOG                     │
  └──────────────┴─────────────────────────────────────┘

  ---
  13. Orden de implementación sugerido

  Fase 1 — Esqueleto funcional

  1. Crear repo zmk-config desde el template
  2. Configurar build.yaml con el board/shield
  3. Las 3 capas con teclas simples (sin combos ni behaviors personalizados)
  4. Compilar con GitHub Actions — verificar que no hay errores
  5. Flashear y confirmar teclas básicas

  Fase 2 — Hold-taps

  6. Implementar gui_esc, l2_tab, alt_g, l2_pscr, gui_qt, bspc_del
  7. Ajustar tapping-term-ms a gusto
  8. Verificar activación de Layer 2

  Fase 3 — Combos de control

  9. Implementar los 10 combos de combos_control
  10. Verificar posiciones en la matrix física
  11. Ajustar timeouts si hay falsos positivos/negativos

  Fase 4 — Combos de símbolos

  12. Brackets (ancla ESC pos 0)
  13. Símbolos de 2 y 3 teclas
  14. Probar cada uno individualmente

  Fase 5 — Navegación y macros

  15. Macros de nav: dev_home, dev_end, wd_left, wd_rght, dev_comment, win_swap
  16. Layer Lock (módulo externo o &tog)
  17. sel_word básico

  Fase 6 — Español

  18. Macros de acentos (Alt+numpad)
  19. Combos españoles
  20. Macros de texto (email, username)

  Fase 7 — RGB y OLED

  21. Underglow con color por capa
  22. OLED básico (capa + logo)
  23. Widget keylog si se desea (opcional, complejo)

  ---
  Referencias

  - https://zmk.dev/docs
  - https://nickcoutsos.github.io/keymap-editor/ — editor visual online
  - https://github.com/zmkfirmware/zmk-config — punto de partida
  - https://github.com/urob/zmk-config — módulos: layer-lock, timeless hold-tap y más
  - https://zmk.dev/docs/hardware — docs del shield oficial
  - https://zmk.dev/docs/behaviors/hold-tap
  - https://zmk.dev/docs/features/combos
  - https://zmk.dev/docs/behaviors/macros

  ---
  El documento está completo. Puntos clave del porteo:

  1. Los combos son el corazón del keymap — requieren traducir posiciones de tecla a índices 0–41 de la matrix. El mapa de posiciones que incluye el documento es esencial.
  2. L2_TAB y ALT_G necesitan flavor = "tap-unless-interrupted" (no el default "balanced") para que los combos E+T y G+SPC no se activen prematuramente.
  3. Layer Lock y Select Word no tienen equivalente nativo — el módulo de urob resuelve el primero, y el segundo se puede aproximar con una macro simple.
  4. Per-key RGB (ledmap[][42][3]) no es soportado por ZMK en Corne — solo underglow por capa.
  5. Los acentos españoles se replican con las mismas secuencias Alt+numpad que ya usa el QMK para Windows.