# Let's build a new Polymer application

> Polymer workshop

The goal of this workshop is to create new Polymer application using [Polymer starter kit](https://github.com/PolymerElements/polymer-starter-kit).

Each step is in separate [branch](https://github.com/jskvara/polymer-workshop-lviv/branches/active) of this repository.

## Step 1 - Polymer Starter Kit

At first, we should install [Polymer Starter Kit](https://github.com/PolymerElements/polymer-starter-kit#get-the-code) which helps us to build new Polymer application very quickly.

Polymer Starter Kit provides us responsive layout with material design elements.

After successful installation we should be able to run the application from our console:

```sh
gulp serve
```

And we should see in our browser the following page:

![Notes application](https://github.com/jskvara/polymer-workshop-lviv/blob/master/docs/polymer-starter-kit.png)

When you save any file, Polymer Starter Kit automatically refreshes your browser, so you don't need to reload your browser manually.

## Step 2 - Theming

> In this step we're going to change colours of our application and we learn how to style Polymer application.

When you open `styles/app-theme.html` file you can see basic css styles for your application.
You can see there special *css variables* starting with two dashes, for example: `--variable-name`.

You can edit colours of your application just by editing these css variables.

When you visit [https://www.materialpalette.com/](https://www.materialpalette.com/) you can choose colours for your application from material design palette and use these values.

```css
/* styles/app-theme.html */
:root {
    --dark-primary-color: #7B1FA2;
    --default-primary-color: #9C27B0;
    --light-primary-color: #E1BEE7;
    --text-primary-color: #ffffff; /*text/icons*/
    --accent-color: #FF5722;
    --primary-background-color: #c5cae9;
}
```

> Step 2 changes: [https://github.com/jskvara/polymer-workshop-lviv/compare/master...step-02](https://github.com/jskvara/polymer-workshop-lviv/compare/master...step-02)

## Step 3 - Add new page

Adding new pages is really easy. At first we need to update `app/routing.html` file and add new route:

```js
// app/routing.html
      ...
      app.route = 'user-info';
      app.params = data.params;
    });

    page('/devfest', scrollToTop, function() {
      app.route = 'devfest';
    });

    page('/contact', scrollToTop, function() {
    ...
```

Now we can add our new section to `index.html` file:

```html
<!-- index.html -->
              ...
              </paper-material>
            </section>

            <section data-route="devfest">
              <paper-material elevation="1">
                <h2 class="page-title">Devfest</h2>
              </paper-material>
            </section>

          </iron-pages>
          ...
```

And we also need to add our page to left menu. We're going to use new icon image.

Visit Iron Icons element on element catalog page, where you can choose any image from SVG icon sets.
[https://elements.polymer-project.org/elements/iron-icons?active=iron-iconset-svg&view=demo:demo/index.html](https://elements.polymer-project.org/elements/iron-icons?active=iron-iconset-svg&view=demo:demo/index.html)

Let's choose `social:school` icon. Because this image is in the social icons set icons, we need to import `social-icons` element.

We add new element imports to `app/elements.html` file, where we put all imported elements:

```html
<!-- app/elements.html -->
...
<link rel="import" href="../bower_components/iron-icons/iron-icons.html">
<link rel="import" href="../bower_components/iron-icons/social-icons.html">
<link rel="import" href="../bower_components/iron-pages/iron-pages.html">
...
```

And we can add link to our page with the chosen icon:

```html
<!-- index.html -->
            ...
            <span>Contact</span>
          </a>

          <a data-route="devfest" href="/devfest" on-click="onDataRouteClick">
            <iron-icon icon="social:school"></iron-icon>
            <span>Devfest</span>
          </a>
        </paper-menu>
      </paper-scroll-header-panel>
      ...
```

> Step 3 changes: [https://github.com/jskvara/polymer-workshop-lviv/compare/step-02...step-03](https://github.com/jskvara/polymer-workshop-lviv/compare/step-02...step-03)

## Step 4 - New Polymer element

> In this step we're going to create a new Polymer element

We add a list of speakers to our devfest page using our own `devfest-speakers` element.

Create a new directory `devfest` in `app/elements` folder. In this directory we create a new file `devfest-speakers.html` with the following content:

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
<link rel="import" href="../../../bower_components/polymer/polymer.html">

<dom-module id="devfest-speakers">
  <template>
    <h2>Devfest speakers</h2>
  </template>

  <script>
    (function() {
      'use strict';

      Polymer({
        is: 'devfest-speakers'
      });
    })();
  </script>
</dom-module>

```

This is the simplest possible Polymer element. We need to specify name of the element in `id` attribute and also in `is` property in Polymer object.

Custom elements should have prefix separated by dash (`-`) and should be placed in file with the same name as element.

When we want to use our element, we need to import it in `elements.html` file:

```html
<!-- app/elements.html -->
...
<link rel="import" href="my-list/my-list.html">

<link rel="import" href="devfest-speakers/devfest-speakers.html">

```

We can use custom element on our page:

```html
<!-- index.html -->
              ...
              <paper-material elevation="1">
                <h2 class="page-title">Devfest</h2>
                <devfest-speakers></devfest-speakers>
              </paper-material>
              ...
```

> Step 4 changes: [https://github.com/jskvara/polymer-workshop-lviv/compare/step-03...step-04](https://github.com/jskvara/polymer-workshop-lviv/compare/step-03...step-04)

## Step 5 - Improve our element

We're going to add list of speakers to our element. At first we add JavaScript collection to our element:

```js
//  app/elements/devfest-speakers/devfest-speakers.html
    ...
    Polymer({
        is: 'devfest-speakers',
        ready: function() {
          this.speakers = [
            {name: 'Jakub', title: 'Polymer developer'},
            {name: 'Jana', title: 'Dart developer'}
          ];
        },
      });
    })();
    ...
```

We use `<template>` element to print this collections.
Don't forget to use your template bindings `{{variable}}` in html elements or attributes, otherwise it won't work.

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
    ...
    <h2>Devfest speakers</h2>

    <template is="dom-repeat" items="{{speakers}}">
      <div>{{item.name}}</div>
    </template>
  </template>
  ...
```

We can use `<paper-card>` element to list our speakers in better way.
You can see paper-card element examples on demo page in elemets catalog:
[https://elements.polymer-project.org/elements/paper-card?view=demo:demo/index.html](https://elements.polymer-project.org/elements/paper-card?view=demo:demo/index.html)

At first we need to import the element in our `devfest-speakers` element:

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
<link rel="import" href="../../../bower_components/polymer/polymer.html">
<link rel="import" href="../../../bower_components/paper-card/paper-card.html">

<dom-module id="devfest-speakers">
...
```

We can use the `paper-card` element:

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
    ...

    <template is="dom-repeat" items="{{speakers}}">
      <paper-card heading="{{item.name}}">
        <div class="card-content">
          <p>{{item.title}}</p>
        </div>
      </paper-card>
    </template>
    ...
```

Last thing we need to fix is to update css styles for `paper-card` element. We can include the `<style>` directly in our `devfest-speakers` element.

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
...

<dom-module id="devfest-speakers">
  <style>
    paper-card {
      display: block;
      margin-bottom: 2ex;
    }
  </style>

  ...
```

> Step 5 changes: [https://github.com/jskvara/polymer-workshop-lviv/compare/step-04...step-05](https://github.com/jskvara/polymer-workshop-lviv/compare/step-04...step-05)

## Step 6 - Use `iron-ajax` element

> We're going to show data from API fetched via AJAX

You can easily fetch data via AJAX using `iron-ajax` element.
We use local json data for this workshop, but you can load the data from any url on the internet.

We add `app/api/speakers.json` file to our application, with the following structure:

```json
[
 {
  "name": "Jana Moudrá",
  "title": "Co-Founder"
 },
 {
  "name": "Jakub Škvára",
  "title": "Frontend engineer"
 }
]
```

Now we're going to import `iron-ajax` element in `devfest-speakers.html` file.

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
...
<link rel="import" href="../../../bower_components/paper-card/paper-card.html">
<link rel="import" href="../../../bower_components/iron-ajax/iron-ajax.html">

<dom-module id="devfest-speakers">
...
```

And we can use the element:

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
  ...
  <template>
    <iron-ajax
      auto
      url="/api/speakers.json"
      handle-as="json"
      last-response="{{speakers}}"></iron-ajax>

    <h2>Devfest speakers</h2>
    ...
```

And we also need to specify, we're using `speakers` property with type of `Array`.

```js
// app/elements/devfest-speakers/devfest-speakers.html
        ...
        is: 'devfest-speakers',
        properties: {
          speakers: Array
        }
      });
      ...
```

You should now see the list of speakers from the JSON file rendered on our page.

We can also log the speakers variable to the console adding *observer* to our `speakers` property.

```js
// app/elements/devfest-speakers/devfest-speakers.html
        ...
        properties: {
          speakers: {
            type: Array,
            observer: '_speakersChanged'
          }
        },
        _speakersChanged: function(speakers) {
          console.log(speakers);
        }
      });
      ...
```

> Step 6 changes: [https://github.com/jskvara/polymer-workshop-lviv/compare/step-05...step-06](https://github.com/jskvara/polymer-workshop-lviv/compare/step-05...step-06)

## Step 7 - Add `paper-input` element

> We add new input to easily filter speakers.

At first we add `paper-input` element to our application.

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
...
<link rel="import" href="../../../bower_components/paper-card/paper-card.html">
<link rel="import" href="../../../bower_components/paper-input/paper-input.html">
<link rel="import" href="../../../bower_components/iron-ajax/iron-ajax.html">
...
```

And we place this input to our element:

```html
<!-- app/elements/devfest-speakers/devfest-speakers.html -->
    ...
    <h2>Devfest speakers</h2>

    <paper-input label="Filter" value="{{filter}}"></paper-input>

    <template is="dom-repeat" items="{{speakers}}">
    ...
```

We also need to add two new properties: `filter` for the value of input element and also `speakersFiltered` property with filtered content of speakers property.

We set the `speakersFiltered` to be computed from speakers `property` with function `_computeSpeakersFiltered(speakers, filter)`.

```js
// app/elements/devfest-speakers/devfest-speakers.html
          ...
          },
          filter: String,
          speakersFiltered: {
            type: Array,
            computed: '_computeSpeakersFiltered(speakers, filter)'
          }
        },
        _speakersChanged: function(speakers) {
        ...
```

And we define the function. If `filter` variable from paper-input filed is empty, we return the whole `speakers` collection.
Otherwise we compare the `filter` value with the objects from original `speakers` collection and return only the speakers that satisfy the condition.

```js
        ...
        },
        _computeSpeakersFiltered: function(speakers, filter) {
          if (filter === '') {
            return speakers;
          }

          return speakers.filter(function (speaker) {
            return speaker.name.substring(0, filter.length) === filter;
          })
        }
      });
      ...
```

Finally we need to update our `dom-repeat` element to use `speakersFiltered` property instead of `speakers`.

```js
    ...
    <paper-input label="Filter" value="{{filter}}"></paper-input>

    <template is="dom-repeat" items="{{speakersFiltered}}">
      <paper-card heading="{{item.name}}">
      ...
```

> Step 7 changes: [https://github.com/jskvara/polymer-workshop-lviv/compare/step-06...step-07](https://github.com/jskvara/polymer-workshop-lviv/compare/step-06...step-07)
