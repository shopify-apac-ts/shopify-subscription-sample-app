# Overview
A Runnable Sample Subscrition app based on [Shopify developer document](https://shopify.dev/docs/apps/selling-strategies/subscriptions/modeling). 


This app is cloned and modified from [this sample](https://github.com/benzookapi/shopify-barebone-app-sample).


# How to run
1. Add the following environmental variables locally or in cloud platforms like Render / Heroku / Fly, etc.
```
SHOPIFY_API_KEY:              YOUR_API_KEY (Copy and paste from your app settings in partner dashboard)
SHOPIFY_API_SECRET:           YOUR_API_SECRET (Copy and paste from your app settings in partner dashboard)
SHOPIFY_API_VERSION:          unstable
SHOPIFY_API_SCOPES:           write_products,write_orders,read_customer_payment_methods,read_own_subscription_contracts,write_own_subscription_contracts,read_customers,write_assigned_fulfillment_orders,write_merchant_managed_fulfillment_orders,write_third_party_fulfillment_orders

SHOPIFY_DB_TYPE:              MONGODB (Default) / POSTGRESQL / MYSQL

// The followings are required if you set SHOPIFY_DB_TYPE 'MONGODB'
SHOPIFY_MONGO_DB_NAME:        YOUR_DB_NAME (any name is OK)
SHOPIFY_MONGO_URL:            mongodb://YOUR_USER:YOUR_PASSWORD@YOUR_DOMAIN:YOUR_PORT/YOUR_DB_NAME

// The followings are required if you set SHOPIFY_DB_TYPE 'POSTGRESQL'
SHOPIFY_POSTGRESQL_URL:       postgres://YOUR_USER:YOUR_PASSWORD@YOUR_DOMAIN(:YOUR_PORT)/YOUR_DB_NAME

// The followings are required if you set SHOPIFY_DB_TYPE 'MYSQL'
SHOPIFY_MYSQL_HOST:           YOUR_DOMAIN
SHOPIFY_MYSQL_USER:           YOUR_USER
SHOPIFY_MYSQL_PASSWORD:       YOUR_PASSWORD
SHOPIFY_MYSQL_DATABASE:       YOUR_DB_NAME

```

2. Build and run the app server locally or in cloud platforms. All settings are described in `package.json` (note that the built React code contains `SHOPIFY_API_KEY` value from its envrionment variable, so if you run it with your own app, you have to run the build command below at least one time).
```
Build command = npm install && npm run build (= cd frontend && npm install && npm run build && rm -rf ../public && mv dist ../public && mv ../public/index.html ../views/index.html  *Replacing Koa view files with Vite buit code)

Start command = npm run start (= node app.js)
```

3. If you run locally, you need to ngrok tunnel for public URL as follows (otherwise, the command lines above are usable in Render or other cloud platform deploy scripts).
```
cd NGROK_DIR && ngrok http 3000
```

4. Set `YOUR_APP_URL` (your ngrok or other platform `root` URL) and `YOUR_APP_URL/callback` to your app settings in [partner dashboard](https://partners.shopify.com/) and also **replace `const APP_URL=... in extensions/my-subscription-ext/src/index.jsx` with your current running app url manually**.

5. (For PostgreSQL or MySQL users only,) create the following table in your database (in `psql` or `mysql` command or other tools).
```
For PostgreSQL:

CREATE TABLE shops ( _id VARCHAR NOT NULL PRIMARY KEY, data json NOT NULL, created_at TIMESTAMP NOT NULL, updated_at TIMESTAMP NOT NULL );

For MySQL:

CREATE TABLE shops ( _id VARCHAR(500) NOT NULL PRIMARY KEY, data JSON NOT NULL, created_at TIMESTAMP NOT NULL, updated_at TIMESTAMP NOT NULL );

```
6. For CLI generated extensions, execute `npm run deploy -- --reset` and follow its instruction (choose your partner account, connecting to the exising app, etc.) which registers extensions to your exising app and create `/.env` file which has extensiton ids used by this sample app. After the command ends  successfully, go to the created extension in your [partner dashboard](https://partners.shopify.com/) and enable its dev. preview if available (it's enough for testing in development stores).

7. For updating the extensions, execute `npm run deploy` (without `-- --reset`) to apply (upload) your local modified files to the created extensions (`-- --reset` is used for changing your targeted app only).

8. (For live stores only, you need to create a version of the extension and publish it. See [this step](https://shopify.dev/apps/deployment/extension).)


# How to install
Access to the following endpoit.
`https://SHOPIFY_SHOP_DOMAIN/admin/oauth/authorize?client_id=YOUR_API_KEY&scope=YOUR_API_SCOPES&redirect_uri=YOUR_APP_URL/callback&state=&grant_options[]=`　

Or 

`you can install to your development stores from the app settings in partner dashboard.`


# TIPS

- You can use the endpoint of `webhookgdpr` for [GDPR Webhooks](https://shopify.dev/docs/apps/store/security/gdpr-webhooks).
- If you fail to get [protected customer data](https://shopify.dev/docs/apps/store/data-protection/protected-customer-data) in [subscriptionContract query](https://shopify.dev/docs/api/admin-graphql/unstable/queries/subscriptionContract), apply the customer data request from your app settings in partner dashboard and submit your app first (the submission can be in-review, doesn't have to be completed for dev. store testing) which enable you get them. 


# Disclaimer

- This code is fully _unofficial_ and NOT guaranteed to pass [the public app review](https://shopify.dev/apps/store/review) for Shopify app store. The official requirements are described [here](https://shopify.dev/apps/store/requirements). 
- You need to follow [Shopi API Licene and Terms of Use](https://www.shopify.com/legal/api-terms) even for custom app usage.
