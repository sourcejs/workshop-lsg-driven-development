# Living Style Guide Driven Development Workshop

Follow workshop scenario and [tagged](/releases) repo versions to reproduce the steps.

Screencast coming soon.

This repo contains [SourceJS](http://sourcejs.com) Living Style Guide Platform `user` folder with engine configuration and components library contents.

## Scenario

### Step 0: Set-up

Follow [SourceJS install instructions](http://sourcejs.com/docs/base/#install) to set-up the Living Style Guide engine.

Note that Style Guide driven development workflow could be achieved also by other tools as well. Check this [in-depth overview](http://www.smashingmagazine.com/2015/04/13/an-in-depth-overview-of-living-style-guide-tools/) of the most powerful tools for organizing Front-end documentation.

### Step 1: Build your first component

After init, you will have few stub Spec pages prepared. Open `sourcejs/user/specs/button/index.src.html` and define you demo markup:

```
<link rel="stylesheet" href="css/button.css">

<h1>Button Spec</h1>

<section class="source_section">
    <h2>Basic button</h2>

    <div class="source_example">
        <a href="#777" class="btn">Button</a>
    </div>
</section>
```

Run SourceJS and open the Spec to view the result http://localhost:8080/specs/button

Use [browser-sync](https://github.com/sourcejs/sourcejs-contrib-browser-sync) plugin to develop your Spec next to browser window with auto-update.

Following Living Style Guide driven development, all components and UI element are developed right in the documentation. Leaving all component states and modifiers in the same page helps to test Front-end faster and gives a better overview of element features.

### Step 2: CSS Doc

Install [sourcejs-contrib-dss](https://github.com/sourcejs/sourcejs-contrib-dss) plugin, create new spec and follow simple syntax for documenting your components right from CSS.

```
/button-dss
    button.css
    readme.md
    info.json
```

* `button.css` - main file with styles and documentation
* `readme.md` - text that will be included on the top of spec page, before DSS examples
* `info.json` - [meta file](http://sourcejs.com/docs/info-json/) for SourceJS engine

DSS syntax example:

```css
/**
  * @name Default
  *
  * @state :hover - Highlights when hovering.
  *
  * @markup
  *   <a href="#777" class="btn">Button</a>
  */


.btn {
    display: inline-block;
    background-color: cyan;
    padding: 10px 20px;
    border-radius: 5px;
    color: black;
    text-decoration: none;
    font-family: Arial, sans;
    }
```


### Step 3: Add Templating

In this step we will add [EJS](http://ejs.co/) templates, to get rid of HTML copy-pasting issue. Templates allow to re-use component code from different endpoints, and constantly sync your Living Style Guide with the original source.

Copy `button` Spec to the new folder, create few templates with `*.ejs` extension and import them into your `index.src.html` file:

```
<div class="source_example">
    <% include templates/button.ejs %>
    <%- include('templates/button.ejs', { mod: "big" }) %>
    <% include templates/button-group.ejs %>
</div>
```

EJS template examples:

```
<a href="#777" class="btn <% if (locals.mod) { %>__<%- mod %><% } %>">Button</a>
```

```
<div class="btn_group">
    <a href="#777" class="btn">Button1</a>
    <a href="#777" class="btn">Button2</a>
    <a href="#777" class="btn">Button3</a>
</div>
```

Server-side EJS is by default included into SourceJS engine, use it to construct your Specs or describing component HTML. EJS is available as client-side module as well, but you're free to use any other technologies like Handlebars, Mustache and etc.

### Step 4: Require component in other project

Initialize and link Bower module in `button-component` folder:

```
bower init && bower link
```

After linking it, you will be able to install it locally to any other projects using `bower link button-component` command.


#### Demo integration

As an example, we took [generator-gulp-webapp](https://github.com/yeoman/generator-gulp-webapp) application template.

```
npm i yo generator-gulp-webapp -g
mkdir webapp && cd webapp
yo gulp-webapp
```

And installed our component there through bower:

```
bower link button-component
```

Now when our dependencies are ready, we can link our component to `webapp/app/index.html` page.

```html
<script src="//github.com/mde/ejs/releases/download/v2.3.1/ejs.min.js"></script>
<link rel="stylesheet" href="/bower_components/button-component/css/button.css">

<!-- Our btn -->
  <span id="btn"></span>

  <script>
    if(self.fetch) {
      fetch('/bower_components/button-component/templates/button.ejs')
        .then(function(response) {
            return response.text();
        }).then(function(body) {
            document.getElementById('btn').outerHTML = ejs.render(body, {mod: "big"});
        });
    } else {
        alert('To view the demo, your browser need to support fetch.')
    }
  </script>
<!-- /Our btn -->
```

Note that you can use any templating engine using this approach.

### Step 5: Nested bundles

Add more example bundles to your components library.

```
cd sourcejs/user/specs
git clone https://github.com/sourcejs/example-bootstrap-bundle
```

Install bundle dependencies.

```
cd example-bootstrap-bundle
bower install
```

Restart SourceJS and view the bundle by this URL http://localhost:8080/specs/example-bootstrap-bundle

___

Initialized with [SourceJS](http://sourcejs.com) 0.5.2 - Living Style Guide Engine and Maintenance Environment for Front-end Components.

Copyright © 2013-2015 [Sourcejs.com](http://sourcejs.com)