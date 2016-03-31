# Apache Analytics with Pig

This bundle is a 6 node cluster designed to scale out. Built around Apache
Hadoop components, it contains the following units:

  * 1 NameNode (HDFS)
  * 1 ResourceManager (YARN)
  * 3 Slaves (DataNode and NodeManager)
  * 1 Pig
    - 1 Plugin (colocated on the Pig unit)


## Usage

This bundle models an Apache Hadoop platform with Apache Pig available for
executing Pig Latin jobs. Deploy using juju-quickstart:

    juju quickstart apache-analytics-pig

See `juju quickstart --help` for deployment options, including machine
constraints and how to deploy a locally modified version of `bundle.yaml`.

Once deployment is complete, you can run Pig in two different modes:

### Local Mode

Run Pig in local mode on the Pig unit with the following:

    juju ssh pig/0
    pig -x local

### MapReduce Mode

MapReduce mode is the default for Pig. To run in this mode, ssh to the Pig unit
and run pig as follows:

    juju ssh pig/0
    pig


## Status and Smoke Test

The services provide extended status reporting to indicate when they are ready:

    juju status --format=tabular

This is particularly useful when combined with `watch` to track the on-going
progress of the deployment:

    watch -n 0.5 juju status --format=tabular

The charm for each core component (namenode, resourcemanager, pig)
also each provide a `smoke-test` action that can be used to verify that each
component is functioning as expected.  You can run them all and then watch the
action status list:

    juju action do namenode/0 smoke-test
    juju action do resourcemanager/0 smoke-test
    juju action do pig/0 smoke-test
    watch -n 0.5 juju action status

Eventually, all of the actions should settle to `status: completed`.  If
any go instead to `status: failed` then it means that component is not working
as expected.  You can get more information about that component's smoke test:

    juju action fetch <action-id>


## Scale Out Usage

This bundle was designed to scale out. To increase the amount of Compute
Slaves, you can add units to the compute-slave service. To add one unit:

    juju add-unit compute-slave

Or you can add multiple units at once:

    juju add-unit -n4 compute-slave


## Contact Information

- <bigdata@lists.ubuntu.com>


## Help

- [Juju mailing list](https://lists.ubuntu.com/mailman/listinfo/juju)
- [Juju community](https://jujucharms.com/community)
