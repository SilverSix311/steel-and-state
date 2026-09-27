# Direction and Decisions

Source: project planning conversation, September 24–27, 2026. These notes are not a promise that every proposed mechanic is feasible. Current user instructions take precedence.

## Established user direction

- Require Factorio: Space Age; consider additional planetary mods.
- Government/corporate multiplayer economy with clocked work, wages, company founding, and separate corporate research.
- Non-tradable startup loans help finance initial corporate contracts (confirmed 2026-09-27). Repayment terms, eligibility, and borrower identity remain open. See [Corporations and Employment](../design/Corporations%20and%20Employment.md).
- Support multiple configurable Play Types and a host/admin interface.
- No hand crafting; government factory-building jobs supply buildings.
- Physical planetary and government station warehouses, cargo delivery/pickup, and government passenger taxis.
- Defense contracting for shipping hazards and planetary hostiles.
- A science-manufacturing industry that corporations can add, importing or producing ingredients.
- Work locally in Git for now; prepare loose docs and pseudocode that can be opened as an Obsidian vault. GitHub publishing is deferred.

## Latest user proposals carried forward

- Explore planetary servers and large corporation counts. Global corporation IDs with local force bindings and Clusterio are recommendations under investigation, not selected infrastructure. See [Cluster and Force Scaling](../architecture/Cluster%20and%20Force%20Scaling.md).

- Start at a large walkable central trade station rather than Nauvis.
- Government trade awards Government Science Packs. Conversion recipes use these as the only material input and output conventional colored science.
- Conversion is intentionally slower than commercial science supply; expanded R&D supports more processing.
- Include one lab with each eligible contract, with the meaning of “each contract” still to resolve.
- Combine science consumption with milestones/prototype trials and common repeatable research.

## Assistant suggestions, not settled rules

- One lab plus a converter and power equipment is a complete starting R&D package.
- Global corporation identities with a local force per corporation only on instances where needed; each save has a 64-force engine limit including built-ins. Work access may temporarily follow an employer force.
- Supplied labs rather than free passive research; common and specialty research initially share normal queue behavior.
- Deposited stock, escrow, bounded government demand, issued-equipment reservations, and explicit acceptance states.
- Dedicated station surface and scripted transit are candidates, not selected implementations.

## Superseded alternatives

- A founding voucher/grant was suggested but the user selected a repayable startup loan instead. Tradable bonds are not the chosen founding mechanism.
- Stock-backed government science redemption and a central conversion queue are superseded by the user's recipe approach. Government packs intentionally synthesize colored packs rather than withdrawing existing colored stock.
- Pure production points are background exploration, not the current research model.
- Base-game-only or Space Exploration as an alternative foundation is out of current scope.

See [Research](../design/Research%20and%20Science.md), [Open Questions](Open%20Questions.md), and [Historical Draft](../archive/Initial%20Brainstorm.md).
