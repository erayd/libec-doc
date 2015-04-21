# Certificate Layout
A libec certificate consists of a version number, a bitfield for flags, and zero or more resource records arranged into sections.

## Version
The version number indicates the layout version used in the certificate. The current (and only) version is 1.

## Flags
The flags bitfield is split into two parts. The LSB is used for flags which are signed and exported along with the certificate, and the MSB contains flags which are intended for internal use.

Available flags are:

Flag | Internal | Description
--: | :-- | :--
EC_CERT_TRUSTED | YES | Certificate is an ultimately trusted root.

## Records
Records are attached to the certificate as a singly-linked list, organised into sections. Each record has a flags bitfield similar to the equivalent field for the certificate itself, a key field, and a data field.

Key fields may be up EC_RECORD_KMAX long, and data fields may be up to EC_RECORD_DMAX. The entire record, including overhead, must be no longer than EC_RECORD_MAX. Both key and data fields are optional; it's possible to define a completely empty record.

Available record flags are:

Flag | Internal | Description
--: | :-- | :--
EC_RECORD_SECTION | NO | The record starts a new section.
EC_RECORD_INHERIT | NO | If this is a section record, then all other records added to the same section will by default inherit this record's flags in addition to their own. Otherwise, setting this flag will cause this record to inherit flags from the header record for its section.
EC_RECORD_NOSIGN | NO | The record will not be included in signature generation. This is used for things like secret keys, and for non-secure convenience records which can be added after a certificate is already signed.
EC_RECORD_KFREE | YES | Free the record's key data when the record is destroyed.
EC_RECORD_KCOPY | YES | Copy the record's key data during creation, rather than just referencing it. Implies EC_RECORD_KFREE.
EC_RECORD_DFREE | YES | As per EC_RECORD_KFREE, but for data.
EC_RECORD_DCOPY | YES | As per EC_RECORD_KCOPY, but for data. Implies EC_RECORD_DFREE.