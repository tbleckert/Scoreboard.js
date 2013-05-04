# Scoreboard.js

Scoreboard.js is a personal project that I finally got around to make the final touches and now I'm making it public here on Github.

## Why did I build it?

The main reason why I started working on Scoreboard.js was to learn Mootools (yes, Scoreboard.js i built with Mootools). Also I needed a scoreboard for a game that I'm currently coding on.

## So what is it?

It's a…ehrm…HTML scoreboard widget. It's easy to create a theme, adding a sport and has a super simple api. Besides from being a widget that displays the score, time, etc…it also contains a notification thing that you can show when someone scored, got a red card or anything really.

## How do I use it?

First of all you need a template. Scoreboard.js makes use of declarative bindings to make it easy for every one to work with. So a scoreboard template is actually plain HTML with some data bindings like this:

```html
<article class="scoreboard" id="scoreboard">
	<div class="scores">
		<span class="team" data-scoreboard-bind="homeTeamName"></span>
		<span class="score">
			<span data-scoreboard-bind="homeTeamGoals"></span>
			-
			<span data-scoreboard-bind="awayTeamGoals"></span>
		</span>
		<span class="team" data-scoreboard-bind="awayTeamName"></span>
	</div>
	<span class="time" data-scoreboard-bind="time"></span>
</article>
```

With that exact markup you have the base for a fully working scoreboard with the default theme.

Next some javascript to create the scoreboard.

```javascript
var scoreboard = new Scoreboard();
```

Finally include the necessary files:

```html
<!-- Inside <head> -->
<link rel="stylesheet" href="source/theme.css">

<!-- Before </body> -->
<script src="vendor/mootools.js"></script>
<script src="vendor/mootools-more.js"></script>
<script src="source/scoreboard.min.js"></script>
```

## API

There is several useful API methods to call. Here is a full list:

__scoreboard.set( what, value )__

Set any option/setting to the given value. If the option doesn't exist, it creates it.
- - -

__scoreboard.get( what )__

Returns the value of the given option/setting.
- - -

__scoreboard.startTime()__

Starts the timer.
- - -

__scoreboard.stopTime()__

Stops the timer.
- - -

__scoreboard.resetTimer()__

Resets the timer to default.
- - -

__scoreboard.increaseTime()__

Adds one second to the timer.
- - -

__scoreboard.decreaseTime()__

Removes one second from the timer.
- - -

__scoreboard.addGoal( team )__

Adds 1 goal to the given team. team should be a string containing _home_ or _away_.
- - -

__scoreboard.removeGoal( team )__

Removes 1 goal from the given team.
- - -

__scoreboard.resetGoals( team )__

Reset goals for the given team to the default.
- - -

__scoreboard.toJSON()__

If you need to save the current state of the scoreboard, you can call the __toJSON()__ method. That will return all data stored in the scoreboard as JSON.
- - -

__scoreboard.showMessage( id, data, position )__

When you need to show a message/notification you will call this method. First parameter is a string with the ID of the template (more on this further down). The second parameter is a object containing the data for the template. The last parameter is the position where you want the message to appear, possible values is the same as for the scoreboard (default is bottomCenter).
- - -

__scoreboard.hideMessage()__

This method will simply hide the currently showing message. Note that the message will automatically dissapear after 3 seconds (or based on the option _duration_).
- - -

__scoreboard.applyFilter( hook, function )__

More on this further down.

## Events

Every method (except _get_) that you call fires a event. The event os prefixed with __on__ and the method starts with a capital letter like this:

```javascript
scoreboard.addEvent('onStartTime', function () {
	alert('Time started!');
});
```

Methods that you can pass parameters to returns those in the event. Take the __set__ (that is also a bit different, the event is called _change_) method:

```javascript
scoreboard.addEvent('change', function ( what, value ) {
	alert('The option: ' + what + ' was changed to: ' + value);
});
```

Another example is the addGoal method that is called onScore:

```javascript
scoreboard.addEvent('onScore', function ( team ) {
	alert(team + ' scored!');
});
```

## Filters

There is certain filters you can use to manipulate the values in some functions. A list of available filters and a example is displayed below:

__addHomeTeamGoal__
- - -
__addAwayTeamGoal__
- - -
__removeHomeTeamGoal__
- - -
__removeAwayTeamGoal__
- - -
__increaseTime__
- - -
__decreaseTime__

### Example

To filter a value you use the _applyFilter (hook, function)_ method. The first parameter is the hook (from the list above) and the second one is the function. In the function you will be provided with the value generated from the function the filter lives in. Use it like this:

```javascript
scoreboard.applyFilter('addHomeTeamGoal', function ( goal ) {
	// Logic logic logic…
	return goal + 2;
});
```

The function you set should always return a value. In the example we add 2 extra goals to the home team.

## Options

There is several options that you can set. All options are optional though, so to get it up and running you don't need to do anything. Default value is in parentheses.

__element (scoreboard)__

This is the ID of the main element.
- - -

__position (topCenter)__

You can let Scoreboard.js absolute position your scoreboard with the this option. Position can be one of these: topLeft topCenter topRight middle bottomLeft bottomCenter bottomRight. You can also set it to _null_ and no positioning will happen.
- - -

__homeTeamName (Home)__

The full home team name.
- - -

__awayTeamName (Away)__

The full away team name.
- - -

__homeTeamLogo (null)__

Path to the logo for the home team.
- - -

__awayTeamLogo (null)__

Path to the logo for the home team.
- - -

__homeTeamShort (null)__

Three letter version of the home team name. If set to _null_ the three first letters of the full name will be used.
- - -

__awayTeamShort (null)__

Three letter version of the away team name. If set to _null_ the three first letters of the full name will be used.
- - -

__homeTeamGoals (0)__

Default goals of the home team.
- - -

__awayTeamGoals (0)__

Default goals of the away team.
- - -

__secondLength (1000)__

If you want the time to go faster or slower than a second, you can set secondLength to what you want, in ms.
- - -

__time (00:00)__

Default time.
- - -

__timeDirection (up)__

The direction of the time. Set to _down_ if you want to count down instead of the normal direction which is up.

__animationSpeed (300)__

Time in ms for the message animation when showing/hiding.
- - -

__duration (3000)__

How long (in ms) the message should be visible.
- - -

## Demos

Remember to watch the source.

* [Basic](http://scoreboardjs.tb-one.se/demos/basic.html/)
* [Messages](http://scoreboardjs.tb-one.se/demos/messages.html/)
* [Methods](http://scoreboardjs.tb-one.se/demos/methods.html/)
* [Extend](http://scoreboardjs.tb-one.se/demos/extend.html/)