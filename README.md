# Overview

This bundle provides an example of how to deploy a CockroachDB cluster via charms written in the operator framework.

# Usage

First, update all submodules recursively to get charm sources.

```
git submodule update --init --recursive
```

Then:

* Choose a virtual IP to be used a on a subnet where haproxy will be deployed. It is important to pre-allocate one
IP address such that there are no IP conflicts with other hosts;
* Add it to your bundle instead of a placeholder.

```
    haproxy:
        charm: ./haproxy-operator
        options:
            # replace this with your own pre-allocated virtual IP.
            virtual-ip: 192.0.2.100
```

```
juju deploy ./bundle.yaml
```

# Accessing CockroachDB via a VIP

Download CockroachDB to your machine to use it as a client:

```
wget -qO- https://binaries.cockroachdb.com/cockroach-v19.2.2.linux-amd64.tgz | tar  xvz
cd cockroach-v19.2.2.linux-amd64
```

Use `cockroach sql` to access the database via the SQL interface.

```
cockroach sql --insecure --url postgres://<virtual-ip>
```

# Limitations

* TLS and user authentication is to be implemented;
* haproxy units must resize on a single L2 broadcast domain to use VIPs (CockroachDB units do not have to).
