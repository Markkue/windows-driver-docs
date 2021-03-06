---
title: Texture Coordinate Transformations
description: Texture Coordinate Transformations
ms.assetid: d0857496-a7ce-4e9f-89d9-03c73d6f59e3
keywords:
- coordinate transformations WDK Direct3D
- texture transforms WDK Direct3D
- transforms WDK Direct3D
ms.author: windowsdriverdev
ms.date: 04/20/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# Texture Coordinate Transformations


## <span id="ddk_texture_coordinate_transformations_gg"></span><span id="DDK_TEXTURE_COORDINATE_TRANSFORMATIONS_GG"></span>


Texture coordinate transformations are enabled for the latest Direct X release.

Texture transforms are vertex-level transformation operations. These operations can be performed by any transform and lighting-enabled HAL driver and by any HAL device type.

Texture transforms are enabled by defining 4X4 matrices associated with each texture stage. All of the key state and data structures used by the software implementation of the geometry pipeline are available at the DDI level.

Using texture transformations, texture coordinates can be moved, relative to the vertices they are being drawn from. The texture transform describes how the texture coordinates are moved inside the texture map. Every time a texture transform is applied, the matrix changes the coordinates of the texture. The standard world matrix is used in these transformations.

At the API level, the **IDirect3DDevice7::SetTransform** method (described in the Direct3D SDK documentation) defines the texture transform matrix associated with the texture currently bound to the *i***-***th* texture stage. This matrix should be applied to the texture coordinates of that texture during rendering. Note that the same vertex-level texture coordinate set can be used at different stages with a separate texture transform matrix that may apply different textures.

The operation of the texture transform is controlled by the **IDirect3DDevice7::SetTextureStageState** method (described in the Direct3D SDK documentation) using the D3DTSS\_TEXTURETRANSFORMFLAGS flag.

The D3DTEXTURETRANSFORMFLAGS enumerated type, described in the DirectX SDK documentation, is used to control the dimension of the texture coordinate set that is generated by the texture coordinate transformation operation, and to control whether any perspective division should be applied to it.

D3DTTFF\_DISABLE indicates that the texture coordinates are passed directly to the hardware.

The number of the D3DTTFF\_COUNT*n* flag signals how many texture coordinates are going to be present. Note that this does not necessarily equate to the dimension of the textures themselves, if projected textures are used.

If projected textures are used, the D3DTTFF\_PROJECTED flag is set to indicate that the texture coordinates are to be divided by the last (COUNT*th*) element of the texture coordinate set. Thus, for a 2D projected texture, the count is three, because the first two elements are divided by the third, resulting in two floats for a 2D texture lookup. That is, both D3DTTFF\_COUNT2 and D3DTTFF\_COUNT3 | D3DTTFF\_PROJECTED refer to a 2D texture.

For nonprojected textures:

-   D3DTTFF\_COUNT1 indicates that the rasterizer should expect 1D texture coordinates.

-   D3DTTFF\_COUNT2 indicates that the rasterizer should expect 2D texture coordinates.

-   D3DTTFF\_COUNT3 indicates that the rasterizer should expect 3D texture coordinates.

-   D3DTTFF\_COUNT4 indicates the rasterizer should expect 4D texture coordinates.

The first byte encodes a count of texture coordinates expected to be used by the texture at a particular stage. Setting this to zero causes no texture transformation to be applied even if one was defined by the **IDirect3DDevice7::SetTransform** method. This is the number that is output from the texture coordinate transform stage.

The number of texture coordinates passed into the texture transform is defined by the Direct3D API [FVF](fvf--flexible-vertex-format-.md) code.

D3DTSS\_TEXTURETRANSFORMFLAGS defaults to D3DTFF\_DISABLED with no D3DTTFF\_PROJECTED flag set. The texture transform matrices default to 4X4 identity matrices.

On nontransform-and-lighting HAL devices (that is, those that require transformation operations on the host processor), the output vertices are provided to the driver with the appropriate DDI-level FVF code that may differ from the one specified at the API level. Devices that do not support hardware-accelerated transform and lighting may still do projected textures, and therefore the drivers must still respond to the D3DTSS\_TEXTURETRANSFORMFLAGS.

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[display\display]:%20Texture%20Coordinate%20Transformations%20%20RELEASE:%20%282/10/2017%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




