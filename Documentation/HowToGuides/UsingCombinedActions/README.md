# Using Combined Actions

> * Level: Beginner
>
> * Reading Time: 5 minutes
>
> * Checked with: Unity 2018.3.14f1

## Introduction

Combined Actions allow for more complex input types and in this guide we'll look at how to use Boolean input such as keyboard keys to mimic an analogue axis such as a thumbstick.

We can then take this sort of axis data and convert it into movement information or rotation information for other objects in our scene.

## Prerequisites

* [Install the Tilia.Input.UnityInputManager.Unity] Package dependency in to your [Unity] project.
* [Install the Tilia.Input.CombinedActions.Unity] Package dependency in to your Unity project.

## Let's Start

### Step 1

Create a new `Capsule` Unity 3D Object by selecting `Main Menu -> GameObject -> 3D Object -> Capsule` and change the Transform properties to:

* Position: `X = 0, Y = 0, Z = 0`
* Scale: `X = 1, Y = 1, Z = 1`

![Create Capsule](assets/images/CreateCapsule.png)

Right click on the `Capsule` GameObject, select `3D Object -> Cube` and change the Transform properties to:

* Position: `X = 0, Y = 0.9, Z = 0.25`
* Scale: `X = 0.1, Y = 0.1, Z = 1`

![Create Cube](assets/images/CreateCube.png)

### Step 2

Expand the `Tilia Input UnityInputManager Unity` Package directory in the Unity Project window and select the `Packages -> Tilia Input UnityInputManager Unity -> Runtime -> Prefabs -> Actions` directory then drag and drop the `Input.UnityInputManager.ButtonAction` prefab into the Unity hierarchy window.

![Import Unity Input Manager](assets/images/ImportUnityInputManager.png)

Set the `Key Code` parameter in the `Unity Input Manager Button Action` component to `W`.

> To make it easier you could rename `Input.UnityInputManager.ButtonAction` GameObject to `Input.UnityInputManager.ButtonAction W` as we will have 4 different total button actions.

![Set Key Code To W](assets/images/SetKeyCodeToW.png)

> Repeat this 3 more times for the keys A, S and D.

![Duplicate Prefab](assets/images/DuplicatePrefab.png)

### Step 3

Expand the `Tilia Input CombinedActions Unity` Package directory in the Unity Project window and select the `Packages -> Tilia Input CombinedActions Unity -> Runtime -> Prefabs` directory then drag and drop the `Input.CombinedActions.BooleanTo1DAxisAction` prefab into the Unity hierarchy window.

![Import Combined Actions Prefab](assets/images/ImportCombinedActionsPrefab.png)

Rename the newly created `Input.CombinedActions.BooleanTo1DAxisAction` GameObject to `Input.CombinedActions.BooleanTo1DAxisAction Horizontal`

![RenameToHorizontal](assets/images/RenameToHorizontal.png)

### Step 4

In the Axis Settings on the `Boolean To 1DAxis Action` component of the `Boolean To 1DAxis Action Horizontal` GameObject, drag and drop the `Input.UnityInputManager.ButtonAction A` GameObject into the `Negative Input` parameter and drag and drop the `Input.UnityInputManager.ButtonAction D` GameObject into the `Positive Input` parameter.

![Drag And Drop Into Horizontal Positive And Negative Parameters](assets/images/DragAndDropIntoHorizontalPositiveAndNegativeParameters.png)

### Step 5

Drag and drop another `Input.CombinedActions.BooleanTo1DAxisAction` prefab into the Unity hierarchy window. Rename the new `Input.CombinedActions.BooleanTo1DAxisAction` GameObject to `Input.CombinedActions.BooleanTo1DAxisAction Vertical`.

![RenameToVertical](assets/images/RenameToVertical.png)

In the Axis Settings on the `Boolean To 1DAxis Action` component of the `Boolean To 1DAxis Action Vertical` GameObject, drag and drop the `Input.UnityInputManager.ButtonAction S` GameObject into the `Negative Input` parameter and drag and drop the `Input.UnityInputManager.ButtonAction W` GameObject into the `Positive Input` parameter.

![Drag And Drop Into Positive And Negative Parameters](assets/images/DragAndDropIntoVerticalPositiveAndNegativeParameters.png)

### Step 6

Create a new `Empty` Unity Object by selecting `Main Menu -> GameObject -> Create Empty` and rename it to `Movement`. Click `Add Component` and add a `Transform Position Mutator` component.

![Add Position Mutator](assets/images/AddPositionMutator.png)

### Step 7

Drag and drop the `Capsule` GameObject into the `Target` parameter on the `Transform Position Mutator` component.

![Drag And Drop Capsule](assets/images/DragAndDropCapsule.png)

### Step 8

Expand the `Tilia Input CombinedActions Unity` Package directory in the Unity Project window and select the `Packages -> Tilia Input CombinedActions Unity -> Runtime -> Prefabs` directory then drag and drop the `Input.CombinedActions.AxesToVector3Action` prefab into the Unity hierarchy window.

![Import Axes To Vector3 Action](assets/images/ImportAxesToVector3Action.png)

### Step 9

On the `Input.CombinedActions.AxesToVector3Action` GameObject, drag and drop the `Input.CombinedActions.BooleanTo1DAxisAction Horizontal` GameObject into the `Lateral Axis` property on the `Axes To Vector3` component.

![Lateral Axis](assets/images/LateralAxis.png)

Drag and drop the `Input.CombinedActions.BooleanTo1DAxisAction Vertical` GameObject into the `Longitudinal Axis` property on the `Axes To Vector3 Action` component.

![Longitudinal Axis](assets/images/LongitudinalAxis.png)

### Step 10

Select the `Input.CombinedActions.AxesToVector3Action` GameObject from the Unity Hierarchy and click the `+` symbol in the bottom right corner of the `Value Changed` event parameter on the `Axes To Vector3 Action` component.

Drag and drop the `Movement` GameObject into the event listener box that appears on the `Value Changed` event parameter on the `Axes To Vector3 Action` component that displays `None (Object)`.

![Drag Movement Into Event Listener](assets/images/DragMovementIntoEventListener.png)

Select a function to perform when the `Value Changed` event is emitted. For this example, select the `TransformPositionMutator -> DoIncrementProperty` function (be sure to select `Dynamic Vector3 - DoIncrementProperty` for this example).

![Set Value Changed Function](assets/images/SetValueChangedFunction.png)

> Play the Unity scene you should now be able to control the GameObject using the W, A, S, D keys.

![Move Capsule](assets/images/MoveCapsule.png)

### Step 11

We are now going to make it possible to rotate the capsule instead. 

Disable the `Movement` GameObject.

![Disable Movement GameObject](assets/images/DisableMovementGameObject.png)

### Step 12

Create a new `Empty` Unity Object by selecting `Main Menu -> GameObject -> Create Empty` and rename it `Rotation`. Click `Add Component` and add a `Float To Vector3` component.

![Add Float To Vector3](assets/images/AddFloatToVector3.png)

Click `Add Component` again and add a `Transform Euler Rotation Mutator` component.

![Add Euler Rotator](assets/images/AddEulerRotator.png)

### Step 13

Drag and drop the `Capsule` GameObject into the `Target` parameter on the `Transform Euler Rotation Mutator` component. On the `Mutate on Axis` property turn off the `X` and `Z`  checkboxes.

> The reason we only want the Y axis turned on is because we only want to rotate around this axis to make our capsule rotate to a new facing direction in the scene.

![Drag Capsule Into Euler Mutator](assets/images/DragCapsuleIntoEulerMutator.png)

### Step 14

Select the `Rotation` GameObject from the Unity Hierarchy and click the `+` symbol in the bottom right corner of the `Transformed` event parameter on the `Float To Vector3` component.

Drag and drop the `Rotation` GameObject into the event listener box that appears on the `Transformed` event parameter on the `Float To Vector3` component that displays `None (Object)`.

![Drag Rotation Into Event Listener](assets/images/DragRotationIntoEventListener.png)

Select a function to perform when the `Value Changed` event is emitted. For this example, select the `TransformEulerRotationMutator -> DoSetProperty` function (be sure to select `Dynamic Vector3 - DoSetProperty` for this example).

![Set Transformed Function](assets/images/SetTransformedFunction.png)

### Step 15

Expand the `Tilia Input CombinedActions Unity` Package directory in the Unity Project window and select the `Packages -> Tilia Input CombinedActions Unity -> Runtime -> Prefabs` directory then drag and drop the `Input.CombinedActions.AxesToAngle` prefab into the Unity hierarchy window.

![Import Axes To Angle](assets/images/ImportAxesToAngle.png)

### Step 16

On the `Input.CombinedActions.AxesToAngle` GameObject, Drag and drop the `Input.CombinedActions.BooleanTo1DAxisAction Vertical` GameObject into the `Vertical Axis` property on the `Axis To Angle Action` component.

![Drag And Drop Axes To Angle Into Horizontal Axis](assets/images/DragAndDropAxesToAngleIntoHorizontalAxis.png)

Drag and drop the `Input.CombinedActions.BooleanTo1DAxisAction Vertical` GameObject into the `Vertical Axis` property on the `Axis To Angle Action` component.

![Drag And Drop Axes To Angle Into Vertical Axis](assets/images/DragAndDropAxesToAngleIntoVerticalAxis.png)

### Step 17

Select the `Input.CombinedActions.AxesToAngle` GameObject from the Unity Hierarchy and click the `+` symbol in the bottom right corner of the `Value Changed` event parameter on the `Axes To Angle Action` component.

Drag and drop the `Rotation` GameObject into the event listener box that appears on the `Value Changed` event parameter on the `Axes To Angle Action` component that displays `None (Object)`.

![Create Axes To Angle Event](assets/images/CreateAxesToAngleEvent.png)

Select a function to perform when the `Value Changed` event is emitted. For this example, select the `Float To Vector3 -> CurrentY` function (be sure to select `Dynamic Vector3 - CurrentY` for this example).

![Set Axes Angle Function](assets/images/SetAxesAngleFunction.png)

### Step 18

Select the `Input.CombinedActions.AxesToAngle` GameObject from the Unity Hierarchy and click the `+` symbol in the bottom right corner of the `Value Changed` event parameter on the `Axes To Angle Action` component to add a second `Value Changed` event.

Drag and drop the `Rotation` GameObject into the event listener box that appears on the `Value Changed` event parameter on the `Axes To Angle Action` component that displays `None (Object)`.

![Create Second Axes To Angle Event](assets/images/CreateSecondAxesToAngleEvent.png)

Select a function to perform when the `Value Changed` event is emitted. For this example, select the `Float To Vector3 -> DoTransform()` function (be sure to select `Static Parameters - DoTransform()` for this example).

![Set Second Axes Angle Function](assets/images/SetSecondAxesAngleFunction.png)

### Done

Play the Unity scene, you will notice that by pressing the keys you can rotate the GameObject to change the direction it is facing.

![Done](assets/images/Done.png)

[Install the Tilia.Input.UnityInputManager.Unity]: https://github.com/ExtendRealityLtd/Tilia.Input.UnityInputManager/tree/master/Documentation/HowToGuides/Installation
[Install the Tilia.Input.CombinedActions.Unity]: https://github.com/ExtendRealityLtd/Tilia.Input.CombinedActions.Unity/tree/adddocs/Documentation/HowToGuides/Installation
[Unity]: https://unity3d.com/