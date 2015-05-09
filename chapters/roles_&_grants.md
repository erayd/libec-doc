# Roles & Grants

Libec uses roles to implement a hierachal set of granular permissions attached to a certificate. This is intended to allow issuing certificates with various embedded permissions, which can be verified up to a trust root without needing to query any third party.

Roles are defined as a set of period-delimited strings, where each string is considered to be a child of the string to its left. The rightmost string may optionally be '*' - this is a wildcard match.

As an example, the role of `com.example.myPond.goFishing` might allow the user to go fishing in the pond, but not feed the fish, or stir the water. In contrast, the role `com.example.myPond.*` would allow the user to do anything to the pond, or anything below it (i.e. the user is also considered to have `com.example.myPond.lilyPad.locateFrog`).

In order for a role to be considered valid, the certificate's signer must have either the same role, or a parent / wildcard role which matches it, defined as a grant. In order for a grant to be considered valid, the certificate's signer must have the same grant, or a parent / wildcard grant which matches it, defined as a grant.

This means that a certificate may also delegate granting authority for any role it is capable of granting.

A certificate with the flag `EC_CERT_TRUSTED` may define any role or grant, with no higher authority required to validate it. Such roles and grants must be explicitly defined in the certificate; by default a certificate defines nothing at all.

##ec_role_add()
`ec_record_t *ec_role_add(ec_cert_t *c, char *role);`

Add a role to a certificate. Returns the new role record on success, NULL otherwise.

```c
#include <ec.h>
...
ec_record_t *r = ec_role_add(c, "com.example.myPond.goFishing");
if(r == NULL) {
    //failed to add role
}
...
```

##ec_role_grant()
`ec_record_t *ec_role_grant(ec_cert_t *c, char *role);`

Allow a certificate to grant a role. Returns the new grant record on success, NULL otherwise.

```c
#include <ec.h>
...
ec_record_t *g = ec_role_grant(c, "com.example.myPond.*");
if(g == NULL) {
    //failed to add grant for role
}
...
```

##ec_role_has()
`ec_err_t ec_role_has(ec_cert_t *c, char *role);`

Check whether a certificate holds the given role. Returns EC_OK on success, or a nonzero error code otherwise.

If the certificate holds a matching wildcard role, this is considered sufficient (e.g. if a certificate holds `com.example.*`, it is also considered to hold `com.example.myPond.goFishing`).

```c
#include <ec.h>
...
if(ec_role_has(c, "com.example.myPond") == EC_OK) {
    //certificate holds the com.example.myPond permission
}
...
```

##ec_role_has_grant()
`ec_err_t ec_role_has_grant(ec_cert_t *c, char *role);`

Check whether a certificate is allowed to grant the given role. This does ***not*** imply that the certificate also holds this role; grants and roles may be held independantly.

If the certificate holds a matching wildcard grant, this is considered sufficient (e.g. if a certificate defines a grant for `com.example.*`, it may also grant `com.example.myPond.goFishing`).

```c
#include <ec.h>
...
if(ec_role_has_grant(c, "com.example.myPond.goFishing") == EC_OK) {
    //certificate may grant com.example.myPond.goFishing
}
...
```