# MB-Rewind-API
MB Rewind allows the mod creators to make their features rewindable. A feature has to be time dependent or interactive in order for it to require this API usage, otherwise the plugin will work just fine.

## Usage
Any feature that should support rewinding has to implement the following methods. Such feature is called Rewindable. Few samples are provided on how to implement this.   
The [%obj] parameters are only used for Object Type rewindables and should not be there for Variable Type rewindables
### `<namespace>::getState([%obj])`
Called during every frame while not rewinding and should return the current state of the %obj object(if it is an Object Type rewindable) or value of a variable

### `<namespace>::setState([%obj],%state)`
Called during rewind. %state is the state to which you have to set the state of the %obj object(if it is Object Type rewindable) or of a variable

### `<namespace>::interpolateState(%one,%two,%ratio,%delta)`
Called during interpolation during rewind. It should return the interpolated value between states %one and %two. The ratio is %ratio and the time difference between the current and previous frame is %delta.

### `<namespace>::onRewind([%obj])`
Called when rewind is started or stopped


## API Functions

### `registerRewindable(string namespace, int type, int storagetype)`
- namespace: the namespace of the rewindable
- type: the type of the rewindable.  
valid values:  
0: Object Type: The rewindable is an interactive object that can be placed in the mission. Eg. Gem states, Powerup states  
1: Variable Type: The rewindable is a global variable of sorts. Eg. Time travel bonus, Current marble gravity
- storagetype: the storage type of the rewindable.  
valid values:
0: int  
1: float  
2: bool  
3: string  

Usage: Registers the given namespace as a rewindable

### `unregisterRewindable(string namespace)`
- namespace: the namespace of the registered rewindable

Usage: Unregisters the given rewindable namespace

### `setGame(string game)`
- game: the short name of the game
Usage: It is used to differentiate between replays of different mods. The mod will only load those replays with the same game metadata as what you have set with this function.

## Miscellaneous Functions
Functions that Torque normally does not provide.

### `ShapeBase.getCameraTransform(%pos)`
Returns the camera transform of the shapebase at %pos

### `getVelocity(Marble m)`
Returns the velocity of the given marble m

### `setVelocity(Marble m, Vector3F value)`
Sets the linear velocity of the given marble m to value

### `getAngularVelocity(Marble m)`
Returns the angular velocity of the given marble m

### `setAngularVelocity(Marble m, Vector3F value)`
Sets the angular velocity of the given marble m to value

### `setPhysics(bool on)`
Enables/Disables physics

### `lockTransforms(bool lock)`
Locks/Unlocks change in transforms of all SceneObjects

### `getThreadDir(ShapeBase s)`
Gets the animation direction of the first thread of given ShapeBase object

### `getThreadPos(ShapeBase s)`
Gets the animation position of the first thread of given ShapeBase object

### `setThreadPos(ShapeBase s, float t)`
Sets the animation position of the first thread of given ShapeBase object to t

### `GetClientShape(Shapebase s)`
Gets the clientside object of the given serverside object s

## Internal Functions
These functions should normally not be used, they are used internally in the mod for functioning

### `setReplayPath(string path)`
Sets the replay save path to path

### `setReplayMission(string missionpath)`
Sets the mission path metadata for current replay

### `getReplayPath()`
Gets the current replay's save path

### `loadReplay(string path)`
Loads replay from path into memory and sets it as current replay

### `clearFrames(bool write)`
Clears the replay and rewind frames and saves it to disk at set replay path if write == true

### `storeFrame(int ms)`
Stores the current state of the mission in a frame and sets its timestamp to ms

### `getFrameCount()`
Gets the number of frames stored

### `saveState()`
Saves the current mission state along with the rewind states

### `loadState(int stateindex)`
Loads the save state at specified index stateindex.

### `getStateCount()`
Returns number of save states


