# Storage Classes

## Static provisioning

![static prov](../images/40_static_provisioning.png)
Every time you have to create a disk in the cloud provider. Would be nice to have this automated. That's why Storage Classes & Dynamic provisioning.

## Dynamic provisioning

![Dynamic prov](../images/40_dynamic_provi.png)

You don't have to create PV manually anymore!

## Storage classes

![Storage classes](../images/40_storage_classes.png)
![Storage classes2](../images/40_storage_classes2.png)

## VolumeBindingMode
If storage class has VolumeBindingMode = _WaitForFirstConsumer_ it delays binding and provisioning of a Persistent Volume until pod using the PVC is created.