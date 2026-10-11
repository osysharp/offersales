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
