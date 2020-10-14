### Módulo 15: Empaquetado de JavaScript para implementación de producción

Tarea 1: instalar y configurar Babel y webpack

1. Limpio y compilo la solución
2. me creo el fichero package.json
3. instalo npm install babel-core babel-loader babel-preset-es2015 webpack --save-dev
4. me creo el archivo webpack.config.js 
````javascript
	var path = require('path');
    var webpack = require('webpack');
	
	/* fichero a distribuir */
	  module.exports = {
        entry: {
            video: './scripts/pages/video.js',
            feedback: './scripts/pages/feedback.js',
            live: './scripts/pages/live.js',
            location: './scripts/pages/location.js',
            locationVenue: './scripts/pages/location-venue.js',
            register: './scripts/pages/register.js',
            schedule: './scripts/pages/schedule.js',
            speakerBadge: './scripts/pages/speaker-badge.js',
            offline: './scripts/offline.js'
        },
    }
	
	/* configuracion de la salida
	
	
    output: {
        path: path.resolve(__dirname,'dist'),
        filename: '[name].bundle.js',
        publicPath: '/dist/'
    },
	
	
	/*reglas para el babel.load */
	
	 module: {
        rules: [
            {
                test: /\.js$/,
                loader: 'babel-loader',
                query: {
                    presets: ['es2015']
                }
            }
        ]
    },
	
	
	/* agregar  los objetos stats , devtool y mode . */


	
	stats: {
        colors: true
    },
    devtool: 'source-map',
    mode: 'production'
````

ejecuta npm run webpack

Ojo verifica que package.json  tenga las depencencias y el scripts para que run corra



  "scripts": {
    "webpack": "webpack"
  },
  "devDependencies": {
    "babel-core": "^6.26.3",
    "babel-loader": "^7.1.5",
    "babel-preset-es2015": "^6.24.1",
    "webpack": "^4.44.2",
    "webpack-cli": "^3.1.2"
  }
  
  
  
  Finalmente en los hamtl los includes deben tirar de dist
	