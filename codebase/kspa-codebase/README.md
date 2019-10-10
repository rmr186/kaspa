# Kaspa Codebase Reference

The following are the important packages used in Kaspa's Go implementation:

<table>
  <thead>
    <tr>
      <th style="text-align:left">File Name</th>
      <th style="text-align:left">Purpose</th>
      <th style="text-align:left">Functionality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">btcd</td>
      <td style="text-align:left">Main package</td>
      <td style="text-align:left">
        <ul>
          <li>runs a full node</li>
          <li>pruning</li>
          <li>consensus (implementation of PHANTOM)</li>
          <li></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">cmd</td>
      <td style="text-align:left">Blockchain commands</td>
      <td style="text-align:left">
        <ul>
          <li>add block</li>
          <li>read next block</li>
          <li>importing blocks</li>
          <li></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">btcctl</td>
      <td style="text-align:left">Local node ctl commands</td>
      <td style="text-align:left">
        <ul>
          <li>add block</li>
          <li>read next block</li>
          <li>importing blocks</li>
          <li></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">dnsseeder</td>
      <td style="text-align:left">Provides list of addresses previously connected to this network</td>
      <td
      style="text-align:left">
        <ul>
          <li></li>
        </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">rpcclient</td>
      <td style="text-align:left">
        <p>Websocket-enabled JSON-RPC client requests to the local Kaspa node
          <br
          />
        </p>
        <p>(<em>rpcclient</em> is relatively heavy, for bigger request volume run
          an <em>apiserver</em> instead.)</p>
      </td>
      <td style="text-align:left">
        <ul>
          <li>dag</li>
          <li>doc</li>
          <li>extensions</li>
          <li>infrastructure</li>
          <li>log</li>
          <li>mining</li>
          <li>net</li>
          <li>notify</li>
          <li>rawrequest</li>
          <li>rawtransactions</li>
          <li></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">apiserver</td>
      <td style="text-align:left">Code for running the Kaspa API Server</td>
      <td style="text-align:left">
        <ul>
          <li></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">addrmgr</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">blockdag</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">indexes</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">btcec</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">btcjson</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">conmgr</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">database</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">dagconfig</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">ffldb</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">integration</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">logger</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">mempool</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">netsync</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">peer</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">server</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">txscript</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">util</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">wire</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">daghash</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">network</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">panic</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">random</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">subnetworkid</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">testtools</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">txsort</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>