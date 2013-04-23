## Errors

When sending requests to the API, it's possible you'll get errors. Here are the response codes and what they mean:

<table>
<tr>
<th>Code</th>
<th>Text</th>
<th>Description</th>
</tr>

<tr>
<td>200</td>
<td>OK</td>
<td>Request Success</td>
</tr>

<tr>
<td>400</td>
<td>Bad Request</td>
<td>The request was invalid. This happens if you try to follow yourself, or something similar.</td>
</tr>

<tr>
<td>401</td>
<td>Unauthorised</td>
<td>No access token was supplied</td>
</tr>

<tr>
<td>403</td>
<td>Forbidden</td>
<td>The request was refused because you don't have access. There will be a message explaining why.</td>
</tr>

<tr>
<td>404</td>
<td>Not Found</td>
<td>Either the endpoint requested does not exist, or the resource being requested does not exist.</td>
</tr>
</table>