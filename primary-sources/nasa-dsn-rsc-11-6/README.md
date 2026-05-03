# NASA DSN — RSC-11-6 Software Interface Specification

## What's in this folder

- `rsc-11-6_unpack.pdf` — *Interpretation and Use of Binary RSC-11-6
  Data*, by Richard Simpson (Stanford / PDS Radio Science Sub-Node),
  29 April 2021. 6 pages.

## What this document is

A detailed walk-through of the binary file format used by the Deep
Space Network to record raw radio-science samples during the Voyager
encounters. Each `.dat` file consists of 56-byte headers followed
by 5,000 8-bit voltage samples per record, originally captured at
150 ksps (kilosamples per second) by 70m DSN antennas tracking
Voyager during planetary flybys.

The document references:

> [1] Document 820-013 (Rev. A), DSN System Requirements, Detailed
> Interface Design, RSC-11-6, DSN Interfaces Radio Science, Medium
> Band Computer Compatible IDR, effective date 1 July 1981.

That underlying interface specification is part of the DSN's
820-013 document family.

## Worked example included in the spec

The document walks through a real Voyager 1 Saturn-encounter file:

- **Spacecraft:** Voyager 1 (S/C number 31)
- **DSN station:** DSS-63 (Madrid, Spain — source station 21)
- **Date:** 1980-318 (13 November 1980)
- **Time:** 04:44:59.999712 UTC — about 5 hours after Voyager 1's
  closest approach to Saturn
- **Sample rate:** 150 ksps recorded; decimation ratio 5
- **First three sample values:** `0xb6 0x72 0x74` (decimal 182, 114, 116)

These are literal voltage-proportional samples of the radio signal
received from Voyager 1 as it transmitted past Saturn's atmosphere
on November 13, 1980.

## Where to get the actual data

The companion data product `vj6001.dat` and its derived files are
archived at NASA's Planetary Data System under the URN:

> `urn:nasa:pds:radiosci.documentation:dsn.rsc-11-6:vj6001`

PDS nodes that mirror Voyager radio-science data:

- [pds-rings.seti.org/voyager/rss/](https://pds-rings.seti.org/voyager/rss/)
- [pds-geosciences.wustl.edu/radiosciencedocs/](https://pds-geosciences.wustl.edu/radiosciencedocs/)
- [pds-atmospheres.nmsu.edu/data_and_services/atmospheres_data/Voyager/rss.html](https://pds-atmospheres.nmsu.edu/data_and_services/atmospheres_data/Voyager/rss.html)

## Use status

NASA-PDS public technical specification. Redistributable for
research and educational purposes.
