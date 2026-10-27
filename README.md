# Chai & Chaat: Campus Canteen Ordering System

A responsive React app where students browse a canteen menu, mark favourites, and build an order in a shared cart.

Built with React 18, Vite, React Router DOM v6, the Context API, and plain CSS.

## Run it

You need Node.js 18 or newer.

```bash
npm install
npm run dev
```

Open the address Vite prints (usually http://localhost:5173).

To make a production build:

```bash
npm run build
npm run preview
```

## How the menu loads

The menu lives in `public/menu.json`. Vite serves everything in `public/` from the site root, so the app fetches it from `/menu.json`. It is loaded with `useEffect`, `fetch` and `async/await` in `MenuPage.jsx` and `ItemDetailsPage.jsx`. No menu data is hardcoded in components.

If the fetch fails, the page shows an error with a "Try again" button. To test this, rename `menu.json` temporarily.

## Folder structure

```
campus-canteen/
  public/menu.json            14 Pakistani canteen items (2 are sold out)
  src/
    main.jsx                  BrowserRouter + CartProvider wrap the app
    App.jsx                   Routes: /, /item/:id, /cart
    index.css                 All styling and media queries
    components/
      Header.jsx              Brand, nav links, live cart count
      MenuList.jsx            Search, category chips, favourites filter
      ItemCard.jsx            Food card with favourite toggle
      Cart.jsx                Cart rows, quantity controls, totals
    context/CartContext.jsx   Shared cart state + localStorage
    pages/
      MenuPage.jsx            "/"
      ItemDetailsPage.jsx     "/item/:id"
      CartPage.jsx            "/cart"
    utils/format.js           PKR formatting + image fallback
```

## Where each requirement is implemented

| Requirement | Files |
| --- | --- |
| Vite + React Router (`Routes`, `Route`, `Link`, `useParams`) | `main.jsx`, `App.jsx`, `ItemDetailsPage.jsx` |
| Shared cart with Context | `CartContext.jsx`, wrapped in `main.jsx` |
| Cart count = total quantity, live in header | `CartContext.jsx` (`totalItems`), `Header.jsx` |
| Same item twice increases quantity | `addToCart` in `CartContext.jsx` |
| Cart saved in localStorage | `useEffect` + lazy initial state in `CartContext.jsx` |
| Menu from JSON with loading, error, retry | `MenuPage.jsx`, `public/menu.json` |
| Search and category filter | `MenuList.jsx` |
| Favourite toggle on every card | `ItemCard.jsx`, state in `MenuPage.jsx` (saved to localStorage) |
| Unavailable items cannot be added | `ItemCard.jsx`, `ItemDetailsPage.jsx` (disabled buttons), `addToCart` guard |
| Quantity selector, Add to Cart | `ItemDetailsPage.jsx` |
| Quantity controls, remove, totals, empty cart | `Cart.jsx` |
| PKR formatting, image fallback | `utils/format.js` |
| Responsive layout, Flexbox/Grid | `index.css` (breakpoints at 640px and 900px) |
| Accessibility (labels, aria-live, skip link, focus styles) | all components, `index.css` |

## Notes

- Food photos are placeholder images from placehold.co. If you are offline or an image fails, an inline SVG fallback appears. To use real photos, change the `image` fields in `menu.json`.
- "Place order" is a demo: it shows a message and clears the cart.
