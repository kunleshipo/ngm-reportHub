# [ReportHub](http://reporthub.immap.org)
>
> Reporting Workflow. Decision Support. Real-Time.
>
> Developer documentation for ReportHub modules

## Notes
- The following opinionated [AngularJs Style Guide](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md) is recommended for coding patterns


### Objective
- Build kobo reporting tool as module in ReportHub


## Solution
- [ ] Create Kobo form [i.e. example](https://www.dropbox.com/s/7g0wlmxmflov626/monitoring_and_supportive_supervision.xlsx?dl=1)

### Notes
- The kobo form will need to use the `theme-grid pages` style defined in the settings tab.
- See the [xlsForm Standards](http://xlsform.org/) for more details.


## ReportHub db Connection
- In order to create a new database, ReportEngine requires a database connection string (once ReportHub is installed).

### Steps
- [ ] Update `/home/ubuntu/nginx/www/ngm-reportEngine/config/local.js`. [i.e. example](https://github.com/pfitzpaddy/ngm-reportShell/blob/master/ngm-reporthub.shell.build.sh#L237)
- [ ] Include the following config object in addition to the existing database connections to establish a new Mongo database with sailsjs
```
		ngmiMMAPServer: {
			adapter: 'sails-mongo',
			host: 'localhost',
			port: 27017,
			// user: 'username',
			// password: 'password',
			database: 'ngmiMMAP',
			schema: false
		},
```
- [ ] Make `iMMAP/Reporting` folder structure in [models](https://github.com/pfitzpaddy/ngm-reportEngine/tree/master/api/models)

### Notes
- Inside models are the schema (as json files) that will be created in the `ngmiMMAP` database as Mongo Collections when you use the [Waterline ORM](http://waterlinejs.org/) syntax to `Create`,  `Read`, `Update`, `Delete` (CRUD) in controllers.


## Kobo Processing APIs
- The monthly report processing script will be an API that fetches Kobo API data for the Monthly Report form and processes it into standardised data tables.

### Steps
- [ ] Create branch in ngm-reportEngine folder `git checkout -b feature-monthly-reporting`
- [ ] Create project folder `iMMAP/Reporting` in [controllers](https://github.com/pfitzpaddy/ngm-reportEngine/tree/master/api/controllers)
- [ ] Processing script in `iMMAP/Reporting` [sample](https://github.com/pfitzpaddy/ngm-reportEngine/blob/master/api/controllers/Country/Eth/Ctc/CtcController.js)
- [ ] Routes to access the script to make it RestFULL. [i.e. routes](https://github.com/pfitzpaddy/ngm-reportEngine/blob/master/config/routes.js)
- [ ] Resulting tables required to store the processed data need to be added as model. [i.e. models](https://github.com/pfitzpaddy/ngm-reportEngine/tree/master/api/models/Ctc)

### Notes
- You will need to configure KOBO connection params in `/home/ubuntu/nginx/www/ngm-reportEngine/config/kobo.js`.



## ReportHub Dashboard APIs for Kobo
- Create indicators and stats from the database available as RestFULL APIs.

### Steps
- [ ]  Processing script in `iMMAP/Reporting` [i.e. sample](https://github.com/pfitzpaddy/ngm-reportEngine/blob/master/api/controllers/Country/Eth/Ctc/CtcDashboardController.js)
- [ ] Routes to access the script to make it RestFULL. [i.e. routes](https://github.com/pfitzpaddy/ngm-reportEngine/blob/master/config/routes.js)


## ReportHub Dashboard Config
- The dashboard page will require a route that will load a dashboard configuration

### Steps

**App**
- [ ] Create `immap` folder in [modules](https://github.com/pfitzpaddy/ngm-reportHub/tree/master/app/scripts/modules)
- [ ] Define `app.js` in `immap` folder.
- [ ] Create base routes in`immap/app.js` as [i.e. sample](https://github.com/pfitzpaddy/ngm-reportHub/blob/master/app/scripts/modules/snapshots/app.js)
```
.when( '/immap/reporting/monthly', {
	templateUrl: '/views/app/dashboard.html',
	controller: 'DashboardiMMAPReportingMonthlyCtrl',
	resolve: {
		access: [ 'ngmAuth', function(ngmAuth) {
			return ngmAuth.grantPublicAccess();
		}],
	}
})
```
- [ ] Name module in `app.js` as `ngmiMMAP`
- [ ] Add `ngmiMMAP` into the main [app.js file](https://github.com/pfitzpaddy/ngm-reportHub/blob/master/app/scripts/app/app.js)

**Dashboard**
- [ ] Create `reporting` folder in `modules/immap` folder
- [ ] Create `dashboards` folder in above `modules/immap/reporting` folder
- [ ] Create `dashboard.immap.reporting.monthly.js` file in above `modules/immap/reporting/dashboards` folder
- [ ] Name module in `dashboard.immap.reporting.monthly.js` as `DashboardiMMAPReportingMonthlyCtrl ` file in above `modules/immap/reporting` folder
- [ ] Create dashboard configuration [i.e. sample](https://github.com/pfitzpaddy/ngm-reportHub/blob/master/app/scripts/modules/snapshots/dashboards/cdc/dashboard.2018.01.js)

**Index**
- [ ] Add all scripts into `app/index.html`
```
<!-- iMMAP -->
<script src="scripts/modules/immap/app.js"></script>
<!-- dashboards -->
<script src="scripts/modules/immap/reporting/dashboards/dashboard.immap.reporting.monthly.js"></script>
```

### On completion
- [ ] Add all new files into the repository at ngm-reportHub `git stage -A`
- [ ] Commit all changes in ngm-reportHub `git commit -m "Feature: New dashboard page for monthly reporting" -a`

### Notes
- Test the URL before making changes to the configuration
- if you get lost just copy the other examples in [modules](https://github.com/pfitzpaddy/ngm-reportHub/tree/master/app/scripts/modules)


## Team Landing Page
- Make a landing splash page in HTML that will load for the iMMAP team members.

### Steps
- [ ] Create branch in ngm-reportHub `git checkout -b feature-monthly-reporting`

**Page View**
- [ ] In `modules/immap` folder create `views` folder
- [ ] in `modules/immap/views` folder create `immap.landing.html`
- [ ] Create HTML template with cards as `Monthly Report`, `Training Report` and `Dashboards` [i.e. sample](https://github.com/pfitzpaddy/ngm-reportHub/blob/master/app/scripts/modules/country/ethiopia/views/ethiopia.assessments.html)

**Page Controller**
- [ ] `modules/immap` folder create `pages` folder
- [ ] `modules/immap/pages` folder create `immap.landing.js` file [i.e. sample](https://github.com/pfitzpaddy/ngm-reportHub/blob/5c540f3e7a964da4dc50e57bdea9e2c741353204/app/scripts/modules/country/ethiopia/assessments/ethiopia.assessments.js)
- [ ] Update controller name to `iMMAPLandingCtrl`
- [ ] in controller update line [53](https://github.com/pfitzpaddy/ngm-reportHub/blob/5c540f3e7a964da4dc50e57bdea9e2c741353204/app/scripts/modules/country/ethiopia/assessments/ethiopia.assessments.js#L53) to the HTML Page View above `/scripts/modules/reporting/immap/views/immap.landing.html`

**Page Route**
- [ ] Create route in `modules/immap/app.js` as
```
.when( '/immap', {
	templateUrl: '/views/app/dashboard.html',
	controller: 'iMMAPLandingCtrl',
	resolve: {
		access: [ 'ngmAuth', function(ngmAuth) {
			return ngmAuth.isAuthenticated();
		}]
	}
})
```

**Index**
- [ ] Add all scripts into `app/index.html`
```
<!-- IMMAP -->
<! -- OTHER IMMAP FILES -->
<!-- pages -->
<script src="scripts/modules/immap/pages/immap.landing.js"></script>
```

### Notes
- Test the URL before making changes to the configuration
- If you get lost just copy the other examples in [modules](https://github.com/pfitzpaddy/ngm-reportHub/tree/master/app/scripts/modules)
