# box-ui-elements-lcc
Box UI Elements Lightning example that leverages the lightning:container component and messaging system.

# Corresponding Documentation

    * [Lightning Container documentation](https://developer.salesforce.com/docs/component-library/bundle/lightning:container)
    * [Box UI Elements](https://developer.box.com/en/guides/embed/ui-elements/)
    * [Box Token Exchange](https://developer.box.com/en/guides/embed/ui-elements/access/)

## Pre-Requisites

1. Open the source from this repo in VS Code.
2. In VS Code, use the cmd+shift+p shortcut and select SFDX: Authorize Org
3. Decrypt your Box JWT Private Key using the [parse_box_config.py](/scripts/parse_box_config.py). The script will decrypt your Box-generated application config file and create a new sfdc_box_config.json file at the root fo the sfdx project.
```
python3 parse_box_config.py /path/to/12345_box_congig.json
```
4. Update the [BoxContentUploaderController](/force-app/main/default/classes/BoxContentUploaderController.cls) with the values found in the newly generated sfdc_box_config.json file.
    > Note: this is necessary since Salesforce will throw an exception if you try to use an encrypted private key.

    * [String publicKeyId = 'PUBLIC_KEY_ID';](/force-app/main/default/classes/BoxContentUploaderController.cls#L9)
    * [String privateKey = 'DECRYPTED_PRIVATE_KEY';](/force-app/main/default/classes/BoxContentUploaderController.cls#L10)
    * [String enterpriseId = 'ENTERPRISE_ID';](/force-app/main/default/classes/BoxContentUploaderController.cls#L11)
    * [String clientId = 'CLIENT_ID';](/force-app/main/default/classes/BoxContentUploaderController.cls#L12)
    * [String clientSecret = 'CLIENT_SECRET';](/force-app/main/default/classes/BoxContentUploaderController.cls#L13)
5. Deploy your project source to either you scratch org or developer org in the next section.

## Deploy to your Org
Push to Scratch Org:
```
sfdx force:source:push
```

Deploy to Developer/Production Org:
```
sfdx force:source:deploy -p force-app -u username@company.com
```
6. In the [Box Developer Console](https://account.box.com/developers/services) configure your application CORS domains as per the following documentation:
  > Note: This may not be a comprehensive list. When testing your Lightning component, monitor the Chrome/Firefox dev tools console output for any CORS errors.

  * https://developer.box.com/en/guides/applications/custom-apps/jwt-setup/#cors-domains with the following Salesforce Lightning Domains
  * https://<your_custom_domain>-dev-ed.lightning.force.com : General Lightning domain
  * https://<your_custom_domain>-dev-ed--c.container.lightning.com : Lightning container domain
  * https://<your_custom_domain>.livepreview.salesforce-communities.com : Communities preview


## Project Contents
1. [BoxContentUploader](/force-app/main/default/aura/BoxContentUploader): Lightning Aura component
    * [BoxContentUploader.cmp](/force-app/main/default/aura/BoxContentUploader/BoxContentUploader.cmp): View / Markup that also contains the lightning:contain (ie iFrame)
    * [BoxContentUploader-meta.xml](/force-app/main/default/aura/BoxContentUploader/BoxContentUploader.cmp-meta.xml): Metadata File
    * [BoxContentUploader.css](/force-app/main/default/aura/BoxContentUploader/BoxContentUploader.css): Contains styles for the component.
    * [BoxContentUploader.design](/force-app/main/default/aura/BoxContentUploader/BoxContentUploader.cmp): Containing the Display name for the component. File required for components used in Lightning App Builder, Lightning pages, Experience Builder, or Flow Builder.
    * [BoxContentUploader-meta.svg](/force-app/main/default/aura/BoxContentUploader/BoxContentUploader.cmp): Custom icon resource for components used in the Lightning App Builder or Experience Builder.
    * [BoxContentUploaderController.js](/force-app/main/default/aura/BoxContentUploader/BoxContentUploader.cmp): Contains client-side controller methods to handle events in the component.
    * [BoxContentUploaderHelper.js](/force-app/main/default/aura/BoxContentUploader/BoxContentUploader.cmp): NOT USED - JavaScript functions that can be called from any JavaScript code in a component’s bundle.
    * [BoxContentUploaderRenderer.js](/force-app/main/default/aura/BoxContentUploader/BoxContentUploader.cmp): NOT USED - Client-side renderer to override default rendering for a component.
2. [BoxContentUploaderController](/force-app/main/default/classes/BoxContentUploaderController.cls): Apex server-side controller that creates a Box JWT Connection and contains token exchange logic.
3. [staticresources/uploader](/force-app/main/default/staticresources/uploader): Static application source files that receive messages from the Lightning Aura component and instantiate the Content Uploader Box UI Element.
4. [js-app](/js-app): Source code for the javascript app that is bundled as a static resource. When changing javascript source, you must rebuild using the following command:
```
yarn install (if this is your first time building)
yarn build (or npm build)
```
5. [CSP Trusted Sites](/force-app/main/default/cspTrustedSites): This essentially whitelists the pertinent Box URLs in the Salesforce CSP Trusted Sites setup page.
    > Why manually configure [CSP Trusted Sites](https://help.salesforce.com/articleView?id=csp_trusted_sites.htm) for Box when you can automate it?
    > Note: You may all need to update Remote Site Settings found in the Setup.


## Disclaimer
This project is a collection open source examples and should not be treated as an officially supported product. Use at your own risk. If you encounter any problems, please log an [issue](https://github.com/kylefernandadams/box-salesforce-blueprints/issues).

## License

The MIT License (MIT)

Copyright (c) 2020 Kyle Adams

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.