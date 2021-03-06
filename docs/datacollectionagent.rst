.. This file has been automatically generated, from _templates/datacollectionagent.rst.rst.j2. Don't modify it

Data Collection Agent
=====================

Configuration
-------------

**data/hosts.yaml**
In data/hosts.yaml you need to provide the list of devices you want to pull information from
For each device, you need to indicate the name ane one or multiple *tags* (at least one).
Tags will be used later to know which credentials should be used for this device and which commands need to be executed

.. code-block:: yaml

  <hostA>: <tag1> <tag4>
  <hostB>: <tag1> <tag4>
  <hostC>: <tag2> <tag4> <tag5>
  <hostD>: <tag1> <tag4>  <--- Those tags relate the Hosts with the credentials and the commands to use with

Example

.. code-block:: yaml

  mx-edge011: edge mx madrid bgp mpls
  mx-agg011: agg mx madrid bgp isis
  qfx-agg022: agg qfx munich bgp
  qfx5100-02: tor qfx madrid isis


.. NOTE::
  The default configuration assume that hosts defined in hosts.yaml can be resolved with DNS
  if your hosts doesn't have DNS entry, it's possible to indicate the IP address in the hosts.yaml file instead of the name

  `192.168.0.1: edge mx madrid bgp mpls`

  To avoid using Ip addresses in the dashboard, you can use the device hostname defined in the configuration
  instead of the value define in hosts.yaml by setting the parameter **use_hostname** to true in **open-nti.variables.yaml**
  `use_hostname: True`


**data/credentials.yaml**

You need to provide at least one credential profile for your devices

.. code-block:: yaml

  jdi_lab:
    username: '*login*'         (Single quote is to force to be imported as string)
    password: '*password*'      (Single quote is to force to be imported as string)
    method: password            (other supported methods 'key' and 'enc_key' for ssh Key-Based Authentication)
    key_file: /opt/open-nti/data/*key_file* (optional: only appies if method key or enc_key is used, it must be located at data directory)
    tags: tag1 tag2

**data/commands.yaml**

.. code-block:: yaml

  generic_commands:  <--- You can name the group as best fits you
     commands: |
        show version | display xml  <--- There is no limit on how many commands can be added into a group
        show isis statistics | display xml <-- Before adding a command, confirm that there is a related parser
        show system buffers
        show system statistics icmp | display xml
        show route summary | display xml
     tags: tag1 tag2

Execution periodic
-------------------
To collect data periodically with the **Data Collection Agent**, you need to setup a cron job inside the container.
As part of the project, open-nti is providing some scripts to easily add/remove cron jobs **inside** the container **from** the host.

Scripts provided:
 - **make cron-add**: Create a new cron job inside the container
 - **make cron-show**: Show all cron jobs configured inside the container
 - **make cron-delete**: Delete a cron job inside the container for a specific tag

To start cron job to execute commands specified above for specific tag every minute:

.. code-block:: text

  make cron-add TAG=lab

To start cron job for more than one tag at the same time:

.. code-block:: text

  make cron-add TAG='lab prod'

To start cron job to execute commands specified above for specific tag every 5 minutes:

.. code-block:: text

  make cron-add TAG=tag1 TIME=5m

To start cron job to execute commands specified above for specific tag every hour:

.. code-block:: text

  make cron-add TAG=tag1 TIME=1h


To stop cron job for specific tag:

.. code-block:: text

  make cron-show TAG=tag1

.. NOTE::
  If you want to configure the cron job yourself, open-nti use this command:
  ``/usr/bin/python /opt/open-nti/open-nti.py -s --tag <tag>``

Junos Parsers
-------------

.. raw:: html

    <table style="width:100%;white-space:normal;" border=1 class="docutils jnpr-table" >
      <thead>
        <tr>
           <th class="jnpr-name">Parser</th>
           <th class="jnpr-desc">Description</th>
           <th class="jnpr-author">Author</th>
        </tr>
      <thead>
      <tbody>

        <tr>
          <td class="jnpr-name">rtsockmon.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-bfd-session-summary.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-bgp-neighbor-10.255.0.206.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-bgp-summary.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-chassis-routing-engine.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-firewall.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-interfaces-media.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-isis-statistics.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-mpls-lsp.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-network-access-aaa-radius-servers-detail.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-network-access-aaa-statistics-address-assignment-pool.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-pfe-statistics-traffic.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-pppoe-statistics.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-route-summary.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-services-l2tp-summary.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-services-nat-pool-detail.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-services-rpm-probe-results.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-services-stateful-firewall-flow-analysis.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-services-stateful-firewall-subscriber-analysis.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-services-video-monitoring-mdi-flow-fpc-slot-1.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-snmp-statistics.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-subscribers-summary-port.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-subscribers-summary.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-system-buffers.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-system-processes-extensive.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-system-resource-monitor-summary.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-system-statistics-icmp.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-system-virtual-memory.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-task-accounting.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-task-io.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

        <tr>
          <td class="jnpr-name">show-version.parser.yaml</td>
          <td class="jnpr-desc">None</td>
          <td class="jnpr-author">anomymous</td>
        </tr>

      <tbody>
    </table>
