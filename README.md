# Nest Camera SDK
Access & Stream the Nest API Camera data through a Reactive Node.JS interface. 

In 2019 Google deprecated the Works With Nest program and shut down the Nest API's for new developers and pigeon hole'd the existing Nest developers
to migrate their Nest account's to Google. Google integrated the Nest products under the "Works With Google" umbrella and essentially made data access to your own
Nest devices a one way street.

You can give your Nest data to Google but you can't get it back out of the cloud. I am a fan of open source IoT and I think this is pretty uncool. This Nest Camera SDK aims
aims to give developers the freedom and ability to access their own camera feed data in order to continue building great applications.

![nest denial letter](./assets/nest_denied.png)

## Quick Start

To get started with the Unofficial Nest Camera API you can install the package from [NPM](https://npmjs.com): 

```shell script
$ npm install nest-cam
```

### Examples

You can do a whole lot more with the Nest Camera SDK but here is a quick example to get you started:

```javascript
const Nest = require('nest-cam');

const nest = new Nest({
    nestId: "YOUR_NEST_ID",
    refreshToken: "YOUR_REFRESH_TOKEN",
    apiKey: "YOUR_API_KEY",
    clientId: "YOUR_CLIENT_ID"
});

nest.init().then(() => {
    nest.saveLatestSnapshot('/User/me/camera/images/my_image.jpg');
    ...
});
```

### Prerequisites

Before you go hooking into your camera data you will need some security credentials. Check out the table below for a description
of each credential.

| **Name**       | **Data Type** | **Description**                                                                                                                                                                                                                                                   |
|----------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `nestId`       | String        | A unique string which identifies your Nest Camera to the Google Nest API. This value will not change.                                                                                                                                                             |
| `refreshToken` | String        | Nest uses OAuth 2.0 to securely authenticate users. A refresh token allows an application to obtain a new OAuth access token without prompting the user for any information. This is a long lived token which will expire but not for an estimated several weeks. |
| `apiKey`       | String        | This value comes with special privileges to tell the Nest servers how often the client wielding the api key can call the backend. This value will not change.                                                                                                     |
| `clientId`     | String        | The Client Id is a unique string which is used by the Nest API to identify which client or application is calling the backend resources.                                                                                                                          |

Retrieving these values can be somewhat cumbersome but I will do my best to make the process as painless as possible. 

## Obtaining Nest Security Credentials

To get the security credentials you will have to setup a proxy on the same WiFi network as your Nest Camera. We will proxy requests from your Android or iPhone 
through the Nest app (and our proxy) to intercept the security credentials before they get to the Nest servers. Don't worry this is **way** easier than it sounds.
This tutorial is written for the Mac & iOS but works in a similar fashion on Windows and Android. Please consult the [Charles Documentation](https://www.charlesproxy.com/documentation/using-charles/ssl-certificates/) to
learn how to trust the SSL certificates on a Windows machine. 

### Installing Charles Proxy

First head to the [Charles Proxy Website](https://www.charlesproxy.com/) and download and install the proxy software. Next we are going to configure
Charles to trust all SSL certificates it proxies so that we can see the request's and response's that Nest uses to communicate with its API's (The credentials you are after are in these requests and responses). 

### Trusting the Root Charles SSL Certificate

Charles generates its own certificates for sites, which it signs using a Charles Root Certificate, which is uniquely generated for your installation of Charles (as of v3.10). In order
to avoid having to trust each website you visit individually you can simply trust the Charles Root Certificate one time and it will take care of everything else for you.

You need to trust the certificates on your Mac/Windows and iPhone/Android

#### Mac 

In Charles go to the Help menu and choose "SSL Proxying > Install Charles Root Certificate". Keychain Access will open. Find the "Charles Proxy..." entry, and double-click to get info on it. 
Expand the "Trust" section, and beside "When using this certificate" change it from "Use System Defaults" to "Always Trust". 
Then close the certificate info window, and you will be prompted for your Administrator password to update the system trust settings. 

![charles trust](./assets/charles_trust.png) 

#### iOS

In order to install the Charles Trust Certificate on iOS there are a couple simple steps.

Set your iOS device to use Charles as its HTTP proxy in the Settings app > Wifi settings. For the IP Address set the IP
address for the device that is running the charles proxy. This is usually something like: `192.168.1.153`. The port will always be `8888`
and there is no Authentication.

**Go into your Wifi setting:**
![ios proxy one](./assets/ios_proxy_one.PNG)

**Update your proxy**
![ios proxy one](./assets/ios_proxy_two.PNG) 

Open Safari and browse to [https://chls.pro/ssl](https://chls.pro/ssl). Safari will prompt you to install the SSL certificate.
If you are on iOS 10.3 or later, open the Settings.app and navigate to General > About > Certificate Trust Settings, and find the Charles Proxy certificate, 
and switch it on to enable full trust for it (More information about this change in iOS 10).

![ios_cert_trust](./assets/ios_cert_trust.PNG)

Done!

## Running the tests

Unit tests for this project are managed with Mocha and can be executed with:

```shell script
$ npm test
```

### Coding Style Tests

This project uses [ESLint](https://eslint.org/) to manage the coding style. To run the eslint tests and ensure the code is formatted correctly
according to the style guide run:

```shell script
$ sh ./scripts/coding_style_tests.sh
```

## Deployment

This project uses Travis CI to test and deploy the built artifact to NPM. Travis CI configuration settings are managed
in the `.travis.yml` file. 

Deployments are done on tagged commits to the master branch when all the unit and coding style tests pass. Make sure
to bump the version in the `package.json` accordingly before deploying!

## Built With

* [Node.JS](http://www.dropwizard.io/1.0.2/docs/) - The server side library used
* [Javascript](https://maven.apache.org/) - Programming Language Used
* [NPM](https://npmjs.com) - Dependency Management
* [RxJS](https://rometools.github.io/rome/) - Reactive Javascript library for working with streaming data and observables

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Christian Bartram** - *Initial work* - [cbartram](https://github.com/cbartram)

See also the list of [contributors](https://github.com/cbartram/Nest/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE) file for details

## Acknowledgments

* Nest for making a cool camera
* Google for taking down the nest API's without which this project would not be possible
* Charles Proxy for helping me decode the Nest API calls
