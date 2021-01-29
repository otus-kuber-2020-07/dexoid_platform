~~~ ➜  ~ helm status vault
NAME: vault
LAST DEPLOYED: Thu Jan 28 17:31:44 2021
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None
NOTES:
Thank you for installing HashiCorp Vault!

Now that you have deployed Vault, you should look over the docs on using
Vault with Kubernetes available here:

https://www.vaultproject.io/docs/


Your release is named vault. To learn more about the release, try:

  $ helm status vault
  $ helm get manifest vault
~~~

~~~
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-0 -- vault operator init --key-shares=1 --key-threshold=1
Unseal Key 1: humy3pQJBU2FFMg9UO6PuyyAW218VZdMd3EUru8HkUs=

Initial Root Token: s.1UTisdmRhh7onkCgyr996Rq8

Vault initialized with 1 key shares and a key threshold of 1. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 1 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 1 key to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
~~~

~~~
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-2 -- vault status
Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.6.1
Storage Type           consul
Cluster Name           vault-cluster-9d402cfd
Cluster ID             86f983be-bf55-d48d-9dcc-dce086baed9f
HA Enabled             true
HA Cluster             https://vault-0.vault-internal:8201
HA Mode                standby
Active Node Address    http://10.4.0.5:8200
~~~

~~~
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-0 -- vault login
Token (will be hidden):
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                s.1UTisdmRhh7onkCgyr996Rq8
token_accessor       esop7GVnmbUmqPpu3CYrVk8T
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
~~~

~~~
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-0 -- vault auth list
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_d90ab727    token based credentials
~~~

~~~
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-0 -- vault read otus/otus-ro/config
Key                 Value
---                 -----
refresh_interval    768h
password            bfhjchfbjcec
username            otus
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-0 -- vault kv get otus/otus-rw/config
====== Data ======
Key         Value
---         -----
password    bfhjchfbjcec
username    otus
~~~

~~~
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-0 -- vault auth list
Path           Type          Accessor                    Description
----           ----          --------                    -----------
kubernetes/    kubernetes    auth_kubernetes_fe87f5bd    n/a
token/         token         auth_token_d90ab727         token based credentials
~~~

~~~
<html>
<body>
<p>Some secrets:</p>
<ul>
<li><pre>username: otus</pre></li>
<li><pre>password: bfhjchfbjcec</pre></li>
</ul>

</body>
</html>
~~~

~~~
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-0 -- vault write pki_int/issue/example-dot-ru common_name="gitlab.example.ru" ttl="24h"
Key                 Value
---                 -----
ca_chain            [-----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUTGQC20IQoi1rBkM95+YC9wDjTmswDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMTAxMjkxMzQ2MDRaFw0yNjAx
MjgxMzQ2MzRaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANYrd2YA0yzG
NpPTA9XKUZTNYB0uT0XglhIxEmjrmycpwB2fVvzg6rm1fh/wrqZ6Pw8L78Ofkgqm
G4/1DPb/9zk8TiYXEhkiJkep1M7C0V+33WSW0sCmzwWuxMrFP8CjaIOBDU0kKL35
9Vnqma4xzfy6sUHpE/B/881AoX7ExymC86fup/RcssYhao8WTPanqAqMB65Vnwrv
8qUCnyYbz1FA6M34a9ZB8/nA5vj/onG4H7039mCf/xNMirMHYcggxRxe99AFi+Dd
bknt4y1sf2Q3AGbf9OhOHyebx8+YNIUtIVJJz2c8S/VFYpLgUfrljjYfU+ZoaWgl
XPXu4YL61lcCAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQUvzvwBpneXlBYcxuBT0+dMLYSkoUwHwYDVR0jBBgwFoAU
H0JYjPAgnFZ2JnIlip9eQV5m5l0wNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
f5bD1MiAyj+YpetWDiZLJrcoankfAW59y3ueD8NsKQhLjomPaXfbXO3C0d2QkDzA
lTP2JR219v0QqMJP1UIxxJ5A7N3W+SmXZzPmbB24PfYbWlgWDsIOSW5xqLtP5k+I
13vGz1kRwlYCoZ5Rt4DaqzuptgexrDBNucPIj8jCvhaJl3tN2/fj02bDLNBLWTu8
rSEuYOgjluLHgADf1n4IQS2kALb/X0Gx0JOpqg1ywUZ83rhMhMi0lDm7A9OW+A52
FN2nNB/Nl4Jwp/sl2cL1Sf6IiK0/I1zrNMHBLUbZVu5zroTH07rK16cTrYcd+IED
KjIACobfQgMklwmPDRhwTQ==
-----END CERTIFICATE-----]
certificate         -----BEGIN CERTIFICATE-----
MIIDZzCCAk+gAwIBAgIUFAsN2HsJF9ABrJAjIiz94Nw9LE8wDQYJKoZIhvcNAQEL
BQAwLDEqMCgGA1UEAxMhZXhhbXBsZS5ydSBJbnRlcm1lZGlhdGUgQXV0aG9yaXR5
MB4XDTIxMDEyOTEzNDg1NloXDTIxMDEzMDEzNDkyNlowHDEaMBgGA1UEAxMRZ2l0
bGFiLmV4YW1wbGUucnUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDB
C0OKctKha6jTJsqVUnS3MMleWxHP3DMo8ZCbXF5c3c9ux6yCA6x6k+rp2bJqclC6
J7KkXn35kL1/kmZthtbiig3drYS3/taq59EiwLWbm0dLi1l1/oqWOQtoDCuuWAP6
G9FkzPRAgoV3Hjs6weHUUnoGYvDhGQaEo1ASLWQIU65Rz0FAOWRQfIQuowVKd7V+
Nq9fCxpFYICZBCvnQdc9PLMTtCIFgpKQavm0/gAAfHt6EH3tc0mUJMO5oaqLibyb
eXnHsuvvdd+lMKfPBJfHFt1cxCyd8ORi/4w+Hv4TppVeUAlqdMVJ0ZDGUdS471mN
onmhc4hN8aVaNmAOdhgHAgMBAAGjgZAwgY0wDgYDVR0PAQH/BAQDAgOoMB0GA1Ud
JQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQUSo3t8NA3YaU2xWjH
/VDwGjDz44IwHwYDVR0jBBgwFoAUvzvwBpneXlBYcxuBT0+dMLYSkoUwHAYDVR0R
BBUwE4IRZ2l0bGFiLmV4YW1wbGUucnUwDQYJKoZIhvcNAQELBQADggEBAAy47htY
J9xveUUW9MQqx/HiYNjHpi7JcQVZUUkO/KPML7qpOJ79Ni0RCB4G2TUov1PTbAly
2e34MPCOvQdqAMa8hERUxcTf3UxcsURM08T0OpncjDMbLb2BaieVU9cLdGF+hthE
kLiEn3L7Rbgoa0UnBnVdYrFfP7ne55/q+eNs7KmozLBv+8V3B3SkDxPosTgDmz8c
uN2IQsX1LqYap4iEc4uVkcWJDRI8goGYX/SE7oK++hfntsx3WLhT0/54768qCdz9
h+wbpiKpYep/8KrRnE45UKfKSEkLsukiQD8GnqLZIBrsn4mD0czOHBZkkZ8odhOY
QcdGAJIuaSA8TpM=
-----END CERTIFICATE-----
expiration          1612014566
issuing_ca          -----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUTGQC20IQoi1rBkM95+YC9wDjTmswDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMTAxMjkxMzQ2MDRaFw0yNjAx
MjgxMzQ2MzRaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANYrd2YA0yzG
NpPTA9XKUZTNYB0uT0XglhIxEmjrmycpwB2fVvzg6rm1fh/wrqZ6Pw8L78Ofkgqm
G4/1DPb/9zk8TiYXEhkiJkep1M7C0V+33WSW0sCmzwWuxMrFP8CjaIOBDU0kKL35
9Vnqma4xzfy6sUHpE/B/881AoX7ExymC86fup/RcssYhao8WTPanqAqMB65Vnwrv
8qUCnyYbz1FA6M34a9ZB8/nA5vj/onG4H7039mCf/xNMirMHYcggxRxe99AFi+Dd
bknt4y1sf2Q3AGbf9OhOHyebx8+YNIUtIVJJz2c8S/VFYpLgUfrljjYfU+ZoaWgl
XPXu4YL61lcCAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQUvzvwBpneXlBYcxuBT0+dMLYSkoUwHwYDVR0jBBgwFoAU
H0JYjPAgnFZ2JnIlip9eQV5m5l0wNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
f5bD1MiAyj+YpetWDiZLJrcoankfAW59y3ueD8NsKQhLjomPaXfbXO3C0d2QkDzA
lTP2JR219v0QqMJP1UIxxJ5A7N3W+SmXZzPmbB24PfYbWlgWDsIOSW5xqLtP5k+I
13vGz1kRwlYCoZ5Rt4DaqzuptgexrDBNucPIj8jCvhaJl3tN2/fj02bDLNBLWTu8
rSEuYOgjluLHgADf1n4IQS2kALb/X0Gx0JOpqg1ywUZ83rhMhMi0lDm7A9OW+A52
FN2nNB/Nl4Jwp/sl2cL1Sf6IiK0/I1zrNMHBLUbZVu5zroTH07rK16cTrYcd+IED
KjIACobfQgMklwmPDRhwTQ==
-----END CERTIFICATE-----
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAwQtDinLSoWuo0ybKlVJ0tzDJXlsRz9wzKPGQm1xeXN3Pbses
ggOsepPq6dmyanJQuieypF59+ZC9f5JmbYbW4ooN3a2Et/7WqufRIsC1m5tHS4tZ
df6KljkLaAwrrlgD+hvRZMz0QIKFdx47OsHh1FJ6BmLw4RkGhKNQEi1kCFOuUc9B
QDlkUHyELqMFSne1fjavXwsaRWCAmQQr50HXPTyzE7QiBYKSkGr5tP4AAHx7ehB9
7XNJlCTDuaGqi4m8m3l5x7Lr73XfpTCnzwSXxxbdXMQsnfDkYv+MPh7+E6aVXlAJ
anTFSdGQxlHUuO9ZjaJ5oXOITfGlWjZgDnYYBwIDAQABAoIBAQCTVyqM8IjNheOM
jk4PXKkS7Vy09OOKeDJgLMhHnF+1l+LsVeDYNz+dK2sgY/Vmg6wT+ZrDFBAovI22
YiJltIYs0Fw292qWGR71wQwB735trz3ibhCxG7/hWtbW6nsojaszaaoPutMurbB6
s4tXlIXuTakvWLMry7MK56+EwDpak1ipENLoY+JBsg1oml4coY7NqlFG6rLq9kf/
hSGo8khy3DgTjZNJrPahgQxaosF7syHQ/JU07PpgDb0G39P4y8nLe8PR6a5DmKeD
cMw+Fo9MGv/eUXBcXdh0l0/jqWV560hxaCslmrfJ9Fbd/GeUglyOAGAaB2WbWV2h
Hbl1G5bBAoGBAOY/Oalvg3dGSrnz8omo+Nt+aafNf9BBAv9QRQt/7wdUOa2s+gqj
nfUhFF9Q5XNsiTxdh/Ro0HYo/TAMWxeule+MRpvzeROVw10bXuzrPNlTy5gdDI2y
AAJ4ce+5Jro8S+t/xAoH6lrsskWgUPXY8LoB3ih00O/KXWdRpuEmxfyhAoGBANai
yZxqRdd5sL4pwrwR2CFXRkrOPz5r9wKRNFb8LnvOzhPHStSARCSpVy1Q3MmR9MjL
370/KlcCMnecz/Kj9hCwvFSXIwDUxuN9jNhrm+hXeMyNkYUg8LrYJTxJ4fsudhFa
lce0MQpaxHKPMb5BFe35pHFNWduMg9WoU+koV2unAoGAffXGyM/NvxnAVbyEShUb
l5CvonJE4xw91IkITMM/c+X66+1+mQVrq6feKkrqS9vrtA+DCg2xRYMlB/0qdGk5
c2RtWHG5IiLDTxNjOf9zOX2jGuOBc58WbEOSKVBERIwqCHrMN6Se7AEjgf2zbNuy
zj+pVr6ZcyVlNdmwjuoUVCECgYEAq9t+aNKjB4w1m54ega4cC14xN1Z7R0T1bEXN
zdRwd8u4MBgD77CgX2aEGhHEJ1XJ1TmA9avs/+wPcrzJuHPWY7jPoz+81D6K3b46
l1Naa6gWXlsAaMNeXY0SC3rxCclC2lRY16WNGshgwYWsg7TjlT+tjtcrmuKg8OOu
BGO708MCgYEAnoIJqzDPFHZODNz2KmcHkNqz+lnEkl3SKmjSmlSlQaxvsnZFHtsb
CU66tSZNo7FrDPbgiR0EHOX0bsznszyopT9L6fM1NrCLCJ4tfxGhFlctJHIS7W43
8EPcVRcjwe89US8Gfdr6V4p+Cg2RwlUjkssSh1oaNdyLeCYG+PvoxoM=
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       14:0b:0d:d8:7b:09:17:d0:01:ac:90:23:22:2c:fd:e0:dc:3d:2c:4f
➜  kubernetes-vault git:(kubernetes-vault) ✗ kubectl exec -it vault-0 -- vault write pki_int/revoke serial_number="14:0b:0d:d8:7b:09:17:d0:01:ac:90:23:22:2c:fd:e0:dc:3d:2c:4f"
Key                        Value
---                        -----
revocation_time            1611928198
revocation_time_rfc3339    2021-01-29T13:49:58.07292034Z
~~~