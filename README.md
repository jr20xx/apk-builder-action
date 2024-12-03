> [!WARNING]
>
> This is a work in progress at the moment and it is still under experimentation. Use it at your own risk and consideration.
>

This is a GitHub Action created with the purpose of easing the process of building an APK using Gradle directly from GitHub. The usage is really simple and here are the parameters you can use when using this action:

| Parameter | Description | Default value |
| --- | --- | --- |
| java-version | The JDK version to use to perform all operations | 17 |
| java-distribution | The name of the JDK distribution to use | temurin |
| encoded-keystore | The Base64 encoded keystore file used to sign the release APK | - |
| keystore-password | The password of the keystore used to sign the release APK | - |
| alias | The keystore's alias used to sign the release APK | - |
| alias-password | The password of the keystore's alias used to sign the release APK | - |

This action will produce either a debug or a release APK depending on when the `encoded-keystore` parameter is defined. **If** this parameter **is defined**, **a release** APK will be built; **otherwise** a **debug version** will be built. 

After building the APK, you are free to do whatever you want to do with it. For example, if you defined the `encoded-keystore`, you can upload the release APK by adding the following lines in your workflow file:

```yml
- name: Upload release app
  uses: actions/upload-artifact@v4
  with:
    name: 'app-release'
    path: app/build/outputs/apk/release/*.apk
    if-no-files-found: error
```

In case you didn't define the `encoded-keystore` param, then you can upload the debug version as it follows:
```yml
- name: Upload debug app
  uses: actions/upload-artifact@v4
  with:
    name: 'app-debug'
    path: app/build/outputs/apk/debug/*.apk
    if-no-files-found: error
```