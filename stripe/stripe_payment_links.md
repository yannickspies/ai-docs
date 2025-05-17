# Stripe Payment Links

Payment Links are a simple way for customers to pay you when you sell online. Create one link that you can share with everyone.

## Overview

Payment Links allow you to:
- Quickly accept payments for goods, services, subscriptions, tips, or donations
- Share a single link for everyone to use
- Support over 30 languages and over 20 payment methods

## Creating Payment Links

Before you begin, decide what pricing model works best for you:

- **Products or subscriptions**: Best for e-commerce or SaaS where you're selling products for a fixed price.
- **Customers choose what to pay**: Best for donations, tipping, or pay-what-you-want. This pricing model currently doesn't support recurring payments or recurring donations. Learn more about the requirements for accepting tips or donations.

### Products or Subscriptions

To create a payment link for products or subscriptions:

1. In the Dashboard, open the Payment Links page and click New (or click the plus sign (➕) and select Payment link).
2. Fill out the payment details including:
   - Type (product or service)
   - Name (e.g., "Sunglasses")
   - Price (e.g., €0.00)
   - Currency (e.g., EUR)
3. Click Create link.

Your completed setup will show:
- Your business name
- Product name
- Price

### Customer-Defined Payments

To let your customers choose what to pay:

1. In the Dashboard, open the Payment Links page and click New (or click the plus sign (➕) and select Payment link).
2. Fill out the payment details.
3. (Optional) Set a preset amount.
4. (Optional) Set minimum and maximum payment amounts. By default, the maximum payment amount is 10,000.00 EUR. Contact support to increase this limit.
5. Click Create link.

### Payment Links on Mobile

If you're creating a product or subscription, use the Stripe Dashboard iOS app to create a payment link on your mobile device. In the app, go to Payments > Payment Links to create a payment link (or click the create icon (➕) and select Payment link). 

Note: The iOS app doesn't currently support creating links where your customers choose how much to pay.

## Sharing Payment Links

Use the Dashboard to copy your payment link, and share it online. Click the copy icon next to an existing link on the Payment Links page, or go to the payment link's details page. 

You can share your payment link multiple times and anywhere online, including:
- Emails
- Text messages
- Social media platforms

### Generate a QR Code

Create a QR code for a payment link in the Dashboard:
1. Choose an existing link from the Payment Links page, or create a new link
2. Click QR code
3. Copy or download a PNG image of the QR code

The QR code doesn't expire. If you deactivate the underlying payment link, the QR code redirects to an expiration page.

### Embed a Button on Your Site

Turn your payment link into an embeddable buy button to sell a product or subscription from your website:
1. Select an existing link from the Payment Links page or create a new link
2. Click Buy button
3. Copy the code and paste it into your website

## Managing Payment Links

### Deactivate a Link

Deactivate a payment link through:
- **Dashboard**: Click the overflow menu (⋮) to the right of the desired payment link, and then Deactivate
- **API**: Use the API to deactivate a payment link

After you deactivate a link, customers are no longer able to make a purchase using it. You can choose to reactivate the payment link at any time.

### Configure Payment Methods

With Dynamic payment methods, Stripe displays the most relevant and compatible payment methods to your customers, including Apple Pay and Google Pay. 

Stripe enables certain payment methods for you by default and might enable additional payment methods after notifying you. Use the Dashboard to enable or disable payment methods at any time. Learn more about supported payment methods and different types of payment methods.

You can review what payment methods your customers see in the Dashboard by entering a transaction ID or setting an order amount and currency.

## Creating an Embeddable Buy Button

Create an embeddable buy button to sell a product, subscription, or accept a payment on your website:

1. Select an existing link from the Payment Links list view or create a new link where you can decide which products to sell and customize the checkout UI
2. Click Buy button to configure the buy button design
3. Generate the code that you can copy and paste into your website

### Customize the Button

By default, your buy button uses the same branding and call to action configured for your payment link. You can:

- Choose between a simple button and a card widget
- Set brand colors, shapes, and fonts to match your website
- Set the language of the button and payment page to match your website's language
- Customize your button's call to action

### Embed the Button

Stripe provides an embed code composed of a `<script>` tag and a `<stripe-buy-button>` web component:

```html
<body>
  <h1>Purchase your new kit</h1>
  <!-- Paste your embed code script here. -->
  <script
    async
    src="https://js.stripe.com/v3/buy-button.js">
  </script>
  <stripe-buy-button
    buy-button-id='{{BUY_BUTTON_ID}}'
    publishable-key="pk_test_VOOyyYjgzqdm8I3SrBqmh9qY"
  >
  </stripe-buy-button>
</body>
```

Click Copy code to copy the code and paste it into your website.

If you're using HTML, paste the embed code into the HTML. If you're using React, include the script tag in your index.html page to mount the `<stripe-buy-button>` component.

> **Caution**: The buy button uses your account's publishable API key. If you revoke the API key, you need to update the embed code with your new publishable API key.

### Customization Attributes

You can add these attributes to customize checkout:

| Parameter | Description | Syntax |
|-----------|-------------|--------|
| client-reference-id | Attach a unique string of your choice to the Checkout Session. The string can be a customer ID or a cart ID (or similar) that you use to reconcile the Session with your internal systems. If you pass this parameter to your `<stripe-buy-button>`, it's sent in the checkout.session.completed webhook upon payment completion. | The client-reference-id can contain alphanumeric characters, dashes, or underscores, and be any value up to 200 characters. Invalid values are silently dropped, but your payment page continues to work as expected. |
| customer-email | Prefill the email address on the payment page. When the property is set, the buy button passes it to the Checkout Session's customer_email attribute. The customer can't edit the email address on the payment page. | The customer-email must be a valid email. Invalid values are silently dropped, but your payment pages continues to work as expected. |
| customer-session-client-secret | Pass an existing Customer object. | The customer-session-client-secret value must be generated from the client_secret. |

### Pass an Existing Customer

You can provide an existing Customer object to Checkout Sessions created from the buy button:

1. Create a customer session for a user you've already authenticated server-side:

```bash
curl https://api.stripe.com/v1/customer_sessions \
  -u "sk_test_YOUR_SECRET_KEY:" \
  -d customer={{CUSTOMER_ID}} \
  -d "components[buy_button][enabled]"=true
```

2. Return the client_secret to the client
3. Set the customer-session-client-secret attribute on the `<stripe-buy-button>` web component:

```html
<body>
  <script
    async
    src="https://js.stripe.com/v3/buy-button.js">
  </script>
  <stripe-buy-button
    buy-button-id='{{BUY_BUTTON_ID}}'
    publishable-key="pk_test_VOOyyYjgzqdm8I3SrBqmh9qY"
    customer-session-client-secret="{{CLIENT_SECRET}}"
  >
  </stripe-buy-button>
</body>
```

> **Note**: You must provide the client_secret within 30 minutes. After providing the client secret, you have an additional 30 minutes until the customer session expires. Any resulting Checkout Sessions created from the buy button will fail. Don't cache the client secret, instead generate a new one every time you render each buy button.

### Content Security Policy

If you've deployed a Content Security Policy, the policy directives that the buy button requires are:

- frame-src, https://js.stripe.com
- script-src, https://js.stripe.com

### Limitations

Rendering the buy button requires a website domain. To test the buy button locally, run a local HTTP server to host your website's index.html file over the localhost domain. To run a local HTTP server, use Python's SimpleHTTPServer or the http-server npm module.

## Tracking Payments

After your customer makes a payment using a payment link, you can see it in the payments overview in the Dashboard.

- If you're new to Stripe, you'll receive an email after your first payment
- To receive emails for all successful payments, update your notification preferences in your Personal details settings
- Stripe creates a new guest customer for one-time payments and a new Customer when selling a subscription or saving a payment method for future use

Learn more about handling payment links post-payment, like how to configure post-payment behavior for a buy button or payment link.

## Setting Up Your Development Environment

### For Non-Developers

If you're not a developer, check out:
- No-code docs
- Prebuilt solutions from Stripe's partner directory
- Hire a Stripe-certified expert

### CLI and SDK Setup

Stripe's server-side SDKs and command-line interface (CLI) allow you to interact with Stripe's REST APIs.

#### Chrome Extensions

For Chrome extensions, build your payment integration with Stripe (such as Elements or Checkout) on your own website. Then, set up your Chrome extension to send users to this payment page when they're ready to complete a purchase. This method is more secure and easier to maintain than trying to handle payments directly within the extension.

### Installing the Stripe CLI

You can install the Stripe CLI using:

- **Homebrew**: 
  ```bash
  brew install stripe/stripe-cli/stripe
  ```
  Or on Linux version of homebrew:
  ```bash
  brew install stripe-cli
  ```
- **apt**: Available for Linux
- **yum**: Available for Linux
- **Scoop**: Available for Windows
- **Direct download**: Available for macOS, Linux, Windows
- **Docker**: Available as a container

#### Authentication

Log in and authenticate your Stripe user Account to generate a set of restricted keys:

```bash
stripe login
```

Press the Enter key on your keyboard to complete the authentication process in your browser.

#### Testing the CLI

After installation, you can test by creating a product:

```bash
stripe products create \
--name="My First Product" \
--description="Created with the Stripe CLI"
```

Next, create a price attached to the product:

```bash
stripe prices create \
  --unit-amount=3000 \
  --currency=usd \
  --product={{PRODUCT_ID}}
```

### Server-Side SDKs

Stripe offers server-side SDKs for:

- Ruby
- Python
- Go
- Java
- Node.js
- PHP
- .NET

#### Ruby SDK (v15.0.0)

Requirements:
- Ruby 2.3+
- RubyGems

Installation:
```bash
bundle add stripe
bundle install
```

Example usage:
```ruby
require 'rubygems'
require 'stripe'
Stripe.api_key = "sk_test_YOUR_SECRET_KEY"

starter_subscription = Stripe::Product.create(
  name: 'Starter Subscription',
  description: '$12/Month subscription',
)

starter_subscription_price = Stripe::Price.create(
  currency: 'usd',
  unit_amount: 1200,
  recurring: {interval: 'month'},
  product: starter_subscription['id'],
)

puts "Success! Here is your starter subscription product id: #{starter_subscription.id}"
puts "Success! Here is your starter subscription price id: #{starter_subscription_price.id}"
```

#### Node.js SDK (v18.0.0)

Requirements:
- Node.js 12+

Installation:
```bash
npm init
npm install stripe --save
```

Example usage:
```javascript
const stripe = require('stripe')('sk_test_YOUR_SECRET_KEY');

stripe.products.create({
  name: 'Starter Subscription',
  description: '$12/Month subscription',
}).then(product => {
  stripe.prices.create({
    unit_amount: 1200,
    currency: 'usd',
    recurring: {
      interval: 'month',
    },
    product: product.id,
  }).then(price => {
    console.log('Success! Here is your starter subscription product id: ' + product.id);
    console.log('Success! Here is your starter subscription price id: ' + price.id);
  });
});
```

## Building a Checkout Page

Use Checkout Sessions to leverage a Stripe-hosted page, integrate your site with an embedded form, or build a customized checkout process with embedded components. These integrations support one-time payments, subscriptions, and over 40 local payment methods.

### Payment UI Options

You can use three different types of payment UIs with the Checkout Sessions API:

1. **Stripe-hosted page**: Customers are redirected to a Stripe-hosted payment page when they're ready to complete their purchase. After the customer completes the transaction, they can be redirected back to your site.

2. **Embedded form**: Customers stay on your site and are shown an embedded form. The customer completes the transaction on the same page in your site without redirections.

3. **Embedded components**: Customizable checkout page, built with Stripe Elements. Customers stay on your site and are shown a customized checkout page.

### Comparison of Options

| Feature | Stripe-Hosted Page | Embedded Form | Embedded Components |
|---------|-------------------|---------------|---------------------|
| UI | Checkout | Checkout | Elements |
| API | Checkout Sessions | Checkout Sessions | Checkout Sessions |
| Integration effort | Low coding | Low coding | More coding |
| Hosting | Stripe-hosted page (optional custom domains) | Embed on your site | Embed on your site |
| UI customization | Limited customization¹ | Limited customization¹ | Extensive customization with Appearance API |

¹ Limited customization provides 20 preset fonts, 3 preset border radius options, logo and background customization, and custom button color.

### Checkout Customization Options

- **Customize look and feel**: Change appearance and behavior of Checkout
- **Collect additional information**: Capture shipping and other customer info during checkout
- **Collect taxes**: Implement tax collection for one-time payments
- **Dynamically update checkout**: Make changes while the customer checks out
- **Add trials, discounts, upsells, and optional items**: Implement promotional features
- **Subscriptions**: Create recurring payment plans for customers
- **Set up future payments**: Save payment details for later use
- **Save payment details during payment**: Capture both immediate and future payment capabilities
- **Support local currencies**: Use Adaptive Pricing for local currency payments
- **Manage product catalog**: Handle inventory and fulfillment
- **Migrate payment methods**: Move payment method management to the Dashboard
- **Customize post-payment experience**: Configure what happens after payment