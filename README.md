

``` Bash
$ npm i -D elm parcel tailwindcss @parcel/transformer-elm
$ npx elm init
$ npx tailwindcss init
```

``` JSON :.postcssrc
{
  "plugins": {
    "tailwindcss": {}
  }
}
```

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

``` CSS :src/index.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```


``` JS :src/index.js
import { Elm } from "./Main.elm";

Elm.Main.init({ node: document.getElementById("root") });
```

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

```
$ npm run build
```
