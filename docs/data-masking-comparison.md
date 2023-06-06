# Compare the data masking component to the data masking plugin

Percona Server for MySQL 8.0.33, the data masking component adds the data masking component. Either the component or the plugin extends the server's functionality, but the architecture of a plugin is different than the component. Plugins have the following limitations:

* Interact only with the server and do not interact with other plugins
* Dependencies can be difficult to initialize 
* Require a running server

The component supports more arguments to the `gen_rnd_email()` function. The main differences between the component and plugin are the following:

| Component | Plugin |
|--- | --- |
| Supports a service interface and loadable functions | Supports an interface only to loadable functions |
| Allows multi-byte character sets for the general-purpose masking functions | Does not allow multi-byte character sets |
| Supports masking PAN, SSN, IBAN, UUID, Canada SIN, and UK NIN | Supports masking PAN and SSN |
| Generates random email, US phone, PAN, SSN, IBAN, UUID, Canada SIN, and UK NIN data |  Generates random email, US phone, PAN, and SSN |
| Supports persisting substitution dictionaries in the database | Supports persisting substitution dictionaries in a file |
| Supports a dedicated privilege to manage the dictionaries | Requires a file privilege |
| Supports automating the loadable function registration or de-registration during install or uninstall | Does not support automating the loadable function registration or de-registration during install or uninstall |
