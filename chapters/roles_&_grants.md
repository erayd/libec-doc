# Roles & Grants

Libec uses roles to implement a hierachal set of granular permissions attached to a certificate. This is intended to allow issuing certificates with various embedded permissions, which can be verified up to a trust root without needing to query any third party.

Roles are defined as a set of period-delimited strings, where each string is considered to be a child of the string to its left. The rightmost string may optionally be '*' - this is a wildcard match.

As an example, the role of `com.example.myPond.goFishing` might allow the user to go fishing in the pond, but not feed the fish, or stir the water. In contrast, the role `com.example.myPond.*` would allow the user to do anything to the pond, or anything below it (i.e. the user is also considered to have `com.example.myPond.lilyPad.locateFrog`).

In order for a role to be considered valid, the certificate's signer must have either the same role, or a parent / wildcard role which matches it, defined as a grant. In order for a grant to be considered valid, the certificate's signer must have the same grant, or a parent / wildcard grant which matches it, defined as a grant.

This means that a certificate may also delegate granting authority for any role it is capable of granting.