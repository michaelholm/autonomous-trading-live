# Watchlist

Tickers the research routine should pull news and moving
averages for, and the trade-evaluation routine may consider
for entries, subject to the position-sizing and order rules in the agent
instructions. This list is a starting universe, not a mandate to buy. In the 
trade-evaluation routine, tickers not on this list that surface in the research routine
should also be considered as trade candidate, each ticker still goes through the full 
Decision Framework (cash, existing positions, news, moving averages, risk) before any trade.

**Inherited from the paper-trading account** (`michaelholm/autonomous-trading`) at the
time this live account launched. See that repo's `watchlist.md` for the full sourcing
history of every batch (13F filings, congressional trade trackers, pruning decisions).

| Ticker | Company | Added |
|---|---|---|
| AAPL | Apple Inc. | inherited |
| GOOGL | Alphabet Inc. (Google) | inherited |
| META | Meta Platforms, Inc. (Facebook) | inherited |
| ORCL | Oracle Corporation | inherited |
| NVDA | Nvidia | inherited |
| DELL | Dell Technologies Inc. | inherited |
| VZ | Verizon Communications Inc. | inherited |
| T | AT&T Inc. | inherited |
| ABBV | AbbVie | inherited |
| NVO | Nordisk | inherited |
| PFE | Pfizer | inherited |
| MRK | Merck | inherited |
| BMY | Bristol-Myers Squibb | inherited |
| MRVL | Marvell | inherited |
| AB | AllianceBernstein Holding, L.P. | inherited |
| CRWD | CrowdStrike Holdings, Inc. | inherited |
| IBKR | Interactive Brokers Group, Inc. | inherited |
| MORN | Morningstar, Inc. | inherited |
| PYPL | PayPal Holdings, Inc. | inherited |
| RBLX | Roblox Corporation | inherited |
| WBD | Warner Bros. Discovery, Inc. | inherited |
| PANW | Palo Alto Networks, Inc. | inherited |
| VST | Vistra Corp. | inherited |
| SPY | State Street SPDR S&P 500 ETF Trust | inherited |
| IVV | iShares Core S&P 500 ETF | inherited |
| TSM | Taiwan Semiconductor Manufacturing Co. | inherited |
| AER | AerCap Holdings N.V. | inherited |
| RPRX | Royalty Pharma plc | inherited |
| APP | AppLovin Corporation | inherited |
| LYV | Live Nation Entertainment Inc. | inherited |
| TRMD | TORM plc | inherited |
| GTX | Garrett Motion Inc. | inherited |
| EXE | Expand Energy Corporation | inherited |
| INDV | Indivior Pharmaceuticals, Inc. | inherited |
| CORZ | Core Scientific, Inc. | inherited |
| VNOM | Viper Energy, Inc. | inherited |
| TDS | Telephone and Data Systems Inc. | inherited |
| CBL | CBL & Associates Properties, Inc. | inherited |
| TLN | Talen Energy Corporation | inherited |
| CRDO | Credo Technology Group Holding Ltd | inherited |
| PBR | Petroleo Brasileiro S.A. - Petrobras (ADS) | inherited |
| B | Barrick Mining Corporation | inherited |
| NOK | Nokia Corporation | inherited |
| ITUB | Itau Unibanco Holding S.A. (ADS) | inherited |
| TAC | TransAlta Corporation | inherited |
| TDW | Tidewater, Inc. | inherited |
| BCC | Boise Cascade Company | inherited |
| VAL | Valaris Limited | inherited |
| HOG | Harley-Davidson, Inc. | inherited |
| RHI | Robert Half Inc. | inherited |
| HCC | Warrior Met Coal, Inc. | inherited |
| FPH | Five Point Holdings, LLC | inherited |
| BN | Brookfield Corporation | inherited |
| ROG | Rogers Corporation | inherited |
| LEN.B | Lennar Corporation Class B | inherited |
| UHAL | U-Haul Holding Company | inherited |
| ECPG | Encore Capital Group Inc | inherited |
| PHM | PulteGroup, Inc. | inherited |
| SUI | Sun Communities, Inc. | inherited |
| UAA | Under Armour, Inc. (Class A) | inherited |
| BB | BlackBerry Limited | inherited |
| ORLA | Orla Mining Ltd. | inherited |
| CLF | Cleveland-Cliffs Inc. | inherited |
| ATS | ATS Corporation | inherited |
| TAP | Molson Coors Beverage Company | inherited |
| HP | Helmerich & Payne, Inc. | inherited |
| BNS | Bank of Nova Scotia | inherited |
| WEN | The Wendy's Company | inherited |
| CNH | CNH Industrial N.V. | inherited |
| IONS | Ionis Pharmaceuticals, Inc. | inherited |
| KOF | Coca-Cola FEMSA, S.A.B. de C.V. (ADS) | inherited |
| ALV | Autoliv, Inc. | inherited |
| NVST | Envista Holdings Corporation | inherited |
| UNF | UniFirst Corp | inherited |
| TFC | Truist Financial Corporation | inherited |
| NOW | ServiceNow, Inc. | inherited |

## Notes
- "Google" and "Alphabet" are the same company (GOOGL); "Facebook" and
  "Meta" are the same company (META, renamed from Facebook Inc. in 2021).
  Each is listed once above to avoid double-counting position sizing
  against a single underlying company.
- This list is a starting universe, not a mandate to buy — each ticker
  still goes through the full Decision Framework (cash, existing
  positions, news, moving averages, risk) before any trade.
- Add/remove tickers here as the account owner's preferences change.
- Full sourcing/provenance history for every ticker on this list lives in
  the paper-trading repo's `watchlist.md` (`michaelholm/autonomous-trading`)
  as of the date this live account launched — not repeated here to avoid
  drift between two copies of the same history. Any changes made to this
  list *after* launch should be logged here going forward.
