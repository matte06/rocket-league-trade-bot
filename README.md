# Rocket League Trade-Bot
A simple Rocket League tradebot running with NodeJS. Running in every environment even without Rocket League being installed

## Disclaimer
This Software as it is, is **against** the **Terms of Service from Psyonix** and is only being developed for experimental purposes.
Be aware that using this Software as a productive service is bannable by Psyonix and they won't refund any lost items (definitly didnt happen to me^^)

# How to Install
#### 1. Clone this Repository into your Project or download it as standalone

#### 2. Install all required modules with npm
```
npm install --save atob btoa crypto-js request steam-user ws
```
And for typescript users
```
npm install --save-dev @types/atob @types/crypto-js @types/request @types/ws
```

#### 3. Including the Module 
- Using TypeScript:
  ```typescript
  import RocketLeague from "./path/to/this/module";
  ```
- Using JavaScript & Precompiled version
  ```javascript
  const RocketLeague = require("./path/to/this/module/dist"); // important to add "/dist" for the precompiled version
  ```
  
#### 4. Load the items list from Rocket Leauge using BakkesMod (or some other Mod that is able to do that)
Use `dumpitems` in the BakkesMod console, this gives you a `items.csv` file. Copy this file somewhere into your Project.

There is already the current version in this repository, but it may not get updated anymore.
  
# Config
*form here on I will only explain everything in TypeScript, for those of you who are using JS just do the same without types and in the dirst folder*

#### Open the config.js / config.js file
link your items.csv file there (once the file is open this should be self explaining), default is `./items.csv`
set the items.csv encoding, default is `utf8`
set the separator, default is `","`

# Classes
## RocketLeague
the base class for the RL client
### Properties
|name|type|description|
|---|---|---|
|username|string|username of the player signed in|
|steam|typeof import "steam-user"|instance of a steam client|
|opts|ILunchOptions|options|
|booted|boolean|if the rl client is connected to the server|
|party|Party|the current party (null if in none)|
|inventory|Inventory|your inventory (empty if not loaded with `loadInventory()`)|
|steamID|string|steamID of the signed in user|

### Methods
### static launch
Creates a new RocketLeague object
#### OVERLOADS
`static launch(steam: typeof import "steam-user", opts?): Promise<RocketLeague>`

`static launch(credentials: IPersona, opts?): Promise<RocketLeague>`

When using the top method make sure that a user is already signed in to steam.
Returns a Promise of a RocketLeague object

#### ARGUMENTS
**steam**: a instance of the [node-steam-user](https://github.com/DoctorMcKay/node-steam-user) module by DoctorMcKay. It has to be signed in already.

**credentials**: (only if no steam is provided) an object with `accountName: string`, `password: string` and if 2fa is enabled a `twoFactorToken: string`. But read more about this on the link on top

**opts**: An optional options object.

- **logger**: function to redirect the logs in the format of `(type: string, source: string, message: string, data?: any)`. example: `("success", "steam", "logged in", { steamID: 123 })`, default is `console.log`
  
- **setToPlayRocketLeague**: set this to `true` if your steam profile should show that you are playing rocket league
  
- **steamOptions**: a object used only if you pass credentials instead of a steam object [docs](https://github.com/DoctorMcKay/node-steam-user#options-)


### boot
`async boot()`
"boots" up the game by connecting a WSS to the rocketleague servers. booting takes about 2 seconds


### pause
`pause()`
disconnects from the rocket league server, you can then just call `boot()` again to reconnect without completly reiniting the game.

its always recommended to pause the bot while its not in use


### sendMessage
`async sendMessage(service: string, params: any)`
sends a WSS Packet to the connected RL server, mostly used internally by other functions (should be irrelevant for a basic bot)


### createParty
`async createParty()`
returns a `Promise<Party>`. Creates a new party in RL. always await this call else the party is unuseable.


### loadInventory
`async loadInventory(loadIfNotSet = true, force = false)`
loads the users inventory & credits.

Notice: the game has to be booted for this so add a `rl.boot()` before if not done already


## Inventory
A holder that can hold items & credits. Its used for the players inventory and for both parties of a trade.

### Properties
|name|type|description|
|---|---|---|
|credits|number|amount of credits obviously|
|items|`IRocketLeagueItem[]`|A holder for all items|
|sysItems|`IItem[]`|A holder for the plain items which RL uses|

### Methods
### updateItems {AwaitAble}
