Astro Starter Kit: Minimal
```sh
npm create astro@latest -- --template minimal
```
> 🧑‍🚀 **Seasoned astronaut?** Delete this file. Have fun!
🚀 Project Structure
Inside of your Astro project, you'll see the following folders and files:
```text
/
├── public/
├── src/
│   └── pages/
│       └── index.astro
└── package.json
```
Astro looks for `.astro` or `.md` files in the `src/pages/` directory. Each page is exposed as a route based on its file name.
There's nothing special about `src/components/`, but that's where we like to put any Astro/React/Vue/Svelte/Preact components.
Any static assets, like images, can be placed in the `public/` directory.
🧞 Commands
All commands are run from the root of the project, from a terminal:
Command	Action
`npm install`	Installs dependencies
`npm run dev`	Starts local dev server at `localhost:4321`
`npm run build`	Build your production site to `./dist/`
`npm run preview`	Preview your build locally, before deploying
`npm run astro ...`	Run CLI commands like `astro add`, `astro check`
`npm run astro -- --help`	Get help using the Astro CLI
👀 Want to learn more?
Feel free to check our documentation or jump into our Discord server.
📊 Analítica y Conversiones
Scripts base en `src/layouts/Layout.astro`:
Servicio	ID
Google Analytics 4 (`gtag`)	`G-1BGH857NRQ`
Meta Pixel (`fbq`)	`1303244100065898`
Google Customer Reviews	`merchant_id: 5756122991`
Eventos
Página	GA4	Meta Pixel
`producto/[id].astro` (carga)	`view_item`	`ViewContent`
`producto/[id].astro` (agregar al carrito)	`add_to_cart`	`AddToCart`
`gracias.astro`	`purchase`	`Purchase`
Todas las llamadas están protegidas con `typeof gtag === 'function'` / `typeof fbq === 'function'`.
Flujo de compra (`purchase`)
`checkout.astro`: tras crear la orden en Syscom, redirige a `/gracias?folio=XXX&email=YYY`. No borra `esben_carrito` (solo `esben_cupon_aplicado`).
`gracias.astro` (`DOMContentLoaded`):
Muestra el folio.
Lee `esben_carrito` de `localStorage`. Si está vacío o no existe, no dispara conversiones (evita compras duplicadas en $0 al recargar).
Calcula `value = Σ precio × cantidad`.
GA4 `purchase`: `transaction_id` (folio), `value`, `currency: 'MXN'`, `items[]` (`item_id`, `item_name`, `price`, `quantity`).
Meta `Purchase`: `value`, `currency: 'MXN'`.
Ejecuta `localStorage.removeItem('esben_carrito')` y `window.dispatchEvent(new Event('carritoActualizado'))` para reiniciar el ícono del carrito.
Si hay `email` y folio, renderiza la encuesta de Google Customer Reviews (entrega estimada: hoy + 5 días).
> ⚠️ No reintroducir `localStorage.removeItem('esben_carrito')` en el flujo de éxito de `checkout.astro`: `gracias` dependería de un carrito vacío y nunca registraría la conversión.
