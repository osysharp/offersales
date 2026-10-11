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
