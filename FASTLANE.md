# Fastlane Tutorial

Below are step by step instructions on how to update and upload your app to iTunes Connect using [Fastlane](https://fastlane.tools/). Fastlane is a set of command line tools which automate the app building and deployment process. __Note: Fastlane is only supported on macOS at the moment. There is no current support for Windows or Linux.__

## Requirements

In order to use Fastlane, you need to first install Fastlane along with it's dependencies on your development computer. You can install Fastlane for the Zype template by:

1. Open __Terminal__ and navigate to the app folder with the following command: `cd <PATH TO APP FOLDER>`
2. Enter the following command to download Fastlane: `bundle install`

### Setup

Before you can build your iOS app, some Developer Account setup and build configuration is required.

1. In order for Fastlane to automate your app building, you will need to log in to your Apple Developer account in XCode. This gives Fastlane permission to run tasks using your Apple login. You can log in to your Apple account by going to __Xcode -> Preferences -> Accounts -> + (bottom left corner)__
<a href="https://drive.google.com/uc?export=view&id=13xDiHGEopmZurpWRYKJb1wORpSgW5kFz"><img src="https://drive.google.com/uc?export=view&id=13xDiHGEopmZurpWRYKJb1wORpSgW5kFz" style="width: 500px; max-width: 100%; height: auto" title="Click for the larger version." /></a>

2. Once you have logged into your Apple Developer account in XCode, you will need to update __`fastlane/Appfile`__. You will need to update the `app_identifier`, `apple_id`, and `team_id`.
  - The `app_identifier` is the app id for your application. This should be unique and used on all future updates. If you already have a live app, please use the app id for your existing app.
  - The `apple_id` is the email for your Apple Developer account (same one you logged in with in step 1).
  - The `team_id` is the id for your development team. You will need this if your Apple account is tied to many development teams. You can find your __team_id__ in Apple's Developer portal by going to __Overview__.
<a href="https://drive.google.com/uc?export=view&id=14XJDbkj29mxln2jynLASDpSnrrtLFENM"><img src="https://drive.google.com/uc?export=view&id=14XJDbkj29mxln2jynLASDpSnrrtLFENM" style="width: 500px; max-width: 100%; height: auto" title="Click for the larger version." /></a>

3. Once the Appfile is updated, you will need to update __`fastlane/Fastfile`__. You will need to update the `app_display_name`, `app_version_num`, and `app_build_num`.
  - The `app_display_name` is the name that shows for your app when it is on the phone.
  - The `app_version_num` is version of your app. If you are updating an existing app, remember to increase the version number to a version higher than your live app.
  - The `app_build_num` is the build number for your app. Normally this is left at 1, however if you are trying to update an app that has not been submitted yet you can simply increase the build number.
<a href="https://drive.google.com/uc?export=view&id=1DmLmPN482FFZTRWnRXZP9golMYPiaLuQ"><img src="https://drive.google.com/uc?export=view&id=1DmLmPN482FFZTRWnRXZP9golMYPiaLuQ" style="width: 500px; max-width: 100%; height: auto" title="Click for the larger version." /></a>

4. After you have updated the Fastfile, you should place your app icon (1024px by 1024px PNG) in base of the app folder.
<a href="https://drive.google.com/uc?export=view&id=1YLDNxkFxwWaWL1oKksfJ9_5bQjALodPt"><img src="https://drive.google.com/uc?export=view&id=1YLDNxkFxwWaWL1oKksfJ9_5bQjALodPt" style="width: 500px; max-width: 100%; height: auto" title="Click for the larger version." /></a>

### Automating with Fastlane

Once you have completed the setup, you are ready to start building your app with Fastlane's automation tools. Inside the `fastlane/Fastfile`, there are a few commands created to help speed up the app building process. You should enter the commands in __Terminal__ within the root of the app directory.

#### Update app configs

5. You can update the app configurations (app id, version/build number, display name) by entering the following command:

```
fastlane update_with_appfile
```

(Optional) If you want to see the updated configurations, you can enter:

```
fastlane display_config
```

#### Update app icon

6. Instead of updating the app icon for all the different sizes, you can enter the following command:

```
fastlane make_app_icons app_icon_path:< APP ICON FILENAME >.png
```

#### Taking screenshots

7. You can take screenshots by entering the following command:

```
fastlane screenshots
```

#### Creating app on iTunes Connect

8. Before you start creating your certificates and building your app, you need to create your app id and register the app under iTunes Connect. In order to do this you can use the following command:

```
fastlane create_itc_app
```

#### Creating certificates and profiles

9. You can create your certificate and the provisioning profile needed to upload your app with the following command:

```
fastlane create_cert_and_prov
```

`fastlane create_cert_and_prov` is the same as

```
fastlane create_cert
fastlane create_prov
```

#### Building and uploading to iTunes Connect

10. You can build and upload your app with the following:

```
fastlane build_and_upload
```

__Note:__ If your Apple ID is linked to multiple iTunes Connect teams, you will be prompted to select your team from a list before the upload starts. The iTunes Connect team id is not the same as the team id for the Apple Developer portal.

(Optional) If you did not create your provisioning profile with Fastlane, you can download your provisioning profile and enter the following:

```
fastlane build_and_upload profile_path:<PATH TO PROVISIONING PROFILE> profile_name:<NAME OF PROVISIONING PROFILE>
```

__Note:__ `fastlane build_and_upload` is the same as:

```
fastlane build # also accepts profile_path and profile_name
fastlane upload
```

Once you have started the build and upload process, be aware Fastlane may ask you for permission (computer password) and confirmation to continue the process. When prompted for your password it is faster to click "Always Allow" or it will ask you multiple times afterward.

#### Updating App Metadata

11. (Optional) If you want use Fastlane to update your app's metadata on iTunes Connect you can update the files within the __`fastlane/metadata`__ folder. By default, the app will only update the metadata for `en-US` using the files found in `fastlane/metadata/default` folder. You can see which languages are updated within the __fastlane/Deliverfile__. The default language is the first language listed in the `languages` field.

To upload the metadata, simply enter:

```
fastlane update_metadata
```

You can also update the review information inside __`fastlane/metadata/review_information`__. This info will be included when you submit your app for review. If you include a demo user and password, make sure that it matches a test consumer in the Zype platform.

When uploading if you are a part of multiple iTunes Connect teams you will be asked to confirm your team before upload. You will also need to confirm that the metadata is correct before upload.

If you want to add additional languages, you can update the `languages` in __fastlane/Deliverfile__ and create a folder named after the language code you want to add metadata for along with the `.txt` files. To see all the supported language codes, you can visit: [https://docs.fastlane.tools/actions/upload_to_app_store/#available-language-codes](https://docs.fastlane.tools/actions/upload_to_app_store/#available-language-codes)
