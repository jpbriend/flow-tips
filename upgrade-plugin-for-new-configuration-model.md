This page describes how to upgrade a CloudBees CD plugin to the new Plugin layout and make the plugin compatible with the Plugin Configuration migration wizard available in CD version 10.3+.

## Symptoms

The Plugin Configuration migration procedure fails with the following error:
```
The plugin does not support new confiugrations objects
```

## Solution

The plugin has to be upgraded.
* Check the current layout version in your plugin: in the file `config/flowpdf.yaml`, the field `flowpdf-plugin-spec` is supposed to be `1`.
* Update your `pdk` binary to the [latest version](https://github.com/electric-cloud-community/flowpdf).
* Execute the following command line to upgrade your plugin: `pdk generate plugin --upgrade --force-cleanup`
* Check in the file `config/flowpdf.yaml` if the `flowpdf-plugin-spec` has been updated to `2`.
* If the field is still in version `1`, manually edit the file and change the version to `2`. Rerun `pdk generate plugin --upgrade --force-cleanup`.
* Multiple files should have been deleted (especially in the `dsl/procedures` folder). This is normal: the new layout removes multiple useless files.
* If you have never updated the `ec_setup.pl` file (at the root of the plugin), you need to delete it. If you have written custom code, you need to move it somewhere else (contact CD Plugin team for help).
* You can now build the plugin using `pdk build`.

This new generated version of your plugin will be compatible with the new Plugin Configuration migration procedure.
