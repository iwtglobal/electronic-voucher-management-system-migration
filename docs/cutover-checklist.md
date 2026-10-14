# EVMS Cutover Checklist

## Before rehearsal

- [ ] Inventory of every data type: stock, PIN state, hierarchy, wallets, pricing, commission, history
- [ ] Decision per data type: migrate, drain, or archive
- [ ] Serial range ownership agreed with the IN / charging team
- [ ] PIN key handling approved by security

## Rehearsal

- [ ] Full rehearsal on production-sized data
- [ ] Freeze window timed end to end
- [ ] Count and sum compare by denomination and state
- [ ] Per-reseller balance and open commission compare
- [ ] Random serial sample checked field by field

## Go-live

- [ ] Reseller notice sent with the freeze time
- [ ] POS shows a clear unavailable message during freeze
- [ ] Legacy write access closed before export
- [ ] Final compare signed off before traffic switch
- [ ] Rollback point and owner named

## After

- [ ] Daily compare for the first weeks
- [ ] Legacy kept read-only for disputes
- [ ] Decommission only after drain and retention rules are satisfied
