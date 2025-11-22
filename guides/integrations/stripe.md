---
title: Stripe Integration
---
> **Status:** TODO - Priority: MEDIUM
>
> **Audience:** Developers

## Description

Integrate Stripe payment data and actions into your Forest Admin interface.

## Sources to migrate

- `documentation-agent-v1/how-tos/integrations/stripe.md`
- Stripe integration examples

## Content outline

1. **Integration Overview**
   - What you can do with Stripe integration
   - Use cases (customer management, subscription management, payments)
   - Architecture options

2. **Setup**
   - Stripe API key configuration
   - Webhook setup
   - Data sync strategy

3. **Displaying Stripe Data**
   - Customer data
   - Subscription information
   - Payment history
   - Invoice details

4. **Smart Actions for Stripe**
   - Charge customer
   - Refund payment
   - Update subscription
   - Cancel subscription
   - Send invoice

5. **Smart Fields from Stripe**
   - Customer lifetime value
   - Subscription status
   - Payment method info
   - MRR calculation

6. **Webhooks**
   - Receiving Stripe webhooks
   - Syncing data
   - Triggering actions

7. **Example Implementations**
   - Node.js + Stripe
   - Ruby + Stripe
   - Python + Stripe

8. **Best Practices**
   - Secure API key storage
   - Error handling
   - Testing with Stripe test mode
   - Rate limiting

## Related pages

- [Integrations Overview](/guides/integrations/overview)
- [Process > Actions](/product/process/actions/overview)
- [REST API Datasources](/product/integration/data-sources/rest-api/overview)
