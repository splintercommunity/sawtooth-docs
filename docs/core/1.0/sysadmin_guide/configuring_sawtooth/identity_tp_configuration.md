# Identity Transaction Processor Configuration File

The Identity transaction processor configuration file specifies the
validator endpoint connection to use.

If the config directory contains a file named `identity.toml`, the
configuration settings are applied when the transaction processor
starts. Specifying a command-line option will override the setting in
the configuration file.

Note: By default, the config directory is /etc/sawtooth/. See
`path_configuration_file`{.interpreted-text role="doc"} for more
information.

An example configuration file is in
`/sawtooth-core/families/identity/packaging/identity.toml.example`. To
create a Identity transaction processor configuration file, copy the
example file to the config directory and name it `identity.toml`. Then
edit the file to change the example configuration options as necessary
for your system.

The `identity.toml` configuration file has the following option:

-   `connect` = \"[URL]{.title-ref}\"

    Identifies the URL of a running validator. Default:
    `tcp://localhost:4004`. For example:

    ``` none
    connect = "tcp://localhost:4004"
    ```

<!--
  Licensed under Creative Commons Attribution 4.0 International License
  https://creativecommons.org/licenses/by/4.0/
-->
