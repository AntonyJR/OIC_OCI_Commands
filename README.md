# Handy OCI Commands for Use with OIC

## List Instances in Udpdate Window 1
OIC uses the OIC_UPGRADE_WINDOW1 tag to identify which instances are to be included in the first update window of the quarterly feature release. See [Choosing Your Update Window][Update Window Blog] for details

The following command can be used to list all the OIC instances in all the regions selected for update window 1.
```shell
for REGION in `oci iam region list | jq -r '.data[] | .name'`; \
    do \
        for INSTANCE in `oci --region $REGION search resource structured-search --query-text "query integrationinstance resources where (freeformTags.key = 'OIC_UPDATE_WINDOW1')" | jq --arg REGION $REGION '.data.items[] | { "region":$REGION, "compartment-id", "identifier", "display-name" }'`; \
            do \
                echo $INSTANCE; \
            done;\
    done
```
*Note* that if you lack permissions on a region you may get an error but the script will continue to execute.

[Update Window Blog]: https://blogs.oracle.com/integration/choosing-your-update-window.
