# nodeelect
Public Readme

# Agent configuration

/etc/nodeelect/config.ini

Possible Configuration values 
```ini
[general]
# if true (default) it will save secure data with tpm support
tpm=true

[activationserver]
url: https://
# Optional certificate. If provided its used for pinning
certificate: /etc/nodeelect/certs/server1.pbkey

# this tags will be exposed to the activation server and could help the
# operator to validate the request
[tags]
key1=value
key2=value
key3=value
```
# REST Service

# Service configuration
Possible Configuration values 
```ini
[general]
# time when the process token is expiring in seconds. 
# default 1800 = 30 min
ttl_processtoken_s=1800
# time when the activation toke is expiring
# default 300 = 5 min
ttl_activationtoken_s=300

# Tenants defined in /etc/nodeelect/tenants as sub dir for each
# ./defaults --> default set of properties assigned to each device,
#                e.g. service URLs
# ./ca/      --> EasyRSA based CA
operator=/etc/nodeelect/operator/

# Tenants defined in /etc/nodeelect/tenants as sub dir for each
# tenants/t1/config   --> tenant configuration file with name etc.
# tenants/t1/passwd   --> tenant admin user authentication
# tenants/t1/defaults --> default set of properties assigned to each device,
#                         e.g. service URLs
# tenants/t1/ca/      --> EasyRSA based CA
tenants=/etc/nodeelect/tenants/

[security]
# not supported in version 1
operator_sso=...


# Hooks can be used to executed additional tasks like pushing data to an asset management system
# they are all optional
[hooks]
edge_request_registration=/etc/nodeelect/hooks/registeredge
edge_activate=/etc/nodeelect/hooks/activate
operator

```

# under development

* security: none
    * /device/SERIAL/register
* security: cert + activation token
    * /device/SERIAL/register/TENANTID/activate --> validate cert
* security: cert
    * /device/SERIAL/register/TENANTID/deactivate  --> revoke cert (called by edge at factory reset)
    * /device/SERIAL/status

* security: user / pwd
    * /tenant/TENANTID/register/SERIAL/MAC  --> allow registration
    * /tenant/TENANTID/assign/SERIAL/MAC    --> allow registration
    * /tenant/TENANTID/revoke/SERIAL        --> revoke all certificates

## Calls used by Edge device

instead of the MAC Addresses also a pre shared key could be used

### POST /register/SERIAL

```json
{ 
    mac: {
        "aa-bb-cc",
        "a1-bb-cc"
    },
    manufacturer: "PC Company",
    model: "XCA1 Small",
    cpu: "ARM"
}
```

return 200 if activation is in progress, otherwise 404

```json
{ 
    activationtoken: "11h-33d-45d-3354-",
    tenant: "99-33-s1-234-1"
}
```

### POST /activate/tenant/SERIAL
Header:
* token: activationtoken

{
    signing_request: "BASE64-PublicKey"
}
return
```json
{ 
    signed_key: "BASE64-SignedKey"
}
```

# Used by Operator
## POST /register/tenant/SERIAL
```json
{ 
    mac: "11h-33d-45d-3354-"
}
```
