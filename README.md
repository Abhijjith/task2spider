# SPIDER WEB DEV TASK 2.
Task 2 for spider inductions.

In this task, i made use of **GOOGLE ANALYTICS API** for displaying the data.

For this , a google analytics account is necessary. [Link](www.google.com/analytics/)

I used PHP with laravel framework and MySQL as my database.I initially learnt about the Google Analytics dashboard and found it interesting and hence i chose this.

The Google Analytics API is divided into a number of APIs. I am making use of the following APIs.
1.Management API: Provides access to Google Analytics configuration data like accounts, properties, views, goals…
2.Metadata API: Provides access to the list of columns (Dimensions, Metrics) so that we don’t need to hard code them inside our app.
3.Core Reporting API: Provides access to dashboard data, and most of the tasks are available through this API.

To start using the API, I created a new project on the [Google Developers Console](https://console.developers.google.com)

When you want to use a Google API you need to provide two things:
1) Your API credentials.  2) Your developer keys.
We need to enable the Analtyics API from the Google developers console. and create a new client ID. The above two mentioned will be available now.

Next we need to set up a Laravel project.
We need to require "google/api-client": "dev-master" in our composer.json and update the dependencies.

In my config folder, i created a new file called analtyics.php where i did all my configuration. it essentially includes the client id and other details regarding the client.

 i created a folder named src inside my app directory and created a file called GA_Service.php where I hav put all request logic. We also hav to add our path app/src in the autoload classmap section in the composer.json file.
 Whenever chamges are made in the composer.json file , run composer dump-autoload in the command window so that it will generate the autogenerated files.
 
 In the app/controllers directory , by default there will be a file called controller.php. Apart from that we create two files. BaseController.php and HomeController.php. HomeController.php will be used as the main app controller.
 We inject GA_Service into our controller, and when the user hits our index route, we check if he’s logged in. If not, we generate the authentication URL.
 
 The GA_Service::properties accepts an account ID, and returns the list of properties for that account. We basically have the same process, like retrieving accounts.
 So basically here we are using account ID as the parameter from a route. And display the info related to it. 
 We are storing it in the database with all the info. we hav function defined so that if the info is less than 1 hour old, we show the information in the form of pictorial data in the resultant HTML page.
 
 Using an ID from the list of properties and the account ID grabbed from the first part, we will query Google Analytics API for the list of available views for a given account property.
 
 In the browser, when hitting /views/{account_id}/{property_id} route, we should get data in the json format.Google Analytics gives us an etag attribute that can be used for caching the response so that we don’t have to query the API on every request.
 
As you can see from the screenshot, when the user selects an account we asynchronously change the property and the view accordingly. 
Our GA_Service::report accepts four arguments: a view ID, a start and end date and an array of metrics. These are the other parameters that are sent through the user's request.

By using various segments and filters , we accept the data and the parameters from the user.

I made  the graphs and charts by making use of [Highcharts](www.highcharts.com)

For the date range selection , i used the [bootstrap-daterangepicker](https://github.com/dangrossman/bootstrap-daterangepicker) and correspondingly we include the css and the js files. In views/home.blade.php , it delas with the above information.

I created a new file called GA_utils in our app/src folder. This is we’re i hav put miscellaneous functions.

First, we include the highchart.js file, then we use some configuration from their documentation.
Inside our controller, we created an array of values, and then we encoded those values to be used in our JS.

The list of server routes that im using are
Route::get('/', 'HomeController@index');        //essentially the home page.

Route::get('/login', 'HomeController@login');     // management

Route::get('/segments', 'HomeController@segments');

Route::get('/accounts', 'HomeController@accounts');

Route::get('/properties/{account_id}', ['uses' => 'HomeController@properties'])->where('account_id', '\d+');

Route::get('/views/{account_id}/{property_id}', ['uses' => 'HomeController@views'])->where([
                                                                                               'account_id',
                                                                                               '\d+',
                                                                                               'property_id',
                                                                                               '\d+'
                                                                                           ]);
Route::get('/metadata', 'HomeController@metadata');   //metadata

Route::post('/report', 'HomeController@report');    //reporting.

The documenation of this API can be referred here. [link](developers.google.com/analytics/devguides/reporting/core/v3/coreDevguide)

The documentation of laravel 5 can be referred here. [link](http://laravel.com/docs/master)

![alt tag](http://i.imgur.com/qejgnAf.png)

