# Project Name  : WellnessRenovate

## Description

this project has the objective that people who have an apartment to reform, initially in the cities of Barcelona and Madrid, can consult what is the cost of their reform and get metrics on sales prices and rent prices/m2 of the area, being able to select the witnesses to get those metrics, and have direct links to the main real state web portals (Fotocasa & Idealista).

## User Stories

-  **404:** As an anon/user I can see a 404 page if I try to reach a page that does not exist so that I know it's my fault
-  **Signup:** As an anon I can sign up in the platform so that I can start searching for budgets, or just checking for sale/rent prices in any address.
-  **Login:** As a user I can login to the platform.
-  **Logout:** As a user I can logout from the platform.
-  **Generate & Save Report** As a user, and giving the address of my apartment, I can request and download at the moment a budget about the cost of the reform of my flat, and get an estimated price I would be able to sell it once reformed. I will get addionational information:

	- More metrics of the area/neighbourhood : rent price/m2, sell price/m2 of unreformed apartments, sell price/m2 of new construccion building, ...
	- List with detail characteristics of the witnesses used to calculate the metrics. The use will see this witnesses remarked on the map. The user would be able to change the witnesses used to estimated the sell rate, changing for other that appears on the map, and considered more suitable.
-  **Search for Metrics in any area** As a user I want to check metrics values of selling/rental prices in any address of these cities.
-  **See my Reports/Budgets** As a user I want to see my budgets

## MVP

- Create React-Route landing page. Short description of the service, the user will be able to login/logout, signup, or click a button for an online demo (the report will be shown only for few seconds, since we want the user to sign-up).

- Create React-Route where the user chooses the type of reform to an apartment: PARCIAL OR TOTAL. If it's parcial, a short questionary will be shown, asking the user to set the % of reform it needs: bathrooms, kitchen, floors, electrical installation, water installation, walls,.... This will be done with progress bars, from 0 to 100%. Once the information is in, the user will be prompted with a Form component to enter the address.

- Create React-Route Witnesses: the user will receive the list of witnesses, viewing in a list and in a map. The user will be able to change the witnesses (select different ones to calculate the metrics), and apply filters to stretch the search: nº of rooms, bathrooms, etc...
Each marker on the map will have a pop-up with the information regarding the flat. At the end, will be able to generate the report.

- Create React-Route Report: Report with detailed information and graphics.

- Login/logout/Sign-up


## Backlog

Geo Location:
- Be able to select specific area into the map (customize the area)

Report:
- Download report as a .pdf 
- User can Update the report


# Client

## Routes ()

- `/`
  - HomePageComponent
  - public
  - just promotional copy
- `/auth/signup`
  - SignupPageComponent
  - anon only
  - signup form, link to login
  - navigate to homepage after signup
- `/auth/login`
  - LoginPageComponent
  - anon only
  - login form, link to signup
  - navigate to homepage after login
- `/reformtype` 
  - ReformTypePageComponent (contain 3 components: partial, integral, only-metrics)
  - user only
  - select type of reform to apply
  
- `/reformtype/partialreform` 
  - Select-Current-State-Form-Component (contain 5 components: 5 bar progress, user will select the % of the actual state of the apartment)
  - Address-Form-Component ( select the address to search )
  - Style-of-Reform-Component (3 different categories )
  - user only
  - input address to search
  
- `/reformtype/integralreform` 
  - Address-Form-Component ( select the address to search )
  - Style-of-Reform-Component (3 different categories )
  - user only
  - input address to search

- `/createreport`
  - Address-Form-Component
  - Metrics-Bar-Component
  - Filters-Form-CheckBox-Component
  - Map-Component
  - Footer-Component (with buttons to SAVE REPORT and GENERATE REPORT)
  - user only

- `/viewreport`
  - Report-Component
  - Graphic-Component (display graphics with metrics)
  - List-witnesses-Component
  - user only
   

- `/viewreport/:id` 
  - Report-Component
  - Graphic-Component (display graphics with metrics)
  - List-witnesses-Component
  - user only
  - user recovers the complet report done some time ago, with all information.

- `/editreport/:id` 
  - Metrics-Bar-Component
  - Filters-Form-CheckBox-Component
  - Map-Component
  - Footer-Component (with buttons to UPDATE REPORT (save changes to BBDD) and GENERATE REPORT (generate a view))

- `/listofreports`
  - List all the reports of the user, with some basic information, and there are buttons in order to UPDATE & DELETE


- `/profile/me` 
  - ProfilePageComponent
  - user only
  - my details
  - my reports
  
  


## Components

- Login Form Component
- Signup Form Component
- TypeofReform Component
- StyleofReform Component
- PartialReformFeatures Component

- Address Form component (Output when submit)
  - Input: address, search radious or neighbourhood_id 
  - Output: ( list of Witnesses painted in Witness-Component
	      list of Witnesses- painted in map
	      metrics - painted in Metrics-Component)
              buttons to Save Report , View Report, Download Report ---> Painted in Footer-Component

- WitnessesList Component
- WitnessesMap Component
- Metrics Component
- Footer Component
- Report Component 
- Graphics Component (child of Report Component)


## Services

- Auth Service
  - auth.login(user)
  - auth.signup(user)
  - auth.logout()
  - auth.me()
  - auth.getUser() // synchronous
- Report Service
  - report.list()
  - report.search(terms)
  - report.create(data)
  - report.detail(id)
  - report.delete(id)
  - report.update(id)
  

# Server

## Models

User model

```
username - String // required
email - String // required & unique
password - String // required
telephone - String
sex: male / female;
dateOfBirth: Date;
Reports - [ObjectID<Report>]
```

Report model

```

_id: ObjectID,
witnessess: { 
         idPortalWeb: number;  // code from which Web Portal comes this item (it comes from the API)
	 refItem: string; //the refrence_id of the Item  (it comes from the API)
}
typeReform : enum('partial','integral','metrics')
styleReform : enum ('classic','standard','deluxe')
filters : { bathrooms: integer;
            rooms: integer;
	    swimming-pool: enum (1,0)  // 1- has swimming pool, 0- no swimming pool
	    air-condition: enum (1,0)
	    terrace: enum (1,0)
            .......
}

metrics : { newConstruccion: {
                              witnessessSell: array [{idPortalWeb: number;
						  refItem: string;},...]
                              witnessessRent: array [{idPortalWeb: number;
						  refItem: string;},...]
			      pricem2 : integer;
			      pricerentm2: integer;},
	   { toReform: {
                              witnessessSell: array [{idPortalWeb: number;
						  refItem: string;},...]
			      witnessessRent: array [{idPortalWeb: number;
						  refItem: string;},...]
			      pricem2 : integer;
			      pricerentm2: integer;},
           { reformed: {
                              witnessessSell: array [{idPortalWeb: number;
						  refItem: string;},...]
			      witnessessRent: array [{idPortalWeb: number;
						  refItem: string;},...]
			      pricem2 : integer;
			      pricerentm2: integer;},

partialReformedFeatures: {
	   walls: integer (limit: from 0 to 100);
	   kitchen: integer (limit: from 0 to 100);
           bathrooms: integer (limit: from 0 to 100);
           electricalInstallation: integer (limit: from 0 to 100);
           waterInstallation: integer (limit: from 0 to 100);
   	}
budgetDetail: {
	councilTaxes: integer;} // pending to define the differents sections and detail on the budget
        painting: 
	paleta: 
	furniture: 
	installation: { air-condition:
                        heating:
                        electric:
                        water:
                        ...... }
	}				



```

## API Endpoints (backend routes)

- GET /auth/me
  - 404 if no user in session
  - 200 with user object
- POST /auth/signup
  - 401 if user logged in
  - body:
    - username
    - email
    - password
  - validation
    - fields not empty (422)
    - user not exists (409)
  - create user with encrypted password
  - store user in session
  - 200 with user object
- POST /auth/login
  - 401 if user logged in
  - body:
    - username
    - password
  - validation
    - fields not empty (422)
    - user exists (404)
    - passdword matches (404)
  - store user in session
  - 200 with user object
- POST /auth/logout
  - body: (empty)
  - 204

- POST /report (route protected)
  - body:
    - { object with properties of Report Model}  // Save a new Report

- PUT /report:id (route protected)
   - body:
    - { object with properties of Report Model}  // Update the Report


  	
- DELETE /report/:id (route protected)
  - body: (empty - the report is already stored in the session)
  - remove from Reports collection
  - remove from Users collection--> Users.Reports[]  // we have to delete the reportID from the array of ReportsID
  
- GET /report (route protected)
   - Get the list of reports of the current user.
  

## Links

### Trello/Kanban

[Link to your trello board](https://trello.com/b/CCtXuMCQ/houserenewal) or picture of your physical board

### Git

The url to your repository and to your deployed project

[Client repository Link](https://github.com/xapi1972/front-end-wellnessrenovate)
[Server repository Link](https://github.com/xapi1972/wellnessRenovate.git)

[Deploy Link](http://heroku.com)

### Slides

The url to your presentation slides

[Slides Link](https://slides.com/albertfarre/deck-2)
