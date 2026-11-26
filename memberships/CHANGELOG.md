# Osysharp.OfferSales.Memberships — changes

## 1.0.0
- Grouped into classes (`osy restructure`). Calls move:
  - `MembershipReference(…)` → `MembershipOffer.MembershipReference(…)`
- Published tools, actions and actors keep their names: what calls them by name is unchanged.
- A project moves when it bumps the kit: `use Osysharp.OfferSales.Memberships@1;`, then the calls above.
