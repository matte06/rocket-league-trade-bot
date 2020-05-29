# Rocket League Trade-Bot
A simple Rocket League tradebot running with NodeJS. Running in every environment even without Rocket League being installed

## Disclaimer
This Software as it is, is **against** the **Terms of Service from Psyonix** and is only being developed for experimental purposes.
Be aware that using this Software as a productive service is bannable by Psyonix and they won't refund any lost items (definitly didnt happen to me^^)

## How to Install
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
  
## Config
*form here on I will only explain everything in TypeScript, for those of you who are using JS just do the same without types and in the dirst folder*

#### Open the config.js / config.js file
link your items.csv file there (once the file is open this should be self explaining), default is `./items.csv`
set the items.csv encoding, default is `utf8`
set the separator, default is `","`

## Classes
### RocketLeague
#### Properties
|name|type|description|
|---|---|---|
|username|string|username of the player signed in|
|steam|typeof import "steam-user"|instance of a steam client|
|opts|ILunchOptions|options|
|booted|boolean|if the rl client is connected to the server|
|party|Party|the current party (null if in none)|
|inventory|Inventory|your inventory (empty if not loaded with `loadInventory()`)|
|steamID|string|steamID of the signed in user|

#### Methods
```typescript
static async lunch(steamClient: Steam, opts?: ILunchOptions): Promise<RocketLeague>;
static async lunch(persona: IPersona, opts?: ILunchOptions): Promise<RocketLeague>;
```
When using the top method make sure that a user is already signed in to steam.
Returns a Promise of a RocketLeague object
