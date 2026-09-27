# Dependencies and Sources

Research snapshot: 2026-09-24. Not reverified during the 2026-09-26 repository setup. No third-party mod has been installed or selected. Confirm versions and asset permissions before implementation.


Space Age is required. No third-party dependencies are selected yet. Pin a tested Factorio version and matching mod versions before implementation; latest online API documentation may differ from the server version.

- Space Age is the selected foundation for planets and moving cargo platforms. The expansion already supplies core interplanetary logistics; corporate trading and custody still need custom design. https://www.factorio.com/space-age/content
- Factorio forces expose separate technologies and recipes, supporting the corporation-per-force proposal. https://lua-api.factorio.com/latest/classes/LuaForce.html
- Players have a writable force, supporting an employment prototype but not proving the full permission model. https://lua-api.factorio.com/latest/classes/LuaPlayer.html
- Space logistics are version-sensitive; inspect the chosen version before specifying landing pads and routing. https://www.factorio.com/blog/post/fff-441
- PlanetsLib is a candidate planet-development helper; evaluate its exact supported features and compatibility. https://mods.factorio.com/mod/PlanetsLib
- Moshine is an example of an optional specialized industrial world, not a selected dependency or a ready-made trade center. https://mods.factorio.com/mod/Moshine
- Black Market 2 is a reference/candidate for automated trading. Its suitability for separate corporate wallets, player orders, and physical freight is unverified. https://mods.factorio.com/mod/BlackMarket2
- Space Exploration is no longer an alternative foundation under consideration now that Space Age is confirmed. Do not import its station systems as though they were a standalone compatible dependency. https://mods.factorio.com/mod/space-exploration

### Station and destination shortlist — researched 2026-09-24

These are candidates from author descriptions, not a verified modpack or completed visual asset audit.

| Candidate | Potential use | Evidence and qualification |
| --- | --- | --- |
| Dectorio | Concourse floors, walls, barriers, lighting, district markings | Author lists painted concrete, walls, bollards, and colored lamps. Portal lists support through 2.0; do not assume 2.1 support. https://mods.factorio.com/mod/Dectorio |
| Tycoon | Civic/city buildings and inspiration for a populated station | Adds cities, buildings, passenger transport, and its own progression/economy. Portal lists 1.1–2.0 and reports multiplayer desync limitations. Evaluate assets and integration separately before adopting its simulation. https://mods.factorio.com/mod/tycoon |
| Asphalt Roads Patched | Service roads and cargo-zone markings | Author describes pavement, lane markings, and hazard areas, with 2.0/2.1 support. https://mods.factorio.com/mod/AsphaltRoadsPatched |
| can leave hub | Feasibility reference for walking on an actual platform | Described as exiting the hub with Enter, for 2.0. Does not itself provide a station city, spawn system, or public transit. https://mods.factorio.com/mod/leave-hub |
| Any Planet Start | Reference/helper for alternate-start technology and initialization | Supports custom starts, but restructures research around a selected start; per-corporation destination packages need our own integration assessment. https://mods.factorio.com/mod/any-planet-start |
| Moshine | Optional computing/AI industrial destination | Author describes an AI-focused hot planet and specialized production. Translate that content into tested concessions and supply jobs. https://mods.factorio.com/mod/Moshine |
| Maraxsis | Optional underwater industrial destination | Author describes underwater exploration and production; safe arrival and equipment must be designed for its environment. https://mods.factorio.com/mod/maraxsis |

No ready-made intergalactic civic station asset pack was verified in this research. Likely art direction combines existing industrial/decorative assets with purpose-made government offices, exchange terminals, passenger gates, and station exterior art. Inspect asset-specific attribution and licenses before extraction or redistribution; portal licensing alone is not a complete asset audit.

Inspect mod licenses before reusing code or assets. Depending on a mod is distinct from copying its assets into this collection.


Return to [Home](../Home.md) or [Station Design](../design/Station%20Planets%20and%20Transit.md).
