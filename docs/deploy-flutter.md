# deploy-flutter.yml

## How to use

It can deploy to multiple store :

```yml
deploy-playstore:
    name: Deploy to PlayStore
    uses: lenra-io/github-actions/.github/workflows/deploy-flutter.yml@main
    with:
      store: playstore
      package-name: com.example.app
      artifact-name: android
      artifact-path: '*.aab'
      playstore-track: alpha
    secrets:
      playstore-service-account-json: ${{ secrets.service_account_json }}

  deploy-appstore:
    name: Deploy to AppStore
    uses: lenra-io/github-actions/.github/workflows/deploy-flutter.yml@main
    with:
      store: appstore
      artifact-name: ios
      artifact-path: '*.ipa'
    secrets:
      appstore-username: ${{ secrets.appstore_username }}
      appstore-password: ${{ secrets.appstore_password }}
      appstoore-apikey: ${{ secrets.appstore_apikey }}
      appstore-apiissuer: ${{ secrets.appstore_apiissuer }}
```
