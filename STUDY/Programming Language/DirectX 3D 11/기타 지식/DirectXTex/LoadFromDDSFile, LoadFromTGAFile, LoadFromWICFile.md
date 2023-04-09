# LoadFromDDSFile

Loads a `.DDS` file (including performing any necessary legacy conversions).
```c++
HRESULT LoadFromDDSFile( _In_z_ const wchar_t* szFile,
    DDS_FLAGS flags, TexMetadata* metadata, ScratchImage& image );
```

- Parameter
For the load functions, the _metadata_ parameter can be nullptr as this information is also available in the returned **ScratchImage**.

- Examples
This is a simple loading example. A `DDS` file can potentially include any kind of Direct3D resource in any DXGI format, so the _TexMetadata_ info is needed to understand the full content of the file.
```c++
TexMetadata info;
auto image = std::make_unique<ScratchImage>();
HRESULT hr = LoadFromDDSFile( L"TEXTURE.DDS",
    DDS_FLAGS_NONE, &info, *image );
if ( FAILED(hr) )
    // error
```

- Ref
https://github.com/microsoft/DirectXTex/wiki/DDS-I-O-Functions

# LoadFromTGAFile

The [Targa Truvision](http://wikipedia.org/wiki/Truevision_TGA) (`.TGA`) format is commonly used as a texture source file format in game development, but this format is not supported by any built-in WIC codec. These functions implement a simple reader and writer for this format.

```c++
HRESULT LoadFromTGAFile( const wchar_t* szFile,
    TGA_FLAGS flags,
    TexMetadata* metadata, ScratchImage& image );
```

- Parameters
For the load functions, the _metadata_ parameter can be nullptr as this information is also available in the returned **ScratchImage**.

For the save functions, a TGA 2.0 extension footer is written if _metadata_ is non-null. It sets a 2.2 gamma for SRGB formats, and includes the alpha mode.

- Examples
This is a simple loading example. The `TGA` format cannot contain complicated multi-image formats, so the _TexMetadata_ info is redundant information.
```c++
auto image = std::make_unique<ScratchImage>();
HRESULT hr = LoadFromTGAFile( L"ROCKS.TGA", TGA_FLAGS_NONE, nullptr, *image );
if ( FAILED(hr) )
    // error
```

A `TGA` file can only store one 2D image.

```c++
const Image* img = image->GetImage(0,0,0);
assert( img );
HRESULT hr = SaveToTGAFile( *img, TGA_FLAGS_NONE, L"NEW_IMAGE.TGA" );
if ( FAILED(hr) )
    // error
```

You can also save data directly from memory without using the intermediate **ScratchImage** at all. This example assumes a single 2D image is being written out.

```c++
Image img;
img.width = /*<width of pixel data>*/;
img.height = /*<height of pixel data>*/;
img.format = /*<a DXGI format from the supported list above>*/;
img.rowPitch = /*<number of bytes in a scanline of the source data>*/;
img.slicePitch = /*<number of bytes in the entire 2D image>*/;
img.pixels = /*<pointer to pixel data>*/;
HRESULT hr = SaveToTGAFile( img, TGA_FLAGS_NONE, L"NEW_IMAGE.TGA" );
if ( FAILED(hr) )
    // error
```

- Ref
https://github.com/microsoft/DirectXTex/wiki/TGA-I-O-Functions

# LoadFromWICFile

These functions use the Windows Imaging Component (WIC) to read or write an image file. There are built-in WIC codecs in Windows for `.BMP`, `.PNG`, `.GIF`, `.TIFF`, `.JPEG`, and JPEG-XR / HD Photo images. Some containers (`.GIF` and `.TIFF`) can contain multi-frame bitmaps files.

```c++
HRESULT LoadFromWICFile( const wchar_t* szFile,
    WIC_FLAGS flags, TexMetadata* metadata, ScratchImage& image,
    std::function<void(IWICMetadataQueryReader*)> getMQR = nullptr );
```

- Parameters
For the load functions, the _metadata_ parameter can be nullptr as this information is also available in the returned **ScratchImage**.

For the metadata and loading functions, _getMQR_ is an optional callback. If it is non-null and the WIC container codec supports a metadata query reader, then the callback is triggered. This allows an application to extract additional metadata from the file that is otherwise unused by DirectXTex.

For the saving functions, _setCustomProps_ is an optional callback. If it is non-null, then the callback is triggered so it can set additional WIC properties such as compression methods.

- Examples
This is a simple loading example. Since it only returns a single 2D image, the _TexMetadata_ info is redundant information.

```c++
auto image = std::make_unique<ScratchImage>();
HRESULT hr = LoadFromWICFile( L"WINLOGO.BMP", WIC_FLAGS_NONE, nullptr, *image );
if ( FAILED(hr) )
    // error
```

This is a multi-image loading example which can load an array of 2D images.

```c++
TexMetadata info;
auto image = std::make_unique<ScratchImage>();
HRESULT hr = LoadFromWICFile( L"MULTIFRAME.TIF",
                 WIC_FLAGS_ALL_FRAMES, &info, *image );
if ( FAILED(hr) )
    // error
```

Here we provide an optional callback to check for the [System.Photo.Orientation](https://docs.microsoft.com/windows/win32/properties/props-system-photo-orientation) metadata query property:

```c++
TexMetadata info;
auto image = std::make_unique<ScratchImage>();
uint16_t orientation = 1;
HRESULT hr = LoadFromWICFile( L"Image.JPG", WIC_FLAGS_NONE, &info, *image,
  [&](IWICMetadataQueryReader* reader)
  {
      PROPVARIANT value;
      PropVariantInit( &value );

      if ( SUCCEEDED(reader->GetMetadataByName( L"System.Photo.Orientation", &value ) )
           && value.vt == VT_UI2 )
      {
          orientation = value.uiVal;
      }

      PropVariantClear( &value );
  } );
if ( FAILED(hr) )
    // error
```

- Ref
https://github.com/microsoft/DirectXTex/wiki/WIC-I-O-Functions

