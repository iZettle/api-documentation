Zettle API Incidents
=====================
# 30 March, 2021
## (status: resolved) Finance API returned wrong `originatorTransactionType` 
The following Finance API endpoint returned wrong `originatorTransactionType`:

`GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/transactions`.

<details><!-- start tag of the incident section-->
<summary>Click to see the incident.</summary>
    <h3>Incident summary</h3>
        <p>The <code>originatorTransactionType</code> field was returned with wrong values.</p>
        <p>The following table shows the expected and returned values for the field.<p>
        <table style="text-align:left">
            <thead>
                <tr>
                  <th>Expected <code>originatorTransactionType</code></th>
                  <th>Returned <code>originatorTransactionType</code></th>
                </tr>
            </thead>
                <tbody>             
                <tr>
                   <td>CARD_PAYMENT</td>
                   <td>PAYMENT</td>
                </tr>
                <tr>
                   <td>CARD_PAYMENT_FEE</td>
                   <td>PAYMENT</td>
                </tr>
                <tr>
                   <td>CARD_REFUND</td>
                   <td>PAYMENT</td>
                </tr>
                <tr>
                   <td>CARD_PAYMENT_FEE_REFUND</td>
                   <td>PAYMENT</td>
                </tr>        
                </tbody>
        </table>

<h4 name="incidentDuration">Incident duration</h4>
<p>Transactions made between the following timestamps were affected:</p>

|Start time | End time |Total duration
|:---- |:---- |:----
|2021-03-30 16:03:11.437237 UTC |2021-03-30 16:59:45.00856  UTC |56 minutes


<h3>What do you need to do</h3>
<ol>
    <li><p>Refetch transactions that happened during the <a href="incidentDuration">incident duration</a> for merchants that your integration serves.</p>
    <p>After refectching, the following values will returned.</p>    
    <table style="text-align:left">
        <thead>
            <tr>
              <th>Expected <code>originatorTransactionType</code></th>
              <th>Refetched <code>originatorTransactionType</code></th>
              <th>Example</th>
            </tr>
        </thead>
            <tbody>             
            <tr>
               <td>CARD_PAYMENT</td>
               <td>PAYMENT</td>
               <td>
                   <pre>
                        ...
                               {
                                   "timestamp": "2021-03-30T16:03:31.003+0000",
                                   "amount": -10,
                                   "originatorTransactionType": "CARD_PAYMENT_FEE",
                                   "originatingTransactionUuid": "ff4f492e-914c-1bbb-bb86-850e353b75b8"
                               },
                               {
                                   "timestamp": "2021-03-30T16:03:31.000+0000",
                                   "amount": 780,
                                   "originatorTransactionType": "CARD_PAYMENT",
                                   "originatingTransactionUuid": "ff4f492e-914c-1bbb-bb86-850e353b75b8"
                               },...
                   </pre>
               </td>
            </tr>
            <tr>
               <td>CARD_PAYMENT_FEE</td>
               <td>PAYMENT_FEE</td>
               <td><pre></pre></td>
            </tr>
            <tr>
               <td>CARD_REFUND</td>
               <td>PAYMENT</td>
               <td><pre></pre></td>
            </tr>
            <tr>
               <td>CARD_PAYMENT_FEE_REFUND</td>
               <td>PAYMENT_FEE</td>
               <td><pre></pre></td>
            </tr>        
            </tbody>
    </table>
    </li>    
    <li><p>If your integration disregards transactions with <code>originatorTransactionType</code> as <code>PAYMENT</code> and <code>PAYMENT_FEE</code>, handle those transactions and bookkeep the same as you used to do with <code>CARD</code>.</p></li> 
    
</ol>
</details>
