
Craete project

``` Bash
$ mkdir elm-tailwindcss-sampleapp
$ cd elm-tailwindcss-sampleapp
```

Install elm and parcel

``` Bash
$ npm i -D elm parcel tailwindcss @parcel/transformer-elm
```

Initialize elm and parcel

``` Bash
$ npx elm init
$ npx tailwindcss init
```

Set up `.postcssrc` to compile tailwindcss with postcss.

``` JSON :.postcssrc
{
  "plugins": {
    "tailwindcss": {}
  }
}
```

Set up `tailwind.config.js` to compile tailwindcss with postcss.

``` JS :tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{html,js,elm}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Write settings in `package.json`.

``` JSON :package.json
{
  "devDependencies": {
    "@parcel/transformer-elm": "^2.6.0",
    "elm": "^0.19.1-5",
    "parcel": "^2.6.0",
    "tailwindcss": "^3.1.2"
  },
  "scripts": {
    "start": "parcel src/index.html",
    "build": "parcel build src/index.html --no-source-maps",
    "clean": "rm dist/*"
  }
}
```


Save the pre-compiled file in `src`.
Prepare `src/index.html`

``` HTML :src/index.html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="index.css" rel="stylesheet">
</head>
<body>
  <div id="root"></div>
  <script type="module" src="index.js"></script>
</body>
</html>
```

Prepare `src/index.css`

``` CSS :src/index.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```


Prepare `src/index.js`

``` JS :src/index.js
import { Elm } from "./Main.elm";

Elm.Main.init({ node: document.getElementById("root") });
```

Write logic in `src/Main.elm` with elm.

``` elm :src/Main.elm
module Main exposing (main)

import Browser
import Html exposing (Html, button, div, text)
import Html.Attributes exposing (class)
import Html.Events exposing (onClick)


main =
    Browser.sandbox { init = init, update = update, view = view }


type alias Model =
    { score : Int
    }


init : Model
init =
    { score = 0
    }


type Msg
    = Select String


update : Msg -> Model -> Model
update msg model =
    case msg of
        Select _ ->
            { model | score = model.score + 1 }


view : Model -> Html Msg
view model =
    div []
        [ button
            [ class "rounded-full bg-gray-300 px-4 py-2", onClick (Select "a") ]
            [ text (String.fromInt model.score) ]
        ]
```

Build

```
$ npm run build
```

Local development

```
$ npm run start
```

Delete built files

```
$ npm run clean
```


