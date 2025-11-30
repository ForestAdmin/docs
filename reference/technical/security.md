# Data Privacy

## Private by design

When logging into the **Forest Admin UI** in your browser, you will connect to:

1. the **Forest Admin servers** to retrieve the **Forest Admin layouts configuration**,
2. the **Agent** to retrieve your **data** and populate the Forest Admin UI with it.

{% hint style="success" %}
As your data is transmitted directly from the Agent hosted on your end and the user browser, **it never transits through our servers**.
{% endhint %}

![](../assets/security-privacy-architecture.png)

## **No third-party vendor tracking**

{% hint style="success" %}
Whatever your project plan (free or paying plan), Forest Admin guarantees the respect of data privacy.
{% endhint %}

In addition, Forest Admin provides an option to completely disable any third-party vendors that could track metadata of your activity available from your browser.

{% hint style="info" %}
You need to be on a [Forest Admin Pro plan](https://www.forestadmin.com/pricing) to have access to this feature.
{% endhint %}

![](../assets/security-privacy-no-3rd-party.png)

# Security

## Tokens

The connection to both the **Agent** and the **Forest Admin Servers** are protected using 2 different JsonWebTokens signed by 2 different keys:

1. `FOREST_ENV_SECRET` to authenticate all requests made to the **Forest Admin Servers**
2. `FOREST_AUTH_SECRET` to authenticate all requests made to the **Agent**

![](../assets/security-privacy-jwt.png)

{% hint style="warning" %}
`FOREST_ENV_SECRET` is provided by Forest Admin and ensures your Agent interacts with the relevant environment configuration on Forest Admin servers.

`FOREST_AUTH_SECRET` is chosen freely by you and is unknown by Forest Admin.

Both secrets must never be disclosed to anyone.
{% endhint %}

The JWT Data Token contains all the details of the requesting user. On any authenticated request to your Agent, you can use that information to implement custom behaviors.

{% code %}

```json
{
  "id": "172",
  "email": "angelicabengtsson@doha2019.com",
  "firstName": "Angelica",
  "lastName": "Bengtsson",
  "team": "Pole Vault",
  "role": "Manager",
  "tags": [{ "key": "country", "value": "Sweden" }],
  "renderingId": "4998",
  "iat": 1569913709,
  "exp": 1571123309
}
```

{% endcode %}

## IP Whitelisting

The [IP whitelisting](https://docs.forestadmin.com/user-guide/project-settings/security-tab#ip-whitelisting) feature allows you to create a list of trusted IP addresses or IP ranges from which your admin users can both access to the **Forest Admin UI** and interact with your **Agent**.

![](../assets/security-privacy-ip-whitelist.png)

## **DMZ & VPN**

You're free to host your **Agent** in the cloud architecture you want to be compliant with your security infrastructure (DMZ, VPN, etc.).

![](../assets/security-privacy-dmz-vpn.png)

## Credentials

We’re already working with companies compliant with the following Industry Standard Certifications.

![](../assets/security-privacy-credentials.png)
