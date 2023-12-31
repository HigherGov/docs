---
description: Getting started with HigherGov API
---

# API

{% hint style="info" %}
**The HigherGov API is only available to subscribers.  We can also offer customized APIs per your needs.  Please** [**contact** ](mailto:contact@highergov.com)**us if you are interested in learning more.** &#x20;
{% endhint %}

### Data Limits and Pricing

All HigherGov plans include access to the API and access to 10,000 records per month through the API.  If you require more data, please [contact us](mailto:contact@highergov.com) with your use case for pricing.  &#x20;

### Endpoints

A list of available endpoints and fields is available in the [documentation](https://www.highergov.com/api-external/docs/). &#x20;

### Creating Keys

API keys can be managed when signed in by selecting the gear icon in the upper right and selecting API or by clicking [here](https://www.highergov.com/api-management/).  To create an API key select the Generate Key button.  Note that the full key will only be available on this screen one time so make sure to copy and securely save the key.

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

### Data Refresh Rate

Data is generally updated shortly after the underlying data source. Some examples are shown below.

| Endpoint                   | Data Update Frequency |
| -------------------------- | --------------------- |
| **Prime Contract Awards**  | Daily                 |
| **Contract Opportunities** | 20 Minutes            |
| **Prime Grant Awards**     | Daily                 |
| **Grant Opportunities**    | 2 Hours               |

### Code Examples

Below are a few code snippets to get started accessing the API.

{% tabs %}
{% tab title="Python" %}
<pre><code><strong>import requests
</strong>import json

# Define the Endpoint and Key
endpoint = 'https://www.highergov.com/api-external/contract/'
api_key = 'your-api-key-here'

#Define Parameters
params = {
    'api_key': api_key,
    'last_modified_date': '2023-07-06',
    'page_number': '1',
}

#Call API
response = requests.get(endpoint, params=params)

#Convert Response to JSON
data = response.json()
#print(json.dumps(data, indent=4))  # Print the JSON

#Loop through results
for result in data.get('results'):
    contract_award_id = result.get('award_id')
    ...
</code></pre>
{% endtab %}

{% tab title="Javascript" %}
```
// Define the Endpoint and Key
const endpoint = 'https://www.highergov.com/api-external/contract/';
const api_key = 'your-api-key-here';

// Define Parameters
let params = {
    'api_key': api_key, 
    'last_modified_date': '2023-07-06',
    'page_number': '1',
};

// Convert parameters to URL query string
let query = Object.keys(params).map(k => `${encodeURIComponent(k)}=${encodeURIComponent(params[k])}`).join('&');

// Call API
fetch(`${endpoint}?${query}`)
    .then(response => response.json()) // Parse response as JSON
    .then(data => {
        // Loop through results
        data.results.forEach(result => {
            let contract_award_id = result.award_id;
            console.log(contract_award_id);
        });  
    }); 

```
{% endtab %}

{% tab title="PHP" %}
```
<?php

// Define the endpoint and parameters
$endpoint = 'https://www.highergov.com/api-external/contract/';
$api_key = 'your-api-key-here';

$params = http_build_query([
    'api_key' => $api_key,
    'last_modified_date' => '2023-07-06',
    'page_number' => '1',
]);

// Initialize cURL
$ch = curl_init();

// Set the options
curl_setopt($ch, CURLOPT_URL, $endpoint.'?'.$params);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);

// Execute the cURL session
$response = curl_exec($ch);

// Close the cURL session
curl_close($ch);

// Convert the response into an associative array
$data = json_decode($response, true);

// Loop through each item in the 'results' array
foreach ($data['results'] as $result) {
    $contract_award_id = $result['award_id'];
}
```
{% endtab %}
{% endtabs %}



