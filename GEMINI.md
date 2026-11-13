# ProductGuru product lookup

The `product_lookup` tool takes one barcode per call (EAN-13, EAN-8 or 12-digit UPC-A, check digit validated)
and returns factual data: name, brand, category, recorded specifications, and any government recall notice
linked to the barcode, with the agency's name, date, stated reason and a link to the agency's notice.

- A "not found" answer means ProductGuru has no published page for that barcode. It says nothing about recalls.
- A product with no linked notice is not a safety guarantee: many official notices carry no barcode.
- The issuing agency is the source of a recall; its notice link is the authoritative record.
- Medicines are not covered. The tool gives no medical, legal or financial advice.
- One barcode per call; there is no search or browse. Fair-use limit: 20 look-ups per minute.
