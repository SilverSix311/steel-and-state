---
status: historical
updated: 2026-09-26
tags: [archive]
---

> Preserved from the original DESIGN.md. This snapshot contains superseded proposals, especially stock-backed science conversion. Current direction: [Research and Science](../design/Research%20and%20Science.md).

# Steel and State — planning draft

Status: brainstorming, not an approved implementation specification.
Created: 2026-09-24. Source: the user's initial project brief in this task.
No gameplay code or dependency installation has been started.

## Confirmed vision

- A multiplayer collection of Factorio mods built around government and corporations.
- Players clock in at a government building, work a public factory, and earn time-based wages.
- Players use savings to create a named corporation with its own faction and starter equipment.
- Each corporation has independent scientific research.
- Players can work paid shifts for other corporations and help build those businesses.
- Corporations set prices and sell resources, with delivery or customer pickup.
- A central trade hub provides a place to trade resources for currency, potentially at discounted prices. The latest preferred direction is a huge walkable space station where players first arrive, rather than spawning on Nauvis.
- Space shipping is a potential specialist industry, with engine, hull, power, and defense progression.
- Purchasable starter contracts provide industry entry and science access; additional contracts diversify an umbrella corporation.
- Existing mods, planets, assets, and functionality may be dependencies.
- Support configurable **Steel and State Play Types**, including cooperative competition, regulated PvP, and open corporate warfare, through a shared framework rather than choosing only one server style.
- Provide a host/admin configuration GUI, with configuration-based setup also desired. Confirmed by the user's follow-up on 2026-09-24; exact presets and controls below are proposals.
- **Space Age is required.** Additional planetary mods are welcome; select a tested collection rather than assuming universal compatibility.
- Players obtain planetary contracts or employment at the central station, then take government transport to the assigned destination.
- Player labor and contracts should expand government-owned mining, production, and shipping infrastructure. Its real resource supply supports further government and corporate growth.
- **No hand crafting.** Factories manufacture all crafted goods, including buildings. This is a core rule across Play Types, not merely a default that a preset silently overrides.
- Government factory-building contracts issue the buildings needed for the job.
- Space warehousing is a contract opportunity. Stock remains physically located in planetary warehouses or government-owned space-station warehouses.
- Government taxis and physical warehouse stock are confirmed directions, not just candidate ideas.
- Defense contracting is an industry: corporations can buy protective equipment for asteroid-exposed shipping and hostile planetary environments; specialist corporations develop, manufacture, and supply that equipment.
- Latest research proposal (2026-09-24): government trade awards a physical **Government Science Pack** whose only use is conversion into conventional science packs. Conversion costs vary by output type and government conversion is deliberately slower than buying available packs or producing them commercially. Corporate labs consume the resulting conventional packs; milestones and prototype trials remain part of progression. Ratios, throughput, and supply rules are not finalized.
- Science-pack manufacturing should be a corporate industry contract that any corporation can add. Producers import ingredients or acquire additional industry contracts to manufacture their own inputs; science is intended to be a substantial economic investment.
- The user proposes generally available infinite research, including military improvements using science bought at the trade center. Whether progress requires supplied labs or accrues passively over time is not yet resolved.

The Space Age requirement and public/private expansion direction were confirmed on 2026-09-24. The walkable station is the user's preferred concept; its technical implementation and the contract rules below remain proposals.

## Steel and State Play Types

Play Types are named presets over shared rules. Hosts can start with a preset, customize individual rules, and save the result as a custom profile. Economy, employment, corporation ownership, and independent research remain the common foundation.

Proposed presets:

| Play Type | Default conflict rules |
| --- | --- |
| Cooperative Commerce | Economic competition with corporate combat and unauthorized asset access disabled |
| Regulated Rivalry | Declared or mutually accepted wars, configurable combat windows and protected zones |
| Corporate Warfare | Broad corporate conflict, with explicit settings for raiding, asset capture, and public-port protection |
| Custom | Host-defined supported combination of rules |

Combat permissions, inventory access, building/mining permissions, and territory rules must be separate policies: disabling damage alone does not protect a factory from theft or dismantling. Enforcement feasibility is an early prototype task, including indirect damage and automation interactions.

Keep content selection separate from conflict presets. Space Age is required for every Play Type. Optional planetary packs are dependency profiles, not live PvP toggles. Base-game-only and alternative overhaul foundations are no longer part of the proposed scope. Support for every mod combination is not promised.

### Host/admin interface

Proposed configuration screen: **Steel and State → Server Setup**.

- Overview: selected Play Type, installed capabilities, and current rule summary.
- Government: public employment, wages, procurement budgets, taxes, and public services.
- Corporations: founding costs, starter packages, membership limits, land access, and permissions.
- Labor: payroll interval, inactivity grace period, and shift rules.
- Markets and Freight: fees, order limits, delivery deadlines, and cargo-loss policy.
- Research: license costs and specialization progression; preserve independent corporate research by default.
- Conflict: diplomacy, declarations, damage, looting, capture, safe zones, and offline protection.
- Profiles: preview changes, apply, save a named profile, and import/export settings.

Ordinary players can view active server rules. Only authorized server administrators can change them; an elected in-game government does not automatically receive server-admin authority. Validate authorization for every mutation, including commands and imports.

Use understandable labels and brief explanations. Show whether a setting applies immediately, to new agreements only, after a restart, or only to a new world. Unsupported options should explain the missing dependency. Previewing a preset must not apply it or overwrite custom changes automatically.

### Configuration behavior

- One versioned configuration schema and validation path serves presets, GUI changes, and headless administration.
- Persist the effective configuration with the save. Initialize new saves from defaults plus the chosen preset and explicit overrides; later GUI/command changes update that saved configuration. Restarting must not silently reapply defaults.
- Provide profile import/export as validated data, never executable Lua supplied through the GUI. Headless servers should have an equivalent admin command/RCON path. A human-readable file workflow can use an external loader; do not assume a runtime mod can directly read arbitrary server files.
- Use Factorio startup settings where prototype/content changes require them, runtime-global settings or saved mod configuration for supported shared rules, and per-user settings for display preferences. Avoid two competing writable copies of the same rule.
- Existing wages, purchases, and freight agreements retain their accepted terms unless an explicit migration is designed. Price changes normally affect new agreements.
- Switching conflict modes requires defined handling of active wars and advance player notification. A return to peace must not delete cargo, balances, or company assets.
- Keep a configuration change history with actor, time, and changed values. Validate the entire proposed change before applying any part.
- Build and test one preset first while keeping the rule framework extensible. Support for additional presets means implementing and verifying their enforcement, not merely adding menu entries.

Technical reference: Factorio exposes startup, runtime-global, and runtime-per-user setting scopes: https://lua-api.factorio.com/latest/classes/LuaModSettingPrototype.html

## Proposed core loop

Join server at the central station → choose a public assignment or corporate job → take government transport to the worksite → earn personal savings → buy a corporate charter, industry contract, and planetary concession → receive equipment and transport → establish production → sell goods → hire workers → research and diversify → expand into interplanetary trade.

Public employment should remain available as a recovery path. Players should also be able to remain employees if that is their preferred role.

## Proposed organization and employment model

Maintain distinct records for a player, corporation, employment agreement, and active shift.

- Players own personal wallets. Corporations own treasuries, assets, licenses, and research.
- A corporation maps to a Factorio force. Government has its own force.
- Corporate ownership and membership persist independently of the player's current employer.
- A player has at most one active paid shift. Disconnecting stops wage accrual.
- Prototype temporarily assigning the player to the employer's force while working. This is a candidate, not a settled permission design.
- Before adopting force switching, test inventories, personal logistics, crafting, research bonuses, vehicles, remote interactions, respawning, and restoring membership after disconnects.
- Worker, manager, and owner permissions need explicit enforcement; sharing a force alone does not establish corporate access controls.
- Proposed initial wages use online game time, with a grace period for inactivity. Planning and supervising automation must count as legitimate work; do not pay only for clicks or manual crafting.
- Corporate shifts reserve a funded payroll allowance. Notify and stop new paid time if funds run out; never silently promise unfunded wages.
- Government shifts use a configurable public budget or explicit subsidy. The choice of where new currency enters the economy must remain visible.

## Industry contracts and research

### Factory-only production

Confirmed on 2026-09-24: players cannot craft items by hand. Placing supplied buildings, configuring machines, and arranging production are still the player's construction work. This instruction does not itself ban manual mining, inventory transfers, or repair; those are separate design decisions.

Government construction contracts issue a complete, site-appropriate equipment manifest: production machines, power equipment, logistics parts, and any fuel or initial materials needed to start the specified process. The exact manifest is a proposal; supplying buildings is confirmed. Corporate starter kits must likewise support production without requiring an initial hand-crafted assembler, power pole, belt, or intermediate component.

Issued equipment is drawn from reserved physical public stock and collected at an authorized depot or delivered to the worksite. The starting world may contain a finite reserve and established production facilities; replenishment comes from factories or purchased factory output. Accepting a contract must not create unlimited new equipment. If supplies are unavailable, expose a supply dependency or delay the project before dispatching a worker.

Public equipment remains assigned to the government project; unused supplies return to public stock. Track issue, placement, recovery, and return, including contract cancellation. Bulk construction equipment uses freight or pre-positioned site stock; the taxi's equipment allowance must not become unlimited free freight.

For every supported planet and industry, audit the chain from delivered equipment to sustainable output, including replacement machinery and consumables. Every required crafted item needs an accessible machine recipe and an available manufacturing service or starter machine. Adapt hand-crafting-only mod recipes if encountered. Recovery from machine destruction must use replacement stock, procurement, or public assistance while preserving the no-hand-crafting rule.

Implementation investigation: Factorio exposes character crafting categories independently of machine crafting categories, and force-level hand-crafting controls. Select and test enforcement for the pinned game version, modded characters, newly created corporate forces, force changes, and save/load. Keep machine recipes enabled. This is a feasibility direction, not implemented code. Reference: https://lua-api.factorio.com/latest/prototypes/CharacterPrototype.html

### Licenses and specialization

Proposed terminology: a charter creates a corporation; an industry license/contract opens a specialization; a freight contract describes a delivery job.

Buying an industry contract grants a limited entry package and access to its research branch. The proposed hybrid below uses labs for higher tiers and selected practical milestones; the research method is still open for discussion.

Possible starting industries:

| Industry | Primary business | Research direction |
| --- | --- | --- |
| Mining and metallurgy | Ore, plates, steel | Extraction and smelting efficiency |
| Manufacturing | Circuits, machines, components | Advanced products and throughput |
| Chemicals and energy | Fuel, plastics, batteries | Processing and energy systems |
| Freight | Pickup and delivery services | Capacity, propulsion, power, protection |
| Defense | Weapons, ammunition, protective equipment, installation and resupply | Specialized defensive products, manufacturing methods, and test programs |
| Science manufacturing | Physical science packs sold to government or other corporations | Pack recipes, advanced science inputs, and manufacturing efficiency |

Divisions under one umbrella corporation share its force and research. Truly independent subsidiary research would need separate forces or a custom research system and is outside the first proposal.

All corporations need a baseline of construction, power, and basic logistics. Specialization should encourage trade without making a server unplayable when one supplier is absent. Additional licenses allow diversification; price and progression should make buying everything immediately unattractive.

Keep starter-kit entitlements in persistent records and define dissolution/recreation rules to prevent repeated free-kit farming.

### Research model comparison — background to current proposal

| Model | Strength | Design cost or risk |
| --- | --- | --- |
| Conventional labs and science packs | Physical investment in R&D, established Factorio production loop | Every corporation may feel forced to rebuild the same broad science factory unless science inputs are tradable and branches are specialized |
| Production points alone | Progress follows operating a business | Encourages surplus grinding, favors large established factories, and poorly represents service companies; output volume is not automatically meaningful innovation |
| Purchased technology | Simple and accessible to specialized firms | Cash can bypass too much factory gameplay and make corporations' progress feel interchangeable |
| Dedicated research projects | Prototypes, test batches, and useful commissioning goals fit each industry | More custom content, objective tracking, and UI work |
| Labs plus selected practical milestones | Combines physical science with industrial identity | Needs clear gates, limited administrative friction, and careful per-corporation attribution |

Current proposal: industry license → useful production sold to government for science-pack rewards, or direct science purchases → corporate lab research → occasional test or operational milestone for a major new tier. This updates the earlier lab/milestone recommendation with the user's trade-funded science idea; it does not introduce a universal spendable production-points currency. See the detailed model below. Avoid requiring a volume grind before every small research upgrade.

Keep research concepts distinct:

- License: eligibility to develop an industry. Does not instantly complete its advanced technologies.
- Manufacturing knowledge: unlocks recipes or processes for the corporation that owns the research.
- Finished product: a tradable machine, component, or ammunition item that another corporation can use without researching how to manufacture it.
- Operational improvements: benefits tied to the corporation operating the machinery. These require their own explicit rules and do not automatically accompany a sale.

Baseline construction, logistics, and access to purchased equipment must remain practical for all corporations. Safety equipment should be usable by buyers without forcing them into defense manufacturing. Essential defense capability should have a public fallback supplier when no defense corporation is active.

Proposed research inputs: retain familiar science packs initially, with one or two specialized physical test inputs where they establish a meaningful industry identity. Corporations may buy science packs and test inputs, so a mining company does not need to become a vertically integrated electronics company to research better drills. Labs and completed research belong to the researching corporation; purchasing supplies does not transfer the supplier's technology.

Science manufacturing and specialist science suppliers are now part of the intended industry scope. Outsourced technology transfer remains a separate possible feature needing explicit ownership/licensing rules; it is not implied by ordinary science-pack sales.

| Specialization | Example R&D activity | Commercial result |
| --- | --- | --- |
| Mining/metallurgy | Test material batches and research extraction/processing | Advanced mining or processing products; efficient resource supply |
| Manufacturing | Evaluate components and prototype production processes | New component recipes and specialized machines |
| Energy/chemicals | Test fuel, power, or chemical processes | Better fuel products, generators, or processing equipment |
| Freight | Lab engineering and verified trial deliveries | Carrier operating capabilities and, if separately licensed, new transport equipment |
| Defense | Manufacture prototype equipment and consume test ammunition in controlled tests | New ammunition, turret, or protection products that customers can purchase |
| Warehousing | Equipment development and verified handling trials | Storage/handling products and operator capabilities; renting space alone need not require an artificial research grind |

These are gameplay examples, not promises that each equipment category already exists in the selected mods. A freight firm can buy engines and defenses from manufacturers while researching its own operating capabilities. Buying a second industry license enables in-house manufacturing under the same umbrella corporation.

Production/service milestones should be discrete, relevant, and credited to the responsible corporation. Distinguish government work from private production; clocking in must not transfer research or grant personal unlocks. Prefer verified test consumption or completed service objectives over raw sales value, kill counts, repeated cargo transfers, or recycled/recrafted output that can be farmed. Permit qualifying trials or public jobs when private market demand is low.

For the first prototype, test government trade rewards feeding one corporate lab branch, alongside a science supplier and one practical milestone. Example defense path: license and basic equipment recipes → trade-funded R&D → verified prototype test → advanced product recipe. Display the exact remaining inputs and objective in one research view.

Technical grounding: Factorio technologies expose science costs, research triggers, and prerequisite technologies. A combined gate can be represented as a milestone prerequisite followed by a lab technology; custom contract objectives need their own verified logic. Corporate research can live on separate forces. Do not assume one technology accepts both native trigger and science-cost mechanisms at once. References: https://lua-api.factorio.com/latest/prototypes/TechnologyPrototype.html and https://lua-api.factorio.com/latest/classes/LuaForce.html

### Government trade rewards and the science economy

Latest user proposal: different products award different quantities of Government Science Packs per stack when sold to the central government trade service. These packs cannot be used directly in labs; their only use is conversion into conventional science packs at output-specific ratios. The example ratios (1 red, 3 green, 1000 yellow) were explicitly illustrative, not balance targets. Milestones and prototype trials remain part of major advancement.

Recommended interpretation: corporations earn Government Science Packs, choose conversion outputs, physically receive those outputs, and consume them in corporate labs over game time. Corporations may instead buy conventional packs or manufacture them under a science-industry contract. Keep earning/conversion, lab research, and milestone verification separate. Accepting a sale or conversion should not instantly grant technology.

Proposed government offers specify accepted item, quality, fixed batch quantity, remaining demand, cash payment if any, pack type and quantity, settlement location, and expiry. Display rewards per stack for convenience, but calculate from explicit item counts so inventory splitting and stack-size-changing mods do not alter payouts. Set rates per product using production inputs, processing burden, scarcity, and government demand; stack count alone does not measure value. Numeric rates remain unbalanced and unset.

Reserve the reward packs and procurement budget when accepting an order. On verified delivery, move the purchased goods into government stock and transfer reserved packs to the seller's allocation at the named warehouse, or hand them over at a physical collection point. A hired carrier receives its freight fee, not automatically the seller's science reward. Research ownership stays corporate; receiving or transporting packs does not make an employee personally own the company's unlocks.

Government Science Packs act as physical research entitlements, not literal compressed versions of every ingredient. Budget their issuance against useful procurement. The mechanism for supplying conversion outputs remains open: (a) recommended, reserve conventional packs from public manufacturing or government purchases from science corporations and release them through a slow conversion service; or (b) synthesize conventional packs from the entitlement alone in a special converter. The latter is simpler but creates a parallel supply source that bypasses ingredient chains and planetary materials. Do not silently describe synthesis as physical procurement.

Under the recommended stock-backed model, a finite initial reserve and public entry-level science production allow play before private suppliers exist. Conversion acceptance reserves both required tokens and output stock; quote completion time before commitment. If stock is absent, show the offer as unavailable or explicitly waiting for procurement, without promising a fixed delivery time. The public conversion interface can still present a simple Government Science Pack → chosen science pack exchange.

### Government science conversion and corporate R&D

Proposed flow: accepted government delivery → Government Science Packs → government conversion order → conventional packs in a named warehouse → freight/collection → corporate labs → corporate technology.

Three routes feed the same corporate R&D department:

| Route | Benefit | Cost or constraint |
| --- | --- | --- |
| Government conversion | Predictable research access funded by useful public trade | Output-specific token cost and deliberately limited throughput |
| Buy conventional packs | Access to available science without waiting for public conversion | Supplier price, stock, and transport time |
| Licensed science production | Control over supply and capacity; surplus can be sold | Facilities, ingredients, power, and logistics |

Slow conversion must mean a real throughput limit, not just a long recipe in infinitely duplicable machines. Candidate controls are a corporate conversion-service allocation, a bounded number of public processing slots, or a limited R&D conversion facility. Prefer predictable per-corporation throughput with visible completion times over an unrestricted server-wide queue that one large firm can monopolize. Account for additional corporations created solely to multiply allowances; exact policy remains open. No clock-based jobs requiring the game to simulate while the server is stopped are assumed.

Normalize conversion tiers by expected material/processing burden, progression, and throughput; pack color alone is not a reliable cost measure. Gate advanced outputs behind appropriate milestones or access rules so early raw-material sales cannot bypass planetary progression. Explain prerequisites in the conversion screen. All stated rates and durations remain placeholders until a balance model exists.

Conventional packs feed their corresponding technology recipes. Tokens never substitute directly for missing colored ingredients. The R&D view should show conversion orders, incoming pack shipments, lab stock, selected research, and outstanding trials.

Government conversion is intended as baseline access; industrial production should be able to scale beyond it. Buying science is faster only when supply and transport are available, not guaranteed instant delivery. Direct manufacture retains normal ingredient recipes and does not require Government Science Packs, preserving science as a business serving private customers as well as government demand.

### Science reward balance

Cash plus science, science-only exchanges, and optional reward selections are possible, but must be deliberately budgeted. Include the pack reward's resale value when balancing the total payout. Avoid buying goods from the government and selling them straight back for a net gain in cash and science. Finite posted demand, coherent buy/sell spreads, and budgeted subsidy terms are needed; labeling a transaction a sale is not proof of new production.

Do not make the government the only attractive customer. Bound research subsidies to useful procurement. Private trade earns money that buys the same research packs, with viable private prices and supply. Public pack sales should not be an unlimited cheap source that makes private science manufacturing pointless. Science-pack procurement itself needs explicit reward terms to prevent recursive pack-for-pack multiplication.

### Science manufacturing as an industry

Any corporation may buy an additional science-manufacturing contract, subject to progression and price. Its license unlocks production eligibility and an initial recipe/machine package; it does not grant all industry technology simply because it can manufacture science.

Factories consume real ingredients delivered from suppliers or made under the corporation's other industry contracts. Packs are stored, shipped, sold, and consumed like other physical goods. Any corporation may use appropriate purchased packs in its research, provided it meets that technology's license/prerequisite rules.

Science producers earn sales revenue and procurement contracts. Their advantage is access to pack manufacturing, increasingly advanced pack recipes, and efficient production, not an automatic monopoly on completed research. Their own advanced manufacturing research also has a viable starting path through entry-level packs, public supplies, or imports.

Science cost comprises ingredients, production capacity, power, shipping, and lab consumption. Keep foundational science affordable enough to establish a viable business; advanced specialization and repeatable improvements can become substantial investments. Do not charge arbitrary high fees at every stage simply to make research expensive.

### Common repeatable research versus specialist technology

User proposal: everyone has access to general infinite research, potentially buying military science at the trade center for ongoing combat improvements.

Recommended interpretation: a common set of repeatable research branches available independently to every corporation, advancing while its labs are powered and supplied. Each corporation owns its levels; common availability does not mean a globally shared research level. Military operating improvements can be available without a defense-manufacturing license, while new defense product recipes remain in the specialist branch.

Factorio supports infinite technology levels. Select moderate benefits and rising pack costs so established corporations cannot cheaply accumulate overwhelming advantages. The relation between a purchased product's stats and the buyer's operating research must remain legible. Reference: https://lua-api.factorio.com/latest/prototypes/TechnologyPrototype.html

Passive time-only advancement remains an alternative requiring a separate decision about online/offline time, government force progression, and late joiners. It should not be silently treated as approved or substituted for the supplied-lab recommendation. A parallel general-research queue is also not assumed: decide whether common and specialist research share the normal force queue before adding custom parallel research.

### Defense contracting and transferable product value

Provide defense listings at the central exchange and let government/corporate buyers issue procurement, installation, maintenance, and resupply contracts. Goods remain factory-produced, physically warehoused, and shipped. Defense retains a purpose in peaceful Play Types through planetary hostiles and asteroids; corporate combat services remain subject to the chosen conflict rules.

Proposed offerings include platform defense packages, planetary perimeter packages, and recurring ammunition/spare-parts supply. Installation and maintenance have measurable acceptance terms; no assumption that a separate escort ship can protect another platform in native Space Age. Evaluate any escort mechanic independently.

Critical technical distinction: a seller's force-wide damage or firing-speed research does not become a property of an ordinary sold turret. Buyers operate under their own force's relevant modifiers. To make defense R&D produce transferable value, favor distinct product recipes/prototypes or supported item quality, with product stats documented separately from operator bonuses. Quality behavior and each modded product require testing. Avoid advertising a seller's research bonus as a portable item upgrade.

Open choices: how much progress uses labs versus milestones; whether specialized testing inputs are tradable; which operational bonuses remain force-wide; and whether a later research-services industry may transfer or license knowledge.

## Market and freight

Keep the order book distinct from physical cargo movement.

- A listing records seller, item, quality, quantity, unit price, pickup location, and fulfillment terms.
- Sellers deposit listed stock in a controlled trade depot. Buyers reserve payment in escrow, meaning funds held until the agreed handover.
- Pickup, seller delivery, and third-party freight are distinct fulfillment options.
- Orders move through explicit states: listed, reserved, in transit, delivered, settled; cancellation and failure rules release only the appropriate money and inventory.
- Freight jobs specify origin, destination, cargo, fee, deadline, and loss liability.
- Validate delivery at the agreed destination and pay the carrier once. Define who owns the cargo during transit and who can access it.
- Initial trading should handle solid items and one quality tier. Fluids, quality variation, and spoilage require explicit terms before expansion.
- A depot is a bounded cargo handoff point, not a way to teleport arbitrary stock between businesses.

An early freight business can use ground vehicles or trains. Space freight extends the same contract model after cross-force transport is proven.

## Space warehousing contracts

Confirmed direction: physical stock can be stored on planets and in government-owned station warehouses; players can take space-warehousing contracts.

Proposed contract types:

- Build or expand a government warehouse using issued buildings and materials.
- Operate receiving, storage, sorting, and dispatch under a service agreement.
- Lease an approved warehouse bay for a corporation's inventory.
- Stage goods for an upcoming public project or consolidate shipments for a carrier.

Separate ownership of the warehouse, ownership of the goods, and the operator's access rights. Storing corporate goods in a government building does not make them government property. Record stock by owner, warehouse, item/quality, available quantity, and reservations. Reservations reference actual inventory; the ledger never replaces it or creates a second copy.

Capacity is physical and finite. Define receiving permissions, authorized withdrawals, reserved loading space, handling compensation, and loss liability. A handling job should pay for verified service; repeated transfers of the same stock must not generate unlimited rewards. Rental rates, operating fees, spoilage handling, and service targets remain open proposals.

Warehouse location drives freight demand: stock in one planetary depot must actually travel before it is usable at a station warehouse or another worksite. First prototype: one government station warehouse with separate public and corporate allocations, an equipment issue manifest, and verified cargo receipt/dispatch.

## Central trade station and planetary deployment

Preferred player experience: a large, walkable public space station with government offices, corporate recruitment, exchange terminals, warehouses, passenger departures, and carrier services. The station is the shared starting location; Nauvis is one possible destination among the available planets.

Proposed districts:

- Arrivals and civic concourse: onboarding, map, public rules, and return transport.
- Government bureau: clock-in terminals, construction tenders, mining jobs, and public logistics assignments.
- Corporate exchange: company registration, industry licenses, planetary concessions, and recruitment.
- Market and warehouse district: player listings, public procurement, physical stock, and carrier dispatch.
- Passenger docks: destinations, departure status, fares, and assignment-funded tickets.
- Industrial service ring: public utilities and visible loading operations, with room for later station expansion.

Make the station feel large through districts and visible infrastructure while keeping first-use services close together. Long walks should not be a prerequisite for every routine transaction.

### Technical station proposal

Compare a dedicated station surface, potentially attached to a custom planet/space-map destination, against an actual Space Age platform with walking modifications. A dedicated surface with constructed decks, inaccessible space around them, and station artwork is the leading proposal for predictable pedestrian movement and civic buildings. This is an implementation hypothesis, not a tested integration.

Native platform travel is hub-centered; the `can leave hub` mod demonstrates one approach to exiting it, but does not establish suitability for a persistent multiplayer city. Test character movement, joining and respawning, building placement, ownership, docking, and all passenger transitions before choosing.

If a station surface is represented as a planet internally, the player-facing presentation can still be a space station. Cargo handoff between that surface and actual platforms remains an explicit integration task. Do not assume platform-to-station docking is automatic.

### Contracts and government taxis

Separate the industry license from the destination concession. A mining license specifies what the corporation can develop; a concession assigns where it can operate. Buying one planetary contract does not grant ownership of the entire planet by default.

Each destination offer should show the planet, assigned site, available resources, hazards, prerequisites, equipment provided, cost or wage, ownership terms, and outbound/return transport. A planetary adapter must define viable starter supplies, necessary recipes and technology, safe arrival, and an extraction route. Discovery access alone does not make a planet a viable start.

Proposed transport is a government passenger service, with sponsored trips for assignments and configurable public fares. Decide between real scheduled platforms and an initial scripted boarding/travel/arrival service through a prototype; preserve the feeling of riding to a destination. Display travel time and provide a return route so new players are not stranded.

Passenger transport should have explicit baggage limits and a separate allowance for issued contract equipment, so free commuting does not replace commercial bulk freight. Handle disconnects, cancelled assignments, full destinations, missing docks, and return trips without duplicating equipment or losing the passenger.

Government transport must serve new corporate forces without granting them the government's full research. Prototype the required access permissions and cross-force passenger handling.

## Government expansion and public procurement

Government is a persistent infrastructure owner and customer. Players help create its resource base, and that base supports economic participation and additional expansion.

Proposed loop: public contract → player builds or operates a public facility → output enters local government stock → freight delivers it where needed → government uses it for public infrastructure and starter supplies → new contracts create further demand.

| Contract or role | What the player does | Resulting ownership |
| --- | --- | --- |
| Public employee | Works a funded shift at an assigned government site | Government retains buildings and output |
| Government construction contractor | Builds a specified working mine, factory, depot, or utility | Accepted public assets belong to government |
| Government operator | Operates a public facility under defined production/service terms | Public assets stay public; compensation follows the agreement |
| Public warehouse contractor | Builds or operates a government space-station warehouse | Warehouse stays public; each stored lot retains its recorded owner |
| Public supplier or carrier | Sells goods or delivers government cargo | Purchased goods become public stock; carrier keeps its business |
| Corporate concession holder | Purchases rights to develop an assigned planetary site | Corporation owns its factory under the concession terms |

Public jobs pay the worker or contractor. Corporate charters, licenses, and concessions are purchases. Keep these transactions visibly distinct in the contract GUI.

Construction acceptance should check functional output or service availability over a defined interval, not just the presence of an easily removed building. Reserve the site, construction budget, issued equipment, and payout before dispatch. Define acceptance, asset ownership, failure, abandoned sites, and unused-material returns.

Track public inventories by location. Materials on Vulcanus are not automatically available at the central station: demand can generate freight jobs to move them. Public projects can progress from proposed to funded, supplied, under construction, and operational.

Use a bounded bootstrap reserve for the starting station and first assignments. Ongoing public stock should support starter equipment, replacement machinery, transport supplies, and expansion. A configurable emergency reserve/recovery service prevents a depleted treasury or warehouse from permanently blocking new players.

Balance public production as baseline supply and strategic infrastructure; leave profitable supply, processing, and freight opportunities for corporations. Public procurement can respond to stock deficits and planned projects, with explicit budgets. It should not endlessly buy and discard arbitrary output.

The station can host both player listings and a public buyer of last resort. Players retain control of their asking prices.

Proposed public procurement uses posted reference prices, finite purchasing budgets, and stock targets. Avoid promising a universal percentage below “market value”: a thin player market has no reliable price, and related accounts could manipulate it. Later, sufficiently liquid markets may inform bounded reference-price adjustments.

Unlimited government buying could make player customers unnecessary. Government demand should help new firms and low-population servers while preserving a reason to negotiate better private deals.

Proposed currency sources: public wages and procurement. Proposed sinks: charters, licenses, public land leases, and port service charges. Corporate payroll and ordinary player trades transfer existing money. Taxes paid into a spendable public treasury redistribute money rather than permanently removing it.

Public transit provides access before a corporation can build its own spacecraft. A commercial shipping starter kit must still have a viable route to operation and must not silently grant every industry a complete advanced spacecraft business.

## Proposed mod collection

These are logical boundaries first; release packaging can follow after the small prototype works.

| Module | Responsibility |
| --- | --- |
| Core | Identity, wallets, corporations, persistence, shared interfaces, validated configuration |
| Play Types and Administration | Rule presets, host/admin GUI, profiles, conflict policies |
| Government and Labor | Public facilities, shifts, wages, roles, payroll, expansion projects |
| Industry | Licenses, starter kits, research rules |
| Markets | Listings, depots, escrow, settlement |
| Freight | Carrier jobs, cargo custody, delivery verification |
| Central Station and Transit | Starting hub, public port, passenger assignments, planetary concessions, space integrations |
| Compatibility packs | Explicit adapters for chosen content mods |

Core financial and ownership state should have one authority. Optional mods should not introduce competing wallets or duplicate settlement logic.

## Dependency investigation

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

## Milestones and feasibility gates

1. **Technical experiments:** walkable starting station; government transport to a worksite and back; two corporate forces with isolated research; public employment and safe force switching; depot exchange between corporations; cross-force space cargo handoff. Establish practical server corporation limits.
2. **Playable station-to-worksite economy:** a small station concourse, one planetary public assignment, round-trip transit, public factory, personal income, corporation creation, one industry starter package, separate research, fixed-price deposited stock, pickup settlement, and one employee shift.
3. **Business specialization:** multiple industries, buy orders, delivery contracts, corporate permissions, low-population economic balancing.
4. **Expanded space commerce:** large station districts, additional tested planetary concessions, public expansion projects and procurement, shipping starter package, carrier-owned platforms, propulsion/capacity/defense progression.
5. **Optional depth:** additional planets, government policies, loans, insurance, subsidiaries, stock ownership, elections, and conflict rules only after scope decisions.

The first playable success case: Alice arrives at the station, accepts a public assignment, rides government transport to a worksite, earns wages, and can return. She later creates a steel company, hires Bob, and sells deposited steel to Carol's separate company. Bob is paid, Carol receives the goods, the seller receives the money, and research remains isolated across save/load and reconnect.

Space gate: a carrier with its own force must load a seller's cargo and deliver it to a buyer's depot without ownership confusion, duplication, or bypassing the freight agreement. Do not promise ordinary platform requests support this automatically.

## Open design decisions

Resolved direction:

- Support cooperative competition, regulated PvP, and open corporate warfare through configurable Play Types and an admin GUI. Exact policy defaults remain proposals.
- Require Space Age and allow selected additional planetary mods.
- Preferred spawn concept is a walkable central space station with contracts, recruitment, and government taxis to planetary worksites.
- Public contracts expand government-owned infrastructure and supply while supporting corporate growth.
- No hand crafting; all crafted goods require factory production, and government construction jobs supply their buildings.
- Physical planetary and government station warehouses, space-warehousing contracts, and government taxis are confirmed.
- Defense contractors supply equipment and services for asteroid threats and planetary hostiles, with corporate combat governed by Play Type.

Still open:

- Research execution: the latest proposal earns science packs from government trade and retains milestones/trials; supplied labs are recommended, with passive general advancement still unresolved.
- Government Science Pack conversion: output ratios, stock-backed exchange versus synthesis, corporate throughput limits, advanced-output access, and cash-plus-token reward terms.
- Common infinite research: benefits, cost scaling, and normal queue versus separate progression.
- Dedicated station surface versus an actual walkable platform?
- Which planetary mods and exact Factorio version form the first tested content set?
- Scheduled physical passenger platforms versus a scripted transit service for the first prototype?

Next questions:

- Expected concurrent players, total corporations, and persistent-world versus seasonal-server format?
- Scripted public government, administrator-run government, or elected player government?
- How are planetary concessions allocated: leased plots, remote resource claims, or other arrangements?
- Does a starter contract grant completed foundational technologies, research eligibility, or both?
- Desired typical time from joining to founding a business?
- What work qualifies for wages, and what inactivity grace period feels fair?
- How should businesses operate when their owner is offline, bankrupt, or absent long-term?
- Should carrier failures mean refunds, cargo replacement, insurance claims, or buyer risk?

All recommendations in this draft remain open to revision as brainstorming continues.
