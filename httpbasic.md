<!---
Copryight 2016 floragunn GmbH
-->

# HTTP Basic Authentication

In order to set up HTTP Basic authentication, you just need to enable it in the http_authenticator section of the configuration:

```
http_authenticator:
  type: basic
  challenge: true
```

In most cases, you will want to set the `challenge` flag to `true`. The flag defines the behaviour of Search Guard, if the `Authorization` field in the HTTP header is not set:

If `challenge` is set to `true`, Search Guard will send a response with status `UNAUTHORIZED` (401) back to the client, and set the `WWW-Authenticate` header to `Basic realm="Search Guard"`. If the client is accessing the Search Guard secured cluster with a browser, this will trigger the authentication dialog and the user is prompted to enter username and password. 

If `challenge` is set to `false`, and no `Authorization` header field is set, Search Guard will **not** sent a `WWW-Authenticate` response back to the client, and authentication will fail. You may want to use this setting if you have another challenging `http_authenticator` in your configured authentication domains (note that there can always be only be one challenging authenticator).  One such scenario is when you plan to use Basic Authentication and Kerberos together, and set Kerberos to `challenging`. In that case, you can still use Basic Authentication, but only with pre-authenticated requests.