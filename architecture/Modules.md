# Conceptual Modules

**Pseudocode/design only.** These are responsibilities, not an approved number of published mods. No Factorio entrypoint or manifest exists yet.

| Area | Owns | Talks to |
| --- | --- | --- |
| Core | Player/company identities, money, shared persistence | All systems |
| Administration | Validated rules and Play Types | Core and permission checks |
| Labor | Agreements, shifts, payroll | Government and companies |
| Contracts | Offers, acceptance, fulfillment, settlement | Stock, money, labor, science rewards |
| Trade/storage | Physical stock references and reservations | Contracts and freight |
| Transit/freight | Passenger trips and cargo custody | Sites, stock, contracts |
| Industry/research | Licenses, recipes, conversion, corporate progress | Rewards and milestones |
| Station/world | Hub layout, spawn, destination/site definitions | Transit and permissions |
| Compatibility | Explicit adapters for selected mods | Industry/world definitions |

Potential records: Player, Corporation, Employment, Shift, Contract, Warehouse, StockReservation, Shipment, IndustryLicense, ResearchMilestone, TransitBooking, ServerProfile. Their exact fields are deliberately unsettled.

Boundaries to preserve: one authority for money and settlement; actual inventory for goods; corporate research separate from personal employment; supplied equipment separate from ownership rights.

Before code, experiment with force isolation, no-hand-crafting enforcement, platform cargo access, and destination-safe spawning. See [Flow Sketches](Flow%20Sketches.md) and [Roadmap](../planning/Roadmap.md).

For multi-server exploration, see [Cluster and Force Scaling](Cluster%20and%20Force%20Scaling.md). Global company IDs must not be identical to local force indices.
