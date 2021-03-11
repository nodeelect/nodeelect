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

# deprecated! deprecated! deprecated!
# Tenants will in further version be handled by an external service
# IDEA: handle tenants in /etc/nodeelect/tenants and having a sub dir for each one
[tenants]
# define the 
tenant1=11-22-33-44

# deprecated! deprecated! deprecated!
[tenant:11-22-33-44]
name=Tenant 1
description=

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
