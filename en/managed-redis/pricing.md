---
editable: false
---
# Pricing for {{ mrd-name }}

{% include [currency-choice](../_includes/pricing/currency-choice.md) %}

{% include [pricing-status.md](../_includes/mdb/pricing-status.md) %}

## What goes into the cost of using {{ mrd-short-name }} {#rules}

The cost of {{ mrd-name }} usage is based on:

{% include [pricing-rules](../_includes/mdb/pricing-rules.md) %}

{% include [pricing-gb-size](../_includes/pricing-gb-size.md) %}

### DB host usage {#rules-hosts-uptime}

The cost is calculated for each hour of operation of the host in accordance with the allocated computing resources. Supported resource configurations are listed in the [{#T}](concepts/instance-types.md) section, prices for using vCPU and RAM — in the [Prices](#prices) section.

The minimum billing unit is a minute (for example, 1.5 minutes of host usage cost the same as 2 minutes). You are not charged for time when the {{ RD }} host is not performing its main functions.

### Disk space usage {#rules-storage}

The following is charged:

* Storage allocated for DB clusters.

    * Storage on non-replicated SSDs (`network-ssd-nonreplicated`) can only be ordered for clusters with three or more hosts running Intel Cascade Lake and Intel Ice Lake, in increments of 93 GB.

    * Storage on local SSDs (`local-ssd`) can only be ordered for clusters running Intel Broadwell and Intel Cascade Lake with three or more hosts, in increments of 100 GB.

    For more information about platform-specific storage limitations, see [{#T}](./concepts/storage.md).

* Space used by DB backups in excess of the storage size requested for the cluster.
    * Backups are stored free of charge as long as the combined size of the DB and all backups is less than the selected storage volume.
    * The requested storage size is allocated for each host, so the number of hosts in the cluster does not affect the free backup size.

The cost is specified for one month of use. The minimum billing unit is 1 GB per minute (for example, storing 1 GB for 1.5 minutes costs the same as storing 1 GB for 2 minutes).


## Pricing {#prices}


All prices are shown without VAT.

### Host computing resources {#prices-hosts}




{% include [usd-host.md](../_pricing/managed-redis/usd-host.md) %}

### Storage and backups {#prices-storage}




{% include [usd-storage.md](../_pricing/managed-redis/usd-storage.md) %}

### Outgoing traffic {#prices-traffic}




{% include notitle [usd-egress-traffic.md](../_pricing/usd-egress-traffic.md) %}

