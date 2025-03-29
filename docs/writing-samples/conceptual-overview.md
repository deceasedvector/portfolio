---
title: Feature overview
---

# Marking the open

## Overview

The Marking the Open alert detects potentially manipulative trading that occurs right after market opening. 
It identifies patterns where traders might artificially influence security prices during early market hours.
This guide explains how the alert works, the regulations it helps enforce, and how to configure it for US Equities trading.

### Key terms

* **VWAP (Volume Weighted Average Price)**: average price of a security weighted by trading volume at each price.
* **ADV (Average Daily Volume)**: average number of shares traded daily over 30 days.
* **Sample Time**: period after market open when alert monitors for manipulation.
* **Baseline Time**: reference period used to establish normal price levels after opening activity settles.

## Regulatory context

The alert helps detect activities prohibited by these regulations:

### SEC

- _Securities Exchange Act, Sections 10(b) and 9(a)(2)_

    Prohibits manipulation of securities prices and makes it unlawful to effect, alone or with one or more other persons, a series of transactions in any security creating actual or apparent active trading in such security, or raising or depressing the price of such security, for the purpose of inducing the purchase or sale of such security by others.

- _SEC Rule10b-5_

    Prohibits fraud, misrepresentation, and deceit in connection with the purchase or sale of any security and makes it unlawful for any person, directly or indirectly, by the use of any means or instrumentality of interstate commerce or of the mails or of any facility of any national securities exchange, to use any device, scheme, or artifice to defraud.

### FINRA

_Rule 2020_ prohibits securities brokers and dealers from effecting any transaction in, or inducing the purchase or sale of, any security by means of any manipulative, deceptive, or other fraudulent device or contrivance.

## How the alert works

The Marking the Open alert:

1. Monitors trading immediately after market open (within the Sample Time).
2. Compares the Volume Weighted Average Price (VWAP) during this period against a VWAP from a later period (Baseline Time).
3. Triggers when:

    * Price movements exceed your configured thresholds.
    * The trader's activity represents a significant portion of total market activity for the security.

## Configuration parameters

| Parameter | Description | Format | Example |
|-----------|-------------|--------|---------|
| **Delay** | Time after market open when Sample Time ends | Seconds | `60` |
| **Sample Time** | Period after market open when monitoring occurs | Seconds | `30` |
| **Baseline Time** | Period after Sample Time used for reference VWAP | Seconds | `300` |
| **Allowable Price Movement (fractional)** | Maximum price deviation from Baseline VWAP | Decimal | `0.02` (2%) |
| **Allowable Price Movement (stddev)** | Maximum deviation in standard deviation units | Number | `2` |
| **Allowable Fraction of Trading** | Maximum ratio of firm's trading to total market activity | Decimal | `0.20` (20%) |
| **ADV Minimum** | Minimum 30-day Average Daily Volume for eligible securities | Quantity | `50000` |
| **ADV Maximum** | Maximum 30-day Average Daily Volume for eligible securities | Quantity | `250000` |

## Implementation guide

### Recommended starting values

!!! warning
    These values are starting points. Adjust them based on your firm's trading patterns, risk tolerance, and compliance requirements.

For initial setup:

* **Baseline Time:** 300 seconds (5 minutes)
* **Sample Time:** 60 seconds
* **Allowable Price Movement:** 0.02 (2%) or 2 standard deviations
* **Allowable Fraction of Trading:** 0.20 (20%)

### Multi-market configuration

If you trade across multiple markets:

1. Create a base alert configuration.
2. Clone it for each market.
3. Adjust parameters for each market's specific characteristics.

### Ongoing optimization

After implementation:

1. Analyze alert performance.
2. Adjust parameters to reduce false positives while capturing genuine manipulation attempts.
3. Review configuration quarterly or after significant trading strategy changes.

## Alert details

The alert triggers for trades in eligible symbols near market open (within Sample Time) when:

* Prices exceed your configured threshold (too far from baseline VWAP), AND
* The account's trades represent more than the configured percentage of total market trades for that symbol

If no price threshold is specified, alerts trigger based solely on the trading volume threshold.

### Alert messages

When triggered, you'll see one of these messages:

- **Allowable price movement (fractional)**

    ``` title="Alert text"
    [Side] fill for qty [Qty] of [Symbol] at [Source TM] may be considered
    marking the open because the price [Dir] by more than [Price Limit]% from the
    baseline time window and the activity during the sample was [Trade PCT]% of
    the overall activity which is greater than [Trade PCT Limit] required.
    ```

- **Allowable price movement (stddev)**

    ``` title="Alert text"
    [Side] fill for qty [Qty] of [Symbol] at [Source TM] may be considered marking the open because the price [Dir] by more than [Stddev Limit] multiples of the standard deviation from the baseline time window and the activity during the sample was [Trade PCT]% of the overall activity which is greater than [Trade PCT Limit] required.
    ```

### Alert variables

* `[Side]`: buy or sell
* `[Qty]`: transaction quantity
* `[Symbol]`: security identifier
* `[Source TM]`: transaction timestamp
* `[Dir]`: price movement direction (increased/decreased)
* `[Price Limit]`: configured fractional price threshold
* `[Stddev Limit]`: configured standard deviation threshold
* `[Trade PCT]`: percentage of market activity from trader
* `[Trade PCT Limit]`: configured maximum allowed percentage