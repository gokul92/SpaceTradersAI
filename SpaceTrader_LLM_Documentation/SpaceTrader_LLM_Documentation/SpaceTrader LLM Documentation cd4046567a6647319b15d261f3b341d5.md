# SpaceTrader LLM Documentation

# # Introduction

SpaceTraders is an open-universe game and learning platform that allows you to control a fleet of ships and explore a multiplayer universe.

You as a player will receive information on the open universe and can choose from a set of instructions to make your next move. You can also request any additional information to help you make the decision. If available, the information will be provided to you.

# # Goal

The goal of the game is to manage a fleet of ships and increase profits as you explore the universe. You can trade goods across systems, mine asteroids for valuable ores or take on faction contracts to earn credits and reputation. 

# # Game Concepts

## ## **Agents and Factions**

### ### Overview

Agents are the primary entity in SpaceTraders. Every player controls a single agent which can be used to manage a fleet of ships and conduct trade with factions.

The API token generated when you register is scoped to your agent. You can use this token to control your agent and view the status of your fleet.

Every endpoint under the **`/my`** namespace requires an API token and returns data specific to your agent. For example, you can use the following API request to view your current reputation with each faction.

Please note that an agent is not the same as your SpaceTraders account. Accounts are tied to an email address and allow you to login to the dashboard to manage API tokens and webhooks.

### ### Creating an agent

- Use the following POST statement and Request body for the creation of an agent
    - `POST - https://api.spacetraders.io/v2/register`
    - Request Body
        
        ```json
        {
          "faction": "Type string. The symbol of the faction. It can be one of the following - COSMIC, VOID, GALACTIC, QUANTUM, DOMINION, ASTRO, CORSAIRS, OBSIDIAN, AEGIS, UNITED, SOLITARY, COBALT, OMEGA, ECHO, LORDS, CULT, ANCIENTS, SHADOW, ETHEREA",
          "symbol": "Type string. Your desired agent symbol. This will be a unique name used to represent your agent, and will be the prefix for your ships. It should be greater than or equal to 3 characters or less than equal to 14 characters. For example - BADGER",
          "email": "Type string. email used to register. This SHOULD always be spacetradersLLM@gmail.com"
        }
        ```
        
    - Once your agent has been created successfully, you will receive the following response. It includes explanations of what the different fields mean. Please go through and retain carefully. If you have any questions, please ask them. Do not assume concepts/make up anything.
        
        ```json
        {
          "data":{
            "agent":{
              "accountId":"Type string. Account ID that is tied to this agent. Only included on your own agent.",
              "symbol":"Type string. Symbol of the agent",
              "headquarters":"Type string. The headquarters of the agent.",
              "credits":"Type integer. The number of credits the agent has available. Credits can be negative if funds have been overdrawn. This comment is a string only to illustrate the nature ofthis property to you. Otherwise, expect an integer here",
              "startingFaction":"Type string. The faction the agent started with.",
              "shipCount":"Type integer. How many ships are owned by the agent. This comment is a string only to illustrate the nature of this property to you. Otherwise, expect an integer here"
            },
            "contract":{
              "id":"Type string. The ID of the contract",
              "factionSymbol":"Type string. The symbol of the faction that this contract is for.",
              "type":"Type string. Can be one of the following - PROCUREMENT, TRANSPORT, SHUTTLE",
              "terms":{
                "deadline":"Type string date-time. The deadline for the contract.",
                "payment":{
                  "onAccepted":"Type integer. The amount of credits received upfront for accepting the contract",
                  "onFulfilled":"Type integer. The amount of credits received when the contract is fulfilled"
                },
                "deliver":[
                  {
                    "tradeSymbol":"Type string. The symbol of the trade good to deliver.",
                    "destinationSymbol":"Type string. The destination where goods need to be delivered.",
                    "unitsRequired":"Type integer. The number of units that need to be delivered on this contract",
                    "unitsFulfilled":"Type integer. The number of units fulfilled on this contract"
                  }
                ]
              },
              "accepted":"Type Boolean. Whether the contract has been accepted by the agent or not. False, by default",
              "fulfilled":"Type Boolean. Whether the contract has been fulfilled. False, by default",
              "deadlineToAccept":"Type date-time string. The time at or after which the contract is no longer available to be accepted."
            },
            "faction":{
              "symbol":"Type string. Faction symbol.",
              "name":"Type string. Name of the faction",
              "description":"Type string. Description of the faction.",
              "headquarters":"Type string. The waypoint in which the faction's HQ is located in.",
              "traits":[
                {
                  "symbol":"Type string. Symbol of the traits that define the faction. Can be one of the following values - BUREAUCRATIC, SECRETIVE, CAPITALISTIC, INDUSTRIOUS, PEACEFUL, DISTRUSTFUL, WELCOMING SMUGGLERS, SCAVENGERS, REBELLIOUS, EXILES, PIRATES, RAIDERS CLAN, GUILD, DOMINION, FRINGE, FORSAKEN, ISOLATED, LOCALIZED ESTABLISHED, NOTABLE, DOMINANT, INESCAPABLE, INNOVATIVE BOLD, VISIONARY, CURIOUS, DARING, EXPLORATORY, RESOURCEFUL FLEXIBLE, COOPERATIVE, UNITED, STRATEGIC, INTELLIGENT RESEARCH_FOCUSED, COLLABORATIVE, PROGRESSIVE, MILITARISTIC TECHNOLOGICALLY_ADVANCED, AGGRESSIVE, IMPERIALISTIC TREASURE_HUNTERS, DEXTEROUS, UNPREDICTABLE, BRUTAL, FLEETING ADAPTABLE, SELF_SUFFICIENT, DEFENSIVE, PROUD, DIVERSE INDEPENDENT, SELF_INTERESTED, FRAGMENTED, COMMERCIAL FREE_MARKETS, ENTREPRENEURIAL",
                  "name":"Type string. Name of the trait.",
                  "description":"Type string. A description of the trait"
                }
              ],
              "isRecruiting":"Boolean. Whether or not the faction is currently recruiting new agents"
            },
            "ship":{
              "symbol":"Type string. The globally unique identifier of the ship in the following format: [AGENT_SYMBOL]-[HEX_ID]",
              "registration":{
                "name":"Type string. The agent's registered name of the ship.",
                "factionSymbol":"Type string. The symbol of the faction the ship is registered with",
                "role":"Type string. The registered role of the ship. Can be one of FABRICATOR, HARVESTER, HAULER, INTERCEPTOR, EXCAVATOR, TRANSPORT, REPAIR, SURVEYOR, COMMAND, CARRIER, PATROL, SATELLITE, EXPLORER, REFINERY"
              },
              "nav":{
                "systemSymbol":"Type string. The system symbol of the ship's current location.",
                "waypointSymbol":"Type string. The waypoint symbol of the ship's current location, or if the ship is in-transit, the waypoint symbol of the ship's destination.",
                "route":{
                  "destination":{
                    "symbol":"Type string. The symbol of the waypoint.",
                    "type":"Type string. The type of the waypoint. Can be one of the following - PLANET, GAS_GIANT, MOON, ORBITAL_STATION, JUMP_GATE, ASTEROID_FIELD, NEBULA, DEBRIS_FIELD, GRAVITY_WELL",
                    "systemSymbol":"Type string. The symbol of the system the waypoint is in.",
                    "x":"Type integer. Position in the universe in the x axis",
                    "y":"Type integer. Position in the universe in the y axis"
                  },
                  "departure":{
                    "symbol":"Type string. The symbol of the waypoint",
                    "type":"Type string. The type of the waypoint. Can be one of the following - PLANET, GAS_GIANT, MOON, ORBITAL_STATION, JUMP_GATE, ASTEROID_FIELD, NEBULA, DEBRIS_FIELD, GRAVITY_WELL",
                    "systemSymbol":"Type string. The symbol of the system the waypoint is in.",
                    "x":"Type integer. Position in the universe in the x axis",
                    "y":"Type integer. Position in the universe in the y axis"
                  },
                  "departureTime":"Type date-time string. The date time of the ship's departure.",
                  "arrival":"Type date-time string. The date time of the ship's arrival. If the ship is in transit, then this is expected time of arrival."
                },
                "status":"Type string. The current status of the ship. This can be one of the following values - IN_TRANSIT, IN_ORBIT, DOCKED",
                "flightMode":"Type string. The ship's set speed when traveling between waypoints or systems. Can be one of the following values - DRIFT, STEALTH, CRUISE, BURN. Default is CRUISE."
              },
              "crew":{
                "current":"Type integer. The current number of crew members on the ship",
                "required":"Type integer. The minimum number of crew members required to maintain the ship",
                "capacity":"Type integer. The maximum number of crew members the ship can support",
                "rotation":"Type string. The rotation of crew shifts. A stricter shift improves the ship's performance. A more relaxed shift improves the crew's morale. Can either be STRICT or RELAXED",
                "morale":"Type integer. A rough measure of the crew's morale. A higher morale means the crew is happier and more productive. A lower morale means the ship is more prone to accidents",
                "wages":"Type integer. The amount of credits per crew member paid per hour. Wages are paid when a ship docks at a civilized waypoint."
              },
              "frame":{
                "symbol":"Type string.The frame of the ship determines the number of modules and mounting points of the ship, as well as base fuel capacity. As the condition of the frame takes more wear, the ship will become more sluggish and less maneuverable. The following values can be specified for the frame of the ship - FRAME_PROBE, FRAME_DRONE, FRAME_INTERCEPTOR, FRAME_RACER, FRAME_FIGHTER, FRAME_FRIGATE, FRAME_SHUTTLE, FRAME_EXPLORER, FRAME_MINER, FRAME_LIGHT_FREIGHTER, FRAME_HEAVY_FREIGHTER, FRAME_TRANSPORT, FRAME_DESTROYER, FRAME_CRUISER, FRAME_CARRIER",
                "name":"Type string. Name of the frame.",
                "description":"Type string. Description of the frame.",
                "condition":"Type integer. Condition is a range of 0 to 100 where 0 is completely worn out and 100 is brand new",
                "moduleSlots":"Type integer. The amount of slots that can be dedicated to modules installed in the ship. Each installed module take up a number of slots, and once there are no more slots, no new modules can be installed",
                "mountingPoints":"Type integer. The amount of slots that can be dedicated to mounts installed in the ship. Each installed mount takes up a number of points, and once there are no more points remaining, no new mounts can be installed",
                "fuelCapacity":"Type integer. The maximum amount of fuel that can be stored in this ship. When refueling, the ship will be refueled to this amount",
                "requirements":{
                  "power":"Type integer. The amount of power required from the reactor",
                  "crew":"Type integer. The number of crew required for operation",
                  "slots":"Type integer. The number of module slots required for installation"
                }
              },
              "reactor":{
                "symbol":"Type string. The reactor of the ship. The reactor is responsible for powering the ship's systems and weapons. The symbol that denotes the reactor can be one of the following values - REACTOR_SOLAR_I, REACTOR_FUSION_I, REACTOR_FISSION_I, REACTOR_CHEMICAL_I, REACTOR_ANTIMATTER_I",
                "name":"Type string. Name of the reactor.",
                "description":"Type string. Description of the reactor.",
                "condition":"Type integer. Condition is a range of 0 to 100 where 0 is completely worn out and 100 is brand new",
                "powerOutput":"Type integer. The amount of power provided by this reactor. The more power a reactor provides to the ship, the lower the cooldown it gets when using a module or mount that taxes the ship's power",
                "requirements":{
                  "power":"Type integer. The amount of power required from the reactor",
                  "crew":"Type integer. The number of crew required for operation",
                  "slots":"Type integer. The number of module slots required for installation"
                }
              },
              "engine":{
                "symbol":"Type string. The symbol of the engine. The engine determines how quickly a ship travels between waypoints. It could be one of the following values - ENGINE_IMPULSE_DRIVE_I, ENGINE_ION_DRIVE_I, ENGINE_ION_DRIVE_II, ENGINE_HYPER_DRIVE_I",
                "name":"Type string. The name of the engine.",
                "description":"Type string. The description of the engine",
                "condition":"Type integer. Condition is a range of 0 to 100 where 0 is completely worn out and 100 is brand new.",
                "speed":"Type integer. The speed stat of this engine. The higher the speed, the faster a ship can travel from one point to another. Reduces the time of arrival when navigating the ship",
                "requirements":{
                  "power":"Type integer. The amount of power required from the reactor",
                  "crew":"Type integer. The number of crew required for operation",
                  "slots":"Type integer. The number of module slots required for installation"
                }
              },
              "modules":[
                {
                  "symbol":"Type string. The symbol of the module. It could be one of the following values - MODULE_MINERAL_PROCESSOR_I, MODULE_CARGO_HOLD_I, MODULE_CREW_QUARTERS_I, MODULE_ENVOY_QUARTERS_I, MODULE_PASSENGER_CABIN_I, MODULE_MICRO_REFINERY_I, MODULE_ORE_REFINERY_I, MODULE_FUEL_REFINERY_I, MODULE_SCIENCE_LAB_I, MODULE_JUMP_DRIVE_I, MODULE_JUMP_DRIVE_II, MODULE_JUMP_DRIVE_III, MODULE_WARP_DRIVE_I, MODULE_WARP_DRIVE_II, MODULE_WARP_DRIVE_III, MODULE_SHIELD_GENERATOR_I, MODULE_SHIELD_GENERATOR_II",
                  "capacity":"Type integer. Modules that provide capacity, such as cargo hold or crew quarters will show this value to denote how much of a bonus the module grants",
                  "range":"Type integer. Modules that have a range will such as a sensor array show this value to denote how far can the module reach with its capabilities",
                  "name":"Type string. Name of this module",
                  "description":"Type string. Description of this module",
                  "requirements":{
                    "power":"Type integer. The amount of power required from the reactor",
                    "crew":"Type integer. The number of crew required for operation",
                    "slots":"Type integer. The number of module slots required for installation."
                  }
                }
              ],
              "mounts":[
                {
                  "symbol":"Type string. Symbol of this mount. Can be one of the following values - MOUNT_GAS_SIPHON_I, MOUNT_GAS_SIPHON_II, MOUNT_GAS_SIPHON_III, MOUNT_SURVEYOR_I, MOUNT_SURVEYOR_II, MOUNT_SURVEYOR_III, MOUNT_SENSOR_ARRAY_I, MOUNT_SENSOR_ARRAY_II, MOUNT_SENSOR_ARRAY_III, MOUNT_MINING_LASER_I, MOUNT_MINING_LASER_II, MOUNT_MINING_LASER_III, MOUNT_LASER_CANNON_I, MOUNT_MISSILE_LAUNCHER_I, MOUNT_TURRET_I",
                  "name":"Type string. Name of this mount",
                  "description":"Type string. Description of this mount",
                  "strength":"Type integer. Mounts that have this value, such as mining lasers, denote how powerful this mount's capabilities are",
                  "deposits":[
                    "Array of strings. Mounts that have this value denote what goods can be produced from using the mount. Can be an array of the following values - QUARTZ_SAND, SILICON_CRYSTALS, PRECIOUS_STONES, ICE_WATER, AMMONIA_ICE, IRON_ORE, COPPER_ORE, SILVER_ORE, ALUMINUM_ORE, GOLD_ORE, PLATINUM_ORE, DIAMONDS, URANITE_ORE, MERITIUM_ORE"
                  ],
                  "requirements":{
                    "power":"Type integer. The amount of power required from the reactor",
                    "crew":"Type integer. The number of crew required for operation",
                    "slots":"Type integer. The number of module slots required for installation"
                  }
                }
              ],
              "cargo":{
                "capacity":"Type integer. The max number of items that can be stored in the cargo hold. Greater than or equal to 0",
                "units":"Type integer. The number of items currently stored in the cargo hold. Greater than or equal to 0",
                "inventory":[
                  {
                    "symbol":"Type string. The unique identifier of the cargo item type.",
                    "name":"Type string. The name of the cargo item type",
                    "description":"Type string. The description of the cargo item type.",
                    "units":"Type integer. The number of units of the cargo item. Greater than or equal to 1"
                  }
                ]
              },
              "fuel":{
                "current":"Type integer. The current amount of fuel in the ship's tanks. Greater than or equal to 0",
                "capacity":"The maximum amount of fuel the ship's tanks can hold. Greater than or equal to 0",
                "consumed":{
                  "amount":"Type integer. consumed only shows up when an action has consumed fuel in the process. Shows the fuel consumption data consumed by the most recen transit or action",
                  "timestamp":"Type date-time string. The time at which the fuel was consumed."
                }
              }
            },
            "token":"Type string"
          }
        }
        ```
        
- All of your ships, contracts, credits and other game assets will be associated with your agent identity.

### ### Get agent

- If you want to retrieve the details of your agent, use the following command -
    - `GET - https://api.spacetraders.io/v2/my/agent`
    - Response example
        
        ```json
        {
          "data": {
            "accountId": "string",
            "symbol": "string",
            "headquarters": "string",
            "credits": 0,
            "startingFaction": "string",
            "shipCount": 0
          }
        }
        ```
        
    - Together, these details constitute the attributes of an agent at any given point in time.

### ### Viewing all agents

- If you want to view details on all public agents, including those not owned by you, use the following API request
    - `GET - https://api.spacetraders.io/v2/agents`
    - Response example
        
        ```json
        {
          "data": {
            "accountId": "string",
            "symbol": "string",
            "headquarters": "string",
            "credits": 0,
            "startingFaction": "string",
            "shipCount": 0
          }
        }
        ```
        

## ## Factions

- Factions are the primary NPC organizations in SpaceTraders. Each faction will have a unique set of ships, contracts, and trade routes for you to explore.
- Factions are spread across the universe and can be found by exploring new systems.

### ### Viewing Factions

- If you want to view the list of all factions, use the following request
    - `GET - https://api.spacetraders.io/v2/factions`
    - Response example
        
        ```json
        {
          "data": [
            {
              "symbol": "COSMIC",
              "name": "string",
              "description": "string",
              "headquarters": "string",
              "traits": [
                {
                  "symbol": "BUREAUCRATIC",
                  "name": "string",
                  "description": "string"
                }
              ],
              "isRecruiting": true
            }
          ],
          "meta": {
            "total": 0,
            "page": 1,
            "limit": 10
          }
        }
        ```
        
- If you want to view the details of any specific faction, use the following request
    - `GET -https://api.spacetraders.io/v2/factions/{factionSymbol}`
    - Request parameter
        
        ```json
        {
        	"factionSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
        "data": {
        "symbol": "COSMIC",
        "name": "string",
        "description": "string",
        "headquarters": "string",
        "traits": [
        {
        "symbol": "BUREAUCRATIC",
        "name": "string",
        "description": "string"
        }
        ],
        "isRecruiting": true
        }
        }
        ```
        

### ### Faction contracts

- Factions will offer contracts to agents in exchange for credits and reputation. Every agent starts with a basic contract to deliver a mined ore to the faction's home world.
- On accepting a contract, the agent will be given a deadline to complete the terms. If the contract is not completed by the deadline, the agent will lose reputation with the faction.

### ### List contracts

- If you want to view a list of all your current contracts, use the following API request
    - `GET - https://api.spacetraders.io/v2/my/contracts`
    - Response example
        
        ```json
        {
          "data": [
            {
              "id": "string",
              "factionSymbol": "string",
              "type": "PROCUREMENT",
              "terms": {
                "deadline": "2019-08-24T14:15:22Z",
                "payment": {
                  "onAccepted": 0,
                  "onFulfilled": 0
                },
                "deliver": [
                  {
                    "tradeSymbol": "string",
                    "destinationSymbol": "string",
                    "unitsRequired": 0,
                    "unitsFulfilled": 0
                  }
                ]
              },
              "accepted": false,
              "fulfilled": false,
              "expiration": "2019-08-24T14:15:22Z",
              "deadlineToAccept": "2019-08-24T14:15:22Z"
            }
          ],
          "meta": {
            "total": 0,
            "page": 1,
            "limit": 10
          }
        }
        ```
        

### ### Get Contract

- If you want to view a specific contract, use the following API request along with the ID of the specific contract whose details you want to view
    - `GET - https://api.spacetraders.io/v2/my/contracts/{contractId}`
    - Request parameter
        
        ```json
        {
        "factionSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "symbol": "COSMIC",
            "name": "string",
            "description": "string",
            "headquarters": "string",
            "traits": [
              {
                "symbol": "BUREAUCRATIC",
                "name": "string",
                "description": "string"
              }
            ],
            "isRecruiting": true
          }
        }
        ```
        

### ### Accept Contract

- To accept a contract by ID use the following request
    - `POST - https://api.spacetraders.io/v2/my/contracts/{contractId}/accept`
    - Request parameter
        
        ```json
        {
        	"contractId": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "agent": {
              "accountId": "string",
              "symbol": "string",
              "headquarters": "string",
              "credits": 0,
              "startingFaction": "string",
              "shipCount": 0
            },
            "contract": {
              "id": "string",
              "factionSymbol": "string",
              "type": "PROCUREMENT",
              "terms": {
                "deadline": "2019-08-24T14:15:22Z",
                "payment": {
                  "onAccepted": 0,
                  "onFulfilled": 0
                },
                "deliver": [
                  {
                    "tradeSymbol": "string",
                    "destinationSymbol": "string",
                    "unitsRequired": 0,
                    "unitsFulfilled": 0
                  }
                ]
              },
              "accepted": false,
              "fulfilled": false,
              "expiration": "2019-08-24T14:15:22Z",
              "deadlineToAccept": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        
- You can only accept contracts that were ****
    - offered to you,
    - not accepted yet, and
    - whose deadlines has not passed yet.

### ### Deliver cargo to contract

- In order to deliver cargo use the following request
    - `POST - https://api.spacetraders.io/v2/my/contracts/{contractId}/deliver`
    - Request parameter
        
        ```json
        {
        	"contractId": "Type string"
        }
        ```
        
    - Request body
        
        ```json
        {
          "shipSymbol": "string",
          "tradeSymbol": "string",
          "units": 0
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "contract": {
              "id": "string",
              "factionSymbol": "string",
              "type": "PROCUREMENT",
              "terms": {
                "deadline": "2019-08-24T14:15:22Z",
                "payment": {
                  "onAccepted": 0,
                  "onFulfilled": 0
                },
                "deliver": [
                  {
                    "tradeSymbol": "string",
                    "destinationSymbol": "string",
                    "unitsRequired": 0,
                    "unitsFulfilled": 0
                  }
                ]
              },
              "accepted": false,
              "fulfilled": false,
              "expiration": "2019-08-24T14:15:22Z",
              "deadlineToAccept": "2019-08-24T14:15:22Z"
            },
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            }
          }
        }
        ```
        
- In order to make this request, you need to ensure that
    - a ship must be at the delivery location (denoted in the delivery terms as **`destinationSymbol`** of a contract), and
    - the ship must have a number of units of a good required by this contract in its cargo.
- Cargo that was delivered will be removed from the ship's cargo.****

### ### Fulfill contract

- Once all the delivery terms for a contract have been fulfilled, you can use the following request to fulfill the contract
    - `POST - https://api.spacetraders.io/v2/my/contracts/{contractId}/fulfill`
    - Request parameter
        
        ```json
        {
        	"contractId": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "agent": {
              "accountId": "string",
              "symbol": "string",
              "headquarters": "string",
              "credits": 0,
              "startingFaction": "string",
              "shipCount": 0
            },
            "contract": {
              "id": "string",
              "factionSymbol": "string",
              "type": "PROCUREMENT",
              "terms": {
                "deadline": "2019-08-24T14:15:22Z",
                "payment": {
                  "onAccepted": 0,
                  "onFulfilled": 0
                },
                "deliver": [
                  {
                    "tradeSymbol": "string",
                    "destinationSymbol": "string",
                    "unitsRequired": 0,
                    "unitsFulfilled": 0
                  }
                ]
              },
              "accepted": false,
              "fulfilled": false,
              "expiration": "2019-08-24T14:15:22Z",
              "deadlineToAccept": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        

## ## Systems and Waypoints

### ### Overview

- The SpaceTraders universe is made of up of systems and waypoints. Every system has a type, which is typically a type of star, and a set of x, y coordinates.
- Waypoints are locations within a system that you can travel to. They can be planets, asteroids fields, orbital stations, or any other type of location.
- Ships can navigate between waypoints and jump or warp between systems.

### ### Systems

- Systems are the primary locations in the SpaceTraders universe. There are roughly 5,000 systems in the universe to explore. You can view a list of all systems in the universe by making the following request
    - `GET - https://api.spacetraders.io/v2/systems`
    - Request parameter
    - Response example
        
        ```json
        {
          "data": [
            {
              "symbol": "string",
              "sectorSymbol": "string",
              "type": "NEUTRON_STAR",
              "x": 0,
              "y": 0,
              "waypoints": [
                {
                  "symbol": "string",
                  "type": "PLANET",
                  "x": 0,
                  "y": 0
                }
              ],
              "factions": [
                {
                  "symbol": "COSMIC"
                }
              ]
            }
          ],
          "meta": {
            "total": 0,
            "page": 1,
            "limit": 10
          }
        }
        ```
        
- To get details on a specific system, use the following request
    - `GET - https://api.spacetraders.io/v2/systems/{systemSymbol}`
    - Request parameter
        
        ```json
        {
        	"systemSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "symbol": "string",
            "sectorSymbol": "string",
            "type": "NEUTRON_STAR",
            "x": 0,
            "y": 0,
            "waypoints": [
              {
                "symbol": "string",
                "type": "PLANET",
                "x": 0,
                "y": 0
              }
            ],
            "factions": [
              {
                "symbol": "COSMIC"
              }
            ]
          }
        }
        ```
        
- The best way to visualize the game would be to plot the x and y coordinates of each system on a graph, which should show a rough spiral shape of the universe.
- Although a majority of system types are stars, there are also a few other types of systems. Some of these include Black Holes, Neutron Stars, and Nebula.
- Be careful when traveling into unknown systems, as some of these types can be dangerous to your ships.

### ### Waypoints

- Waypoints are locations within a system that you can travel to. They can be planets, asteroids fields, orbital stations, or any other type of traversable location with an x, y coordinate.
- You can view a list of all waypoints in a system by making the following request:
    - `GET - https://api.spacetraders.io/v2/systems/{systemSymbol}/waypoints`
    - Request parameter
        
        ```json
        {
        	"systemSymbol": "Type String",
        	"limit": Type integer. number of entries per page. default to 10. Greater
        						than or equal to 1 and less than or equal to 20,
        	"page": Type integer. number of pages. Default 1
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": [
            {
              "symbol": "string",
              "type": "PLANET",
              "systemSymbol": "string",
              "x": 0,
              "y": 0,
              "orbitals": [
                {
                  "symbol": "string"
                }
              ],
              "faction": {
                "symbol": "COSMIC"
              },
              "traits": [
                {
                  "symbol": "UNCHARTED",
                  "name": "string",
                  "description": "string"
                }
              ],
              "chart": {
                "waypointSymbol": "string",
                "submittedBy": "string",
                "submittedOn": "2019-08-24T14:15:22Z"
              }
            }
          ],
          "meta": {
            "total": 0,
            "page": 1,
            "limit": 10
          }
        }
        ```
        
- A waypoint has a type, a set of x, y coordinates, and a unique symbol name that you can use to reference it. Waypoints may also have orbital waypoints, which are locations within the orbit of the location.
- For example, a planet may have an space station as well as two moons that orbit it. The planet itself would be the parent waypoint, and the space station and moons would be orbital waypoints.
- When visualizing a system, you can map the coordinates of each waypoint to a graph to see where they are located in the system. Orbitals will share the same x, y coordinates as the location they orbit.
- You can get the details on a specific Waypoint by using the following request:
    - `GET - https://api.spacetraders.io/v2/systems/{systemSymbol}/waypoints/{waypointSymbol}`
    - Request parameter
        
        ```json
        {
        	"systemSymbol": "Type string",
        	"waypointSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "symbol": "string",
            "type": "PLANET",
            "systemSymbol": "string",
            "x": 0,
            "y": 0,
            "orbitals": [
              {
                "symbol": "string"
              }
            ],
            "faction": {
              "symbol": "COSMIC"
            },
            "traits": [
              {
                "symbol": "UNCHARTED",
                "name": "string",
                "description": "string"
              }
            ],
            "chart": {
              "waypointSymbol": "string",
              "submittedBy": "string",
              "submittedOn": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        

### ### Traits

- Waypoints have a list of traits that describe the details, features and characteristics of the location. For example, a planet may have a trait that describes the atmosphere as "toxic", the political leanings as "authoritarian", or the population as "sparse".
- Two traits that are particularly important are the **`MARKETPLACE`** and **`SHIPYARD`** traits. These traits indicate that the location has a marketplace and shipyard, respectively.
- The Marketplace trait allows you to buy or sell goods at the waypoint.
    - To retrieve imports, exports and exchange data from a waypoint, it is required that the waypoint have the **`Marketplace`** trait to use.
    - Once you confirm a waypoint has a Marketplace trait, use the following request to access trade good prices and recent transactions
    - `GET - https://api.spacetraders.io/v2/systems/{systemSymbol}/waypoints/{waypointSymbol}/market`
    - Request parameter
        
        ```json
        {
        	"systemSymbol": "Type string",
        	"waypointSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "symbol": "string",
            "exports": [
              {
                "symbol": "PRECIOUS_STONES",
                "name": "string",
                "description": "string"
              }
            ],
            "imports": [
              {
                "symbol": "PRECIOUS_STONES",
                "name": "string",
                "description": "string"
              }
            ],
            "exchange": [
              {
                "symbol": "PRECIOUS_STONES",
                "name": "string",
                "description": "string"
              }
            ],
            "transactions": [
              {
                "waypointSymbol": "string",
                "shipSymbol": "string",
                "tradeSymbol": "string",
                "type": "PURCHASE",
                "units": 0,
                "pricePerUnit": 0,
                "totalPrice": 0,
                "timestamp": "2019-08-24T14:15:22Z"
              }
            ],
            "tradeGoods": [
              {
                "symbol": "string",
                "tradeVolume": 1,
                "supply": "SCARCE",
                "purchasePrice": 0,
                "sellPrice": 0
              }
            ]
          }
        }
        ```
        
    - It’s important to note that the list of recent transactions and tradeGoods at a Marketplace are only available when a ship is present at the market.
- The Shipyard trait allows you to purchase a new ship.
    - To retrieve information on list of ship types available for purchase at a yard, it is required that the waypoint have the `Shipyard` trait to use.
    - Once you confirm a waypoint has a Shipyard trait, use the following request to access list of ships available for purchase
    - `GET - https://api.spacetraders.io/v2/systems/{systemSymbol}/waypoints/{waypointSymbol}/shipyard`
    - Request parameter
        
        ```json
        {
        	"systemSymbol": "Type string",
        	"waypointSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "symbol": "string",
            "shipTypes": [
              {
                "type": "SHIP_PROBE"
              }
            ],
            "transactions": [
              {
                "waypointSymbol": "string",
                "shipSymbol": "string",
                "price": 0,
                "agentSymbol": "string",
                "timestamp": "2019-08-24T14:15:22Z"
              }
            ],
            "ships": [
              {
                "type": "SHIP_PROBE",
                "name": "string",
                "description": "string",
                "purchasePrice": 0,
                "frame": {
                  "symbol": "FRAME_PROBE",
                  "name": "string",
                  "description": "string",
                  "condition": 0,
                  "moduleSlots": 0,
                  "mountingPoints": 0,
                  "fuelCapacity": 0,
                  "requirements": {
                    "power": 0,
                    "crew": 0,
                    "slots": 0
                  }
                },
                "reactor": {
                  "symbol": "REACTOR_SOLAR_I",
                  "name": "string",
                  "description": "string",
                  "condition": 0,
                  "powerOutput": 1,
                  "requirements": {
                    "power": 0,
                    "crew": 0,
                    "slots": 0
                  }
                },
                "engine": {
                  "symbol": "ENGINE_IMPULSE_DRIVE_I",
                  "name": "string",
                  "description": "string",
                  "condition": 0,
                  "speed": 1,
                  "requirements": {
                    "power": 0,
                    "crew": 0,
                    "slots": 0
                  }
                },
                "modules": [
                  {
                    "symbol": "MODULE_MINERAL_PROCESSOR_I",
                    "capacity": 0,
                    "range": 0,
                    "name": "string",
                    "description": "string",
                    "requirements": {
                      "power": 0,
                      "crew": 0,
                      "slots": 0
                    }
                  }
                ],
                "mounts": [
                  {
                    "symbol": "MOUNT_GAS_SIPHON_I",
                    "name": "string",
                    "description": "string",
                    "strength": 0,
                    "deposits": [
                      "QUARTZ_SAND"
                    ],
                    "requirements": {
                      "power": 0,
                      "crew": 0,
                      "slots": 0
                    }
                  }
                ]
              }
            ]
          }
        }
        ```
        
- Apart from `Shipyard` and `Marketplace`, a third important trait is that of `Jump Gate`
    - To know the list of systems that have a Jump Gate in range of the current Waypoint’s Jump Gate, use the following request
    - `GET - https://api.spacetraders.io/v2/systems/{systemSymbol}/waypoints/{waypointSymbol}/jump-gate`
    - Request parameter
        
        ```json
        {
        	"systemSymbol": "Type string",
        	"waypointSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "jumpRange": 0,
            "factionSymbol": "string",
            "connectedSystems": [
              {
                "symbol": "string",
                "sectorSymbol": "string",
                "type": "NEUTRON_STAR",
                "factionSymbol": "string",
                "x": 0,
                "y": 0,
                "distance": 0
              }
            ]
          }
        }
        ```
        

## ## Ship Navigation

### ### Overview

- Ships can navigate between waypoints within a system and jump or warp between systems across the SpaceTraders universe. Ships can also dock at waypoints to refuel, repair, and trade goods.
- Movement in the SpaceTraders universe is based on a grid system. Ships can only move from and to other waypoints within the same system. To move between systems, you must either jump or warp your ship.
- When traveling to a new waypoint or system, your ship nav's **`arrival`** timestamp will be updated to reflect when your ship will arrive at it's destination.

### ### Ships

- To list the ships under your agent’s ownership, use the following request
    - `GET - https://api.spacetraders.io/v2/my/ships`
    - Request parameter
        
        ```json
        {
        	"limit": Type integer. How many entries per page. Can be greater than 
        						or equal to 1 and less than or equal to 20,
        	"page": Type integer. What entry offset to request
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": [
            {
              "symbol": "string",
              "registration": {
                "name": "string",
                "factionSymbol": "string",
                "role": "FABRICATOR"
              },
              "nav": {
                "systemSymbol": "string",
                "waypointSymbol": "string",
                "route": {
                  "destination": {
                    "symbol": "string",
                    "type": "PLANET",
                    "systemSymbol": "string",
                    "x": 0,
                    "y": 0
                  },
                  "departure": {
                    "symbol": "string",
                    "type": "PLANET",
                    "systemSymbol": "string",
                    "x": 0,
                    "y": 0
                  },
                  "departureTime": "2019-08-24T14:15:22Z",
                  "arrival": "2019-08-24T14:15:22Z"
                },
                "status": "IN_TRANSIT",
                "flightMode": "CRUISE"
              },
              "crew": {
                "current": 0,
                "required": 0,
                "capacity": 0,
                "rotation": "STRICT",
                "morale": 0,
                "wages": 0
              },
              "frame": {
                "symbol": "FRAME_PROBE",
                "name": "string",
                "description": "string",
                "condition": 0,
                "moduleSlots": 0,
                "mountingPoints": 0,
                "fuelCapacity": 0,
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              },
              "reactor": {
                "symbol": "REACTOR_SOLAR_I",
                "name": "string",
                "description": "string",
                "condition": 0,
                "powerOutput": 1,
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              },
              "engine": {
                "symbol": "ENGINE_IMPULSE_DRIVE_I",
                "name": "string",
                "description": "string",
                "condition": 0,
                "speed": 1,
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              },
              "modules": [
                {
                  "symbol": "MODULE_MINERAL_PROCESSOR_I",
                  "capacity": 0,
                  "range": 0,
                  "name": "string",
                  "description": "string",
                  "requirements": {
                    "power": 0,
                    "crew": 0,
                    "slots": 0
                  }
                }
              ],
              "mounts": [
                {
                  "symbol": "MOUNT_GAS_SIPHON_I",
                  "name": "string",
                  "description": "string",
                  "strength": 0,
                  "deposits": [
                    "QUARTZ_SAND"
                  ],
                  "requirements": {
                    "power": 0,
                    "crew": 0,
                    "slots": 0
                  }
                }
              ],
              "cargo": {
                "capacity": 0,
                "units": 0,
                "inventory": [
                  {
                    "symbol": "string",
                    "name": "string",
                    "description": "string",
                    "units": 1
                  }
                ]
              },
              "fuel": {
                "current": 0,
                "capacity": 0,
                "consumed": {
                  "amount": 0,
                  "timestamp": "2019-08-24T14:15:22Z"
                }
              }
            }
          ],
          "meta": {
            "total": 0,
            "page": 1,
            "limit": 10
          }
        }
        ```
        
- To retrieve the cargo of a ship under your agent’s ownership, use the following request
    - `GET - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/cargo`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "capacity": 0,
            "units": 0,
            "inventory": [
              {
                "symbol": "string",
                "name": "string",
                "description": "string",
                "units": 1
              }
            ]
          }
        }
        ```
        

### ### Purchase a ship

- In order to purchase a ship, a ship under your agent's ownership must be in a waypoint that has the **`Shipyard`** trait, and the Shipyard must sell the type of the desired ship.
- Shipyards typically offer ship types, which are predefined templates of ships that have dedicated roles. A template comes with a preset of an engine, a reactor, and a frame. It may also include a few modules and mounts.****
- Use the following request to purchase a ship from a shipyard.
    - `POST - https://api.spacetraders.io/v2/my/ships`
    - Request parameter
        
        ```json
        {
        
        “shipType”: “Type string”
        “waypointSymbol”: "Type string"
        
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "agent": {
              "accountId": "string",
              "symbol": "string",
              "headquarters": "string",
              "credits": 0,
              "startingFaction": "string",
              "shipCount": 0
            },
            "ship": {
              "symbol": "string",
              "registration": {
                "name": "string",
                "factionSymbol": "string",
                "role": "FABRICATOR"
              },
              "nav": {
                "systemSymbol": "string",
                "waypointSymbol": "string",
                "route": {
                  "destination": {
                    "symbol": "string",
                    "type": "PLANET",
                    "systemSymbol": "string",
                    "x": 0,
                    "y": 0
                  },
                  "departure": {
                    "symbol": "string",
                    "type": "PLANET",
                    "systemSymbol": "string",
                    "x": 0,
                    "y": 0
                  },
                  "departureTime": "2019-08-24T14:15:22Z",
                  "arrival": "2019-08-24T14:15:22Z"
                },
                "status": "IN_TRANSIT",
                "flightMode": "CRUISE"
              },
              "crew": {
                "current": 0,
                "required": 0,
                "capacity": 0,
                "rotation": "STRICT",
                "morale": 0,
                "wages": 0
              },
              "frame": {
                "symbol": "FRAME_PROBE",
                "name": "string",
                "description": "string",
                "condition": 0,
                "moduleSlots": 0,
                "mountingPoints": 0,
                "fuelCapacity": 0,
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              },
              "reactor": {
                "symbol": "REACTOR_SOLAR_I",
                "name": "string",
                "description": "string",
                "condition": 0,
                "powerOutput": 1,
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              },
              "engine": {
                "symbol": "ENGINE_IMPULSE_DRIVE_I",
                "name": "string",
                "description": "string",
                "condition": 0,
                "speed": 1,
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              },
              "modules": [
                {
                  "symbol": "MODULE_MINERAL_PROCESSOR_I",
                  "capacity": 0,
                  "range": 0,
                  "name": "string",
                  "description": "string",
                  "requirements": {
                    "power": 0,
                    "crew": 0,
                    "slots": 0
                  }
                }
              ],
              "mounts": [
                {
                  "symbol": "MOUNT_GAS_SIPHON_I",
                  "name": "string",
                  "description": "string",
                  "strength": 0,
                  "deposits": [
                    "QUARTZ_SAND"
                  ],
                  "requirements": {
                    "power": 0,
                    "crew": 0,
                    "slots": 0
                  }
                }
              ],
              "cargo": {
                "capacity": 0,
                "units": 0,
                "inventory": [
                  {
                    "symbol": "string",
                    "name": "string",
                    "description": "string",
                    "units": 1
                  }
                ]
              },
              "fuel": {
                "current": 0,
                "capacity": 0,
                "consumed": {
                  "amount": 0,
                  "timestamp": "2019-08-24T14:15:22Z"
                }
              }
            },
            "transaction": {
              "waypointSymbol": "string",
              "shipSymbol": "string",
              "price": 0,
              "agentSymbol": "string",
              "timestamp": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        
- To retrieve details of a specific ship under your agent’s ownership, then use the following request
    - `GET -https://api.spacetraders.io/v2/my/ships/{shipSymbol}`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "symbol": "string",
            "registration": {
              "name": "string",
              "factionSymbol": "string",
              "role": "FABRICATOR"
            },
            "nav": {
              "systemSymbol": "string",
              "waypointSymbol": "string",
              "route": {
                "destination": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departure": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departureTime": "2019-08-24T14:15:22Z",
                "arrival": "2019-08-24T14:15:22Z"
              },
              "status": "IN_TRANSIT",
              "flightMode": "CRUISE"
            },
            "crew": {
              "current": 0,
              "required": 0,
              "capacity": 0,
              "rotation": "STRICT",
              "morale": 0,
              "wages": 0
            },
            "frame": {
              "symbol": "FRAME_PROBE",
              "name": "string",
              "description": "string",
              "condition": 0,
              "moduleSlots": 0,
              "mountingPoints": 0,
              "fuelCapacity": 0,
              "requirements": {
                "power": 0,
                "crew": 0,
                "slots": 0
              }
            },
            "reactor": {
              "symbol": "REACTOR_SOLAR_I",
              "name": "string",
              "description": "string",
              "condition": 0,
              "powerOutput": 1,
              "requirements": {
                "power": 0,
                "crew": 0,
                "slots": 0
              }
            },
            "engine": {
              "symbol": "ENGINE_IMPULSE_DRIVE_I",
              "name": "string",
              "description": "string",
              "condition": 0,
              "speed": 1,
              "requirements": {
                "power": 0,
                "crew": 0,
                "slots": 0
              }
            },
            "modules": [
              {
                "symbol": "MODULE_MINERAL_PROCESSOR_I",
                "capacity": 0,
                "range": 0,
                "name": "string",
                "description": "string",
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              }
            ],
            "mounts": [
              {
                "symbol": "MOUNT_GAS_SIPHON_I",
                "name": "string",
                "description": "string",
                "strength": 0,
                "deposits": [
                  "QUARTZ_SAND"
                ],
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              }
            ],
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            },
            "fuel": {
              "current": 0,
              "capacity": 0,
              "consumed": {
                "amount": 0,
                "timestamp": "2019-08-24T14:15:22Z"
              }
            }
          }
        }
        ```
        

### ### Orbiting

- Before a ship can travel, you must first confirm that the ship is undocked and in-orbit.
- To confirm if the ship is in orbit or to get it to orbit, use the following request
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/orbit`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "nav": {
              "systemSymbol": "string",
              "waypointSymbol": "string",
              "route": {
                "destination": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departure": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departureTime": "2019-08-24T14:15:22Z",
                "arrival": "2019-08-24T14:15:22Z"
              },
              "status": "IN_TRANSIT",
              "flightMode": "CRUISE"
            }
          }
        }
        ```
        
    - The request is idempotent - successive requests will succeed even if the ship is already in orbit.****

### ### Docking

- Conversely to dock a ship, use the following request
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/dock`
    - Request parameter
        
        ```json
        {
        	shipSymbol: "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "nav": {
              "systemSymbol": "string",
              "waypointSymbol": "string",
              "route": {
                "destination": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departure": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departureTime": "2019-08-24T14:15:22Z",
                "arrival": "2019-08-24T14:15:22Z"
              },
              "status": "IN_TRANSIT",
              "flightMode": "CRUISE"
            }
          }
        }
        ```
        
    - The request is idempotent - successive requests will succeed even if the ship is already docked.

### ### Flight mode

- Your ship's flight mode determines the rate at which it travels and the amount of fuel consumed.
- There are four flight modes:
    - **CRUISE** - Cruise flight mode is the default mode for all ships. It consumes fuel at a normal rate and travels at a normal speed.
    - **BURN** - Burn flight mode consumes fuel at a faster rate and travels at a faster speed.
    - **DRIFT** - Drift flight mode consumes the least fuel and travels at a much slower speed. Drift mode is useful when your ship has run out of fuel and you need to conserve what little fuel you have left.
    - **STEALTH** - Stealth flight mode runs with systems at a minimum, making it difficult to detect. It consumes fuel at a normal rate but travels at a reduced speed.
- To update your ship’s flight mode, use the following request
    - `PATCH - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/nav`
    - Request parameter
        
        ```json
        {
        	"flightModel" : "Type string. Can be one of following values - CRUISE,
        									BURN, DRIFT, STEALTH"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "systemSymbol": "string",
            "waypointSymbol": "string",
            "route": {
              "destination": {
                "symbol": "string",
                "type": "PLANET",
                "systemSymbol": "string",
                "x": 0,
                "y": 0
              },
              "departure": {
                "symbol": "string",
                "type": "PLANET",
                "systemSymbol": "string",
                "x": 0,
                "y": 0
              },
              "departureTime": "2019-08-24T14:15:22Z",
              "arrival": "2019-08-24T14:15:22Z"
            },
            "status": "IN_TRANSIT",
            "flightMode": "CRUISE"
          }
        }
        ```
        

### ### Waypoint navigation

- When navigating, a ship will move to the destination, but it will not arrive immediately. Instead, it will take time to travel to the destination, and you can use the navigation status to determine when the ship will arrive.
- To get the navigation status of a ship, use the following endpoint
    - `GET - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/nav`
    - Request parameter
        
        ```json
        {
        	"shipSymbol" : "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "systemSymbol": "string",
            "waypointSymbol": "string",
            "route": {
              "destination": {
                "symbol": "string",
                "type": "PLANET",
                "systemSymbol": "string",
                "x": 0,
                "y": 0
              },
              "departure": {
                "symbol": "string",
                "type": "PLANET",
                "systemSymbol": "string",
                "x": 0,
                "y": 0
              },
              "departureTime": "2019-08-24T14:15:22Z",
              "arrival": "2019-08-24T14:15:22Z"
            },
            "status": "IN_TRANSIT",
            "flightMode": "CRUISE"
          }
        }
        ```
        
- Navigation also consumes fuel, which is required to power the ship's engines. The amount of fuel consumed is based on the distance between the current location and the destination.
- The returned response will detail the route information including the expected time of arrival. Most ship actions are unavailable until the ship has arrived at it's destination.
- To navigate your ship to a waypoint, you can make the following request
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/navigate`
    - Request parameter
        
        ```json
        {
        	"waypointSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "fuel": {
              "current": 0,
              "capacity": 0,
              "consumed": {
                "amount": 0,
                "timestamp": "2019-08-24T14:15:22Z"
              }
            },
            "nav": {
              "systemSymbol": "string",
              "waypointSymbol": "string",
              "route": {
                "destination": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departure": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departureTime": "2019-08-24T14:15:22Z",
                "arrival": "2019-08-24T14:15:22Z"
              },
              "status": "IN_TRANSIT",
              "flightMode": "CRUISE"
            }
          }
        }
        ```
        
- The destination waypoint must be within the same system as the ship's current location.
- To travel between systems, see the ship's Warp or Jump actions.

### ### Warping

- When moving between systems, the ship can choose to jump or warp depending on the ship's technology.
- Warping your ship moves it into inter-dimensional space, and it behaves very similar to normal waypoint travel in that it takes time and consumes normal fuel.
- To warp your ship to a new system, you can make the following request
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/warp`
    - Request parameter
        
        ```json
        {
        	"waypointSymbol" : "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "fuel": {
              "current": 0,
              "capacity": 0,
              "consumed": {
                "amount": 0,
                "timestamp": "2019-08-24T14:15:22Z"
              }
            },
            "nav": {
              "systemSymbol": "string",
              "waypointSymbol": "string",
              "route": {
                "destination": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departure": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departureTime": "2019-08-24T14:15:22Z",
                "arrival": "2019-08-24T14:15:22Z"
              },
              "status": "IN_TRANSIT",
              "flightMode": "CRUISE"
            }
          }
        }
        ```
        
    - The ship must be in orbit to use this function.
    - The ship must have the **`Warp Drive`** module installed. Warping will consume the necessary fuel from the ship's manifest.

### ### Jumping

- Jumping a ship however is instantaneous, but it requires a jump drive and a unit of antimatter. Jump drives are expensive and rare, but your command ship starts with one.
- To jump your ship to a new system, you can make the following request
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/jump`
    - Request parameter
        
        ```json
        {
        	"systemSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "cooldown": {
              "shipSymbol": "string",
              "totalSeconds": 0,
              "remainingSeconds": 0,
              "expiration": "2019-08-24T14:15:22Z"
            },
            "nav": {
              "systemSymbol": "string",
              "waypointSymbol": "string",
              "route": {
                "destination": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departure": {
                  "symbol": "string",
                  "type": "PLANET",
                  "systemSymbol": "string",
                  "x": 0,
                  "y": 0
                },
                "departureTime": "2019-08-24T14:15:22Z",
                "arrival": "2019-08-24T14:15:22Z"
              },
              "status": "IN_TRANSIT",
              "flightMode": "CRUISE"
            }
          }
        }
        ```
        
    - The ship must be in orbit to use this function.
    - When used while in orbit of a Jump Gate waypoint, any ship can use this command, jumping to the target system's Jump Gate waypoint.
    - When used elsewhere, jumping requires the ship to have a **`Jump Drive`** module installed and consumes a unit of antimatter from the ship's cargo. The command will fail if there is no antimatter to consume. ****
    - When jumping via the **`Jump Drive`** module, the ship ends up at its largest source of energy in the system, such as a gas planet or a jump gate.

### ### Gate travel

- If your ship isn't equipped with a jump or warp drive, you can still travel between systems by using faction gates. Gates are special waypoints that allow you to travel between systems.
- Some faction gates are restricted based on your agent's reputation with the faction. To unlock access to new systems, you can increase your reputation with the faction by completing contracts for them.

### ### Refueling

- Ships can be refueled at any waypoint with a marketplace that offers fuel for sell.
- One unit of fuel at the marketplace replenishes 100 units in the ship's tank. Often fuel prices can be driven high by demand, so it's best to refuel your ship when fuel prices are low or to help drive prices down by selling fuel at high-traffic marketplaces.
- Use the following request to refuel a ship
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/refuel`
    - Request parameter
        
        ```json
        {
        	"units": Type integer. The amount of fuel to fill in the ship's tanks. 
        					When not specified, the ship will be refueled to its maximum 
        					fuel capacity. If the amount specified is greater than the 
        					ship's remaining capacity, the ship will only be refueled to 
        					its maximum fuel capacity. The amount specified is not in market 
        					units but in ship fuel units.
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "agent": {
              "accountId": "string",
              "symbol": "string",
              "headquarters": "string",
              "credits": 0,
              "startingFaction": "string",
              "shipCount": 0
            },
            "fuel": {
              "current": 0,
              "capacity": 0,
              "consumed": {
                "amount": 0,
                "timestamp": "2019-08-24T14:15:22Z"
              }
            },
            "transaction": {
              "waypointSymbol": "string",
              "shipSymbol": "string",
              "tradeSymbol": "string",
              "type": "PURCHASE",
              "units": 0,
              "pricePerUnit": 0,
              "totalPrice": 0,
              "timestamp": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        

### ### Scanning Systems

- Some ships can scan for nearby systems, retrieving information on the systems' distance from the ship and their waypoints.
- However, this requires a ship to have the **`Sensor Array`** mount installed to use.
- The ship will enter a cooldown after using this function, during which it cannot execute certain actions.****
- To scan nearby systems, use the following request -
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/scan/systems`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "cooldown": {
              "shipSymbol": "string",
              "totalSeconds": 0,
              "remainingSeconds": 0,
              "expiration": "2019-08-24T14:15:22Z"
            },
            "systems": [
              {
                "symbol": "string",
                "sectorSymbol": "string",
                "type": "NEUTRON_STAR",
                "x": 0,
                "y": 0,
                "distance": 0
              }
            ]
          }
        }
        ```
        

### ### Scanning Waypoints

- Ships can scan for nearby waypoints within a system, retrieving detailed information on each waypoint in range. Scanning uncharted waypoints will allow you to ignore their uncharted state and will list the waypoints' traits.
- However, this requires a ship to have the **`Sensor Array`** mount installed to use.
- The ship will enter a cooldown after using this function, during which it cannot execute certain actions.****
- To scan nearby waypoints, use the following request -
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/scan/waypoints`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "cooldown": {
              "shipSymbol": "string",
              "totalSeconds": 0,
              "remainingSeconds": 0,
              "expiration": "2019-08-24T14:15:22Z"
            },
            "waypoints": [
              {
                "symbol": "string",
                "type": "PLANET",
                "systemSymbol": "string",
                "x": 0,
                "y": 0,
                "orbitals": [
                  {
                    "symbol": "string"
                  }
                ],
                "faction": {
                  "symbol": "COSMIC"
                },
                "traits": [
                  {
                    "symbol": "UNCHARTED",
                    "name": "string",
                    "description": "string"
                  }
                ],
                "chart": {
                  "waypointSymbol": "string",
                  "submittedBy": "string",
                  "submittedOn": "2019-08-24T14:15:22Z"
                }
              }
            ]
          }
        }
        ```
        

### ### Scanning ships

- Ships can also scan for nearby ships, retrieving information on all ships in range.
- However, this requires a ship to have the **`Sensor Array`** mount installed to use.
- The ship will enter a cooldown after using this function, during which it cannot execute certain actions.
- To scan nearby ships, use the following request -
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/scan/ships`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "cooldown": {
              "shipSymbol": "string",
              "totalSeconds": 0,
              "remainingSeconds": 0,
              "expiration": "2019-08-24T14:15:22Z"
            },
            "ships": [
              {
                "symbol": "string",
                "registration": {
                  "name": "string",
                  "factionSymbol": "string",
                  "role": "FABRICATOR"
                },
                "nav": {
                  "systemSymbol": "string",
                  "waypointSymbol": "string",
                  "route": {
                    "destination": {
                      "symbol": "string",
                      "type": "PLANET",
                      "systemSymbol": "string",
                      "x": 0,
                      "y": 0
                    },
                    "departure": {
                      "symbol": "string",
                      "type": "PLANET",
                      "systemSymbol": "string",
                      "x": 0,
                      "y": 0
                    },
                    "departureTime": "2019-08-24T14:15:22Z",
                    "arrival": "2019-08-24T14:15:22Z"
                  },
                  "status": "IN_TRANSIT",
                  "flightMode": "CRUISE"
                },
                "frame": {
                  "symbol": "string"
                },
                "reactor": {
                  "symbol": "string"
                },
                "engine": {
                  "symbol": "string"
                },
                "mounts": [
                  {
                    "symbol": "string"
                  }
                ]
              }
            ]
          }
        }
        ```
        

### ### Ship Cooldown

- Some actions such as activating your jump drive, scanning, or extracting resources taxes your reactor and results in a cooldown.
- Your ship cannot perform additional actions until your cooldown has expired. The duration of your cooldown is relative to the power consumption of the related modules or mounts for the action taken.
- To retrieve the ship’s cooldown, if any, use the following request -
    - `GET - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/cooldown`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "shipSymbol": "string",
            "totalSeconds": 0,
            "remainingSeconds": 0,
            "expiration": "2019-08-24T14:15:22Z"
          }
        }
        ```
        
- In the case that ship has no cooldown, then no response is provided

### ### Create Charts

- Most waypoints in the universe are uncharted by default. These waypoints have their traits hidden until they have been charted by a ship.
- Charting a waypoint will record your agent as the one who created the chart, and all other agents would also be able to see the waypoint's traits.****
- Use the following request to command a ship to chart the waypoint at its current location.
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/chart`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "chart": {
              "waypointSymbol": "string",
              "submittedBy": "string",
              "submittedOn": "2019-08-24T14:15:22Z"
            },
            "waypoint": {
              "symbol": "string",
              "type": "PLANET",
              "systemSymbol": "string",
              "x": 0,
              "y": 0,
              "orbitals": [
                {
                  "symbol": "string"
                }
              ],
              "faction": {
                "symbol": "COSMIC"
              },
              "traits": [
                {
                  "symbol": "UNCHARTED",
                  "name": "string",
                  "description": "string"
                }
              ],
              "chart": {
                "waypointSymbol": "string",
                "submittedBy": "string",
                "submittedOn": "2019-08-24T14:15:22Z"
              }
            }
          }
        }
        ```
        

## ## Resource Extraction

### ### Overview

- Ships can extract resources from asteroids and other celestial bodies. The amount of resources extracted is based on the ship's mounts and modules, such as mining lasers and mineral processing bays.
- Typically yields from an extraction will be a random quantity of those available based on the traits of the location. However, if you have a surveying mount installed, you can survey the location and find more targeted locations to extract from, which will yield more of the resource you are looking for.

### ### Extracting resources

- To extract resources from a location, make the following request -
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/extract`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Request body
        
        ```json
        {
          "survey": {
            "signature": "Type string. A unique signature for the location of 
        									this survey. This signature is verified when 
        									attempting an extraction using this survey.",
            "symbol": "Type string. The symbol of the waypoint that this survey 
        							is for.",
            "deposits": [
              {
                "symbol": "Type string. A list of deposits that can be found at 
        									this location."
              }
            ],
            "expiration": "2019-08-24T14:15:22Z Type date-time string. The date 
        									and time when the survey expires. After this date 
        									and time, the survey will no longer be available 
        									for extraction.",
            "size": "Type string. The size of the deposit. This value indicates 
        						how much can be extracted from the survey before it is 
        						exhausted. It can be one of the following values - SMALL,
        						MODERATE, LARGE"
          }
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "cooldown": {
              "shipSymbol": "string",
              "totalSeconds": 0,
              "remainingSeconds": 0,
              "expiration": "2019-08-24T14:15:22Z"
            },
            "extraction": {
              "shipSymbol": "string",
              "yield": {
                "symbol": "PRECIOUS_STONES",
                "units": 0
              }
            },
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            }
          }
        }
        ```
        
    - The ship must be in orbit to be able to extract and must have mining equipments installed that can extract goods, such as the **`Gas Siphon`** mount for gas-based goods or **`Mining Laser`** mount for ore-based goods.****
    - After an extraction, your ship will be on a cooldown. You can still navigate while on a cooldown, but you cannot perform most other ship actions.

### ### Refining resources

- If your ship has a `Refinery` module installed, then it will be able to refine the raw materials extracted.
- When refining, 30 basic goods will be converted into 10 processed goods.****
- Use the following request to refine extracted resources
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/refine`
    - Request parameter
    - Request body
        
        ```json
        {
        	"produce": "Type string. Can be one of the following - IRON, COPPER,
        							SILVER, GOLD, ALUMINUM, PLATINUM, URANITE, MERITIUM, FUEL"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            },
            "cooldown": {
              "shipSymbol": "string",
              "totalSeconds": 0,
              "remainingSeconds": 0,
              "expiration": "2019-08-24T14:15:22Z"
            },
            "produced": [
              {
                "tradeSymbol": "string",
                "units": 0
              }
            ],
            "consumed": [
              {
                "tradeSymbol": "string",
                "units": 0
              }
            ]
          }
        }
        ```
        
- After refining, your ship will be on a cooldown. You can still navigate while on a cooldown, but you cannot perform most other ship actions.

### ### Surveying

- You can survey a waypoint to find more targeted locations to extract from. Surveying a waypoint will yield a list of locations that are more likely to yield the resource you are looking for, such as asteroid fields.
- Use the following request to create a survey
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/survey`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "cooldown": {
              "shipSymbol": "string",
              "totalSeconds": 0,
              "remainingSeconds": 0,
              "expiration": "2019-08-24T14:15:22Z"
            },
            "surveys": [
              {
                "signature": "string",
                "symbol": "string",
                "deposits": [
                  {
                    "symbol": "string"
                  }
                ],
                "expiration": "2019-08-24T14:15:22Z",
                "size": "SMALL"
              }
            ]
          }
        }
        ```
        
- A survey focuses on specific types of deposits from the extracted location. When ships extract using this survey, they are guaranteed to procure a high amount of one of the goods in the survey.
- Each survey may have multiple deposits, and if a symbol shows up more than once, that indicates a higher chance of extracting that resource.
- Your ship will enter a cooldown after surveying in which it is unable to perform certain actions. Surveys will eventually expire after a period of time or will be exhausted after being extracted several times based on the survey's size. Multiple ships can use the same survey for extraction.
- A ship must have the **`Surveyor`** mount installed in order to use this function.
- To use a previously conducted survey, you must call the extract endpoint with the full response of the survey.
- Surveys have a limited lifetime, so you must use the survey result before it expires. Surveys also have a limited deposit size, and will eventually be depleted.

## ## Markets and Trading

### ### Overview

- Markets in SpaceTraders are driven by supply and demand. Marketplaces will list of goods for trade that are either imports, exports, or exchange goods.
- Exports are goods produced at the waypoint, and typically have a lower purchase price than import goods.
- Import goods are consumed at the waypoint, and typically have a higher sell price.
- Trade routes between imports and exports will typically be the most profitable way to earn credits in SpaceTraders.

### ### Visibility

- Price data and transaction history are only available if you have a ship located at the waypoint. It is common to send cheap probe ships into orbit around a waypoint to gather and monitor market data.
- Having visibility into market data over time can be highly profitable. You can use this data to predict the future price of goods and plan your trade routes accordingly.

### ### Exports

- Exports are goods produced over time by the civilization where the good is listed.
- The purchase price of an export good tends to decrease over time as supply naturally increases from production. As agents buy up supply from an export good, prices fall and production tends to increase to meet the demand.
- Exports are also constrained by the supply of imports. When a market has an unmet need for importing goods, production for export goods will be constrained, typically leading to an increase in export prices.

### ### Imports

- Imports are goods consumed over time by the civilization where the good is listed.
- The sell price of an import good tends to increase over time as supply naturally decreases from consumption. As agents supply more of an import good, consumption will typically increase as the price for that good goes down.
- If a waypoint has an unmet supply for their imports, it can contribute to instability and potentially cause a collapse of the market. This can lead to increased piracy in the system, making it more dangerous for agents to trade and exchange goods.

### ### Exchange goods

- Another type of market listing includes exchange goods. These goods are neither consumed nor produced at the waypoint. Instead, they are traded strictly among agents.
- The price of an exchange good tends to fluctuate based on supply and demand driven by agents.
- The following list of goods can be exported, imported or traded
    
    PRECIOUS_STONES, QUARTZ_SAND, SILICON_CRYSTALS, AMMONIA_ICE, LIQUID_HYDROGEN, LIQUID_NITROGEN, ICE_WATER, EXOTIC_MATTER
    ADVANCED_CIRCUITRY, GRAVITON_EMITTERS, IRON, IRON_ORE, COPPER
    COPPER_ORE, ALUMINUM, ALUMINUM_ORE, SILVER, SILVER_ORE, GOLD, GOLD_ORE
    PLATINUM, PLATINUM_ORE, DIAMONDS, URANITE, URANITE_ORE, MERITIUM, MERITIUM_ORE, HYDROCARBON, ANTIMATTER, FERTILIZERS, FABRICS, FOOD, JEWELRY, MACHINERY FIREARMS, ASSAULT_RIFLES, MILITARY_EQUIPMENT, EXPLOSIVES, LAB_INSTRUMENTS, AMMUNITION, ELECTRONICS, SHIP_PLATING
    EQUIPMENT, FUEL, MEDICINE, DRUGS, CLOTHING, MICROPROCESSORS, PLASTICS
    POLYNUCLEOTIDES, BIOCOMPOSITES, NANOBOTS, AI_MAINFRAMES, QUANTUM_DRIVES, ROBOTIC_DRONES, CYBER_IMPLANTS, GENE_THERAPEUTICS, NEURAL_CHIPSl, MOOD_REGULATORS, VIRAL_AGENTS, MICRO_FUSION_GENERATORS
    SUPERGRAINS, LASER_RIFLES, HOLOGRAPHICS, SHIP_SALVAGE, RELIC_TECH, NOVEL_LIFEFORMS, BOTANICAL_SPECIMENS, CULTURAL_ARTIFACTS, REACTOR_SOLAR_I, REACTOR_FUSION_I, REACTOR_FISSION_I, REACTOR_CHEMICAL_I, REACTOR_ANTIMATTER_I, ENGINE_IMPULSE_DRIVE_I, ENGINE_ION_DRIVE_I, ENGINE_ION_DRIVE_II, ENGINE_HYPER_DRIVE_I, MODULE_MINERAL_PROCESSOR_I, MODULE_CARGO_HOLD_I, MODULE_CREW_QUARTERS_I, MODULE_ENVOY_QUARTERS_I, MODULE_PASSENGER_CABIN_I, MODULE_MICRO_REFINERY_I
    MODULE_ORE_REFINERY_I, MODULE_FUEL_REFINERY_I, MODULE_SCIENCE_LAB_I
    MODULE_JUMP_DRIVE_I, MODULE_JUMP_DRIVE_II, MODULE_JUMP_DRIVE_III
    MODULE_WARP_DRIVE_I, MODULE_WARP_DRIVE_II, MODULE_WARP_DRIVE_III
    MODULE_SHIELD_GENERATOR_I, MODULE_SHIELD_GENERATOR_II, MOUNT_GAS_SIPHON_I, MOUNT_GAS_SIPHON_II, MOUNT_GAS_SIPHON_III
    MOUNT_SURVEYOR_I, MOUNT_SURVEYOR_II, MOUNT_SURVEYOR_III
    MOUNT_SENSOR_ARRAY_I, MOUNT_SENSOR_ARRAY_II, MOUNT_SENSOR_ARRAY_III
    MOUNT_MINING_LASER_I, MOUNT_MINING_LASER_II, MOUNT_MINING_LASER_III
    MOUNT_LASER_CANNON_I, MOUNT_MISSILE_LAUNCHER_I, MOUNT_TURRET_I
    
- Use the following query to retrieve the imports, exports and exchange data associated with a Marketplace
    - `GET - https://api.spacetraders.io/v2/systems/{systemSymbol}/waypoints/{waypointSymbol}/market`
    - Request parameter
        
        ```json
        {
        	"systemSymbol": "Type string",
        	"waypointSymbol": "Type string"
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "symbol": "string",
            "exports": [
              {
                "symbol": "PRECIOUS_STONES. The entry here can be any item from 
        										the full list of goods that can be imported, exported 
        										or traded",
                "name": "string",
                "description": "string"
              }
            ],
            "imports": [
              {
                "symbol": "PRECIOUS_STONES. The entry here can be any item from 
        										the full list of goods that can be imported, exported 
        										or traded",
                "name": "string",
                "description": "string"
              }
            ],
            "exchange": [
              {
                "symbol": "PRECIOUS_STONES. The entry here can be any item from 
        										the full list of goods that can be imported, exported 
        										or traded",
                "name": "string",
                "description": "string"
              }
            ],
            "transactions": [
              {
                "waypointSymbol": "string",
                "shipSymbol": "string",
                "tradeSymbol": "string",
                "type": "PURCHASE",
                "units": 0,
                "pricePerUnit": 0,
                "totalPrice": 0,
                "timestamp": "2019-08-24T14:15:22Z"
              }
            ],
            "tradeGoods": [
              {
                "symbol": "string",
                "tradeVolume": 1,
                "supply": "SCARCE. A rough estimate of the total supply of this 
        									good in the marketplace. Can be one of the following
        									values - SCARCE, LIMITED, MODERATE, ABUNDANT",
                "purchasePrice": 0,
                "sellPrice": 0
              }
            ]
          }
        }
        ```
        

### ### Commodity goods

- Fuel is the most common exchange good listed across markets. Fuel is consumed by agents to travel between waypoints, which means there is always a natural demand for the good.
- The price of fuel can skyrocket at popular locations with high demand, but where players aren't providing enough supply.

### ### Growing markets

- Smaller civilizations typically start with a limited number of basic goods in their market. As agents trade and exchange goods, the market will grow and trigger the introduction of new types of goods.
- Larger civilizations will typically export higher technology and more advanced goods, but in turn have a higher demand for commodity goods that must be met by agents. Failing to meet the demand for commodity goods can cause the market to collapse.
- Maintaining a healthy market is a key part of maximizing profitability for agents. Later-stage markets tend to offer much high margins on goods, but they are also more competitive and require more attention to maintain.

### ### Sell Cargo

- You can sell any of the cargo acquired through your previous mining contracts in a market that trades this cargo. You should confirm the list of goods traded before attempting to sell.
- It’s important to remember that the ship must be docked in a waypoint that has the **`Marketplace`** trait and trades the cargo in order to view the transactions and prices.
- After confirmation, use the following request to sell cargo
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/sell`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Request body
        
        ```json
        {
        	"symbol": "Type string. For example - PRECIOUS_STONES",
        	"units": Type integer
        }
        ```
        
    - Response body
        
        ```json
        {
          "data": {
            "agent": {
              "accountId": "string",
              "symbol": "string",
              "headquarters": "string",
              "credits": 0,
              "startingFaction": "string",
              "shipCount": 0
            },
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            },
            "transaction": {
              "waypointSymbol": "string",
              "shipSymbol": "string",
              "tradeSymbol": "string",
              "type": "PURCHASE",
              "units": 0,
              "pricePerUnit": 0,
              "totalPrice": 0,
              "timestamp": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        

### ### Purchase Cargo

- The ship must be docked in a waypoint that has **`Marketplace`** trait, and the market must be selling a good to be able to purchase it.
- The maximum amount of units of a good that can be purchased in each transaction are denoted by the **`tradeVolume`** value of the good, which can be viewed by using the Get Market action.
- Use the following request to purchase cargo
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/purchase`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Request body
        
        ```json
        {
        	"symbol": "Type string. For example - PRECIOUS STONES",
        	units""
        }
        ```
        
    - Response body
        
        ```json
        {
          "data": {
            "agent": {
              "accountId": "string",
              "symbol": "string",
              "headquarters": "string",
              "credits": 0,
              "startingFaction": "string",
              "shipCount": 0
            },
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            },
            "transaction": {
              "waypointSymbol": "string",
              "shipSymbol": "string",
              "tradeSymbol": "string",
              "type": "PURCHASE",
              "units": 0,
              "pricePerUnit": 0,
              "totalPrice": 0,
              "timestamp": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        
- Purchased goods are then added to the ship's cargo hold.

### ### Transfer Cargo

- You can also transfer cargo between ships.
- The receiving ship must be in the same waypoint as the transferring ship, and it must able to hold the additional cargo after the transfer is complete.
- Both ships also must be in the same state to transfer cargo, i.e. either both are docked or both are orbiting.
- Use the following request to transfer cargo
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/transfer`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Request body
        
        ```json
        {
          "tradeSymbol": "Type string. For example - PRECIOUS_STONES",
          "units": Type integer,
          "shipSymbol": "Type string. Ship to which cargo will be transfered"
        }
        ```
        
    - Response body
        
        ```json
        {
          "data": {
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            }
          }
        }
        ```
        
- The response body's cargo shows the cargo of the transferring ship after the transfer is complete.

### ### Get Mounts

- To get the list of mounts installed in a ship, use the following request
    - `GET - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/mounts`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Response body
        
        ```json
        {
          "data": [
            {
              "symbol": "MOUNT_GAS_SIPHON_I",
              "name": "string",
              "description": "string",
              "strength": 0,
              "deposits": [
                "QUARTZ_SAND"
              ],
              "requirements": {
                "power": 0,
                "crew": 0,
                "slots": 0
              }
            }
          ]
        }
        ```
        

### ### Install Mount

- In order to install a mount, the ship must be docked and located in a waypoint that has a **`Shipyard`** trait.
- The ship also must have the mount to install in its cargo hold.
- Use the following request to install a specific mount on a ship
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/mounts/install`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Request body
        
        ```json
        {
          "symbol": "Type string. Symbol of the mount to be installed"
        }
        ```
        
    - Response body
        
        ```json
        {
          "data": {
            "agent": {
              "accountId": "string",
              "symbol": "string",
              "headquarters": "string",
              "credits": 0,
              "startingFaction": "string",
              "shipCount": 0
            },
            "mounts": [
              {
                "symbol": "MOUNT_GAS_SIPHON_I",
                "name": "string",
                "description": "string",
                "strength": 0,
                "deposits": [
                  "QUARTZ_SAND"
                ],
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              }
            ],
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            },
            "transaction": {
              "waypointSymbol": "string",
              "shipSymbol": "string",
              "tradeSymbol": "string",
              "totalPrice": 0,
              "timestamp": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        
- An installation fee will be deduced by the Shipyard for installing the mount on the ship.****

### ### Remove Mount

- Use the following request to remove a specific mount from a ship
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/remove`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string"
        }
        ```
        
    - Request body
        
        ```json
        {
          "symbol": "Type string. Symbol of the mount to be removed"
        }
        ```
        
    - Response body
        
        ```json
        {
          "data": {
            "agent": {
              "accountId": "string",
              "symbol": "string",
              "headquarters": "string",
              "credits": 0,
              "startingFaction": "string",
              "shipCount": 0
            },
            "mounts": [
              {
                "symbol": "MOUNT_GAS_SIPHON_I",
                "name": "string",
                "description": "string",
                "strength": 0,
                "deposits": [
                  "QUARTZ_SAND"
                ],
                "requirements": {
                  "power": 0,
                  "crew": 0,
                  "slots": 0
                }
              }
            ],
            "cargo": {
              "capacity": 0,
              "units": 0,
              "inventory": [
                {
                  "symbol": "string",
                  "name": "string",
                  "description": "string",
                  "units": 1
                }
              ]
            },
            "transaction": {
              "waypointSymbol": "string",
              "shipSymbol": "string",
              "tradeSymbol": "string",
              "totalPrice": 0,
              "timestamp": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```
        
- The ship must be docked in a waypoint that has the **`Shipyard`** trait, and must have the desired mount that it wish to remove installed.
- A removal fee will be deducted by the Shipyard for installing the mount on the ship.****

### ### Negotiate Contract

- Agents can negotiate a new contract with the headquarters, if required.
- In order to negotiate a new contract, an agent must not have ongoing or offered contracts over the allowed maximum amount. Currently the maximum contracts an agent can have at a time is 1.
- Once a contract is negotiated, it is added to the list of contracts offered to the agent, which the agent can then accept.
- The ship must be present at a faction's HQ waypoint to negotiate a contract with that faction.
- Use the following request to negotiate a contract
    - `POST - https://api.spacetraders.io/v2/my/ships/{shipSymbol}/negotiate/contract`
    - Request parameter
        
        ```json
        {
        	"shipSymbol": "Type string."
        }
        ```
        
    - Response example
        
        ```json
        {
          "data": {
            "contract": {
              "id": "string",
              "factionSymbol": "string",
              "type": "PROCUREMENT",
              "terms": {
                "deadline": "2019-08-24T14:15:22Z",
                "payment": {
                  "onAccepted": 0,
                  "onFulfilled": 0
                },
                "deliver": [
                  {
                    "tradeSymbol": "string",
                    "destinationSymbol": "string",
                    "unitsRequired": 0,
                    "unitsFulfilled": 0
                  }
                ]
              },
              "accepted": false,
              "fulfilled": false,
              "expiration": "2019-08-24T14:15:22Z",
              "deadlineToAccept": "2019-08-24T14:15:22Z"
            }
          }
        }
        ```