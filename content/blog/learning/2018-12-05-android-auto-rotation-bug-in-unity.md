---
date: 2018-12-05T06:00:00+06:00
lastmod: 2018-12-05T17:30:00+06:00
title: Android Auto Rotation Bug in Unity
authors: ["foysalahamed"]
categories:
  - Programming
tags:
  - unity
  - c#
  - bug
  - rotation
slug: android-auto-rotation-bug-in-unity
comments: false
toc: false
draft: false
---
On Android device, Unity auto-rotation overrides user's device `Auto-rotation` setting while it respects user setting on the iOS device which determine if an app should be able to auto-rotate and change orientation between; Portrait, LandscapeLeft, etc.

Let's see the required code to solve this first:

```csharp
void OnApplicationFocus(bool haveFocus)
{
#if UNITY_ANDROID && !UNITY_EDITOR
    if (haveFocus) ToggleAutoRotation();
#endif
}

private static void ToggleAutoRotation()
{
    var autoRotationOn = DeviceAutoRotationIsOn();
    Screen.autorotateToPortrait = autoRotationOn;
    Screen.autorotateToLandscapeLeft = autoRotationOn;
    Screen.autorotateToLandscapeRight = autoRotationOn;

    Screen.orientation = autoRotationOn
        ? ScreenOrientation.AutoRotation
        : ScreenOrientation.Portrait;
}

private static bool DeviceAutoRotationIsOn()
{
    using (var actClass = new AndroidJavaClass("com.unity3d.player.UnityPlayer"))
    {
        var context = actClass.GetStatic<AndroidJavaObject>("currentActivity");
        var systemGlobal = new AndroidJavaClass("android.provider.Settings$System");
        var rotationOn = systemGlobal.CallStatic<int>("getInt",
            context.Call<AndroidJavaObject>("getContentResolver"), "accelerometer_rotation");

        return rotationOn == 1;
    }
}
```

Confused? Here is how this code work.

```csharp
private static bool DeviceAutoRotationIsOn()
{
    using (var actClass = new AndroidJavaClass("com.unity3d.player.UnityPlayer"))
    {
        var context = actClass.GetStatic<AndroidJavaObject>("currentActivity");
        var systemGlobal = new AndroidJavaClass("android.provider.Settings$System");
        var rotationOn = systemGlobal.CallStatic<int>("getInt",
            context.Call<AndroidJavaObject>("getContentResolver"), "accelerometer_rotation");

        return rotationOn == 1;
    }
}
```

This piece of code check from android settings if the device's auto-rotation is on or off. It returns an `int` value. which is `0` if auto-rotation is disabled, otherwise `1`.

```csharp
private static void ToggleAutoRotation()
{
    var autoRotationOn = DeviceAutoRotationIsOn();
    Screen.autorotateToPortrait = autoRotationOn;
    Screen.autorotateToLandscapeLeft = autoRotationOn;
    Screen.autorotateToLandscapeRight = autoRotationOn;

    Screen.orientation = autoRotationOn
        ? ScreenOrientation.AutoRotation
        : ScreenOrientation.Portrait;
}
```

Here, we are taking the output from previous code and updating the rotation-related flag. If auto-rotation is disabled, we are setting the device orientation to **Portrait**.

```csharp
void OnApplicationFocus(bool haveFocus)
{
#if UNITY_ANDROID && !UNITY_EDITOR
    if (haveFocus) ToggleAutoRotation();
#endif
}
```

We don't want to continuously check the device auto-rotation status, so we are executing the code only when the application gets focused.

I've created a unity 2D project with a demo scene. Go to this [**GitHub repository**](https://github.com/Lazyb0y/unity-android-auto-rotation) for full source code.
