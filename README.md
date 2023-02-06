# Vue Storefront Build and Deployment Reusable Workflows

## Storefront Cloud Action

### Requirements

  - **Environment Variables** - multiline string that contains all the needed environment variables you need to run your application.

  Example below is a sample for SAP
  ```
  API_BASE_URL=/api/
  API_SSR_BASE_URL=http://additional-app-middleware:8181
  SAPCC_MEDIA_HOST=<URL>
  SAPCC_CURRENCY=USD
  SAPCC_LANG=en
  SAPCC_API_URI=<URL>
  SAPCC_API_CLIENT_ID=vuestorefront
  SAPCC_API_CLIENT_SECRET=${{ secrets.SAPCC_API_CLIENT_SECRET }}
  SAPCC_API_BASE_STORE=electronics
  SAPCC_API_BASE_STORE_ID=electronics
  SAPCC_API_CATALOG_ID=electronicsProductCatalog
  SAPCC_API_CATALOG_VERSION=Online
  SAPCC_OAUTH_SERVER_HOST=<URL>
  SAPCC_OAUTH_SERVER_AUTHENTICATION_ENDPOINT=/authorizationserver/oauth/token
  SAPCC_OAUTH_SERVER_AUTHENTICATION_TOKEN_REVOKE_ENDPOINT=/authorizationserver/oauth/revoke
  NUXT_IMAGE_PROVIDER_BASE_URL=<URL>
  NUXT_IMAGE_PROVIDER_UPLOAD_DIR=sap
  ```

### Recommended

- **.vuestorefrontcloud** directory consist the ff
  - Dockerfile - for your storefront or nuxt app setup with the correct ARG and ENV needed for your application
  - middleware.Dockerfile - for your middleware server setup with the correct ARG and ENV needed for your application
  - middleware.package.json
  - vue-storefront.sh

### Usage

Example below is an example on usage for SAP storefront integration

```yaml
# .github/workflows/deploy-vue-storefront-cloud.yml

name: Deploy to Vue Storefront Cloud
on:
  push:
    branches:
      - main

jobs:
  Integration:
    uses: vuestorefront/storefront-action/.github/workflows/storefrontcloud-action.yml@main
    with:
      # This will be pass to docker parameters --buildargs to make your docker build look environment variables for your storefront application
      STOREFRONT_BUILDARGS: NPM_EMAIL,NPM_PASS,NPM_USER,NPM_REGISTRY,API_BASE_URL,API_SSR_BASE_URL,VSF_API_URI,VSF_API_HOST,VSF_API_AUTH_HOST,VSF_API_CLIENT_ID,VSF_API_CLIENT_SECRET,VSF_API_SCOPES,VSF_SERVER_API_CLIENT_ID,VSF_SERVER_API_CLIENT_SECRET,VSF_SERVER_API_SCOPES,VSF_SERVER_API_OPERATIONS,NUXT_IMAGE_PROVIDER,NUXT_IMAGE_PROVIDER_BASE_URL,SAPCC_MEDIA_HOST,NUXT_IMAGE_PROVIDER_UPLOAD_DIR

      # This will be pass to docker parameters --buildargs to make your docker build look environment variables for your middleware application
      MIDDLEWARE_BUILDARGS: NPM_EMAIL,NPM_PASS,NPM_USER,NPM_REGISTRY,VSF_REDIS_ENABLED,VSF_REDIS_HOST,VSF_REDIS_PORT,VSF_REDIS_CACHE_INVALIDATE_KEY,VSF_REDIS_CACHE_INVALIDATE_URL,API_BASE_URL,API_SSR_BASE_URL,VSF_API_URI,VSF_API_HOST,VSF_API_AUTH_HOST,VSF_API_CLIENT_ID,VSF_API_CLIENT_SECRET,VSF_API_SCOPES,VSF_SERVER_API_CLIENT_ID,VSF_SERVER_API_CLIENT_SECRET,VSF_SERVER_API_SCOPES,VSF_SERVER_API_OPERATIONS,NUXT_IMAGE_PROVIDER,NUXT_IMAGE_PROVIDER_BASE_URL,MIDDLEWARE_ALLOWED_ORIGINS,SAPCC_API_URI,SAPCC_OAUTH_SERVER_HOST,SAPCC_OAUTH_SERVER_AUTHENTICATION_ENDPOINT,SAPCC_OAUTH_SERVER_AUTHENTICATION_TOKEN_REVOKE_ENDPOINT,SAPCC_API_CLIENT_ID,SAPCC_API_CLIENT_SECRET,SAPCC_API_BASE_STORE,SAPCC_API_BASE_STORE_ID,SAPCC_API_CATALOG_ID,SAPCC_API_CATALOG_VERSION,SAPCC_CURRENCY,SAPCC_LANG

    secrets:
      # These secrets are required and should always be here to make your build and deployment succeed
      PROJECT_NAME: ${{ secrets.PROJECT_NAME }}
      DOCKER_REGISTRY_URL: ${{ secrets.DOCKER_REGISTRY_URL }}
      CLOUD_USERNAME: ${{ secrets.CLOUD_USERNAME }}
      CLOUD_PASSWORD: ${{ secrets.CLOUD_PASSWORD }}
      CLOUD_REGION: ${{ secrets.CLOUD_REGION }} 
      NPM_EMAIL: ${{ secrets.NPM_EMAIL }}
      NPM_USER: ${{ secrets.NPM_USER }}
      NPM_PASS: ${{ secrets.NPM_PASS }}
      NPM_REGISTRY: ${{ secrets.NPM_REGISTRY }}
      ENV: |
        API_BASE_URL=/api/
        API_SSR_BASE_URL=http://additional-app-middleware:8181
        SAPCC_MEDIA_HOST=<URL>
        SAPCC_CURRENCY=USD
        SAPCC_LANG=en
        SAPCC_API_URI=<URL>
        SAPCC_API_CLIENT_ID=vuestorefront
        SAPCC_API_CLIENT_SECRET=${{ secrets.SAPCC_API_CLIENT_SECRET }}
        SAPCC_API_BASE_STORE=electronics
        SAPCC_API_BASE_STORE_ID=electronics
        SAPCC_API_CATALOG_ID=electronicsProductCatalog
        SAPCC_API_CATALOG_VERSION=Online
        SAPCC_OAUTH_SERVER_HOST=${{ secrets.SAPCC_OAUTH_SERVER_HOST }}
        SAPCC_OAUTH_SERVER_AUTHENTICATION_ENDPOINT=/authorizationserver/oauth/token
        SAPCC_OAUTH_SERVER_AUTHENTICATION_TOKEN_REVOKE_ENDPOINT=/authorizationserver/oauth/revoke
        NUXT_IMAGE_PROVIDER_BASE_URL=<URL>
        NUXT_IMAGE_PROVIDER_UPLOAD_DIR=sap
```

## More to domain to add here
