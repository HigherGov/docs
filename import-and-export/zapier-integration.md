---
description: >-
  Quickly and easily add HigherGov opportunities as pursuits in your CRM using
  Zapier
---

# Zapier Integration

{% hint style="info" %}
Zapier integration requires a HigherGov Standard or Leader plan subscription.  If you would like to upgrade or learn more, please [contact us](mailto:contact@highergov.com).
{% endhint %}

### Prerequisites

**Zapier Account**: If you do not already have a Zapier account, you can create one [here](https://zapier.com/sign-up).  Many users may be able to connect HigherGov to their CRM using a free Zapier account.  If you expect to send more than 100 opportunities per month to your CRM (or are using Zapier for other tasks), you may require a [paid plan](https://zapier.com/pricing).  Pricing for a Zapier account is separate from a HigherGov subscription.&#x20;

**CRMs Supported by Zapier**: A list of applications supported by Zapier is available [here](https://zapier.com/apps). The most popular integrations on HigherGov include [HubSpot](https://www.hubspot.com/), [Salesforce](https://www.salesforce.com/), [Dynamics 365](https://www.microsoft.com/en-us/dynamics-365), [Monday.com](https://monday.com/), [Pipedrive](https://www.pipedrive.com/), [SugarCRM](https://www.sugarcrm.com/), [Zoho](https://www.zoho.com/crm/), and [ClickUp](https://clickup.com/), but any [CRM](https://zapier.com/apps/categories/sales-crm) that supports creating "pursuits", "deals", or "opportunities" should be able to ingest HigherGov opportunities. &#x20;

### **Creating a Zapier Pipeline in HigherGov**

{% hint style="info" %}
To create a Zapier Pipeline you must have an active HigherGov subscription and be signed in as an Admin. &#x20;
{% endhint %}

Select the gear icon in the upper right of the platform and select Zapier or click [here](https://www.highergov.com/zapier/).

<div align="left"><figure><img src="../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure></div>

Click the Create Pipeline button. &#x20;

<figure><img src="../.gitbook/assets/image (61).png" alt="" width="563"><figcaption></figcaption></figure>

Give your pipeline a name and select the CRM you are linking to.  If your CRM is not listed you can type in the name and press enter.  &#x20;

<figure><img src="../.gitbook/assets/image (60).png" alt="" width="563"><figcaption></figcaption></figure>

Securely save your Secret Key as it will only be available on this screen once.

<figure><img src="../.gitbook/assets/image (53).png" alt="" width="563"><figcaption></figcaption></figure>

### **Configuring Zapier**

To connect HigherGov to your CRM with Zapier, you will need to create a Zap.  Detailed instructions on creating a Zap are available [here](https://help.zapier.com/hc/en-us/articles/8496309697421-Create-Zaps#h_01H91E3WYAFDT8KEE45ST76WKW).  A list of available templates is available [here](https://zapier.com/apps/highergov/integrations#zap-template-list).

HigherGov provides one Trigger called the Pursuit Added Trigger that sends key information on the opportunity to your linked CRM.  As part of creating your Zap and authenticating with HigherGov, you will provide the Secret Key you created above.&#x20;

### **Adding Pursuits to Your Zapier Pipeline**

Once you have set up your Zap you can add pursuits to your Pipeline.&#x20;

#### Find Opportunities

HigherGov has many tools to help identify potential opportunities.  See the below topics for more details on some of the available ways to identify opportunities.

{% content-ref url="../find-opportunities/federal-contracts.md" %}
[federal-contracts.md](../find-opportunities/federal-contracts.md)
{% endcontent-ref %}

{% content-ref url="../find-opportunities/federal-grants.md" %}
[federal-grants.md](../find-opportunities/federal-grants.md)
{% endcontent-ref %}

{% content-ref url="../find-opportunities/state-and-local-contracts.md" %}
[state-and-local-contracts.md](../find-opportunities/state-and-local-contracts.md)
{% endcontent-ref %}

#### Adding Pursuit Automatically

You can automatically create new pursuits from Federal Contract Opportunity,  State and Local Contract Opportunity, Forecast Opportunity, DIBBS Opportunity, Grant Opportunity, IDV Award, Prime Contract Award, and Prime Grant Award pages by selecting the Pipeline dropdown in the upper right of the relevant page and then selecting the Zapier pipeline you previously created.

<figure><img src="../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

You will receive a message shortly after adding, indicating that the Pursuit has been successfully sent to Zapier using the Pipeline you selected or you will receive an error. &#x20;

<figure><img src="../.gitbook/assets/image (57).png" alt="" width="334"><figcaption></figcaption></figure>



## Zapier API Endpoints

{% hint style="info" %}
The below covers the technical endpoints used to support the Zapier integration.  Documentation on the main HigherGov API endpoints is available [here](highergov-api.md).
{% endhint %}

### Base URL

The base URL for our Zapier endpoints is `https://www.highergov.com/zapier` this will be referred to as `{{BASE_URL}}`.

### Authentication

This is the endpoint that Zapier calls when a user is attempting to log into Zapier.

Location:

&#x20;`{{BASE_URL}}/auth/`.

Sample Reques&#x74;**:**

```
const options = {
  url: '{{BASE_URL}/auth/',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
    'X-API-KEY': api_key,
  },
};

return z.request(options)
  .then((response) => {
    response.throwForStatus();
    const results = response.json;

    return results;
})
```

Successful Response (201):

`{"status": "success", "message": "API Key Validated", "subscription_name": "AcmeCo Pipeline"}`

### Subscribe

This is the endpoint that Zapier calls when a user is attempting to subscribe to a Zapier webhook.

Location:

&#x20;`{{BASE_URL}}/pipeline/subscribe/`.

Sample Reques&#x74;**:**

```
const options = {
  url: '{{BASE_URL}/pipeline/subscribe/',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
    'X-API-KEY': api_key,
  },
},
  body: JSON.stringify({
    target_url: bundle.targetUrl  
  }),
};

return z.request(options)
  .then((response) => {
    response.throwForStatus();
    const results = response.json;

    return results;
})
```

Successful Response (201):

`{"status": "success", "message": "New subscription created"}`

### Unsubscribe

This is the endpoint that Zapier calls when a user is attempting to unsubscribe from a Zapier webhook.

Location:

&#x20;`{{BASE_URL}}/pipeline/unsubscribe/`.

Sample Reques&#x74;**:**

```
const options = {
  url: '{{BASE_URL}/pipeline/unsubscribe/',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
    'X-API-KEY': api_key,
  },
};

return z.request(options)
  .then((response) => {
    response.throwForStatus();
    const results = response.json;

    return results;
})
```

Successful Response (201):

`{"status": "success", "message": "Subscription cancelled"}`

### Perform List

Returns sample data.

Location:

&#x20;`{{BASE_URL}}/perform/`

Sample Reques&#x74;**:**

```
const options = {
  url: '{{BASE_URL}/perform/',
  method: 'Get',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
  },
};

return z.request(options)
  .then((response) => {
    response.throwForStatus();
    const results = response.json;

    return results;
})
```

Successful Response (201):

```
{
"title": "string",
"description_text": "string",
"source_id": "string",
"source_id_version": "string",
"captured_date": "string",
"posted_date": "string",
"due_date": "string",
"agency": {
"agency_key": "string",
"agency_name": "string",
"agency_abbreviation": "string",
"agency_type": "string",
"path": "string"
},
"naics_code": {
"naics_code": "string"
},
"psc_code": {
"psc_code": "string"
},
"primary_contact_email": {
"contact_title": "string",
"contact_name": "string",
"contact_first_name": "string",
"contact_last_name": "string",
"contact_email": "string",
"contact_phone": "string"
},
"secondary_contact_email": {
"contact_title": "string",
"contact_name": "string",
"contact_first_name": "string",
"contact_last_name": "string",
"contact_email": "string",
"contact_phone": "string"
},
"set_aside": "string",
"opp_key": "string",
"version_key": "string",
"source_type": "string",
"unweighted_value": "string",
"current_datetime": "string",
"user_email": "string",
"path": "string"
}
```

