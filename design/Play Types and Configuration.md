# Play Types and Configuration

**Direction:** one customizable framework supports cooperative commerce, regulated rivalry, and corporate warfare. Host/admin GUI and configuration-based setup are desired. Space Age and no hand crafting remain core requirements.

Preset names and details are proposals:

| Preset | Conflict policy |
| --- | --- |
| Cooperative Commerce | Economic competition and protected corporate assets |
| Regulated Rivalry | Declared/accepted wars and configurable protected areas/windows |
| Corporate Warfare | Broad conflict with explicit raiding/capture rules |
| Custom | Supported host-selected combination |

Damage, theft, dismantling, territory, and automation access need separate enforcement. A menu toggle does not prove protection works.

Proposed admin sections: government, corporations, labor, markets/freight, research, conflict, profiles. Players can view the active rules; only authorized admins edit them. In-game political office does not automatically grant server privileges.

One versioned configuration model should serve GUI, saved profiles, and headless commands. Explain which changes apply immediately, to new agreements, after restart, or only to a new world. Preserve agreed contract terms; preview changes before applying them. Do not silently reset a customized save on restart.

Import profiles as validated data, not executable Lua. Runtime access to arbitrary external configuration files is not assumed; a later external loader is an option. Content/dependency selection remains separate from runtime conflict policy.

See [Modules](../architecture/Modules.md), [Decisions](../planning/Decisions.md), and [Dependencies](../research/Dependencies%20and%20Sources.md).
