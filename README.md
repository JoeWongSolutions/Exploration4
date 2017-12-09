# Exploration 4
CS4830 Exploration 4 Assignment: Explore Ember.js from Job Description

Explorer: Joe Wong

Course: CS4830

Date: 12/8/2017

# Example of Running Server on my EC2
* [Ember Quickstart App](http://joewong.me/CS4830/Exploration4/ember-quickstart/dist)
* [Ember Quickstart App "Programmers"](http://joewong.me/CS4830/Exploration4/ember-quickstart/dist/programmers)
* [Ember Quickstart App "Scientists"](http://joewong.me/CS4830/Exploration4/ember-quickstart/dist/scientists)
# Sources
* [State Farm Job Description](https://statefarm.csod.com/ats/careersite/JobDetails.aspx?site=1&id=1960)
* [Ember.js Quickstart Tutorial](https://guides.emberjs.com/v2.17.0/getting-started/quick-start/)
* [Ember.js Deployment](https://guides.emberjs.com/v2.17.0/tutorial/deploying/)
* [Fix root path for deployment](https://github.com/ember-cli/ember-cli/issues/551)

## Components Explored
* [Install Ember.js](#install-ember.js)
* [Initial Setup](#initial-setup)
* [Creating an ember component](#creating-an-ember-component)
* [Creating a model](#creating-a-model)
* [Linking the model to component](#linking-the-model-to-component)
* [Deployment](#deployment)

* [Troubleshooting Tidbits](#troubleshooting-tidbits)
* [Journal](#journal)

#### What this exploration covers:
Looking into the Software Developer position at my summer internship, I noticed that they prefer ember.js as one of the skills for developers. I had not used this framework before so I decided to look deeper into it. This framework is dependent on Node.js and NPM but I will not go into these topics as I have already covered them in previous explorations.

#### Install Ember.js
This is pretty straight-forward, we need to correctly install Ember.js onto our EC2 so that we can use it. 
```Shell
npm install -g ember-cli@2.17
```

#### Initial Setup
Create a directory somewhere in your public_html folder, I'll call mine Exploration4/.
```Shell
mkdir public_html/CS4830/Exploration4
```
Now we need to navigate to the folder and initialize with ember.
```Shell
cd public_html/CS4830/Exploration4
ember new ember-quickstart
```
This will create a huge directory called ember-quickstart that contains all of the necessary files for development. A default application was created with a ember logo and landing page. We will be editing the files in this directory to meet our needs.
If we want to run the default application, we can use:
```Shell
ember serve
```
Then if we navigate to our "localhost:4200" we can see our new app.

#### Creating an ember component
We start by generating a component. Components are like templates that can use models to fill in information defined in the component. We can create multiple models that contain information and direct them to the component for easy rendering.
```Shell
ember generate component people-list
```
Using this command automatically generates a component called people-list in the components folder. Now we can edit people-list.hbs with our template:
```XML
<h2>{{title}}</h2>

<ul>
  {{#each people as |person|}}
    <li>{{person}}</li>
  {{/each}}
</ul>
```
As we can see, ember uses handlebars/mustache syntax to bind data to templates. 

#### Creating a model
Models in ember are not stand alone objects. We access models through objects called routes. The easist way for me to understand routes was to think about the address bar. When you want to search for a specific file, you enter the address or "route" into the address bar. Something like "http://mywebsite.me" can be routed to something like "http://mywebsite.me/programmers" just by adding the "/programmers". Inside this route, there will be a model which holds the names of notable programmers. Lets create this route:
```Shell
ember generate route programmers
```
Now if we navigate to app/routes we will see a file called programmers.js. Inside this file we need to add:
```Javascript
import Route from '@ember/routing/route';

export default Route.extend({
  model() {
    return ['Joe', 'Nishant', 'Nick'];
  }
});
```

#### Linking the model to component
So now we have a component and a model. Models are normally used by ember templates but since we want to use it in our component, we need to modify our template to use the component. Navigate to the app/templates/programmers.hbs file and add:
```Javascript
{{people-list title="List of Scientists" people=model}}
```
This line of code tells the template that "people-list" is the component to use. Parameters after the component name will correspond to the data in the component. Here, we used title and a model called people. "model" refers to the model in programmers.js. Each template refers to it's own route file and call any variables/functions in the file.

#### Deployment
We can test our code using the aformentioned command:
```Shell
ember serve
```
However this is only intended during the development process. If we are ready to distribute our product to the world we need to build the distribution.
```Shell
ember build
ember build --env production
```
Both of these commands will build a usable product from our code base but the second command adds more security to the code and minifies everything to improve speed. After the build is finished, we will see our finished code in the folder called "dist". Simply copying the contents of this folder into an apache webserver will run the code.

## Troubleshooting Tidbits
One of the biggest issues I had with this project was understanding deployment. My webserver was configured to not run the ember project from the root. To fix this, I found a solution [Fix root path for deployment](https://github.com/ember-cli/ember-cli/issues/551) that suggested to modify the root path in the config/environment.js file.

## Journal
This exploration challenged me to think in a more structured way. While simplifying MVC, ember.js does have somewhat of a learning curve. The tutorial was only the tip of the iceberg. I found out that ember.js along with Angular are leading the frontend development frameworks. Ember is a couple years younger than Angular and some say it is easier to pick up. 