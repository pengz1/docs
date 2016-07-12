Event Notification
------------------

Node Events
~~~~~~~~~~~~~~~~~~~~

All node related events could be retrieved by subscribing amqp's `node.alert.<node_id>` queue.

An example of listening node events using tool 'sniff.js' located at https://github.com/RackHD/on-tools/blob/master/dev_tools/README.md.

.. code-block:: Bash

    $ sudo node sniff.js "on.events" "node.alert.#"

An example of event message format is below:

.. code-block:: JSON

    {
        nodeId: '577d758e5bee93f307b9c062',
        nodeType: 'compute',
        state: 'discovered'
    }

The attributes of the node event format:

========= ====== =================================
Attribute Type   Description
========= ====== =================================
nodeId    String The node's unique identifier
nodeType  String The node type could be `compute`, `pdu`, `switch`, `mgmt`, `enclosure`
state     String The node's state event pre-defined by RackHD, See events_ for more details.
========= ====== =================================

.. _events:

All node state events:

+---------------+-----------+------------+----------------------------------+--------------------------------+
| Event `state` | Frequency | Supported  | Cases                            | When Triggered                 |
|               |           | `nodeType` |                                  |                                |
+===============+===========+============+==================================+================================+
| discovered    | one-shot  | compute,   | Event occurs in node's           | Triggered when node is         |
|               |           | switch,    | discovery process,it has         | created and all catalog        |
|               |           | pdu,       | two cases:                       | jobs are finished              |
|               |           | mgmt       |                                  |                                |
|               |           |            | - Automatic discovery            |                                |
|               |           |            | - Passive discovery by           |                                |
|               |           |            |   post a node by REST API        |                                |
+---------------+-----------+------------+----------------------------------+--------------------------------+
| identified    | one-shot  | compute,   | Event occurs after node has      | Triggered when node's SKU is   |
|               |           | switch,    | been discovered, and SKU is      | created or updated. and        |
|               |           | pdu,       | assigned, and obm setting        | obm is not set or removed      |
|               |           | mgmt       | doesn't exist                    |                                |
+---------------+-----------+------------+----------------------------------+--------------------------------+
| managed       | one-shot  | compute,   | Event occurs after node has      | Triggered when node's obm      |
|               |           | switch,    | been discovered and obm          | setting is set or updated      |
|               |           | pdu,       | setting exist,no matter SKU      |                                |
|               |           | mgmt       | is assigned or not               |                                |
+---------------+-----------+------------+----------------------------------+--------------------------------+
| removed       | one-shot  | compute,   | Event occurs when node is        | Triggered when node's all      |
|               |           | switch,    | deleted by REST API              | information are deleted,       |
|               |           | pdu,       |                                  | it includes node,catalogs,     |
|               |           | mgmt,      |                                  | relationships with other       |
|               |           | enclosure  |                                  | models, etc.                   |
+---------------+-----------+------------+----------------------------------+--------------------------------+

