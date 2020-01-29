# saatutkaapp

Project uses Open Weather Maps API. URIs and API Key will be stored in .env file.
.env file must be created in the projects root folder with this structure:

```
VUE_APP_APIKEY={ Insert your API key here }
VUE_APP_IMGURL=https://openweathermap.org/img/wn/
VUE_APP_BASE=https://api.openweathermap.org/data/2.5/
```

## Project setup

To install all libraries

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Lints and fixes files

```
npm run lint
```

## Project

Project is created with Vue CLI tool and uses bootstrap for fast mobile first experience development.

Application is tested with:
Mozilla Firefox version 72.0.2
Chrome (desktop and mobile) versions 79.0

### Noticed issues:

- Precipitation 3h: not always resulted from API, will mostly show 0.00
