# Osysharp.OfferSales.PrepaidCredits — changes

## 1.0.0
- Grouped into classes (`osy restructure`). Calls move:
  - `CreditPackReference(…)` → `CreditPackOffer.CreditPackReference(…)`
- Published tools, actions and actors keep their names: what calls them by name is unchanged.
- A project moves when it bumps the kit: `use Osysharp.OfferSales.PrepaidCredits@1;`, then the calls above.
