# Explanation of data.json

The file `data/data.json` contains Topside's model of sensor values, and what topside sends down to the rover.

The file looks like this:

```json
{
    "Key1" : 1 // Value (e.g. 0 or 1.)
    "Key2" : 256
}
```

It can also contain nested values, meaning:

```json
{
    "Buttons" : {
        "Button1" : 0,    // Button is not pressed
        "Button2" : 1     // Button is pressed
    }
}
```

The file is read using the json library:

```python
import json      # loads the json library

# Opens the file and loads the data into __data_dict__.
with open('data/data.json', 'r') as file:
    data_dict = json.load(file)            # Change the name of __data_dict__ to whatever you prefer.
```

Once it is loaded, you read values using the syntax:

```json
x = data_dict["Key"]
```

Nested keys are read with:

```json
x = data_dict["Key"]["Subkey"]

# Example:
y = data_dict["Buttons"]["Button1"]
```

## Overview of values

#### Thrust:

`"Thrust" : [x,y,z,pitch,roll,yaw]` 
List (array) of 6 values. `data_dict["Thrust"][1]` gives the y value.

#### Buttons

Buttons from the joystick - 0 or 1. (not held down vs. held down.)

```json
"Buttons" : {
    "button_surface" : 0    // Go to surface button
}
```

#### thrusters

Measured values for the thrusters. Ex:

```json
"thrusters": {
        "U_FWD_P": {
            "power": 400,
            "temp": 20
        }
}
```

Each has `"power"` and `"temp"`.

#### 9dof

Measured 9dof values - `"acceleration"`, `"gyroscope"`, and `"magnetometer"`.
Each has an `x`, `y`, and `z` value.

Example:

```json
"9dof": {
    "acceleration": {
        "x": 0.02,
        "y": -0.01,
        "z": 9.81
    },
    "gyroscope": {
        "x": 0.01,
        "y": 0.02,
        "z": 0.00
    },
    "magnetometer": {
        "x": 30.6,
        "y": -22.3,
        "z": 15.5
    }
}
```

#### Lights

Lights.

Example:

```json
"lights": {
    "Light1": 50,
    "Light2": 50,
    "Light3": 50,
    "Light4": 50
}
```

#### Battery

Battery. Percentage.

```json
"battery": 100
```

#### Depth


```json
"depth": {
    "dpt": 123,
    "dptSet": 124
}
```

`"dpt"` is the measured depth.
`"dptSet"` is the depth it is trying to reach (target).