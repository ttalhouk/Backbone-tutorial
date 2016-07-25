# Backbone JS notes  
-*Codeschool course*  

## Models and Views  

Creating a Model:
```javascript
// creating a model Class
var TodoItem = Backbone.Model.extend({});

// creating an instance
var todoItem = new TodoItem(
  { description: 'Pick up milk', status: 'incomplete', id: 1 }
);
```
Typical to capitalize first letter of a class

**Working with object**  

* `todoItem.get('description');` this results in "Pick up milk"
* `todoItem.set({status: 'complete'});` updates status to complete
* `todoItem.save();` sync to server configureation needed

Creating Views:
```javascript
// creating a view Class
var TodoView = Backbone.View.extend({});

// creating an instance
var todoView = new TodoView({ model: todoItem });
```

Rendering a View
```javascript
var TodoView = Backbone.View.extend({
  render: function(){ // renders HTML
    var html = '<h3>' + this.model.get('description') + '</h3>';
    $(this.el).html(html);
  }
});
```
Every view has a top level element `.el` in the code above
```javascript
var todoView = new TodoView({ model: todoItem }); // instanciates view
todoView.render(); // executes render function
console.log(todoView.el); log result

// <div>
//   <h3>Pick up milk</h3>
// </div>
```

## Model Details  

To get JSON data for model first set the url for the model.

* `todoItem.url = '/todo';` // sets the route
* `todoItem.fetch();` // gets the JSON from the route
* `todoItem.get('description');` // gets the value from key = 'description'

The url can be established when creating the Model.

```javascript
var TodoItem = Backbone.Model.extend({urlRoot: '/todos'});

var todoItem = new TodoItem({id: 1})
// Get Route
todoItem.fetch(); // goes to route /totos/1

// Put Route
todoItem.set({description: 'Pick Up clothes'});
todoItem.save() // goes to put route /totos/1

// Create Route
var todoItem = new TodoItem();
todoItem.set({description: 'Fill prescription'});
todoItem.save(); // Post request

// Destroy Route
todoItem.get('id');
todoItem.destroy(); // destroys current todoItem
```

To convert object to JSON
`todoItem.toJSON();`

During Object creation, default values can  be specified.

```javascript
var TodoItem = Backbone.Model.extend({
  defaults: {
    description: 'Empty todo...',
    status: 'incomplete'
  }
});
```

**Models can Have Events**  
```javascript
todoItem.on('event-name', function(){ // callback
  alert('event-name happened!');
});

todoItem.trigger('event-name'); // triggers call back

// Special Events


todoItem.on('change', doThing); // event set to doThing on change

todoItem.set({description: 'Fill prescription.'}); // triggers change

todoItem.set({description: 'Fill prescription.'},{silent:true}); // silents event-name

todoItem.off('change', doThing); // turns event off
```

*Built in events include*

- `change` listens for change
- `change:<attr>` listens for change in specified attribute
- `destroy` listens for  object to be destroyed
- `error` listens for when model saves or vadidation fails
- `sync` listens for sync with server successful
- `all` Any trggered event.

## Views

`.el` defaults to `<div></div>` but...

``` javascript
var SimpleView = Backbone.View.extend({tagName: 'li'}); // sets .el tag
var simpleView = new SimpleView();

console.log(SimpleView.el); // <li></li>
```

Other properties can be added as well

```javascript
var TodoView = Backbone.View.extend({
  tagName: 'article',
  id: 'todo-view',
  className: 'todo'
});

var simpleView = new SimpleView();

console.log(SimpleView.el); // <article class="todo" id="todo-view"></article>
```
The .el also allows for Jquery to target the element like this to add HTML...
`todoView.$el.html();`

**Using Templates**

Backbone uses Underscore library to do tempting.
```javascript
var TodoView = Backbone.View.extend({
  ...
  template: _.template('<h3><%= description %></h3>'), // assigns template to view
  render: function(){
  var attributes = this.model.toJSON();
  this.$el.html(this.template(attributes));} // sets html of element to template passing in model
});

var todoView = new TodoView({ model: todoItem });// sets model for view
todoView.render();
console.log(todoView.el);

// <article id="todo-view" class="todo">
//   <h3>Pick up milk</h3>
// </article>
```
**View Events**
```javascript
var TodoView = Backbone.View.extend({
  events: {
    "click h3": "alertStatus" // sets the alert status for the h3 of this element
  },
  alertStatus: function(e){
    alert('Hey you clicked the h3!');
  }
});

// Views can have many events

var DocumentView = Backbone.View.extend({

events: {
"dblclick"                :"open", // anywhere in the element
"click .icon.doc"         :"select", // nested class
"click .show_notes"       :"toggleNotes", // specific class
"click .title .lock"      :"editAccessLevel", // multiple classes
"mouseover .title .date"  :"showToolTip"
 }, ...
});
// format
var SampleView = Backbone.View.extend({
  events: {
    "<event> <selector>": "<method>"
  },
});
```

JQuery events available  
change click dblclick focus focusin
focusout hover keydown keypress load
mousedown mouseenter mouseleave mousemove mouseout
mouseover mouseup ready resize scroll
select unload
