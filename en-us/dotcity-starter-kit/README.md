# 3. Develop IoT Frontend

.CITY Starter Kit is a simple example that help makers making IoT web frontend.

## Install

1. [Download CSK](https://github.com/wotcity/dotcity-starter-kit/releases).
2. Run `$ cd dotcity-starter-kit` to change the directory.
3. Run `$ npm install --global gulp` to install Gulp.
4. Run `$ npm install` to install the dependencies if you don't already have them.
5. Run `$ bower install` to install the dependencies if your don't already have them.
6. Run `$ gulp build` to build the application scripts.
7. Open `index.html` with your faroviate browser. The web app page is empty now.
8. Append the device name in the URL as an frontend routing parameter. For example `index.html#5547870f4dd3e08d63000007`

### Prerequisites

1. [Node.js](https://nodejs.org). Note: Node should be with a version above 0.10.x.
2. [Gulp](http://gulpjs.com). Note: Run `$ npm install --global gulp` to install the latest version.

## Usage

.CITY Starter Kit is built by BackboneJS. *Backbone.Rounter* is used to identify the device name. Refer to step 8 above, plesae open *index.html* as the following example.

```
html#5547870f4dd3e08d63000007
```

Once your mbed device starting push JSON data, WoT.City will forward the data to web frontend. The example reads data over Websocket and triggers *Backbone.Model* change state. 

## JSON Format

The JSON format of this example should be the following.

```
{
    "temperature": <Number>
}
```

The format of IoT device and IoT frontend must be unified.
