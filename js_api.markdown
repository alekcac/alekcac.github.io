---
layout: default
title: JS API
nav_order: 3
parent: Business integration
---

# [](#header-1)JS API

The JS API in MetaPerson Creator has several functions that you can use to set up your developer credentials, configure export parameters, set up the UI, and get the link to the resulting avatar. Here's a brief overview of each function:

* **Developer Credentials** - This is the call we discussed [earlier](web_integration) that allows you to add your developer credentials to the creator iframe. This ensures that your website or application is authorized to access the creator. If you type incorrect CLIENT_ID or CLIENT_ID export functionality will be unavailable, so please check these values. 

* **Export Parameters** - This function allows you to configure the export parameters for your avatar. You can specify the format of the exported file (such as GLB, GLTF, or FBX), the quality of the exported file, and other parameters.

* **Get Avatar Link** - This event allows you to get the link to the resulting avatar after it has been exported. This link can then be used to download or integrate the avatar into your website or application.

* **Export Avatar** - This function allows you to export avatar from API. It can be required if you hide the button for export from the UI and want to control export functionality not from the iframe, but from an external website or application.

* **UI Parameters** - This function allows you to configure some parts of the UI of MetaPerson Creator. E.g. hide some buttons or rename text for them etc. 

These events or functions should be called after the load of the corresponding version of MetaPerson Creator. In the Desktop version, we use the "unity_loaded" event to signal about it, in the Mobile version the "mobile_loaded" is used. Please check the corresponding business.html sample described [here](web_integration).

## [](#header-2)Export Parameters

The call is for setting the export parameters. This allows you to customize the output of your avatar by specifying things like the file format, resolution, and other options. Here's an example of how you can use the export parameters request:

```
    let exportParametersMessage = {
        "eventName": "set_export_parameters",
        "format" : "gltf",
        "lod" : 1,
        "textureProfile" : "1K.jpg"
    };
    evt.source.postMessage(exportParametersMessage, "*");
```

In this call, you can specify the format of the exported file, the level of detail (LOD), and the texture profile. This part is the same for Desktop and Mobile versions of MetaPerson Creator. 

Here's a breakdown of the parameters:

* **eventName** - This is the name of the event, which in this case is "set_export_parameters". This tells MetaPerson Creator which request you're making.
* **format** - This parameter specifies the format of the exported file. In this example, the format is set to "gltf". However, MetaPerson Creator also supports other formats such as "gltf", "glb", and "fbx".
* **lod** - This parameter specifies the level of detail (LOD) for the exported file. The higher the LOD, the more detailed the exported file will be. In this example, the LOD is set to 1. However, MetaPerson Creator also supports a LOD of 2.
* **textureProfile** - This parameter specifies the texture profile for the exported file. This determines the quality of the textures used in the exported file. In this example, the texture profile is set to "1K.jpg", which indicates that the textures will be 1K resolution and in the JPEG format. You can also use other texture profiles such as:
  * "4K.png", "2K.png", "1K.png"
  * "4K.jpg", "2K.jpg", "1K.jpg"
  * "4K.webp", "2K.webp", "1K.webp"

After setting these parameters, you can use the `postMessage()` method to send the Export Parameters call to MetaPerson Creator. This ensures that your exported avatar meets your specific needs and requirements.

## [](#header-2)Get Avatar Link

You can use the `model_exported` event to receive the link to the exported avatar once the user has completed the export process. 

![](assets/img/export.png)

Here's an example code snippet that demonstrates how to do this:

```
function onWindowMessage(evt) {
    if (evt.type === "message") {
        if (evt.data?.source === "metaperson_creator"){
            let data = evt.data;
            let evtName = data?.eventName;
            if (evtName === "model_exported"){
                console.log("model url: " + data.url)
            }
        }
    }
}
```

This code snippet sets up an event listener for the message event and checks if the eventName in the message data is `model_exported`. If it is, the code retrieves the avatar link from the message data and logs it to the console. You can modify this code to download the avatar or display it in your application as needed.

## [](#header-2)Export Avatar

To use you need to post a message to the iframe component, so you first need to find it on your page. 

E.g. it can be done in the following way:

```
function onExportClicked() {
    let iframe = document.getElementById("editor_iframe");
    let exportAvatarMessage = {
        "eventName": "export_avatar"
    };
    iframe.contentWindow.postMessage(exportAvatarMessage, "*");
}
```

You need to be sure that the avatar is ready for export and displayed on the scene before calling this function. 

## [](#header-2)UI Parameters

This function allows to customize a bit UI of MetaPerson Creator. This functionality differs between Mobile and Desktop version, so we split this description correspondingly. 

## [](#header-3)MetaPerson Creator Desktop

In the Desktop version, it allows to hide export button to use the **Export Avatar** function manually and control the modal window with an export link that shows after export.

Here's a breakdown of the parameters:

* **eventName** - This is the name of the event, which in this case is "set_ui_parameters". This tells MetaPerson Creator which request you're making.
* **isExportButtonVisible** - This parameter specifies should the UI include the export button or not.
* **closeExportDialogWhenExportComlpeted** - This parameter specifies closing the modal window with the export link right after export completion. 

```
let uiParametersMessage = {
       "eventName": "set_ui_parameters",
       "isExportButtonVisible" : true,
       "closeExportDialogWhenExportComlpeted" : false,
};
evt.source.postMessage(uiParametersMessage, "*");
```

## [](#header-3)MetaPerson Creator Mobile

In the Mobile version, this function includes a bit more options. 

```
let uiParametersMessage = {
    "eventName": "set_ui_parameters",
    "isExportButtonVisible": true,
    "isLoginButtonVisible": false,
    "exportButtonText": "Go",
    "theme": "light", 
    "gender": "female",
};
evt.source.postMessage(uiParametersMessage, "*");
```

The parameters of this code are:

* **eventName** - This is the name of the event, which in this case is "set_ui_parameters". This tells MetaPerson Creator which request you're making.
* **isExportButtonVisible** - This parameter specifies should the UI include the export button or not.
* **isLoginButtonVisible** - This parameter specifies should the UI include the profile button or not.
* **exportButtonText** - It allows to change the text of the export button. 
* **theme** - It allows to choose the visual theme for the UI (available options: "dark", "light").
* **gender** - If the application or website already has the information about the required gender, we can skip this question in the UI of the Mobile version. In this case, it shows the second screen with choosing input photo and you can't get back to the first screen with the home button. Available options are "male" and "female".

If you need any help or guidance with our JS API or any other aspect of MetaPerson Creator, our [support team](mailto:support@avatarsdk.com) is always available to assist you.
