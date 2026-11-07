# Onboarding al Comité Ejecutivo del Franco — guía para el agente

> Alcance: integrantes del Comité Ejecutivo y de la Junta Directiva de la AFCE. Si la
> persona que te pidió leer esto no lo es, díselo y no continúes: la organización solo
> invita a sus miembros.

Estás ayudando a una persona del Comité Ejecutivo o de la Junta Directiva de la AFCE
(Liceo Franco-Costarricense) a quedar lista para trabajar con el resto del Comité a través
de ti. La persona **no es técnica**: no conoce GitHub, git, terminales ni *pull requests*,
y no tiene que aprenderlos. Tú haces todo lo que se pueda hacer desde la terminal; ella
solo hace lo que exige a un ser humano (crear su cuenta, confirmar un correo, escanear un
código con el teléfono, teclear un código de 8 letras en el navegador).

## Cómo comportarte durante el onboarding

- Habla en **español**, con frases cortas y sin jerga. Di «tu cuenta», «tu agente»,
  «la carpeta Franco», no «repositorio remoto» ni «autenticación OAuth».
- **Una cosa a la vez.** Anuncia el paso, hazlo o pide lo mínimo, confirma que salió bien y
  pasa al siguiente. No muestres listas de comandos ni salidas largas: resume.
- **Nunca pidas, escribas ni guardes una contraseña**, ni un código de 2FA, ni códigos de
  recuperación. Si la persona te los dicta, dile amablemente que no los necesitas y que no
  debe compartirlos con nadie.
- Antes de ejecutar algo que instale software o cambie el sistema, di en una frase qué vas
  a hacer. Si pide contraseña del equipo (administrador), explícale que la escriba ella en
  la ventana que aparece.
- Guarda tu avance en `~/Franco/.onboarding-estado` (una línea por paso completado) para
  poder **reanudar** si la conversación se corta. Al empezar, lee ese archivo si existe y
  salta lo ya hecho.
- Si algo falla dos veces, no insistas: explica en lenguaje sencillo qué pasó y propón
  escribirle a la Presidencia (Carlos) con el mensaje que tú redactes.

## Paso 0 — Diagnóstico (sin preguntar nada)

Detecta sistema operativo y qué hay instalado: `git`, `gh` (GitHub CLI), `python3`.
Instala lo que falte:

| Sistema | git | gh | python3 |
|---|---|---|---|
| macOS | `xcode-select --install` (abre un diálogo; la persona pulsa «Instalar») o Homebrew | `brew install gh`; sin Homebrew: descarga el `.pkg` de https://cli.github.com y abre el instalador | ya viene |
| Windows | `winget install --id Git.Git -e` | `winget install --id GitHub.cli -e` | `winget install --id Python.Python.3.12 -e` |
| Linux | gestor de paquetes | https://github.com/cli/cli/blob/trunk/docs/install_linux.md | gestor de paquetes |

En Windows, tras instalar Git, los pasos siguientes se hacen en **Git Bash** (viene con
Git), que ejecuta el script del paso 5 sin cambios. Cierra y vuelve a abrir la terminal
tras instalar para que encuentre los programas nuevos.

## Paso 1 — Cuenta de GitHub

Pregunta: «¿Ya tienes una cuenta en GitHub?». Lo normal es que no.

Si no tiene:
1. Pregúntale qué correo quiere usar (el que lee a diario; puede ser el personal).
   Sugiérele un nombre de usuario sencillo: `nombre-apellido` en minúsculas, sin tildes.
2. Abre https://github.com/signup en su navegador (`open`/`start`/`xdg-open`). Explica:
   «Escribe tu correo, inventa una contraseña y guárdala donde guardas las demás; usa el
   nombre de usuario que te sugerí si está libre».
3. GitHub le mandará un código al correo: que lo copie en la página. Las preguntas de
   personalización se pueden saltar; el plan es **Free**.
4. Cuando termine, pídele que te diga su nombre de usuario y anótalo.

Si ya tiene cuenta: pídele el nombre de usuario y sigue.

## Paso 2 — Verificación en dos pasos (obligatoria en la organización)

La organización exige 2FA; sin esto la invitación no se puede aceptar.

1. Abre https://github.com/settings/two_factor_authentication/setup/intro.
2. Recomienda la **aplicación de autenticación**: en iPhone sirve la app «Contraseñas»
   (iOS 17+) o Google/Microsoft Authenticator; en Android, Google Authenticator. Que
   escanee el código QR con el teléfono y escriba en la página el código de 6 dígitos que
   la app le muestre.
3. GitHub ofrece **códigos de recuperación**: que pulse «Download» y los guarde con sus
   documentos importantes. Tú no los ves ni los guardas.
4. Opcional y recomendado: añadir una *passkey* (Touch ID / Face ID / Windows Hello) en
   https://github.com/settings/security para entrar sin teclear códigos.

Confirma: la página de seguridad debe decir «Two-factor authentication: Enabled».

## Paso 3 — Conectar su cuenta contigo (sin contraseñas ni tokens)

Ejecuta:

```sh
gh auth login --hostname github.com --git-protocol https --web
```

Aparece un código de 8 caracteres (`XXXX-XXXX`) y se abre https://github.com/login/device
(si no se abre, ábrelo tú). Dile: «Escribe este código en la página que se abrió y pulsa
Authorize». Cuando termine, comprueba con `gh api user -q .login` que devuelve su usuario
y corre `gh auth setup-git`. Anota el paso.

Así tú usas su cuenta a través de GitHub CLI; no hay que crear llaves ni tokens a mano.

## Paso 4 — Invitación a la organización

La Presidencia tiene que invitar al usuario a la organización `LiceoFrancoCostarricense`.
Comprueba si ya está:

```sh
gh api orgs/LiceoFrancoCostarricense/memberships/$(gh api user -q .login) -q .state
```

- `active` → sigue al paso 5.
- `pending` → hay una invitación esperando: abre https://github.com/LiceoFrancoCostarricense
  y dile que pulse «Join» / «Accept invitation» (también le llegó por correo). Vuelve a
  comprobar.
- Error 404 → aún no hay invitación. Redacta este mensaje para que ella lo envíe a Carlos
  por WhatsApp y **detente aquí** hasta que la invitación llegue:

  > Hola Carlos, ya tengo cuenta de GitHub con verificación en dos pasos. Mi usuario es
  > **@usuario**. ¿Me invitas a la organización del Franco?

  Cuando la persona diga que ya la aceptó, vuelve a comprobar y sigue.

## Paso 5 — Instalación (desatendida)

```sh
mkdir -p ~/Franco
gh repo clone LiceoFrancoCostarricense/skills ~/Franco/skills
bash ~/Franco/skills/onboarding/setup.sh
```

El script clona los otros dos repositorios en `~/Franco/`, configura git con su nombre,
enlaza las skills del Franco en los agentes que encuentre (Claude Code, Codex, Hermes) y
escribe las instrucciones globales. Si falla, lee el mensaje: dice qué falta y cómo
arreglarlo. Repite el script después de arreglarlo; es seguro correrlo varias veces.

Explica en una frase qué quedó: «Ya tienes la carpeta Franco con las decisiones, los
proyectos y la identidad del colegio, y yo ya sé cómo trabajar ahí».

## Paso 6 — Primera propuesta (para aprender el ciclo haciendo)

Lee `~/Franco/comite-ejecutivo/AGENTS.md` (reglas) y `README.md` (áreas y dueños).
Pregúntale a la persona: su nombre como quiere aparecer, su rol en la AFCE o el Liceo
(por ejemplo «Vicepresidencia de la Junta Directiva», «Tesorería», «Vocal») y qué área o
áreas de las del README le interesan. Luego, **tú solo**:

1. En `~/Franco/comite-ejecutivo`: `git switch main && git pull`, rama
   `comunidad/participante-<usuario>`.
2. Agrega su fila a la tabla de `PARTICIPANTES.md` (nombre, rol, `@usuario`, áreas, arnés
   que usa). Sin correos ni teléfonos.
3. `python3 scripts/lint_docs.py` debe pasar.
4. Commit «Agregar a <Nombre> a los participantes», `git push -u origin <rama>`,
   `gh pr create --fill` (la plantilla del PR se completa sola; en «Área y dueño» pon
   `comunidad — @carlosrve`).
5. Dile: «Envié tu primera propuesta para revisión: <enlace>. Carlos la aprobará y desde
   ese momento apareces como participante».

## Paso 7 — Cierre

Explícale, en cinco líneas, qué puede pedirte de ahora en adelante:

- «¿Qué hay pendiente en finanzas?» → lees las áreas y respondes con fuente.
- «Anota esta idea: …» → la escribes con la plantilla y la envías a revisión.
- «Prepara la sesión del miércoles» → agenda a partir de lo abierto en las áreas.
- «Hazme una presentación sobre …» → con la identidad del colegio (`lfc-deck`).
- «¿Qué se decidió sobre …?» → buscas en decisiones y citas el archivo.

Y que **todo lo que proponga pasa por revisión del dueño del área** antes de quedar en
firme: nadie rompe nada. Marca el onboarding como completo en `~/Franco/.onboarding-estado`.

## Nota para Codex

Codex solo trabaja dentro de una carpeta que sea un repositorio git. Si eres Codex y la
conversación empezó fuera de `~/Franco/`, haz los pasos 0–5 desde cualquier carpeta con
`--skip-git-repo-check` si hace falta y, al terminar, pídele a la persona que abra
`~/Franco/comite-ejecutivo` como proyecto para el trabajo diario.

## Si la persona vuelve otro día

Al iniciar una conversación sobre el Franco, la skill `lfc-comite` (ya instalada) te dice
qué hacer: actualizar `~/Franco/comite-ejecutivo` y trabajar ahí. Si la persona cambia de
computadora, basta repetir desde el paso 3 con este mismo documento.
