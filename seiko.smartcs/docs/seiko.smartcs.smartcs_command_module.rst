.. _seiko.smartcs.smartcs_command_module:


*****************************
seiko.smartcs.smartcs_command
*****************************

**Run commands on remote devices running SmartCS**


Version added: 1.3.0

.. contents::
   :local:
   :depth: 1


Synopsis
--------
- Sends arbitrary commands to an SmartCS node and returns the results read from the device. This module includes an argument that will cause the module to wait for a specific condition before returning or timing out if the condition is not met. This module does not support running commands in configuration mode. Please use :ref:`smartcs_config <smartcs_config_module>` to configure SmartCS.




Parameters
----------

.. raw:: html

    <table  border=0 cellpadding=0 class="documentation-table">
        <tr>
            <th colspan="1">Parameter</th>
            <th>Choices/<font color="blue">Defaults</font></th>
            <th width="100%">Comments</th>
        </tr>
            <tr>
                <td colspan="1">
                    <div class="ansibleOptionAnchor" id="parameter-"></div>
                    <b>commands</b>
                    <a class="ansibleOptionLink" href="#parameter-" title="Permalink to this option"></a>
                    <div style="font-size: small">
                        <span style="color: purple">list</span>
                         / <span style="color: purple">elements=raw</span>
                         / <span style="color: red">required</span>
                    </div>
                </td>
                <td>
                </td>
                <td>
                        <div>List of commands to send to the remote smartcs over the configured provider. The resulting output from the command is returned. If the <em>wait_for</em> argument is provided, the module is not returned until the condition is satisfied or the number of retries has expired. If a command sent to the device requires answering a prompt, it is possible to pass a dict containing <em>command</em>, <em>answer</em> and <em>prompt</em>. Common answers are &#x27;y&#x27; or &quot;\r&quot; (carriage return, must be double quotes). See examples.</div>
                </td>
            </tr>
            <tr>
                <td colspan="1">
                    <div class="ansibleOptionAnchor" id="parameter-"></div>
                    <b>interval</b>
                    <a class="ansibleOptionLink" href="#parameter-" title="Permalink to this option"></a>
                    <div style="font-size: small">
                        <span style="color: purple">integer</span>
                    </div>
                </td>
                <td>
                        <b>Default:</b><br/><div style="color: blue">1</div>
                </td>
                <td>
                        <div>Configures the interval in seconds to wait between retries of the command. If the command does not pass the specified conditions, the interval indicates how long to wait before trying the command again.</div>
                </td>
            </tr>
            <tr>
                <td colspan="1">
                    <div class="ansibleOptionAnchor" id="parameter-"></div>
                    <b>match</b>
                    <a class="ansibleOptionLink" href="#parameter-" title="Permalink to this option"></a>
                    <div style="font-size: small">
                        <span style="color: purple">string</span>
                    </div>
                </td>
                <td>
                        <ul style="margin: 0; padding: 0"><b>Choices:</b>
                                    <li>any</li>
                                    <li><div style="color: blue"><b>all</b>&nbsp;&larr;</div></li>
                        </ul>
                </td>
                <td>
                        <div>The <em>match</em> argument is used in conjunction with the <em>wait_for</em> argument to specify the match policy.  Valid values are <code>all</code> or <code>any</code>.  If the value is set to <code>all</code> then all conditionals in the wait_for must be satisfied.  If the value is set to <code>any</code> then only one of the values must be satisfied.</div>
                </td>
            </tr>
            <tr>
                <td colspan="1">
                    <div class="ansibleOptionAnchor" id="parameter-"></div>
                    <b>retries</b>
                    <a class="ansibleOptionLink" href="#parameter-" title="Permalink to this option"></a>
                    <div style="font-size: small">
                        <span style="color: purple">integer</span>
                    </div>
                </td>
                <td>
                        <b>Default:</b><br/><div style="color: blue">10</div>
                </td>
                <td>
                        <div>Specifies the number of retries a command should by tried before it is considered failed. The command is run on the target device every retry and evaluated against the <em>wait_for</em> conditions.</div>
                </td>
            </tr>
            <tr>
                <td colspan="1">
                    <div class="ansibleOptionAnchor" id="parameter-"></div>
                    <b>wait_for</b>
                    <a class="ansibleOptionLink" href="#parameter-" title="Permalink to this option"></a>
                    <div style="font-size: small">
                        <span style="color: purple">list</span>
                         / <span style="color: purple">elements=string</span>
                    </div>
                </td>
                <td>
                </td>
                <td>
                        <div>List of conditions to evaluate against the output of the command. The task will wait for each condition to be true before moving forward. If the conditional is not true within the configured number of retries, the task fails. See examples.</div>
                        <div style="font-size: small; color: darkgreen"><br/>aliases: waitfor</div>
                </td>
            </tr>
    </table>
    <br/>


Notes
-----

.. note::
   - Tested against SmartCS NS-2250 System Software Ver 2.1



Examples
--------

.. code-block:: yaml

    - name: run show version on remote devices
      seiko.smartcs.smartcs_command:
        commands: show version

    - name: run show version and check to see if output contains NS-2250
      seiko.smartcs.smartcs_command:
        commands: show version
        wait_for: result[0] contains NS-2250

    - name: run multiple commands on remote nodes
      seiko.smartcs.smartcs_command:
        commands:
        - show version
        - show ipinterface

    - name: run multiple commands and evaluate the output
      seiko.smartcs.smartcs_command:
        commands:
        - show version
        - show ipinterfaces
        wait_for:
        - result[0] contains NS-2250
        - result[1] contains lo

    - name: run commands that require answering a prompt
      seiko.smartcs.smartcs_command:
        commands:
        - command: 'copy startup 2 to startup 4'
          prompt: 'Do you really want to copy external startup1 to external startup3 [y/n] ? '
          answer: 'y'
        - command: 'clear startup 2'
          prompt: 'Do you really want to clear external & internal startup2 [y/n] ? '
          answer: "y"



Return Values
-------------
Common return values are documented `here <https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values>`_, the following are the fields unique to this module:

.. raw:: html

    <table border=0 cellpadding=0 class="documentation-table">
        <tr>
            <th colspan="1">Key</th>
            <th>Returned</th>
            <th width="100%">Description</th>
        </tr>
            <tr>
                <td colspan="1">
                    <div class="ansibleOptionAnchor" id="return-"></div>
                    <b>failed_conditions</b>
                    <a class="ansibleOptionLink" href="#return-" title="Permalink to this return value"></a>
                    <div style="font-size: small">
                      <span style="color: purple">list</span>
                    </div>
                </td>
                <td>failed</td>
                <td>
                            <div>The list of conditionals that have failed</div>
                    <br/>
                        <div style="font-size: smaller"><b>Sample:</b></div>
                        <div style="font-size: smaller; color: blue; word-wrap: break-word; word-break: break-all;">[&#x27;...&#x27;, &#x27;...&#x27;]</div>
                </td>
            </tr>
            <tr>
                <td colspan="1">
                    <div class="ansibleOptionAnchor" id="return-"></div>
                    <b>stdout</b>
                    <a class="ansibleOptionLink" href="#return-" title="Permalink to this return value"></a>
                    <div style="font-size: small">
                      <span style="color: purple">list</span>
                    </div>
                </td>
                <td>always apart from low level errors (such as action plugin)</td>
                <td>
                            <div>The set of responses from the commands</div>
                    <br/>
                        <div style="font-size: smaller"><b>Sample:</b></div>
                        <div style="font-size: smaller; color: blue; word-wrap: break-word; word-break: break-all;">[&#x27;...&#x27;, &#x27;...&#x27;]</div>
                </td>
            </tr>
            <tr>
                <td colspan="1">
                    <div class="ansibleOptionAnchor" id="return-"></div>
                    <b>stdout_lines</b>
                    <a class="ansibleOptionLink" href="#return-" title="Permalink to this return value"></a>
                    <div style="font-size: small">
                      <span style="color: purple">list</span>
                    </div>
                </td>
                <td>always apart from low level errors (such as action plugin)</td>
                <td>
                            <div>The value of stdout split into a list</div>
                    <br/>
                        <div style="font-size: smaller"><b>Sample:</b></div>
                        <div style="font-size: smaller; color: blue; word-wrap: break-word; word-break: break-all;">[[&#x27;...&#x27;, &#x27;...&#x27;], [&#x27;...&#x27;], [&#x27;...&#x27;]]</div>
                </td>
            </tr>
    </table>
    <br/><br/>


Status
------


Authors
~~~~~~~

- Seiko Solutions Inc.
