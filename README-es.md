# 🛠️ MellonKey◉fs◉  — Portfolio Project

> MellonKey◉fs◉, inspirado en la puerta de Moria.

v1.0.0 *MellonKeySimple* - Documentación

Documento redactado originalmente en español por https://github.com/fabiola-serralunga. Traducido al inglés mediante un modelo de lenguaje grande (LLM) y alojado en README.md

🌐 https://testpages-simple.pages.dev -- sitio web de *MellonKeySimple*

🔗 https://testpages-simple.pages.dev/deliver -- entrega el digital asset mediante Soulbound Token (SBT) - visibiliza el Certificado de Adquisición - muestra estado del contrato

🔗 https://base-sepolia.blockscout.com/address/0x9DF53bB4F3A56b7927560C2638074f55F5DDD20E?tab=tokens_nfts -- para visualizar los SBT acuñados en el proyecto en su entorno de prueba. 

📜⛓️🔷 `TestnetSBTGaladrielNenya.sol` -- Se evitó un nombre abstracto a pesar de ser un contrato base. Esto permite una trazabilidad visual e inmediata que relaciona sin ambigüedades la versión, el entorno y la implementación concreta del contrato para este caso puntual que es un proyecto de portfolio.

Documento redactado originalmente en español por la desarrolladora. Traducido al inglés mediante un modelo de lenguaje grande (LLM).

**MellonKey◉fs◉** es un ecosistema de desarrollo creado como portfolio técnico. Reúne las Pruebas de Concepto (PoC) y los resultados de un proceso de diseño iterativo originado en el Proyecto ·sarocha·. Durante la fase de investigación, este último evolucionó hacia una arquitectura que se ajustaba de forma más eficiente a sus objetivos y alcances finales, permitiendo que MellonKey◉fs◉ permanezca como un valioso repositorio de soluciones, dilemas resueltos y casos de uso transferibles a otros proyectos.

En esta versión 1.0.0, el enfoque principal es desarrollar un flujo integral para la entrega de un activo digital (digital asset), garantizando los registros inmutables de procedencia, de autoría y de un certificado de adquisición mediante tecnología Blockchain.

Para evaluar la integración de los componentes en un entorno controlado y analizar cómo interactúan en tiempo real, se estructuró la colección inicial *The Three Elven Rings* con tres activos digitales que responden a la temática, donde esta sirve como marco conceptual para validar el sistema.

Con el propósito de validar el comportamiento base del sistema antes de escalar la infraestructura, el despliegue se ejecutó mediante un contrato inteligente ERC-721 individual para cada activo. En esta etapa temprana de desarrollo, se prescindió deliberadamente del uso de patrones complejos como Factory o Registry. 


| | |
|---|---|
| **Proyecto** | MellonKey◉fs◉  (v.1.0.0 *MellonKeySimple*). |
| **Galería** | The Three Elven Rings |
| **Qué es** | Soulbound Tokens (SBT) que certifican la adquisición de un activo digital y funcionan como llave de acceso a su archivo original |
| **Red** | Base Sepolia Testnet (chainId 84532 / `0x14a34`) |
| **Estándar del token** | ERC-721, open edition, non-transferable (soulbound) |
| **Contrato base** | `TestnetSBTGaladrielNenya` — Solidity ^0.8.20 + OpenZeppelin v5 |
| **Infraestructura web** | Cloudflare Pages (sitio estático + Pages Function sobre el runtime de Workers) + Cloudflare R2 |
| **Metadatos** | JSON alojado en Arweave (compatible con IPFS) |
| **Estado** | v1.0.0 en testnet — Ediciones activas: *Galadriel and Nenya* and *Mithrandir and Narya*. Edición establecida por diseño como "coming soon" con *Elrond and Vilya* |

---

## Índice
- [🛠️ MellonKey◉fs◉  — Portfolio Project](#️-mellonkeyfs---portfolio-project)
  - [Índice](#índice)
  - [1. ¿Qué es MellonKey◉fs◉ ?](#1-qué-es-mellonkeyfs-)
  - [2. Principios de diseño y fundamentación](#2-principios-de-diseño-y-fundamentación)
  - [3. Arquitectura general: Smart Contracts y Validación de Flujos](#3-arquitectura-general-smart-contracts-y-validación-de-flujos)
    - [3.1. Entorno de Pruebas](#31-entorno-de-pruebas)
    - [3.2. Desafíos Técnicos e Investigación (PoC)](#32-desafíos-técnicos-e-investigación-poc)
  - [4. El contrato base: `TestnetSBTGaladrielNenya`](#4-el-contrato-base-testnetsbtgaladrielnenya)
    - [4.1 Qué es y cómo se usa](#41-qué-es-y-cómo-se-usa)
    - [4.2 ERC-721 - Cuando el certificado amerita un dueño único](#42-erc-721---cuando-el-certificado-amerita-un-dueño-único)
    - [4.3 Características del contrato](#43-características-del-contrato)
    - [4.4 Los datos que se fijan al desplegar (constructor)](#44-los-datos-que-se-fijan-al-desplegar-constructor)
    - [4.5 El registro que deja cada mint](#45-el-registro-que-deja-cada-mint)
    - [4.6 La interfaz mínima que consume el sitio](#46-la-interfaz-mínima-que-consume-el-sitio)
  - [5. La metadata de la edición (JSON en Arweave)](#5-la-metadata-de-la-edición-json-en-arweave)
    - [5.1 Qué contiene](#51-qué-contiene)
    - [5.2 Arweave (e IPFS también)](#52-arweave-e-ipfs-también)
    - [5.3 Un solo JSON para toda la open edition](#53-un-solo-json-para-toda-la-open-edition)
  - [6. `editions.json`](#6-editionsjson)
    - [6.1 El problema que resuelve](#61-el-problema-que-resuelve)
    - [6.2 Quién lee qué](#62-quién-lee-qué)
    - [6.3 El ciclo completo para publicar una edición nueva](#63-el-ciclo-completo-para-publicar-una-edición-nueva)
  - [7. La página de minteo: `index.html`](#7-la-página-de-minteo-indexhtml)
    - [7.1 El minteo se hace desde la página web, sin servidor](#71-el-minteo-se-hace-desde-la-página-web-sin-servidor)
    - [7.2 Cómo funciona por dentro](#72-cómo-funciona-por-dentro)
    - [7.3 El flujo del claim, paso a paso](#73-el-flujo-del-claim-paso-a-paso)
  - [8. Uso del SBT: `deliver.html` y entrega del archivo](#8-uso-del-sbt-deliverhtml-y-entrega-del-archivo)
    - [8.1 La acuñación no requiere servidor; la entrega sí](#81-la-acuñación-no-requiere-servidor-la-entrega-sí)
    - [8.2 Autorización de la entrega](#82-autorización-de-la-entrega)
    - [8.3 Estado actual del archivo: R2 sin encriptar (y la etapa siguiente)](#83-estado-actual-del-archivo-r2-sin-encriptar-y-la-etapa-siguiente)
    - [8.4 Panel "Certificate of Acquisition"](#84-panel-certificate-of-acquisition)
    - [8.5 Panel público en `deliver.html`](#85-panel-público-en-deliverhtml)
  - [9. Red de prueba y financiamiento con faucets](#9-red-de-prueba-y-financiamiento-con-faucets)
  - [10. Visualización completa](#10-visualización-completa)
    - [10.1 Lo observado](#101-lo-observado)
    - [10.2 Explorador Blockscout - Chain Base Sepolia](#102-explorador-blockscout---chain-base-sepolia)
    - [10.3 Marketplaces](#103-marketplaces)
  - [11. Tabla de decisiones: qué se eligió y por qué](#11-tabla-de-decisiones-qué-se-eligió-y-por-qué)
  - [12. Estado actual y hoja de ruta](#12-estado-actual-y-hoja-de-ruta)
    - [**Estado actual (v1.0.0, testnet):**](#estado-actual-v100-testnet)
    - [**Hoja de ruta:**](#hoja-de-ruta)
  - [13. Estructura del repositorio](#13-estructura-del-repositorio)
  - [14. Glosario mínimo](#14-glosario-mínimo)

---

## 1. ¿Qué es MellonKey◉fs◉ ?

MellonKey◉fs◉  es un sistema que registra en blockchain la entrega de un **certificado de adquisición** de un activo digital dando acceso a éste a través de un token. Cada activo digital se edita como una tirada abierta (open edition), y cada copia adquirida recibe un certificado con sus datos — artista, obra, descripción, id de token, edición, fecha — y un único dueño. Quien adquiere un MellonKey SBT recibe un certificado on-chain, no transferible, que prueba que esa persona adquirió un activo digital de determinado creador, y que además funciona físicamente como llave para descargar el archivo original. 

Tengamos presente la idea dicha de forma llana: en este caso, el MellonKey SBT *no es el digital asset*, sino su certificado de adquisición y su forma de acceder a ella. En otro desarrollo vigente se está codificando para que el token sea el digital asset en sí mismo con su correspondiente certificado de adquisición e identificación única para el poseedor. 

Cada activo digital tiene su propio **Soulbound Token** (SBT): un token ERC-721 que se acuña mediante un `claim()` público, queda atado para siempre a la wallet que lo reclamó y no puede venderse ni transferirse. Es, en sentido estricto, una credencial personal de propiedad. El nombre del proyecto lo resume: *mellon* es "amigo" en quenya, la palabra que abría la puerta de Moria en *El Señor de los Anillos*. 

En esta testnet se ha completado la prueba de concepto con la serie *The Three Elven Rings* que consiste en *Galadriel and Nenya*, *Mithrandir and Narya*, y *Elrond y Vilya*, donde **cada edición es un contrato ERC-721 independiente desplegado desde el mismo contrato base**, y donde el archivo original de cada obra vive en un bucket privado de Cloudflare R2, entregado únicamente tras verificar on-chain que la wallet solicitante posee el token de esa edición.

Se optó por un soulbound token porque el objetivo es certificar adquisición y otorgar acceso, no crear un mercado secundario. Si el token se puede vender, la llave se separa del comprador original y el certificado deja de ser una credencial de quien adquirió el activo digital. El carácter no transferible mantiene la relación creador → coleccionista intacta, y simplifica toda la capa de autorización: la wallet dueña del token es, por definición, la wallet que minteó.

## 2. Principios de diseño y fundamentación

**2.1 — Un contrato base, un despliegue por edición.** MellonKey toma como modelo mental la **serigrafía**: cada digital asset se publica como una edición, una tirada abierta de copias,  y cada copia adquirida recibe su certificado de adquisición, con sus datos y un único dueño. Técnicamente, esa lógica se materializa con **un contrato base en Solidity** (`TestnetSBTGaladrielNenya`) que se **despliega una vez por cada edición**, y cada despliegue produce una dirección de contrato distinta con su nombre, su símbolo, su metadata y su precio propios — la matriz pública de esa edición. Los tokens que emite son los certificados numerados de las copias adquiridas. La obra queda así completamente aislada: pausar una edición no toca las otras, el precio es independiente, el historial en el explorador es limpio, y la dirección del contrato se convierte en la "identidad" pública de la edición. El costo de este diseño es trivial (desplegar el mismo código N veces) y el beneficio es enorme en simplicidad: el contrato no necesita saber que existen otras ediciones, y el certificado no necesita saber que existen otras obras.

**2.2 — La blockchain es la base de datos.** El registro de "quién adquirió qué y cuándo" vive en el contrato: el mapping `ownerOf` de ERC-721 registra la wallet dueña de cada tokenId, el evento `Claimed` emite la acuñación con su timestamp, y la función `mintedAt(tokenId)` expone la fecha de acuñación. La autorización de descarga del activo digital se resuelve consultando el contrato. 

**2.3 — La configuración (red, contratos, R2, precios, estado) vive en `editions.json`**. El frontend y la función de entrega lo leen desde ahí. Las direcciones de cada smart contract están hardcodeadas.

**2.4 — Mint sin servidor, entrega con servidor mínimo.** El minteo es una transacción directa entre la wallet del usuario y el contrato: ninguna pieza propia del proyecto participa en la transacción, así que el mint **no requiere servidor**. La entrega del archivo, en cambio, **sí requiere servidor**: 

**2.5 — Todo el ciclo se validó en Base Sepolia con ETH de faucets públicos.** Ese mismo flujo aplica en mainnet.

**2.6 - La metadata se encuentra publicada en Arweave,** almacenamiento permanente, descentralizado y con pago perpetuo. El smart contract guarda una URI. El mismo metadata.JSON alojado en Arweave funcionaría idéntico apuntando a IPFS.

## 3. Arquitectura general: Smart Contracts y Validación de Flujos

Con el propósito de validar el comportamiento base del sistema antes de escalar la infraestructura, el despliegue se ejecutó mediante un contrato inteligente ERC-721 individual para cada activo.

Esta decisión de diseño simplificado permitió aislar variables, acelerar la curva de aprendizaje en Web3 y consolidar los fundamentos de la lógica del negocio antes de avanzar hacia una automatización arquitectónica más avanzada.

### 3.1. Entorno de Pruebas

* Red / Financiación: Base Sepolia Testnet (Faucets de ETH Sepolia).
* Estándar: ERC-721 (configurado como Soulbound Token / No transferible).
* Contrato Inicial: `TestnetSBTGaladrielNenya`.

### 3.2. Desafíos Técnicos e Investigación (PoC)

Para validar la arquitectura antes de implementar automatizaciones complejas (como patrones Factory o Registry), se optó por un enfoque de diseño iterativo. Esto permitió aislar variables, acelerar la curva de aprendizaje en Web3 y auditar el comportamiento del contrato directamente en producción.

Durante el despliegue del contrato base y la acuñación (mint) desde la interfaz de pruebas, se identificó y documentó el siguiente escenario:

* Comportamiento en Exploradores (Blockscout): Tanto los metadatos del activo digital como las funciones globales del contrato se indexan y visualizan de manera correcta y exitosa.
* Incidencia en Client-Side (Extensiones de Billeteras Web3): Se experimentó con un bug visual reconocido por la industria donde ciertas extensiones de navegador no renderizan los métodos públicos `name()` y `symbol()`.
* Análisis de la Incidencia: Este comportamiento no afecta la integridad ni la lógica de negocio del smart contract. Está asociado a la forma en que el proveedor de la billetera (RPC/caché del cliente) parsea los contratos individuales en redes de prueba, lo cual ratifica la importancia de realizar estas pruebas de integración en entornos staging para mapear la experiencia de usuario (UX).
* No obstante, revisado el código `TestnetSBTGaladrielNenya` efectué un cambio menor, procedí a su deploy haciendo solo una modificación -uso de comillas para strings- al rellenar los campos de `name`, `symbol`, `tokenURI`, `mintPrice` en Remix IDE, volví a acuñar el SBT y el resultado fue idéntico. 
* Para aislar otras variables, el siguiente activo digital fue acuñado desde `TestnetSBTMithrandirNarya`. 
* A esta altura pude contar con información suficiente y valiosa que es capitalizada en **MellonKey◉fs◉ v2.0.0**.

Para visualizar los tokens experimentales acuñados para este proyecto (y de otros) puedes visitar 
https://base-sepolia.blockscout.com/address/0x9DF53bB4F3A56b7927560C2638074f55F5DDD20E?tab=tokens_nfts

```
              ┌──────────────────────────────────────────────────┐
              │             Base Sepolia                         │
              │                                                  │
              │   TestnetSBTMellonKey @ 0xc76B…e1B   (Galadriel) │
              │   TestnetSBTMellonKey @ 0x…          (Mithrandir)│
              │   TestnetSBTMellonKey @ 0x…          (Elrond)    │  
              │   • claim() payable      • ownerOf()             │
              │   • mintPrice()          • mintedAt()            │
              │   • balanceOf()          • tokenURI()            │
              └────────────▲───────────────▲─────────────────────┘
                                     │               │
              (1) claim + ETH        │               │  (3) ownerOf(tid)
              wallet ↔ contrato      │               │      verificación
                                     │               │
┌────────────────────┐   (2) lee     │               │
│   editions.json    │◀──────────────┴───────────────┘
│                    │      ┌──────────────────────────────┐
│  red · contratos   │─────▶│  Cloudflare Pages            │
│  r2File · precios  │      │  index.html   (mint / claim) │
└────────────────────┘      │  deliver.html (uso del SBT)  │
                            │  functions/api/download.js   │
                            │  (Pages Function / Workers)  │
                            └───────┬───────────────┬──────┘
                                    │               │
                       (4) lee r2File│              │ (5) tokenURI()
                                    ▼               ▼
                            ┌──────────────┐   ┌──────────────────┐
                            │ Cloudflare   │   │ Arweave          │
                            │ R2 (privado) │   │ metadata JSON    │
                            │ *digitalasset│   │                  │
                            └──────────────┘   └──────────────────┘
```

**Cómo se integran las partes, en orden real de uso:**

1. **El creador de contenido despliega** una instancia de `TestnetSBTGaladrielNenya` por edición (en esta versión se usó Remix IDE) pasando al constructor: nombre de colección, símbolo, URI de la metadata JSON en Arweave y precio en wei. La dirección del smart contract resultante se pega en `editions.json`.
2. **El visitante mintea desde la web**: `index.html` lee la configuración de `editions.json`, consulta `mintPrice()` on-chain y envía `claim()` con el ETH adjunto desde MetaMask. No hay servidor involucrado.
3. **El coleccionista usa el SBT**: en `deliver.html` elige la edición, ingresa su tokenId, firma un mensaje con su wallet y la página envía todo a la función de entrega.
4. **La función verifica y entrega**: `functions/api/download.js` valida la firma, consulta `ownerOf(tokenId)` contra el contrato de esa edición y, si la wallet dueña coincide, sirve el archivo `r2File` de esa edición desde R2. La función de entrega usa la clave del objeto en R2 para resolver la descarga. No es una URL completa.
5. **El certificado de adquisición es público: queda registrado on-chain y, según el marketplace, también puede verse en su interfaz.**: el `tokenURI()` del contrato apunta al JSON de Arweave que exploradores (Basescan, Blockscout) y marketplaces (OpenSea y otros) leen para mostrar nombre, imagen, descripción y atributos del mismo.

Cada pieza consume solo lo que necesita. El contrato es autónomo una vez desplegado: no lee archivos del proyecto. La fuente de datos en `editions.json` no es leída por la blockchain. La metadata JSON en Arweave no la lee el sitio: la resuelven exploradores y marketplaces a través del contrato. Ese desacoplamiento es lo que permite agregar ediciones sin tocar código. 

---

## 4. El contrato base: `TestnetSBTGaladrielNenya`

### 4.1 Qué es y cómo se usa

`TestnetSBTGaladrielNenya` es **un único contrato base** escrito en Solidity (^0.8.20) sobre OpenZeppelin v5. Cuando se despliega **una instancia de este contrato por cada edición de un digital asset**, cada instancia queda viviendo en su propia dirección de contrato en blockchain con sus propios datos. El modelo es el de la serigrafía: el contrato base es la **matriz** (única, reutilizable, auditada una sola vez), cada despliegue es la **edición** estampada con esa matriz (con su nombre, su símbolo, su metadata y su precio), y cada token que emite es una **copia firmada y numerada** de esa edición — un certificado de adquisición con sus datos y un único dueño:

name_: `Galadriel and Nenya (Testnet SBT)` <br>
symbol: `GNENYA` <br>
MetadataURI_: JSON de Arweave de esta obra <br>
mintPrice. `0.02 ETH` en wei <br>

Se optó por un contrato por edición porque (a) el despliegue es el evento que "publica" la obra, con su nombre y símbolo inmutables en el explorador; (b) el precio y la metadata son propios y no requieren lógica condicional; (c) un problema en una edición (por ejemplo, pausarla indefinidamente) no afecta a las demás; (d) el historial de eventos `Claimed` de cada contrato es exactamente el historial de adquisiciones de esa obra — no hay que filtrar nada. Y como el molde es único, auditar una vez alcanza para todas las ediciones.

En esta versión 1.0.0 todavía no se ha implementado ninguna forma de agrupar estos contratos independientes a nivel de blockchain y de indexación. Más allá del sitio web no existe todavía nada que permita a un usuario que observa las ediciones en exploradores y marketplaces entender que mantienen una relación colectiva entre ellas. 

En la versión 2.0.0 como solución previa a la implementación de una Factory o Registry, la agrupación de las tres ediciones dentro de una misma colección se resolverá a nivel de metadatos on-chain mediante la función `name()` codificada en cada contrato. Nuevamente, el objetivo es evaluar el alcance de esta solución en exploradores y marketplaces dado que existe consenso en que la indexación ocurre igual. En la práctica se hará lo siguiente: aunque cada contrato mantendrá su propia dirección e independencia en la red, se utilizará un prefijo común en la casilla `name` para observar si esta identificación también se indexa por semántica en exploradores y aplicaciones. Para que se entienda: `HolderNarya Gil-galad`, `HolderNarya Cirdan`, `HolderNarya Mithrandir`.

La pregunta es si esta convención en `name` garantiza una trazabilidad visual y coherencia de marca inmediata en billeteras y exploradores, o no. No obstante, la integración definitiva del Registry/Factory es la solución técnica para agrupar formalmente las colecciones bajo un registro único on-chain, automatizar su despliegue y validar la legitimidad de cada contrato; también en pos de la indexación unificada, el descubrimiento de contratos y la gobernanza centralizada de la colección; y el camino hacia una interoperabilidad efectiva con dApps y marketplaces.

### 4.2 ERC-721 - Cuando el certificado amerita un dueño único

La metáfora de la serigrafía define también el estándar. Un certificado de adquisición tiene que poder responder, sin ambigüedad y sin consultar a nadie más: **¿qué obra es?** (la metadata de la edición), **¿qué copia es?** (un número por adquisición), **¿de quién es?** (un único dueño) y **¿cuándo se adquirió?** (una fecha por copia). ERC-721 responde a las cuatro: cada token es una unidad única (`tokenId`), `ownerOf(tokenId)` devuelve *una* wallet dueña, y el contrato suma `mintedAt(tokenId)` como fecha de acuñación.

Al momento de evaluar cómo resolver la entrega del archivo digital se consideró también el estándar ERC-1155. Por el contrario, este es un estándar **semi-fungible que piensa en saldos**: una misma wallet puede tener varias unidades del mismo `id` (`balanceOf(wallet, id) = 5`), no existe `ownerOf` por unidad, las unidades son intercambiables entre sí y no hay emisión ni fecha por copia. Quien tiene "3 unidades del id 1" bajo ERC-1155 no es el dueño de tres certificados numerados: es un tenedor de saldo 3. Para ítems fungibles (municiones de un juego, entradas equivalentes) es perfecto; para un certificado de adquisición es la unidad equivocada — limitarlo a 1 por wallet sería un parche sobre un estándar diseñado para lo contrario.

Así cierra en la práctica: cuando alguien reclama su copia, el contrato acuña el siguiente número, guarda la fecha y registra al dueño; cuando alguien pide el archivo original, el servidor en la web solicita su **firma** y le pregunta al contrato **quién es el dueño de ese número**. El certificado de adquisición queda garantizado por el estándar.

### 4.3 Características del contrato

ERC-721 (OpenZeppelin v5)

Estándar universal: el token se ve en wallets, exploradores y marketplaces sin trabajo extra.

Soulbound (no transferible)

- Override de `_update` que solo permite mint (`from = 0x0`) y burn (`to = 0x0`).
- Nadie puede vender, regalar o mover el token: SBT, transfer not allowed.
- Es una credencial personal y una llave que no se puede prestar.

Open edition con `claim()`

- `claim() external payable`, 1 token por wallet (`balanceOf(msg.sender) == 0`).
- Cualquiera que pague el precio reclama su certificado; no hay cupo fijo ni lista de permitidos.
- Una wallet = un certificado = una llave.

Pausable

- OpenZeppelin `Pausable`, `whenNotPaused` sobre `claim()`, `pause()`/`unpause()` solo owner.
- La creadora o el creador puede frenar nuevas acuñaciones en cualquier momento (por cierre de la edición, revisión o emergencia).
- Los tokens ya minteados conservan su acceso: pausar no rompe llaves existentes.

ReentrancyGuard

- OpenZeppelin `ReentrancyGuard`, `nonReentrant` en `claim()` y `withdraw()`.
- Protección explícita contra reentrancia en las dos funciones que mueven ETH.
- Complementada con el patrón checks-effects-interactions (el reembolso va al final).

Reembolso automático

- Si `msg.value > mintPrice`, se devuelve el excedente.
- Mandar ETH de más nunca sobre-paga: el contrato devuelve la diferencia en la misma transacción.

Fecha de acuñación on-chain

- Mapping `_mintedAt[tokenId]` con `block.timestamp`.
- Getter público `mintedAt(uint256)`.
- Evento `Claimed(to, tokenId, mintedAt)`.
- El certificado registra cuándo se adquirió, consultable por cualquiera para siempre y emitido en logs que indexan los exploradores.

Metadata por URI

- `tokenURI()` devuelve la URI fija guardada en el constructor (Arweave; sería idéntico con IPFS).
- Todos los tokens de la edición comparten el mismo JSON de metadata (ver §5), coherente con una open edition.

Administración del owner

- `pause()`/`unpause()`, `setMintPrice()`, `setMetadataURI()`, `withdraw(to)`.
- Control razonable y acotado: ajustar precio, corregir/actualizar metadata, pausar y limpiar fondos manuales.
- En un nuevo contrato, se fijará el retiro automático: Al ejecutarse `claim()`, el pago se enviará directamente a la wallet del owner. La función `withdraw(to)` quedará como respaldo de seguridad por si algún ETH quedara atascado (por ejemplo, si alguien envía fondos al contrato sin usar el mint).
- Nada de lo ya acuñado se ve afectado por estas funciones.

Ownable(msg.sender)

- El deployer queda como owner.
- En esta versión 1.0.0 el owner es la wallet del developer.


### 4.4 Los datos que se fijan al desplegar (constructor)

El constructor recibe **cuatro parámetros**, que se tipean a mano al desplegar y quedan fijados en el storage del contrato:

```solidity
constructor(
        string memory name_,         
        string memory symbol_,       
        string memory metadataURI_, 
        uint256 mintPrice_,          // precio en wei, ej: 20000000000000000 (= 0.02 ETH)            
    ) ERC721(name_, symbol_) Ownable(msg.sender) {
        _metadataURI = metadataURI_;
        mintPrice = mintPrice_;
    }
```

Puntos importantes sobre estos parámetros dentro de MellonKey◉fs◉:

- **`name` y `symbol` pertenecen al contrato (on-chain)**: Se definen mediante las funciones `name()` y `symbol()` del estándar ERC-721 y no pueden ser sobrescritos por metadatos externos. Su función es establecer la identidad de cada edición en la blockchain. Existe también el campo **`name` en el JSON subido a Arweave que representa el activo individual (off-chain)**; y que da nombre al token y coexiste en paralelo con la identidad del contrato sin sustituirla (más sobre esto en §10).
- **`metadataURI` es una URI, no el contenido.** El contrato solo guarda el puntero. En este caso la metadata está en Arweave porque se opta por la permanencia, la descentralización y el pago perpetuo; apuntar a un CID de IPFS o gateway equivalente funcionaría exactamente igual — el contrato lo sirve por `tokenURI()`.
- **`mintPrice` va en wei.** 0.02 ETH = `20000000000000000` wei (18 decimales). La página de mint no hardcodea este valor: lo lee on-chain con `mintPrice()` al momento de mintear, así que un cambio de precio vía `setMintPrice()` se refleja en la web sin tocar nada más.
- **El deployer es el owner.** `Ownable(msg.sender)` hace dueño del contrato a la wallet que despliega, única autorizada para pausar, cambiar precio, cambiar metadata URI y retirar fondos.

En la versión 2.0.0, el nuevo contrato dejará asentado `author` (texto) que expresa una identidad/atribución semántica. También dejará asentadas las **identidades criptográficas verificables de `creator`, `crafter` y `artist`** (direcciones de wallet) en el momento del despliegue. Gracias a **`setAttribution()`**, el owner tendrá la flexibilidad para corregir o actualizar estas direcciones en el futuro sin necesidad de hacer un nuevo deploy.


### 4.5 El registro que deja cada mint

Al llamar `claim()` con el ETH correspondiente, el contrato ejecuta y registra:

```text
require(msg.value >= mintPrice)      → si no: "SBT: insufficient payment"
require(balanceOf(msg.sender) == 0)  → si no: "SBT: already minted" (1 por wallet)
tokenId = _nextTokenId++             → numeración secuencial desde 1
_safeMint(msg.sender, tokenId)       → mapping ownerOf: tokenId → wallet  ★ el registro clave
_mintedAt[tokenId] = block.timestamp → fecha de acuñación on-chain
emit Claimed(to, tokenId, mintedAt)  → log del evento (indexable por exploradores)
emit Transfer(0x0 → wallet)          → evento estándar ERC-721 del mint
refund del excedente (si lo hubo)    → interacción al final (patrón CEI)
```

El ★ marca la pieza que sostiene todo el sistema de entrega: `ownerOf(tokenId)` es la única "tabla de usuarios" que MellonKey tiene, y es inmutable, pública y verificable por cualquiera.

### 4.6 La interfaz mínima que consume el sitio

El sitio usa una ABI human-readable mínima, declarada inline en las páginas y en la función de entrega — no hace falta archivo ABI aparte:

```text
claim()                payable                        → página de mint
mintPrice()            view returns (uint256)         → página de mint
balanceOf(address)     view returns (uint256)         → página de mint (estado de la tarjeta)
ownerOf(uint256)       view returns (address)         → función de entrega (autorización)
mintedAt(uint256)      view returns (uint64)          → panel de certificado en deliver.html
```

---

## 5. La metadata de la edición (JSON en Arweave)

### 5.1 Qué contiene

Cada edición tiene **un JSON de metadata** siguiendo el estándar ERC-721 Metadata, alojado de forma permanente y con pago perpetuo en Arweave. Es el JSON que los exploradores y marketplaces leen vía `tokenURI()` para mostrar el certificado. El de la primera edición (resumido) es:

```json
{
    "attributes": [
        {
            "trait_type": "Artist",
            "value": "MellonKey◉fs◉"
        },
        {
            "trait_type": "Collection",
            "value": "Testnet The Three Elven Rings"
        },
        {
            "trait_type": "Artwork",
            "value": "Testnet Mithrandir and Narya"
        },
        {
            "trait_type": "Edition",
            "value": "Open Edition (Testnet)"
        }
    ],
    "description": "Mithrandir, known in Middle-earth as Gandalf, was one of the Istari sent by the Valar to aid the free peoples against Sauron. Among his most treasured secrets was Narya, the Ring of Fire, one of the Three Elven Rings forged by Celebrimbor. Círdan, the Shipwright, guarded it for centuries and, seeing Gandalf arrive, knew he must give it to him. Narya granted no power of domination, but the ability to kindle hearts, inspire courage, and reawaken hope in times of darkness. With it, Mithrandir inspired Men, Elves, and Hobbits to resist the Shadow, to fight when all seemed lost. The ring, set with a ruby, symbolized the inner fire that does not yield. Gandalf wore it hidden, without boast, and its influence was felt in the Shire, in Rohan, in Gondor, and in the last battle before the Black Gate. After the destruction of the One Ring, the power of the Three faded, and Mithrandir departed into the West bearing Narya, the ring that kindled the flame of resistance.",
    "image": "https://arweave.net/adHMMYCWVpyA8vxyN_cZW5soQzSSbYpvF4Xsa-Rw5fE",
    "name": "Test Mithrandir and Narya by MellonKey◉fs◉ "
}
```

En la implementación de la versión 2.0.0 se tendrá en cuenta que como ciertas plataformas no muestran `name()` ni `symbol()`, se aprovechará `description` para mostrar esa información. 

```json
{
  "description": "CERTIFICATE OF ACQUISITION — Soulbound Token (Testnet). Artist: MellonKey◉fs◉. Collection: Testnet The Three Elven Rings. Artwork: Testnet Galadriel and Nenya. This non-transferable ERC-721 SBT certifies the acquisition of the artwork and unlocks the download of its exclusive file on the MellonKey project page. […] Nenya, the Ring of Water: Galadriel's Ring of Preservation and the Three Elven Rings. […lore completo de Nenya…]"
}
```

**Los `attributes` del JSON son la parte crítica del diseño.** Los SBT de MellonKey son certificados de adquisición y, en consecuencia, el nombre de la creadora (`Creator`), la colección, la obra y el tipo de edición están codificados como atributos estructurados (`trait_type` / `value`). Este es el formato que Basescan, Blockscout, OpenSea y el resto de los exploradores y marketplaces indexan y muestran como "atributos" o "propiedades" del token. Las wallet no suelen visualizarlos. 

### 5.2 Arweave (e IPFS también)

- **Arweave** ofrece almacenamiento permanente con pago único: la URI del JSON no caduca, no depende de que el creador mantenga un hosting, y el certificado queda replicado en la permaweb. Para un certificado de adquisición que se espera que dure décadas, esta permanencia es una buena solución. 
- **IPFS sería válido**, aunque la prestación es diferente. El contrato guarda una cadena de texto y la sirve por `tokenURI()`; no sabe si apunta a Arweave, a un gateway de IPFS ni a ningún otro lugar. Publicar en IPFS y pasar `https://ipfs.io/ipfs/<CID>` al constructor produce exactamente el mismo comportamiento. La opción por Arweave fue de política de permanencia, no de compatibilidad.
- **La imagen también vive en Arweave** (`image` del JSON), de modo que el certificado completo (imagen + datos) es independiente de la infraestructura de la galería.
- Finalmente, resta considerar que en esta versión se ha optado por **el servidor centralizado y de web2 de Cloudflare** dado que todo corre desde la misma web y el proyecto es de portfolio. 

### 5.3 Un solo JSON para toda la open edition

En una open edition todos los tokens son iguales entre sí (misma obra, mismo certificado, distinto número), así que **`tokenURI()` devuelve la misma URI para cualquier tokenId**: 

- **Economía:** no hay que publicar (y pagar) un JSON por token ni guardar URIs por tokenId on-chain.
- **Coherencia:** el certificado N° 3 y el N° 300 declaran exactamente el mismo digital asset y los mismos atributos; pero cada certificado es único respecto al poseedor y por eso se actualiza como dato on-chain el tokenId.
- **Contraste con otros flujos:** en proyectos con metadata por token (donde cada token es distinto) sí hace falta un servicio que genere el JSON a demanda. En MellonKey, al ser una open edition con certificado idéntico: un solo JSON en Arweave alcanza.

> **Nota:** si en el futuro una edición necesitara metadata distinta por token, el contrato ya expone `setMetadataURI()` (owner) para repuntar la URI de la edición; y para el caso por-token existiría la variante de guardar una base URI + sufijo por tokenId. No se implementó porque la open edition no lo necesita.
---

## 6. `editions.json` 

### 6.1 El problema que resuelve

MellonKey centraliza **todo** lo que cambia entre ediciones en un único JSON que leen las tres piezas del sitio:

```json
{
  "chain": {
    "name": "Base Sepolia Testnet",
    "chainIdHex": "0x14a34",
    "chainIdDec": 84532,
    "rpcUrls": ["https://sepolia.base.org"],
    "blockExplorerUrls": ["https://sepolia.basescan.org"],
    "alchemyUrlTemplate": "https://base-sepolia.g.alchemy.com/v2/{ALCHEMY_API_KEY}"
  },
  "editions": [
    {
      "id": "galadriel-nenya",
      "title": "Galadriel and Nenya",
      "author": "Portfolio Project MellonKey",
      "creator": "MellonKey◉fs◉",
      "image": "https://arweave.net/RW6F24scwbOfgL_6cwnBqqPB93tOMtWoxOpPbSOZJxo",
      "priceLabel": "0.02 ETH",
      "contract": "0x...",
      "active": true,
      "r2File": "galadriel-nenya.png",
      "downloadName": "galadriel-nenya.png"
    },
    {
      "id": "mithrandir-narya",
      "title": "Mithrandir and Narya",
      "author": "Portfolio Project MellonKey",
      "creator": "MellonKey◉fs◉",
      ...
      ...
    }
  ]
}
```

### 6.2 Quién lee qué

| Consumidor | Qué usa de `editions.json` |
|---|---|
| `index.html` (mint) | lista de ediciones para renderizar tarjetas, `contract` para interactuar, `chain` para configurar la red en la wallet, `priceLabel`/`image`/`title` para la UI |
| `deliver.html` (descarga) | ediciones activas para el selector, `artist` para el panel de certificado, `contract` para leer `mintedAt` on-chain |
| `functions/api/download.js` (entrega) | resuelve la edición por `id`, valida `active` y `contract`, busca `r2File` en R2, arma la URI de RPC con `alchemyUrlTemplate` |

Ninguna dirección de contrato está hardcodeada en el código. El JSON es leído por el backend **desde los assets estáticos del propio deploy de Cloudflare** (`env.ASSETS.fetch`), con lo cual el deploy y la configuración viajan juntos y siempre consistentes.

### 6.3 El ciclo completo para publicar una edición nueva

```text
1. Desplegar una instancia de `TestnetSBTGaladrielNenya` con `name`, `symbol`, `metadataURI` (JSON de Arweave del digital asset), `mintPrice` en wei.
2. Agregar la dirección del contrato en la nueva entrada del `editions.json`.
3. Subir el archivo original de la obra a R2 con el nombre exacto de "r2File".
4. Poner "active": true.
5. Redeployar el sitio web en Cloudflare. 
```

---

## 7. La página de minteo: `index.html`

### 7.1 El minteo se hace desde la página web, sin servidor

El minteo de cada Soulbound Token en el sitio web de la galería *The Three Elven Rings* proyectado por MellonKey es un `claim()` — un reclamo, no una compra mediada por nadie. El visitante abre la galería, conecta su wallet y reclama su certificado de adquisición en una transacción que va **directamente de su wallet al contrato**. Ningún servidor propio toca la transacción: no hay carrito, no hay API de checkout, no hay "procesador de pagos". El ETH adjunto a la transacción es el precio, y el precio no lo decide la página: la página **lee `mintPrice()` del contrato on-chain** y envía exactamente ese valor como `value` de la transacción. Si la creadora/owner actualizó el precio de mint en el contrato, el sitio web lo refleja automáticamente en la siguiente carga.

### 7.2 Cómo funciona por dentro

- **Tarjetas dinámicas desde `editions.json`.** Las ediciones no están escritas en el HTML: se renderizan leyendo el JSON. Una edición sin contrato desplegado o con `active: false` se muestra como *Coming soon*.
- **Red correcta sin fricción.** Antes de operar, `ensureNetwork()` verifica el `chainId` de la wallet contra el de `editions.json`; si difiere, pide el cambio con `wallet_switchEthereumChain` (y ofrece `wallet_addEthereumChain` si la red no está agregada), sin recargar la página.
- **ABI mínima compartida.** Las tarjetas necesitan `claim()`, `mintPrice()` y `balanceOf(address)` — declaradas como ABI human-readable inline. 
- **Estado real por wallet.** Tras conectar, `balanceOf` se consulta en cada contrato para renderizar el estado verdadero de cada tarjeta (*Mint* / *Owned*).
- **Un solo listener delegado.** Los eventos de click se manejan con delegación sobre el contenedor de tarjetas, de modo que agregar ediciones al JSON no agrega código JS.

### 7.3 El flujo del claim, paso a paso

```text
[Usuario]                      [index.html]                   [Contrato SBT]
    │ conecta wallet                  │                              │
    │───────────────────────────────▶│                              │
    │                     ensureNetwork() + balanceOf()              │
    │ click "Mint"                    │                              │
    │ ─────────────────────────────▶ │ mintPrice()  (lectura)        │
    │                                 │─────────────────────────────▶│
    │                                 │ claim({ value: precio })     │
    │ firma/envía tx en wallet        │─────────────────────────────▶│
    │                                 │        Claimed(to, id, ts)   │
    │◀───────────────────────────────│ tx confirmada                │
    │ SBT en la wallet + estado "Owned"                              │
    │ click "Reveal Here"  ──▶  deliver.html?edition=<id>            │
```

Ese último paso es el puente entre las dos mitades del producto: la mitad *certificado* (mint, visible en wallet y exploradores) y la mitad *llave* (descarga del archivo original). 

---

## 8. Uso del SBT: `deliver.html` y entrega del archivo

### 8.1 La acuñación no requiere servidor; la entrega sí

La acuñación (*mint*) es concretada sin un servidor dado que la transacción es el registro: no hay nada que procesar más allá de lo que la blockchain ya procesa. La entrega del archivo, en cambio, necesita un servidor porque el archivo original requiere autorización para su descarga. Se implementó una **Cloudflare Pages Function** (`functions/api/download.js`) — que corre sobre el mismo runtime de los Workers de Cloudflare y se entrega el archivo desde R2.

### 8.2 Autorización de la entrega

En todos sus proyectos actuales, la creadora y desarrolladora mantiene la misma iniciativa: el registro inmutable, público y fácilmente chequeable que ofrece blockchain para procedencia, autoría y adquisición de cada digital asset. La ventaja o bonus en la adopción de un token registrado en la blockchain es que éste también da solución a una entrega segura del archivo original cuantas veces lo requiera el comprador, sin depender de una base de datos de compradores. La prueba de propiedad se construye en el cliente y se verifica contra la cadena:

```text
[deliver.html — cliente]                          [functions/api/download.js — servidor]
1. Usuario selecciona edición adquirida 
2. `.html` construye el mensaje:
   "Download 'digital asset' with tokenId 3
    at Gallery - 1758…(timestamp en ms)"
3. Usuario confirma personal_sign del mensaje (en la wallet)
4. POST /api/download
   { wallet, signature, message, tokenId, edition }
                                    ─────────────▶ 5. Valida que el mensaje sea reciente
                                                       (TTL de n minutos — anti-replay)
                                                   6. verifyMessage(message, signature)
                                                       → recupera la wallet firmante
                                                   7. ownerOf(tokenId) en el CONTRATO
                                                       de esa edición (vía RPC)
                                                   8. ¿ownerOf == wallet firmante?
                                                       NO → 403
                                                       SÍ → 9
                                                   9. bucket.get(edition.r2File)
                                                       → sirve el archivo con
                                                         Content-Disposition: downloadName
[cliente]
10. Recibe el blob y dispara la descarga
```

- **La firma liga todo en un solo string:** edición + tokenId + wallet + momento. No se puede reusar una firma vieja (TTL de `n` minutos), no se puede usar la firma de una edición para otra, y no se puede firmar con una wallet que no sea la dueña.
- **`ownerOf` es la fuente.** Aunque alguien falsificara el frontend, el servidor verifica la firma criptográficamente y luego consulta al contrato quién es el dueño de ese tokenId **en el contrato de esa edición**. El "registro de compradores" es el mapping del ERC-721.
- **El archivo se resuelve por edición, no por token.** `r2File` está declarado una vez por edición en `editions.json`: todos los tokens de la open edition descargan el mismo archivo original, con el nombre de descarga (`downloadName`) también definido por edición.
- **El secreto de Alchemy queda en el servidor.** El frontend usa el RPC público de Base Sepolia; la función de entrega arma su URI RPC con `ALCHEMY_API_KEY` desde los secretos del entorno (`alchemyUrlTemplate` del JSON). 

### 8.3 Estado actual del archivo: R2 sin encriptar (y la etapa siguiente)

En esta instancia, MellonKey v1.0.0 (*MellonKeySimple*) el archivo original de la obra está en R2 **sin encriptar**: la protección del activo es el control de acceso de la función de entrega — R2 no es público, no hay URL directa al objeto, y la única puerta es `POST /api/download` con verificación on-chain. El archivo se transporta siempre por HTTPS y nunca se expone su ubicación.

**La etapa siguiente, ya definida como roadmap, es la encriptación del archivo en reposo:** el objeto se guardará cifrado (por ejemplo, AES-256 por edición) de modo que el contenido sea inútil sin la clave, y la entrega consista en descifrar al vuelo para wallets verificadas — o, más adelante, esquemas donde la posesión del SBT participe de la derivación de la clave. El cambio es ortogonal al resto del sistema: `editions.json` ya declara el archivo por edición, y la función es el único punto por donde pasan los bytes, así que cifrar ahí no toca contrato, ni frontend, ni flujos existentes.

### 8.4 Panel "Certificate of Acquisition" 

Como los SBT son certificados de adquisición, `deliver.html` incluye un panel que **repite y completa los datos del certificado en la superficie donde el proyecto tiene control total**: creador, título, colección y contrato (desde `editions.json`), tokenId, holder y fecha de acuñación leídos on-chain con un provider de solo lectura que requiere conectar la wallet. Este requisito hace que el certificado de adquisición se comporte como documento personal en la web. Este panel existe precisamente porque las wallets no garantizan la visualización de esos datos (ver §10): el certificado no puede depender del renderer ajeno para mostrar lo esencial.
Si bien todos los datos se encuentran rastreables en la blockchain, y de hecho el panel enlaza además a la página del token en Basescan, en la web es visualizado y reconocido como certificado de adquisición. 


### 8.5 Panel público en `deliver.html`

Un **panel público** en `deliver.html` muestra el título de la edición tal y como se declara en el sitio web del proyecto, pero debajo aparecen datos obtenidos de la blockchain: `name` y `symbol` del contrato; y luego, estado de la edición (`totalMinted`, último certificado, última emisión). Sin conectar ninguna wallet cualquier visitante puede leer esta información. Y cierra el listado de datos con una invitación a adquirir el digital asset. 


## 9. Red de prueba y financiamiento con faucets

Todo el ciclo del proyecto **MellonKey** se desarrolló, se deployó y se probó en **Base Sepolia**, la red de prueba de Base (chainId 84532), por razones que siguen siendo válidas también como política del proyecto:

- **Costo cero de iteración.** El contrato se redeployó varias veces durante el desarrollo (cada deploy produce una dirección nueva que se pega en `editions.json`). En mainnet cada iteración habría costado gas real; en testnet, nada.
- **El flujo es idéntico al de producción.** Base Sepolia es una red EVM real con bloques reales y confirmaciones reales: el claim, la firma de mensajes, la lectura de `ownerOf` y la entrega desde R2 funcionan exactamente igual que en Base mainnet. Migrar es cambiar el bloque `chain` de `editions.json` y desplegar el contrato en la nueva red.
- **El ETH salió de faucets públicos.** El gas usado para desplegar y para los mints de prueba se obtuvo de **faucets de ETH para Base Sepolia** (los disponibles públicamente, que regalan ETH de testnet al pegar la dirección de la wallet). Ningún dinero real está en juego en esta etapa, y aun así el comportamiento económico del sistema (precio, reembolso de excedente, `withdraw`) es plenamente ejercitable: el ETH de faucet es ETH "de verdad" para la red de prueba.
- **El precio de prueba es 0.02 ETH** por certificado, configurado en wei en el constructor (`20000000000000000`). Al pasar a mainnet se decide el precio definitivo y se despliega una instancia nueva del contrato — el precio no se "migra", se declara al desplegar.

Nota práctica: los SBT de testnet **no aparecen en OpenSea ni en la mayoría de los marketplaces** (que indexan mainnet), y es normal que las wallets no los muestren con la metadata completa. Para verificarlos se usan los exploradores de la red de prueba — ver la siguiente sección, que documenta el comportamiento real observado.

## 10. Visualización completa

### 10.1 Lo observado

Previamente, en otro proyectos, se observó lo siguiente: al menos en la extensión del navegador de la wallet Metamask la visualización de la data del contrato y de la data del JSON difiere en campos fundamentales dentro de la misma plataforma y respecto de otras plataformas. Desde este desarrollo no es un tema menor. 
- NFT, estandar ERC-721, edición 1:1, Base Mainnet: visualización de `name`, `description` e `image` del metadata JSON y de address, ID del token, `name` y `symbol` del contrato.   
- NFT, estandar ERC-1155, Base Mainnet: visualización de `name`, `description` e `image` del metadata JSON; y de address, ID del token, estándar del contrato (no `name` ni `symbol`).   
- NFT y SBT, estándar ERC-721, Base Sepolia: visualización de `name`, `description` e `image` del metadata JSON; y de address, ID del token, estándar del contrato (no `name` ni `symbol`).    

En este proyecto, al mintear desde la web, al menos la billetara MetaMask en su extensión para navegadores web muestra: el `name`, la `description` y la `image` del JSON de Arweave, junto con la dirección del contrato, el tokenId y el estándar (ERC-721). No muestra: `attributes` de metadata.JSON, `name` y `symbol` del address del contrato, ni la fecha de minteo establecida como función del contrato. Esto no es un bug del proyecto ni de la metadata — es una limitación conocida del renderer de MetaMask en navegadores web, observable igualmente en NFTs acuñados en mainnet (sí, sorprendente pero corroborado que no siempre toma la información completa)-. De hecho, la wallet no resuelve el `tokenURI` escrito en el address del contrato sino que consulta datos de marketplaces y exploradores: muestra un payload básico, no renderiza el array de atributos, ni de funciones `name` y `symbol`,menos las custom como `mintedAt()`.


### 10.2 Explorador Blockscout - Chain Base Sepolia  

El Soulbound Token minteado en Testnet se puede ver con **su metadata completa — incluidos todos los `attributes` —** copiando el address del contrato desde la wallet y buscándolo en un explorador como Blockscout en la Red/Chain Base Sepolia:

**https://base-sepolia.blockscout.com/**

Tambíén desde allí pueden verse todos los tokens adquiridos en Testnet (con la denominación NFT) de cada billetera. 

Para los token del estandar ERC-721, Blockscout resuelve los tres campos del constructor del contrato al momento de visualizar el Soulbound Token en cuestión (*View the Collection*): `name`, `symbol` and `tokenURI` (campos completos) y muestra toda esa información sumado el ID de token. También allí se puede consultar por separado la metadata JSON alojada en Arweave. 

También Basescan (`sepolia.basescan.org`) muestra los atributos en la página del token, y `deliver.html` enlaza directo a esa página desde el panel del certificado.

Cabe señalar que este explorador cuenta con más de 3000 redes para consultar la blockchain. En el caso de Base Mainnet también aquí son visualizados los mismos campos del contrato y de la metadata JSON que en Base Sepolia, sin diferencias al menos entre estandares ERC-721 y ERC-1155. En consecuencia, la falta de datos en las wallets no parece provenir de su fuente de consulta, sino de una forma errática en el renderizado de los datos del token (NFT y/ó SBT).

### 10.3 Marketplaces

Blockscout y los marketplaces leen **los mismos campos** desde el address del contrato y desde este hacia la metadata JSON alojada en Arweave. Blockscout y OpenSean/Rarible/otros renderizan el estándar completo, algo que algunas wallets no hacen. 

> **Si el token se ve completo en Blockscout de Base Sepolia, se verá completo en OpenSea y en los demás marketplaces de la red Ethereum** cuando la edición se publique en mainnet — porque todos consumen la data del address del contrato por la misma vía on-chain. Esto configura el registro inmutable de la tenencia del SBT. 

MellonKey◉fs◉ ha optado por mostrar el Certificado de Adquisición con toda la data, y el proyecto lo asegura por **tres superficies simultáneas**:

| Superficie | Qué muestra | Controlado por |
|---|---|---|
| `description` del JSON (texto plano) | Los datos del certificado como texto: artista, colección, obra + nota de atributos | WalletS renderizan campos `name` y `description` del JSON |
| `attributes` estructurados | Artist / Collection / Artwork / Edition como propiedades | Exploradores y marketplaces (Blockscout, Basescan, OpenSea) |
| Panel "Certificate of Acquisition" en `deliver.html` | Artista, obra, tokenId, contrato y **fecha de acuñación on-chain** | El propio sitio — siempre completo, sin depender de wallets |

---

## 11. Tabla de decisiones: qué se eligió y por qué

| # | Decisión | Alternativa descartada | Fundamento |
|---|---|---|---|
| 1 | **Un contrato base, una instancia por edición (ERC-721)** | Un contrato único multi-edición  | Cada obra es una serigrafía digital: certificado numerado, con sus datos y un único dueño (`ownerOf`). Además: aislamiento por obra; nombre/símbolo/precio/metadata propios; auditar una vez sirve para todas |
| 2 | **Soulbound (no transferible)** | NFT transferible con mercado secundario | El token es certificado de adquisición y llave personal; la venta rompería la relación artista→coleccionista y separaría la llave del comprador |
| 3 | **Open edition con `claim()` público** | Edición limitada con allowlist | Acceso abierto y simple: paga y reclama; 1 por wallet impide acaparamiento; pausable si hay que cerrar la edición |
| 4 | **Pausable + ReentrancyGuard** | Contrato mínimo sin defensas | Control editorial (pausar sin romper llaves ya emitidas) y seguridad estándar sobre ETH en las funciones que mueven fondos |
| 5 | **Metadata en Arweave** | Hosting propio / solo IPFS | Permanencia con pago único; el certificado no depende de un hosting vivo. IPFS queda como opción no equivalente dado la necesidad de nodos que mantengan vivos los archivos |
| 6 | **Un solo JSON compartido por la open edition** | Un JSON por tokenId | Tokens idénticos entre sí salvo el número; enorme ahorro de publicación; sin worker de metadata |
| 7 | **`editions.json` como fuente única** | Direcciones hardcodeadas / configuración por ambiente | Agregar edición = editar JSON + subir archivo a R2 + redeploy del sitio web; frontend y backend leen la misma config en el mismo deploy |
| 8 | **Mint sin servidor** | Checkout con backend propio | La transacción es el registro; cero infraestructura en el camino crítico del pago |
| 9 | **Entrega con Pages Function (runtime de Workers)** | Worker separado / servidor tradicional | Mismo repo y deploy que el sitio; verificación on-chain + R2 en un solo punto; sin base de datos |
| 10 | **Autorización por firma + `ownerOf`** | Base de datos de compradores / tokens de sesión | La blockchain ES el registro; la firma prueba identidad y frescura; `ownerOf` prueba propiedad; nada que sincronizar |
| 11 | **Archivo por edición en R2 (`r2File`)** | Un archivo por tokenId | Todos los tokens de la open edition dan acceso al mismo original; el tokenId individual importa para el certificado, no para el archivo |
| 12 | **TTL de firma de `n` min** | Firma válida para siempre | Anti-replay: una firma interceptada caduca rápido; re-firmar es gratis para el usuario |
| 13 | **Base Sepolia + faucets primero** | Salir directo a mainnet | Iteración sin costo real con flujo idéntico al de producción; migración = redeploy + config |
| 14 | **Panel de certificado en el propio sitio** | Confiar solo en wallets/marketplaces | El certificado muestra artista y fecha de acuñación donde el proyecto controla el render, sin depender del desvarío de plataformas |

## 12. Estado actual y hoja de ruta

### **Estado actual (v1.0.0, testnet):**

- Contrato base `TestnetSBTGaladrielNenya` desplegado en Base Sepolia — instancia de las tres ediciones configuradas en `editions.json`.
- Metadata de la edición publicada en Arweave, con certificado de adquisición en `description` y atributos estructurados.
- Sitio completo en Cloudflare Pages: mint por `claim()` sin servidor y entrega del archivo con verificación on-chain vía Pages Function + R2.
- Panel *Certificate of Acquisition* operativo, con fecha de acuñación leída on-chain, sólo visible por holders.
- Panel público en `deliver.html` 
- Financiamiento de gas y mints de prueba con ETH de faucets públicos de Base Sepolia.

### **Hoja de ruta:**

**Encriptación del archivo en R2** — el objeto original se guardará cifrado; la entrega descifrará al vuelo tras la verificación on-chain. Es la evolución natural del control de acceso actual (definida en §8.3).

**Deployar un nuevo contrato base `TestnetSBTMellonKey.sol`** con modificaciones: 
- `name()` del contrato unificará de manera semántica las tres ediciones. Revisar indexación en exploradores y marketplaces.
- `_nextTokenId` fijado igual a 1, para evitar que el primer ID sea 0; estado que fuerza un nuevo código en `deliver.html`. 
- dejará asentado `author` (texto) que expresa una identidad/atribución semántica. También dejará asentadas las **identidades criptográficas verificables de `creator`, `crafter` y `artist`** (direcciones de wallet) en el momento del despliegue. Gracias a **`setAttribution()`**, el owner tendrá la flexibilidad para corregir o actualizar estas direcciones en el futuro sin necesidad de hacer un nuevo deploy.
- se fijará el retiro automático: Al ejecutarse `claim()`, el pago se enviará directamente a la wallet del owner. La función `withdraw(to)` quedará como respaldo de seguridad por si algún ETH quedara atascado (por ejemplo, si alguien envía fondos al contrato sin usar el mint).

**Nuevos datos en `description`** de metadata.JSON de cada digital asset subido a Arweave: se agregará información ya presente en `attributes` para que conste en plataformas donde no son visualizados.

> Fuera de este proyecto de portfolio y ya en desarrollo para proyecto ·sarocha·: el archivo original es el soulbound token, se aloja encriptado en Arweave, y se desencripta mediante sobre criptográfico.

## 13. Estructura del repositorio

```text
testpages-simple/
├── index.html                  # Página de minteo: tarjetas dinámicas + claim() sin servidor
├── deliver.html                # Uso del SBT: firma + descarga + panel público + panel certificado de adquisición
├── editions.json               # FUENTE ÚNICA: red, contratos, r2File, precios, estado
└── functions/
    └── api/
        └── download.js         # Pages Function (runtime Workers): verifica y entrega desde R2
└── css/
    └── styles-deliver.css
    └── styles.css

```

## 14. Glosario mínimo

| Término | Significado en **MellonKey◉fs◉** |
|---|---|
| **SBT (Soulbound Token)** | ERC-721 no transferible: certifica adquisición y actúa como llave; no se puede vender ni mover de la wallet |
| **ERC-1155** | Estándar semi-fungible: una misma wallet puede tener varias unidades del mismo id (un saldo). Descartado para los certificados: sin `ownerOf` por unidad no hay número de copia ni dueño único — un tenedor de saldo no es un coleccionista de un certificado |
| **Open edition** | Edición sin cupo fijo: cualquiera que pague el precio reclama su certificado (1 por wallet) |
| **`claim()`** | El mint como reclamo: transacción payable directa wallet→contrato, con reembolso automático del excedente |
| **wei** | Unidad mínima de ETH (10⁻¹⁸). 0.02 ETH = `20000000000000000` wei |
| **Pausable** | Interruptor del owner que frena nuevos `claim()` sin afectar los tokens ya emitidos |
| **ReentrancyGuard** | Escudo de OpenZeppelin contra ataques de reentrancia en `claim()` y `withdraw()` |
| **`ownerOf`** | Función del estándar que dice qué wallet es dueña de un tokenId — el "registro de compradores" del sistema |
| **`mintedAt`** | Getter propio del contrato: fecha de acuñación de cada token, en segundos unix |
| **`tokenURI`** | URI que apunta al JSON de metadata de la edición (Arweave en esta etapa; IPFS sería equivalente) |
| **Arweave / IPFS** | Almacenamientos descentralizados. Elegimos Arweave por permanencia con pago único; el contrato es agnóstico al transporte |
| **Cloudflare R2** | Bucket de objetos compatible con S3 donde vive el archivo original de cada obra (privado, sin URL pública) |
| **Pages Function / Workers** | El runtime serverless de Cloudflare que ejecuta `functions/api/download.js`: verifica la firma, consulta `ownerOf` y sirve el archivo |
| **Base Sepolia** | Red de prueba de Base (chainId 84532) donde se desarrolló y deployó esta etapa, con ETH de faucets |
| **Faucet** | Servicio público que regala ETH de testnet para pagar gas durante el desarrollo |
| **Blockscout** | Explorador de bloques (base-sepolia.blockscout.com) que muestra el token con su metadata completa, atributos incluidos |

♾️ *Stay human*
◉fs◉
