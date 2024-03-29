# PoET SGX Enclave Configuration File

This configuration file specifies configuration settings for a PoET SGX
enclave.

If the config directory contains a file named `poet_enclave_sgx.toml`,
the configuration settings are applied when the component starts. (By
default, the config directory is `/etc/sawtooth/`; see
`path_configuration_file`{.interpreted-text role="doc"} for more
information.) Specifying a command-line option will override the setting
in the configuration file.

An example configuration file is in
`/sawtooth-core/consensus/poet/sgx/packaging/poet_enclave_sgx.toml.example`.
To create a PoET SGX enclave configuration file, copy the example file
to the config directory and name it `poet_enclave_sgx.toml`. Then edit
the file to change the example configuration options as necessary for
your system.

::: note
::: title
Note
:::

See `../configure_sgx`{.interpreted-text role="doc"} for an example of
changing settings in `poet_enclave_sgx.toml` when configuring Sawtooth
with the SGX implementation of PoET.
:::

The `poet_enclave_sgx.toml` configuration file has the following
options:

-   `spid` = \'[string]{.title-ref}\'

    Specifies the Service Provider ID (SPID), which is linked to the key
    pair used to authenticate with the attestation service. Default:
    none. The SPID value is a 32-digit hex string tied to the enclave
    implementation; for example:

    ``` none
    spid = 'DEADBEEF00000000DEADBEEF00000000'
    ```

-   `ias_url` = \'[URL]{.title-ref}\'

    Specifies the URL of the Intel Attestation Service (IAS) server.
    Default: none. Note that the URL shown in
    `poet_enclave_sgx.toml.example` is an example server for debug
    enclaves only:

    ``` none
    ias_url = 'https://test-as.sgx.trustedservices.intel.com:443'
    ```

-   `spid_cert_file` = \'[/full/path/to/certificate.pem]{.title-ref}\'

    Identifies the PEM-encoded certificate file that was submitted to
    Intel in order to obtain a SPID. Default: none. Specify the full
    path to the certificate file. This pem file can be created from
    `cert.crt` and `cert.key` files with this command:

    ``` console
    $ cat cert.crt cert.key > cert.pem
    ```

    Or from `cert.pfx` file with following command:

    ``` console
    $ openssl pkcs12 -in cert.pfx -out cert.pem -nodes
    ```

<!--
  Licensed under Creative Commons Attribution 4.0 International License
  https://creativecommons.org/licenses/by/4.0/
-->
