# Weather Radar (säätutka)

Project uses Open Weather Maps API. URIs and API Key will be stored in .env file.

To use API: Create .env file on the root of project with this structure:

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
Vue.js provides fast and mimimal framework for this project which is ideal for mobile devices.

Application is tested with:

- Mozilla Firefox version 72.0.2
- Chrome (desktop and mobile) versions 79.0

### Libraries

- Bootstrap Vue & Bootstrap - Responsive style and design libraries, using tree shaking technique for minimal build size
- Axios - for HTTP requests
- Moment.js - datetime library for time formats

### Noticed issues:

- Precipitation 3h: not always resulted from API, will mostly show 0.00

### Ideas for further development

- Progressive Web Application for more native mobile/ desktop app experience
- Get users geolocation to check weather from current location
