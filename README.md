# Electronic Voucher Management System Migration

A guide for telecom operators and VAS partners moving from a legacy voucher platform to a new **electronic voucher management system** (EVMS). It covers what data has to move, how to cut over without losing PIN state, and how to prove afterwards that nothing went missing. Published by [MoboGage](https://evdsystem.com/about-mobogage/) alongside [EVD System](https://evdsystem.com/).

---

## What you are actually migrating

An EVMS migration is not a database copy. The legacy system holds several kinds of state, and each has a different risk if it moves badly.

**Voucher stock and PIN state.** Every serial with its state: generated, in stock, allocated, sold, used, expired, blocked. Getting a "used" voucher back to "in stock" is how double redemptions happen.

**Hierarchy and credit.** Distributors, sub-dealers and retailers, their parent links, wallet balances and credit limits.

**Pricing and commission.** Denominations, dealer prices, commission plans and any open commission not yet settled.

**History.** Sales and redemption records needed for disputes, tax and regulatory retention. You may not need all of it live, but you need to know where it lives.

**Integrations.** The links to the operator's IN or charging system (often UCIP or a similar protocol for recharge), the POS fleet, the reseller apps and any partner APIs.

## Three cutover patterns

| Pattern | How it works | When it fits |
|---------|-------------|--------------|
| Big bang | Freeze legacy, migrate everything, switch all traffic | Small estates, strong rehearsal |
| Stock drain | New batches generated only on the new EVMS; legacy keeps serving its old stock until it runs out | When PIN migration is risky or the legacy encryption cannot be exported |
| Channel by channel | Move one distributor group or region at a time | Large reseller trees, mixed POS fleets |

Stock drain is often the safest for PIN vouchers because PINs never leave the system that generated them. The cost is running two platforms in parallel, and the redemption path on the IN side has to know which system owns a given serial range.

## The freeze window

For any pattern that moves live state, there is a period where the legacy system must not change. Plan it around the lowest sales hour, and agree in advance what resellers see during it. A POS that shows "Service unavailable, try in 30 minutes" is better than one that accepts a sale the new system never hears about.

## Proving the migration

Before switching traffic, run counts and sums on both sides and compare:

- Voucher count by denomination and state
- Sum of face value by state
- Wallet balance per reseller and in total
- Open commission per reseller
- Number of active POS terminals and users

Then pick a random sample of serials and check each one field by field. Counts can match while individual records are wrong.

## Worked example (figures are illustrative)

An operator has 2.4 million serials in the legacy EVMS. They choose stock drain for PIN vouchers and big bang for the reseller tree. In rehearsal, wallet totals match but the per-reseller compare shows 14 resellers off by small amounts; the cause is a rounding rule on commission credits in the legacy system. The rule is replicated, rehearsal is repeated, and all per-reseller balances match. On the go-live night, the reseller tree moves in a 40-minute freeze. Legacy keeps redeeming its old PIN ranges for the following months while new batches come from the new EVMS.

A cutover checklist is in [docs/cutover-checklist.md](./docs/cutover-checklist.md).

---

## FAQ

**What is an electronic voucher management system migration?**
It is the move of voucher stock, PIN states, reseller hierarchy, balances and integrations from one EVMS to another, with a controlled cutover and a reconciliation that proves nothing was lost.

**Can encrypted voucher PINs be migrated?**
Sometimes. It depends on whether the legacy encryption keys can be used or exported securely. If not, let legacy serve its existing stock and generate new stock on the new platform.

**How long should the freeze window be?**
As short as rehearsal shows is safe. Measure it in a full rehearsal with production-sized data, then add margin.

**What is the biggest risk?**
Voucher state errors, especially a used or blocked serial coming back as sellable. Reconcile by state, not only by total count.

**How does EVD System fit?**
EVD System is MoboGage's electronic voucher distribution and management platform for telecom airtime, gift cards and loyalty vouchers.

---

## Further reading

- [Electronic voucher management system for telecom](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) on evdsystem.com
- [Electronic voucher distribution software](https://evdsystem.com/evd-software/electronic-voucher-distribution-software/) on evdsystem.com
- Related repos: [electronic-voucher-management-disaster-recovery](https://github.com/iwtglobal/electronic-voucher-management-disaster-recovery), [voucher-inventory-reconciliation](https://github.com/iwtglobal/voucher-inventory-reconciliation)

## License

MIT. See [LICENSE](./LICENSE). Educational material only.
