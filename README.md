# Pokedex

A beginner-friendly React application that browses Pokemon using the free
[PokeAPI](https://pokeapi.co/).

## Project Overview

This app is a searchable Pokedex:

- Loads every Pokemon name from PokeAPI when the page opens
- Shows them as a grid of cards with artwork and Pokedex number
- **Search by name** as you type (try `char`, `eevee`, `pika`)
- **Load more** shows the next 24 cards
- Click any card to open a **detail dialog** with types, height, weight,
  abilities, and base stat bars — loaded with a second request, on demand
- Friendly **loading**, **error**, and **empty** screens

> **Note:** unlike the other three workshop projects, this one does **not** use
> MockAPI.io. Pokemon data already exists in a free public API, so there is
> nothing to create by hand — you just read from it. It is the same `fetch()`
> you already know, pointed somewhere else. Because this API is read-only,
> this project practises `GET` only (no POST / PUT / DELETE).

## Technologies

- **React** – builds the user interface out of components
- **Vite** – the dev server and build tool
- **Tailwind CSS** – utility classes for styling
- **shadcn/ui** – ready-made, accessible components (Button, Card, Input, Label, Badge, Dialog, Progress, Alert)
- **PokeAPI** – a free public REST API, no account or key needed
- **Fetch API** – how the browser talks to PokeAPI

## Prerequisites

- [Node.js](https://nodejs.org/) (version 20 or newer)
- npm (comes with Node.js)
- [Git](https://git-scm.com/)

No account is needed for this project — PokeAPI is open to everyone.

## Installation

```bash
npm install
```

## Environment Setup

Copy:

```text
.env.example
```

to:

```text
.env
```

On macOS / Linux:

```bash
cp .env.example .env
```

On Windows (PowerShell):

```powershell
Copy-Item .env.example .env
```

The value already works as-is, so you do not have to change anything:

```env
VITE_API_URL=https://pokeapi.co/api/v2
```

We still keep it in `.env` for the same reason as the other projects: the
address of an API is configuration, not something to hardcode in a component.

Notes:

- Variables must start with `VITE_` or Vite will not pass them to the browser.
- Restart `npm run dev` after changing `.env`.

## Run the Project

```bash
npm run dev
```

Then open the address Vite prints (usually http://localhost:5173).

## Build

```bash
npm run build
```

## Preview

```bash
npm run preview
```

## API

This app uses two PokeAPI endpoints.

| Method | Endpoint               | Used for                                   |
| ------ | ---------------------- | ------------------------------------------ |
| `GET`  | `/pokemon?limit=1400`  | Every Pokemon name, once, when the page opens |
| `GET`  | `/pokemon/:name`       | The details of one Pokemon, when a card is clicked |

The list response looks like this:

```json
{
  "count": 1351,
  "results": [
    { "name": "bulbasaur", "url": "https://pokeapi.co/api/v2/pokemon/1/" },
    { "name": "ivysaur",   "url": "https://pokeapi.co/api/v2/pokemon/2/" }
  ]
}
```

Notice the list gives us a **name and a url**, but no picture. Two helpers in
`src/api.js` solve that without extra requests:

```js
idFromUrl('https://pokeapi.co/api/v2/pokemon/25/') // -> 25
artworkUrl(25) // -> the address of Pikachu's picture
```

The detail response is much bigger. We only use a few fields:

```json
{
  "id": 25,
  "height": 4,
  "weight": 60,
  "types": [{ "type": { "name": "electric" } }],
  "abilities": [{ "ability": { "name": "static" } }],
  "stats": [{ "base_stat": 35, "stat": { "name": "hp" } }]
}
```

`height` is in decimetres and `weight` is in hectograms, which is why the code
divides both by 10 to show metres and kilograms.

## Project Structure

```text
src/
├── components/
│   ├── ui/                 # shadcn/ui components
│   ├── SearchBar.jsx       # the search box + result count
│   ├── PokemonGrid.jsx     # maps over the visible Pokemon
│   ├── PokemonCard.jsx     # one card: picture, number, name
│   └── PokemonDialog.jsx   # detail view, fetches its own data
├── api.js                  # reads VITE_API_URL + url helpers
├── pokemonTypes.js         # type name -> Tailwind colours
├── App.jsx                 # state + the list fetch + search
├── main.jsx                # starts React
└── index.css               # Tailwind + theme
```

## Learning Objectives

By reading and changing this project you practise:

- **Components and props** – `App.jsx` passes the visible list and a callback down the tree
- **useState** – the list, loading, error, search text, how many cards to show, which card is open
- **Two different useEffect patterns** – `App.jsx` uses `[]` to fetch **once**, while
  `PokemonDialog.jsx` uses `[name]` to re-fetch **whenever the name changes**. This is the
  big new idea in this project.
- **A component that fetches its own data** – the dialog has its own loading and error states
- **Event handlers** – typing, clicking a card, loading more, closing the dialog
- **Controlled inputs** – the search box value comes from state
- **Conditional rendering** – loading vs. error vs. empty vs. grid
- **`Array.map()`** – cards, type badges, ability names, stat bars
- **`Array.filter()`** – the search, done in the browser over the full list
- **`Array.slice()`** – showing only the first N results ("Load more")
- **Derived state** – the filtered list and the "is there more?" flag are calculated during render
- **Fetch API with async/await** – `GET` wrapped in `try / catch / finally`
- **Working with a real API's shape** – digging values out of nested objects like `entry.type.name`

### Things to try on your own

1. Add a dropdown that filters by type (you will need the type on each card).
2. Show the card's types on the grid, not just in the dialog.
3. Add Previous / Next buttons inside the dialog to walk through Pokemon.
4. Sort the results by Pokedex number or alphabetically.
