# Osysharp.OfferSales.Memberships

A membership plan is something [Osysharp.OfferSales](https://github.com/osysharp) sells: a day ticket, a monthly desk, a
yearly pass, a card of visits. Once a purchase is paid — online, by transfer or invoice marked received, or at the
counter — the membership is sold to whoever it was bought for, at what was paid, **once**, however often the payment is
reported. Refunded in full, it ends, saying so. A part refund leaves it running.

## Use it

Nothing to write but the offers. It arrives by itself when an app uses both kits (`osy lock` includes it and says so);
to keep it out, `without Osysharp.OfferSales.Memberships;` in `app.osy`.

```osy
new MembershipOffer { Key = "monthly", Name = "Monthly desk", Price = 22000, Plan = monthly };
```

The offer's price is what is charged; the plan's length, visits and pausing are the membership's. One membership per
one bought, each under its own reference, so a refund ends exactly those.

## Source

[github.com/osysharp/offersales/memberships](https://github.com/osysharp/offersales/tree/main/memberships); its tests are a co-working space's.

## Examples

Taken from this package's own tests, which compile and run on every release.

```osy
using Osysharp.Identity;
using Osysharp.UserAccounts;
using Osysharp.OfferSales;
using Osysharp.Memberships;
using Osysharp.OfferSales.Memberships;

[Test(Space)]
[runas(Noor)]
void a_monthly_desk_paid_by_invoice_is_sold_once_and_lets_her_in() {
  var p = StartPurchase(Monthly(), "invoice").Purchase;
  runas(Hal) {
    MarkPurchasePaid(Purchase.Single(x => x.Id == p.Id));
    MarkPurchasePaid(Purchase.Single(x => x.Id == p.Id));
  }
  Background.Settle();
  runas(Hal) {
    Assert.Equal(1, Membership.Count(m => m.Holder == Noor));
    var m = Membership.Single(x => x.Holder == Noor);
    Assert.Equal(220m, m.Price);
    Assert.Equal(new DateTime(2026, 5, 1, 9, 0, 0), m.ValidUntil);
    Assert.True(EntryFor(Noor).CanEnter);
  }
}
```

From [`tests/desks.test.osy`](tests/desks.test.osy).

```osy
using Osysharp.Identity;
using Osysharp.UserAccounts;
using Osysharp.OfferSales;
using Osysharp.Memberships;
using Osysharp.OfferSales.Memberships;

[Test(Space)]
[runas(Hal)]
void a_whole_refund_ends_the_membership_and_a_part_does_not() {
  var p = SellAtCounter(Monthly(), Noor, "Card on the terminal");
  Background.Settle();
  RefundPurchase(Purchase.Single(x => x.Id == p.Id), "Goodwill", 2000);
  Background.Settle();
  Assert.True(EntryFor(Noor).CanEnter);
  RefundPurchase(Purchase.Single(x => x.Id == p.Id), "Left in the first week");
  Background.Settle();
  Assert.False(EntryFor(Noor).CanEnter);
  Assert.Contains("Refunded", Membership.Single(x => x.Holder == Noor).EndedBecause);
}
```

From [`tests/desks.test.osy`](tests/desks.test.osy).

## Learn more

- [The Osysharp.OfferSales.Memberships reference](https://osysharp.com/reference/ui/offer-sales-kit/)
- [Osysharp.OfferSales.Memberships in the package catalogue](https://osysharp.com/packages/#osysharp-offersales-memberships)
- [The Osy# reference](https://osysharp.com/reference/) — the language, the runtime and every kit
- [`llms-full.txt`](https://osysharp.com/llms-full.txt) — the whole reference and specification as one Markdown file, for a coding agent

## Licence

MIT — see [`LICENSE`](LICENSE).
