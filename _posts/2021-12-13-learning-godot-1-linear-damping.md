---
subjects: []
title: 'Learning Godot 1: Linear damping'
subtitle: Why doesn't linear damping doesn't appear to do anything?
blurb: ''
date: 

---
I'm learning Godot at the moment so have resurected this half-baked blog in order to make some notes.

## The Problem

It's a top-down 2d game I'm making, I've got a _RidgidBody2d_ representing the player of said game. I want this _RidgidBody2d_ to slow down at different rates depending on the type of terain it's moving over. This is achieved through linear damping but precisely where this damping should be applied was a bit of mystery to me.

## The Solution

By default the _RigidBody2d_ will be damped at the global value set in project settings. To slow it down more (or less) over particular screen areas you can create an _Area2d_ and in it's physics overrides dialogue box you set _Space Override_ to _Replace_ and the _Linear Damp_ to whatever value you're after.

[Here's a link to the relevant commit in my project](https://github.com/tomgp/Godot-Learning/commit/e3348d6b401afa08af8f77b64439bac79b7f9a1d)

On the player representing node I also set the mode to _Character_ which stops it from rotating which is what I want at this stage.