# Osysharp.OfferSales.PrepaidCredits

A pack of prepaid credits is something [Osysharp.OfferSales](https://github.com/osysharp) sells: a ten-class card, five
hours of studio time. Once a purchase is paid — online, by transfer marked received, or at the counter — the pack is
granted to whoever it was bought for, **once**, however often the payment is reported. Refunded in full, the pack is
voided. A part refund leaves it as it is.

## Use it

Nothing to write but the offers. It arrives by itself when an app uses both kits (`osy lock` includes it and says so);
to keep it out, `without Osysharp.OfferSales.PrepaidCredits;` in `app.osy`.

```osy
new CreditPackOffer { Key = "wheel-10", Name = "Ten wheel sessions", Price = 18000, Credits = 10, ValidDays = 180, Scope = "wheel" };
```

`Credits` per one bought (two cards are twenty), `ValidDays` from the day it is paid (0 — they do not lapse), `Scope`
the purse they are spent from. The pack's reference is the purchase's, and each credit's value is what was paid for it,
so the value still owed is right.

## Source

[github.com/osysharp/offersales/prepaidcredits](https://github.com/osysharp/offersales/tree/main/prepaidcredits); its tests are a pottery studio's.

## Examples

Taken from this package's own tests, which compile and run on every release.

```osy
using Osysharp.Identity;
using Osysharp.UserAccounts;
using Osysharp.OfferSales;
using Osysharp.PrepaidCredits;
using Osysharp.OfferSales.PrepaidCredits;

[Test(Studio)]
[runas(Pip)]
void a_card_paid_by_transfer_grants_ten_sessions_once() {
  var p = StartPurchase(Card(), "transfer").Purchase;
  Assert.Equal(0, CreditsLeft(Pip, "wheel"));
  runas(Ola) {
    MarkPurchasePaid(Purchase.Single(x => x.Id == p.Id));
    MarkPurchasePaid(Purchase.Single(x => x.Id == p.Id));
  }
  Background.Settle();
  Assert.Equal(10, CreditsLeft(Pip, "wheel"));
  var pack = CreditPack.Single(x => x.Holder == Pip);
  Assert.Equal(new DateTime(2026, 8, 29, 10, 0, 0), pack.ExpiresAt);
  Assert.Equal(18m, pack.UnitValue);
}
```

From [`tests/cards.test.osy`](tests/cards.test.osy).

```osy
using Osysharp.Identity;
using Osysharp.UserAccounts;
using Osysharp.OfferSales;
using Osysharp.PrepaidCredits;
using Osysharp.OfferSales.PrepaidCredits;

[Test(Studio)]
[runas(Ola)]
void two_cards_at_the_counter_grant_twenty_and_a_whole_refund_voids_them() {
  var p = SellAtCounter(Card(), Pip, "Cash", 2);
  Background.Settle();
  Assert.Equal(20, CreditsLeft(Pip, "wheel"));
  RefundPurchase(Purchase.Single(x => x.Id == p.Id), "Moved away", 1000);
  Background.Settle();
  Assert.Equal(20, CreditsLeft(Pip, "wheel"));
  RefundPurchase(Purchase.Single(x => x.Id == p.Id), "Moved away");
  Background.Settle();
  Assert.Equal(0, CreditsLeft(Pip, "wheel"));
}
```

From [`tests/cards.test.osy`](tests/cards.test.osy).

```osy
using Osysharp.Identity;
using Osysharp.UserAccounts;
using Osysharp.OfferSales;
using Osysharp.PrepaidCredits;
using Osysharp.OfferSales.PrepaidCredits;

[Test(Studio)]
[runas(Pip)]
void a_potter_cannot_add_a_card_offer() {
  Assert.Denied(() => { new CreditPackOffer { Key = "free", Name = "Free sessions", Price = 0, Credits = 100 }; UnitOfWork.Commit(); });
}
```

From [`tests/cards.test.osy`](tests/cards.test.osy).

## Learn more

- [The Osysharp.OfferSales.PrepaidCredits reference](https://osysharp.com/reference/ui/offer-sales-kit/)
- [Osysharp.OfferSales.PrepaidCredits in the package catalogue](https://osysharp.com/packages/#osysharp-offersales-prepaidcredits)
- [The Osy# reference](https://osysharp.com/reference/) — the language, the runtime and every kit
- [`llms-full.txt`](https://osysharp.com/llms-full.txt) — the whole reference and specification as one Markdown file, for a coding agent

## Licence

MIT — see [`LICENSE`](LICENSE).
