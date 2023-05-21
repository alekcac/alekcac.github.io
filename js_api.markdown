---
layout: default
title: JS API
nav_order: 3
parent: Business integration
---

# [](#header-1)JS API

The JS API in MetaPerson Editor has three functions that you can use to set up your developer credentials, configure export parameters, and get the link to the resulting avatar. Here's a brief overview of each function:

* **Developer Credentials** - This is the call we discussed [earlier](web_integration) that allows you to add your developer credentials to the editor iframe. This ensures that your website or application is authorized to access the editor.

* **Export Parameters** - This function allows you to configure the export parameters for your avatar. You can specify the format of the exported file (such as GLB, GLTF, or FBX), the quality of the exported file, and other parameters.

* **Get Avatar Link** - This event allows you to get the link to the resulting avatar after it has been exported. This link can then be used to download or integrate the avatar into your website or application.


## [](#header-2)Export Parameters

The call is for setting the export parameters. This allows you to customize the output of your avatar by specifying things like the file format, resolution, and other options. Here's an example of how you can use the export parameters request:

```
function onUnityLoaded(evt, data) {
          let exportParametersMessage = {
              "eventName": "set_export_parameters",
              "format" : "gltf",
              "lod" : 1,
              "textureProfile" : "1K.jpg"
          };
          evt.source.postMessage(exportParametersMessage, "*");
      }
```

In this call, you can specify the format of the exported file, the level of detail (LOD), and the texture profile.

Here's a breakdown of the parameters:

* **eventName** - This is the name of the event, which in this case is "set_export_parameters". This tells MetaPerson Editor which request you're making.
* **format** - This parameter specifies the format of the exported file. In this example, the format is set to "gltf". However, MetaPerson Editor also supports other formats such as "gltf", "glb", and "fbx".
* **lod** - This parameter specifies the level of detail (LOD) for the exported file. The higher the LOD, the more detailed the exported file will be. In this example, the LOD is set to 1. However, MetaPerson Editor also supports a LOD of 2.
* **textureProfile** - This parameter specifies the texture profile for the exported file. This determines the quality of the textures used in the exported file. In this example, the texture profile is set to "1K.jpg", which indicates that the textures will be 1K resolution and in the JPEG format. You can also use other texture profiles such as:
  * "4K.png", "2K.png", "1K.png"
  * "4K.jpg", "2K.jpg", "1K.jpg"
  * "4K.webp", "2K.webp", "1K.webp"

After setting these parameters, you can use the `postMessage()` method to send the Export Parameters call to MetaPerson Editor. This ensures that your exported avatar meets your specific needs and requirements.

## [](#header-2)Get Avatar Link

You can use the `model_exported` event to receive the link to the exported avatar once the user has completed the export process. 

![](assets/img/export.png)

Here's an example code snippet that demonstrates how to do this:

```
function onWindowMessage(evt) {
          if (evt.type === "message") {
              if (evtName === "model_exported"){
                  console.log("model url: " + data.url)
              }
          }
      }
```

This code snippet sets up an event listener for the message event and checks if the eventName in the message data is `model_exported`. If it is, the code retrieves the avatar link from the message data and logs it to the console. You can modify this code to download the avatar or display it in your application as needed.

If you need any help or guidance with our JS API or any other aspect of MetaPerson Editor, our [support team](mailto:support@avatarsdk.com) is always available to assist you.
