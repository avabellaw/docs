Install tpm2 tools (and therefore tpm2 pool) using:
```sudo dnf builddep tpm2-tools```


RSA 3072-bit eqvuivalent to ed25519.

## list

tpm2_ptool listobjects --label sshtok

## del

tpm2_ptool objdel [id]

# export

tpm2_ptool export [--id/--label] --format pem


https://jade.fyi/blog/tpm-ssh/

tpm2_pool init without path to store in default ~/.tpm2_pkcs11

```bash
tpm2_ptool init
tpm2_ptool addtoken --pid 1 --label sshtok --sopin SUPER_VISOR_PIN --userpin USER_PIN
tpm2_ptool import --label sshtok --key-label my-ssh-key --userpin [USER PIN] \
    --privkey /tmp/crypto/tpm_rsa --algorithm rsa/--algorithm=rsa4096
```

## Error creating key 

rsa3072 worked for me which is convenient as it's all I need. 4096 doesnt.

tpm2_testparms ecc256
tpm2_testparms rsa2048
tpm2_testparms rsa4096


```TPM2_PKCS11_SO=/usr/lib64/pkcs11/libtpm2_pkcs11.so```

**pull out public keys** ssh-keygen -D $TPM2_PKCS11_SO

**add provider to ssh config** 
```bash
cat <(echo "PKCS11Provider $TPM2_PKCS11_SO") ~/.ssh/config \
    | tee ~/.ssh/config
```

https://github.com/tpm2-software/tpm2-pkcs11/blob/master/docs/INITIALIZING.md
