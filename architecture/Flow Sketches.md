# Flow Sketches

Non-executable pseudocode. Names describe intent rather than Factorio APIs. Failure behavior is part of the design, not implemented validation.

## Public construction job

```text
accept job:
    check applicant, site, and eligibility
    reserve budget, issued equipment, and assignment entitlement
    arrange passenger transport and equipment delivery

commission site:
    verify required functioning service/output
    record public ownership and return unused equipment
    settle the agreed reward exactly once

cancel or abandon:
    recover what can be recovered
    release outstanding reservations under the agreement
    preserve a passenger return path
```

## Corporate employment

```text
clock in:
    check role, accepted pay, and funded shift allowance
    grant the necessary work access
    keep company ownership independent of active employer

payroll interval:
    pay eligible elapsed work time from employer funds
    if funding ends, notify and stop new paid accrual

clock out or disconnect:
    settle remaining eligible time once
    restore appropriate access and stop wage accrual
```

## Warehouse sale and freight

```text
place order:
    reserve actual seller inventory and buyer payment
    capture price, location, ownership-transfer point, and failure terms

handover:
    verify goods and authorized custody change
    track where the shipment physically is

fulfill:
    verify agreed delivery once
    release seller/carrier payments and clear reservations
```

## Corporate research

```text
settle eligible government trade:
    award the agreed Government Science Packs to the seller

run conversion machine:
    consume government packs as the only recipe material
    after processing, output the selected colored pack

run lab:
    consume required colored packs
    advance the corporation's eligible technology
```

Research ratios, milestone attribution, refunds, and conversion capacity need design work. See [Research](../design/Research%20and%20Science.md) and [Government](../design/Government%20and%20Contracts.md).
