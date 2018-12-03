---
date: 2018-12-03T06:00:00+06:00
lastmod: 2018-12-03T17:30:00+06:00
title: Phaser 3 Animation Using Local Json File
authors: ["foysalahamed"]
categories:
  - programming
tags:
  - phaser
  - animation
  - json
  - game
slug: phaser-3-animation-using-local-json-file
comments: false
toc: false
draft: false
---
While just showing the tile is an option, it's just a static image. No matter how cute your tiles look, they are just flat images if you don't animate them a bit. That's why you are going to learn to create animations with Phaser 3 using JSON file.

For this, you need a Sprite Sheet and a [JSON](https://www.json.org/) file which contains your animation data. A sprite sheet is an image that consists of several smaller images (sprites) and/or animations. Combining the small images in one big image improves the game performance, reduces the memory usage and speeds up the startup time of the game. For this tutorial, we will use this image:

![Cross Sprite Sheet](https://i.ibb.co/ZgLRSwc/crosscube.png)

I used this sprite sheet for my `Tic Tac Toe` game. It has 6 frames to show an animated Cross. To load this sprite sheet, use this code:

```javascript
this.load.spritesheet("crossCube", "crosscube.png", {
    frameWidth: 250,
    frameHeight: 250
});
```

Here, we are telling Phaser 3 to load sprite sheet and we are giving it a **key** which is `crossCube`, the **url** of that sprite sheet is `crosscube.png`. We are also providing its each **frameWidth** and **frameHeight** which is 250, in a separate object. Full signature of this function is:

`spritesheet(key, url, frameWidth, frameHeight, frameMax, margin, spacing)`

**key**: `string`
Unique asset key of the sheet file.

**url**: `string`
URL of the sheet file.

**frameWidth**: `number`
Width of each single frame.

**frameHeight**: `number`
Height of each single frame.

**frameMax**: `number`
How many frames in this sprite sheet. If not specified it will divide the whole image into frames.

**margin**: `number`
If the frames have been drawn with a margin, specify the amount here.

**spacing**: `number`
If the frames have been drawn with spacing between them, specify the amount here.


And use this code to load JSON object.

```javascript
this.load.animation('crossCubeAnimations', 'crossCube.json');
```

This JSON file has the required configuration data to show the animation. Go ahead and try to change the values to see what effects does it have on this animation.

```json
{
  "anims": [
    {
      "key": "drawCrossCube",
      "type": "frame",
      "frames": [
        {
          "key": "crossCube",
          "frame": 0,
          "duration": 0
        },
        {
          "key": "crossCube",
          "frame": 1,
          "duration": 0
        },
        {
          "key": "crossCube",
          "frame": 2,
          "duration": 0
        },
        {
          "key": "crossCube",
          "frame": 3,
          "duration": 0
        },
        {
          "key": "crossCube",
          "frame": 4,
          "duration": 0
        },
        {
          "key": "crossCube",
          "frame": 5,
          "duration": 0
        }
      ],
      "frameRate": 25,
      "duration": 0,
      "skipMissedFrames": true,
      "delay": 0,
      "repeat": 0,
      "repeatDelay": 0,
      "yoyo": false,
      "showOnStart": false,
      "hideOnComplete": false
    }
  ],
  "globalTimeScale": 1
}
```

**key**: `string`
The unique identifying string for this animation.

**type**: `string`
A frame based animation (as opposed to a bone based animation).

**frames**: `Array.<Phaser.Animations.AnimationFrame>`
Extract all the frame data into the frames array.

**frameRate**: `integer`
The frame rate of playback in frames per second (default 24 if duration is null).

**skipMissedFrames**: `boolean`
Skip frames if the time lags, or always advanced anyway?

**delay**: `integer`
The delay in ms before the playback will begin.

**repeat**: `integer`
Number of times to repeat the animation. Set to -1 to repeat forever.

**repeatDelay**: `integer`
The delay in ms before the a repeat playthrough starts.

**yoyo**: `boolean`
Should the animation yoyo? (reverse back down to the start) before repeating?

**showOnStart**: `boolean`
Should sprite.visible = true when the animation starts to play?

**hideOnComplete**: `boolean`
Should sprite.visible = false when the animation finishes?

**globalTimeScale**: `integer`
Global time scale of this animation.

Inside `frames` object, we are telling the sprite name and frame number of that sprite. Play this animation using this lines of code:

```javascript
let animName = "drawCrossCube";
// This is the sprite which we are going to animate
let turnCube = this.add.sprite(0, 0, 'crossCube', 0);
turnCube.on('animationcomplete', function (animation) {
    if (animation.key === animName) {
        /*
        Checking animation key to execute custom
        game logic after animation complete
        */
    }
}, this);
turnCube.anims.play(animName);
```

That's it. You will find the full project in this [repository](https://github.com/Lazyb0y/tic-tac-toe).
